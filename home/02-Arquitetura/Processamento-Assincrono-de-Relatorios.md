# Processamento Assíncrono de Relatórios

> **Estado documental:** esta página descreve a arquitetura canônica proposta para relatórios PDF. Ela não comprova implementação, capacidade medida ou tecnologia selecionada.

## Objetivo

No Prometheus V1, a geração síncrona de relatórios divide CPU, memória, conexões e operações de entrada e saída com a navegação. A evidência disponível sustenta a existência de contenção em recursos compartilhados, mas não comprova bloqueios de banco como causa específica.

No V2, a decisão vigente é retirar a geração pesada de PDFs do caminho síncrono. A solicitação é registrada e processada em segundo plano, permitindo que a navegação continue enquanto o documento é produzido.

## Fluxo canônico

> requisição → registro do trabalho → fila → Report Workers → geração do PDF → armazenamento do resultado → consulta ou download posterior

1. **Requisição:** Grades valida autenticação, autorização, vínculo e parâmetros necessários ao relatório.
2. **Registro do trabalho:** a solicitação recebe uma referência rastreável e um estado persistente antes de ser tratada como aceita para processamento.
3. **Fila:** o trabalho é publicado em uma fila que desacopla a taxa de solicitações da capacidade de geração. O produto de mensageria ainda não foi selecionado.
4. **Report Workers:** um worker disponível consome o trabalho e consulta os dados acadêmicos autorizados. “Report Worker” identifica uma responsabilidade de processamento, não um microsserviço confirmado.
5. **Geração:** o worker aplica as regras necessárias e produz o PDF fora da requisição web original.
6. **Armazenamento do resultado:** o arquivo concluído é gravado em Report Storage ou Object Storage, fora do banco transacional; o registro persistente conserva estado, autoria, parâmetros, versão e referência do objeto.
7. **Consulta ou download posterior:** o usuário acompanha o estado e, quando o resultado estiver concluído, realiza o download mediante nova validação de autorização.

Esse fluxo não define endpoint, código HTTP, formato de mensagem, produto de fila ou mecanismo de notificação. Essas escolhas continuam abertas.

## Diagrama canônico

O diagrama abaixo é uma representação conceitual do fluxo. Fila, Report Workers e armazenamento são responsabilidades ou abstrações arquiteturais; o desenho não seleciona produto nem define unidades independentes de implantação.

![image](uploads/f0ef047b4517b42f2fe8e5f72a3dabd7/image.png){width=825 height=600}

A [fonte editável equivalente em PlantUML](/home/02-Arquitetura/Fonte-PlantUML-Processamento-Assincrono-de-Relatorios) detalha a sequência, os estados e a falha controlada sem selecionar broker, padrão auxiliar ou topologia independente.

## Estados do relatório

O acompanhamento precisa distinguir, conceitualmente, pelo menos:

| Estado conceitual | Significado |
|---|---|
| **Pendente** | Solicitação registrada e aguardando processamento |
| **Em processamento** | Trabalho sob responsabilidade de um Report Worker |
| **Concluído** | PDF persistido e referência do resultado disponível |
| **Falha controlada** | Processamento não concluído; o estado e o diagnóstico foram preservados para nova tentativa ou tratamento |

Os nomes exatos, transições, prazos de retenção e política de reprocessamento ainda não foram definidos. Um relatório não deve ser apresentado como concluído antes de o arquivo e o estado necessário estarem persistidos.

## Idempotência e consistência

Fila e paralelismo podem fazer o mesmo trabalho chegar mais de uma vez ao processamento. O fluxo deve ser idempotente: repetição ou nova entrega da mesma solicitação não pode produzir duplicações visíveis nem associar um documento incorreto ao usuário.

O registro do trabalho precisa fornecer uma identidade estável para correlação. A coordenação entre esse registro e a publicação na fila deve impedir que o sistema confirme um trabalho que não possa ser localizado ou processado. O mecanismo técnico dessa coordenação, inclusive eventual uso de padrões adicionais, ainda não foi selecionado.

Dados acadêmicos e metadados persistentes permanecem fontes necessárias para reconstruir o estado. Nenhum cache é fonte de autoridade para notas ou conclusão do relatório.

## Tratamento de falhas

- se um Report Worker falhar antes da conclusão confirmada, o trabalho deve permanecer recuperável para processamento posterior;
- novas tentativas devem ser controladas e seguras para repetição;
- falha da fila, dos workers ou do armazenamento não deve ser reportada como sucesso;
- quando a geração estiver temporariamente indisponível, a interface deve apresentar estado pendente, atraso ou falha controlada;
- relatórios já concluídos podem continuar disponíveis aos usuários autorizados quando o armazenamento estiver acessível;
- falhas e retomadas devem ser observáveis e permitir diagnóstico e reconciliação.

Semântica de entrega, confirmações, limites de tentativas, tratamento de mensagens problemáticas, retenção e redundância dependem da tecnologia escolhida e permanecem pontos abertos.

## Isolamento da navegação

A resposta síncrona não aguarda a montagem completa do PDF. A fila absorve variações de entrada e os Report Workers executam a carga intensiva fora do atendimento web original. Se a capacidade de geração estiver atrasada, a degradação deve permanecer limitada aos relatórios sempre que as demais dependências permitirem.

Essa separação de responsabilidade não altera o estilo do sistema: o Prometheus V2 continua sendo um **monólito modular inicialmente implantado como uma unidade**. A documentação não estabelece Report Workers ou Grades como microsserviços independentes. A topologia física e a quantidade de workers continuam dependentes de validação e métricas.

## Relação com o RNF05

O [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) exige suporte a até **500 relatórios PDF simultâneos sem degradar significativamente a navegação**. Fila e Report Workers são a resposta arquitetural registrada para isolar e paralelizar essa carga.

A existência desses componentes não comprova o requisito. A aceitação depende de teste com navegação concorrente e medição de tempo de fila, duração de processamento, taxa de erro, relatórios concluídos e utilização dos recursos. Duração, mix de relatórios, ambiente e limite quantitativo de “degradação significativa” ainda precisam ser definidos.

## Decisões não tomadas nesta arquitetura

Cache distribuído, Redis, RabbitMQ, CDN privada, Transactional Outbox, Dead Letter Queue e uma resposta HTTP específica aparecem em estudos como exemplos ou alternativas. Eles não são requisitos do fluxo canônico e não são tecnologias selecionadas.

As escolhas abertas são mantidas em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos). As decisões relacionadas estão em [DA-05 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais) e nos [ADRs](/home/02-Arquitetura/ADRs).
