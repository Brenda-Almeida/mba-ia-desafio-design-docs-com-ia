# PRD — Sistema de Webhooks de Notificação de Pedidos

**Status** · Para revisão de Produto, Pedidos, Plataforma e Segurança.

## 1. Resumo e contexto da feature

A plataforma enviará notificações de mudança de status de pedidos aos sistemas B2B inscritos. Cada cliente poderá cadastrar destinos HTTPS e escolher os status que deseja receber. O consumidor poderá validar a origem e a integridade do evento, identificar duplicatas e consultar o pedido pela API existente quando precisar de mais detalhes. A plataforma manterá histórico de tentativas e oferecerá recuperação administrativa de falhas definitivas.

Os horários citados neste PRD apontam para a [transcrição da reunião](../TRANSCRICAO.md). Decisões da reunião têm evidência verbal. A [RFC](RFC.md), os sete ADRs e o [FDD](FDD.md) estão propostos. Comportamentos adicionais desses documentos estão identificados como derivados e sujeitos à revisão.

## 2. Problema e motivação

Atlas Comercial, MaxDistribuição e Nova Cargo consultam periodicamente `GET /orders` para descobrir mudanças. Segundo Marcos, isso torna a integração lenta e cara para esses clientes e cria necessidade de atualizações manuais quando a informação atrasa. A Atlas sinalizou possível migração para um concorrente se a necessidade ficasse sem atendimento até o fim do trimestre. A expectativa comunicada pelos clientes foi receber a notificação em menos de dez segundos. [Reunião, 09:00–09:02](../TRANSCRICAO.md)

O fluxo de pedidos já atualiza status, histórico e estoque aplicável em conjunto. O envio de notificações deve preservar essas regras e permitir que a mudança seja confirmada mesmo quando o destinatário estiver lento ou indisponível. Os relatórios descrevem a plataforma atual e servem de referência para a regressão. [Reunião, 09:03–09:08 e 09:40–09:41](../TRANSCRICAO.md) · [Arquitetura atual](reports/architectural-analyzer/architectural-report-2026-11-11_20-40-00.md) · [Pedidos](reports/component-deep-analyzer/component-analysis-orders-2026-11-11_20-40-00.md)

## 3. Público-alvo e cenários de uso

| Público | Necessidade, interação e benefício |
|---|---|
| Usuários autenticados que representam clientes B2B | Configuram destinos e filtros pela API, rotacionam segredos e consultam o histórico para diagnosticar entregas. |
| Sistemas consumidores da Atlas Comercial, MaxDistribuição e Nova Cargo | Recebem mudanças de interesse, verificam a assinatura e deduplicam eventos. Podem consultar o pedido para obter detalhes sob demanda. |
| Administração da plataforma | Investiga falhas definitivas e solicita reprocessamento manual, com registro de quem tomou a ação. |

- **Mudança de interesse.** Um cliente inscreve um destino para `SHIPPED` e `DELIVERED`. Ao confirmar a transição para `SHIPPED`, a plataforma envia ao destino o estado do pedido no momento da mudança.
- **Mudança sem inscritos.** A transição válida segue o fluxo normal do pedido, sem gerar entrega quando nenhum cadastro elegível está interessado.
- **Indisponibilidade e recuperação.** O pedido muda de status enquanto o destino está fora do ar. A entrega pode se recuperar automaticamente. Caso chegue à fila de falhas definitivas (DLQ), um administrador pode solicitar novo envio.
- **Rotação e duplicata.** O consumidor migra para o novo segredo durante a janela de coexistência e usa o identificador do evento para evitar reaplicar efeitos.

[Reunião, 09:15–09:26, 09:31–09:36, 09:43–09:45 e 09:51–09:52](../TRANSCRICAO.md)

## 4. Objetivos e métricas de sucesso

