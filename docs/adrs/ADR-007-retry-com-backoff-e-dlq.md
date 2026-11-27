# ADR-007: Retry limitado com backoff e DLQ persistida

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: desconhecida.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: ORDERS e DATA.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-006: Worker separado com polling](ADR-006-worker-separado-com-polling.md)

**Relacionado a:**

- [ADR-004: HTTP outbound at-least-once com X-Event-Id](ADR-004-entrega-http-at-least-once-com-event-id.md)

## Contexto e problema

- **ADR-007-INT-01** — Schema atual ainda não inclui persistência de outbox/DLQ.

## Direcionadores da decisão

A operação precisa tolerar indisponibilidade temporária e encerrar tentativas esgotadas.

## Alternativas consideradas

- **ADR-007-ALT-01** — Retry indefinido deixa eventos pendurados.
- **ADR-007-ALT-02** — Três tentativas insuficientes para manutenção de duas horas.
- **ADR-007-ALT-03** — Falha na própria outbox dificulta separar pendências, investigar e recuperar.

## Resultado da decisão

- **ADR-007** — Retentar até o limite de cinco tentativas totais.
- **ADR-007-DEC-01** — DLQ separada após esgotamento preserva informações para diagnóstico e replay.
- **ADR-007-DEC-02** — Replay manual por ADMIN retorna a pendente e registra autor.
- **ADR-007-DEC-03** — Timeout de 10 segundos seguido de retry.

## Prós e contras das opções

- **ADR-007-TRD-01** — Após limite, recuperação exige intervenção manual.

## Consequências

O retry limitado dá tempo para o destino se recuperar e conserva falhas para diagnóstico. Após o esgotamento, a recuperação exige replay administrativo.

- **ADR-007-Q-01** — O total anunciado de cinco tentativas diverge dos intervalos citados de 1 min, 5 min, 30 min, 2 h e 12 h.

### Questão em aberto

- **ADR-007-DEC-04** — Como resolver o intervalo excedente de 12 h mantendo o total anunciado de cinco tentativas?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [prisma/schema.prisma](../../prisma/schema.prisma).
