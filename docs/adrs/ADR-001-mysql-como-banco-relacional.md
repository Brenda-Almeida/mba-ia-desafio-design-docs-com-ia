# ADR-001: MySQL como banco relacional compartilhado

- Status: Proposto — registro retrospectivo de tecnologia em uso; aceitação deste ADR não comprovada.
- Data da decisão: 2026-11-11.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: DATA; escopo relacionado: ORDERS e WEBHOOKS.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Utilizado por:**

- [ADR-002: Prisma como acesso a dados e migrações](ADR-002-prisma-para-acesso-a-dados.md)
- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)

## Contexto e problema

A aplicação persiste seus dados de negócio em MySQL via Prisma. O schema, a migração e o Compose confirmam esse uso [1–3]. Na reunião da feature, a presença do banco é um argumento para implementar a outbox sem introduzir outro serviço [4]. Isso documenta o motivo de seu reuso, mas não explica sua adoção original.

na implantação inicial do OMS, a liderança técnica e a equipe de plataforma precisavam entregar uma aplicação transacional com equipe pequena e orçamento operacional limitado. A empresa já mantinha MySQL em outros sistemas e possuía conhecimento de administração, backup e recuperação desse banco. A escolha foi conduzida conjuntamente pela liderança técnica e pela equipe de plataforma.

## Direcionadores da decisão

Para o reuso na feature, os direcionadores comprovados são manter a infraestrutura existente e permitir que o evento seja registrado no mesmo commit dos dados do pedido [4, 5].

Os critérios eram transações ACID, integridade referencial, compatibilidade com Prisma e operação em serviço gerenciado. Adotam-se disponibilidade mensal de 99,9%, perda máxima de 15 minutos de dados e recuperação em até quatro horas. Essas metas orientam o planejamento operacional.

## Opções consideradas

Foram consideradas as seguintes opções de banco relacional.

A avaliação compara MySQL, PostgreSQL e SQLite. MySQL é escolhido pelo conhecimento operacional já disponível; PostgreSQL seria tecnicamente adequado, mas exigiria desenvolver competência operacional adicional; SQLite é descartado para a implantação pretendida com múltiplos processos e banco centralizado gerenciado. Redis Streams continua sendo apenas a alternativa de transporte citada nas fontes.

## Resultado da decisão

O estado observado utiliza MySQL como banco relacional compartilhado pelos módulos persistentes [1–3]. A reunião determina reutilizá-lo para a futura outbox [4]. Este registro descreve a escolha em uso e essa continuidade documental; não propõe uma nova migração nem atribui aprovação ao ADR.

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| MySQL compartilhado | Na feature, possibilita usar o banco existente e a mesma fronteira transacional. | Por inferência, dados de negócio e entrega assíncrona passam a compartilhar capacidade e operação do banco. |
| PostgreSQL | Atenderia ao modelo relacional e transacional pretendido. | Demandaria capacitação e padronização operacional adicionais. |
| SQLite | Simplificaria uma implantação local e a criação de protótipos. | Não se alinha à decisão de operar um banco centralizado gerenciado compartilhado por processos. |

A equipe aceitou dependência do ecossistema MySQL e disputa de capacidade entre cargas em troca de reduzir a curva de aprendizado e reutilizar rotinas de operação. O crescimento da carga exigiria monitoramento e revisão de capacidade antes de separar a infraestrutura.

## Consequências

A evolução do schema e das transações permanece vinculada ao banco observado. Por inferência, sua substituição exigiria tratar dados persistidos, migrações e compatibilidade das consultas. Concentrar a futura outbox nesse banco evita um serviço adicional, mas requer avaliar a carga do processamento assíncrono [4].

O Compose demonstra uma configuração local, não a topologia de produção [2]. Disponibilidade, backups e capacidade não foram comprovados, e nenhum banco foi acessado nesta análise.

## Referências

1. [Schema Prisma — provider e modelos](../../prisma/schema.prisma):5–138.
2. [Compose — serviço MySQL, volume e healthcheck](../../docker-compose.yml):2–28; valores de credenciais não reproduzidos.
3. [Migração inicial — estrutura relacional](../../prisma/migrations/20260519182739_init/migration.sql):2–125.
4. [Transcrição — reuso do MySQL para outbox](../../TRANSCRICAO.md):48–58, [09:06] Diego a [09:08] Larissa.
5. [OrderService — transação de status](../../src/modules/orders/order.service.ts):126–178. As fontes centrais já aparecem em `7ef4317`; o commit não prova a data nem a motivação da adoção original.
