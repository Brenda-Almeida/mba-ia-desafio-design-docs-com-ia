# Tracker de rastreabilidade — Webhooks de mudança de status de pedidos

## Inventário e método

O inventário corresponde à versão revisada do PRD, da RFC, do FDD e dos sete ADRs. Os IDs aparecem nos próprios documentos. Repetições em exemplos, diagramas e referências de navegação contam uma vez por documento. O README de ADRs é um índice. As questões em aberto integram o denominador e ficam fora do numerador quando as fontes não sustentam a questão ou sua resposta.

O inventário inclui decisões verificadas, propostas necessárias ao padrão de entrega e questões em aberto. Os exemplos dos contratos administrativos ilustram as regras propostas no FDD e não geram linhas adicionais por amostra. As datas civis dos ADRs permanecem desconhecidas.

- PRD — 60 de 68 (88,24%), com 8 itens sem evidência.
- RFC — 59 de 64 itens rastreados (92,19%), com 5 questões sem evidência.
- FDD — 105 de 140 (75,00%), com 35 itens sem evidência.
- ADR-001 — 5 de 6 itens rastreados (83,33%), com 1 questão sem evidência.
- ADR-002 — 7 de 8 itens rastreados (87,50%), com 1 questão sem evidência.
- ADR-003 — 9 de 10 itens rastreados (90,00%), com 1 questão sem evidência.
- ADR-004 — 7 de 8 itens rastreados (87,50%), com 1 questão sem evidência.
- ADR-005 — 9 de 10 itens rastreados (90,00%), com 1 questão sem evidência.
- ADR-006 — 9 de 10 itens rastreados (90,00%), com 1 questão sem evidência.
- ADR-007 — 10 de 11 itens rastreados (90,91%), com 1 questão sem evidência.
- ADRs — 56 de 63 itens rastreados (88,89%), com 7 questões sem evidência.
- Total — 280 de 335 itens rastreados (83,58%), com 55 itens sem evidência.

## Verificação

- **PASSOU** · Tabela com seis colunas e IDs únicos · 280 linhas de evidência.
- **PASSOU** · Cobertura mínima de 80% · 280/335 (83,58%).
- **PASSOU** · Pelo menos 70% da transcrição · 211/280 (75,36%).
- **PASSOU** · Pelo menos cinco linhas de código · 69 linhas em 32 arquivos distintos.