| ID | Objetivo | Métrica | Meta quantitativa | Forma de medição |
|---|---|---|---|---|
| OBJ-001 | Informar rapidamente mudanças de interesse | Latência da confirmação da mudança até a resposta de sucesso da primeira tentativa | **< 10 s**, expectativa relatada na reunião e registrada na RFC. Percentil, volume e janela de avaliação pendentes de validação. | No piloto, correlacionar a confirmação do pedido com a resposta do destino. Publicar distribuição, volume, falhas na primeira tentativa e resultados de retry, replay e bloqueio de ordem em separado. Definir antes do piloto a carga e as condições consideradas saudáveis. |

A meta expressa a expectativa dos clientes. O intervalo de consulta de pendências e o timeout por chamada têm finalidades próprias. A medição depende da instrumentação e da homologação com a carga acordada. [Reunião, 09:01–09:10 e 09:42](../TRANSCRICAO.md) · [RFC, §§ 1 e 5](RFC.md) · [FDD, §§ 10 e 14](FDD.md)

## 5. Escopo

### Dentro do escopo

- Gestão de destinos por cliente via API autenticada, com URL HTTPS, estado ativo, filtros e rotação de segredo.
- Notificação exclusivamente de saída para mudanças de status, com seleção na origem, retrato do pedido no momento do evento e envio assíncrono assinado.
- Entrega com identidade estável, recuperação automática limitada, registro de falhas definitivas e reprocessamento manual auditado por ADMIN.
- Histórico de entregas via API e orientação de integração no portal de desenvolvedores, incluindo validação de assinatura e deduplicação. [Reunião, 09:26, 09:40 e 09:48](../TRANSCRICAO.md)

### Fora de escopo

| Item | Motivo e decisão | Evidência |
|---|---|---|
| Avisos por e-mail sobre falhas | Adiados para possível fase posterior, após medir o impacto. | [09:37–09:38 e 09:48](../TRANSCRICAO.md) |
| Dashboard visual | A fase entrega operações via API. O painel é projeto separado do time de frontend. | [09:39–09:40 e 09:48](../TRANSCRICAO.md) |
| Limitação da taxa de envios por destino | Adiada até observar se rajadas de notificações causam problemas. | [09:38–09:39 e 09:48](../TRANSCRICAO.md) |
| Rotina de arquivamento de entregas concluídas | Excluída da fase. Aproximadamente 30 dias foi uma sugestão informal, com política e justificativa do prazo em aberto. | [09:07–09:08](../TRANSCRICAO.md) |
| Escala com múltiplos workers | Adiada para uma etapa em que seja necessário reavaliar coordenação e ordenação. | [09:12–09:13](../TRANSCRICAO.md) |

A reunião delimitou a integração a notificações de saída e excluiu os itens do pedido do conteúdo para manter a mensagem enxuta. A API de pedidos segue disponível para consulta detalhada. [Reunião, 09:02–09:03 e 09:43–09:44](../TRANSCRICAO.md)

## 6. Requisitos funcionais

### Confirmados na reunião

Os doze requisitos abaixo compõem a primeira fase. A reunião definiu o escopo dessa fase, sem graduar a prioridade individual das operações.

