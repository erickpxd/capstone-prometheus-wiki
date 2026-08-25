# ADR-006 — Integração GRA por Grades Bridge

## Status

Aceito na documentação arquitetural.

Esse status registra uma decisão para o Prometheus V2 e não comprova sua implementação.

## Contexto

No Prometheus V1, as notas são transcritas manualmente para o GRA, embora o sistema externo disponibilize REST e *webhooks*. A integração precisa reduzir esse trabalho sem fazer a confirmação local de uma nota depender da latência ou disponibilidade momentânea do GRA.

## Decisão

O Prometheus V2 usa o **Grades Bridge** como adaptador lógico entre Grades e os contratos REST e *webhooks* do GRA. A nota é validada e registrada no domínio Grades antes da sincronização externa, que ocorre de forma assíncrona por uma abstração de fila ou Message Broker.

O fluxo deve preservar idempotência, novas tentativas controladas, auditoria e reconciliação. O Grades Bridge não é documentado como microsserviço nem como unidade independente de implantação, e o produto de mensageria ainda não foi selecionado. Esta decisão não define a fonte final de autoridade nem as regras de conflito durante a convivência entre os sistemas.

## Alternativas consideradas

- **Manter a transcrição manual:** corresponde ao processo do V1, mas preservaria atraso e risco de erro operacional; não foi escolhida para o V2.
- **Depender de uma chamada síncrona ao GRA na operação principal:** tornaria a confirmação ao professor sensível à latência e indisponibilidade externa; não foi escolhida.
- **Integrar de forma assíncrona por Grades Bridge:** alternativa escolhida para isolar o contrato externo e permitir processamento posterior controlado.

## Consequências

### Benefícios

- elimina a transcrição manual como fluxo pretendido do V2;
- impede que indisponibilidade temporária do GRA bloqueie a confirmação local da nota;
- concentra a adaptação dos contratos externos em um limite lógico;
- possibilita rastreamento, auditoria e reconciliação da sincronização.

### Custos e riscos

- a atualização do GRA ocorre posteriormente e pode permanecer pendente ou falhar;
- repetição e concorrência exigem idempotência e controle explícito de estado;
- divergências entre os sistemas exigem diagnóstico e reconciliação;
- mudanças no contrato, autenticação ou disponibilidade do GRA continuam fora do controle do Prometheus.

## Relação com requisitos

- [RF-BASE-11](/home/01-Requisitos/Requisitos-Funcionais) requer integração automática com o GRA para eliminar a transcrição manual; REST e *webhooks* pertencem à decisão arquitetural descrita neste ADR.
- [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais) determina que lentidão ou indisponibilidade do GRA não bloqueie o registro local confirmado ao professor.

## Relação com outras decisões

- detalha a **DA-06** do índice de [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) e a integração descrita em [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes);
- respeita o limite de Grades definido no [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios);
- é uma aplicação concreta da política do [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva).

## Pontos ainda abertos

- contrato completo, autenticação e validação dos *webhooks* do GRA;
- fonte de autoridade e regras de conflito durante a convivência;
- limites de novas tentativas, prazos, política de reconciliação e retenção da auditoria;
- produto de mensageria e semântica de entrega;
- SLA e procedimentos operacionais acordados com o sistema externo.

## Evidência de implementação

Não confirmada no material disponível.
