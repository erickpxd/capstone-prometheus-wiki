# 2. Arquitetura

> **Estado documental:** esta seção descreve a arquitetura vigente na documentação do Prometheus V2. Ela não comprova implementação ou implantação.

Esta seção corresponde ao Documento de Design de Arquitetura. A fonte canônica está organizada por responsabilidade documental, sem transformar módulos lógicos em serviços independentes.

## Visão e estrutura

- [Visão Geral](/home/02-Arquitetura/Visao-Geral) — V1 × V2, estilo vigente, implantação inicial e mapa da solução;
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) — limites lógicos, contratos e propriedade de regras;
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) — persistência, armazenamento e sistemas externos;
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) — fluxo canônico de PDFs, estados, idempotência e falhas;
- [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) — cenários e formas de verificação;
- [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) — abstrações, alternativas estudadas e escolhas pendentes.

## Decisões

- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) — inventário DA-01–DA-11 e seus estados;
- [Architecture Decision Records](/home/02-Arquitetura/ADRs) — contexto e consequências de DA-01 a DA-09;
- [Análise de Trade-offs](/home/03-Trade-offs) — benefícios, custos e tensões ainda abertas.

## Diagramas

- [Diagramas C4](/home/02-Arquitetura/Diagramas-C4)
  - [Nível 1 — Contexto](/home/02-Arquitetura/Diagramas-C4/Nivel-1-Contexto)
  - [Nível 2 — Contêineres](/home/02-Arquitetura/Diagramas-C4/Nivel-2-Conteineres)

O Prometheus V2 permanece documentado como **monólito modular inicialmente implantado como uma unidade**. Tecnologias, topologias e fornecedores sem decisão explícita continuam abertos ou condicionados.