| ID | Requisito | Comportamento e regras de negócio | Fonte |
|---|---|---|---|
| RF-001 | Cadastrar destino | Usuário autenticado de qualquer papel pode cadastrar webhook, indicando explicitamente o cliente e informando URL HTTPS e status de interesse. A plataforma gera um segredo exclusivo e o devolve na criação. O identificador do cliente vem da requisição, conforme correção feita na reunião. | [09:21–09:23 e 09:31–09:37](../TRANSCRICAO.md) |
| RF-002 | Editar destino | Usuário autenticado pode alterar a configuração do destino via API. O cadastro mantém URL, estado ativo e filtro por status. O efeito da edição nas entregas já pendentes requer validação. | [09:21 e 09:33–09:37](../TRANSCRICAO.md) |
| RF-003 | Remover destino | Usuário autenticado pode remover um cadastro via API. O efeito sobre tentativas pendentes e histórico requer validação. | [09:33–09:37](../TRANSCRICAO.md) |
| RF-004 | Listar destinos | Usuário autenticado pode listar as configurações de um cliente explicitamente identificado na requisição. | [09:32–09:37](../TRANSCRICAO.md) |
| RF-005 | Gerar evento para inscritos | Na mudança de status, selecionar os cadastros ativos do cliente interessados no status de destino. Se houver interessados, registrar o evento com os dados daquele momento. Sem interessados, a mudança válida segue sem registro de entrega. | [09:06, 09:21, 09:33–09:34, 09:40–09:41 e 09:51–09:52](../TRANSCRICAO.md) |
| RF-006 | Enviar notificação de saída | Após a confirmação do pedido, enviar ao destino mensagem JSON com tipo `order.status_changed`, evento, instante da mudança, identificador e número do pedido, status anterior e novo, cliente e total em centavos. Os itens ficam disponíveis na API de pedidos. Informar destino cadastrado, instante do envio e tipo de conteúdo nos headers `X-Webhook-Id`, `X-Timestamp` e `Content-Type`. | [09:02–09:06 e 09:43–09:45](../TRANSCRICAO.md) |
| RF-007 | Identificar o evento | Atribuir UUID único por evento e enviá-lo em `X-Event-Id` para o consumidor reconhecer duplicatas da mesma mudança. | [09:24–09:26 e 09:44](../TRANSCRICAO.md) |
| RF-008 | Rotacionar segredo | Permitir que o consumidor solicite novo segredo via API. O anterior continua válido em paralelo por 24 horas e depois expira. | [09:21–09:22 e 09:48](../TRANSCRICAO.md) |
| RF-009 | Retentar entrega | Em caso de indisponibilidade temporária ou timeout, tentar novamente automaticamente com espera crescente e limite por ciclo. O ciclo tem até cinco tentativas totais, incluindo o envio inicial. As quatro retentativas usam esperas de 1 min, 5 min, 30 min e 2 h. A divergência sobre o intervalo de 12 h está registrada na seção 8. | [09:14–09:17, 09:42 e 09:48](../TRANSCRICAO.md) |
| RF-010 | Registrar falha definitiva | Ao esgotar as tentativas automáticas, manter a falha em DLQ separada das pendências, com conteúdo original, motivo e data/hora para diagnóstico e recuperação. | [09:15–09:19 e 09:48](../TRANSCRICAO.md) |
| RF-011 | Consultar histórico | Disponibilizar histórico por destino, com resultado, conteúdo enviado, resposta do destino quando disponível e tempo de resposta. A reunião citou os últimos 100 como exemplo. | [09:34](../TRANSCRICAO.md) |
| RF-012 | Reprocessar falha definitiva | Somente ADMIN pode solicitar o reprocessamento de um registro da DLQ. A entrega volta a ficar pendente e o autor da ação fica registrado para auditoria. | [09:18–09:19 e 09:35–09:36](../TRANSCRICAO.md) |

### Derivados de ADRs e FDD, sujeitos à revisão

| ID | Requisito | Comportamento proposto | Fonte |
|---|---|---|---|
| RF-013 | Acompanhar entregas por destino | Destinos inscritos na mesma mudança compartilham o ID do evento e mantêm resultados independentes. Retry e replay preservam a identidade e o conteúdo originais. | [ADR-003](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) · [ADR-004](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) |
| RF-014 | Classificar respostas | Uma resposta 2xx confirma sucesso. Falha transitória permite retry. Rejeição permanente do destino encerra o ciclo na DLQ de imediato. | [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md) · [FDD, § 5.3](FDD.md) |
| RF-015 | Paginar histórico | Consultar tentativas mais recentes primeiro e navegar em páginas de até 100 registros. | [FDD, § 6.7](FDD.md) |
| RF-016 | Evitar replay administrativo duplicado | Uma falha elegível abre um novo ciclo com histórico preservado. Uma segunda solicitação concorrente para a mesma falha recebe conflito. A aceitação do replay indica agendamento. | [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md) · [FDD, §§ 5.4 e 6.8](FDD.md) |

