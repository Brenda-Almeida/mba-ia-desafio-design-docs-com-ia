# Da Reunião ao Documento — Design Docs Gerados por IA

## Sobre o desafio

O desafio consiste em transformar a [transcrição de uma reunião técnica](TRANSCRICAO.md) e o código de um sistema de gestão de pedidos (OMS) em um pacote de design docs que permita à equipe revisar a proposta e iniciar a implementação. O enunciado original, incluindo o requisito 6 e os critérios de aceite, está preservado no histórico e pode ser consultado com `git show e7f6311:README.md`.

O Sistema de Webhooks de Notificação de Pedidos foi proposto para que clientes recebam mudanças de status via HTTP sem consultar periodicamente a API. A arquitetura prevê o registro do evento na mesma transação da mudança de status e o envio assíncrono por um worker, mantendo a operação de pedidos independente da disponibilidade do destinatário.

## Como navegar a entrega

| Ordem | Artefato | Finalidade | Localização |
| --- | --- | --- | --- |
| 1 | PRD | Problema, público, escopo, requisitos e métricas de produto | [docs/PRD.md](docs/PRD.md) |
| 2 | RFC | Proposta arquitetural para revisão, alternativas e questões em aberto | [docs/RFC.md](docs/RFC.md) |
| 3 | ADRs | Sete registros de decisão com contexto, alternativas e consequências | [docs/adrs/](docs/adrs/) |
| 4 | FDD | Fluxos, contratos, erros, integrações e critérios técnicos | [docs/FDD.md](docs/FDD.md) |
| 5 | Tracker | Matriz de fontes e inventário das lacunas de evidência | [docs/TRACKER.md](docs/TRACKER.md) |

Consulte a [transcrição](TRANSCRICAO.md) para conferir as discussões e o [índice dos relatórios técnicos](docs/reports/README-2026-11-11_20-40-00.md) para localizar o diagnóstico do sistema existente.

## Ferramentas de IA utilizadas

| Ferramenta | Etapas e contribuição | Como os resultados foram revisados |
| --- | --- | --- |
| Codex CLI, em interface de terminal | Auditoria e correção de ADRs, produção e revisão da RFC, elaboração e auditoria do FDD e do PRD, inventário de fontes no Tracker e consolidação deste README. Leu as fontes, redigiu Markdown e verificou critérios e referências. | As auditorias cruzaram o conteúdo com a transcrição, o código e os critérios aplicáveis. O usuário orientou as correções dos ADRs, confirmou a autoria a registrar na RFC e solicitou a redução de repetições. |

A identificação da ferramenta e dos prompts vem dos registros locais das sessões de 16 a 18/11/2026. Os horários nesta página identificam essas sessões. Os registros integrais são locais e os resultados podem ser confrontados com os artefatos vinculados.

## Workflow adotado

