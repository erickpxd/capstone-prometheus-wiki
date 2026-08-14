# Priorização e Rastreabilidade

## Critério de priorização

A priorização considera impacto sobre dados e operação, relação com os problemas do V1 e dificuldade de reversão das decisões arquiteturais. A atividade mais recente sobre atributos estabelece segurança, manutenibilidade, desempenho e escalabilidade como ordem principal. Preservação dos dados aparece imediatamente depois da segurança por constituir uma restrição central da modernização.

| Prioridade | Grupo | Justificativa |
|---:|---|---|
| 1 | Segurança e privacidade | Uma falha pode expor dados pessoais, notas e registros institucionais. |
| 2 | Preservação e confiabilidade | O acervo histórico impede a substituição imediata do V1. |
| 3 | Manutenibilidade e modularidade | Limites claros evitam reproduzir o acoplamento do legado. |
| 4 | Desempenho | Conteúdos e relatórios já são gargalos concretos. |
| 5 | Escalabilidade e disponibilidade | A plataforma deve suportar picos e atingir 99,9% de disponibilidade. |
| 6 | Integrações essenciais | GRA e vídeo habilitam funções centrais e removem trabalho manual. |
| 7 | Novas capacidades acadêmicas | Avaliações, fóruns, portais e requisitos adicionais entram incrementalmente. |

Prioridade não significa implantação simultânea. Segurança, preservação e limites modulares devem orientar todos os incrementos. Os gargalos de Content e Grades são tratados cedo por já produzirem degradação no V1; novas funções entram sobre essa base sem ampliar novamente o acoplamento.

## Aplicação dos requisitos à arquitetura

A arquitetura de referência possui Users API, Courses API, Content API, Grades API e Videos API. Os requisitos adicionais introduzem responsabilidades de notificações, análise acadêmica, *proctoring*, gamificação, portfólio, comunicação interna, recuperação de credenciais, geração documental e processamento assíncrono.

Essas responsabilidades devem ser consideradas durante a decomposição, mas não criam automaticamente um microsserviço para cada requisito. Uma capacidade pode ser um componente interno de um módulo existente, uma colaboração entre domínios ou uma integração. A decisão depende de coesão, propriedade das regras, carga, disponibilidade e ciclo de mudança.

| Elemento arquitetural | Requisitos que mais o influenciam | Consequência principal |
|---|---|---|
| Users API | RF-BASE-01, RF-BASE-12, RF10, RNF02 | Identidade, perfis, recuperação e controles de acesso |
| Courses API | RF-BASE-02, RF-BASE-07, RF07, RF09 | Cursos, fóruns, avisos e calendário |
| Content API e armazenamento | RF-BASE-03, RF04, RF08, RNF03 | Metadados, gravações, portfólio e ativos fora do banco |
| Grades API | RF-BASE-04, RF-BASE-06, RF02, RF03, RF05, RF11, RNF05 | Avaliações, notas, análise, documentos, exportações e carga de relatórios |
| Videos API | RF-BASE-08, RF04, RF07 | Sessões, links, calendário e gravações |
| Grades Bridge e mensageria | RF-BASE-11, RNF04 | Sincronização assíncrona e isolamento do GRA |
| Comunicação e notificações | RF-BASE-07, RF01, RF09, RF12 | Fóruns, avisos, notificações e mensagens |
| Infraestrutura transversal | RF-BASE-09, RNF01, RNF02 | Acesso externo, disponibilidade e proteção de dados |

## Rastreabilidade inicial por domínio

| Domínio ou preocupação | Requisitos relacionados |
|---|---|
| Usuários e autenticação | RF-BASE-01, RF-BASE-12, RF10, RNF02 |
| Cursos | RF-BASE-02, RF-BASE-07, RF07, RF09 |
| Conteúdo | RF-BASE-03, RF04, RF08, RNF03 |
| Avaliações | RF-BASE-06, RF02 |
| Notas e relatórios | RF-BASE-04, RF03, RF05, RF11, RNF05 |
| GRA e integração | RF-BASE-11, RNF04 |
| Fóruns e comunicação | RF-BASE-07, RF01, RF09, RF12 |
| Videoconferência | RF-BASE-08, RF04, RF07 |
| Gamificação | RF06 |
| Portfólio | RF08 |
| Pais ou responsáveis | RF-BASE-10, RF01, RF08, RF09, RNF02 |
| Disponibilidade | RNF01 |
| Desempenho | RNF03, RNF05 |
| Segurança | RF-BASE-12, RF10, RNF02 |
| Escalabilidade | RNF01, RNF05 |

## Uso da rastreabilidade

A matriz é inicial e deve ser revisada quando casos de uso, regras e critérios de aceitação forem detalhados. Ela permite localizar quais módulos são afetados por uma mudança, relacionar requisitos aos testes e impedir que uma nova função seja atribuída a uma API apenas por conveniência. Requisitos transversais, como segurança e disponibilidade, devem ser verificados em todos os componentes aplicáveis, ainda que apareçam em uma linha específica da matriz.