## 7. Requisitos não funcionais

| ID | Atributo | Condição verificável | Origem |
|---|---|---|---|
| RNF-001 | Consistência | Confirmar a mudança elegível junto com o registro durável do evento. Falha nesse registro reverte status, histórico e estoque aplicável. O envio externo ocorre após a confirmação e falhas externas preservam a mudança já confirmada. | [Reunião, 09:03–09:08 e 09:40–09:41](../TRANSCRICAO.md) |
| RNF-002 | Segurança de transporte | Exigir HTTPS e rejeitar cadastro ou edição com URL HTTP com erro de validação. | [Reunião, 09:23](../TRANSCRICAO.md) · [FDD, §§ 6 e 9](FDD.md) |
| RNF-003 | Autenticidade e integridade | Assinar o corpo transmitido com HMAC-SHA256 e segredo por destino em `X-Signature`. Uma alteração no corpo invalida a assinatura. | [Reunião, 09:19–09:22 e 09:44](../TRANSCRICAO.md) |
| RNF-004 | Limite de mensagem | Rejeitar com erro conteúdo de notificação acima de 64 KB e preservar a integridade do conteúdo, sem truncamento. Unidade exata e resposta da operação de status a confirmar. | [Reunião, 09:23–09:24](../TRANSCRICAO.md) · [FDD, § 5.1](FDD.md) |
| RNF-005 | Limite por chamada | Tratar ausência de resposta após 10 s como falha sujeita à política de retry. | [Reunião, 09:42](../TRANSCRICAO.md) |
| RNF-006 | Independência operacional | Reiniciar a API sem interromper o processo de entregas. Iniciar com um worker separado e consulta de pendências em intervalos de dois segundos. | [Reunião, 09:09–09:13](../TRANSCRICAO.md) · [ADR-006](adrs/ADR-006-worker-separado-com-polling.md) |
| RNF-007 | Semântica de entrega | Admitir duplicatas com o mesmo ID no modelo at-least-once. A ordem prevista é por pedido com um worker, com comportamento durante retry em aberto. A recuperação tem limite e pode terminar em falha definitiva. | [Reunião, 09:12–09:17 e 09:24–09:26](../TRANSCRICAO.md) · [RFC, §§ 3 e 5](RFC.md) |
| RNF-008 | Compatibilidade | Preservar regras, respostas e erros do fluxo de Pedidos. Seguir as convenções existentes de validação, logging e erros, com prefixo `WEBHOOK_` para erros próprios da feature. | [Reunião, 09:27–09:30 e 09:40–09:41](../TRANSCRICAO.md) · [FDD, §§ 7 e 12](FDD.md) |
| RNF-009 | Proteção de credenciais | Restringir a exposição do segredo ao retorno de criação e rotação, protegê-lo em repouso e omiti-lo de consultas e logs. | Derivado do [ADR-005](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) e do [FDD, § 9](FDD.md). |
| RNF-010 | Observabilidade | Correlacionar a mudança do pedido com evento, tentativa, resultado e eventual replay. Medir atrasos, pendências e falhas definitivas sem incluir segredos na telemetria. | Derivado do [FDD, § 10](FDD.md). Coleta e alertas pendentes. |

## 8. Decisões e trade-offs principais

