# Requisitos Não Funcionais

## RNF01 — Disponibilidade e resiliência

O Prometheus V2 deve garantir 99,9% de tempo de atividade. A arquitetura precisa suportar períodos críticos, como fechamento de semestre, e permitir ampliação horizontal das capacidades que apresentarem maior demanda.

## RNF02 — Segurança e proteção de dados

Dados pessoais e acadêmicos de estudantes e responsáveis devem ser criptografados em trânsito e em repouso. O Portal de Pais deve exigir autenticação multifator. O acesso às operações deve respeitar os papéis de administrador, professor, estudante e responsável.

## RNF03 — Desempenho de conteúdos

Apresentações e PDFs distribuídos por CDN devem carregar em menos de dois segundos para usuários fora da rede interna da School X. O requisito justifica retirar os binários do banco principal e utilizar armazenamento dedicado.

## RNF04 — Desacoplamento da integração com o GRA

A sincronização de notas deve ocorrer de forma assíncrona por fila ou *message broker*. Lentidão ou indisponibilidade do GRA não pode impedir que o professor conclua a operação correspondente no Prometheus.

## RNF05 — Escalabilidade de relatórios

A Grades API deve suportar a geração simultânea de até 500 PDFs sem degradar o tempo de resposta das demais funções de navegação. A geração deve ser isolada do fluxo síncrono e preparada para escalabilidade horizontal.

## Restrições complementares

A solução deve preservar os dados existentes, reduzir dependência de sessão local, manter alta coesão e baixo acoplamento e permitir modernização gradual. APIs REST não devem ser interpretadas automaticamente como microsserviços: a arquitetura de referência permanece um monólito modular.
