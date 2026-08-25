# Tecnologias e Pontos Abertos

> **Regra central:** exemplos estudados não equivalem a tecnologias selecionadas, contratadas ou implementadas.

A arquitetura do Prometheus V2 define comportamentos e capacidades antes de escolher produtos. Quando uma linha informa “tecnologia ainda não selecionada”, a Wiki não deve promover nenhum exemplo a padrão vigente. A presença de uma ferramenta em laboratório, referência ou diagrama preliminar também não comprova sua adoção.

| Capacidade | Abstração arquitetural | Exemplos estudados | Status |
|---|---|---|---|
| Banco relacional | Banco SQL principal inicialmente compartilhado | MySQL no V1; MySQL ou PostgreSQL no C4 preliminar | A base relacional é a referência vigente; o SGBD do V2 é tecnologia ainda não selecionada |
| Cache | Cache para estado compartilhado, consultas ou referências reutilizáveis | Redis; padrão Cache-Aside | Proposta condicionada; produto, escopo, TTL e invalidação ainda não definidos |
| Message Broker | Fila para relatórios, GRA e outros efeitos tolerantes a atraso | RabbitMQ | O comportamento assíncrono é requerido; o produto de mensageria ainda não foi selecionado |
| Elasticidade de Report Workers | Dimensionamento controlado da capacidade de geração de PDFs | Pool `1..N`; profundidade e idade da fila; throughput; CPU, memória e saturação do SQL como sinais estudados | Proposta condicionada; topologia, mínimo, máximo, concorrência, *cooldown* e implantação dos workers ainda não definidos |
| Object Storage | Armazenamento dedicado de conteúdos, gravações, portfólios e PDFs concluídos | Amazon S3 como referência estudada | Capacidade arquitetural vigente; fornecedor e produto ainda não selecionados |
| CDN | Distribuição de conteúdos apropriados a partir de uma origem persistente | Amazon CloudFront como referência estudada | Capacidade vigente para conteúdo; fornecedor, regiões, expiração e controle de acesso ainda não selecionados |
| Identity Provider | Autenticação, autorização e suporte ao MFA exigido | JWT, OAuth 2.0 e OpenID Connect foram citados como mecanismos ou protocolos | Provedor, protocolos, sessão, algoritmos e gestão de chaves ainda não selecionados |
| Video Provider | Plataforma externa de transmissão de áudio e vídeo | 100ms.live | Delegação a provedor externo é decisão vigente; fornecedor e contrato ainda não selecionados |
| Load Balancer | Distribuição de tráfego e retirada de instâncias não saudáveis | API Gateway e Load Balancer aparecem como papéis arquiteturais; nenhum produto confirmado | Topologias redundantes ou escala isolada são propostas condicionadas; tecnologia ainda não selecionada |
| Observabilidade | Plataforma para métricas, logs, alertas, saúde e visão externa | Prometheus Monitoring, Grafana e Alertmanager | Capacidade requerida; produtos não selecionados e implantação não comprovada |
| Resiliência | Políticas de timeout, Retry, Circuit Breaker, Bulkhead e degradação controlada | *Exponential backoff*, *jitter*, Polly, `Microsoft.Extensions.Resilience` e `Microsoft.Extensions.Http.Resilience` | Comportamentos foram estudados; biblioteca, composição, limites e parâmetros ainda não selecionados |
| Frontend | Camada de apresentação web e portais por perfil | SPA como possibilidade | Tecnologia de frontend ainda não selecionada |
| Orquestração e implantação | Execução e operação da unidade de implantação e de sua infraestrutura | Kubernetes foi citado, mas não é decisão documentada | Plataforma ainda não selecionada; o estilo vigente continua monólito modular com implantação inicial unificada |
| Persistência especializada de Grades | Repositório próprio para uma capacidade que venha a ser extraída | NoSQL e MongoDB no C4 preliminar | Evolução futura condicionada a métricas, consistência, auditoria, migração, GRA e recuperação |
| Replicação de dados | Capacidade adicional de leitura ou recuperação | Primary + Read Replicas; *primary/standby* | Propostas condicionadas distintas; SGBD, RPO, RTO, promoção e política de consistência permanecem abertos |
| Distribuição privada de relatórios | Entrega protegida de PDFs concluídos | CDN privada | Alternativa condicionada a volume, latência, autorização e expiração; não integra a decisão canônica atual |

## Decisões de capacidade e escolhas de produto

Algumas capacidades já integram a arquitetura proposta, embora seu produto continue aberto:

- base SQL principal;
- Object Storage para retirar binários do banco e armazenar resultados;
- CDN para distribuição de conteúdo;
- fila e Report Workers para relatórios;
- fila ou Message Broker para sincronização assíncrona com o GRA;
- provedor externo para transmissão de vídeo;
- observabilidade operacional.

Outras alternativas permanecem condicionadas, como cache distribuído, elasticidade automática de Report Workers, persistência própria de Grades, réplicas, topologias ativa/ativa, composição concreta de Retry/Circuit Breaker e CDN privada para relatórios. Aumentar workers só é justificável quando medições mostrarem ganho e quando SQL, mensageria e armazenamento suportarem a pressão adicional.

## Critério para fechar um ponto aberto

Uma tecnologia só deve passar a “selecionada” quando houver evidência explícita de decisão, incluindo o problema atendido, alternativas avaliadas, consequências e requisitos relacionados. Quando aplicável, também devem ser definidos contrato, segurança, consistência, operação, migração, custo, métricas e estratégia de substituição.

Até essa decisão existir, as páginas canônicas devem preferir as abstrações **Banco SQL**, **Cache**, **Message Broker**, **Object Storage**, **CDN**, **Identity Provider**, **Video Provider**, **Load Balancer**, **Observability Platform** e **políticas de resiliência**.

O inventário canônico permanece em [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais), e os fundamentos das decisões significativas estão nos [Architecture Decision Records](/home/02-Arquitetura/ADRs). As responsabilidades e integrações relacionadas são detalhadas em [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) e [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).