| Decisão registrada na reunião | Motivação e benefício | Consequência para produto e operação | Referência |
|---|---|---|---|
| Outbox transacional no MySQL | Vincula a intenção de entrega à mudança confirmada, com envio posterior. | A confirmação depende da escrita adicional no banco. O destinatário pode receber depois. | [ADR-003](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) |
| Retry limitado com backoff e DLQ | Dá tempo para o destino voltar e mantém evidências da falha. | Após o limite, a recuperação passa a depender de ação administrativa. | [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md) |
| HMAC-SHA256 e segredo por endpoint | Permite validar a mensagem e limita o impacto do vazamento de um segredo. | Cada consumidor precisa gerenciar a credencial e a janela de rotação. | [ADR-005](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) |
| At-least-once com `X-Event-Id` | Reduz a coordenação necessária entre produtor e destinatário. | O consumidor deduplica antes de aplicar efeitos. Replay tardio exige avaliar o risco de reaplicação. | [ADR-004](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) |
| Worker separado com polling de 2 s | Isola o ciclo de vida da entrega e reutiliza o banco existente. | Adiciona processo e consultas ao banco. Backlog e duração do envio afetam a latência. | [ADR-006](adrs/ADR-006-worker-separado-com-polling.md) |
| Reuso de MySQL, Prisma e padrões da API | Preserva convenções de integração e reduz a introdução de infraestrutura. | Banco e conexões passam a atender também a carga de notificações. Adaptações de erros e proteção de dados do webhook requerem revisão. | [ADR-001](adrs/ADR-001-mysql-como-banco-relacional.md) · [ADR-002](adrs/ADR-002-prisma-para-acesso-a-dados.md) · [FDD, §§ 7 e 12](FDD.md) |

### Divergências, propostas e pendências

| Tema | Evidência e decisão pendente |
|---|---|
| Contagem de tentativas | A reunião registra **total de cinco tentativas** e enumera cinco esperas. O ADR-007 e o FDD seguem o total anunciado, com um envio inicial, até quatro retentativas e as quatro primeiras esperas. O intervalo de 12 h permanece em revisão. [Reunião, 09:15–09:17 e 09:48](../TRANSCRICAO.md) · [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md) · [FDD, § 5.3](FDD.md) |
| Ordem e replay | A ordem por pedido discutida para um worker deixa em aberto o efeito das retentativas. ADRs e FDD propõem esperar a resolução da entrega anterior para o mesmo pedido e destino, com aumento de latência e possibilidade de replay tardio fora da ordem histórica. [Reunião, 09:12–09:14](../TRANSCRICAO.md) · [ADR-006](adrs/ADR-006-worker-separado-com-polling.md) · [FDD, §§ 5.3–5.4](FDD.md) |
| Identidade e autorização | A reunião corrigiu a sugestão inicial de obter o cliente pelo JWT. O identificador explícito define o alvo da operação. O vínculo de autorização por cliente segue pendente e o cadastro público com papel ADMIN descrito no relatório bloqueia a liberação segura do replay até correção. [Reunião, 09:31–09:37](../TRANSCRICAO.md) · [RFC, § 5](RFC.md) · [Relatório de autenticação](reports/component-deep-analyzer/component-analysis-auth-2026-11-11_20-40-00.md) |
| Assinatura e segurança | A reunião sugere uso de `X-Timestamp` para detectar replay. O contrato proposto autentica somente o corpo, portanto o timestamp do header é informativo e a prevenção de reaplicação depende da deduplicação. Assinatura secundária durante a rotação, retenção de deduplicação de sete dias, recuperação de segredo e política de destinos permitidos seguem sujeitos à revisão. [Reunião, 09:44](../TRANSCRICAO.md) · [ADR-004](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) · [ADR-005](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) · [FDD, § 9](FDD.md) |
| Ciclo de vida do destino e dados | Validar o destino de entregas pendentes após edição, desativação e remoção, além da consulta histórica e da política de retenção. O FDD propõe usar a URL vigente e registrar pendências de destinos desativados ou removidos na DLQ. [RFC, § 5](RFC.md) · [FDD, §§ 6 e 16](FDD.md) |
| Conteúdo acima do limite | Confirmar a interpretação de 64 KB como 65.536 bytes e a proposta de reverter a mudança de status quando o evento exceder o limite. [Reunião, 09:23–09:24](../TRANSCRICAO.md) · [FDD, §§ 5.1 e 16](FDD.md) |
| Operação e planejamento | Aprovar RFC, ADRs e FDD. Definir carga de homologação, coleta de métricas, alertas e responsáveis. Três sprints são estimativa verbal e a previsão para a Atlas ficou a confirmar por Marcos. [Reunião, 09:45–09:50](../TRANSCRICAO.md) · [RFC, §§ 5 e 8](RFC.md) |

