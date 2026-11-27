# ADR-006: Worker separado com polling

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: desconhecida.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: ORDERS e DATA.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-002: Prisma como acesso a dados e migrações](ADR-002-prisma-para-acesso-a-dados.md)

**Utilizado por:**

- [ADR-007: Retry limitado com backoff e DLQ persistida](ADR-007-retry-com-backoff-e-dlq.md)

## Contexto e problema

- **ADR-006-INT-01** — Bootstrap atual atende API; manifesto não contém comando de worker.

## Direcionadores da decisão

- **ADR-006-OBJ-01** — Atender à expectativa de notificar abaixo de dez segundos.

## Alternativas consideradas

- **ADR-006-ALT-01** — Worker na API compartilharia seus reinícios.
- **ADR-006-ALT-02** — Trigger não notifica diretamente processo externo.

## Resultado da decisão

- **ADR-006** — Um worker separado lê a outbox por polling a cada 2 segundos.
- **ADR-006-DEC-01** — Reusar banco e stack, com Prisma próprio por processo.
- **ADR-006-DEC-02** — Lógica do worker no módulo da feature e entry point independente.

## Prós e contras das opções

- **ADR-006-TRD-01** — Polling é consulta periódica com espera até a próxima busca.

## Consequências

O worker pode continuar operando durante reinícios da API. O polling acrescenta espera até a próxima consulta e o processo mantém seu próprio cliente Prisma.

- **ADR-006-ESC-01** — Escala horizontal adiada; locks e particionamento são hipóteses futuras.

### Questão em aberto

- **ADR-006-DEC-04** — Como ordenar entregas durante retry e recuperar trabalho abandonado?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [src/server.ts](../../src/server.ts).
- [package.json](../../package.json).
