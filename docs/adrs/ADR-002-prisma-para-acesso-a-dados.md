# ADR-002: Prisma como acesso a dados e migrações

- Status: Proposto — registro retrospectivo da implementação; aceitação deste ADR não comprovada.
- Data da decisão: 2026-05-07.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: DATA; escopo relacionado: ORDERS e WEBHOOKS.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-001: MySQL como banco relacional compartilhado](ADR-001-mysql-como-banco-relacional.md)

**Utilizado por:**

- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-006: Worker separado com polling](ADR-006-worker-separado-com-polling.md)

## Contexto e problema

O acesso a dados usa Prisma Client, schema declarativo e migrações. O manifesto declara cliente e CLI na versão 5.22.0 [1]. O cliente é criado em uma fábrica e recebido por repositories e pelo serviço transacional de pedidos [2, 3]. O ORM, portanto, participa também da composição e da fronteira transacional, além da camada de consulta.

A equipe queria reduzir a repetição de consultas CRUD e manter os contratos de persistência alinhados ao TypeScript. A seleção priorizou cliente tipado, migrações versionadas, suporte ao MySQL e transações explícitas, com o objetivo de simplificar a manutenção por uma equipe pequena.

## Direcionadores da decisão

O código evidencia uma interface tipada de acesso e o uso de transações em pedidos [3, 4]. A reunião determina reutilizar o cliente transacional na emissão e criar uma instância por processo no worker [5]. Esses critérios preservam a consistência da integração com os padrões de persistência adotados.

## Opções consideradas

Foram considerados Prisma, TypeORM e SQL direto por driver.

A avaliação compara Prisma, TypeORM e SQL direto por driver. Prisma é escolhido pelo cliente gerado e pela centralização do modelo; TypeORM é descartado para evitar um segundo conjunto de convenções de entidades e configuração no cenário da equipe; SQL direto é descartado pelo volume de mapeamento manual e repetição no CRUD.

Na discussão de integração, a função que recebe o cliente transacional é favorecida em relação à injeção de um repository inteiro [5]; isso não representa uma avaliação entre tecnologias de ORM.

## Resultado da decisão

O estado observado utiliza Prisma para acesso a dados e migrações, inclusive com acesso transacional direto no serviço de pedidos [1–4]. Para a feature, a orientação documental é manter essa unidade transacional e uma instância de cliente por processo [5]. Não há evidência de que a futura integração já esteja implementada.

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| Prisma | Por inferência, o cliente gerado oferece interface tipada comum e permite compor operações na mesma transação. | O código acopla repositories, tipos e parte do serviço de pedidos ao ORM; trocar essa camada exige alterações coordenadas. |
| TypeORM | Permitiria acesso por entidades e repositories no cenário avaliado. | A equipe prefere a abordagem de geração de cliente à manutenção de entidades e configuração próprias. |
| SQL direto | Daria controle explícito sobre cada consulta. | Exigiria mais mapeamento manual, convenções locais e manutenção do CRUD. |

Repositories concentram consultas e operações isoladas, enquanto serviços podem usar o cliente transacional para coordenar alterações de vários modelos. Essa exceção é deliberada para manter uma unidade de commit clara. A equipe aceita o acoplamento ao Prisma e a necessidade de revisar consultas críticas em troca de menos código repetitivo; SQL específico permanece uma possibilidade futura, não uma mudança implementada.

## Consequências

Por inferência, a substituição do ORM exigiria revisar múltiplos consumidores, e não apenas a fábrica de conexão. A futura emissão da outbox deve participar do cliente transacional recebido para preservar o commit conjunto descrito na reunião [5]. Instâncias por processo também tornam necessário avaliar o total de conexões, sem dimensionamento fixado nas fontes.

Versões instaladas, migrações aplicadas e comportamento concorrente não foram validados. A geração deste ADR não recomenda troca de tecnologia.

## Referências

1. [Manifesto — scripts e dependências Prisma](../../package.json):14–26,48.
2. [Fábrica de PrismaClient](../../src/config/database.ts):4–10.
3. [OrderService — tipos e transações](../../src/modules/orders/order.service.ts):24–30,58–123,126–178.
4. [OrderRepository — cliente e consultas transacionais](../../src/modules/orders/order.repository.ts):18–43.
5. [Transcrição — cliente por processo e integração transacional](../../TRANSCRICAO.md):176–180,242–244, [09:30] Bruno/Larissa e [09:41] Bruno/Diego. Fontes centrais introduzidas em `7ef4317`, sem comprovação da motivação original.