## 9. Dependências

| Dependência | Efeito na entrega |
|---|---|
| Pedidos, Clientes e persistência MySQL/Prisma | Pedidos fornece transições válidas e o contexto para as notificações. Clientes identifica a conta elegível. A persistência compartilhada precisa suportar a carga adicional de escrita e leitura. [Pedidos](reports/component-deep-analyzer/component-analysis-orders-2026-11-11_20-40-00.md) · [Persistência](reports/component-deep-analyzer/component-analysis-persistence-2026-11-11_20-40-00.md) |
| Autenticação e API REST | O JWT identifica usuário e papel. A autorização por cliente e a atribuição segura de ADMIN são pré-condições para liberação B2B e replay, conforme seção 8. [Autenticação](reports/component-deep-analyzer/component-analysis-auth-2026-11-11_20-40-00.md) · [Camada HTTP](reports/component-deep-analyzer/component-analysis-http-2026-11-11_20-40-00.md) |
| Plataforma e Segurança | Exige operação do worker, saída HTTPS/DNS, gestão de chaves, supervisão, coleta de métricas e capacidade do banco. Reservar pelo menos dois dias úteis para a revisão de Segurança antes do deploy, conforme solicitado na reunião. [Reunião, 09:46–09:49](../TRANSCRICAO.md) · [FDD, §§ 9–11](FDD.md) |
| Consumidores e portal de desenvolvedores | Cada consumidor precisa de um destino HTTPS, validação da assinatura, deduplicação e retorno compatível com o contrato. Produto publica as instruções de integração. [Reunião, 09:19–09:26 e 09:40](../TRANSCRICAO.md) · [FDD, § 6.9](FDD.md) |
| Atualização do fluxo produtor | A cobertura começa após a atualização completa das instâncias que alteram status. Durante a implantação, instâncias antigas ainda podem confirmar mudanças sem gerar evento. [FDD, § 11](FDD.md) |

## 10. Riscos e mitigação

Probabilidade e impacto são estimativas qualitativas desta análise na escala baixa, média e alta.

| ID | Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|---|
| R-001 | Mudança confirmada sem registro da notificação elegível | Média | Alto | Usar registro atômico e validar reversão em teste de integração. Confirmar a atualização de todo o fluxo produtor antes do piloto. [RFC, § 3](RFC.md) · [FDD, §§ 11 e 13](FDD.md) |
| R-002 | Duplicata ou replay tardio reaplica efeitos no consumidor | Alta | Alto | Preservar identidade, documentar deduplicação e validar com consumidor de teste. Avaliar a janela de deduplicação antes de replay tardio. [ADR-004](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) |
| R-003 | Destino indisponível esgota tentativas ou atrasa eventos seguintes | Alta | Alto | Aplicar timeout e retry limitado, disponibilizar histórico e DLQ, definir quem acompanha falhas e iniciar replay após diagnóstico. [ADR-007](adrs/ADR-007-retry-com-backoff-e-dlq.md) |
| R-004 | Exposição de dados de outro cliente, uso indevido de ADMIN ou acesso a destino interno | Média | Alto | Condicionar liberação à revisão de autorização, atribuição de papéis e política de destinos. Validar limites de acesso e proteção de credenciais. [RFC, § 5](RFC.md) · [FDD, § 9](FDD.md) |
| R-005 | Backlog ou disputa por capacidade no MySQL eleva a latência e afeta Pedidos | Média | Alto | Medir atraso, volume e desempenho de Pedidos sob carga. Acompanhar crescimento do histórico e acordar retenção e capacidade operacional. [RFC, § 6](RFC.md) · [FDD, §§ 10–11](FDD.md) |

