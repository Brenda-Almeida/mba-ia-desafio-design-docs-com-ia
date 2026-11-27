# ADR-002: Prisma como acesso a dados e migrações

- Status: Proposto — registro retrospectivo da implementação; aceitação deste ADR não comprovada.
- Data da decisão: desconhecida.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: DATA; escopo relacionado: ORDERS e WEBHOOKS.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-001: MySQL como banco relacional compartilhado](ADR-001-mysql-como-banco-relacional.md)

**Utilizado por:**

- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-006: Worker separado com polling](ADR-006-worker-separado-com-polling.md)

## Contexto e problema

- **ADR-002-INT-01** — Fábrica fornece PrismaClient e composição injeta o cliente em repository e serviço de Pedidos.
- **ADR-002-INT-02** — Repository faz consultas e serviço de Pedidos coordena alterações de múltiplos modelos no tx.

## Direcionadores da decisão

O cliente tipado e a transação atual orientam a integração com Pedidos.

## Alternativas consideradas

- **ADR-002-ALT-01** — Preferir função que recebe o tx a injetar repository integral.

## Resultado da decisão

- **ADR-002** — Projeto usa Prisma Client, schema declarativo e migrações; cliente e CLI declarados em 5.22.0.
- **ADR-002-DEC-01** — Futura emissão usa o tx recebido para commit conjunto.
- **ADR-002-DEC-02** — Worker cria instância de Prisma própria por processo.

## Prós e contras das opções

- **ADR-002-TRD-01** — Trocar Prisma exigiria adaptar consumidores acoplados a seus tipos e operações.

## Consequências

Receber o cliente transacional preserva um único commit. O processamento separado mantém uma instância própria de Prisma e exige planejamento de conexões.

### Questão em aberto

- **ADR-002-Q-01** — Qual orçamento de conexões acomodará API e worker?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [package.json](../../package.json).
- [prisma/schema.prisma](../../prisma/schema.prisma).
- [prisma/migrations/20260519182739_init/migration.sql](../../prisma/migrations/20260519182739_init/migration.sql).
- [src/config/database.ts](../../src/config/database.ts).
- [src/app.ts](../../src/app.ts).
- [src/modules/orders/order.repository.ts](../../src/modules/orders/order.repository.ts).
- [src/modules/orders/order.service.ts](../../src/modules/orders/order.service.ts).
