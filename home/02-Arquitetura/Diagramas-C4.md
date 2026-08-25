# Diagramas C4

> **Estado documental:** estes diagramas representam a arquitetura vigente na documentação do Prometheus V2. Eles não comprovam implementação ou implantação.

## Catálogo

| Diagrama | Escopo | Fonte | Estado |
|---|---|---|---|
| [Nível 1 — Contexto](/home/02-Arquitetura/Diagramas-C4/Nivel-1-Contexto) | Pessoas, Prometheus V2 e dependências externas | PNG preservado, leitura textual e PlantUML reconstruído | Vigente na documentação |
| [Nível 2 — Contêineres](/home/02-Arquitetura/Diagramas-C4/Nivel-2-Conteineres) | Unidade inicial de implantação, persistência, abstrações de infraestrutura e sistemas externos | Mermaid na página e PlantUML equivalente | Vigente na documentação |

## Regra de leitura

O C4 Nível 1 apresenta o Prometheus V2 como um único sistema. O C4 Nível 2 detalha sua **aplicação modular inicialmente implantada como uma unidade** e as capacidades de persistência e integração necessárias.

Users, Courses, Content, Grades e Videos são módulos lógicos dentro da aplicação. Contratos de API, Grades Bridge e Report Workers não são automaticamente contêineres, microsserviços ou unidades independentes de implantação. Banco SQL, fila, Object Storage, Report Storage e CDN são representados por função, sem selecionar produto ou fornecedor.

A fonte textual e as decisões canônicas prevalecem sobre qualquer material preliminar ou histórico. Diagramas acadêmicos que estudaram produtos, padrões ou topologias condicionadas não integram o catálogo C4 vigente. A interpretação detalhada está em [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades), [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes), [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) e [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais).

As fontes PlantUML não dependem de bibliotecas ou inclusões externas. Elas devem ser renderizadas e revisadas antes de substituir uma imagem publicada.
