# ADR-004: HTTP outbound at-least-once com X-Event-Id

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: desconhecida.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: ORDERS e DATA.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Relacionado a:**

- [ADR-007: Retry limitado com backoff e DLQ persistida](ADR-007-retry-com-backoff-e-dlq.md)
- [ADR-005: HMAC-SHA256 com segredo por endpoint](ADR-005-hmac-sha256-com-segredo-por-endpoint.md)
- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)

## Contexto e problema

- **ADR-004-INT-01** — Router atual não monta webhooks.

## Direcionadores da decisão

- **ADR-004-DEP-01** — Documentação ao consumidor dá destaque à possibilidade de repetição.

## Alternativas consideradas

- **ADR-004-ALT-01** — Exactly-once exige coordenação complexa entre produtor e consumidor.

## Resultado da decisão

- **ADR-004** — Entrega outbound at-least-once com deduplicação no consumidor por X-Event-Id.
- **ADR-004-CON-01** — UUID gerado na outbox também aparece no payload.
- **ADR-004-DEC-01** — Limitar o ciclo de entrega a cinco tentativas totais.

## Prós e contras das opções

- **ADR-004-TRD-01** — Consumidor assume deduplicação; duplicatas HTTP continuam possíveis.

## Consequências

O ID permite reconhecer repetições sem coordenar processamento exatamente uma vez. O consumidor assume a deduplicação e o destino pode receber chamadas repetidas.

### Questão em aberto

- **ADR-004-CON-02** — Qual retenção de IDs os consumidores precisam adotar para deduplicar e aceitar replays tardios?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [src/routes/index.ts](../../src/routes/index.ts).
