# ADR-006: Worker separado com polling

- Status: Proposto — rascunho do registro; escolha explicitada na reunião, não implementada.
- Data da decisão: 2026-06-18.
- Data de geração: 2026-11-12, America/Sao_Paulo (UTC−03:00).
- Módulo: WEBHOOKS; escopo relacionado: ORDERS e DATA.
- Revisão analisada: `e7f6311b96ee3b21bf8cb7077f53a6e4045143eb`.

**Depende de:**

- [ADR-003: Outbox no MySQL na transação de mudança de status](ADR-003-outbox-mysql-na-transacao-de-status.md)
- [ADR-002: Prisma como acesso a dados e migrações](ADR-002-prisma-para-acesso-a-dados.md)

**Utilizado por:**

- [ADR-007: Retry limitado com backoff e DLQ persistida](ADR-007-retry-com-backoff-e-dlq.md)

## Contexto e problema

As notificações precisam ser enviadas após o registro transacional, sem compartilhar o ciclo de vida da API. A reunião aceita polling como mecanismo de leitura da outbox e define processo separado para que reiniciar a API não implique reiniciar o processamento [1, 2]. O código inspecionado ainda apresenta apenas o bootstrap da API e não declara um script de worker [4, 5].

## Direcionadores da decisão

- Atender à expectativa de notificação abaixo de dez segundos com uma solução simples no banco existente [1].
- Separar a execução da entrega do ciclo de vida da API [2].
- Reutilizar stack e banco, com cliente Prisma próprio de cada processo [3].

## Opções consideradas

1. Processo separado com polling periódico da outbox.
2. Worker no mesmo processo da API.
3. Notificação reativa ao processo externo por mecanismo associado a trigger do banco.

As três opções aparecem na discussão [1, 2]. Múltiplos workers, particionamento e locks são citados como evolução futura, não como desenho adotado [2].

## Resultado da decisão

A decisão observada é iniciar com um worker em processo separado, consultando pendências por polling a cada dois segundos, no mesmo banco e com instância Prisma própria [1–3]. A lógica segue o módulo da feature e os padrões existentes, sem implicar sua presença no código atual.

O worker respeita uma sequência monotônica de eventos por pedido, atribuída na transação de emissão. Para cada par pedido/endpoint, uma entrega posterior aguarda a anterior terminar com sucesso ou ser encaminhada à DLQ, inclusive durante o backoff. Outros pedidos e endpoints continuam elegíveis. A ordem pretendida vale para o fluxo automático; replay administrativo pode produzir chegada tardia de evento antigo e não recompõe a ordem histórica.

## Prós e contras das opções

| Opção | Prós | Contras e trade-offs |
|---|---|---|
| Processo separado com polling | Isola o ciclo de vida da API e usa o banco existente. | Por inferência, cria outro processo a operar e acrescenta consultas periódicas e espera até a próxima leitura. |
| Worker junto à API | Por inferência, reduz o número de processos a administrar. | A reunião rejeita o acoplamento de reinícios da API e do worker. |
| Notificação reativa associada a trigger | Busca reagir sem esperar o próximo polling. | Na discussão, a trigger não resolve diretamente a comunicação com um processo externo; exigiria mecanismo adicional. |

## Consequências

O serviço de entrega terá operação e conexões próprias. O intervalo de polling foi aceito como trade-off de simplicidade e latência [1–3]. Por inferência, esse intervalo não limita a latência total: backlog e duração dos envios também contam. Não houve validação de desempenho.

A escala inicial é um worker; aumentar o paralelismo exigirá reavaliar coordenação e ordenação [2]. Cada entrega é reivindicada atomicamente com uma concessão de processamento de 60 segundos e um token exclusivo. Após expirar a concessão, o trabalho volta a ser elegível; atualizações só são aceitas com o token ainda válido. Se houver interrupção depois do envio e antes da confirmação no banco, a entrega poderá ser repetida com o mesmo `event_id`, comportamento compatível com a deduplicação pelo consumidor.

## Referências

1. [Transcrição — meta de latência e polling](../../TRANSCRICAO.md):22,60–68, [09:02] Marcos e [09:09–09:10] Diego/Larissa.
2. [Transcrição — separação de processo e ordem](../../TRANSCRICAO.md):70–86, [09:11] Diego a [09:13] Larissa.
3. [Transcrição — organização e Prisma por processo](../../TRANSCRICAO.md):162–180, [09:27] Bruno a [09:30] Larissa.
4. [Bootstrap atual da API](../../src/server.ts):6–27.
5. [Manifesto — scripts atuais](../../package.json):10–20. A transcrição e o bootstrap já constam de `7ef4317`; não se observou a implementação do worker no histórico consultado.