## 11. Critérios de aceitação

Os critérios que incluem propostas declaram a condição de aprovação correspondente. Os demais descrevem o comportamento esperado para a primeira fase, a validar quando a feature estiver disponível.

| ID | Rastreabilidade | Dado / Quando / Então |
|---|---|---|
| CA-001 | RF-001, RF-004, RNF-002 | Dado um usuário autenticado com acesso ao cliente, quando cadastrar um destino HTTPS com filtros e listar os cadastros do cliente, então a criação devolve um segredo exclusivo e a lista inclui o cadastro. Quando cadastrar URL HTTP ou fizer a operação sem autenticação, então recebe erro e nenhum cadastro é criado. |
| CA-002 | RF-002, RF-003, RF-005, RNF-002 | Dado um cadastro ativo, quando alterar seu filtro, desativá-lo ou removê-lo, então novas mudanças usam a configuração atual, e destinos desativados ou removidos deixam de receber novas inscrições em eventos. Uma edição para HTTP é rejeitada e preserva a configuração anterior. |
| CA-003 | RF-005, RF-006, RF-007, RNF-008 | Dado um pedido com inscritos para o status de destino, quando a transição válida for confirmada e o pedido mudar novamente antes do envio, então os inscritos recebem o JSON e os headers do contrato, com o status e os dados da primeira mudança. Dada uma transição inválida, a API mantém o erro de negócio e nenhum evento é gerado. Dada uma mudança válida sem inscritos, ela é confirmada sem gerar entrega. |
| CA-004 | RF-005, RF-006, RNF-001, RNF-006 | Dada falha ao registrar uma entrega elegível, quando a mudança for solicitada, então status, histórico, estoque aplicável e registro do evento permanecem sem efeitos parciais. Dado um consumidor fora do ar, quando o registro for concluído, então a mudança é confirmada, a entrega é tentada em separado e reiniciar apenas a API não interrompe o processo de entregas. |
| CA-005 | RF-008, RNF-003, RNF-009 | Dado um segredo conhecido, quando o destino verificar a notificação, então a assinatura coincide com o corpo original e falha com o corpo alterado. Durante as 24 h após a rotação o segredo antigo ainda permite validação. Após a janela, apenas o novo permite. Para a proteção proposta, consultas e logs omitem as credenciais. |
| CA-006 | RF-009, RF-010, RNF-005 | Dado um destino sem resposta em 10 s, quando o ciclo prosseguir com falhas transitórias, então registra o timeout, agenda retentativas com esperas de 1 min, 5 min, 30 min e 2 h e, após a quinta falha transitória, registra o conteúdo original, o motivo e o instante na DLQ e encerra os envios automáticos do ciclo. |
| CA-007 | RF-007, RNF-007 | Dados dois status elegíveis do mesmo pedido em sequência e um único worker, quando as entregas forem bem-sucedidas sem retry, então o destino observa a ordem das mudanças. Quando receber uma duplicata, então o ID coincide e o consumidor de homologação aplica o efeito uma vez. Ordem durante retry depende da decisão da seção 8. |
| CA-008 | RF-011, RF-015 | Dado um destino com sucesso e falha registrados, quando o usuário autorizado consultar o histórico, então verá resultado, conteúdo, duração e resposta quando houver. Para a paginação proposta, consultar páginas consecutivas de um conjunto fixo retorna tentativas em ordem decrescente, sem omissões nem duplicatas. |
| CA-009 | RF-012, RF-016 | Dada uma falha elegível, quando ADMIN pedir replay, então a entrega fica pendente e registra o autor. Quando um usuário sem esse papel solicitar a ação, então ela é recusada e o estado permanece. Para o controle de concorrência proposto, duas solicitações simultâneas criam um único ciclo e retornam conflito na outra solicitação. |
| CA-010 | RF-013, RF-014 | Dado o desenho derivado aprovado e dois destinos inscritos, quando um retornar 2xx e outro apresentar falha transitória, então só o segundo é retentado, preservando o conteúdo e o ID compartilhado. Uma rejeição classificada como permanente encerra a entrega correspondente e aparece na DLQ. |
| CA-011 | RNF-004 | Dado um evento elegível acima do limite de 64 KB, quando a emissão for solicitada, então ocorre erro e nenhuma mensagem truncada é enviada. A fronteira em bytes e a resposta da mudança de status seguem a política aprovada na seção 8. |
| CA-012 | RF-011, RF-012, RNF-008, RNF-010, OBJ-001 | Dado um fluxo de homologação com instrumentação aprovada, quando houver mudança, entrega e replay, então é possível correlacionar seus registros sem expor segredos e aferir atrasos e falhas. Os erros próprios da feature usam o prefixo acordado. Em carga e janela acordadas, registrar a distribuição e confrontar a latência observada com a meta de < 10 s pelo critério aprovado previamente. |

