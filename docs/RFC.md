# RFC — Webhooks de mudança de status de pedidos

- **Autora** · Larissa (Tech Lead)
- **Status** · Proposta para revisão
- **Data do documento** · 16/11/2026
- **Revisores** · Larissa (Tech Lead), Marcos (Produto), Bruno (Pedidos), Diego (Plataforma) e Sofia (Segurança)

## Resumo executivo (TL;DR)

Usar outbox transacional no MySQL e um worker independente para entregar mudanças de status via HTTP assinado. Limitar retries, registrar falhas esgotadas em DLQ e permitir replay administrativo. O consumidor deduplica pelo ID do evento. Reutilizar os padrões e a infraestrutura do OMS mantém a proposta compatível com Pedidos.

## 1. Contexto e problema

- **RFC-CTX-01** — Três clientes B2B querem substituir polling caro e lento em GET /orders por notificações outbound.
- **RFC-CTX-02** — Atlas sinalizou migração para concorrente se a necessidade não for atendida.
- **RFC-OBJ-01** — Meta de notificar em menos de dez segundos.
- **RFC-INT-01** — Plataforma modular REST TypeScript/Express usa Prisma/MySQL. [app.ts](../src/app.ts) · [package.json](../package.json) · [schema.prisma](../prisma/schema.prisma)
- **RFC-INT-02** — Mudança de status transaciona histórico e estoque; débito em PENDING → PAID. [order.service.ts](../src/modules/orders/order.service.ts) · [order.status.ts](../src/modules/orders/order.status.ts)
- **RFC-INT-03** — Código atual sem rotas webhook, modelos de outbox/DLQ ou script de worker. [index.ts](../src/routes/index.ts) · [schema.prisma](../prisma/schema.prisma) · [package.json](../package.json)

## 2. Escopo e requisitos confirmados

- **RFC-RF-01** — Criar destino com URL, estado ativo, filtro e segredo gerado e devolvido.
- **RFC-RF-02** — Editar configuração via PATCH.
- **RFC-RF-03** — Remover configuração via DELETE.
- **RFC-RF-04** — Listar webhooks por cliente via GET.
- **RFC-RF-05** — Rotacionar segredo via API.
- **RFC-RF-06** — Consultar histórico de entregas.
- **RFC-RF-07** — Replay manual da DLQ por ADMIN com auditoria do autor.
- **RFC-REG-01** — CRUD admite qualquer papel autenticado.
- **RFC-REG-02** — Identificar cliente em body ou path, sem extrair do JWT.
- **RFC-REG-03** — Filtrar na inserção por status de destino e dispensar enqueue sem inscritos.
- **RFC-SEG-01** — HTTPS obrigatório.
- **RFC-SEG-02** — HMAC-SHA256 sobre o corpo com segredo por endpoint.
- **RFC-SEG-03** — Segredo anterior válido por 24 h.
- **RFC-LIM-01** — Limite de 64 KB com erro e sem truncamento.
- **RFC-LIM-02** — Timeout HTTP de 10 s tratado como falha.
- **RFC-ESC-01** — Email de fallback adiado.
- **RFC-ESC-02** — Dashboard visual adiado.
- **RFC-DEP-01** — Recebedor precisa validar corpo e reconhecer duplicatas.

## 3. Proposta técnica

- **RFC-DEC-01** — Outbox MySQL participa da transação de status e falha na gravação reverte alteração.
- **RFC-DEC-02** — HTTP após commit preserva confirmação do pedido independentemente do destino.
- **RFC-DEC-03** — Snapshot na mudança para preservar estado original, com UUID.
- **RFC-DEC-05** — Um worker em processo separado.
- **RFC-DEC-06** — Polling de 2 segundos.
- **RFC-DEC-07** — Prisma próprio por processo, mesmo banco.
- **RFC-DEC-08** — Com um worker, a reunião previu ordem por pedido. A garantia global ficou fora da entrega.
- **RFC-DEC-11** — At-least-once com X-Event-Id para deduplicação no consumidor.
- **RFC-DEC-12** — Retry finito com cinco tentativas totais e recuperação manual após o limite.
- **RFC-DEC-13** — DLQ em tabela separada preserva payload, motivo e timestamp.
- **RFC-CON-01** — Enviar JSON enxuto com identificador, dados da transição e total do pedido. Os headers discutidos são `X-Event-Id`, `X-Signature`, `X-Timestamp`, `Content-Type` e `X-Webhook-Id`.
- **RFC-SEG-04** — Como o HMAC cobre o corpo, o cabeçalho `X-Timestamp` de envio fica fora da assinatura.
- **RFC-INT-04** — Reusar organização modular, Zod, Pino, AppError e middleware.

