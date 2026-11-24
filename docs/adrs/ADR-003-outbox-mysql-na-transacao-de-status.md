# ADR-003: Outbox no MySQL na transação de mudança de status

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, ainda não implementada.
- Data da decisão: 2026-06-18.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: ORDERS; escopo relacionado: DATA e WEBHOOKS.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-001: MySQL como banco relacional compartilhado](ADR-001-mysql-como-banco-relacional.md)
- [ADR-002: Prisma como acesso a dados e migrações](ADR-002-prisma-para-acesso-a-dados.md)

**Utilizado por:**

- [ADR-006: Worker separado com polling](ADR-006-worker-separado-com-polling.md)
- [ADR-007: Retry limitado com backoff e DLQ persistida](ADR-007-retry-com-backoff-e-dlq.md)

**Relacionado a:**

- [ADR-004: HTTP outbound at-least-once com X-Event-Id](ADR-004-entrega-http-at-least-once-com-event-id.md)

## Contexto e problema

O sistema já altera status, estoque e histórico do pedido em uma transação. A feature precisa registrar a intenção de notificar o cliente sem fazer a mudança de status depender da disponibilidade de um servidor externo. A reunião identifica que uma chamada HTTP síncrona alongaria a transação e criaria um conflito entre falha remota e confirmação do pedido [1, 2].

## Direcionadores da decisão

- Atomicidade entre a mudança de status e o registro do evento elegível para notificação [1, 2].
- Reuso do MySQL existente, evitando a operação de infraestrutura adicional pelo time [1].
- Preservação do estado do pedido no momento do evento, mesmo que ele mude antes do envio [3].

## Opções consideradas

1. Outbox no MySQL, persistida na transação de mudança de status, com envio posterior.
2. Chamada HTTP síncrona durante a mudança de status.
3. Redis Streams como infraestrutura de eventos.

Essas opções foram discutidas na reunião [1]. A escolha complementar de snapshot em vez de renderização no envio está registrada em [3]; o candidato de origem preserva a discussão completa no índice.

## Resultado da decisão

A decisão observada é registrar os eventos elegíveis na outbox dentro da mesma transação que altera o pedido e seu histórico, deixando o HTTP para processamento assíncrono. Uma falha ao registrar a intenção deve impedir o commit dessa alteração. O payload será capturado como snapshot na inserção, com identificação UUID [2, 3].

O código atual contém a transação de pedido, mas ainda não contém outbox [2, 4]. Esta é documentação da decisão da reunião, sem atribuir implantação ou aprovação formal a este novo registro.

Cada mudança elegível gera um evento de domínio com UUID e snapshot imutável. Cada endpoint inscrito recebe um registro de entrega próprio, que referencia esse evento e mantém seu estado de processamento. O `event_id` é compartilhado entre os destinos; o identificador da entrega é distinto. Uma restrição de unicidade por evento e endpoint evita criar duas entregas iguais. Evento e entregas são registrados na transação do pedido.

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| Outbox no MySQL | Mantém intenção de notificação e mudança de status no mesmo commit, usando o banco existente. | Exige processamento assíncrono; por inferência, acrescenta escrita e carga operacional ao banco. |
| HTTP síncrono | Envio diretamente associado ao fluxo que inicia a mudança, por inferência. | Cliente lento aumenta a duração da transação; falha externa passa a interferir na confirmação do pedido, conforme discutido. |
| Redis Streams | Oferece um mecanismo de eventos separado do banco, por inferência da alternativa. | Introduz infraestrutura que a reunião considerou excessiva; a discussão não estabeleceu uma solução de atomicidade entre banco e stream. |

## Consequências

O commit poderá assegurar a presença da intenção de notificação, sem assegurar a entrega remota. O snapshot mantém o significado temporal do evento [2, 3]. A emissão dependerá do sucesso da escrita adicional, trade-off explícito para impedir que status seja confirmado sem o registro elegível [2].

Por inferência, processamento, recuperação e retenção da outbox passam a ser responsabilidades operacionais. A atomicidade pretendida não comprova isolamento de estoque sob concorrência nem entrega exatamente uma vez; esses comportamentos não foram validados dinamicamente.

## Referências

1. [Transcrição — alternativas e escolha da outbox](../../TRANSCRICAO.md):30–58, [09:03] Larissa a [09:08] Larissa.
2. [Transcrição — fronteira transacional](../../TRANSCRICAO.md):238–244, [09:40] Bruno a [09:41] Diego.
3. [Transcrição — UUID e snapshot](../../TRANSCRICAO.md):302–314, [09:51] Diego a [09:52] Bruno.
4. [OrderService — transação existente](../../src/modules/orders/order.service.ts):126–178.
5. [Schema Prisma — modelos atuais](../../prisma/schema.prisma):25–138; código e transcrição já presentes em `7ef4317`, sem comprovar a data histórica da decisão.
