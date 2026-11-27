# PRD — Sistema de Webhooks de Notificação de Pedidos

**Status** · Para revisão de Produto, Pedidos, Plataforma e Segurança.

## 1. Resumo e contexto da feature

O sistema enviará notificações de mudança de status aos destinos cadastrados pelos clientes B2B. O envio será assíncrono e o consumidor poderá verificar a assinatura e reconhecer duplicatas.

## 2. Problema e motivação

- **PRD-CTX-01** — Atlas Comercial, MaxDistribuição e Nova Cargo dependem de polling em GET /orders, com custo e lentidão.
- **PRD-CTX-02** — Atlas sinalizou possível migração se a necessidade não for atendida até fim do trimestre.
- **PRD-INT-01** — Fluxo atual transaciona status, histórico e estoque aplicável. [order.service.ts](../src/modules/orders/order.service.ts)

## 3. Público-alvo e cenários de uso

Atlas Comercial, MaxDistribuição e Nova Cargo integram seus sistemas ao fluxo de pedidos. Um cliente pode cadastrar um destino para acompanhar apenas `SHIPPED` e `DELIVERED`. A Administração pode reprocessar falhas esgotadas de forma auditada. [Transcrição, 09:00, 09:33 e 09:36](../TRANSCRICAO.md)

## 4. Objetivos e métricas de sucesso

- **PRD-OBJ-001** — Notificar mudanças de interesse em menos de dez segundos.

- **PRD-MET-03** — Proposta de medição: medir da confirmação do status até a primeira entrega bem-sucedida. Validar carga, janela e percentil antes de publicar resultados.

## 5. Escopo

### Incluso

Configuração de destinos por cliente, envio assíncrono e assinado, histórico de entregas e replay administrativo.

- **PRD-ESC-08** — Produto documentará a integração via API e a deduplicação no portal.

### Fora de escopo

- **PRD-ESC-01** — Integração exclusivamente outbound.
- **PRD-ESC-02** — Excluir itens do payload e consultar detalhes pela API de pedidos.
- **PRD-ESC-03** — Adiar email de falhas.
- **PRD-ESC-04** — Entregar API sem dashboard visual nesta fase.
- **PRD-ESC-05** — Observar rajadas antes de decidir rate limiting por destino.
- **PRD-ESC-06** — Adiar arquivamento; cerca de 30 dias foi sugestão informal.
- **PRD-ESC-07** — Adiar múltiplos workers e reavaliar coordenação e ordem.

## 6. Requisitos funcionais

- **PRD-RF-001** — Cadastrar webhook via API autenticada com cliente explícito, URL HTTPS, filtros e devolução de segredo exclusivo gerado pela plataforma.
- **PRD-RF-002** — Editar configuração de destino via API autenticada, com URL, estado ativo e filtro.
- **PRD-RF-003** — Remover destino pela API autenticada.
- **PRD-RF-004** — Listar destinos de um cliente explícito pela API autenticada.
- **PRD-RF-005** — Filtrar cadastros ativos por status de destino na origem; sem interessados, confirmar mudança sem registrar entrega.
- **PRD-RF-005-SNAPSHOT** — Registrar os dados do pedido no momento da mudança.
- **PRD-RF-006** — Enviar após o commit o JSON order.status_changed com UUID, instante, ID e número do pedido, status anterior e novo, cliente e total em centavos.
- **PRD-RF-006-HEADERS** — Informar cadastro, instante do envio e tipo de conteúdo em X-Webhook-Id, X-Timestamp e Content-Type.
- **PRD-RF-007** — Enviar UUID único em X-Event-Id para identificação de duplicatas pelo consumidor.
- **PRD-RF-008** — Rotacionar segredo via API com validade paralela do anterior por 24 h.
- **PRD-RF-009** — Limitar cada ciclo de retry a cinco tentativas totais, conforme o total anunciado na reunião.
- **PRD-RF-010** — Persistir falha esgotada em DLQ separada com payload, motivo e data/hora.
- **PRD-RF-011** — Consultar histórico por destino com resultado, payload, resposta quando disponível e duração; últimos 100 foram exemplo.
- **PRD-RF-012** — Replay manual restrito a ADMIN recoloca entrega como pendente e audita autor.

## 7. Requisitos não funcionais

- **PRD-RNF-001** — Confirmar intenção com status, histórico e estoque; falha de enqueue reverte a transação, e falha HTTP ocorre depois.
- **PRD-RNF-002** — Exigir HTTPS e rejeitar URL HTTP com erro de validação.
- **PRD-RNF-003** — Autenticar o corpo por HMAC-SHA256 com segredo por destino em X-Signature.
- **PRD-RNF-004** — Rejeitar payload acima de 64 KB com erro, sem truncamento.
- **PRD-RNF-005** — Tratar ausência de resposta em 10 segundos como falha sujeita a retry.
- **PRD-RNF-006** — Isolar reinícios da API com worker único e separado; polling a cada 2 segundos.
- **PRD-RNF-007** — At-least-once admite duplicatas e exige deduplicação pelo consumidor.
- **PRD-RNF-007-ORDEM** — Ordem prevista por pedido com um worker, sem garantia global.
- **PRD-RNF-008** — Reusar padrões de Zod, Pino, AppError e middleware; erros próprios com prefixo WEBHOOK_.

