# Priorização e Rastreabilidade

## Estado e limites da priorização

Esta página preserva a ordem inicial por grupos registrada na documentação disponível. A ordem é revisável, não constitui cronograma, não significa implantação simultânea e não atribui prioridade numérica individual aos 24 requisitos funcionais nem aos cinco requisitos não funcionais.

Os IDs relacionados a cada grupo fornecem rastreabilidade. Eles não estabelecem uma ordem entre requisitos dentro do grupo, e um mesmo requisito pode contribuir para mais de uma preocupação.

## Critérios documentados

A priorização considera o impacto sobre dados e operação, a relação com os problemas do Prometheus V1 e a dificuldade de reversão das respostas arquiteturais. Segurança, manutenibilidade, desempenho e escalabilidade aparecem entre as maiores preocupações, enquanto a preservação dos dados constitui uma restrição central da modernização.

A sequência abaixo conserva a síntese documental existente. As posições exatas não possuem validação independente por responsáveis, orçamento ou capacidade da equipe e, portanto, não devem ser interpretadas como ranking definitivo ou compromisso de entrega.

## Prioridade inicial por grupos

| Prioridade | Grupo | Justificativa | IDs e escopos relacionados |
|---:|---|---|---|
| 1 | Segurança e privacidade | Uma falha pode expor dados pessoais, notas e registros institucionais. | [RF-BASE-01, RF-BASE-09, RF-BASE-10, RF-BASE-12 e RF10](/home/01-Requisitos/Requisitos-Funcionais); [RNF02](/home/01-Requisitos/Requisitos-Nao-Funcionais) — Users, identidade, acesso externo e Portal de Pais. |
| 2 | Preservação e confiabilidade | O acervo histórico impede a substituição imediata do V1. | [RF-BASE-05](/home/01-Requisitos/Requisitos-Funcionais); [DA-01](/home/02-Arquitetura/Decisoes-Arquiteturais) e [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) — migração e conservação do acervo histórico. |
| 3 | Manutenibilidade e modularidade | Limites claros evitam reproduzir o acoplamento do legado. | [DA-02 e DA-03](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) e [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) — módulos lógicos e limites de domínio. |
| 4 | Desempenho | Conteúdos e relatórios já são gargalos concretos do V1. | [RF-BASE-03, RF-BASE-04, RF04 e RF05](/home/01-Requisitos/Requisitos-Funcionais); [RNF03 e RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) — Content, Grades, gravações e relatórios. |
| 5 | Escalabilidade e disponibilidade | A plataforma deve suportar picos e atender à meta de disponibilidade documentada. | [RF-BASE-09](/home/01-Requisitos/Requisitos-Funcionais); [RNF01 e RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) — acesso externo, continuidade e isolamento da carga de relatórios. |
| 6 | Integrações essenciais | GRA e vídeo habilitam funções centrais e reduzem trabalho manual. | [RF-BASE-08, RF-BASE-11, RF04 e RF07](/home/01-Requisitos/Requisitos-Funcionais); [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais) — videoconferência, gravações e sincronização de notas. |
| 7 | Novas capacidades acadêmicas | Avaliações, fóruns, portais e requisitos adicionais entram incrementalmente sobre a base modernizada. | [RF-BASE-02, RF-BASE-06, RF-BASE-07 e RF-BASE-10; RF01–RF12](/home/01-Requisitos/Requisitos-Funcionais) — cursos, avaliações, comunicação, notificações, análise acadêmica, gamificação, portfólio e exportações, sem ordem interna confirmada. |

Prioridade não significa implantação simultânea. Segurança, preservação e limites modulares orientam todos os incrementos; os gargalos de Content e Grades recebem atenção por já produzirem degradação no V1. A associação de um requisito a um escopo não cria automaticamente módulo, API REST, microsserviço ou unidade independente de implantação.

## Limitações conhecidas

- não há prioridade individual confirmada para RF-BASE-01–RF-BASE-12, RF01–RF12 ou RNF01–RNF05;
- datas, responsáveis, orçamento, capacidade da equipe e dependências de execução não foram fornecidos;
- autoridade, gatilhos e critérios formais para revisar a ordem inicial ainda não foram definidos;
- casos de uso, regras de negócio e critérios de aceitação individuais ainda precisam ser detalhados;
- a rastreabilidade deve ser revisada quando essas informações forem confirmadas.

## Fontes relacionadas

- [Análise de Lacunas](/home/01-Requisitos/Analise-de-Lacunas)
- [Requisitos Funcionais](/home/01-Requisitos/Requisitos-Funcionais)
- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais)
- [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade)
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades)
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais)
- [Architecture Decision Records](/home/02-Arquitetura/ADRs)
- [Análise de Trade-offs](/home/03-Trade-offs)
- [Cronograma e Riscos](/home/04-Cronograma-e-Riscos)
