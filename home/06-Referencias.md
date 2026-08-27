# 6. Referências

> **Estado documental:** esta página consolida as fontes usadas na análise, no desenho arquitetônico e na implementação referencial do Prometheus V2. Uma referência técnica ou um produto citado como exemplo não constitui, por si só, tecnologia selecionada ou implementação comprovada.

## Critérios de organização

As referências estão separadas em quatro grupos:

1. materiais acadêmicos que definem o caso e registram sua evolução;
2. bibliografia que fundamenta os conceitos de arquitetura, qualidade e DDD;
3. documentação técnica oficial citada nos estudos e na implementação referencial;
4. páginas canônicas e artefatos produzidos no próprio projeto.

Os arquivos acadêmicos mencionados pelo nome em `docs/` foram usados como fontes locais durante a consolidação, mas não estão publicados na Wiki. Quando autor, data ou endereço eletrônico não estavam disponíveis no material recebido, a lacuna foi preservada em vez de preenchida por inferência.

## Materiais acadêmicos do Capstone

| Material | Identificação disponível | Contribuição para o projeto |
|---|---|---|
| `ArchSw_Capstone.pptx.pdf` | Jala University. *Introdução ao Projeto Final*. Material da disciplina Arquitetura de Software II, 2026 | Escopo, processo e entregáveis do Capstone |
| `week-1.pt.pdf` | Jala University. *Projeto Capstone — Prometheus V1/V2*. Material da disciplina Arquitetura de Software II, 2026 | Caso-base, diagnóstico do V1, objetivos, arquitetura de referência, requisitos e possibilidades de evolução |
| `Laboratório B.1 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. Laboratório B.1, Arquitetura de Software II, 2026 | Diagnóstico inicial, modularização, binários fora do banco e modernização incremental; os diagramas de SkyDelivery nele presentes não representam o Prometheus |
| `Tarefa 1.4 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Tarefa 1.4*, Arquitetura de Software II. Paulista, jul. 2026 | Análise de sistema de referência e lacunas do Prometheus V1 |
| `B.2 Laboratório Semana 2 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Laboratório da Semana 2*, Arquitetura de Software II, 2026 | Arquitetura preliminar, portais, contratos, persistência, C4 e classificação arquitetura × design |
| `Tarefa 2.4 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Tarefa 2.4*, Arquitetura de Software II, 2026 | Arquitetura × design e princípios SoC, SRP, DRY, DIP, encapsulamento e Lei de Demeter |
| `B.3 Laboratório Semana 3 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Laboratório da Semana 3*, Arquitetura de Software II, 2026 | Comparação entre monólito modular e serviços e priorização de atributos de qualidade |
| `Tarefa 3.4 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Tarefa 3.4*, Arquitetura de Software II, 2026 | Manutenibilidade, desempenho, escalabilidade, segurança, confiabilidade e trade-offs |
| `B.3 Laboratório Semana 4 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Laboratório da Semana 4*, Arquitetura de Software II, 2026 | Coesão, acoplamento, decomposição e componentes internos de Grades |
| `Tarefa 4.4 AS2 Erick Fernando de Lima Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Tarefa 4.4*, Arquitetura de Software II, 2026 | Diagnóstico modular, propriedade de regras, contratos, encapsulamento e interfaces |
| `CONTEXTO_COMPLETO_PROMETHEUS.md` | Consolidação local dos materiais das primeiras semanas; autoria e data bibliográfica não informadas | Fonte auxiliar de rastreabilidade, não substitui os materiais originais nem a documentação canônica |
| `tarefa 5  Semana 5AS2 Erick Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Tarefa 5 — Plano de Melhoria de Desempenho e Disponibilidade: Relatórios PDF*. Paulista, ago. 2026 | Fila, Report Workers, armazenamento, cache estudado, métricas e observabilidade |
| `B.3 Laboratório Semana 6 B.5 Laboratório Semana 5AS2 Erick Monteiro.pdf` | MONTEIRO, Erick Fernando de Lima. *Laboratório 6 — Estratégias de Dimensionamento e Padrões de Resiliência*. Arquitetura de Software II, 2026 | Escala de Grades e padrões Retry, Circuit Breaker, Bulkhead e Graceful Degradation para vídeo |
| Enunciados da Semana 7 | Material da plataforma sobre requisitos de negócio, DDD, classificação de domínios e *bounded contexts*; metadados completos não fornecidos | Base acadêmica da hipótese de modelagem estratégica e do mapa de contextos DDD |

## Bibliografia de arquitetura, design e domínio

- BASS, Len; CLEMENTS, Paul; KAZMAN, Rick. *Software Architecture in Practice*. 4. ed. Boston: Addison-Wesley Professional, 2021.
- BROWN, Simon. *The C4 Model for Visualising Software Architecture*. Disponível em: [c4model.com](https://c4model.com/). Acessos registrados nos trabalhos em jul. 2026; consulta de consolidação em 26 ago. 2026.
- EVANS, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Boston: Addison-Wesley, 2004. Síntese oficial disponível em: [DDD Reference](https://www.domainlanguage.com/ddd/reference/). Acesso em: 26 ago. 2026.
- FOWLER, Martin. *Patterns of Enterprise Application Architecture*. Boston: Addison-Wesley, 2002.
- FOWLER, Martin. *Software Architecture Guide*. Disponível em: [martinfowler.com/architecture](https://martinfowler.com/architecture/). Acessos registrados nos trabalhos em jul. 2026; consulta de consolidação em 26 ago. 2026.
- FORD, Neal; RICHARDS, Mark; SADALAGE, Pramod; DEHGHANI, Zhamak. *Software Architecture: The Hard Parts — Modern Trade-Off Analyses for Distributed Architectures*. Sebastopol: O'Reilly Media, 2021.
- ISO; IEC. *ISO/IEC 25010:2023 — Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Product quality model*. 2. ed., 2023. Disponível em: [ISO 25010](https://www.iso.org/standard/78176.html). Acesso em: 26 ago. 2026.
- MARTIN, Robert C. *Clean Architecture: A Craftsman's Guide to Software Structure and Design*. Boston: Pearson, 2017.
- MARTIN, Robert C. *Clean Code: A Handbook of Agile Software Craftsmanship*. Upper Saddle River: Prentice Hall, 2008.
- NEWMAN, Sam. *Building Microservices: Designing Fine-Grained Systems*. 2. ed. Sebastopol: O'Reilly Media, 2021.
- NYGARD, Michael. *Documenting Architecture Decisions*. Cognitect, 15 nov. 2011. Disponível em: [Cognitect](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions). Acesso em: 26 ago. 2026.
- PARNAS, David L. On the Criteria To Be Used in Decomposing Systems into Modules. *Communications of the ACM*, v. 15, n. 12, p. 1053–1058, 1972.
- RICHARDS, Mark; FORD, Neal. *Fundamentals of Software Architecture: An Engineering Approach*. Sebastopol: O'Reilly Media, 2020.

## Documentação técnica oficial

### Arquitetura, escala e resiliência

| Tema | Referência | Uso no projeto |
|---|---|---|
| C4 | [C4 Model — Diagrams](https://c4model.com/diagrams) | Níveis de contexto e contêineres |
| ADR | [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) | Estrutura e finalidade dos registros de decisão |
| DDD | [DDD Reference — Eric Evans](https://www.domainlanguage.com/ddd/reference/) | Domínios, *bounded contexts*, linguagem ubíqua e relações entre contextos |
| Qualidade de produto | [ISO/IEC 25010:2023](https://www.iso.org/standard/78176.html) | Vocabulário de atributos de qualidade |
| Padrões de nuvem | [Cloud Design Patterns — Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/patterns/) | Catálogo conceitual de integração, escala e resiliência |
| Nivelamento de carga | [Queue-Based Load Leveling](https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling) | Fundamentação do uso de fila para absorver picos de relatórios |
| Circuit Breaker | [Circuit Breaker Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker) | Contenção de falhas persistentes em dependências externas |
| Retry | [Retry Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/retry) | Tratamento controlado de falhas transitórias |
| Bulkhead | [Bulkhead Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead) | Isolamento de recursos e falhas |
| Retry storm | [Retry Storm Antipattern](https://learn.microsoft.com/en-us/azure/architecture/antipatterns/retry-storm/) | Risco de novas tentativas agressivas |
| Resiliência HTTP em .NET | [Build resilient HTTP apps](https://learn.microsoft.com/en-us/dotnet/core/resilience/http-resilience) | Alternativas estudadas para políticas de chamadas externas |
| Balanceamento | [Load-balancing options](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview) | Comparação de mecanismos de distribuição de tráfego |
| Particionamento de dados | [Data partitioning guidance](https://learn.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning) | Fundamentação de evoluções condicionadas da persistência |
| CDN | [How CloudFront delivers content](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowCloudFrontWorks.html) | Exemplo estudado de entrega por CDN; não seleciona fornecedor |
| Mensageria | [RabbitMQ Reliability Guide](https://www.rabbitmq.com/docs/reliability) | Exemplo estudado de confiabilidade em broker; RabbitMQ não foi selecionado |
| Cache | [Redis `EXPIRE`](https://redis.io/docs/latest/commands/expire/) | Exemplo estudado de expiração; Redis e política de cache não foram selecionados |
| Observabilidade | [Prometheus — Overview](https://prometheus.io/docs/introduction/overview/) | Exemplo estudado de coleta de métricas; não comprova implantação |
| Dashboards | [Grafana — Dashboards](https://grafana.com/docs/grafana/latest/dashboards/) | Exemplo estudado de visualização; não comprova implantação |
| Diagramas textuais | [PlantUML](https://plantuml.com/) | Fonte editável dos diagramas versionados |
| Sequência PlantUML | [Sequence Diagram](https://plantuml.com/sequence-diagram) | Fluxos de relatórios e resiliência |
| Implantação PlantUML | [Deployment Diagram](https://plantuml.com/deployment-diagram) | Topologias conceituais estudadas |

As referências desta subseção incluem produtos avaliados nos laboratórios. Elas documentam o estudo realizado e **não alteram** o estado registrado em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos).

### Implementação referencial

| Fonte | Relação com o código existente |
|---|---|
| [Repositório da implementação referencial](https://gitlab.com/jala-university1/cohort-3/PT.CSAR-364.GA.T2.26.M1/SB/erick.lima/capstone-prometheus) | Fonte primária do recorte Users/Identity e Portal Web |
| Arquivos `Prometheus.Users.Api.csproj` e `package.json` do repositório | Fonte primária das versões efetivamente declaradas; prevalecem sobre resumos documentais |
| [.NET 8](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-8/overview) e [ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/?view=aspnetcore-8.0) | Plataforma e framework do backend referencial |
| [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/) | Acesso a dados e migrações do backend referencial |
| [SQLite](https://www.sqlite.org/docs.html) | Banco usado somente na prova de conceito local |
| [RFC 7519 — JSON Web Token](https://www.rfc-editor.org/info/rfc7519/) | Especificação do formato JWT usado na implementação referencial |
| [BCrypt.Net-Next](https://www.nuget.org/packages/bcrypt.net-next/) | Pacote de hash de senha declarado pelo backend |
| [React](https://react.dev/learn) | Biblioteca de interface do Portal Web |
| [TypeScript](https://www.typescriptlang.org/docs/) | Linguagem do frontend |
| [Vite](https://vite.dev/guide/) | Ferramenta de desenvolvimento e build do frontend |
| [React Router](https://reactrouter.com/) | Roteamento do Portal Web |
| [Axios](https://axios-http.com/docs/intro) | Cliente HTTP usado pelo frontend |
| [xUnit](https://xunit.net/) | Framework dos testes de integração do backend |

SQLite, JWT, BCrypt e as bibliotecas do frontend descrevem o recorte implementado. Eles não selecionam automaticamente o SGBD, o Identity Provider ou a plataforma definitiva do Prometheus V2.

## Documentação canônica do Prometheus V2

### Requisitos e diagnóstico

- [Requisitos](/home/01-Requisitos)
- [Análise de Lacunas](/home/01-Requisitos/Analise-de-Lacunas)
- [Requisitos Funcionais](/home/01-Requisitos/Requisitos-Funcionais)
- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais)
- [Priorização](/home/01-Requisitos/Priorizacao)

### Arquitetura e decisões

- [Visão Geral](/home/02-Arquitetura/Visao-Geral)
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades)
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes)
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios)
- [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade)
- [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos)
- [Decisões Arquiteturais — DA-01 a DA-11](/home/02-Arquitetura/Decisoes-Arquiteturais)
- [Architecture Decision Records — ADR-001 a ADR-009](/home/02-Arquitetura/ADRs)
- [Análise de Trade-offs](/home/03-Trade-offs)
- [Cronograma e Gestão de Riscos](/home/04-Cronograma-e-Riscos)

### Diagramas e fontes editáveis

- [Catálogo de Diagramas C4](/home/02-Arquitetura/Diagramas-C4)
- [C4 Nível 1 — Contexto](/home/02-Arquitetura/Diagramas-C4/Nivel-1-Contexto)
- [Fonte PlantUML do C4 Nível 1](/home/02-Arquitetura/Diagramas-C4/Fonte-PlantUML-Nivel-1-Contexto)
- [C4 Nível 2 — Contêineres](/home/02-Arquitetura/Diagramas-C4/Nivel-2-Conteineres)
- [Fonte PlantUML do C4 Nível 2](/home/02-Arquitetura/Diagramas-C4/Fonte-PlantUML-Nivel-2-Conteineres)
- [Fonte PlantUML — Módulos e Responsabilidades](/home/02-Arquitetura/Fonte-PlantUML-Modulos-e-Responsabilidades)
- [Fonte PlantUML — Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Fonte-PlantUML-Processamento-Assincrono-de-Relatorios)
- [Fonte PlantUML — Mapa de Contextos DDD Proposto](/home/02-Arquitetura/Fonte-PlantUML-Mapa-de-Contextos-DDD-Proposto)

### Apresentações e implementação

- [Apresentações](/home/05-Apresentacoes)
- [Apresentação Parcial 1](/home/05-Apresentacoes/Apresentacao-Parcial-1)
- [Apresentação Final](/home/05-Apresentacoes/Apresentacao-Final)
- [Implementação Referencial](/home/07-Implementacao-Referencial)

## Precedência e atualização

Para o estado atual do projeto, a documentação canônica e os ADRs prevalecem sobre atividades antigas, diagramas preliminares e exemplos de produto. Os materiais acadêmicos permanecem como evidência da evolução do raciocínio. O repositório de código prevalece para afirmar o que foi efetivamente implementado no recorte referencial.

Referências web consolidadas em **26 de agosto de 2026**. Datas de acesso anteriores foram preservadas quando constavam nos trabalhos originais.
