# Tracker de rastreabilidade — Webhooks de mudança de status de pedidos

## Inventário e método

Cada item representa uma afirmação verificável no documento indicado. Repetições internas em resumos, exemplos e diagramas foram consolidadas. Ocorrências em documentos distintos mantêm seu próprio ID. Critérios de aceitação e estratégias de teste contam quando acrescentam uma condição verificável. Os itens com evidência constam na tabela e os demais constam no inventário de lacunas. Linhas classificadas como derivadas explicitam a aplicação ao desenho e indicam as falas que sustentam suas premissas. `docs/adrs/README.md` é um índice estrutural e acrescenta zero itens da feature.

- PRD — 102 identificados, 61 rastreados, 41 sem evidência, cobertura de 59,80%.
- RFC — 79 identificados, 62 rastreados, 17 sem evidência, cobertura de 78,48%.
- FDD — 285 identificados, 112 rastreados, 173 sem evidência, cobertura de 39,30%.
- ADR-001 — 14 identificados, 5 rastreados, 9 sem evidência, cobertura de 35,71%.
- ADR-002 — 13 identificados, 7 rastreados, 6 sem evidência, cobertura de 53,85%.
- ADR-003 — 13 identificados, 9 rastreados, 4 sem evidência, cobertura de 69,23%.
- ADR-004 — 13 identificados, 7 rastreados, 6 sem evidência, cobertura de 53,85%.
- ADR-005 — 16 identificados, 9 rastreados, 7 sem evidência, cobertura de 56,25%.
- ADR-006 — 16 identificados, 9 rastreados, 7 sem evidência, cobertura de 56,25%.
- ADR-007 — 17 identificados, 11 rastreados, 6 sem evidência, cobertura de 64,71%.
- ADRs em conjunto — 102 identificados, 57 rastreados, 45 sem evidência, cobertura de 55,88%.
- Total — 568 identificados, 292 rastreados, 276 sem evidência, cobertura de 51,41%.

A tabela contém 292 linhas. São 221 com TRANSCRICAO (75,68%) e 71 com CODIGO (32 arquivos distintos). As localizações de transcrição mantêm os horários e os falantes do registro. Os vínculos de código sustentam o estado observado e os pontos de integração.

## Matriz de rastreabilidade

