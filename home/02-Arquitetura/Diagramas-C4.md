# Diagramas C4

> **Estado documental:** estes diagramas representam a arquitetura vigente na documentação do Prometheus V2. Eles não comprovam implementação ou implantação.

## Catálogo

| Diagrama | Escopo | Fonte | Estado |
|---|---|---|---|
| [Nível 1 — Contexto](/home/02-Arquitetura/Diagramas-C4/Nivel-1-Contexto) | Pessoas, Prometheus V2 e dependências externas | PNG publicado, leitura textual e fonte PlantUML editável | Vigente na documentação |
| [Nível 2 — Contêineres](/home/02-Arquitetura/Diagramas-C4/Nivel-2-Conteineres) | Unidade inicial de implantação, persistência, abstrações de infraestrutura e sistemas externos | PNG publicado, leitura textual e fonte PlantUML editável | Vigente na documentação |

## Regra de leitura

O C4 Nível 1 apresenta o Prometheus V2 como um único sistema. O C4 Nível 2 detalha sua **aplicação modular inicialmente implantada como uma unidade** e as capacidades de persistência e integração necessárias.

No Nível 1, Object Storage e CDN aparecem como dependência de infraestrutura da solução. No Nível 2, essas abstrações são agrupadas no escopo da solução para explicitar suas relações com a aplicação. Esse posicionamento não define propriedade operacional, hospedagem, produto ou fornecedor.

Users, Courses, Content, Grades e Videos são módulos lógicos dentro da aplicação. Contratos de API, Grades Bridge e Report Workers não são automaticamente contêineres, microsserviços ou unidades independentes de implantação. Banco SQL, fila, Object Storage, Report Storage e CDN são representados por função, sem selecionar produto ou fornecedor.

A fonte textual e as decisões canônicas prevalecem sobre qualquer material preliminar ou histórico. Diagramas acadêmicos que estudaram produtos, padrões ou topologias condicionadas não integram o catálogo C4 vigente. A interpretação detalhada está em [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades), [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes), [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) e [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais).

As imagens publicadas são renderizações das fontes PlantUML editáveis, que não dependem de bibliotecas ou inclusões externas. A leitura textual e as decisões canônicas orientam qualquer revisão futura; uma fonte alterada deve ser renderizada e conferida antes de substituir o PNG correspondente.
