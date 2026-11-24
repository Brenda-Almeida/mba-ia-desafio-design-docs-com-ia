# ADR-005: HMAC-SHA256 com segredo por endpoint

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: 2026-06-18.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: DATA e autenticação da integração externa.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Relacionado a:**

- [ADR-004: HTTP outbound at-least-once com X-Event-Id](ADR-004-entrega-http-at-least-once-com-event-id.md)

## Contexto e problema

O destinatário das notificações precisa verificar que o corpo recebido foi autenticado com o segredo compartilhado e não foi adulterado. Sofia também identifica o risco de um único segredo global ampliar o impacto de um vazamento [1]. A autenticação JWT de usuários da API atual é um mecanismo distinto da assinatura de notificações outbound [4].

## Direcionadores da decisão

- Autenticidade e integridade do corpo enviado [1].
- Limitação do impacto de um segredo comprometido ao seu endpoint [1].
- Possibilidade de migrar segredos sem interromper imediatamente a integração do cliente [1].

## Opções consideradas

1. HMAC-SHA256 com segredo distinto por endpoint e suporte a rotação.
2. Segredo global compartilhado pelos endpoints, explicitamente rejeitado pelo risco de vazamento [1].

Não foi documentada uma comparação de algoritmos ou mecanismos alternativos ao HMAC. A variante global é uma alternativa de escopo do segredo, não uma análise de mTLS, OAuth ou JWT para envio outbound.

## Resultado da decisão

A decisão observada é assinar o corpo com HMAC-SHA256, manter segredo por endpoint e permitir rotação com coexistência de 24 horas. A URL de destino deve usar HTTPS [1]. A reunião prevê um header de assinatura e um timestamp de envio [2]; não estabelece que o timestamp faça parte dos dados autenticados.

O corpo é serializado uma única vez em JSON UTF-8; o HMAC incide exatamente sobre os bytes transmitidos, sem incluir headers. A assinatura é hexadecimal minúscula, com prefixo `sha256=`, em `X-Signature`. Na janela de rotação, o envio apresenta a assinatura com o segredo atual nesse header e uma segunda com o anterior em `X-Signature-Previous`; após 24 horas, a segunda deixa de ser enviada e o segredo anterior é removido. O consumidor pode validar uma das assinaturas durante a transição.

Esta geração não representa a revisão de segurança solicitada antes do deploy [3].

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| Segredo por endpoint, rotacionável | Limita o alcance de vazamento e concede janela de migração ao cliente. | Por inferência, exige administrar múltiplas credenciais e seus estados durante a rotação. |
| Segredo global | Por inferência, reduz a quantidade de credenciais distintas. | A reunião rejeita o compartilhamento por expor outros endpoints quando um segredo vaza. |

## Consequências

Os consumidores precisarão verificar o corpo autenticado com o segredo correspondente. Por inferência, alterar o contrato de assinatura exigirá coordenação com esses consumidores. O reuso de logging não garante proteção de novos campos: o logger atual redige campos conhecidos, sem um campo de segredo de webhook [5].

Os segredos são gerados com 32 bytes aleatórios criptograficamente seguros e apresentados ao cliente apenas na criação ou rotação. São persistidos cifrados com AES-256-GCM e uma chave de serviço fornecida pelo ambiente; logs e respostas de consulta omitem segredos e assinaturas. O consumidor faz comparação de assinatura em tempo constante e deduplica por endpoint e `event_id`. O timestamp permanece informativo e não integra o HMAC; por isso, não se promete impedir replay apenas pela idade indicada nesse header. A proteção contra reaplicação depende da retenção dos identificadores pelo consumidor.

A revisão de segurança permanece uma atividade solicitada, sem evidência de conclusão [3].

## Referências

1. [Transcrição — motivação, HMAC, segredo, rotação e HTTPS](../../TRANSCRICAO.md):118–138, [09:19] a [09:23] Sofia e demais participantes.
2. [Transcrição — headers de envio](../../TRANSCRICAO.md):260–266, [09:44] Marcos/Diego/Sofia.
3. [Transcrição — revisão de segurança antes do deploy](../../TRANSCRICAO.md):274–276, [09:46] Sofia/Larissa. A transcrição consta de `7ef4317`; o commit não prova a data da decisão.
4. [Middleware — autenticação JWT atual](../../src/middlewares/auth.middleware.ts):27–43.
5. [Logger — campos de redação existentes](../../src/shared/logger/index.ts):4–18.
