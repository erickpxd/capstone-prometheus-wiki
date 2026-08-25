# Nível 1 — Contexto

> **Estado documental:** diagrama vigente na documentação arquitetural, revisado em 25 de agosto de 2026. A imagem não comprova implementação.

## Leitura consolidada

O diagrama de contexto apresenta o Prometheus V2 como uma única unidade e o relaciona aos atores Administrador, Professor, Estudante e Pai ou Responsável. As dependências documentadas são o GRA, a plataforma externa de videoconferência e a infraestrutura de CDN/Object Storage usada pelos conteúdos. A posição dessa infraestrutura no desenho não define propriedade operacional, hospedagem, produto ou fornecedor.

O GRA troca informações de notas por API REST e *webhooks*. A plataforma externa oferece sessões e gravações. A presença da CDN/Object Storage registra uma dependência de infraestrutura do Prometheus; banco de dados, APIs, fila e workers continuam fora deste nível por serem detalhes internos da solução.

## Diagrama

![C4 Nível 1 — contexto do Prometheus V2](uploads/8a3b4dfe03c84ea3e767d80fb7b655a8/image.png){width=900 height=336}

A imagem publicada acima é uma renderização da [fonte PlantUML reconstruída](/home/02-Arquitetura/Diagramas-C4/Fonte-PlantUML-Nivel-1-Contexto), que representa a mesma semântica sem produtos ou fornecedores. A fonte foi reconstruída a partir da leitura canônica porque a fonte editável do PNG histórico anterior não foi localizada.

## Rastreabilidade

O desenho é compatível com [DA-04, DA-06 e DA-07](/home/02-Arquitetura/Decisoes-Arquiteturais). A leitura canônica dos limites está em [Visão Geral](/home/02-Arquitetura/Visao-Geral) e [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).

A fonte textual, a imagem publicada e a leitura consolidada formam agora o conjunto vigente do diagrama. Futuras alterações devem manter essa equivalência e continuar subordinadas às decisões arquiteturais canônicas.