1. **Leitura da reunião e investigação do código.** A transcrição orientou o levantamento de requisitos, decisões, descartes e adiamentos. A investigação mapeou, entre outros pontos, a transação de `changeStatus` no [serviço de pedidos](src/modules/orders/order.service.ts), os modelos e o provedor MySQL no [schema Prisma](prisma/schema.prisma) e o [middleware de autenticação](src/middlewares/auth.middleware.ts).
2. **Relatórios técnicos.** O [manifesto de 11/11](docs/reports/MANIFEST.md) registra a análise estática da arquitetura, das dependências e dos componentes, seguida de síntese e verificação das referências. O registro confirma a execução da análise e as checagens dos relatórios.
3. **ADRs e RFC.** Os ADRs já existiam quando foram auditados e corrigidos em 16/11. A RFC foi elaborada em seguida, consolidando o desenho com outbox, worker, assinatura e recuperação de falhas. Ela vincula os ADRs e preserva as alternativas descartadas e os pontos ainda em discussão. [ADRs](docs/adrs/) · [RFC](docs/RFC.md)
4. **FDD e PRD.** Em 17/11, o FDD desdobrou a proposta em fluxos, contratos e pontos de integração. Em 18/11, o PRD consolidou os resultados esperados para o cliente, o escopo e os critérios de aceite, com referências às decisões e ao desenho técnico. [FDD](docs/FDD.md) · [PRD](docs/PRD.md)
5. **Tracker e revisão.** O Tracker foi construído em 18/11 a partir dos quatro conjuntos documentais. Houve auditorias de ADRs, RFC, FDD e PRD, além da conferência das fontes no Tracker. A revisão da proposta pela equipe, a implementação, os testes da feature e a homologação constam como encaminhamentos ou planos. [Tracker](docs/TRACKER.md) · [Encaminhamento da RFC](docs/RFC.md#8-revisão-e-encaminhamento)

A sequência da RFC em diante é sustentada pelos registros de sessão. O histórico Git disponível preserva o enunciado e a remoção da antiga parte em HTML no commit `f4354f9`. A investigação usou também a área de trabalho, já que o histórico de commits não registra a sequência de elaboração dos design docs.

## Prompts customizados

### 1. Elaboração da RFC · 16/11, 20h16

Resumo fiel do prompt enviado ao Codex CLI para transformar a transcrição e as decisões registradas em uma proposta para revisão arquitetural.

```text
Atue como um Staff Software Engineer / Software Architect Sênior, especialista em:- Arquitetura de software e sistemas distribuídos.- Documentação técnica e processos de RFC (Request for Comments).

- Architecture Decision Records (ADRs).
- Design de sistemas assíncronos e orientados a eventos.
- Comunicação técnica e revisão de propostas arquiteturais.

Sua responsabilidade é elaborar uma RFC profissional, concisa, tecnicamente fundamentada e pronta para revisão pela equipe de engenharia, utilizando exclusivamente as informações disponíveis no repositório.

A RFC não é um documento de implementação. Seu objetivo é comunicar a proposta arquitetural, apresentar as decisões e seus trade-offs, registrar alternativas descartadas e explicitar questões ainda não resolvidas.

———

## 1. Objetivo

Produzir o documento docs/RFC.md, em Markdown, consolidando a proposta técnica da feature discutida na reunião registrada em TRANSCRICAO.md.

O documento deve:

1. Representar fielmente as discussões da reunião.
2. Consolidar as decisões arquiteturais documentadas nos ADRs.
3. Incorporar o contexto técnico relevante dos relatórios existentes.
4. Apresentar a solução proposta em nível arquitetural.
5. Documentar alternativas descartadas e suas justificativas.
6. Registrar questões em aberto para discussão assíncrona.
7. Referenciar os ADRs relacionados por links relativos válidos.
8. Estar pronto para ser submetido à revisão da equipe.

Resultado esperado: uma RFC de aproximadamente 2 a 4 páginas, com linguagem técnica, objetiva e profissional.

———

## 2. Fontes de informação

Antes de escrever qualquer conteúdo, explore o repositório e analise as seguintes fontes.

### 2.1. Fonte primária — Transcrição da reunião

Localize e leia integralmente o arquivo TRANSCRICAO.md.

Extraia e classifique:

- Problema de negócio ou técnico apresentado.
- Objetivos e necessidades da feature.
- Requisitos funcionais e não funcionais mencionados.
- Decisões efetivamente tomadas.
- Propostas arquiteturais discutidas.
- Alternativas consideradas e descartadas.
- Motivos e trade-offs dos descartes.
- Riscos e preocupações levantados.
- Dúvidas não respondidas.
- Questões adiadas para discussão futura.
- Participantes identificáveis da reunião.

Regra crítica: diferencie explicitamente uma decisão confirmada, uma sugestão, uma hipótese e uma questão ainda não resolvida.

Não transforme sugestões ou hipóteses em decisões definitivas.

### 2.2. Fonte de decisões — ADRs

Leia todos os arquivos de docs/adrs/.

Identifique:

- Decisões arquiteturais formalizadas.
- Contexto e motivação de cada decisão.
- Alternativas avaliadas.
- Consequências positivas e negativas.
- Trade-offs assumidos.
- Relações e dependências entre decisões.

Considere especialmente, quando documentados:

- Transactional Outbox no MySQL.
- Retry com backoff e DLQ.
- Assinatura HMAC-SHA256 com secret por endpoint.
- Entrega at-least-once com X-Event-Id.
- Worker em processo separado com polling.
- Reutilização de padrões existentes no projeto.

Não assuma que todas essas decisões estão aprovadas ou presentes. Confirme o conteúdo e o status real dos ADRs.

### 2.3. Fonte complementar — Relatórios técnicos

Explore a pasta reports/ e identifique os relatórios relevantes para a feature.

Utilize essas informações para compreender:

- Arquitetura atual do sistema.
- Componentes e tecnologias existentes.
- Restrições técnicas.
- Integrações e dependências.
- Padrões arquiteturais já adotados.
- Riscos e limitações identificados.
- Viabilidade técnica das decisões propostas.

Não reproduza integralmente os relatórios.

Extraia apenas informações necessárias para fundamentar a RFC.

### 2.4. RFC existente

Verifique se existe um arquivo rfc.md ou RFC.md na raiz do projeto, bem como docs/RFC.md.

Caso exista conteúdo anterior:

- Leia antes de escrever.
- Preserve informações válidas e relevantes.
- Reorganize o conteúdo para atender ao formato solicitado.
- Elimine redundâncias e informações desatualizadas.
- Não descarte informações relevantes sem justificativa.
```

**Resultado.** A RFC foi produzida com arquitetura, alternativas, questões abertas e links para os ADRs. A auditoria seguinte apontou ajustes de estrutura, metadados e nível de detalhe. [RFC](docs/RFC.md)

### 2. Auditoria da RFC · 17/11, 11h05

Resumo fiel do prompt enviado ao Codex CLI para conferir a RFC contra as fontes e os critérios de aceite.

```text
Atue como um Engenheiro de Software Sênior especializado em Arquitetura de Software e revisão de documentação técnica.
Analise o arquivo docs/RFC.md e verifique se ele atende integralmente aos requisitos abaixo. Utilize TRANSCRICAO.md, os ADRs em docs/adrs/ e os relatórios em reports/ como fontes de validação.
## 1. Requisitos obrigatórios:
A RFC deve operar em nível arquitetural, apresentando a abordagem escolhida, as alternativas discutidas e as questões deixadas em aberto. Deve ser um documento conciso, equivalente a 2–4 páginas, sem antecipar detalhes de implementação que pertencem ao FDD.
Verifique se contém todas as seções obrigatórias:
- Metadados: autor, status, data e revisores (participantes da reunião).
- Resumo executivo (TL;DR): síntese da proposta.
- Contexto e problema: contextualização e motivação da solução.
- Proposta técnica: visão geral da arquitetura, sem detalhamento de implementação.
- Alternativas consideradas: pelo menos duas alternativas reais discutidas e descartadas na reunião, cada uma com o trade-off que motivou o descarte.
- Questões em aberto: pelo menos dois pontos levantados na reunião que permaneceram sem decisão ou foram adiados.
- Impacto e riscos: consequências e riscos relevantes da proposta.- Decisões relacionadas: links válidos para os ADRs correspondentes.
## 2. Critérios de avaliação
- Valide o conteúdo das seções, não apenas a existência dos títulos.
- Confirme na transcrição se as alternativas foram realmente descartadas e se as questões permaneceram em aberto.
- Verifique se os revisores correspondem aos participantes da reunião.
- Verifique a consistência da proposta com os ADRs e os relatórios existentes.
```

**Resultado.** Na versão auditada, quatro dos oito itens passaram, três falharam e a extensão ficou sem comprovação. O usuário pediu as correções e a separação entre pendências da reunião e lacunas da análise. [RFC](docs/RFC.md)

## Iterações e ajustes

Os registros sustentam três ciclos principais com alterações concretas.

| Ciclo | Artefato | Problema e detecção | Ajuste realizado | Resultado e evidência |
| --- | --- | --- | --- | --- |
| 1 · 16/11 | ADRs | A auditoria encontrou 47 referências relativas quebradas e uma divergência entre a expressão “5 tentativas” e os cinco intervalos da reunião. | Corrigiu os caminhos e marcou um envio inicial mais cinco retentativas como interpretação documental da sequência. | A nova checagem encontrou 56 de 56 destinos existentes. A interpretação ficou explícita e pendente de validação. Sessão das 19h47. [ADR-004](docs/adrs/ADR-004-entrega-http-at-least-once-com-event-id.md) · [ADR-007](docs/adrs/ADR-007-retry-com-backoff-e-dlq.md) |
| 2 · 17/11 | RFC | A auditoria detectou falta de TL;DR, metadados incompletos e excesso de detalhes de implementação. | Incluiu o resumo, registrou Larissa como autora após confirmação do usuário, identificou os revisores e condensou os detalhes. Também agrupou os ADRs e distinguiu pendências da reunião de lacunas da análise. | A versão revisada apresenta as seções e metadados adicionados, com links locais conferidos. Sessão das 11h05. [RFC](docs/RFC.md) |
| 3 · 17/11 | RFC | A revisão de extensão identificou repetições sobre latência, retry e ordenação. Durante o ajuste, Arial 12 ainda produzia uma quinta página. | Condensou repetições e texto de apoio, preservando decisões, alternativas, trade-offs e questões em aberto. | O registro final informa redução de 2.060 para 1.426 palavras e quatro páginas em A4, com Arial 11 e 12, margens de 2 cm e entrelinha 1,2. Sessão das 11h23. [RFC](docs/RFC.md) |

Em 24/11, a revisão de consistência alinhou a política de retry ao total registrado no fechamento da reunião. O desenho atual prevê um envio inicial, até quatro retentativas e DLQ ao esgotar o ciclo. O intervalo de 12 h permanece registrado para revisão. [ADR-007](docs/adrs/ADR-007-retry-com-backoff-e-dlq.md)

## Validação e controle de qualidade

| Área | Verificação documentada |
| --- | --- |
| Relatórios | O manifesto registra a conferência de seções, links locais e 255 referências com existência de arquivo e limites de linha verificados. [Manifesto](docs/reports/MANIFEST.md) |
| ADRs | A auditoria conferiu os sete arquivos, a variante MADR, alternativas, consequências, cobertura das decisões e referências concretas a módulos do OMS. Os links foram verificados novamente após a correção. [ADRs](docs/adrs/) |
| RFC | As revisões confrontaram alternativas como HTTP síncrono e Redis Streams e pendências como rate limiting e retenção com a reunião. Houve conferência de links e verificação de extensão nas configurações indicadas acima. [RFC](docs/RFC.md) |
| FDD | Foram conferidos fluxos, contratos HTTP, matriz `WEBHOOK_*`, observabilidade, coerência com RFC e ADRs e integração com arquivos existentes. A auditoria incorporada ao FDD registra sete operações administrativas e 18 arquivos reais de integração. [FDD](docs/FDD.md#163-auditoria-de-aceitação-documental) |
| PRD | A auditoria de 18/11 conferiu as 12 seções, os 12 requisitos funcionais discutidos, a meta de latência inferior a 10 s, os itens fora do escopo e os riscos com mitigação. Percentil, janela e carga da meta seguiram pendentes. [PRD](docs/PRD.md) |
| Tracker | O inventário conferiu horários e falantes na transcrição e caminhos de código. Das 292 linhas, 221 têm fonte `TRANSCRICAO` (75,68%) e 71 têm fonte `CODIGO`, em 32 arquivos. A cobertura ficou em 51,41% (292 de 568 itens), abaixo do mínimo de 80%. [Tracker](docs/TRACKER.md) |

## Rastreabilidade e uso responsável da IA

- **Requisitos discutidos.** A evidência vem da fala identificada por horário e participante. A transcrição sustenta, por exemplo, notificações outbound e replay restrito a ADMIN. [Transcrição](TRANSCRICAO.md) · [Tracker](docs/TRACKER.md)
- **Decisões registradas.** Os ADRs dão contexto e consequências às escolhas, com status *Proposto*. A RFC registra a Outbox no MySQL e o polling como escolhas da reunião e indica o que ainda depende de revisão. [ADRs](docs/adrs/) · [RFC](docs/RFC.md)
- **Estado confirmado no código.** O serviço de pedidos comprova a fronteira transacional existente e o schema comprova o uso de MySQL via Prisma. O FDD identifica os pontos em que a implementação futura deve integrar-se a essas estruturas. [Serviço de pedidos](src/modules/orders/order.service.ts) · [Schema Prisma](prisma/schema.prisma) · [FDD](docs/FDD.md)
- **Propostas técnicas e pendências.** Detalhes de contratos e concorrência foram elaborados no FDD. A contagem exata de tentativas e a autorização por cliente são exemplos de pontos ainda sujeitos a validação. O Tracker mantém itens sem fonte primária no inventário de lacunas, inclusive no denominador da cobertura. [FDD](docs/FDD.md) · [Lacunas do Tracker](docs/TRACKER.md#itens-sem-evidência-primária-suficiente)

## Resultados e limitações

O pacote reúne PRD, RFC, FDD, sete ADRs, Tracker e relatórios técnicos. Os prompts dirigidos permitiram examinar a proposta em diferentes níveis e transformar problemas encontrados nas auditorias em correções verificáveis. O inventário de lacunas mostra onde o desenho precisa de revisão e validação adicionais, especialmente nos detalhes de implementação do FDD.

A cobertura do Tracker permanece abaixo do critério do desafio. A análise foi documental e estática. Seguem pendentes a revisão da proposta pela equipe, a validação de segurança e a comprovação em execução dos contratos, da resiliência e da meta de latência.

## Checklist de conformidade do README

| Requisito | Status | Evidência |
| --- | --- | --- |
| Contém todas as seções obrigatórias do requisito 6 | PASSOU | Seis seções conferidas contra `git show e7f6311:README.md`. |
| Lista pelo menos uma ferramenta de IA efetivamente utilizada | PASSOU | Codex CLI identificado nos registros locais das sessões de produção. |
| Apresenta pelo menos dois prompts customizados em blocos de código | PASSOU | Dois resumos fiéis dos prompts de elaboração e auditoria da RFC. |
| Descreve pelo menos duas iterações ou ajustes concretos | PASSOU | Três ciclos com problemas detectados, alterações realizadas e resultados registrados. |

**Veredito do README: APROVADO.** O Tracker permanece abaixo do critério de cobertura do pacote.
