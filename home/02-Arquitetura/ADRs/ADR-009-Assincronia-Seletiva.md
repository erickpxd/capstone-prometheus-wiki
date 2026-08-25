# ADR-009 — Assincronia Seletiva

## Status

Aceito na documentação arquitetural.

## Contexto

Relatórios demorados e integrações externas podem bloquear o fluxo principal ou ampliar o impacto de uma falha. Ao mesmo tempo, operações acadêmicas que exigem confirmação não podem ser convertidas indiscriminadamente em efeitos concluídos posteriormente.

## Decisão

O Prometheus V2 usa assincronia de forma seletiva para efeitos tolerantes a atraso. Operações acadêmicas que exigem confirmação permanecem síncronas no fluxo principal; trabalhos demorados e efeitos externos podem prosseguir em segundo plano quando seu contrato explicitar estado, falha e conclusão posterior.

O processamento de relatórios e a sincronização com o GRA são aplicações concretas desta política. A decisão não transforma todo o Prometheus em uma arquitetura orientada a eventos.

## Alternativas consideradas

- **Executar todos os efeitos de forma síncrona:** não adotada para trabalhos demorados e dependências externas tolerantes a atraso, pois os acopla à resposta do fluxo principal.
- **Aplicar assincronia seletivamente:** adotada conforme a tolerância a atraso e o contrato de cada operação.

## Consequências

### Benefícios

- isola o fluxo principal de tarefas demoradas e falhas externas toleráveis;
- preserva a responsividade da navegação;
- permite processamento posterior e concorrente nos casos aprovados.

### Custos e riscos

- efeitos podem terminar depois da operação principal;
- exige estados explícitos, idempotência, novas tentativas, auditoria e reconciliação conforme o fluxo;
- aumenta a necessidade de observabilidade e tratamento de falhas assíncronas.

Os trade-offs gerais estão consolidados em [Análise de Trade-offs](/home/03-Trade-offs).

## Relação com requisitos

- [RF-BASE-04](/home/01-Requisitos/Requisitos-Funcionais) e [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais): geração de relatórios sem degradar significativamente a navegação;
- [RF-BASE-11](/home/01-Requisitos/Requisitos-Funcionais) e [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais): sincronização com o GRA sem bloquear o registro local.

## Relação com outras decisões

- fundamenta o fluxo de relatórios definido no [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios);
- fundamenta a integração com o GRA definida no [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge);
- depende da observabilidade orientada à operação registrada na DA-11, sem selecionar sua plataforma.

O índice completo está em [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais).

## Pontos ainda abertos

- critérios formais para classificar outros efeitos como tolerantes a atraso;
- produto de mensageria e garantias de entrega;
- políticas de ordenação, repetição, retenção e tratamento de falha por fluxo;
- métricas, alertas e procedimentos operacionais dos processos assíncronos.

## Evidência de implementação

Não confirmada no material disponível.