## 8. Decisões e trade-offs principais

- **PRD-DEC-01** — Reusar MySQL e Prisma para o processamento separado e a integração transacional.
- **PRD-TRD-01** — Retry limitado transfere recuperação após esgotamento à operação administrativa.
- **PRD-TRD-02** — Segredo exclusivo limita exposição e exige gestão de janela de migração pelo destinatário.
- **PRD-Q-01** — Conciliar o total anunciado de cinco tentativas com a lista verbal de esperas de 1 min, 5 min, 30 min, 2 h e 12 h.
- **PRD-Q-04** — Cadastro público aceita ADMIN, comprometendo a barreira de papel proposta para replay. [auth.routes.ts](../src/modules/auth/auth.routes.ts) · [auth.schemas.ts](../src/modules/auth/auth.schemas.ts) · [auth.controller.ts](../src/modules/auth/auth.controller.ts) · [auth.service.ts](../src/modules/auth/auth.service.ts) · [user.service.ts](../src/modules/users/user.service.ts) · [user.repository.ts](../src/modules/users/user.repository.ts)
- **PRD-Q-05** — X-Timestamp foi sugerido para detectar replay, mas fica fora do corpo assinado.
- **PRD-PLAN-01** — Estimativa de três sprints incluindo revisão.
- **PRD-PLAN-02** — Atlas pediu fim de novembro; Marcos ficou de confirmar prazo.
## 9. Dependências

- **PRD-DEP-01** — Pedidos fornece transições e contexto do pedido, com referência ao cliente. [order.service.ts](../src/modules/orders/order.service.ts)
- **PRD-DEP-02** — JWT atual identifica usuário e papel. [auth.middleware.ts](../src/middlewares/auth.middleware.ts)
- **PRD-DEP-03** — Reservar ao menos dois dias úteis para revisão de segurança pré-deploy.
## 10. Riscos e mitigação

As classificações de probabilidade e impacto são estimativas qualitativas para revisão.

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| **PRD-RISCO-01** — Confirmar status sem registrar intenção se enqueue ficar fora da transação. | Média | Alto | Gravar evento na mesma transação e validar rollback no aceite PRD-CA-004. |
| **PRD-RISCO-03** — Destino indisponível pode esgotar o orçamento de tentativas. | Média | Médio | Aplicar timeout e retry limitado e encaminhar à DLQ para replay administrativo. |

- **PRD-RISCO-EST-01** — Avaliação qualitativa do risco de perda de evento quando a gravação fica fora da transação: probabilidade média e impacto alto.
- **PRD-RISCO-EST-02** — Avaliação qualitativa do risco de indisponibilidade do destino esgotar as tentativas: probabilidade média e impacto médio.

## 11. Critérios de aceitação

- **PRD-CA-001** — Ao cadastrar com JWT, URL HTTPS e filtro válido, retornar o segredo exclusivo e incluir o cadastro na listagem. Rejeitar URL HTTP.
- **PRD-CA-002** — Mudanças futuras usam o filtro atual e deixam de selecionar cadastros desativados ou removidos.
- **PRD-CA-003** — Notificação elegível mantém snapshot original mesmo após nova mudança; mudança válida sem inscritos não gera entrega.
- **PRD-CA-004** — Com falha ao registrar um evento elegível, reverter status, histórico e estoque aplicável. Com destino indisponível após o commit, manter a alteração de status.
- **PRD-CA-005** — Validar integridade do corpo e coexistência de segredos nas primeiras 24 h.
- **PRD-CA-006** — Timeout em 10 s aciona retry limitado; o esgotamento persiste falha com payload, motivo e instante.
- **PRD-CA-007** — Com worker único e entregas sem retry, observar ordem por pedido e deduplicar por ID.
- **PRD-CA-008** — Histórico mostra resultado, payload, duração e resposta quando disponível.
- **PRD-CA-009** — Somente ADMIN pode agendar replay e o autor deve ser registrado.
- **PRD-CA-011** — Acima de 64 KB, retornar erro e preservar conteúdo sem truncar.
- **PRD-CA-003-CONTRATO** — Para a mudança elegível, verificar conteúdo do JSON e headers de envio acordados.

## 12. Estratégia de testes e validação

- **PRD-TESTE-03** — Executar testes ponta a ponta na integração do serviço de Pedidos.
- **PRD-TESTE-04** — Validar o contrato com um destino de teste e usar uma falha controlada de persistência para conferir rollback de status, histórico e estoque.
- Exercitar os cenários de assinatura, retry, deduplicação e replay descritos nos critérios de aceitação.

## Questões em aberto

- **PRD-Q-03** — Como comprovar que o usuário autenticado pode administrar o cliente informado?
- **PRD-Q-07** — Como edições, desativação e remoção de destinos afetam entregas pendentes e o histórico?
- **PRD-MET-02** — Qual percentil, carga e janela serão usados para aferir a meta de latência?
- **PRD-Q-08** — Qual é o limite exato em bytes e qual deve ser o efeito do excesso sobre a transição?

## Documentos relacionados

- [Transcrição da reunião](../TRANSCRICAO.md) · [Tracker](TRACKER.md) · [RFC](RFC.md) · [FDD](FDD.md).
