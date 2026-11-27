# ADR-003: Outbox no MySQL na transação de mudança de status

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, ainda não implementada.
- Data da decisão: desconhecida.
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

- **ADR-003-INT-01** — Fluxo atual transaciona status, estoque aplicável e histórico.
- **ADR-003-INT-02** — Schema atual não contém outbox de webhook.

## Direcionadores da decisão

A confirmação de status precisa ser independente da disponibilidade do destinatário.

## Alternativas consideradas

- **ADR-003-ALT-01** — Chamada síncrona prolongaria transação e acoplaria falha externa.
- **ADR-003-ALT-02** — Redis Streams exige infraestrutura adicional excessiva.
- **ADR-003-ALT-03** — Renderização tardia pode refletir estado posterior.

## Resultado da decisão

- **ADR-003** — Usar outbox MySQL na transação de mudança; falha no registro impede o commit.
- **ADR-003-DEC-01** — Envio HTTP ocorre em separado após commit.
- **ADR-003-DEC-02** — Guardar snapshot na inserção com identidade UUID.

## Prós e contras das opções

- **ADR-003-TRD-01** — Escrita adicional é condição do commit, que garante somente intenção.

## Consequências

A outbox protege a atomicidade entre a mudança de status e a intenção de entrega. A escrita da outbox passa a ser condição para o commit e o envio permanece assíncrono.

### Questão em aberto

- **ADR-003-DEC-03** — Como modelar a relação entre um evento do pedido e os destinos inscritos?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [src/modules/orders/order.service.ts](../../src/modules/orders/order.service.ts).
- [prisma/schema.prisma](../../prisma/schema.prisma).