## 12. Estratégia de testes e validação

| Estratégia | Cobertura |
|---|---|
| Funcional e de contrato | Verificar CRUD, filtros, rotação, lista e histórico, respostas de erro, formato JSON e headers outbound contra o contrato do FDD. CA-001 a CA-003, CA-005 e CA-008 a CA-012. |
| Integração e regressão de Pedidos | Simular falha de registro da notificação, confirmar rollback completo e executar a suíte de Pedidos para preservar transições, histórico, estoque e respostas. CA-003 e CA-004. |
| Ponta a ponta e resiliência | Usar destino controlado para testar sucesso, timeout, reinício, duplicatas, DLQ, replay e ordem. Usar relógio controlado para exercitar as quatro esperas e verificar o encaminhamento à DLQ após a quinta falha. CA-004 e CA-006 a CA-010. |
| Segurança e autorização | Verificar recusa de URL sem HTTPS, adulteração do corpo, rotação na fronteira de 24 h, ausência de credenciais em consultas e logs, isolamento entre clientes e atribuição de ADMIN antes da exposição externa. CA-001, CA-002, CA-005, CA-009 e CA-012. |
| Carga e homologação | Testar tamanho limite e carga representativa acordada. Medir latência de primeira entrega, backlog e impacto em Pedidos. Homologar assinatura, deduplicação, histórico e recuperação com um consumidor piloto e validar OBJ-001 antes da expansão. CA-003, CA-007, CA-008 e CA-011 a CA-012. |

[FDD, §§ 6, 9, 10, 13 e 14](FDD.md)

## Documentos relacionados

- Evidência primária e documentação complementar. [Transcrição](../TRANSCRICAO.md) · [RFC](RFC.md) · [FDD](FDD.md)
- Decisões arquiteturais. [ADR-001 MySQL](adrs/ADR-001-mysql-como-banco-relacional.md) · [ADR-002 Prisma](adrs/ADR-002-prisma-para-acesso-a-dados.md) · [ADR-003 Outbox](adrs/ADR-003-outbox-mysql-na-transacao-de-status.md) · [ADR-004 Entrega e identidade](adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) · [ADR-005 Assinatura e segredo](adrs/ADR-005-hmac-sha256-com-segredo-por-endpoint.md) · [ADR-006 Worker](adrs/ADR-006-worker-separado-com-polling.md) · [ADR-007 Retry e DLQ](adrs/ADR-007-retry-com-backoff-e-dlq.md)
- Diagnóstico do sistema atual. [Visão do projeto](reports/PROJECT-OVERVIEW-2026-11-11_20-40-00.md) · [Arquitetura](reports/architectural-analyzer/architectural-report-2026-11-11_20-40-00.md) · [Dependências](reports/dependency-auditor/dependencies-report-2026-11-11_20-40-00.md)
