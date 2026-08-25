# Nível 1 — Contexto

> **Estado documental:** diagrama vigente na documentação arquitetural, revisado em 19 de agosto de 2026. A imagem não comprova implementação.

## Leitura consolidada

O diagrama de contexto apresenta o Prometheus V2 como uma única unidade e o relaciona aos atores Administrador, Professor, Estudante e Pai ou Responsável. As dependências externas documentadas são o GRA, a plataforma de videoconferência e a infraestrutura de CDN/Object Storage usada pelos conteúdos.

O GRA troca informações de notas por API REST e *webhooks*. A plataforma externa oferece sessões e gravações. A presença da CDN/Object Storage registra uma dependência de infraestrutura do Prometheus; banco de dados, APIs, fila e workers continuam fora deste nível por serem detalhes internos da solução.

## Diagrama

![Prometheus V2 — C4 Nível 1: Contexto](/uploads/9d118ca185cda520cbcba2d2fcab4570/Captura_de_tela_de_2026-07-19_21-15-51.png){width=830 height=600}

Uma [fonte PlantUML reconstruída](/home/02-Arquitetura/Diagramas-C4/Fonte-PlantUML-Nivel-1-Contexto) representa a mesma semântica sem produtos ou fornecedores. Ela não é a fonte original do PNG e só deve substituí-lo depois de renderização e revisão visual.

## Rastreabilidade

O desenho é compatível com [DA-04, DA-06 e DA-07](/home/02-Arquitetura/Decisoes-Arquiteturais). A leitura canônica dos limites está em [Visão Geral](/home/02-Arquitetura/Visao-Geral) e [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).

A fonte editável original do PNG ainda não foi localizada. O ativo é preservado porque continua coerente com o nível de contexto e a reconstrução PlantUML ainda não possui renderização aprovada.
