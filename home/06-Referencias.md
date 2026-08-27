# 6. Referências

> **Escopo:** esta página reúne fontes externas, públicas e verificáveis usadas para fundamentar a análise, o desenho arquitetônico e a implementação referencial do Prometheus V2. A citação de uma tecnologia ou de um serviço como exemplo não significa que ele tenha sido selecionado para a arquitetura vigente nem comprova sua implementação.

Todos os endereços eletrônicos foram consultados em **26 de agosto de 2026**.

## Arquitetura de software e princípios de design

- BASS, Len; CLEMENTS, Paul; KAZMAN, Rick. *Software Architecture in Practice*. 4. ed. Addison-Wesley Professional, 2021. Disponível em: [InformIT — Software Architecture in Practice](https://www.informit.com/store/software-architecture-in-practice-9780136885887).
- RICHARDS, Mark; FORD, Neal. *Fundamentals of Software Architecture: An Engineering Approach*. O'Reilly Media, 2020. Disponível em: [O'Reilly — Fundamentals of Software Architecture](https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/).
- FORD, Neal; RICHARDS, Mark; SADALAGE, Pramod; DEHGHANI, Zhamak. *Software Architecture: The Hard Parts*. O'Reilly Media, 2021. Disponível em: [O'Reilly — Software Architecture: The Hard Parts](https://www.oreilly.com/library/view/software-architecture-the/9781492086888/).
- PARNAS, David L. On the Criteria To Be Used in Decomposing Systems into Modules. *Communications of the ACM*, v. 15, n. 12, p. 1053–1058, 1972. Disponível em: [ACM Digital Library](https://dl.acm.org/doi/10.1145/361598.361623).
- FOWLER, Martin. *Software Architecture Guide*. Disponível em: [martinfowler.com/architecture](https://martinfowler.com/architecture/).
- FOWLER, Martin. *Monolith First*. 2015. Disponível em: [martinfowler.com — Monolith First](https://martinfowler.com/bliki/MonolithFirst.html).
- MICROSOFT. *Architectural principles*. Disponível em: [Microsoft Learn — Architectural principles](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles).
- MICROSOFT. *Common web application architectures*. Disponível em: [Microsoft Learn — Common web application architectures](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures).

## Documentação arquitetônica e decisões

- BROWN, Simon. *The C4 model for visualising software architecture*. Disponível em: [C4 Model](https://c4model.com/).
- BROWN, Simon. *C4 model — Diagrams*. Disponível em: [C4 Model — Diagrams](https://c4model.com/diagrams).
- NYGARD, Michael. *Documenting Architecture Decisions*. Cognitect, 2011. Disponível em: [Cognitect — Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).
- PLANTUML. *PlantUML Language Reference Guide*. Disponível em: [PlantUML](https://plantuml.com/).
- PLANTUML. *Sequence Diagram*. Disponível em: [PlantUML — Sequence Diagram](https://plantuml.com/sequence-diagram).
- PLANTUML. *Deployment Diagram*. Disponível em: [PlantUML — Deployment Diagram](https://plantuml.com/deployment-diagram).

## Domain-Driven Design

- EVANS, Eric. *Domain-Driven Design Reference: Definitions and Pattern Summaries*. Domain Language, 2015. Disponível em: [Domain Language — DDD Reference](https://www.domainlanguage.com/ddd/reference/).
- MICROSOFT. *Use domain analysis to model microservices*. Disponível em: [Azure Architecture Center — Domain analysis](https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis).

Essas fontes sustentam o uso de domínios, subdomínios, linguagem ubíqua, *bounded contexts* e mapas de contexto. A aplicação desses conceitos no Prometheus V2 não implica que cada contexto seja um microsserviço ou uma unidade independente de implantação.

## Qualidade, disponibilidade e observabilidade

- ISO; IEC. *ISO/IEC 25010:2023 — Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Product quality model*. 2. ed., 2023. Disponível em: [ISO — ISO/IEC 25010:2023](https://www.iso.org/standard/78176.html).
- BEYER, Betsy et al. *Site Reliability Engineering: How Google Runs Production Systems*. O'Reilly Media, 2016. Capítulo 4: *Service Level Objectives*. Disponível em: [Google SRE — Service Level Objectives](https://sre.google/sre-book/service-level-objectives/).
- BEYER, Betsy et al. *Site Reliability Engineering: How Google Runs Production Systems*. Capítulo 6: *Monitoring Distributed Systems*. Disponível em: [Google SRE — Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/).
- BEYER, Betsy et al. *Site Reliability Engineering: How Google Runs Production Systems*. Apêndice A: *Availability Table*. Disponível em: [Google SRE — Availability Table](https://sre.google/sre-book/availability-table/).
- GOOGLE. *The Site Reliability Workbook*. Capítulo 6: *Monitoring*. Disponível em: [Google SRE — Monitoring](https://sre.google/workbook/monitoring/).
- PROMETHEUS AUTHORS. *Prometheus — Overview*. Disponível em: [Prometheus Documentation](https://prometheus.io/docs/introduction/overview/).
- GRAFANA LABS. *Dashboards*. Disponível em: [Grafana Documentation — Dashboards](https://grafana.com/docs/grafana/latest/dashboards/).

Prometheus Monitoring e Grafana são exemplos estudados de ferramentas de observabilidade; a plataforma definitiva de observabilidade do Prometheus V2 continua sem seleção documentada.

## Escalabilidade, integração e resiliência

- MICROSOFT. *Cloud Design Patterns*. Disponível em: [Azure Architecture Center — Cloud Design Patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/).
- MICROSOFT. *Queue-Based Load Leveling pattern*. Disponível em: [Azure Architecture Center — Queue-Based Load Leveling](https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling).
- MICROSOFT. *Circuit Breaker pattern*. Disponível em: [Azure Architecture Center — Circuit Breaker](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker).
- MICROSOFT. *Retry pattern*. Disponível em: [Azure Architecture Center — Retry](https://learn.microsoft.com/en-us/azure/architecture/patterns/retry).
- MICROSOFT. *Bulkhead pattern*. Disponível em: [Azure Architecture Center — Bulkhead](https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead).
- MICROSOFT. *Health Endpoint Monitoring pattern*. Disponível em: [Azure Architecture Center — Health Endpoint Monitoring](https://learn.microsoft.com/en-us/azure/architecture/patterns/health-endpoint-monitoring).
- MICROSOFT. *Retry Storm antipattern*. Disponível em: [Azure Architecture Center — Retry Storm](https://learn.microsoft.com/en-us/azure/architecture/antipatterns/retry-storm/).
- MICROSOFT. *Build resilient HTTP apps: Key development patterns*. Disponível em: [Microsoft Learn — HTTP resilience](https://learn.microsoft.com/en-us/dotnet/core/resilience/http-resilience).
- MICROSOFT. *Load-balancing options*. Disponível em: [Azure Architecture Center — Load balancing](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview).
- MICROSOFT. *The API gateway pattern versus the direct client-to-microservice communication*. Disponível em: [Azure Architecture Center — API gateways](https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway).
- MICROSOFT. *Data partitioning guidance*. Disponível em: [Azure Architecture Center — Data partitioning](https://learn.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning).
- OPENAPI INITIATIVE. *OpenAPI Specification*. Disponível em: [OpenAPI Specification](https://spec.openapis.org/oas/latest.html).

As fontes desta seção fundamentam padrões e alternativas. Elas não tornam API Gateway, implantação independente, replicação, particionamento ou *auto-scaling* partes automaticamente vigentes da arquitetura; essas evoluções permanecem condicionadas a requisitos, métricas e decisões explícitas.

## Armazenamento, distribuição de conteúdo e mensageria

- AMAZON WEB SERVICES. *What is Amazon S3?* Disponível em: [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html).
- AMAZON WEB SERVICES. *How CloudFront delivers content*. Disponível em: [Amazon CloudFront Developer Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowCloudFrontWorks.html).
- RABBITMQ. *Reliability Guide*. Disponível em: [RabbitMQ Documentation — Reliability](https://www.rabbitmq.com/docs/reliability).
- REDIS. *EXPIRE command*. Disponível em: [Redis Documentation — EXPIRE](https://redis.io/docs/latest/commands/expire/).

S3 e CloudFront são referências concretas para compreender as abstrações de Object Storage e CDN. RabbitMQ e Redis são exemplos estudados de *message broker* e cache. Nenhum desses produtos ou fornecedores está selecionado como tecnologia definitiva do Prometheus V2.

## Identidade e segurança

- JONES, Michael; BRADLEY, John; SAKIMURA, Nat. *RFC 7519 — JSON Web Token (JWT)*. IETF, 2015. Disponível em: [RFC Editor — RFC 7519](https://www.rfc-editor.org/info/rfc7519/).
- SHEFFER, Yaron; HARDT, Dick; JONES, Michael. *RFC 8725 — JSON Web Token Best Current Practices*. IETF, 2020. Disponível em: [RFC Editor — RFC 8725](https://www.rfc-editor.org/info/rfc8725/).
- HARDT, Dick. *RFC 6749 — The OAuth 2.0 Authorization Framework*. IETF, 2012. Disponível em: [RFC Editor — RFC 6749](https://www.rfc-editor.org/info/rfc6749/).
- SAKIMURA, Nat et al. *OpenID Connect Core 1.0 incorporating errata set 2*. OpenID Foundation, 2023. Disponível em: [OpenID Foundation — OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html).
- OWASP FOUNDATION. *Application Security Verification Standard*. Disponível em: [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/).
- OWASP FOUNDATION. *Password Storage Cheat Sheet*. Disponível em: [OWASP Cheat Sheet Series — Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html).
- OWASP FOUNDATION. *Session Management Cheat Sheet*. Disponível em: [OWASP Cheat Sheet Series — Session Management](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

JWT e BCrypt descrevem mecanismos presentes no recorte referencial. OAuth 2.0, OpenID Connect e um Identity Provider externo permanecem alternativas arquiteturais, não escolhas definitivas do sistema.

## Tecnologias da implementação referencial

- MICROSOFT. *.NET 8 — What's new*. Disponível em: [Microsoft Learn — .NET 8](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-8/overview).
- MICROSOFT. *ASP.NET Core documentation*. Disponível em: [Microsoft Learn — ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/?view=aspnetcore-8.0).
- MICROSOFT. *Entity Framework Core documentation*. Disponível em: [Microsoft Learn — Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/).
- SQLITE CONSORTIUM. *SQLite Documentation*. Disponível em: [SQLite Documentation](https://www.sqlite.org/docs.html).
- REACT TEAM. *React documentation*. Disponível em: [React — Learn](https://react.dev/learn).
- MICROSOFT. *TypeScript Documentation*. Disponível em: [TypeScript Documentation](https://www.typescriptlang.org/docs/).
- VOIDZERO INC. *Vite Guide*. Disponível em: [Vite Documentation](https://vite.dev/guide/).
- REMIX SOFTWARE. *React Router documentation*. Disponível em: [React Router](https://reactrouter.com/).
- AXIOS CONTRIBUTORS. *Axios documentation*. Disponível em: [Axios Docs](https://axios-http.com/docs/intro).
- XUNIT.NET. *xUnit.net*. Disponível em: [xUnit.net](https://xunit.net/).
- THE NUGET GALLERY. *BCrypt.Net-Next*. Disponível em: [NuGet — BCrypt.Net-Next](https://www.nuget.org/packages/BCrypt.Net-Next/).

Estas referências documentam as tecnologias observadas na implementação referencial. Elas não definem, por extensão, o SGBD, o Identity Provider, a infraestrutura ou a plataforma tecnológica final de todo o Prometheus V2.
