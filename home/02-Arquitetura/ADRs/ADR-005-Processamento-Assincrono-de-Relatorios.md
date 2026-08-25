# ADR-005 — Processamento Assíncrono de Relatórios

## Status

Aceito na documentação arquitetural.

Esse status registra uma decisão para o Prometheus V2 e não comprova sua implementação.

## Contexto

No Prometheus V1, a geração síncrona de relatórios PDF disputa recursos com a navegação. O RNF05 requer até 500 relatórios simultâneos sem degradação significativa das demais funções, mas a evidência disponível ainda não comprova capacidade implementada nem resultados de teste.

## Decisão

O fluxo canônico retira a geração do PDF da requisição web original:

> requisição → registro do trabalho → fila → Report Workers → geração do PDF → armazenamento do resultado → consulta ou download posterior

O registro persistente identifica o trabalho e permite acompanhar estados conceituais de pendente, em processamento, concluído ou falha controlada. A fila desacopla a entrada da capacidade de geração; Report Workers processam os trabalhos; e o resultado concluído é mantido fora do banco transacional para acesso posterior autorizado.

O processamento deve ser idempotente e recuperável diante de repetição, nova entrega ou falha. Fila e Report Workers são responsabilidades arquiteturais, não microsserviços ou unidades independentes de implantação confirmados. O Prometheus V2 permanece um monólito modular inicialmente implantado como uma unidade, e o produto de mensageria continua aberto.

## Alternativas consideradas

- **Gerar o PDF de forma síncrona na requisição web:** corresponde ao fluxo do V1, mas mantém a carga pesada no caminho da navegação; não foi escolhida para o V2.
- **Registrar e processar o trabalho de forma assíncrona por fila e Report Workers:** alternativa escolhida para isolar a navegação e permitir paralelismo controlado.

## Consequências

### Benefícios

- preserva a resposta da navegação sem aguardar a montagem completa do PDF;
- desacopla a taxa de solicitações da capacidade instantânea de geração;
- permite ampliar a capacidade de processamento conforme métricas e testes;
- torna estado, falha e retomada de cada trabalho rastreáveis.

### Custos e riscos

- o resultado passa a ficar disponível posteriormente, e não na resposta original;
- fila, workers, estados e armazenamento exigem operação e observabilidade próprias;
- repetição, falha parcial e concorrência exigem idempotência e consistência entre registro e resultado;
- a aceitação do RNF05 ainda depende de cenário e teste de carga definidos.

## Relação com requisitos

- [RF-BASE-04 e RF05](/home/01-Requisitos/Requisitos-Funcionais) estabelecem a gestão de relatórios acadêmicos e a geração de PDFs com autenticidade.
- [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) estabelece a meta de até 500 PDFs simultâneos sem degradação significativa da navegação.

## Relação com outras decisões

- detalha a **DA-05** do índice de [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) e a fonte canônica de [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios);
- usa a separação de binários definida no [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco);
- é uma aplicação concreta da política registrada no [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva);
- respeita os limites lógicos definidos no [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios).

## Pontos ainda abertos

- produto de mensageria, semântica de entrega e confirmações;
- transições finais de estado, limites de novas tentativas e tratamento de trabalhos problemáticos;
- retenção de registros e PDFs, política de reprocessamento e forma de notificação;
- topologia, quantidade e critérios de ampliação dos Report Workers;
- duração, combinação de relatórios, ambiente e limite mensurável de degradação do RNF05.

## Evidência de implementação

Não confirmada no material disponível.
