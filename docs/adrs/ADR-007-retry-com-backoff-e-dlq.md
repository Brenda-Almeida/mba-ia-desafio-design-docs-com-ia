# ADR-007: Retry limitado com backoff e DLQ persistida

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: 2026-06-18.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: ORDERS e DATA.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-006: Worker separado com polling](ADR-006-worker-separado-com-polling.md)

**Relacionado a:**

- [ADR-004: HTTP outbound at-least-once com X-Event-Id](ADR-004-entrega-http-at-least-once-com-event-id.md)

## Contexto e problema

Os clientes podem ficar indisponíveis por períodos de manutenção. Desistir rapidamente reduz a chance de recuperação; insistir indefinidamente acumula eventos para destinos que podem não retornar. A reunião escolhe recuperação limitada e persistência separada das falhas esgotadas para investigação e replay [1, 2]. O schema atual ainda não implementa essa infraestrutura [5].

## Direcionadores da decisão

- Tolerar indisponibilidade temporária, inclusive a manutenção de duas horas citada na reunião [1].
- Evitar eventos pendurados indefinidamente e manter evidências para recuperação [1, 2].
- Restringir replay a administração autenticada, com auditoria do autor [3].

## Opções consideradas

1. Retry limitado com backoff e encaminhamento de falhas esgotadas à DLQ.
2. Retry indefinido.
3. Limite mais agressivo de três tentativas.

A discussão também compara marcar falha na própria outbox e manter DLQ separada, favorecendo a segunda por clareza da leitura e investigação [2]. O registro completo dessas alternativas permanece no candidato associado pelo índice.

## Resultado da decisão

A reunião escolhe retry limitado, DLQ persistida separadamente e replay manual por ADMIN com auditoria [1–3]. Chamadas que excedam dez segundos são tratadas como falha para retry [4].

O desenho segue o limite anunciado no fechamento da reunião, “total 5 tentativas” [4]. Cada ciclo tem um envio inicial e até quatro retentativas. Após cada uma das quatro primeiras falhas recuperáveis, aguardar respectivamente 1 min, 5 min, 30 min e 2 h. A quinta falha esgota o ciclo e leva a entrega à DLQ. A reunião também enumera um quinto intervalo de 12 h [1, 4]. Esse intervalo fica registrado para revisão da equipe e permanece fora do agendamento.

O desenho está documentado, mas não implementado. A sequência é a citada na reunião; não se deduz uma fórmula exponencial exata nem uma nova política de intervalos.

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| Retry limitado e DLQ | Permite recuperação temporária e preserva falhas para investigação/replay. | Encerra o processamento automático após o limite e exige recuperação operacional. |
| Retry indefinido | Por inferência, mantém tentativas automáticas sem um teto de esgotamento. | A reunião aponta o risco de eventos ficarem pendentes para sempre. |
| Três tentativas | Por inferência, abandona mais cedo os destinos indisponíveis. | Considerado insuficiente diante da janela de manutenção discutida. |

## Consequências

As falhas permanentes ficam visíveis e recuperáveis manualmente, em vez de ocupar indefinidamente a leitura de pendentes [2]. O trade-off é exigir intervenção após o esgotamento. Por inferência, a DLQ também cria responsabilidades de retenção e proteção dos dados armazenados.

Não há promessa de entrega bem-sucedida a um destino permanentemente indisponível. Timeout, falha de rede e respostas HTTP 408, 429 ou 5xx permitem retry; respostas 2xx confirmam sucesso; demais respostas encerram o ciclo na DLQ sem nova tentativa automática. Redirecionamentos não são seguidos. O replay exige ADMIN, gera um novo ciclo de entrega com contador reiniciado e preserva `event_id` e snapshot; o autor e a origem do replay ficam registrados para auditoria. A unicidade de um ciclo ativo por evento/endpoint impede replay concorrente duplicado. Durante retries automáticos, eventos posteriores do mesmo pedido/endpoint aguardam; após DLQ podem prosseguir, de modo que replay posterior pode chegar fora da ordem histórica.

## Referências

1. [Transcrição — alternativas e progressão do retry](../../TRANSCRICAO.md):92–108, [09:15] Diego a [09:17] Larissa.
2. [Transcrição — DLQ e replay](../../TRANSCRICAO.md):108–116, [09:17] Larissa a [09:19] Larissa.
3. [Transcrição — autorização e auditoria](../../TRANSCRICAO.md):204–216, [09:35] Larissa a [09:37] Sofia.
4. [Transcrição — timeout e resumo das decisões](../../TRANSCRICAO.md):248–252,282, [09:42] Sofia/Diego e [09:48] Larissa.
5. [Schema Prisma — ausência de persistência da feature](../../prisma/schema.prisma):25–138. A transcrição foi introduzida em `7ef4317`; isso não fornece a data civil da decisão nem comprova implantação.