| ID | Documento | Tipo | Conteúdo (resumo) | Fonte | Localização |
|---|---|---|---|---|---|
| PRD-OBJ-001 | docs/PRD.md | Métrica | Notificar mudanças de interesse em menos de dez segundos | TRANSCRICAO | [09:02] Marcos |
| PRD-CTX-01 | docs/PRD.md | Contexto | Atlas Comercial, MaxDistribuição e Nova Cargo dependem de polling em GET /orders, com custo e lentidão | TRANSCRICAO | [09:00] Marcos |
| PRD-CTX-02 | docs/PRD.md | Risco | Atlas sinalizou possível migração se a necessidade não for atendida até fim do trimestre | TRANSCRICAO | [09:00] Marcos |
| PRD-INT-01 | docs/PRD.md | Integração | Fluxo atual transaciona status, histórico e estoque aplicável | CODIGO | src/modules/orders/order.service.ts |
| PRD-ESC-01 | docs/PRD.md | Restrição | Integração exclusivamente outbound | TRANSCRICAO | [09:02] Marcos |
| PRD-ESC-02 | docs/PRD.md | Fora de Escopo | Excluir itens do payload e consultar detalhes pela API de pedidos | TRANSCRICAO | [09:43] Diego |
| PRD-ESC-03 | docs/PRD.md | Fora de Escopo | Adiar email de falhas | TRANSCRICAO | [09:37] Larissa |
| PRD-ESC-04 | docs/PRD.md | Fora de Escopo | Entregar API sem dashboard visual nesta fase | TRANSCRICAO | [09:40] Larissa |
| PRD-ESC-05 | docs/PRD.md | Questão em Aberto | Observar rajadas antes de decidir rate limiting por destino | TRANSCRICAO | [09:38] Diego; [09:39] Larissa |
| PRD-ESC-06 | docs/PRD.md | Fora de Escopo | Adiar arquivamento; cerca de 30 dias foi sugestão informal | TRANSCRICAO | [09:08] Diego |
| PRD-ESC-07 | docs/PRD.md | Fora de Escopo | Adiar múltiplos workers e reavaliar coordenação e ordem | TRANSCRICAO | [09:12] Diego; [09:13] Diego |
| PRD-ESC-08 | docs/PRD.md | Dependência | Produto documentará a integração via API e a deduplicação no portal | TRANSCRICAO | [09:26] Marcos; [09:40] Marcos |
| PRD-RF-001 | docs/PRD.md | Requisito Funcional | Cadastrar webhook via API autenticada com cliente explícito, URL HTTPS, filtros e devolução de segredo exclusivo gerado pela plataforma | TRANSCRICAO | [09:31] Marcos; [09:32] Larissa; [09:37] Sofia; [09:23] Sofia; [09:22] Sofia |
| PRD-RF-002 | docs/PRD.md | Requisito Funcional | Editar configuração de destino via API autenticada, com URL, estado ativo e filtro | TRANSCRICAO | [09:33] Bruno; [09:21] Bruno; [09:21] Sofia; [09:37] Sofia |
| PRD-RF-003 | docs/PRD.md | Requisito Funcional | Remover destino pela API autenticada | TRANSCRICAO | [09:33] Bruno; [09:37] Sofia |
| PRD-RF-004 | docs/PRD.md | Requisito Funcional | Listar destinos de um cliente explícito pela API autenticada | TRANSCRICAO | [09:32] Larissa; [09:33] Bruno; [09:37] Sofia |
| PRD-RF-005 | docs/PRD.md | Requisito Funcional | Filtrar cadastros ativos por status de destino na origem; sem interessados, confirmar mudança sem registrar entrega | TRANSCRICAO | [09:21] Bruno; [09:21] Sofia; [09:33] Marcos; [09:34] Bruno; [09:34] Diego |
| PRD-RF-005-SNAPSHOT | docs/PRD.md | Decisão | Registrar os dados do pedido no momento da mudança | TRANSCRICAO | [09:52] Larissa; [09:52] Diego |
| PRD-RF-006 | docs/PRD.md | Contrato Público | Enviar após o commit o JSON order.status_changed com UUID, instante, ID e número do pedido, status anterior e novo, cliente e total em centavos | TRANSCRICAO | [09:06] Diego; [09:43] Diego |
| PRD-RF-006-HEADERS | docs/PRD.md | Contrato Público | Informar cadastro, instante do envio e tipo de conteúdo em X-Webhook-Id, X-Timestamp e Content-Type | TRANSCRICAO | [09:44] Diego; [09:45] Diego |
| PRD-RF-007 | docs/PRD.md | Requisito Funcional | Enviar UUID único em X-Event-Id para identificação de duplicatas pelo consumidor | TRANSCRICAO | [09:25] Diego; [09:26] Larissa |
| PRD-RF-008 | docs/PRD.md | Requisito Funcional | Rotacionar segredo via API com validade paralela do anterior por 24 h | TRANSCRICAO | [09:21] Sofia; [09:22] Sofia |
| PRD-RF-009 | docs/PRD.md | Requisito Funcional | Retry automático limitado a cinco tentativas totais | TRANSCRICAO | [09:17] Larissa; [09:42] Diego |
| PRD-RF-010 | docs/PRD.md | Requisito Funcional | Persistir falha esgotada em DLQ separada com payload, motivo e data/hora | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| PRD-RF-011 | docs/PRD.md | Requisito Funcional | Consultar histórico por destino com resultado, payload, resposta quando disponível e duração; últimos 100 foram exemplo | TRANSCRICAO | [09:34] Marcos |
| PRD-RF-012 | docs/PRD.md | Requisito Funcional | Replay manual restrito a ADMIN recoloca entrega como pendente e audita autor | TRANSCRICAO | [09:18] Diego; [09:19] Larissa; [09:36] Sofia; [09:36] Larissa |
| PRD-RNF-001 | docs/PRD.md | Requisito Não Funcional | Confirmar intenção com status, histórico e estoque; falha de enqueue reverte a transação, e falha HTTP ocorre depois | TRANSCRICAO | [09:06] Diego; [09:40] Bruno |
| PRD-RNF-002 | docs/PRD.md | Requisito Não Funcional | Exigir HTTPS e rejeitar URL HTTP com erro de validação | TRANSCRICAO | [09:23] Sofia |
| PRD-RNF-003 | docs/PRD.md | Requisito Não Funcional | Autenticar o corpo por HMAC-SHA256 com segredo por destino em X-Signature | TRANSCRICAO | [09:20] Sofia; [09:22] Sofia; [09:44] Diego |
| PRD-RNF-004 | docs/PRD.md | Requisito Não Funcional | Rejeitar payload acima de 64 KB com erro, sem truncamento | TRANSCRICAO | [09:23] Sofia; [09:24] Larissa |
| PRD-RNF-005 | docs/PRD.md | Requisito Não Funcional | Tratar ausência de resposta em 10 segundos como falha sujeita a retry | TRANSCRICAO | [09:42] Diego |
| PRD-RNF-006 | docs/PRD.md | Requisito Não Funcional | Isolar reinícios da API com worker único e separado; polling a cada 2 segundos | TRANSCRICAO | [09:10] Larissa; [09:11] Diego; [09:12] Diego |
| PRD-RNF-007 | docs/PRD.md | Requisito Não Funcional | At-least-once admite duplicatas e exige deduplicação pelo consumidor | TRANSCRICAO | [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| PRD-RNF-007-ORDEM | docs/PRD.md | Restrição | Ordem prevista por pedido com um worker, sem garantia global | TRANSCRICAO | [09:12] Diego; [09:13] Larissa |
| PRD-RNF-008 | docs/PRD.md | Integração | Reusar padrões de Zod, Pino, AppError e middleware; erros próprios com prefixo WEBHOOK_ | TRANSCRICAO | [09:30] Larissa; [09:29] Larissa |
| PRD-DEC-01 | docs/PRD.md | Decisão | Reusar MySQL e Prisma para o processamento separado e a integração transacional | TRANSCRICAO | [09:08] Larissa; [09:11] Diego; [09:30] Bruno; [09:41] Diego |
| PRD-TRD-01 | docs/PRD.md | Trade-off | Retry limitado transfere recuperação após esgotamento à operação administrativa | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| PRD-TRD-02 | docs/PRD.md | Trade-off | Segredo exclusivo limita exposição e exige gestão de janela de migração pelo destinatário | TRANSCRICAO | [09:21] Sofia |
| PRD-TRD-03 | docs/PRD.md | Trade-off | At-least-once reduz coordenação e transfere deduplicação ao consumidor | TRANSCRICAO | [09:25] Diego |
| PRD-Q-01 | docs/PRD.md | Questão em Aberto | Resolver a divergência entre cinco tentativas totais e cinco esperas | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| PRD-Q-04 | docs/PRD.md | Risco | Cadastro público aceita ADMIN, comprometendo a barreira de papel proposta para replay | CODIGO | src/modules/auth/auth.routes.ts; src/modules/auth/auth.schemas.ts; src/modules/auth/auth.controller.ts; src/modules/auth/auth.service.ts; src/modules/users/user.service.ts; src/modules/users/user.repository.ts |
| PRD-Q-05 | docs/PRD.md | Risco | X-Timestamp foi sugerido para detectar replay, mas fica fora do corpo assinado | TRANSCRICAO | [09:44] Diego; [09:22] Sofia |
| PRD-PLAN-01 | docs/PRD.md | Restrição | Estimativa de três sprints incluindo revisão | TRANSCRICAO | [09:47] Larissa |
| PRD-PLAN-02 | docs/PRD.md | Questão em Aberto | Atlas pediu fim de novembro; Marcos ficou de confirmar prazo | TRANSCRICAO | [09:45] Marcos; [09:47] Marcos |
| PRD-DEP-01 | docs/PRD.md | Dependência | Pedidos fornece transições e contexto do pedido, com referência ao cliente | CODIGO | src/modules/orders/order.service.ts |
| PRD-DEP-02 | docs/PRD.md | Dependência | JWT atual identifica usuário e papel | CODIGO | src/middlewares/auth.middleware.ts |
| PRD-DEP-03 | docs/PRD.md | Dependência | Reservar ao menos dois dias úteis para revisão de segurança pré-deploy | TRANSCRICAO | [09:46] Sofia; [09:47] Larissa |
| PRD-RISCO-01 | docs/PRD.md | Risco | Confirmar status sem registrar intenção se enqueue ficar fora da transação | TRANSCRICAO | [09:40] Bruno; [09:41] Diego |
| PRD-RISCO-03 | docs/PRD.md | Risco | Destino indisponível pode esgotar o orçamento de tentativas | TRANSCRICAO | [09:15] Diego; [09:16] Diego; [09:17] Larissa |
| PRD-CA-001 | docs/PRD.md | Critério de Aceitação | Criação autenticada com HTTPS, filtros e segredo único; listagem inclui cadastro e HTTP é rejeitado | TRANSCRICAO | [09:31] Marcos; [09:32] Larissa; [09:33] Bruno; [09:23] Sofia; [09:21] Sofia |
| PRD-CA-002 | docs/PRD.md | Critério de Aceitação | Mudanças futuras usam o filtro atual e deixam de selecionar cadastros desativados ou removidos | TRANSCRICAO | [09:33] Bruno; [09:33] Marcos; [09:34] Bruno; [09:21] Sofia |
| PRD-CA-003 | docs/PRD.md | Critério de Aceitação | Notificação elegível mantém snapshot original mesmo após nova mudança; mudança válida sem inscritos não gera entrega | TRANSCRICAO | [09:52] Larissa; [09:34] Bruno |
| PRD-CA-004 | docs/PRD.md | Critério de Aceitação | Falha de enqueue reverte efeitos; consumidor offline e reinício isolado da API preservam o processamento independente | TRANSCRICAO | [09:40] Bruno; [09:06] Diego; [09:11] Diego |
| PRD-CA-005 | docs/PRD.md | Critério de Aceitação | Validar integridade do corpo e coexistência de segredos nas primeiras 24 h | TRANSCRICAO | [09:19] Sofia; [09:22] Sofia |
| PRD-CA-006 | docs/PRD.md | Critério de Aceitação | Timeout em 10 s aciona agenda acordada e esgotamento persiste falha com payload, motivo e instante | TRANSCRICAO | [09:42] Diego; [09:17] Larissa; [09:18] Diego; [09:48] Larissa |
| PRD-CA-007 | docs/PRD.md | Critério de Aceitação | Com worker único e entregas sem retry, observar ordem por pedido e deduplicar por ID | TRANSCRICAO | [09:12] Diego; [09:13] Larissa; [09:25] Diego |
| PRD-CA-008 | docs/PRD.md | Critério de Aceitação | Histórico mostra resultado, payload, duração e resposta quando disponível | TRANSCRICAO | [09:34] Marcos |
| PRD-CA-009 | docs/PRD.md | Critério de Aceitação | Somente ADMIN pode agendar replay e o autor deve ser registrado | TRANSCRICAO | [09:18] Diego; [09:36] Sofia; [09:36] Larissa |
| PRD-CA-011 | docs/PRD.md | Critério de Aceitação | Acima de 64 KB, retornar erro e preservar conteúdo sem truncar | TRANSCRICAO | [09:23] Sofia; [09:24] Larissa |
| PRD-TESTE-03 | docs/PRD.md | Estratégia de Teste | Executar testes ponta a ponta na integração do serviço de Pedidos | TRANSCRICAO | [09:46] Larissa |
| PRD-CA-003-CONTRATO | docs/PRD.md | Critério de Aceitação | Para a mudança elegível, verificar conteúdo do JSON e headers de envio acordados | TRANSCRICAO | [09:43] Diego; [09:44] Diego; [09:45] Diego |
| RFC-CTX-01 | docs/RFC.md | Contexto | Três clientes B2B querem substituir polling caro e lento em GET /orders por notificações outbound | TRANSCRICAO | [09:00] Marcos; [09:02] Marcos |
| RFC-CTX-02 | docs/RFC.md | Risco | Atlas sinalizou migração para concorrente se a necessidade não for atendida | TRANSCRICAO | [09:00] Marcos |
| RFC-OBJ-01 | docs/RFC.md | Métrica | Meta de notificar em menos de dez segundos | TRANSCRICAO | [09:02] Marcos |
| RFC-INT-01 | docs/RFC.md | Integração | Plataforma modular REST TypeScript/Express usa Prisma/MySQL | CODIGO | src/app.ts; package.json; prisma/schema.prisma |
| RFC-INT-02 | docs/RFC.md | Integração | Mudança de status transaciona histórico e estoque; débito em PENDING → PAID | CODIGO | src/modules/orders/order.service.ts; src/modules/orders/order.status.ts |
| RFC-INT-03 | docs/RFC.md | Integração | Código atual sem rotas webhook, modelos de outbox/DLQ ou script de worker | CODIGO | src/routes/index.ts; prisma/schema.prisma; package.json |
| RFC-RF-01 | docs/RFC.md | Requisito Funcional | Criar destino com URL, estado ativo, filtro e segredo gerado e devolvido | TRANSCRICAO | [09:31] Marcos; [09:21] Bruno; [09:21] Sofia |
| RFC-RF-02 | docs/RFC.md | Requisito Funcional | Editar configuração via PATCH | TRANSCRICAO | [09:33] Bruno |
| RFC-RF-03 | docs/RFC.md | Requisito Funcional | Remover configuração via DELETE | TRANSCRICAO | [09:33] Bruno |
| RFC-RF-04 | docs/RFC.md | Requisito Funcional | Listar webhooks por cliente via GET | TRANSCRICAO | [09:33] Bruno |
| RFC-RF-05 | docs/RFC.md | Requisito Funcional | Rotacionar segredo via API | TRANSCRICAO | [09:21] Sofia |
| RFC-RF-06 | docs/RFC.md | Requisito Funcional | Consultar histórico de entregas | TRANSCRICAO | [09:34] Marcos |
| RFC-RF-07 | docs/RFC.md | Requisito Funcional | Replay manual da DLQ por ADMIN com auditoria do autor | TRANSCRICAO | [09:18] Diego; [09:19] Larissa; [09:36] Sofia; [09:36] Larissa |
| RFC-REG-01 | docs/RFC.md | Regra de Negócio | CRUD admite qualquer papel autenticado | TRANSCRICAO | [09:37] Sofia |
| RFC-REG-02 | docs/RFC.md | Regra de Negócio | Identificar cliente em body ou path, sem extrair do JWT | TRANSCRICAO | [09:32] Larissa |
| RFC-REG-03 | docs/RFC.md | Regra de Negócio | Filtrar na inserção por status de destino e dispensar enqueue sem inscritos | TRANSCRICAO | [09:33] Marcos; [09:34] Bruno; [09:34] Diego |
| RFC-SEG-01 | docs/RFC.md | Restrição | HTTPS obrigatório | TRANSCRICAO | [09:23] Sofia |
| RFC-SEG-02 | docs/RFC.md | Decisão | HMAC-SHA256 sobre o corpo com segredo por endpoint | TRANSCRICAO | [09:22] Sofia |
| RFC-SEG-03 | docs/RFC.md | Restrição | Segredo anterior válido por 24 h | TRANSCRICAO | [09:22] Sofia |
| RFC-LIM-01 | docs/RFC.md | Restrição | Limite de 64 KB com erro e sem truncamento | TRANSCRICAO | [09:23] Sofia; [09:24] Larissa |
| RFC-LIM-02 | docs/RFC.md | Restrição | Timeout HTTP de 10 s tratado como falha | TRANSCRICAO | [09:42] Diego |
| RFC-ESC-01 | docs/RFC.md | Fora de Escopo | Email de fallback adiado | TRANSCRICAO | [09:37] Larissa |
| RFC-ESC-02 | docs/RFC.md | Fora de Escopo | Dashboard visual adiado | TRANSCRICAO | [09:40] Larissa |
| RFC-DEC-01 | docs/RFC.md | Decisão | Outbox MySQL participa da transação de status e falha na gravação reverte alteração | TRANSCRICAO | [09:08] Larissa; [09:06] Diego; [09:40] Bruno |
| RFC-DEC-02 | docs/RFC.md | Decisão | HTTP após commit preserva confirmação do pedido independentemente do destino | TRANSCRICAO | [09:04] Bruno; [09:04] Larissa; [09:06] Diego |
| RFC-DEC-03 | docs/RFC.md | Decisão | Snapshot na mudança para preservar estado original, com UUID | TRANSCRICAO | [09:25] Diego; [09:51] Larissa; [09:52] Larissa |
| RFC-DEC-05 | docs/RFC.md | Decisão | Um worker em processo separado | TRANSCRICAO | [09:11] Diego; [09:12] Diego |
| RFC-DEC-06 | docs/RFC.md | Decisão | Polling de 2 segundos | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| RFC-DEC-07 | docs/RFC.md | Decisão | Prisma próprio por processo, mesmo banco | TRANSCRICAO | [09:30] Bruno |
| RFC-DEC-08 | docs/RFC.md | Restrição | Ordem por pedido com worker único, sem ordem global | TRANSCRICAO | [09:12] Diego; [09:13] Larissa |
| RFC-DEC-11 | docs/RFC.md | Decisão | At-least-once com X-Event-Id para deduplicação no consumidor | TRANSCRICAO | [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| RFC-DEC-12 | docs/RFC.md | Decisão | Retry finito com cinco tentativas totais e recuperação manual após o limite | TRANSCRICAO | [09:15] Diego; [09:17] Larissa; [09:19] Larissa |
| RFC-DEC-13 | docs/RFC.md | Decisão | DLQ em tabela separada preserva payload, motivo e timestamp | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| RFC-CON-01 | docs/RFC.md | Contrato Público | JSON enxuto e headers de envio definidos na conversa, com X-Webhook-Id | TRANSCRICAO | [09:43] Diego; [09:44] Diego; [09:45] Diego |
| RFC-SEG-04 | docs/RFC.md | Restrição | X-Timestamp de envio fica fora do HMAC sobre o corpo | TRANSCRICAO | [09:22] Sofia; [09:44] Diego |
| RFC-INT-04 | docs/RFC.md | Integração | Reusar organização modular, Zod, Pino, AppError e middleware | TRANSCRICAO | [09:27] Bruno; [09:30] Larissa |
| RFC-ALT-01 | docs/RFC.md | Alternativa Descartada | HTTP síncrono prolongaria a transação e acoplaria falha externa ao negócio | TRANSCRICAO | [09:04] Bruno; [09:04] Larissa; [09:06] Diego |
| RFC-ALT-02 | docs/RFC.md | Alternativa Descartada | Redis Streams adicionaria infraestrutura excessiva para equipe pequena | TRANSCRICAO | [09:07] Larissa; [09:07] Diego |
| RFC-ALT-03 | docs/RFC.md | Alternativa Descartada | Trigger não notifica processo externo diretamente; polling foi escolhido | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| RFC-ALT-04 | docs/RFC.md | Alternativa Descartada | Worker junto à API acoplaria reinícios | TRANSCRICAO | [09:11] Diego |
| RFC-ALT-05 | docs/RFC.md | Alternativa Descartada | Exactly-once exigiria coordenação mais complexa entre as duas partes | TRANSCRICAO | [09:25] Diego; [09:26] Larissa |
| RFC-ALT-06 | docs/RFC.md | Alternativa Descartada | Retry indefinido deixaria eventos pendurados | TRANSCRICAO | [09:15] Diego; [09:17] Larissa |
| RFC-ALT-07 | docs/RFC.md | Alternativa Descartada | Três tentativas insuficientes para manutenção de duas horas | TRANSCRICAO | [09:16] Diego; [09:17] Larissa |
| RFC-ALT-08 | docs/RFC.md | Alternativa Descartada | Falha apenas na outbox perde separação e clareza para investigação | TRANSCRICAO | [09:17] Larissa; [09:18] Diego; [09:48] Larissa |
| RFC-ALT-09 | docs/RFC.md | Alternativa Descartada | Segredo global ampliaria impacto de vazamento | TRANSCRICAO | [09:21] Sofia |
| RFC-ALT-10 | docs/RFC.md | Alternativa Descartada | Montar payload no envio pode refletir estado posterior | TRANSCRICAO | [09:51] Bruno; [09:52] Larissa |
| RFC-Q-01 | docs/RFC.md | Questão em Aberto | Observar rate limiting de saída para decidir depois | TRANSCRICAO | [09:38] Diego; [09:39] Larissa |
| RFC-Q-02 | docs/RFC.md | Questão em Aberto | Arquivamento adiado; 30 dias foi sugestão sem política aprovada | TRANSCRICAO | [09:08] Diego |
| RFC-Q-03 | docs/RFC.md | Questão em Aberto | Adiar escala; locks e particionamento foram aventados | TRANSCRICAO | [09:12] Diego; [09:13] Diego |
| RFC-Q-04 | docs/RFC.md | Questão em Aberto | Resolver divergência de cinco chamadas totais e cinco esperas | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| RFC-Q-07 | docs/RFC.md | Risco | Cadastro público permite aceitar ADMIN, fragilizando requireRole no replay | CODIGO | src/modules/auth/auth.routes.ts; src/modules/auth/auth.schemas.ts; src/modules/auth/auth.controller.ts; src/modules/auth/auth.service.ts; src/modules/users/user.service.ts; src/modules/users/user.repository.ts |
| RFC-RISCO-04 | docs/RFC.md | Risco | Status e estoque têm leitura e atualização separadas; transação atual não introduz trava explícita nessa leitura | CODIGO | src/modules/orders/order.service.ts |
| RFC-PLAN-01 | docs/RFC.md | Dependência | Larissa revisará design com Bruno e Diego antes da codificação | TRANSCRICAO | [09:50] Larissa |
| RFC-PLAN-02 | docs/RFC.md | Dependência | Sofia pediu dois dias úteis de revisão de segurança antes do deploy | TRANSCRICAO | [09:46] Sofia |
| RFC-PLAN-03 | docs/RFC.md | Restrição | Estimativa verbal de três sprints incluindo revisão | TRANSCRICAO | [09:47] Larissa |
| RFC-PLAN-04 | docs/RFC.md | Questão em Aberto | Atlas pediu fim de novembro; Marcos ficou de confirmar prazo | TRANSCRICAO | [09:45] Marcos; [09:47] Marcos |
| RFC-ESC-03 | docs/RFC.md | Restrição | Feature produz notificações outbound para mudanças de status | TRANSCRICAO | [09:02] Marcos; [09:43] Diego |
| RFC-DEP-01 | docs/RFC.md | Dependência | Recebedor precisa validar corpo e reconhecer duplicatas | TRANSCRICAO | [09:19] Sofia; [09:25] Diego |
| RFC-TRD-02 | docs/RFC.md | Trade-off | Processo independente exige nova instância Prisma e operação própria | TRANSCRICAO | [09:11] Diego; [09:30] Bruno |
| RFC-TRD-03 | docs/RFC.md | Trade-off | A leitura periódica usa espera de polling em troca de simplicidade | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| RFC-TRD-05 | docs/RFC.md | Trade-off | Retry finito não assegura recepção de destino permanentemente indisponível | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| FDD-CTX-01 | docs/FDD.md | Contexto | Clientes B2B querem substituir consultas periódicas por notificações outbound | TRANSCRICAO | [09:00] Marcos; [09:02] Marcos |
| FDD-INT-01 | docs/FDD.md | Integração | Aplicação modular TypeScript/Express compõe dependências manualmente e monta API em /api/v1 | CODIGO | src/app.ts; package.json |
| FDD-INT-02 | docs/FDD.md | Integração | changeStatus lê, valida e grava status, histórico e estoque aplicável em transação Prisma | CODIGO | src/modules/orders/order.service.ts |
| FDD-INT-03 | docs/FDD.md | Regra de Negócio | Débito só em PENDING → PAID | CODIGO | src/modules/orders/order.status.ts |
| FDD-INT-04 | docs/FDD.md | Regra de Negócio | Cancelamento de PAID/PROCESSING repõe estoque | CODIGO | src/modules/orders/order.status.ts |
| FDD-INT-05 | docs/FDD.md | Integração | Router e schema atuais não contêm rotas webhook, nem tabelas de outbox/DLQ; manifesto não tem script de worker | CODIGO | src/routes/index.ts; prisma/schema.prisma; package.json |
| FDD-INT-06 | docs/FDD.md | Integração | Schema usa provider MySQL e gera o cliente Prisma | CODIGO | prisma/schema.prisma |
| FDD-OBJ-01 | docs/FDD.md | Métrica | Meta de notificar em menos de 10 s | TRANSCRICAO | [09:02] Marcos |
| FDD-ESC-01 | docs/FDD.md | Fora de Escopo | Excluir inbound | TRANSCRICAO | [09:02] Marcos |
| FDD-ESC-02 | docs/FDD.md | Fora de Escopo | Escopo de emissão limitado às mudanças de status | TRANSCRICAO | [09:43] Diego; [09:40] Bruno |
| FDD-ESC-03 | docs/FDD.md | Fora de Escopo | Excluir itens do payload | TRANSCRICAO | [09:43] Diego |
| FDD-ESC-04 | docs/FDD.md | Fora de Escopo | Excluir email | TRANSCRICAO | [09:37] Larissa |
| FDD-ESC-05 | docs/FDD.md | Fora de Escopo | Excluir dashboard | TRANSCRICAO | [09:40] Larissa |
| FDD-ESC-06 | docs/FDD.md | Alternativa Descartada | Descartar broker Redis para evitar infraestrutura extra | TRANSCRICAO | [09:07] Diego |
| FDD-ESC-07 | docs/FDD.md | Alternativa Descartada | Descartar exactly-once pela coordenação exigida | TRANSCRICAO | [09:25] Diego; [09:26] Larissa |
| FDD-ESC-08 | docs/FDD.md | Restrição | Sem garantia de ordem global | TRANSCRICAO | [09:13] Larissa |
| FDD-ESC-09 | docs/FDD.md | Fora de Escopo | Adiar múltiplos workers | TRANSCRICAO | [09:12] Diego; [09:13] Diego |
| FDD-ESC-10 | docs/FDD.md | Questão em Aberto | Observar necessidade de rate limiting | TRANSCRICAO | [09:38] Diego; [09:39] Larissa |
| FDD-ESC-11 | docs/FDD.md | Fora de Escopo | Arquivamento adiado; cerca de 30 dias foi sugestão | TRANSCRICAO | [09:08] Diego |
| FDD-MOD-01 | docs/FDD.md | Decisão | Organizar módulo webhooks com controller, service, repository, rotas e schemas | TRANSCRICAO | [09:27] Bruno; [09:28] Diego; [09:30] Larissa |
| FDD-MOD-02 | docs/FDD.md | Decisão | Entry point src/worker.ts com lógica no módulo, independente da API | TRANSCRICAO | [09:11] Larissa; [09:28] Bruno; [09:28] Diego |
| FDD-MOD-03 | docs/FDD.md | Integração | Seguir composição manual, classes e convenção ESM .js | CODIGO | src/app.ts |
| FDD-MOD-04 | docs/FDD.md | Integração | Handlers propagam next(err) | CODIGO | src/modules/orders/order.controller.ts |
| FDD-MOD-05 | docs/FDD.md | Decisão | Publisher usa cliente da transação atual e evita injetar repository inteiro | TRANSCRICAO | [09:41] Bruno; [09:41] Diego |
| FDD-DATA-01 | docs/FDD.md | Integração | Schema atual usa camelCase, mapeamento de tabelas e UUID CHAR(36) | CODIGO | prisma/schema.prisma |
| FDD-DATA-02 | docs/FDD.md | Decisão | Modelar cadastro com URL, segredo, customer_id e indicador ativo | TRANSCRICAO | [09:21] Bruno; [09:21] Sofia |
| FDD-DATA-05 | docs/FDD.md | Decisão | Persistir snapshot imutável com UUID na mudança | TRANSCRICAO | [09:25] Diego; [09:51] Larissa; [09:52] Larissa |
| FDD-DATA-10 | docs/FDD.md | Decisão | Persistir falha em DLQ separada com payload, motivo e instante | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| FDD-DATA-13 | docs/FDD.md | Restrição | Limite de 64 KB: erro e ausência de truncamento | TRANSCRICAO | [09:23] Sofia; [09:24] Larissa |
| FDD-DATA-15 | docs/FDD.md | Proposta | Indexar estado e criação para ler pendências antigas em lotes pequenos | TRANSCRICAO | [09:08] Diego; [09:09] Diego |
| FDD-TX-03 | docs/FDD.md | Risco | Validação e decremento de estoque são etapas distintas, sem trava explícita nessa leitura | CODIGO | src/modules/orders/order.service.ts |
| FDD-FLUXO-01 | docs/FDD.md | Integração | PATCH /api/v1/orders/:id/status chama o controller de status | CODIGO | src/app.ts; src/routes/index.ts; src/modules/orders/order.routes.ts |
| FDD-FLUXO-02 | docs/FDD.md | Integração | Controller passa id, body e usuário ao serviço, devolvendo pedido atualizado | CODIGO | src/modules/orders/order.controller.ts |
| FDD-FLUXO-03 | docs/FDD.md | Regra de Negócio | Mesmo status ou transição proibida gera erro no fluxo atual | CODIGO | src/modules/orders/order.service.ts |
| FDD-FLUXO-04 | docs/FDD.md | Regra de Negócio | Filtrar por cliente e status de destino na inserção; zero registros sem interessados | TRANSCRICAO | [09:33] Marcos; [09:34] Bruno; [09:34] Diego |
| FDD-FLUXO-05 | docs/FDD.md | Decisão | Falha de enqueue reverte status, histórico e estoque aplicável; HTTP ocorre após commit | TRANSCRICAO | [09:06] Diego; [09:40] Bruno |
| FDD-WORK-01 | docs/FDD.md | Decisão | Rodar inicialmente um worker separado, isolado dos reinícios da API | TRANSCRICAO | [09:11] Diego; [09:12] Diego |
| FDD-WORK-02 | docs/FDD.md | Restrição | Polling com intervalo de dois segundos | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| FDD-WORK-03 | docs/FDD.md | Restrição | Ordem por pedido discutida para um único worker | TRANSCRICAO | [09:12] Diego; [09:13] Larissa |
| FDD-WORK-04 | docs/FDD.md | Decisão | Mesmo banco e DATABASE_URL, instância Prisma própria do processo | TRANSCRICAO | [09:30] Bruno |
| FDD-WORK-05 | docs/FDD.md | Proposta | Criar script npm run worker | TRANSCRICAO | [09:11] Larissa |
| FDD-WORK-09 | docs/FDD.md | Restrição | Aplicar timeout HTTP de 10 s e tratar como falha para retry | TRANSCRICAO | [09:42] Diego |
| FDD-WORK-12 | docs/FDD.md | Integração | Bootstrap atual da API chama process.exit após desconectar Prisma no shutdown | CODIGO | src/server.ts |
| FDD-RETRY-01 | docs/FDD.md | Decisão | Limitar a cinco tentativas totais por ciclo | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| FDD-RETRY-02 | docs/FDD.md | Questão em Aberto | Cinco tentativas totais foram enunciadas junto com cinco esperas | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| FDD-DLQ-02 | docs/FDD.md | Requisito Funcional | Replay manual retorna falha da DLQ à fila pendente | TRANSCRICAO | [09:18] Diego; [09:19] Larissa |
| FDD-DLQ-03 | docs/FDD.md | Requisito de Segurança | Replay exige ADMIN e registra autor | TRANSCRICAO | [09:36] Sofia; [09:36] Larissa |
| FDD-API-01 | docs/FDD.md | Contrato Público | CRUD via JWT com qualquer role autenticada | TRANSCRICAO | [09:32] Marcos; [09:32] Larissa; [09:37] Sofia |
| FDD-API-02 | docs/FDD.md | Integração | AuthUser possui id, email e role; authenticate usa Bearer e requireRole confere papel | CODIGO | src/middlewares/auth.middleware.ts |
| FDD-API-03 | docs/FDD.md | Integração | Schema não liga usuários a clientes | CODIGO | prisma/schema.prisma |
| FDD-API-04 | docs/FDD.md | Risco | Registro público aceita ADMIN e preserva papel no serviço | CODIGO | src/modules/auth/auth.routes.ts; src/modules/auth/auth.schemas.ts; src/modules/auth/auth.controller.ts; src/modules/auth/auth.service.ts; src/modules/users/user.service.ts; src/modules/users/user.repository.ts |
| FDD-API-06 | docs/FDD.md | Contrato Público | Schema e API de Pedidos usam nomes camelCase e IDs UUID, como referência de convenção | CODIGO | prisma/schema.prisma; src/modules/orders/order.schemas.ts |
| FDD-API-07 | docs/FDD.md | Integração | Padrão de paginação de pedidos usa page >= 1 e pageSize de 1 a 100, com padrões 1 e 20 | CODIGO | src/modules/orders/order.schemas.ts |
| FDD-API-08 | docs/FDD.md | Integração | Helpers padronizam envelope data/pagination | CODIGO | src/shared/http/response.ts |
| FDD-API-10 | docs/FDD.md | Integração | Middleware atual reflete ou cria X-Request-Id | CODIGO | src/middlewares/request-logger.middleware.ts |
| FDD-API-11 | docs/FDD.md | Contrato Público | POST para cadastrar com URL, status, cliente explícito e devolução de segredo gerado | TRANSCRICAO | [09:31] Marcos; [09:32] Larissa |
| FDD-API-13 | docs/FDD.md | Contrato Público | GET autentica e lista cadastros de um cliente | TRANSCRICAO | [09:33] Bruno; [09:37] Sofia |
| FDD-API-15 | docs/FDD.md | Contrato Público | PATCH edita configuração e filtros | TRANSCRICAO | [09:33] Bruno; [09:33] Marcos |
| FDD-API-18 | docs/FDD.md | Contrato Público | DELETE remove destino | TRANSCRICAO | [09:33] Bruno |
| FDD-API-20 | docs/FDD.md | Contrato Público | Rotação via API mantém antigo válido por 24 h | TRANSCRICAO | [09:21] Sofia; [09:22] Sofia |
| FDD-API-23 | docs/FDD.md | Contrato Público | GET /webhooks/:id/deliveries mostra payload, resultado, resposta e tempo | TRANSCRICAO | [09:34] Marcos |
| FDD-API-26 | docs/FDD.md | Contrato Público | POST /admin/webhooks/dead-letter/:id/replay | TRANSCRICAO | [09:35] Diego |
| FDD-OUT-01 | docs/FDD.md | Contrato Público | JSON de mudança contém event_id, event_type, timestamp ISO 8601, IDs, número, estados e total em centavos | TRANSCRICAO | [09:43] Diego |
| FDD-OUT-02 | docs/FDD.md | Contrato Público | Headers X-Event-Id, X-Webhook-Id, X-Timestamp, X-Signature e Content-Type application/json | TRANSCRICAO | [09:44] Diego; [09:45] Diego |
| FDD-OUT-03 | docs/FDD.md | Decisão | Assinar corpo com HMAC-SHA256 e segredo por endpoint | TRANSCRICAO | [09:22] Sofia |
| FDD-OUT-04 | docs/FDD.md | Restrição | X-Timestamp de envio não integra HMAC sobre o corpo | TRANSCRICAO | [09:22] Sofia; [09:44] Diego |
| FDD-OUT-05 | docs/FDD.md | Decisão | At-least-once e deduplicação por identidade de evento | TRANSCRICAO | [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| FDD-ERR-BASE | docs/FDD.md | Decisão | Adotar prefixo WEBHOOK_ nos erros do módulo | TRANSCRICAO | [09:29] Larissa |
| FDD-ERR-APP | docs/FDD.md | Integração | AppError informa mensagem, statusCode, errorCode e detalhes | CODIGO | src/shared/errors/app-error.ts |
| FDD-ERR-MW | docs/FDD.md | Integração | Middleware trata AppError/Zod/Prisma e fallback de erro inesperado | CODIGO | src/middlewares/error.middleware.ts |
| FDD-ERR-VALID | docs/FDD.md | Integração | Validação atual aplica Zod a body/query/params e usa erro genérico | CODIGO | src/middlewares/validate.middleware.ts |
| FDD-ERR-AUTH | docs/FDD.md | Integração | Autenticação e papel têm erros genéricos atuais | CODIGO | src/middlewares/auth.middleware.ts |
| FDD-ERR-PARSER | docs/FDD.md | Integração | express.json de 1 MB precede logger e router | CODIGO | src/app.ts |
| FDD-SEG-01 | docs/FDD.md | Requisito de Segurança | Exigir HTTPS e validar com schema | TRANSCRICAO | [09:23] Sofia |
| FDD-OBS-01 | docs/FDD.md | Integração | Pino e redação de campos conhecidos já existem | CODIGO | src/shared/logger/index.ts |
| FDD-OBS-02 | docs/FDD.md | Integração | Logger HTTP próprio acrescenta ID e registra duração | CODIGO | src/middlewares/request-logger.middleware.ts |
| FDD-OBS-03 | docs/FDD.md | Integração | Manifesto não declara SDK de métricas ou tracing dedicado; pino-http consta como dependência | CODIGO | package.json |
| FDD-OBS-08 | docs/FDD.md | Integração | AppError não é logado automaticamente no ramo correspondente | CODIGO | src/middlewares/error.middleware.ts |
| FDD-DEP-01 | docs/FDD.md | Dependência | Manifesto declara Node >=20 e versões de Express, Prisma, Zod, Pino, uuid, Vitest e Supertest | CODIGO | package.json |
| FDD-DEP-03 | docs/FDD.md | Integração | Loader Zod valida ambiente e encerra em configuração inválida; carrega banco, JWT, porta e logging | CODIGO | src/config/env.ts |
| FDD-DEP-04 | docs/FDD.md | Integração | Schema declara SHADOW_DATABASE_URL | CODIGO | prisma/schema.prisma |
| FDD-DEP-07 | docs/FDD.md | Integração | Fábrica e singleton de PrismaClient existentes | CODIGO | src/config/database.ts |
| FDD-DEP-13 | docs/FDD.md | Integração | Compose configura MySQL local | CODIGO | docker-compose.yml |
| FDD-DEP-15 | docs/FDD.md | Integração | OrderRepository concentra leitura com relações e transação de listagem | CODIGO | src/modules/orders/order.repository.ts |
| FDD-DEP-16 | docs/FDD.md | Integração | Router agrega controllers e rotas dos módulos atuais | CODIGO | src/routes/index.ts |
| FDD-DEP-17 | docs/FDD.md | Integração | Setup de teste usa Prisma e limpa tabelas em ordem referencial antes de cada teste | CODIGO | tests/setup.ts |
| FDD-TESTE-01 | docs/FDD.md | Integração | Testes atuais usam Vitest/Supertest, buildApp com Prisma, MySQL e factories | CODIGO | tests/orders.test.ts; tests/setup.ts; tests/helpers/factories.ts |
| FDD-TESTE-02 | docs/FDD.md | Integração | Vitest configura arquivos sem paralelismo e forks/singleFork | CODIGO | vitest.config.ts |
| FDD-AC02 | docs/FDD.md | Critério de Aceitação | Falha da persistência de evento elegível reverte status, histórico e estoque aplicável | TRANSCRICAO | [09:40] Bruno; [09:06] Diego |
| FDD-AC03 | docs/FDD.md | Critério de Aceitação | Sem interessados, zero registros e mudança válida confirmada | TRANSCRICAO | [09:34] Bruno; [09:34] Diego |
| FDD-AC05 | docs/FDD.md | Critério de Aceitação | Consumidor offline não bloqueia mudança; worker envia fora da transação | TRANSCRICAO | [09:04] Bruno; [09:06] Diego; [09:11] Diego |
| FDD-AC07 | docs/FDD.md | Critério de Aceitação | HMAC valida corpo original e falha após adulteração | TRANSCRICAO | [09:19] Sofia; [09:22] Sofia |
| FDD-AC14-AUD | docs/FDD.md | Critério de Aceitação | Apenas ADMIN executa replay e autor é auditado | TRANSCRICAO | [09:36] Sofia; [09:36] Larissa |
| FDD-RISCO-01 | docs/FDD.md | Risco | Escrever outbox fora do tx perde garantia entre mudança e intenção | TRANSCRICAO | [09:40] Bruno; [09:41] Diego |
| FDD-RISCO-03 | docs/FDD.md | Risco | Consumidor indisponível pode esgotar retry e requerer replay | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| FDD-PLAN-01 | docs/FDD.md | Dependência | Sofia solicitou pelo menos dois dias úteis de revisão de segurança antes do deploy | TRANSCRICAO | [09:46] Sofia |
| FDD-PLAN-02 | docs/FDD.md | Restrição | Três sprints com revisão e prazo da Atlas a confirmar | TRANSCRICAO | [09:45] Marcos; [09:47] Larissa; [09:47] Marcos |
| FDD-TX-11 | docs/FDD.md | Restrição | Evitar rede e segundo cliente/transação no publisher | TRANSCRICAO | [09:40] Bruno; [09:41] Bruno; [09:41] Diego; [09:06] Diego |
| FDD-SEG-10 | docs/FDD.md | Integração | Pedido referencia Customer e usuário criador; histórico de status referencia o usuário autor | CODIGO | prisma/schema.prisma |
| FDD-OUT-13 | docs/FDD.md | Restrição | Centavos inteiros e estados do enum OrderStatus atual | CODIGO | prisma/schema.prisma |
| FDD-API-32 | docs/FDD.md | Decisão | Usar requireRole existente para replay | TRANSCRICAO | [09:36] Larissa |
| FDD-DEP-20 | docs/FDD.md | Dependência | Segurança precisa revisar HMAC e geração de segredo | TRANSCRICAO | [09:46] Sofia |
| FDD-ERR-28 | docs/FDD.md | Integração | Código de erro de Pedidos INVALID_STATUS_TRANSITION é usado na transição repetida | CODIGO | src/modules/orders/order.service.ts |
| FDD-ERR-29 | docs/FDD.md | Integração | Fallback de parser pode cair em INTERNAL_SERVER_ERROR quando erro não for AppError, Zod ou Prisma conhecido | CODIGO | src/app.ts; src/middlewares/error.middleware.ts |
| FDD-RISC-07 | docs/FDD.md | Risco | Fluxo de status e fluxo de exclusão leem e depois escrevem sem trava explícita na leitura | CODIGO | src/modules/orders/order.service.ts; src/modules/orders/order.repository.ts |
| FDD-ERR-PROP-01 | docs/FDD.md | Proposta | WEBHOOK_NOT_FOUND e WEBHOOK_INVALID_URL foram sugeridos como nomes de erro do módulo | TRANSCRICAO | [09:28] Bruno; [09:29] Larissa |
| FDD-INT-07 | docs/FDD.md | Integração | OrderStatus é a referência atual de estados e o schema de Pedidos valida o destino com esse enum | CODIGO | prisma/schema.prisma; src/modules/orders/order.schemas.ts |
| FDD-INT-08 | docs/FDD.md | Integração | OrderService.create cria o pedido PENDING com histórico de criação | CODIGO | src/modules/orders/order.service.ts |
| FDD-INT-09 | docs/FDD.md | Integração | O projeto já oferece GET /api/v1/orders/:id para consulta detalhada | CODIGO | src/app.ts; src/routes/index.ts; src/modules/orders/order.routes.ts |
| ADR-001 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Decisão | Aplicação usa MySQL relacional compartilhado com Prisma | CODIGO | prisma/schema.prisma; prisma/migrations/20260519182739_init/migration.sql |
| ADR-001-DEC-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Decisão | Reusar MySQL existente para a outbox e o mesmo commit dos dados de Pedidos | TRANSCRICAO | [09:06] Diego; [09:07] Diego; [09:08] Larissa |
| ADR-001-INT-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Integração | Compose demonstra serviço MySQL local com volume e healthcheck | CODIGO | docker-compose.yml |
| ADR-001-ALT-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Alternativa Descartada | Redis Streams adicionaria infraestrutura considerada excessiva | TRANSCRICAO | [09:07] Larissa; [09:07] Diego |
| ADR-001-TRD-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Trade-off | Há dependência do provider MySQL, modelos e migração que uma substituição precisaria revisar | CODIGO | prisma/schema.prisma; prisma/migrations/20260519182739_init/migration.sql |
| ADR-002 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Decisão | Projeto usa Prisma Client, schema declarativo e migrações; cliente e CLI declarados em 5.22.0 | CODIGO | package.json; prisma/schema.prisma; prisma/migrations/20260519182739_init/migration.sql |
| ADR-002-INT-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Integração | Fábrica fornece PrismaClient e composição injeta o cliente em repository e serviço de Pedidos | CODIGO | src/config/database.ts; src/app.ts |
| ADR-002-INT-02 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Integração | Repository faz consultas e serviço de Pedidos coordena alterações de múltiplos modelos no tx | CODIGO | src/modules/orders/order.repository.ts; src/modules/orders/order.service.ts |
| ADR-002-DEC-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Decisão | Futura emissão usa o tx recebido para commit conjunto | TRANSCRICAO | [09:40] Bruno; [09:41] Bruno; [09:41] Diego |
| ADR-002-DEC-02 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Decisão | Worker cria instância de Prisma própria por processo | TRANSCRICAO | [09:30] Bruno |
| ADR-002-ALT-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Alternativa Descartada | Preferir função que recebe o tx a injetar repository integral | TRANSCRICAO | [09:41] Bruno; [09:41] Diego |
| ADR-002-TRD-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Trade-off | Trocar Prisma exigiria adaptar consumidores acoplados a seus tipos e operações | CODIGO | src/config/database.ts; src/modules/orders/order.service.ts; src/modules/orders/order.repository.ts |
| ADR-003 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Decisão | Usar outbox MySQL na transação de mudança; falha no registro impede o commit | TRANSCRICAO | [09:06] Diego; [09:08] Larissa; [09:40] Bruno |
| ADR-003-INT-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Integração | Fluxo atual transaciona status, estoque aplicável e histórico | CODIGO | src/modules/orders/order.service.ts |
| ADR-003-INT-02 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Integração | Schema atual não contém outbox de webhook | CODIGO | prisma/schema.prisma |
| ADR-003-DEC-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Decisão | Envio HTTP ocorre em separado após commit | TRANSCRICAO | [09:06] Diego |
| ADR-003-DEC-02 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Decisão | Guardar snapshot na inserção com identidade UUID | TRANSCRICAO | [09:25] Diego; [09:51] Larissa; [09:52] Larissa |
| ADR-003-ALT-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Alternativa Descartada | Chamada síncrona prolongaria transação e acoplaria falha externa | TRANSCRICAO | [09:04] Bruno; [09:04] Larissa; [09:06] Diego |
| ADR-003-ALT-02 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Alternativa Descartada | Redis Streams exige infraestrutura adicional excessiva | TRANSCRICAO | [09:07] Larissa; [09:07] Diego |
| ADR-003-ALT-03 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Alternativa Descartada | Renderização tardia pode refletir estado posterior | TRANSCRICAO | [09:51] Bruno; [09:52] Larissa |
| ADR-003-TRD-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Trade-off | Escrita adicional é condição do commit, que garante somente intenção | TRANSCRICAO | [09:06] Diego; [09:40] Bruno; [09:24] Diego |
| ADR-004 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Decisão | Entrega outbound at-least-once com deduplicação no consumidor por X-Event-Id | TRANSCRICAO | [09:02] Marcos; [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| ADR-004-CON-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Contrato Público | UUID gerado na outbox também aparece no payload | TRANSCRICAO | [09:25] Diego; [09:43] Diego |
| ADR-004-ALT-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Alternativa Descartada | Exactly-once exige coordenação complexa entre produtor e consumidor | TRANSCRICAO | [09:25] Diego; [09:26] Larissa |
| ADR-004-TRD-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Trade-off | Consumidor assume deduplicação; duplicatas HTTP continuam possíveis | TRANSCRICAO | [09:25] Sofia; [09:25] Diego |
| ADR-004-DEP-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Dependência | Documentação ao consumidor dá destaque à possibilidade de repetição | TRANSCRICAO | [09:26] Marcos |
| ADR-004-INT-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Integração | Router atual não monta webhooks | CODIGO | src/routes/index.ts |
| ADR-005 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Decisão | HMAC-SHA256 sobre o corpo, segredo distinto por endpoint e envio de assinatura ao consumidor | TRANSCRICAO | [09:19] Sofia; [09:20] Sofia; [09:22] Sofia; [09:44] Diego |
| ADR-005-ALT-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Alternativa Descartada | Segredo global amplia alcance de vazamento | TRANSCRICAO | [09:21] Sofia |
| ADR-005-DEC-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Decisão | Rotação com segredo anterior válido em paralelo por 24 horas | TRANSCRICAO | [09:21] Sofia; [09:22] Sofia |
| ADR-005-SEG-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Restrição | Exigir HTTPS | TRANSCRICAO | [09:23] Sofia |
| ADR-005-CON-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Contrato Público | Timestamp de envio no header fica fora do corpo autenticado | TRANSCRICAO | [09:22] Sofia; [09:44] Diego |
| ADR-005-INT-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Integração | JWT atual autentica usuários da API | CODIGO | src/middlewares/auth.middleware.ts |
| ADR-005-INT-02 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Integração | Pino redige campos conhecidos sem campo de segredo de webhook | CODIGO | src/shared/logger/index.ts |
| ADR-005-TRD-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Trade-off | Rotação de credencial exclusiva exige migração do consumidor na janela de convivência | TRANSCRICAO | [09:21] Sofia |
| ADR-005-DEP-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Dependência | Revisão de segurança solicitada antes do deploy | TRANSCRICAO | [09:46] Sofia; [09:49] Sofia |
| ADR-006 | docs/adrs/ADR-006-worker-separado-com-polling.md | Decisão | Um worker separado lê a outbox por polling a cada 2 segundos | TRANSCRICAO | [09:09] Diego; [09:10] Larissa; [09:11] Diego; [09:12] Diego |
| ADR-006-OBJ-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Métrica | Atender à expectativa de notificar abaixo de dez segundos | TRANSCRICAO | [09:02] Marcos; [09:09] Diego |
| ADR-006-DEC-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Decisão | Reusar banco e stack, com Prisma próprio por processo | TRANSCRICAO | [09:11] Diego; [09:30] Bruno |
| ADR-006-DEC-02 | docs/adrs/ADR-006-worker-separado-com-polling.md | Decisão | Lógica do worker no módulo da feature e entry point independente | TRANSCRICAO | [09:11] Larissa; [09:28] Bruno; [09:28] Diego |
| ADR-006-ALT-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Alternativa Descartada | Worker na API compartilharia seus reinícios | TRANSCRICAO | [09:11] Diego |
| ADR-006-ALT-02 | docs/adrs/ADR-006-worker-separado-com-polling.md | Alternativa Descartada | Trigger não notifica diretamente processo externo | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| ADR-006-ESC-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Fora de Escopo | Escala horizontal adiada; locks e particionamento são hipóteses futuras | TRANSCRICAO | [09:12] Diego; [09:13] Diego |
| ADR-006-INT-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Integração | Bootstrap atual atende API; manifesto não contém comando de worker | CODIGO | src/server.ts; package.json |
| ADR-006-TRD-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Trade-off | Polling é consulta periódica com espera até a próxima busca | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| ADR-007 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão | Retentar até o limite de cinco tentativas totais | TRANSCRICAO | [09:15] Diego; [09:17] Larissa; [09:48] Larissa |
| ADR-007-DEC-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão | DLQ separada após esgotamento preserva informações para diagnóstico e replay | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| ADR-007-DEC-02 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão | Replay manual por ADMIN retorna a pendente e registra autor | TRANSCRICAO | [09:18] Diego; [09:19] Larissa; [09:36] Sofia; [09:36] Larissa |
| ADR-007-DEC-03 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Restrição | Timeout de 10 segundos seguido de retry | TRANSCRICAO | [09:42] Diego |
| ADR-007-ALT-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Alternativa Descartada | Retry indefinido deixa eventos pendurados | TRANSCRICAO | [09:15] Diego; [09:17] Larissa |
| ADR-007-ALT-02 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Alternativa Descartada | Três tentativas insuficientes para manutenção de duas horas | TRANSCRICAO | [09:16] Diego; [09:17] Larissa |
| ADR-007-ALT-03 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Alternativa Descartada | Falha na própria outbox dificulta separar pendências, investigar e recuperar | TRANSCRICAO | [09:17] Larissa; [09:18] Diego; [09:48] Larissa |
| ADR-007-TRD-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Trade-off | Após limite, recuperação exige intervenção manual | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| ADR-007-Q-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Questão em Aberto | Enunciado de cinco tentativas totais diverge da sequência de cinco intervalos | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| ADR-007-INT-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Integração | Schema atual ainda não inclui persistência de outbox/DLQ | CODIGO | prisma/schema.prisma |
| RFC-PLAN-06 | docs/RFC.md | Questão em Aberto derivada | Revisar a divergência entre o intervalo de 12 h e o limite de cinco tentativas totais | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| FDD-OUT-12 | docs/FDD.md | Contrato Público | Enviar o evento por chamada HTTP à URL inscrita; POST é o método proposto no FDD | TRANSCRICAO | [09:06] Diego; [09:21] Bruno; [09:21] Sofia |
| FDD-RETRY-03 | docs/FDD.md | Decisão derivada | Usar as quatro primeiras esperas para respeitar o total de cinco tentativas; manter os 12 h em revisão | TRANSCRICAO | [09:17] Diego; [09:48] Larissa |
| FDD-AC09 | docs/FDD.md | Critério de Aceitação derivado | Verificar quatro esperas entre cinco tentativas e encaminhar à DLQ ao esgotar o ciclo | TRANSCRICAO | [09:17] Diego; [09:18] Diego; [09:48] Larissa |
| ADR-004-DEC-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Decisão derivada | Aplicar o limite de cinco tentativas totais ao ciclo de entrega, com o quinto intervalo em revisão | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| ADR-007-DEC-04 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão derivada | Aplicar as quatro primeiras esperas para respeitar o total de cinco tentativas | TRANSCRICAO | [09:17] Diego; [09:48] Larissa |

## Itens sem evidência primária suficiente

As propostas e lacunas de validação abaixo permanecem no inventário. A coluna Documento localiza a definição ou a afirmação auditada. Esses vínculos são internos ao pacote e ficam fora da cobertura por fonte primária. A marca **Afirmação histórica sem comprovação** identifica atribuições de datas, motivações, alternativas ou metas históricas que as fontes disponíveis não sustentam.

### PRD

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| PRD-MET-01 | [PRD.md](PRD.md) | Medir do commit à resposta bem-sucedida da primeira tentativa e publicar distribuição e volume no piloto. |
| PRD-MET-02 | [PRD.md](PRD.md) | Definir percentil, janela, carga e condições saudáveis; separar retry, replay e bloqueios na aferição. |
| PRD-RF-013 | [PRD.md](PRD.md) | Resultados por destino, com ID compartilhado e preservação de identidade e conteúdo em retry e replay. |
| PRD-RF-014 | [PRD.md](PRD.md) | 2xx confirma sucesso e rejeição permanente vai diretamente à DLQ. |
| PRD-RF-015 | [PRD.md](PRD.md) | Paginar histórico em ordem decrescente com até 100 registros por página. |
| PRD-RF-016 | [PRD.md](PRD.md) | Replay abre novo ciclo, preserva o histórico, indica agendamento e retorna conflito em concorrência. |
| PRD-RNF-008-PEDIDOS | [PRD.md](PRD.md) | Preservar regras, respostas e erros atuais de Pedidos na integração. |
| PRD-RNF-009 | [PRD.md](PRD.md) | Proteger segredo em repouso e omiti-lo de consultas e logs, com exposição apenas na criação/rotação. |
| PRD-RNF-010 | [PRD.md](PRD.md) | Correlacionar mudança, evento, tentativa, resultado e replay; medir atrasos, pendências e DLQ sem segredos. |
| PRD-TRD-04 | [PRD.md](PRD.md) | Polling e novas escritas compartilham capacidade do banco; backlog e duração do envio afetam latência. |
| PRD-Q-02 | [PRD.md](PRD.md) | Validar bloqueio por pedido/destino durante retry e ordem do replay tardio. |
| PRD-Q-03 | [PRD.md](PRD.md) | Definir autorização por cliente antes da liberação B2B. |
| PRD-Q-06 | [PRD.md](PRD.md) | Validar assinatura secundária, retenção de IDs, recuperação de segredo e destinos permitidos. |
| PRD-Q-07 | [PRD.md](PRD.md) | Definir efeitos da edição, desativação e remoção sobre pendências e histórico. |
| PRD-Q-08 | [PRD.md](PRD.md) | Confirmar 65.536 bytes e rollback do status por payload excessivo. |
| PRD-Q-09 | [PRD.md](PRD.md) | Definir carga, coleta, alertas e responsáveis antes da homologação. |
| PRD-Q-10 | [PRD.md](PRD.md) | Deliberar formalmente sobre PRD/RFC/ADRs/FDD. |
| PRD-DEP-04 | [PRD.md](PRD.md) | Operar egress HTTPS/DNS, supervisão, chaves e monitoramento de capacidade. |
| PRD-DEP-05 | [PRD.md](PRD.md) | Atualizar todos os produtores antes de considerar emissão completa no rollout. |
| PRD-RISCO-02 | [PRD.md](PRD.md) | Reaplicar efeitos após replay tardio e expiração da janela de deduplicação. |
| PRD-RISCO-04 | [PRD.md](PRD.md) | Isolamento insuficiente e acesso a destino interno exigem revisão antes de liberar. |
| PRD-RISCO-05 | [PRD.md](PRD.md) | Contenção no MySQL, backlog e crescimento do histórico exigem validação de capacidade e retenção. |
| PRD-RISCO-06 | [PRD.md](PRD.md) | Probabilidades e impactos qualitativos atribuídos a R-001 a R-005. |
| PRD-CA-001-ACESSO | [PRD.md](PRD.md) | Validar o vínculo de acesso ao cliente no cadastro e na consulta. |
| PRD-CA-003-INVALIDA | [PRD.md](PRD.md) | Transição inválida preserva erro de Pedidos e não emite evento. |
| PRD-CA-008-PAG | [PRD.md](PRD.md) | Páginas consecutivas em conjunto fixo vêm em ordem decrescente, sem lacunas ou duplicatas. |
| PRD-CA-009-CONC | [PRD.md](PRD.md) | Dois replays concorrentes geram um ciclo; o segundo retorna conflito. |
| PRD-CA-010 | [PRD.md](PRD.md) | Validar recuperação independente de destinos e encaminhamento de rejeição permanente à DLQ. |
| PRD-CA-012 | [PRD.md](PRD.md) | Validar correlação sem credenciais e medir distribuição de atrasos sob carga acordada. |
| PRD-TESTE-01 | [PRD.md](PRD.md) | Cobertura funcional e de contrato para operações, erros, payload e headers. |
| PRD-TESTE-02 | [PRD.md](PRD.md) | Injetar falha de enqueue e executar regressão de Pedidos. |
| PRD-TESTE-04 | [PRD.md](PRD.md) | Usar destino e relógio controlados para testar retry, reinício, ordem e duplicatas. |
| PRD-TESTE-05 | [PRD.md](PRD.md) | Validar TLS, integridade, fronteira da rotação, sigilo e autorização em testes. |
| PRD-TESTE-06 | [PRD.md](PRD.md) | Homologar com piloto e carga representativa, medindo primeira entrega, backlog e impacto em Pedidos. |
| PRD-MIT-01 | [PRD.md](PRD.md) | Confirmar atualização de todos os produtores antes do piloto e simular rollback em teste de integração. |
| PRD-MIT-02 | [PRD.md](PRD.md) | Avaliar janela de deduplicação antes do replay tardio e validar com consumidor de teste. |
| PRD-MIT-03 | [PRD.md](PRD.md) | Definir responsável por falhas e diagnosticar com histórico/DLQ antes de replay. |
| PRD-MIT-04 | [PRD.md](PRD.md) | Condicionar liberação à revisão de atribuição de papéis, destinos e limites de acesso. |
| PRD-MIT-05 | [PRD.md](PRD.md) | Medir backlog, desempenho de Pedidos e crescimento do histórico para definir retenção/capacidade. |
| PRD-CA-005-SIGILO | [PRD.md](PRD.md) | Consultas e logs omitem segredos. |
| PRD-REG-02 | [PRD.md](PRD.md) | Edição para HTTP é rejeitada e preserva configuração anterior. |

### RFC

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| RFC-DEC-04 | [RFC.md](RFC.md) | Evento imutável e entregas independentes por endpoint na mesma transação. |
| RFC-DEC-09 | [RFC.md](RFC.md) | Bloquear sucessores por pedido/destino durante retry, com custo de latência. |
| RFC-DEC-10 | [RFC.md](RFC.md) | Interrupção entre HTTP e confirmação pode duplicar entrega; replay pode chegar fora de ordem. |
| RFC-DEC-14 | [RFC.md](RFC.md) | Preservar identidade e snapshot em retry e replay. |
| RFC-Q-05 | [RFC.md](RFC.md) | Validar a interação de retry com a ordem por pedido. |
| RFC-Q-06 | [RFC.md](RFC.md) | Definir isolamento por cliente antes de exposição. |
| RFC-Q-08 | [RFC.md](RFC.md) | Revisar segredos/deduplicação e definir política contra destinos internos. |
| RFC-Q-09 | [RFC.md](RFC.md) | Definir carga, medição de atraso, falhas e DLQ. |
| RFC-Q-10 | [RFC.md](RFC.md) | Definir retenção de outbox, DLQ e histórico. |
| RFC-Q-11 | [RFC.md](RFC.md) | Definir impacto de mudanças do cadastro sobre pendências. |
| RFC-Q-12 | [RFC.md](RFC.md) | Definir efeito de payload excessivo sobre status. |
| RFC-RISCO-01 | [RFC.md](RFC.md) | Banco acumula escritas e polling; operação do processo e backlog afetam capacidade e latência. |
| RFC-RISCO-02 | [RFC.md](RFC.md) | Replay após expiração de deduplicação pode reaplicar efeitos. |
| RFC-RISCO-03 | [RFC.md](RFC.md) | Snapshot, histórico e DLQ exigem retenção e proteção próprias. |
| RFC-PLAN-05 | [RFC.md](RFC.md) | Deliberar formalmente sobre RFC e ADRs. |
| RFC-SEG-05 | [RFC.md](RFC.md) | Definir formato da assinatura e gestão de segredos com revisão de Segurança. |
| RFC-TRD-04 | [RFC.md](RFC.md) | Concentrar disponibilidade no MySQL e gerir armazenamento adicional de snapshots e DLQ. |

### FDD

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| FDD-OBJ-02 | [FDD.md](FDD.md) | Isolar medição de retry/replay/bloqueios e demonstrar primeira entrega sob carga saudável acordada. |
| FDD-ESC-12 | [FDD.md](FDD.md) | Sem teste de destino ou listagem global de DLQ; consulta operacional pelo banco. |
| FDD-MOD-06 | [FDD.md](FDD.md) | Assinatura estendida do publisher usa sequence/context e chama após atualizar histórico e versão. |
| FDD-MOD-07 | [FDD.md](FDD.md) | Separar processor, componente de segurança, erros locais e transporte/relógio injetáveis. |
| FDD-DATA-03 | [FDD.md](FDD.md) | Definir FK RESTRICT, hash/limite da URL, unicidade por cliente e remoção lógica do cadastro. |
| FDD-DATA-04 | [FDD.md](FDD.md) | Usar JSON de status e filtrar em memória no tx sem índice JSON. |
| FDD-DATA-06 | [FDD.md](FDD.md) | Armazenar snapshot em LONGTEXT e manter IDs copiados sem FK destrutiva. |
| FDD-DATA-07 | [FDD.md](FDD.md) | Separar evento de domínio e entregas, com unicidade evento/endpoint. |
| FDD-DATA-08 | [FDD.md](FDD.md) | Modelar lease, estado, ciclo e agendamento na Outbox. |
| FDD-DATA-09 | [FDD.md](FDD.md) | Modelar tentativas duráveis com estado, token, versão de segredo, resultado e chave única de tentativa. |
| FDD-DATA-11 | [FDD.md](FDD.md) | Restringir a uma DLQ por ciclo, copiar autor sem FK e manter metadados de replay. |
| FDD-DATA-12 | [FDD.md](FDD.md) | Incrementar Order.statusVersion para sequência monotônica mesmo sem inscritos. |
| FDD-DATA-14 | [FDD.md](FDD.md) | Medir bytes UTF-8 e interpretar 64 KB como 65.536 bytes. |
| FDD-DATA-16 | [FDD.md](FDD.md) | Definir índices compostos de agenda/lease/endpoint/tentativas/DLQ e validar plano de join. |
| FDD-DATA-17 | [FDD.md](FDD.md) | Normalizar URL preservando caixa de path e query; hash para unicidade e proteção de colisão. |
| FDD-DATA-18 | [FDD.md](FDD.md) | Reservar URL de cadastro removido até política de retenção. |
| FDD-TX-01 | [FDD.md](FDD.md) | Lock parametrizado e releitura do pedido para serializar transições concorrentes. |
| FDD-TX-02 | [FDD.md](FDD.md) | Sem repetição automática do PATCH de Pedidos após deadlock/commit incerto. |
| FDD-TX-04 | [FDD.md](FDD.md) | Claim revê estado, data, orçamento, predecessor e endpoint; trava outbox e grava STARTED com lease UUID de 60 s. |
| FDD-TX-05 | [FDD.md](FDD.md) | Selecionar devidos/leases expiradas por createdAt e ID e tratar predecessor somente em ciclo automático original. |
| FDD-TX-06 | [FDD.md](FDD.md) | Claim por entrega imediatamente antes do HTTP; um envio por vez e lote sugerido de 20 candidatos. |
| FDD-TX-07 | [FDD.md](FDD.md) | Finalizar com token válido, estado PROCESSING e relógio do banco, gravando tentativa e resultado em conjunto. |
| FDD-TX-08 | [FDD.md](FDD.md) | Estado UNKNOWN de lease abandonada consome orçamento e agenda recuperação a partir do instante no banco. |
| FDD-FLUXO-06 | [FDD.md](FDD.md) | Acima do limite, erro 422 WEBHOOK_PAYLOAD_TOO_LARGE e rollback integral do pedido. |
| FDD-FLUXO-07 | [FDD.md](FDD.md) | Restringir emissão ao serviço, sem publicar por seed ou pela escrita direta Prisma. |
| FDD-WORK-06 | [FDD.md](FDD.md) | Comandos tsx/node, bootstrap sem listen e conexão única, com scripts de dev e produção. |
| FDD-WORK-07 | [FDD.md](FDD.md) | Polling sem sobreposição e nova leitura imediata após trabalho. |
| FDD-WORK-08 | [FDD.md](FDD.md) | POST HTTPS sem redirect, com validação pré-envio e timeout que abrange DNS/conexão/resposta/leitura. |
| FDD-WORK-10 | [FDD.md](FDD.md) | 2xx inclusive 204 confirma sucesso sem exigir JSON. |
| FDD-WORK-11 | [FDD.md](FDD.md) | Shutdown por sinais aguarda finalização, desconecta Prisma e aborta HTTP após prazo sugerido de 15 s. |
| FDD-RETRY-04 | [FDD.md](FDD.md) | Rede/DNS transitório, 408, 429 e 5xx são elegíveis a retry. |
| FDD-RETRY-05 | [FDD.md](FDD.md) | Outros HTTP não 2xx, TLS inválido, URL bloqueada e snapshot inválido são permanentes. |
| FDD-RETRY-06 | [FDD.md](FDD.md) | Persistir próxima data pela conclusão no banco; sem jitter, Retry-After efetivo ou retry oculto. |
| FDD-RETRY-07 | [FDD.md](FDD.md) | Uma quinta tentativa UNKNOWN esgota o orçamento e leva à DLQ. |
| FDD-RETRY-08 | [FDD.md](FDD.md) | Bloquear só sucessores do mesmo pedido/destino durante backoff até resolução; replay não restaura ordem. |
| FDD-DLQ-01 | [FDD.md](FDD.md) | Gravar tentativa, DLQ e Outbox terminal atomicamente; preservar outbox fora do polling. |
| FDD-DLQ-04 | [FDD.md](FDD.md) | Replay trata apenas DLQ não usada e destino ativo em ciclo terminal, criando novo ciclo com contador reiniciado e sem apagar histórico. |
| FDD-DLQ-05 | [FDD.md](FDD.md) | Replay preserva bytes e ID, ignora filtro atual, registra correlação e audita atomicamente. |
| FDD-DLQ-06 | [FDD.md](FDD.md) | Replays concorrentes resultam em 202/409; nova falha cria nova DLQ por ciclo. |
| FDD-API-05 | [FDD.md](FDD.md) | Corrigir atribuição de ADMIN e definir autorização por cliente antes de exposição. |
| FDD-API-09 | [FDD.md](FDD.md) | Exigir mídia JSON quando houver body, campos estritos, Accept opcional e regras de ausência de body/query. |
| FDD-API-12 | [FDD.md](FDD.md) | POST /api/v1/webhooks retorna 201, versiona segredo, exige cliente existente e 1 a 6 status únicos, ativo por padrão e sem challenge. |
| FDD-API-14 | [FDD.md](FDD.md) | GET /api/v1/webhooks usa customerId na query, paginação, filtro active e ordenação; exclui removidos. |
| FDD-API-16 | [FDD.md](FDD.md) | PATCH /api/v1/webhooks/:id usa DTO parcial não vazio, troca lista e proíbe editar cliente ou segredo. |
| FDD-API-17 | [FDD.md](FDD.md) | URL nova vale para novas tentativas; filtro só para futuras emissões; desativado vai à DLQ e reativação não aciona replay. |
| FDD-API-19 | [FDD.md](FDD.md) | DELETE /api/v1/webhooks/:id responde 204 idempotente, elimina segredos mas preserva histórico, e pendências vão à DLQ. |
| FDD-API-21 | [FDD.md](FDD.md) | POST rotate-secret exige confirmação JSON, retorna versão/expiração com no-store, conflita durante grace e aceita endpoint inativo. |
| FDD-API-22 | [FDD.md](FDD.md) | Procedimento para recuperar resposta perdida da rotação. |
| FDD-API-24 | [FDD.md](FDD.md) | Histórico pagina tentativas por startedAt/id, inclui STARTED/UNKNOWN e permite consultar removidos. |
| FDD-API-25 | [FDD.md](FDD.md) | Histórico expõe deadLetterId e resposta sanitizada com limite sugerido de 4096 bytes e indicador de corte. |
| FDD-API-27 | [FDD.md](FDD.md) | Replay usa ID da DLQ, exige confirmação JSON e responde 202 com metadados de agendamento. |
| FDD-OUT-06 | [FDD.md](FDD.md) | Enviar bytes UTF-8 originais sem reserializar, transformar ou comprimir. |
| FDD-OUT-07 | [FDD.md](FDD.md) | Formato sha256= seguido de 64 hex minúsculos e X-Signature-Previous durante coexistência. |
| FDD-OUT-08 | [FDD.md](FDD.md) | Consumidor compara digests em tempo constante e verifica correspondência ID corpo/header. |
| FDD-OUT-09 | [FDD.md](FDD.md) | Deduplicar por par webhook/evento, gravar efeitos atomicamente e renovar retenção de 7 dias após duplicata. |
| FDD-OUT-10 | [FDD.md](FDD.md) | Retornar 2xx após aceitação durável ou duplicata e versionar futuras mudanças incompatíveis. |
| FDD-OUT-11 | [FDD.md](FDD.md) | Não encaminhar JWT/cookies nem acrescentar query; excluir outros campos sensíveis do payload. |
| FDD-ERR-LOCAL | [FDD.md](FDD.md) | Adaptar apenas erros conhecidos por wrappers locais e preservar erros de Pedidos. |
| FDD-ERR-PARSER-NOVO | [FDD.md](FDD.md) | Handler restrito adapta 400/413 do parser e assegura correlação. |
| FDD-RES-01 | [FDD.md](FDD.md) | Sem claim persistido, não enviar; reconectar ao banco com espera 2/4/8/16/30s. |
| FDD-RES-02 | [FDD.md](FDD.md) | Crash após HTTP e antes de confirmar pode repetir entrega. |
| FDD-RES-03 | [FDD.md](FDD.md) | Chave indisponível suspende operação sem DLQ em massa. |
| FDD-RES-04 | [FDD.md](FDD.md) | Backlog pede alerta e monitoramento sem descartar pendências ou escalar automaticamente. |
| FDD-SEG-02 | [FDD.md](FDD.md) | Segredo whsec_ com 32 bytes aleatórios em base64url. |
| FDD-SEG-03 | [FDD.md](FDD.md) | Cifrar em AES-256-GCM com chave de 32 bytes, IV 12 bytes, tag 16 bytes, AAD e envelope versionado. |
| FDD-SEG-04 | [FDD.md](FDD.md) | Definir recuperação de chave e recifragem operacional. |
| FDD-SEG-05 | [FDD.md](FDD.md) | Credencial só em criação/rotação, no-store, DTO sem segredo antigo e redação com allowlist. |
| FDD-SEG-06 | [FDD.md](FDD.md) | Bloquear userinfo/fragmento, portas e resoluções impróprias no cadastro e em cada tentativa. |
| FDD-SEG-07 | [FDD.md](FDD.md) | Fixar destino validado na conexão, preservar TLS e aplicar egress restrito. |
| FDD-SEG-08 | [FDD.md](FDD.md) | Sanitizar respostas de diagnóstico e omitir URL completa e credenciais de logs. |
| FDD-OBS-04 | [FDD.md](FDD.md) | Exportar métricas por logs Pino kind=metric e consultar gauges a cada 30 s; pipeline pendente. |
| FDD-OBS-05 | [FDD.md](FDD.md) | Evitar labels de alta cardinalidade, emitir counters pós-commit e reconciliar pelos dados duráveis. |
| FDD-OBS-06 | [FDD.md](FDD.md) | Definir buckets, carga, thresholds, responsáveis e canal dos alertas. |
| FDD-OBS-07 | [FDD.md](FDD.md) | Logger filho identifica worker; mapear níveis/campos e excluir payloads, URLs e credenciais. |
| FDD-OBS-09 | [FDD.md](FDD.md) | Trilha lógica em logs com traceId/spanId/parentSpanId, origem durável e spans encerrados no commit. |
| FDD-OBS-10 | [FDD.md](FDD.md) | Retry conserva origem; replay vincula nova requisição ao evento; sem traceparent não conforme. |
| FDD-OBS-11 | [FDD.md](FDD.md) | Aprovar SDK/backend de tracing distribuído. |
| FDD-DEP-02 | [FDD.md](FDD.md) | Usar bibliotecas nativas de crypto/HTTPS/DNS em vez de dependência HTTP nova. |
| FDD-DEP-05 | [FDD.md](FDD.md) | Acrescentar configurações de chave e feature, validar lease/timeout e falhar cedo. |
| FDD-DEP-06 | [FDD.md](FDD.md) | Parametrizar polling, timeout, agenda, lease, batch, tamanho, resposta e shutdown. |
| FDD-DEP-08 | [FDD.md](FDD.md) | Migração aditiva com novos modelos, FKs/índices e statusVersion; gerar cliente sem editar migração inicial. |
| FDD-DEP-09 | [FDD.md](FDD.md) | Rollout migra schema, atualiza todos produtores, ativa worker único e depois cadastros/piloto. |
| FDD-DEP-10 | [FDD.md](FDD.md) | Instâncias antigas podem mudar status sem registrar evento durante rollout. |
| FDD-DEP-11 | [FDD.md](FDD.md) | Rollback para worker, preserva dados/migração, e retorno de API antiga interrompe emissão. |
| FDD-DEP-12 | [FDD.md](FDD.md) | Prover egress HTTPS/DNS, relógio sincronizado, gestão de chaves e supervisão. |
| FDD-DEP-14 | [FDD.md](FDD.md) | Avaliar soma dos pools, capacidade de escrita, locks, backlog e retenção. |
| FDD-DEP-18 | [FDD.md](FDD.md) | Incluir novas tabelas na limpeza e criar três arquivos de teste de webhooks. |
| FDD-TESTE-03 | [FDD.md](FDD.md) | Unitários cobrem fronteira UTF-8, vetor HMAC, rotação, backoff, classes HTTP, destinos, sanitização e DTO. |
| FDD-TESTE-04 | [FDD.md](FDD.md) | Integração MySQL exercita fan-out, rollback injetado, ordem, claims concorrentes e atomicidade da DLQ/replay. |
| FDD-TESTE-05 | [FDD.md](FDD.md) | Validar sete contratos com status, DTOs, paginação, parser e exposição controlada de segredo. |
| FDD-TESTE-06 | [FDD.md](FDD.md) | Processor com destino controlado valida classificação HTTP, timeout, limite de resposta e bytes/assinaturas. |
| FDD-TESTE-07 | [FDD.md](FDD.md) | Testar processos separados e recuperação de crash/lease antes e depois de HTTP. |
| FDD-TESTE-08 | [FDD.md](FDD.md) | Exercitar ordem, replay tardio, ciclo de vida de destinos e rotação concorrente. |
| FDD-TESTE-09 | [FDD.md](FDD.md) | Regressão de Orders/Auth, reconstrução de trilha, falhas de banco, shutdown e plano SQL. |
| FDD-TESTE-10 | [FDD.md](FDD.md) | Separar configuração dos unitários, injetar relógio/transporte e usar conexões independentes para concorrência. |
| FDD-AC01 | [FDD.md](FDD.md) | Dois inscritos geram um evento e duas entregas com IDs próprios no mesmo commit. |
| FDD-AC04 | [FDD.md](FDD.md) | Transição inválida ou repetida conserva erro e não gera evento. |
| FDD-AC06 | [FDD.md](FDD.md) | Bytes, ID e timestamp de origem idênticos durante retry, replay e nova mudança. |
| FDD-AC08 | [FDD.md](FDD.md) | Duas assinaturas na janela, expurgo após 24 h e ausência de segredo em consulta/log. |
| FDD-AC10 | [FDD.md](FDD.md) | Falha permanente/redirect causa DLQ atômica sem seguir Location. |
| FDD-AC11 | [FDD.md](FDD.md) | Recuperar lease, rejeitar token antigo e deduplicar efeito após crash. |
| FDD-AC12 | [FDD.md](FDD.md) | Somente um token vigente vence claims concorrentes. |
| FDD-AC13 | [FDD.md](FDD.md) | Predecessor em backoff bloqueia só mesmo pedido/destino; replay não restaura ordem. |
| FDD-AC14 | [FDD.md](FDD.md) | Concorrência no replay resulta em um ciclo e respostas 202/409. |
| FDD-AC15 | [FDD.md](FDD.md) | Sete contratos atendem DTOs, status, paginação e política de devolução de segredo. |
| FDD-AC16 | [FDD.md](FDD.md) | Bloquear destinos proibidos, DNS rebinding e TLS inválido sem expor credenciais. |
| FDD-AC17 | [FDD.md](FDD.md) | 65.537 bytes produzem 422 e rollback sem truncamento. |
| FDD-AC18 | [FDD.md](FDD.md) | Correlacionar origem/tx/evento/ciclo/tentativa/HTTP e medir espera, duração e DLQ sem segredos. |
| FDD-AC19 | [FDD.md](FDD.md) | Demonstrar <10 s em carga acordada e publicar a distribuição após rollout completo. |
| FDD-AC20 | [FDD.md](FDD.md) | Suíte atual continua válida e isolamento é revisado antes da liberação B2B. |
| FDD-RISCO-02 | [FDD.md](FDD.md) | Cascata destrutiva, concorrência de status/estoque/exclusão e replay fora de ordem exigem tratamento separado. |
| FDD-RISCO-04 | [FDD.md](FDD.md) | Perda de chaves ou da resposta de rotação exige recuperação segura. |
| FDD-RISCO-05 | [FDD.md](FDD.md) | Sem coletor e alertas a operação precisa definir pipeline e responsável. |
| FDD-RISCO-06 | [FDD.md](FDD.md) | Probabilidades e impactos qualitativos atribuídos aos riscos de desenho. |
| FDD-PLAN-03 | [FDD.md](FDD.md) | Aprovar RFC/ADRs/FDD e confirmar responsáveis propostos. |
| FDD-ERR-01 | [FDD.md](FDD.md) | `WEBHOOK_VALIDATION_ERROR` — Params/query/body ou JSON inválido; 400 API; Rejeitar sem mutação; path/message.. |
| FDD-ERR-02 | [FDD.md](FDD.md) | `WEBHOOK_INVALID_URL` — URL inválida/não HTTPS/userinfo/fragmento; 400 API; Não cadastrar/editar.. |
| FDD-ERR-03 | [FDD.md](FDD.md) | `WEBHOOK_UNSUPPORTED_MEDIA_TYPE` — JSON com mídia incompatível; 415 API; Rejeitar entrada.. |
| FDD-ERR-04 | [FDD.md](FDD.md) | `WEBHOOK_REQUEST_TOO_LARGE` — Entrada administrativa acima de 1 MB; 413 API; Rejeitar parser; distinto do snapshot.. |
| FDD-ERR-05 | [FDD.md](FDD.md) | `WEBHOOK_NOT_FOUND` — Cadastro inexistente/removido na operação; 404 API; Não alterar dados.. |
| FDD-ERR-06 | [FDD.md](FDD.md) | `WEBHOOK_CUSTOMER_NOT_FOUND` — Cliente inexistente; 404 API; Não cadastrar/listar referência inválida.. |
| FDD-ERR-07 | [FDD.md](FDD.md) | `WEBHOOK_DUPLICATE_ENDPOINT` — Cliente + URL normalizada repetidos; 409 API; Constraint protege corrida.. |
| FDD-ERR-08 | [FDD.md](FDD.md) | `WEBHOOK_UNAUTHORIZED` — JWT ausente/inválido/expirado; 401 API; Adaptar erro compartilhado; sem mutação.. |
| FDD-ERR-09 | [FDD.md](FDD.md) | `WEBHOOK_FORBIDDEN` — Role/acesso negados; 403 API; Impedir ação; vínculo de cliente ainda pendente.. |
| FDD-ERR-10 | [FDD.md](FDD.md) | `WEBHOOK_DESTINATION_BLOCKED` — SSRF/IP/faixa/porta proibidos; 422 API; N/A worker; Rejeitar configuração/replay; worker DLQ.. |
| FDD-ERR-11 | [FDD.md](FDD.md) | `WEBHOOK_ROTATION_IN_PROGRESS` — Rotação durante grace vigente; 409 API; Preservar segredo anterior.. |
| FDD-ERR-12 | [FDD.md](FDD.md) | `WEBHOOK_SECRET_UNAVAILABLE` — Geração/cifragem/decifragem falha; 500 API; N/A worker; Rollback; suspender worker/alertar, sem DLQ em massa por chave ausente.. |
| FDD-ERR-13 | [FDD.md](FDD.md) | `WEBHOOK_PAYLOAD_TOO_LARGE` — Snapshot acima de 65536 bytes; 422 status; N/A worker; Rollback da emissão; legado inválido no worker DLQ.. |
| FDD-ERR-14 | [FDD.md](FDD.md) | `WEBHOOK_PERSISTENCE_FAILED` — Falha conhecida de armazenamento; 500 API; N/A worker; Rollback; recuperar lease. Sem retry cego de mutação.. |
| FDD-ERR-15 | [FDD.md](FDD.md) | `WEBHOOK_DELIVERY_TIMEOUT` — Chamada excede 10s; N/A; Retry ou DLQ ao esgotar.. |
| FDD-ERR-16 | [FDD.md](FDD.md) | `WEBHOOK_COMMUNICATION_FAILED` — Rede/DNS transitório; N/A; Retry limitado com identidade estável.. |
| FDD-ERR-17 | [FDD.md](FDD.md) | `WEBHOOK_REMOTE_RETRYABLE` — Remoto 408/429/5xx; N/A; guardar remoto; Backoff persistido.. |
| FDD-ERR-18 | [FDD.md](FDD.md) | `WEBHOOK_REMOTE_REJECTED` — Outro HTTP não 2xx/TLS inválido; N/A; guardar remoto se houver; DLQ sanitizada.. |
| FDD-ERR-19 | [FDD.md](FDD.md) | `WEBHOOK_SIGNATURE_INVALID` — Consumidor rejeita HMAC (ex.: 401); N/A; possível 401 remoto; Diagnóstico local/DLQ. Consumidor possui envelope próprio; sem rota inbound.. |
| FDD-ERR-20 | [FDD.md](FDD.md) | `WEBHOOK_RETRY_EXHAUSTED` — Orçamento do ciclo consumido; N/A; DLQ, preservando causa original.. |
| FDD-ERR-21 | [FDD.md](FDD.md) | `WEBHOOK_PROCESSING_FAILED` — Snapshot inválido/erro local; N/A; Dado inválido conhecido DLQ; bug inesperado suspende loop, loga e recupera lease após correção.. |
| FDD-ERR-22 | [FDD.md](FDD.md) | `WEBHOOK_LEASE_LOST` — Update com token vencido/obsoleto; N/A; Não sobrescrever novo dono.. |
| FDD-ERR-23 | [FDD.md](FDD.md) | `WEBHOOK_ENDPOINT_DISABLED` — Cadastro desativado; 409 replay; N/A worker; Não enviar; pendência DLQ.. |
| FDD-ERR-24 | [FDD.md](FDD.md) | `WEBHOOK_ENDPOINT_DELETED` — Cadastro removido; 409 replay; N/A worker; Não enviar; manter evidência.. |
| FDD-ERR-25 | [FDD.md](FDD.md) | `WEBHOOK_DEAD_LETTER_NOT_FOUND` — DLQ inexistente; 404 API; Não enfileirar.. |
| FDD-ERR-26 | [FDD.md](FDD.md) | `WEBHOOK_REPLAY_CONFLICT` — DLQ usada/ciclo não terminal; 409 API; Não duplicar ciclo.. |
| FDD-MET-01 | [FDD.md](FDD.md) | `webhook_events_created_total` (Counter) — Eventos confirmados; dimensões: eventType. |
| FDD-MET-02 | [FDD.md](FDD.md) | `webhook_deliveries_created_total` (Counter) — Fan-out/ciclos confirmados; dimensões: eventType, origin. |
| FDD-MET-03 | [FDD.md](FDD.md) | `webhook_pending` (Gauge) — Pendentes/em processamento; dimensões: state. |
| FDD-MET-04 | [FDD.md](FDD.md) | `webhook_processed_total` (Counter) — Ciclos concluídos; dimensões: outcome. |
| FDD-MET-05 | [FDD.md](FDD.md) | `webhook_delivery_attempts_total` (Counter) — Volume do worker; dimensões: outcome, statusClass, cycleType. |
| FDD-MET-06 | [FDD.md](FDD.md) | `webhook_delivery_success_total` (Counter) — Respostas 2xx; dimensões: statusClass. |
| FDD-MET-07 | [FDD.md](FDD.md) | `webhook_delivery_failure_total` (Counter) — Falhas; dimensões: errorCode, statusClass. |
| FDD-MET-08 | [FDD.md](FDD.md) | `webhook_retries_scheduled_total` (Counter) — Retries persistidos; dimensões: retryNumber, errorCode. |
| FDD-MET-09 | [FDD.md](FDD.md) | `webhook_dead_letter_total` (Counter) — Entradas DLQ; dimensões: failureCode. |
| FDD-MET-10 | [FDD.md](FDD.md) | `webhook_http_duration_seconds` (Histogram) — Duração HTTP; dimensões: outcome, statusClass. |
| FDD-MET-11 | [FDD.md](FDD.md) | `webhook_outbox_wait_seconds` (Histogram) — Emissão até primeiro claim; dimensões: eventType. |
| FDD-MET-12 | [FDD.md](FDD.md) | `webhook_delivery_latency_seconds` (Histogram) — Emissão até confirmação; dimensões: outcome, cycleType. |
| FDD-MET-13 | [FDD.md](FDD.md) | `webhook_oldest_pending_age_seconds` (Gauge) — Atraso/backlog; dimensões: state. |
| FDD-MET-14 | [FDD.md](FDD.md) | `webhook_worker_batches_total` (Counter) — Atividade/polling; dimensões: result=empty/work/error. |
| FDD-MET-15 | [FDD.md](FDD.md) | `webhook_lease_recovered_total` (Counter) — Interrupções recuperadas; dimensões: exhausted. |
| FDD-TX-10 | [FDD.md](FDD.md) | Máquina da Outbox distingue PENDING, PROCESSING, RETRY_WAIT, DELIVERED e DEAD_LETTER; replay reinicia ciclo. |
| FDD-API-28 | [FDD.md](FDD.md) | POST repetido não sobrescreve segredo; recuperar criação perdida via listagem e rotação. |
| FDD-API-29 | [FDD.md](FDD.md) | Listagem/histórico vazios devolvem 200 com coleção vazia. |
| FDD-API-30 | [FDD.md](FDD.md) | Histórico devolve status/resposta nulos quando ausentes; entrega lê string original e consulta parseia JSON. |
| FDD-API-31 | [FDD.md](FDD.md) | Validar autor do replay por usuário autenticado e correlação da requisição atual. |
| FDD-OBS-12 | [FDD.md](FDD.md) | Validar alertas para inatividade, backlog, DLQ, banco e chave durante piloto. |
| FDD-DEP-19 | [FDD.md](FDD.md) | Worker importa apenas singleton compartilhado para evitar dois clientes no mesmo processo. |
| FDD-AC02-FANOUT | [FDD.md](FDD.md) | Falha na última entrega elegível reverte todo o fan-out e o estado de Pedidos. |
| FDD-RISC-08 | [FDD.md](FDD.md) | Nova configuração/limpeza de testes deve preservar ordem referencial. |
| FDD-RISC-09 | [FDD.md](FDD.md) | Acúmulo das tabelas de entrega requer medir espaço e decidir retenção. |
| FDD-RISC-10 | [FDD.md](FDD.md) | Concorrência de replay/claim requer constraints e locks curtos. |
| FDD-RISC-11 | [FDD.md](FDD.md) | Falha HTTP ou rejeição pode atrasar sucessores enquanto houver backoff. |
| FDD-Q-01 | [FDD.md](FDD.md) | Validar valores sugeridos de lote, resposta, backoff de banco, lease e shutdown. |
| FDD-Q-02 | [FDD.md](FDD.md) | Validar política de destino, revisão de assinatura, recuperação de chave e rotação perdida. |
| FDD-Q-04 | [FDD.md](FDD.md) | Validar retenção para snapshots, histórico, DLQ e auditoria. |
| FDD-API-34 | [FDD.md](FDD.md) | Definir papéis específicos para histórico e rotação; o FDD propõe ADMIN/OPERATOR. |
| FDD-SEG-11 | [FDD.md](FDD.md) | Não usar hash unidirecional como única representação do segredo, pois o worker precisa assinar. |

### ADR-001

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-001-HIST-01 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Data original da decisão, equipe responsável, orçamento e conhecimento prévio de MySQL na empresa. |
| ADR-001-HIST-02 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Adoção original guiada por ACID, integridade referencial, Prisma e operação gerenciada. |
| ADR-001-MET-01 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Disponibilidade mensal de 99,9%. |
| ADR-001-MET-02 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Perda máxima de dados de 15 minutos. |
| ADR-001-MET-03 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Recuperação em até quatro horas. |
| ADR-001-ALT-02 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Avaliação histórica considerou PostgreSQL e alegou necessidade de capacitação adicional. |
| ADR-001-ALT-03 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Avaliação histórica descartou SQLite por cenário gerenciado e multiprocesso. |
| ADR-001-TRD-02 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | **Afirmação histórica sem comprovação.** Equipe teria aceitado contenção entre cargas em troca de aprendizado e reuso de rotinas operacionais. |
| ADR-001-Q-01 | [adrs/ADR-001-mysql-como-banco-relacional.md](adrs/ADR-001-mysql-como-banco-relacional.md) | Dimensionar carga combinada e revisar capacidade antes de separar infraestrutura. |

### ADR-002

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-002-HIST-01 | [adrs/ADR-002-prisma-para-acesso-a-dados.md](adrs/ADR-002-prisma-para-acesso-a-dados.md) | **Afirmação histórica sem comprovação.** Data e motivação histórica de adoção: reduzir CRUD repetitivo e manter contratos TypeScript para equipe pequena. |
| ADR-002-HIST-02 | [adrs/ADR-002-prisma-para-acesso-a-dados.md](adrs/ADR-002-prisma-para-acesso-a-dados.md) | **Afirmação histórica sem comprovação.** Seleção original priorizou tipagem, migrações versionadas, MySQL e transações explícitas. |
| ADR-002-ALT-02 | [adrs/ADR-002-prisma-para-acesso-a-dados.md](adrs/ADR-002-prisma-para-acesso-a-dados.md) | **Afirmação histórica sem comprovação.** TypeORM teria sido descartado por convenções adicionais. |
| ADR-002-ALT-03 | [adrs/ADR-002-prisma-para-acesso-a-dados.md](adrs/ADR-002-prisma-para-acesso-a-dados.md) | **Afirmação histórica sem comprovação.** SQL direto teria sido descartado pela repetição do CRUD e mapeamento manual. |
| ADR-002-TRD-02 | [adrs/ADR-002-prisma-para-acesso-a-dados.md](adrs/ADR-002-prisma-para-acesso-a-dados.md) | **Afirmação histórica sem comprovação.** Equipe teria aceitado o acoplamento como troca por menos código e definido deliberadamente a exceção transacional do service. |
| ADR-002-Q-01 | [adrs/ADR-002-prisma-para-acesso-a-dados.md](adrs/ADR-002-prisma-para-acesso-a-dados.md) | Dimensionar total de conexões com instâncias por processo. |

### ADR-003

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-003-DEC-03 | [adrs/ADR-003-outbox-mysql-na-transacao-de-status.md](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) | Separar evento imutável com ID compartilhado e entregas independentes registradas no mesmo tx. |
| ADR-003-DEC-04 | [adrs/ADR-003-outbox-mysql-na-transacao-de-status.md](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) | Impor unicidade por evento/endpoint. |
| ADR-003-Q-01 | [adrs/ADR-003-outbox-mysql-na-transacao-de-status.md](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) | Operar recuperação, retenção e isolamento de estoque sob concorrência. |
| ADR-003-HIST-01 | [adrs/ADR-003-outbox-mysql-na-transacao-de-status.md](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) | **Afirmação histórica sem comprovação.** Data civil 18/06/2026 atribuída à decisão. |

### ADR-004

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-004-DEC-02 | [adrs/ADR-004-entrega-http-at-least-once-com-event-id.md](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) | ID do evento compartilhado entre endpoints e distinto de tentativas e ciclos. |
| ADR-004-DEC-03 | [adrs/ADR-004-entrega-http-at-least-once-com-event-id.md](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) | Retries e replay mantêm identificador e snapshot original. |
| ADR-004-CON-02 | [adrs/ADR-004-entrega-http-at-least-once-com-event-id.md](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) | Consumidor guarda par webhook/evento por pelo menos sete dias após última recepção. |
| ADR-004-RISCO-01 | [adrs/ADR-004-entrega-http-at-least-once-com-event-id.md](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) | Replay após vencer deduplicação pode reaplicar efeitos e requer avaliação administrativa. |
| ADR-004-DEC-04 | [adrs/ADR-004-entrega-http-at-least-once-com-event-id.md](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) | Sucessor do mesmo pedido/endpoint espera até sucesso/DLQ; replay não restaura ordem histórica. |
| ADR-004-HIST-01 | [adrs/ADR-004-entrega-http-at-least-once-com-event-id.md](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) | **Afirmação histórica sem comprovação.** Data civil 18/06/2026 atribuída à decisão. |

### ADR-005

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-005-CON-02 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | Serializar JSON UTF-8 uma vez e assinar bytes exatos com hex minúsculo e prefixo sha256=. |
| ADR-005-CON-03 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | Emitir X-Signature-Previous durante a janela e eliminar o material anterior após 24 h. |
| ADR-005-SEG-02 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | Gerar segredo com 32 bytes aleatórios e apresentar só em criação/rotação. |
| ADR-005-SEG-03 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | Cifrar em repouso com AES-256-GCM e chave de serviço via ambiente. |
| ADR-005-SEG-04 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | Omitir assinatura e segredo de logs e consultas. |
| ADR-005-SEG-05 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | Consumidor compara assinatura em tempo constante e deduplica por par de IDs. |
| ADR-005-HIST-01 | [adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) | **Afirmação histórica sem comprovação.** Data civil 18/06/2026 atribuída à decisão. |

### ADR-006

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-006-TRD-02 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | Backlog e duração do envio elevam a latência total e exigem aferição. |
| ADR-006-DEC-03 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | Atribuir sequência monotônica por pedido na emissão. |
| ADR-006-DEC-04 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | Bloquear sucessores por pedido/destino durante backoff; replay pode chegar tarde. |
| ADR-006-DEC-05 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | Claim atômico com token exclusivo e concessão de 60 segundos. |
| ADR-006-DEC-06 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | Lease expirada torna entrega recuperável e rejeita finalização antiga. |
| ADR-006-RISCO-01 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | Crash entre HTTP e confirmação pode repetir entrega com mesmo ID. |
| ADR-006-HIST-01 | [adrs/ADR-006-worker-separado-com-polling.md](adrs/ADR-006-worker-separado-com-polling.md) | **Afirmação histórica sem comprovação.** Data civil 18/06/2026 atribuída à decisão. |

### ADR-007

| ID | Documento | Item sem evidência primária suficiente |
|---|---|---|
| ADR-007-DEC-05 | [adrs/ADR-007-retry-com-backoff-e-dlq.md](adrs/ADR-007-retry-com-backoff-e-dlq.md) | Classificar rede/408/429/5xx como recuperáveis, 2xx como sucesso e demais como terminais sem redirects. |
| ADR-007-DEC-06 | [adrs/ADR-007-retry-com-backoff-e-dlq.md](adrs/ADR-007-retry-com-backoff-e-dlq.md) | Replay cria ciclo, reinicia contador, conserva ID/snapshot e registra origem. |
| ADR-007-DEC-07 | [adrs/ADR-007-retry-com-backoff-e-dlq.md](adrs/ADR-007-retry-com-backoff-e-dlq.md) | Permitir um único ciclo ativo por evento/endpoint em replay concorrente. |
| ADR-007-DEC-08 | [adrs/ADR-007-retry-com-backoff-e-dlq.md](adrs/ADR-007-retry-com-backoff-e-dlq.md) | Sucessores esperam durante retry até DLQ ou sucesso; replay tardio pode sair de ordem. |
| ADR-007-DEP-01 | [adrs/ADR-007-retry-com-backoff-e-dlq.md](adrs/ADR-007-retry-com-backoff-e-dlq.md) | Gerenciar retenção e proteção da DLQ. |
| ADR-007-HIST-01 | [adrs/ADR-007-retry-com-backoff-e-dlq.md](adrs/ADR-007-retry-com-backoff-e-dlq.md) | **Afirmação histórica sem comprovação.** Data civil 18/06/2026 atribuída à decisão. |

## Divergências e limites das evidências

- A reunião registra cinco esperas e «total 5 tentativas». O ADR-007 e o FDD seguem o total anunciado com quatro esperas. O intervalo de 12 h permanece em revisão.
- A reunião inicialmente sugeriu obter `customer_id` do JWT e corrigiu a origem para a requisição. O código atual identifica usuário e papel e não declara vínculo User–Customer. A inscrição pública pode atribuir ADMIN, o que compromete a proteção pretendida para o futuro replay.
- A discussão menciona `X-Timestamp` para detectar replay. Como a decisão de HMAC autentica o corpo, o cabeçalho de envio fica fora da assinatura. O timestamp sozinho não comprova frescor.
- O polling de 2 s determina a frequência de busca. A fala sobre latência de 2 s não comprova limite fim a fim nem o atendimento da meta de 10 s. A ordem sugerida na reunião para um worker não resolve a ordem durante retry ou replay.
- A reunião menciona débito de estoque ao descrever a transação. O código debita em `PENDING → PAID` e repõe estoque no cancelamento a partir de `PAID` ou `PROCESSING`.
- Os ADRs atribuem datas civis às decisões e descrevem motivações históricas para MySQL e Prisma. A transcrição e o código confirmam tecnologias em uso e seu reuso na feature. A adoção original, as comparações históricas e as metas de disponibilidade e recuperação do ADR-001 ficaram sem comprovação.
- A reunião propõe reutilizar o middleware de erros. O código confirma o envelope e os tratamentos genéricos. O FDD propõe adaptação para os códigos `WEBHOOK_` e tratamento do parser; os códigos e comportamentos específicos permanecem sujeitos a implementação.
- Os relatórios do repositório estão em `docs/reports/`. Foram usados para orientar a leitura do código.