## 4. Alternativas consideradas

- **RFC-ALT-01** — HTTP síncrono prolongaria a transação e acoplaria falha externa ao negócio.
- **RFC-ALT-02** — Redis Streams adicionaria infraestrutura excessiva para equipe pequena.
- **RFC-ALT-03** — Trigger não notifica processo externo diretamente; polling foi escolhido.
- **RFC-ALT-04** — Worker junto à API acoplaria reinícios.
- **RFC-ALT-05** — Exactly-once exigiria coordenação mais complexa entre as duas partes.
- **RFC-ALT-06** — Retry indefinido deixaria eventos pendurados.
- **RFC-ALT-07** — Três tentativas insuficientes para manutenção de duas horas.
- **RFC-ALT-08** — Falha apenas na outbox perde separação e clareza para investigação.
- **RFC-ALT-09** — Segredo global ampliaria impacto de vazamento.
- **RFC-ALT-10** — Montar payload no envio pode refletir estado posterior.

## 5. Questões em aberto

- **RFC-Q-01** — Observar rate limiting de saída para decidir depois.
- **RFC-Q-02** — Arquivamento adiado; 30 dias foi sugestão sem política aprovada.
- **RFC-Q-03** — Adiar escala; locks e particionamento foram aventados.
- **RFC-Q-04** — Conciliar o total anunciado de cinco tentativas com a lista verbal de esperas de 1 min, 5 min, 30 min, 2 h e 12 h.

## 6. Impacto e riscos

- **RFC-Q-07** — Cadastro público permite aceitar ADMIN, fragilizando requireRole no replay. [auth.routes.ts](../src/modules/auth/auth.routes.ts) · [auth.schemas.ts](../src/modules/auth/auth.schemas.ts) · [auth.controller.ts](../src/modules/auth/auth.controller.ts) · [auth.service.ts](../src/modules/auth/auth.service.ts) · [user.service.ts](../src/modules/users/user.service.ts) · [user.repository.ts](../src/modules/users/user.repository.ts)
- **RFC-RISCO-04** — Status e estoque têm leitura e atualização separadas; transação atual não introduz trava explícita nessa leitura. [order.service.ts](../src/modules/orders/order.service.ts)
- **RFC-TRD-02** — Processo independente exige nova instância Prisma e operação própria.
- **RFC-TRD-05** — Retry finito não assegura recepção de destino permanentemente indisponível.

## 7. Decisões relacionadas

- [ADR-001](adrs/ADR-001-mysql-como-banco-relacional.md)
- [ADR-002](adrs/ADR-002-prisma-para-acesso-a-dados.md)
- [ADR-003](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-004](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md)
- [ADR-005](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md)
- [ADR-006](adrs/ADR-006-worker-separado-com-polling.md)
- [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md)

## 8. Revisão e encaminhamento

- **RFC-PLAN-01** — Larissa revisará design com Bruno e Diego antes da codificação.
- **RFC-PLAN-02** — Sofia solicitou pelo menos dois dias úteis para revisão de segurança antes do deploy.
- **RFC-PLAN-03** — Estimativa verbal de três sprints incluindo revisão.
- **RFC-PLAN-04** — Atlas pediu fim de novembro; Marcos ficou de confirmar prazo.

## Pendências adicionais de revisão

- **RFC-Q-05** — Como preservar a ordem por pedido quando uma entrega entra em retry ou é reprocessada?
- **RFC-Q-06** — Como vincular o usuário ao cliente e assegurar que o papel ADMIN seja concedido apenas por fluxo autorizado?
- **RFC-Q-08** — Que política de destinos externos e proteção de credenciais deve ser aprovada na revisão de Segurança?
- **RFC-Q-10** — Quais prazos de retenção se aplicam à Outbox, à DLQ e ao histórico?
- **RFC-Q-12** — Como tratar payload excessivo na fronteira transacional?

## Documentos relacionados

- [Transcrição da reunião](../TRANSCRICAO.md) · [Tracker](TRACKER.md) · [PRD](PRD.md) · [FDD](FDD.md).
