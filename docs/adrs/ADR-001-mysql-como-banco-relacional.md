# ADR-001: MySQL como banco relacional compartilhado

- Status: Proposto — registro retrospectivo de tecnologia em uso; aceitação deste ADR não comprovada.
- Data da decisão: desconhecida.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: DATA; escopo relacionado: ORDERS e WEBHOOKS.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Utilizado por:**

- [ADR-002: Prisma como acesso a dados e migrações](ADR-002-prisma-para-acesso-a-dados.md)
- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)

## Contexto e problema

- **ADR-001-INT-01** — Compose demonstra serviço MySQL local com volume e healthcheck.

## Direcionadores da decisão

A feature precisa compartilhar a transação de Pedidos com o registro da intenção de entrega.

## Alternativas consideradas

- **ADR-001-ALT-01** — Redis Streams adicionaria infraestrutura considerada excessiva.

## Resultado da decisão

- **ADR-001** — Aplicação usa MySQL relacional compartilhado com Prisma.
- **ADR-001-DEC-01** — Reusar MySQL existente para a outbox e o mesmo commit dos dados de Pedidos.

## Prós e contras das opções

- **ADR-001-TRD-01** — Há dependência do provider MySQL, modelos e migração que uma substituição precisaria revisar.

## Consequências

Reusar o MySQL permite confirmar a intenção de entrega no mesmo commit de Pedidos. A solução fica vinculada ao mesmo banco, cujo dimensionamento precisa considerar a nova carga.

### Questão em aberto

- **ADR-001-Q-01** — Qual capacidade será necessária para a carga conjunta de Pedidos e Outbox?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [prisma/schema.prisma](../../prisma/schema.prisma).
- [prisma/migrations/20260519182739_init/migration.sql](../../prisma/migrations/20260519182739_init/migration.sql).
- [docker-compose.yml](../../docker-compose.yml).
