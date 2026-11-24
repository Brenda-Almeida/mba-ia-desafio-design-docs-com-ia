# ADR-004: HTTP outbound at-least-once com X-Event-Id

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: 2026-06-18.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: ORDERS e DATA.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Relacionado a:**

- [ADR-007: Retry limitado com backoff e DLQ persistida](ADR-007-retry-com-backoff-e-dlq.md)
- [ADR-005: HMAC-SHA256 com segredo por endpoint](ADR-005-hmac-sha256-com-segredo-por-endpoint.md)
- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)

## Contexto e problema

O escopo da feature é enviar notificações de mudança de status a sistemas externos [1]. A reunião reconhece que o cliente pode receber o mesmo evento mais de uma vez e precisa distinguir duplicatas de eventos distintos. O contrato entre os sistemas, portanto, precisa explicitar identidade e responsabilidade de deduplicação [2]. A API atual ainda não inclui webhooks [5].

## Direcionadores da decisão

- Aceitar duplicação sem exigir coordenação complexa para processamento exatamente uma vez [2].
- Dar ao consumidor uma identidade estável para reconhecer o evento [2].
- Tornar explícita a responsabilidade do cliente pela deduplicação [2].

## Opções consideradas

1. Entrega HTTP outbound at-least-once, com identidade do evento e deduplicação pelo consumidor.
2. Exactly-once, contraposto na reunião e rejeitado pela complexidade da coordenação entre os dois lados [2].

O esclarecimento de que a integração é apenas outbound delimita o escopo; não constitui comparação entre transportes [1].

## Resultado da decisão

A decisão observada é adotar a semântica denominada at-least-once na reunião e enviar `X-Event-Id` para que o cliente reconheça duplicatas. A identidade é UUID gerado quando o evento entra na outbox, também presente no contrato do payload [2, 3].

O contrato admite múltiplas recepções do mesmo evento. Conforme o [ADR-007](ADR-007-retry-com-backoff-e-dlq.md), cada ciclo tem um envio inicial e até quatro retentativas, respeitando o total de cinco tentativas registrado na reunião. Esgotado o limite, a entrega vai à DLQ e a recuperação depende de replay administrativo. O ADR-007 registra a divergência sobre o quinto intervalo. A semântica at-least-once requer deduplicação pelo consumidor e o sucesso depende da disponibilidade do destino.

O registro preserva a decisão de aceitar duplicatas; não afirma que o processamento externo ou a idempotência interna já foram implementados.

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| At-least-once com identidade | Evita a coordenação adicional de exactly-once discutida na reunião e permite reconhecer duplicatas. | Transfere ao consumidor a deduplicação e não elimina múltiplas chamadas HTTP. |
| Exactly-once | Busca evitar a reaplicação de um evento, objetivo inferido da alternativa. | A reunião considera a coordenação entre os dois lados mais complexa. |

## Consequências

A documentação de integração precisará destacar a possibilidade de repetição [2]. Por inferência, a identidade do evento precisa permanecer distinta da identidade de cada tentativa para sustentar a deduplicação pretendida.

Todos os endpoints inscritos recebem o mesmo `event_id` de domínio, com entregas e ciclos de processamento distintos. Retries e replay mantêm esse identificador e o snapshot original. O consumidor registra a chave `(webhook_id, event_id)` por pelo menos sete dias após a última recepção, renovando a retenção a cada repetição. Se o evento já tiver sido aplicado, deve reconhecer a repetição sem repetir seus efeitos. Replay após a retenção pode provocar reaplicação e exige avaliação administrativa; não se promete deduplicação ilimitada.

Deduplicação não equivale a ordenação. As fontes não resolvem a interação entre retry e ordem por pedido [4]. O fluxo automático bloqueia eventos posteriores do mesmo pedido/endpoint até sucesso ou DLQ da entrega anterior; replay não restaura a ordem histórica. Não há evidência de implementação ou verificação do comportamento dos consumidores externos.

## Referências

1. [Transcrição — escopo outbound](../../TRANSCRICAO.md):24–28, [09:02] Sofia/Marcos e [09:03] Sofia.
2. [Transcrição — duplicação, identidade e exactly-once](../../TRANSCRICAO.md):146–158, [09:24] Diego a [09:26] Larissa.
3. [Transcrição — contrato de payload e headers](../../TRANSCRICAO.md):254–266, [09:43] Marcos a [09:45] Diego.
4. [Transcrição — ordem e recuperação](../../TRANSCRICAO.md):78–86,108–116, [09:12–09:13] Larissa/Diego e [09:17–09:19] Larissa/Diego. A transcrição aparece em `7ef4317`, sem data civil comprovada para a decisão.
5. [Router agregado — módulos atuais](../../src/routes/index.ts):24–28.
