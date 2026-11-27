# ADR-005: HMAC-SHA256 com segredo por endpoint

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: desconhecida.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: DATA e autenticação da integração externa.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Relacionado a:**

- [ADR-004: HTTP outbound at-least-once com X-Event-Id](ADR-004-entrega-http-at-least-once-com-event-id.md)

## Contexto e problema

- **ADR-005-INT-01** — JWT atual autentica usuários da API.
- **ADR-005-INT-02** — Pino redige campos conhecidos sem campo de segredo de webhook.

## Direcionadores da decisão

- **ADR-005-SEG-01** — Exigir HTTPS.
- **ADR-005-DEP-01** — Revisão de segurança solicitada antes do deploy.

## Alternativas consideradas

- **ADR-005-ALT-01** — Segredo global amplia alcance de vazamento.

## Resultado da decisão

- **ADR-005** — HMAC-SHA256 sobre o corpo, segredo distinto por endpoint e envio de assinatura ao consumidor.
- **ADR-005-DEC-01** — Rotação com segredo anterior válido em paralelo por 24 horas.
- **ADR-005-CON-01** — Como o HMAC cobre o corpo, o timestamp de envio informado em `X-Timestamp` fica fora da assinatura.

## Prós e contras das opções

- **ADR-005-TRD-01** — Rotação de credencial exclusiva exige migração do consumidor na janela de convivência.

## Consequências

Um segredo por endpoint limita o alcance de um vazamento. Cada integração precisa administrar sua credencial e a janela de troca de 24 h.

### Questão em aberto

- **ADR-005-CON-03** — Como representar dois segredos válidos no contrato e como proteger seu armazenamento?

## Referências

- [Transcrição da reunião](../../TRANSCRICAO.md) · [Tracker](../TRACKER.md).
- [src/middlewares/auth.middleware.ts](../../src/middlewares/auth.middleware.ts).
- [src/shared/logger/index.ts](../../src/shared/logger/index.ts).