**Veredito · APROVADO nos critérios de rastreabilidade.**

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
| PRD-RF-009 | docs/PRD.md | Requisito Funcional | Limitar cada ciclo de retry a cinco tentativas totais, conforme o total anunciado na reunião | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
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
| PRD-Q-01 | docs/PRD.md | Questão em Aberto | Conciliar o total anunciado de cinco tentativas com a lista verbal de esperas de 1 min, 5 min, 30 min, 2 h e 12 h | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| PRD-Q-04 | docs/PRD.md | Risco | Cadastro público aceita ADMIN, comprometendo a barreira de papel proposta para replay | CODIGO | src/modules/auth/auth.routes.ts; src/modules/auth/auth.schemas.ts; src/modules/auth/auth.controller.ts; src/modules/auth/auth.service.ts; src/modules/users/user.service.ts; src/modules/users/user.repository.ts |
| PRD-Q-05 | docs/PRD.md | Risco | X-Timestamp foi sugerido para detectar replay, mas fica fora do corpo assinado | TRANSCRICAO | [09:44] Diego; [09:22] Sofia |
| PRD-PLAN-01 | docs/PRD.md | Restrição | Estimativa de três sprints incluindo revisão | TRANSCRICAO | [09:47] Larissa |
| PRD-PLAN-02 | docs/PRD.md | Questão em Aberto | Atlas pediu fim de novembro; Marcos ficou de confirmar prazo | TRANSCRICAO | [09:45] Marcos; [09:47] Marcos |
| PRD-DEP-01 | docs/PRD.md | Dependência | Pedidos fornece transições e contexto do pedido, com referência ao cliente | CODIGO | src/modules/orders/order.service.ts |
| PRD-DEP-02 | docs/PRD.md | Dependência | JWT atual identifica usuário e papel | CODIGO | src/middlewares/auth.middleware.ts |
| PRD-DEP-03 | docs/PRD.md | Dependência | Reservar ao menos dois dias úteis para revisão de segurança pré-deploy | TRANSCRICAO | [09:46] Sofia; [09:47] Larissa |
| PRD-RISCO-01 | docs/PRD.md | Risco | Confirmar status sem registrar intenção se enqueue ficar fora da transação | TRANSCRICAO | [09:40] Bruno; [09:41] Diego |
| PRD-RISCO-03 | docs/PRD.md | Risco | Destino indisponível pode esgotar o orçamento de tentativas | TRANSCRICAO | [09:15] Diego; [09:16] Diego; [09:17] Larissa |
| PRD-CA-001 | docs/PRD.md | Critério de Aceitação | Ao cadastrar com JWT, URL HTTPS e filtro válido, retornar o segredo exclusivo e incluir o cadastro na listagem. Rejeitar URL HTTP | TRANSCRICAO | [09:31] Marcos; [09:32] Larissa; [09:33] Bruno; [09:23] Sofia; [09:21] Sofia |
| PRD-CA-002 | docs/PRD.md | Critério de Aceitação | Mudanças futuras usam o filtro atual e deixam de selecionar cadastros desativados ou removidos | TRANSCRICAO | [09:33] Bruno; [09:33] Marcos; [09:34] Bruno; [09:21] Sofia |
| PRD-CA-003 | docs/PRD.md | Critério de Aceitação | Notificação elegível mantém snapshot original mesmo após nova mudança; mudança válida sem inscritos não gera entrega | TRANSCRICAO | [09:52] Larissa; [09:34] Bruno |
| PRD-CA-004 | docs/PRD.md | Critério de Aceitação | Com falha ao registrar um evento elegível, reverter status, histórico e estoque aplicável. Com destino indisponível após o commit, manter a alteração de status | TRANSCRICAO | [09:40] Bruno; [09:06] Diego; [09:11] Diego |
| PRD-CA-005 | docs/PRD.md | Critério de Aceitação | Validar integridade do corpo e coexistência de segredos nas primeiras 24 h | TRANSCRICAO | [09:19] Sofia; [09:22] Sofia |
| PRD-CA-006 | docs/PRD.md | Critério de Aceitação | Timeout em 10 s aciona retry limitado; o esgotamento persiste falha com payload, motivo e instante | TRANSCRICAO | [09:42] Diego; [09:18] Diego; [09:48] Larissa |
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
| RFC-DEP-01 | docs/RFC.md | Dependência | Recebedor precisa validar corpo e reconhecer duplicatas | TRANSCRICAO | [09:19] Sofia; [09:25] Diego |
| RFC-DEC-01 | docs/RFC.md | Decisão | Outbox MySQL participa da transação de status e falha na gravação reverte alteração | TRANSCRICAO | [09:08] Larissa; [09:06] Diego; [09:40] Bruno |
| RFC-DEC-02 | docs/RFC.md | Decisão | HTTP após commit preserva confirmação do pedido independentemente do destino | TRANSCRICAO | [09:04] Bruno; [09:04] Larissa; [09:06] Diego |
| RFC-DEC-03 | docs/RFC.md | Decisão | Snapshot na mudança para preservar estado original, com UUID | TRANSCRICAO | [09:25] Diego; [09:51] Larissa; [09:52] Larissa |
| RFC-DEC-05 | docs/RFC.md | Decisão | Um worker em processo separado | TRANSCRICAO | [09:11] Diego; [09:12] Diego |
| RFC-DEC-06 | docs/RFC.md | Decisão | Polling de 2 segundos | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| RFC-DEC-07 | docs/RFC.md | Decisão | Prisma próprio por processo, mesmo banco | TRANSCRICAO | [09:30] Bruno |
| RFC-DEC-08 | docs/RFC.md | Restrição | Com um worker, a reunião previu ordem por pedido. A garantia global ficou fora da entrega | TRANSCRICAO | [09:12] Diego; [09:13] Larissa |
| RFC-DEC-11 | docs/RFC.md | Decisão | At-least-once com X-Event-Id para deduplicação no consumidor | TRANSCRICAO | [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| RFC-DEC-12 | docs/RFC.md | Decisão | Retry finito com cinco tentativas totais e recuperação manual após o limite | TRANSCRICAO | [09:15] Diego; [09:17] Larissa; [09:19] Larissa |
| RFC-DEC-13 | docs/RFC.md | Decisão | DLQ em tabela separada preserva payload, motivo e timestamp | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| RFC-CON-01 | docs/RFC.md | Contrato Público | Enviar JSON enxuto com identificador, dados da transição e total do pedido. Os headers discutidos são `X-Event-Id`, `X-Signature`, `X-Timestamp`, `Content-Type` e `X-Webhook-Id` | TRANSCRICAO | [09:43] Diego; [09:44] Diego; [09:45] Diego |
| RFC-SEG-04 | docs/RFC.md | Restrição | Como o HMAC cobre o corpo, o cabeçalho `X-Timestamp` de envio fica fora da assinatura | TRANSCRICAO | [09:22] Sofia; [09:44] Diego |
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
| RFC-Q-04 | docs/RFC.md | Questão em Aberto | Conciliar o total anunciado de cinco tentativas com a lista verbal de esperas de 1 min, 5 min, 30 min, 2 h e 12 h | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| RFC-Q-07 | docs/RFC.md | Risco | Cadastro público permite aceitar ADMIN, fragilizando requireRole no replay | CODIGO | src/modules/auth/auth.routes.ts; src/modules/auth/auth.schemas.ts; src/modules/auth/auth.controller.ts; src/modules/auth/auth.service.ts; src/modules/users/user.service.ts; src/modules/users/user.repository.ts |
| RFC-RISCO-04 | docs/RFC.md | Risco | Status e estoque têm leitura e atualização separadas; transação atual não introduz trava explícita nessa leitura | CODIGO | src/modules/orders/order.service.ts |
| RFC-TRD-02 | docs/RFC.md | Trade-off | Processo independente exige nova instância Prisma e operação própria | TRANSCRICAO | [09:11] Diego; [09:30] Bruno |
| RFC-TRD-05 | docs/RFC.md | Trade-off | Retry finito não assegura recepção de destino permanentemente indisponível | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| RFC-PLAN-01 | docs/RFC.md | Dependência | Larissa revisará design com Bruno e Diego antes da codificação | TRANSCRICAO | [09:50] Larissa |
| RFC-PLAN-02 | docs/RFC.md | Dependência | Sofia solicitou pelo menos dois dias úteis para revisão de segurança antes do deploy | TRANSCRICAO | [09:46] Sofia |
| RFC-PLAN-03 | docs/RFC.md | Restrição | Estimativa verbal de três sprints incluindo revisão | TRANSCRICAO | [09:47] Larissa |
| RFC-PLAN-04 | docs/RFC.md | Questão em Aberto | Atlas pediu fim de novembro; Marcos ficou de confirmar prazo | TRANSCRICAO | [09:45] Marcos; [09:47] Marcos |
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
| FDD-MOD-02 | docs/FDD.md | Decisão | Criar um entry point para o worker separado da API e manter a lógica no módulo de webhooks | TRANSCRICAO | [09:11] Larissa; [09:28] Bruno; [09:28] Diego |
| FDD-MOD-03 | docs/FDD.md | Integração | Seguir composição manual, classes e convenção ESM .js | CODIGO | src/app.ts |
| FDD-MOD-04 | docs/FDD.md | Integração | Handlers propagam next(err) | CODIGO | src/modules/orders/order.controller.ts |
| FDD-MOD-05 | docs/FDD.md | Decisão | Passar o cliente da transação de `changeStatus` ao produtor, conforme a proposta `publishWebhookEvent(tx, order, fromStatus, toStatus)` | TRANSCRICAO | [09:41] Bruno; [09:41] Diego |
| FDD-DATA-01 | docs/FDD.md | Integração | Schema atual usa camelCase, mapeamento de tabelas e UUID CHAR(36) | CODIGO | prisma/schema.prisma |
| FDD-DATA-02 | docs/FDD.md | Decisão | Modelar cadastro com URL, segredo, customer_id e indicador ativo | TRANSCRICAO | [09:21] Bruno; [09:21] Sofia |
| FDD-DATA-05 | docs/FDD.md | Decisão | Persistir snapshot imutável com UUID na mudança | TRANSCRICAO | [09:25] Diego; [09:51] Larissa; [09:52] Larissa |
| FDD-DATA-10 | docs/FDD.md | Decisão | Persistir falha em DLQ separada com payload, motivo e instante | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| FDD-DATA-15 | docs/FDD.md | Proposta | Diego propôs índices por estado e criação para consultar as pendências mais antigas em lotes pequenos | TRANSCRICAO | [09:08] Diego; [09:09] Diego |
| FDD-TX-11 | docs/FDD.md | Restrição | Evitar rede e segundo cliente/transação no publisher | TRANSCRICAO | [09:40] Bruno; [09:41] Bruno; [09:41] Diego; [09:06] Diego |
| FDD-FLUXO-01 | docs/FDD.md | Integração | PATCH /api/v1/orders/:id/status chama o controller de status | CODIGO | src/app.ts; src/routes/index.ts; src/modules/orders/order.routes.ts |
| FDD-FLUXO-02 | docs/FDD.md | Integração | Controller passa id, body e usuário ao serviço, devolvendo pedido atualizado | CODIGO | src/modules/orders/order.controller.ts |
| FDD-FLUXO-03 | docs/FDD.md | Regra de Negócio | Mesmo status ou transição proibida gera erro no fluxo atual | CODIGO | src/modules/orders/order.service.ts |
| FDD-FLUXO-04 | docs/FDD.md | Regra de Negócio | Filtrar por cliente e status de destino na inserção; zero registros sem interessados | TRANSCRICAO | [09:33] Marcos; [09:34] Bruno; [09:34] Diego |
| FDD-FLUXO-05 | docs/FDD.md | Decisão | Falha de enqueue reverte status, histórico e estoque aplicável; HTTP ocorre após commit | TRANSCRICAO | [09:06] Diego; [09:40] Bruno |
| FDD-WORK-01 | docs/FDD.md | Decisão | Iniciar com um único worker em processo separado, independente dos reinícios da API | TRANSCRICAO | [09:11] Diego; [09:12] Diego |
| FDD-WORK-02 | docs/FDD.md | Restrição | Consultar as pendências a cada dois segundos, começando pelas mais antigas | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| FDD-WORK-03 | docs/FDD.md | Restrição | Ordem por pedido discutida para um único worker | TRANSCRICAO | [09:12] Diego; [09:13] Larissa |
| FDD-WORK-04 | docs/FDD.md | Decisão | Mesmo banco e DATABASE_URL, instância Prisma própria do processo | TRANSCRICAO | [09:30] Bruno |
| FDD-WORK-05 | docs/FDD.md | Proposta | Larissa sugeriu `npm run worker` para iniciar o futuro processo de entregas | TRANSCRICAO | [09:11] Larissa |
| FDD-WORK-09 | docs/FDD.md | Restrição | Aplicar timeout HTTP de 10 s e tratar como falha para retry | TRANSCRICAO | [09:42] Diego |
| FDD-RETRY-01 | docs/FDD.md | Decisão | Limitar a cinco tentativas totais por ciclo. Ao esgotar o limite sem sucesso, persistir a falha na DLQ | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| FDD-RETRY-02 | docs/FDD.md | Questão em Aberto | A lista verbal de esperas foi 1 min, 5 min, 30 min, 2 h e 12 h. Ela diverge do total de cinco tentativas | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| FDD-DLQ-02 | docs/FDD.md | Requisito Funcional | Replay manual retorna falha da DLQ à fila pendente | TRANSCRICAO | [09:18] Diego; [09:19] Larissa |
| FDD-DLQ-03 | docs/FDD.md | Requisito de Segurança | Exigir ADMIN e registrar a identidade de quem pediu o replay | TRANSCRICAO | [09:36] Sofia; [09:36] Larissa |
| FDD-API-11 | docs/FDD.md | Contrato Público | Cadastrar via POST com URL HTTPS, cliente explícito e status de interesse. A plataforma gera e devolve o segredo | TRANSCRICAO | [09:31] Marcos; [09:32] Larissa; [09:23] Sofia |
| FDD-API-13 | docs/FDD.md | Contrato Público | Listar via GET os webhooks de um cliente explicitamente identificado | TRANSCRICAO | [09:33] Bruno; [09:37] Sofia |
| FDD-API-15 | docs/FDD.md | Contrato Público | Editar via PATCH a configuração do destino, incluindo o filtro por status | TRANSCRICAO | [09:33] Bruno; [09:33] Marcos |
| FDD-API-18 | docs/FDD.md | Contrato Público | Remover o cadastro via DELETE | TRANSCRICAO | [09:33] Bruno |
| FDD-API-20 | docs/FDD.md | Contrato Público | Rotacionar o segredo pela API, com validade paralela do anterior por 24 h | TRANSCRICAO | [09:21] Sofia; [09:22] Sofia |
| FDD-API-23 | docs/FDD.md | Contrato Público | Consultar `GET /webhooks/:id/deliveries` com payload, resultado, resposta disponível e tempo de resposta. A referência aos últimos 100 foi um exemplo | TRANSCRICAO | [09:34] Marcos |
| FDD-API-26 | docs/FDD.md | Contrato Público | Solicitar replay em `POST /admin/webhooks/dead-letter/:id/replay` | TRANSCRICAO | [09:35] Diego |
| FDD-OUT-01 | docs/FDD.md | Contrato Público | O corpo JSON contém `event_id`, `event_type` (`order.status_changed`), `timestamp` ISO 8601, `order_id`, `order_number`, `from_status`, `to_status`, `customer_id` e `total_cents` | TRANSCRICAO | [09:43] Diego |
| FDD-OUT-02 | docs/FDD.md | Contrato Público | Enviar `X-Event-Id` (UUID do evento), `X-Webhook-Id` (ID do cadastro), `X-Timestamp` (instante do envio), `X-Signature` (HMAC) e `Content-Type: application/json` | TRANSCRICAO | [09:44] Diego; [09:45] Diego |
| FDD-OUT-03 | docs/FDD.md | Decisão | Assinar corpo com HMAC-SHA256 e segredo por endpoint | TRANSCRICAO | [09:22] Sofia |
| FDD-OUT-04 | docs/FDD.md | Restrição | X-Timestamp de envio não integra HMAC sobre o corpo | TRANSCRICAO | [09:22] Sofia; [09:44] Diego |
| FDD-OUT-05 | docs/FDD.md | Decisão | At-least-once e deduplicação por identidade de evento | TRANSCRICAO | [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| FDD-OUT-13 | docs/FDD.md | Restrição | Centavos inteiros e estados do enum OrderStatus atual | CODIGO | prisma/schema.prisma |
| FDD-ERR-BASE | docs/FDD.md | Decisão | Adotar prefixo WEBHOOK_ nos erros do módulo | TRANSCRICAO | [09:29] Larissa |
| FDD-ERR-APP | docs/FDD.md | Integração | AppError informa mensagem, statusCode, errorCode e detalhes | CODIGO | src/shared/errors/app-error.ts |
| FDD-ERR-MW | docs/FDD.md | Integração | Middleware trata AppError/Zod/Prisma e fallback de erro inesperado | CODIGO | src/middlewares/error.middleware.ts |
| FDD-ERR-VALID | docs/FDD.md | Integração | Validação atual aplica Zod a body/query/params e usa erro genérico | CODIGO | src/middlewares/validate.middleware.ts |
| FDD-ERR-AUTH | docs/FDD.md | Integração | Autenticação e papel têm erros genéricos atuais | CODIGO | src/middlewares/auth.middleware.ts |
| FDD-ERR-PARSER | docs/FDD.md | Integração | express.json de 1 MB precede logger e router | CODIGO | src/app.ts |
| FDD-ERR-29 | docs/FDD.md | Integração | Fallback de parser pode cair em INTERNAL_SERVER_ERROR quando erro não for AppError, Zod ou Prisma conhecido | CODIGO | src/app.ts; src/middlewares/error.middleware.ts |
| FDD-ERR-PROP-01 | docs/FDD.md | Proposta | WEBHOOK_NOT_FOUND e WEBHOOK_INVALID_URL foram sugeridos como nomes de erro do módulo | TRANSCRICAO | [09:28] Bruno; [09:29] Larissa |
| FDD-RISCO-01 | docs/FDD.md | Risco | Escrever outbox fora do tx perde garantia entre mudança e intenção | TRANSCRICAO | [09:40] Bruno; [09:41] Diego |
| FDD-RISCO-03 | docs/FDD.md | Risco | Consumidor indisponível pode esgotar retry e requerer replay | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| FDD-API-01 | docs/FDD.md | Contrato Público | CRUD via JWT com qualquer role autenticada | TRANSCRICAO | [09:32] Marcos; [09:32] Larissa; [09:37] Sofia |
| FDD-API-02 | docs/FDD.md | Integração | AuthUser possui id, email e role; authenticate usa Bearer e requireRole confere papel | CODIGO | src/middlewares/auth.middleware.ts |
| FDD-API-03 | docs/FDD.md | Integração | Schema não liga usuários a clientes | CODIGO | prisma/schema.prisma |
| FDD-API-04 | docs/FDD.md | Risco | Registro público aceita ADMIN e preserva papel no serviço | CODIGO | src/modules/auth/auth.routes.ts; src/modules/auth/auth.schemas.ts; src/modules/auth/auth.controller.ts; src/modules/auth/auth.service.ts; src/modules/users/user.service.ts; src/modules/users/user.repository.ts |
| FDD-SEG-01 | docs/FDD.md | Requisito de Segurança | Exigir HTTPS e validar com schema | TRANSCRICAO | [09:23] Sofia |
| FDD-SEG-10 | docs/FDD.md | Integração | Pedido referencia Customer e usuário criador; histórico de status referencia o usuário autor | CODIGO | prisma/schema.prisma |
| FDD-API-32 | docs/FDD.md | Decisão | Usar requireRole existente para replay | TRANSCRICAO | [09:36] Larissa |
| FDD-API-10 | docs/FDD.md | Integração | Middleware atual reflete ou cria X-Request-Id | CODIGO | src/middlewares/request-logger.middleware.ts |
| FDD-OBS-01 | docs/FDD.md | Integração | Pino e redação de campos conhecidos já existem | CODIGO | src/shared/logger/index.ts |
| FDD-OBS-02 | docs/FDD.md | Integração | Logger HTTP próprio acrescenta ID e registra duração | CODIGO | src/middlewares/request-logger.middleware.ts |
| FDD-OBS-03 | docs/FDD.md | Integração | Manifesto não declara SDK de métricas ou tracing dedicado; pino-http consta como dependência | CODIGO | package.json |
| FDD-OBS-08 | docs/FDD.md | Integração | AppError não é logado automaticamente no ramo correspondente | CODIGO | src/middlewares/error.middleware.ts |
| FDD-WORK-12 | docs/FDD.md | Integração | Bootstrap atual da API chama process.exit após desconectar Prisma no shutdown | CODIGO | src/server.ts |
| FDD-DEP-01 | docs/FDD.md | Dependência | O manifesto declara Node.js ≥20, Express 4.21.1, Prisma 5.22.0, Zod 3.23.8, Pino 9.5.0, uuid 11.0.3, Vitest 2.1.4 e Supertest 7.0.0 | CODIGO | package.json |
| FDD-DEP-03 | docs/FDD.md | Integração | Loader Zod valida ambiente e encerra em configuração inválida; carrega banco, JWT, porta e logging | CODIGO | src/config/env.ts |
| FDD-DEP-04 | docs/FDD.md | Integração | Schema declara SHADOW_DATABASE_URL | CODIGO | prisma/schema.prisma |
| FDD-DEP-07 | docs/FDD.md | Integração | Fábrica e singleton de PrismaClient existentes | CODIGO | src/config/database.ts |
| FDD-DEP-13 | docs/FDD.md | Integração | Compose configura MySQL local | CODIGO | docker-compose.yml |
| FDD-API-06 | docs/FDD.md | Contrato Público | Schema e API de Pedidos usam nomes camelCase e IDs UUID, como referência de convenção | CODIGO | prisma/schema.prisma; src/modules/orders/order.schemas.ts |
| FDD-API-07 | docs/FDD.md | Integração | Padrão de paginação de pedidos usa page >= 1 e pageSize de 1 a 100, com padrões 1 e 20 | CODIGO | src/modules/orders/order.schemas.ts |
| FDD-API-08 | docs/FDD.md | Integração | Helpers padronizam envelope data/pagination | CODIGO | src/shared/http/response.ts |
| FDD-DEP-15 | docs/FDD.md | Integração | OrderRepository concentra leitura com relações e transação de listagem | CODIGO | src/modules/orders/order.repository.ts |
| FDD-DEP-16 | docs/FDD.md | Integração | Router agrega controllers e rotas dos módulos atuais | CODIGO | src/routes/index.ts |
| FDD-DEP-17 | docs/FDD.md | Integração | Setup de teste usa Prisma e limpa tabelas em ordem referencial antes de cada teste | CODIGO | tests/setup.ts |
| FDD-INT-08 | docs/FDD.md | Integração | OrderService.create cria o pedido PENDING com histórico de criação | CODIGO | src/modules/orders/order.service.ts |
| FDD-INT-09 | docs/FDD.md | Integração | O projeto já oferece GET /api/v1/orders/:id para consulta detalhada | CODIGO | src/app.ts; src/routes/index.ts; src/modules/orders/order.routes.ts |
| FDD-TESTE-01 | docs/FDD.md | Integração | Testes atuais usam Vitest/Supertest, buildApp com Prisma, MySQL e factories | CODIGO | tests/orders.test.ts; tests/setup.ts; tests/helpers/factories.ts |
| FDD-TESTE-02 | docs/FDD.md | Integração | Vitest configura arquivos sem paralelismo e forks/singleFork | CODIGO | vitest.config.ts |
| FDD-AC02 | docs/FDD.md | Critério de Aceitação | Falha da persistência de evento elegível reverte status, histórico e estoque aplicável | TRANSCRICAO | [09:40] Bruno; [09:06] Diego |
| FDD-AC03 | docs/FDD.md | Critério de Aceitação | Sem interessados, zero registros e mudança válida confirmada | TRANSCRICAO | [09:34] Bruno; [09:34] Diego |
| FDD-AC05 | docs/FDD.md | Critério de Aceitação | Consumidor offline não bloqueia mudança; worker envia fora da transação | TRANSCRICAO | [09:04] Bruno; [09:06] Diego; [09:11] Diego |
| FDD-AC07 | docs/FDD.md | Critério de Aceitação | HMAC valida corpo original e falha após adulteração | TRANSCRICAO | [09:19] Sofia; [09:22] Sofia |
| FDD-AC14-AUD | docs/FDD.md | Critério de Aceitação | Apenas ADMIN executa replay e autor é auditado | TRANSCRICAO | [09:36] Sofia; [09:36] Larissa |
| FDD-TX-03 | docs/FDD.md | Risco | Validação e decremento de estoque são etapas distintas, sem trava explícita nessa leitura | CODIGO | src/modules/orders/order.service.ts |
| FDD-PLAN-01 | docs/FDD.md | Dependência | Sofia solicitou pelo menos dois dias úteis de revisão de segurança antes do deploy | TRANSCRICAO | [09:46] Sofia |
| FDD-PLAN-02 | docs/FDD.md | Restrição | Três sprints com revisão e prazo da Atlas a confirmar | TRANSCRICAO | [09:45] Marcos; [09:47] Larissa; [09:47] Marcos |
| FDD-RISC-07 | docs/FDD.md | Risco | Fluxo de status e fluxo de exclusão leem e depois escrevem sem trava explícita na leitura | CODIGO | src/modules/orders/order.service.ts; src/modules/orders/order.repository.ts |
| ADR-001-INT-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Integração | Compose demonstra serviço MySQL local com volume e healthcheck | CODIGO | docker-compose.yml |
| ADR-001-ALT-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Alternativa Descartada | Redis Streams adicionaria infraestrutura considerada excessiva | TRANSCRICAO | [09:07] Larissa; [09:07] Diego |
| ADR-001 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Decisão | Aplicação usa MySQL relacional compartilhado com Prisma | CODIGO | prisma/schema.prisma; prisma/migrations/20260519182739_init/migration.sql |
| ADR-001-DEC-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Decisão | Reusar MySQL existente para a outbox e o mesmo commit dos dados de Pedidos | TRANSCRICAO | [09:06] Diego; [09:07] Diego; [09:08] Larissa |
| ADR-001-TRD-01 | docs/adrs/ADR-001-mysql-como-banco-relacional.md | Trade-off | Há dependência do provider MySQL, modelos e migração que uma substituição precisaria revisar | CODIGO | prisma/schema.prisma; prisma/migrations/20260519182739_init/migration.sql |
| ADR-002-INT-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Integração | Fábrica fornece PrismaClient e composição injeta o cliente em repository e serviço de Pedidos | CODIGO | src/config/database.ts; src/app.ts |
| ADR-002-INT-02 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Integração | Repository faz consultas e serviço de Pedidos coordena alterações de múltiplos modelos no tx | CODIGO | src/modules/orders/order.repository.ts; src/modules/orders/order.service.ts |
| ADR-002-ALT-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Alternativa Descartada | Preferir função que recebe o tx a injetar repository integral | TRANSCRICAO | [09:41] Bruno; [09:41] Diego |
| ADR-002 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Decisão | Projeto usa Prisma Client, schema declarativo e migrações; cliente e CLI declarados em 5.22.0 | CODIGO | package.json; prisma/schema.prisma; prisma/migrations/20260519182739_init/migration.sql |
| ADR-002-DEC-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Decisão | Futura emissão usa o tx recebido para commit conjunto | TRANSCRICAO | [09:40] Bruno; [09:41] Bruno; [09:41] Diego |
| ADR-002-DEC-02 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Decisão | Worker cria instância de Prisma própria por processo | TRANSCRICAO | [09:30] Bruno |
| ADR-002-TRD-01 | docs/adrs/ADR-002-prisma-para-acesso-a-dados.md | Trade-off | Trocar Prisma exigiria adaptar consumidores acoplados a seus tipos e operações | CODIGO | src/config/database.ts; src/modules/orders/order.service.ts; src/modules/orders/order.repository.ts |
| ADR-003-INT-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Integração | Fluxo atual transaciona status, estoque aplicável e histórico | CODIGO | src/modules/orders/order.service.ts |
| ADR-003-INT-02 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Integração | Schema atual não contém outbox de webhook | CODIGO | prisma/schema.prisma |
| ADR-003-ALT-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Alternativa Descartada | Chamada síncrona prolongaria transação e acoplaria falha externa | TRANSCRICAO | [09:04] Bruno; [09:04] Larissa; [09:06] Diego |
| ADR-003-ALT-02 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Alternativa Descartada | Redis Streams exige infraestrutura adicional excessiva | TRANSCRICAO | [09:07] Larissa; [09:07] Diego |
| ADR-003-ALT-03 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Alternativa Descartada | Renderização tardia pode refletir estado posterior | TRANSCRICAO | [09:51] Bruno; [09:52] Larissa |
| ADR-003 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Decisão | Usar outbox MySQL na transação de mudança; falha no registro impede o commit | TRANSCRICAO | [09:06] Diego; [09:08] Larissa; [09:40] Bruno |
| ADR-003-DEC-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Decisão | Envio HTTP ocorre em separado após commit | TRANSCRICAO | [09:06] Diego |
| ADR-003-DEC-02 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Decisão | Guardar snapshot na inserção com identidade UUID | TRANSCRICAO | [09:25] Diego; [09:51] Larissa; [09:52] Larissa |
| ADR-003-TRD-01 | docs/adrs/ADR-003-outbox-mysql-na-transacao-de-status.md | Trade-off | Escrita adicional é condição do commit, que garante somente intenção | TRANSCRICAO | [09:06] Diego; [09:40] Bruno; [09:24] Diego |
| ADR-004-INT-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Integração | Router atual não monta webhooks | CODIGO | src/routes/index.ts |
| ADR-004-DEP-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Dependência | Documentação ao consumidor dá destaque à possibilidade de repetição | TRANSCRICAO | [09:26] Marcos |
| ADR-004-ALT-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Alternativa Descartada | Exactly-once exige coordenação complexa entre produtor e consumidor | TRANSCRICAO | [09:25] Diego; [09:26] Larissa |
| ADR-004 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Decisão | Entrega outbound at-least-once com deduplicação no consumidor por X-Event-Id | TRANSCRICAO | [09:02] Marcos; [09:24] Diego; [09:25] Diego; [09:26] Larissa |
| ADR-004-CON-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Contrato Público | UUID gerado na outbox também aparece no payload | TRANSCRICAO | [09:25] Diego; [09:43] Diego |
| ADR-004-DEC-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Restrição | Limitar o ciclo de entrega a cinco tentativas totais | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |
| ADR-004-TRD-01 | docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md | Trade-off | Consumidor assume deduplicação; duplicatas HTTP continuam possíveis | TRANSCRICAO | [09:25] Sofia; [09:25] Diego |
| ADR-005-INT-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Integração | JWT atual autentica usuários da API | CODIGO | src/middlewares/auth.middleware.ts |
| ADR-005-INT-02 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Integração | Pino redige campos conhecidos sem campo de segredo de webhook | CODIGO | src/shared/logger/index.ts |
| ADR-005-SEG-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Restrição | Exigir HTTPS | TRANSCRICAO | [09:23] Sofia |
| ADR-005-DEP-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Dependência | Revisão de segurança solicitada antes do deploy | TRANSCRICAO | [09:46] Sofia; [09:49] Sofia |
| ADR-005-ALT-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Alternativa Descartada | Segredo global amplia alcance de vazamento | TRANSCRICAO | [09:21] Sofia |
| ADR-005 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Decisão | HMAC-SHA256 sobre o corpo, segredo distinto por endpoint e envio de assinatura ao consumidor | TRANSCRICAO | [09:19] Sofia; [09:20] Sofia; [09:22] Sofia; [09:44] Diego |
| ADR-005-DEC-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Decisão | Rotação com segredo anterior válido em paralelo por 24 horas | TRANSCRICAO | [09:21] Sofia; [09:22] Sofia |
| ADR-005-CON-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Contrato Público | Como o HMAC cobre o corpo, o timestamp de envio informado em `X-Timestamp` fica fora da assinatura | TRANSCRICAO | [09:22] Sofia; [09:44] Diego |
| ADR-005-TRD-01 | docs/adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md | Trade-off | Rotação de credencial exclusiva exige migração do consumidor na janela de convivência | TRANSCRICAO | [09:21] Sofia |
| ADR-006-INT-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Integração | Bootstrap atual atende API; manifesto não contém comando de worker | CODIGO | src/server.ts; package.json |
| ADR-006-OBJ-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Métrica | Atender à expectativa de notificar abaixo de dez segundos | TRANSCRICAO | [09:02] Marcos; [09:09] Diego |
| ADR-006-ALT-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Alternativa Descartada | Worker na API compartilharia seus reinícios | TRANSCRICAO | [09:11] Diego |
| ADR-006-ALT-02 | docs/adrs/ADR-006-worker-separado-com-polling.md | Alternativa Descartada | Trigger não notifica diretamente processo externo | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| ADR-006 | docs/adrs/ADR-006-worker-separado-com-polling.md | Decisão | Um worker separado lê a outbox por polling a cada 2 segundos | TRANSCRICAO | [09:09] Diego; [09:10] Larissa; [09:11] Diego; [09:12] Diego |
| ADR-006-DEC-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Decisão | Reusar banco e stack, com Prisma próprio por processo | TRANSCRICAO | [09:11] Diego; [09:30] Bruno |
| ADR-006-DEC-02 | docs/adrs/ADR-006-worker-separado-com-polling.md | Decisão | Lógica do worker no módulo da feature e entry point independente | TRANSCRICAO | [09:11] Larissa; [09:28] Bruno; [09:28] Diego |
| ADR-006-TRD-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Trade-off | Polling é consulta periódica com espera até a próxima busca | TRANSCRICAO | [09:09] Diego; [09:10] Larissa |
| ADR-006-ESC-01 | docs/adrs/ADR-006-worker-separado-com-polling.md | Fora de Escopo | Escala horizontal adiada; locks e particionamento são hipóteses futuras | TRANSCRICAO | [09:12] Diego; [09:13] Diego |
| ADR-007-INT-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Integração | Schema atual ainda não inclui persistência de outbox/DLQ | CODIGO | prisma/schema.prisma |
| ADR-007-ALT-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Alternativa Descartada | Retry indefinido deixa eventos pendurados | TRANSCRICAO | [09:15] Diego; [09:17] Larissa |
| ADR-007-ALT-02 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Alternativa Descartada | Três tentativas insuficientes para manutenção de duas horas | TRANSCRICAO | [09:16] Diego; [09:17] Larissa |
| ADR-007-ALT-03 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Alternativa Descartada | Falha na própria outbox dificulta separar pendências, investigar e recuperar | TRANSCRICAO | [09:17] Larissa; [09:18] Diego; [09:48] Larissa |
| ADR-007 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão | Retentar até o limite de cinco tentativas totais | TRANSCRICAO | [09:15] Diego; [09:17] Larissa; [09:48] Larissa |
| ADR-007-DEC-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão | DLQ separada após esgotamento preserva informações para diagnóstico e replay | TRANSCRICAO | [09:18] Diego; [09:48] Larissa |
| ADR-007-DEC-02 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Decisão | Replay manual por ADMIN retorna a pendente e registra autor | TRANSCRICAO | [09:18] Diego; [09:19] Larissa; [09:36] Sofia; [09:36] Larissa |
| ADR-007-DEC-03 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Restrição | Timeout de 10 segundos seguido de retry | TRANSCRICAO | [09:42] Diego |
| ADR-007-TRD-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Trade-off | Após limite, recuperação exige intervenção manual | TRANSCRICAO | [09:15] Diego; [09:18] Diego; [09:19] Larissa |
| ADR-007-Q-01 | docs/adrs/ADR-007-retry-com-backoff-e-dlq.md | Questão em Aberto | O total anunciado de cinco tentativas diverge dos intervalos citados de 1 min, 5 min, 30 min, 2 h e 12 h | TRANSCRICAO | [09:17] Larissa; [09:48] Larissa |

## Itens sem evidência primária suficiente

As propostas e perguntas abaixo permanecem no denominador. Cada referência indica onde o item foi documentado.

### PRD

- **PRD-Q-03** — Como comprovar que o usuário autenticado pode administrar o cliente informado? [PRD](PRD.md)
- **PRD-Q-07** — Como edições, desativação e remoção de destinos afetam entregas pendentes e o histórico? [PRD](PRD.md)
- **PRD-MET-02** — Qual percentil, carga e janela serão usados para aferir a meta de latência? [PRD](PRD.md)
- **PRD-Q-08** — Qual é o limite exato em bytes e qual deve ser o efeito do excesso sobre a transição? [PRD](PRD.md)

- **PRD-MET-03** — Proposta de medição: medir da confirmação do status até a primeira entrega bem-sucedida. Validar carga, janela e percentil antes de publicar resultados. [PRD](PRD.md)
- **PRD-RISCO-EST-01** — Avaliação qualitativa do risco de perda de evento quando a gravação fica fora da transação: probabilidade média e impacto alto. [PRD](PRD.md)
- **PRD-RISCO-EST-02** — Avaliação qualitativa do risco de indisponibilidade do destino esgotar as tentativas: probabilidade média e impacto médio. [PRD](PRD.md)
- **PRD-TESTE-04** — Validar o contrato com um destino de teste e usar uma falha controlada de persistência para conferir rollback de status, histórico e estoque. [PRD](PRD.md)

### RFC

- **RFC-Q-05** — Como preservar a ordem por pedido quando uma entrega entra em retry ou é reprocessada? [RFC](RFC.md)
- **RFC-Q-06** — Como vincular o usuário ao cliente e assegurar que o papel ADMIN seja concedido apenas por fluxo autorizado? [RFC](RFC.md)
- **RFC-Q-08** — Que política de destinos externos e proteção de credenciais deve ser aprovada na revisão de Segurança? [RFC](RFC.md)
- **RFC-Q-10** — Quais prazos de retenção se aplicam à Outbox, à DLQ e ao histórico? [RFC](RFC.md)
- **RFC-Q-12** — Como tratar payload excessivo na fronteira transacional? [RFC](RFC.md)

### FDD

- **FDD-DATA-07** — Qual será a cardinalidade entre evento e entrega por destino, com quais chaves e índices? [FDD](FDD.md)
- **FDD-TX-04** — Como o worker reivindicará trabalho, recuperará execução interrompida e evitará confirmações concorrentes? [FDD](FDD.md)
- **FDD-RETRY-03** — Como conciliar o total de cinco tentativas com os cinco intervalos verbalizados, incluindo 12 h? [FDD](FDD.md)
- **FDD-RETRY-04** — Quais respostas HTTP e falhas de rede permitem retry, confirmam sucesso ou encerram o ciclo? [FDD](FDD.md)
- **FDD-RETRY-08** — Como retry e replay interagem com ordem por pedido, identidade do evento e preservação do snapshot? [FDD](FDD.md)
- **FDD-DLQ-06** — Como evitar replays administrativos concorrentes para a mesma falha? [FDD](FDD.md)
- **FDD-API-05** — Como corrigir a atribuição de ADMIN e estabelecer isolamento por cliente? [FDD](FDD.md)
- **FDD-API-12** — Quais paths, DTOs, status HTTP e paginação devem ser adotados onde a reunião definiu somente a operação? [FDD](FDD.md)
- **FDD-API-17** — Qual destino usar em tentativas pendentes após edição e como tratar desativação e remoção? [FDD](FDD.md)
- **FDD-OUT-12** — Qual método HTTP usar no envio outbound? [FDD](FDD.md)
- **FDD-SEG-04** — Como gerar, armazenar, recuperar e rotacionar os segredos e representar a coexistência de assinaturas no wire? [FDD](FDD.md)
- **FDD-SEG-06** — Quais destinos e políticas de saída serão permitidos após a revisão de Segurança? [FDD](FDD.md)
- **FDD-DATA-14** — Como interpretar 64 KB em bytes e qual resposta dará a API de Pedidos em caso de excesso? [FDD](FDD.md)
- **FDD-OBS-06** — Como medir latência e falhas e correlacionar origem, entrega e replay usando observabilidade aprovada? [FDD](FDD.md)
- **FDD-TESTE-10** — Quais fixtures, cenários de concorrência, carga e ambiente de homologação devem ser adotados? [FDD](FDD.md)

- **FDD-PROP-DADOS-01** — Persistir uma entrega por destino inscrito, com ID do evento, ID do destino, snapshot, estado, contador e próxima tentativa. [FDD](FDD.md)
- **FDD-PROP-DADOS-02** — Usar a combinação evento e destino como chave única para evitar duplicação de registros no produtor. [FDD](FDD.md)
- **FDD-PROP-WORK-01** — Reivindicar a entrega por atualização condicional; confirmar a posse antes de registrar o resultado. Executar HTTP fora da transação e tornar a reivindicação recuperável após interrupção. [FDD](FDD.md)
- **FDD-PROP-DLQ-01** — Persistir o registro da DLQ e a transição para estado terminal na mesma transação. No replay, voltar a pendente e registrar a auditoria sem descartar o histórico de tentativas. [FDD](FDD.md)
- **FDD-PROP-HTTP-01** — Usar Authorization: Bearer, Content-Type: application/json para os corpos e base /api/v1 no router da aplicação. [FDD](FDD.md)
- **FDD-PROP-HTTP-02** — Usar POST /api/v1/webhooks para cadastro e PATCH /api/v1/webhooks/:id para edição. [FDD](FDD.md)
- **FDD-PROP-HTTP-03** — Cadastro recebe customerId, url e statuses, responde 201 com id e secret e falha com 400 para URL inválida. [FDD](FDD.md)
- **FDD-PROP-HTTP-04** — Edição do filtro recebe statuses, responde 200 com id e filtro atualizado e retorna 404 se o cadastro não existir. [FDD](FDD.md)
- **FDD-PROP-HTTP-05** — Usar POST /api/v1/webhooks/:id/rotate-secret, com confirmação explícita no corpo. Responder 200 com novo segredo e 404 se o cadastro não existir. [FDD](FDD.md)
- **FDD-PROP-HTTP-06** — Replay recebe confirmação explícita, responde 202 com a entrega recolocada em pendente e retorna 403 se faltar o papel ADMIN. A resposta 202 indica aceitação, sem confirmação de entrega. [FDD](FDD.md)
- **FDD-PROP-ERR-01** — Mapear WEBHOOK_INVALID_URL para 400 e WEBHOOK_NOT_FOUND para 404 no contrato administrativo. [FDD](FDD.md)
- **FDD-PROP-ERR-02** — Registrar timeout de entrega como WEBHOOK_DELIVERY_TIMEOUT. [FDD](FDD.md)
- **FDD-PROP-ERR-03** — Mapear replay por usuário autenticado sem ADMIN para WEBHOOK_REPLAY_FORBIDDEN, com 403. [FDD](FDD.md)
- **FDD-PROP-RET-01** — Para implementação inicial, propor 1 min, 5 min, 30 min e 2 h entre as quatro retentativas após a chamada inicial. O intervalo de 12 h da reunião permanece pendente de decisão. [FDD](FDD.md)
- **FDD-PROP-RET-02** — Interpretar 2xx como sucesso e disponibilizar classificação de falhas HTTP e de transporte para revisão antes de estabilizar a política de retry. [FDD](FDD.md)
- **FDD-PROP-OBS-01** — Métricas: medir duração entre commit e primeira entrega bem-sucedida e acompanhar volume de pendências, falhas finais e retries. [FDD](FDD.md)
- **FDD-PROP-OBS-02** — Logs: registrar início e resultado da tentativa, ida à DLQ e replay auditado. Usar Pino e omitir segredo e payload integral. [FDD](FDD.md)
- **FDD-PROP-OBS-03** — Tracing: correlacionar alteração de pedido, registro da outbox e envio pelo ID do evento, mantendo a correlação após reinício e replay. Avaliar o mecanismo de exportação. [FDD](FDD.md)
- **FDD-PROP-INTEG-01** — Adicionar ao schema os modelos de configuração, entrega pendente e DLQ e montar as novas rotas no router existente. [FDD](FDD.md)
- **FDD-PROP-RISCO-01** — Mitigar duplicação após interrupção do worker preservando o ID do evento em tentativas repetidas e exercitando recuperação com um consumidor de teste. [FDD](FDD.md)

### ADR-001

- **ADR-001-Q-01** — Qual capacidade será necessária para a carga conjunta de Pedidos e Outbox? [ADR-001](adrs/ADR-001-mysql-como-banco-relacional.md)

### ADR-002

- **ADR-002-Q-01** — Qual orçamento de conexões acomodará API e worker? [ADR-002](adrs/ADR-002-prisma-para-acesso-a-dados.md)

### ADR-003

- **ADR-003-DEC-03** — Como modelar a relação entre um evento do pedido e os destinos inscritos? [ADR-003](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md)

### ADR-004

- **ADR-004-CON-02** — Qual retenção de IDs os consumidores precisam adotar para deduplicar e aceitar replays tardios? [ADR-004](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md)

### ADR-005

- **ADR-005-CON-03** — Como representar dois segredos válidos no contrato e como proteger seu armazenamento? [ADR-005](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md)

### ADR-006

- **ADR-006-DEC-04** — Como ordenar entregas durante retry e recuperar trabalho abandonado? [ADR-006](adrs/ADR-006-worker-separado-com-polling.md)

### ADR-007

- **ADR-007-DEC-04** — Como resolver o intervalo excedente de 12 h mantendo o total anunciado de cinco tentativas? [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md)

## Divergências e limites das evidências

- A reunião registra cinco tentativas totais e enumera cinco intervalos. O pacote preserva os dois dados e deixa o agendamento definitivo em aberto.
- A reunião corrigiu `customer_id` implícito no JWT para identificador explícito na requisição. O vínculo de autorização com o cliente e a correção da atribuição pública de ADMIN continuam pendentes.
- O intervalo de polling é de dois segundos. A meta de notificação em menos de dez segundos exige aferição de ponta a ponta.
- O cabeçalho `X-Timestamp` de envio está fora do corpo autenticado. A deduplicação usa a identidade do evento.
- As linhas de código descrevem o estado existente. A outbox, o worker, as rotas webhook e a DLQ fazem parte do desenho.
