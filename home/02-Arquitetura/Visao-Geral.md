# Visão Geral da Arquitetura

## Estado documental

Esta página apresenta a arquitetura proposta e vigente na documentação do Prometheus V2. Ela não comprova que o V2 esteja implementado ou implantado.

## Contexto

O Prometheus V1 é o LMS utilizado pela School X há aproximadamente quinze anos. Sua arquitetura atual documentada é uma aplicação web monolítica e pouco modular, com estado mantido no servidor e banco MySQL centralizado para dados relacionais e arquivos binários.

O V2 busca preservar as funções e os dados históricos enquanto corrige limitações estruturais e permite evolução gradual. O diagnóstico detalhado do legado e das motivações da modernização está em [Análise de Lacunas](/home/01-Requisitos/Analise-de-Lacunas).

## Estratégia arquitetônica

A modernização é incremental, com validação e possibilidade de retorno. O estilo de referência do V2 é um **monólito modular inicialmente implantado como uma unidade**, organizado por limites lógicos e contratos explícitos.

Users, Courses, Content, Grades e Videos são módulos lógicos. Seus contratos de API não significam microsserviços, processos ou unidades independentes de implantação. Uma futura extração somente deve ocorrer quando métricas demonstrarem benefício de escala, disponibilidade ou evolução superior ao custo da distribuição.

## Prometheus V1 × Prometheus V2

| Aspecto | Prometheus V1 atual | Prometheus V2 proposto |
|---|---|---|
| Organização | Monólito pouco modular e com responsabilidades misturadas | Monólito modular com limites e contratos explícitos |
| Implantação | Aplicação única do legado | Inicialmente uma unidade de implantação |
| Estado | Sessão mantida no servidor | Redução de estado local; mecanismo ainda não selecionado |
| Dados | MySQL centralizado para dados e binários | Banco SQL principal inicialmente compartilhado, com propriedade lógica por módulo; SGBD ainda não selecionado |
| Arquivos | BLOBs armazenados no banco principal | Object Storage e CDN; fornecedores ainda não selecionados |
| Relatórios | Geração síncrona no fluxo web | Processamento assíncrono por fila e Report Workers, com armazenamento do resultado para consulta posterior |
| GRA | Transcrição manual de notas | Grades Bridge, REST, *webhooks* e sincronização assíncrona |
| Vídeo | Capacidade não oferecida pelo legado | Contexto acadêmico no Prometheus e transmissão por provedor externo ainda não selecionado |
| Acesso | Originalmente restrito à rede interna | Acesso web externo com os controles de segurança exigidos |

## Mapa resumido da arquitetura

| Área | Conteúdo | Fonte principal |
|---|---|---|
| Módulos | Limites, responsabilidades e diferença entre módulo, API e implantação | [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) |
| Dados e integrações | Persistência, propriedade lógica, armazenamento e sistemas externos | [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) |
| Relatórios | Fluxo assíncrono, estados, idempotência, falhas e relação com o RNF05 | [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) |
| Tecnologias | Abstrações, exemplos estudados, escolhas abertas e evoluções condicionadas | [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) |
| Qualidade | Atributos, cenários e formas de verificação | [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) |
| Diagramas | Contexto e visão de contêineres | [Diagramas C4](/home/02-Arquitetura/Diagramas-C4) |
| Decisões | Inventário canônico DA-01–DA-11 e seu estado documental | [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) |
| ADRs | Contexto, decisão e consequências de DA-01 a DA-09 | [Architecture Decision Records](/home/02-Arquitetura/ADRs) |
| Trade-offs | Benefícios obtidos e custos ou riscos aceitos | [Análise de Trade-offs](/home/03-Trade-offs) |

## Evolução

Especialização de Grades, persistência própria, NoSQL, cache distribuído, replicação, topologias ativa/ativa e implantação independente de módulos são propostas ou evoluções condicionadas a métricas e novas decisões. Elas não fazem parte automaticamente da arquitetura vigente, e as tecnologias correspondentes continuam abertas quando não houver decisão explícita.
