# Visão Geral da Arquitetura

## Estratégia escolhida

O Prometheus V2 adota um **monólito modular** como etapa de modernização do sistema legado. A aplicação permanece inicialmente como uma unidade de implantação, mas seu interior é organizado por domínios com responsabilidades, regras e interfaces explícitas.

Essa escolha equilibra modularidade e simplicidade operacional. Uma arquitetura orientada a serviços permitiria implantação e escala independentes, mas também exigiria comunicação de rede, observabilidade distribuída, tratamento de falhas parciais, coordenação de dados e múltiplos processos de entrega. Para um sistema com quinze anos de dados e necessidade de migração gradual, esse custo foi considerado prematuro.

As APIs do V2 representam contratos entre portais e módulos. A presença de Users API, Courses API, Content API, Grades API e Videos API não significa que cada uma seja um microsserviço. Elas podem compartilhar implantação e infraestrutura enquanto mantêm limites lógicos. A extração de uma capacidade só deve ocorrer quando medições demonstrarem necessidade própria de escala, disponibilidade ou evolução.

## Camada de apresentação

O Portal Principal realiza autenticação e direciona cada usuário para a experiência correspondente. Os portais documentados são:

- Portal do Estudante, para cursos, conteúdos, fóruns, avaliações, notas e videoconferências;
- Portal do Professor, para cursos, materiais, avaliações, notas, relatórios e sessões;
- Portal do Administrador, para usuários, perfis e cursos;
- Portal de Pais, para consulta limitada das notas dos estudantes vinculados.

Os materiais admitem aplicações SPA, mas não selecionam uma tecnologia específica de *frontend*. A separação por papel organiza a experiência e reduz a exposição indevida de funções.

## Módulos de negócio

### Users API

Users concentra dados cadastrais, perfis e permissões. A decomposição mais recente recomenda separar internamente identidade — autenticação e autorização — dos dados de estudantes, professores, administradores e responsáveis. Outros módulos consultam contratos de identidade e autorização sem modificar diretamente o cadastro.

### Courses API

Courses administra cursos, disciplinas, turmas, períodos e vínculos acadêmicos. Matrículas constituem uma responsabilidade interna própria: Grades consulta o vínculo para validar uma nota, mas não altera diretamente os dados de matrícula. Fóruns e calendário permanecem associados ao contexto do curso.

### Content API

Content gerencia metadados e o ciclo de vida dos materiais didáticos. Arquivos deixam de ser binários no banco principal e passam para armazenamento dedicado com distribuição por CDN. O módulo continua responsável pela autorização e pela associação entre o ativo e o curso.

### Grades API

Grades controla avaliações, lançamento e consulta de notas, cálculo acadêmico, publicação de resultados, histórico e relatórios. O domínio é decomposto internamente em serviços de Avaliações, Validação, Lançamento, Cálculo, Publicação e Relatórios.

A geração de PDFs é retirada do fluxo síncrono e enviada a uma fila processada em segundo plano. Isso permite atender ao requisito de até 500 relatórios simultâneos sem degradar a navegação. Grades é o principal candidato à escalabilidade horizontal seletiva.

Relatórios de períodos encerrados ou resultados pouco mutáveis podem usar cache ou pré-computação seletiva. Qualquer mudança de nota, publicação de resultado ou alteração da regra de cálculo deve invalidar o artefato anterior. Os relatórios concluídos são conservados em armazenamento próprio, separado da fila que representa o trabalho pendente.

### Videos API

Videos coordena agendamento, autorização, links de acesso e gravações. A transmissão de áudio e vídeo é delegada a uma plataforma externa especializada. O material cita 100ms.live apenas como exemplo, não como fornecedor escolhido.

## Integrações

O Grades Bridge concentra a comunicação com o GRA por REST e *webhooks*. A sincronização de escrita é assíncrona para impedir que lentidão externa bloqueie o professor. Mensagens repetidas precisam ser tratadas sem duplicação, e o fluxo deve preservar auditoria e permitir reconciliação.

A plataforma de videoconferência permanece fora do limite do Prometheus. Se ela estiver indisponível, notas, cursos e conteúdos devem continuar funcionando. Gravações retornam ao contexto acadêmico e são disponibilizadas conforme as permissões do curso.

Notificações, recuperação de senha por e-mail ou SMS e entrega *push* também exigem adaptadores externos, mas os documentos não definem fornecedores. O *message broker* é requerido para a integração GRA e pode ser reutilizado por relatórios e notificações que toleram atraso.

## Persistência

O V1 usa um MySQL central para dados e binários. A arquitetura de referência do V2 continua centrada em uma base SQL principal para usuários, cursos, fóruns, sessões e inicialmente notas. O material não confirma SQL Server nem outro SGBD definitivo.

Banco fisicamente compartilhado não significa acesso irrestrito. Cada módulo deve controlar logicamente seus dados e publicar operações para os consumidores. Essa disciplina impede que tabelas compartilhadas se transformem em uma interface informal e restaurem o acoplamento do V1.

Para Grades, os materiais propõem como evolução um repositório individual, possivelmente NoSQL, para separar consultas pesadas. Essa possibilidade não é uma decisão implementada. Antes da adoção, deve ser comprovada a compatibilidade com consistência, transações, histórico, auditoria, reconciliação com o GRA e recuperação.

## Conteúdos e CDN

Content mantém metadados e referências, o armazenamento dedicado conserva o arquivo e a CDN distribui o ativo. Retirar o binário do banco não o torna público: autorização, vínculo com o curso, exclusão e retenção continuam sob controle do Prometheus.

Essa estrutura atende ao requisito de carregamento inferior a dois segundos para apresentações e PDFs acessados fora da rede interna. Gravações e portfólios podem usar a mesma capacidade de armazenamento, respeitando políticas próprias.

## Diagnóstico e resposta de desempenho

O estudo da Semana 5 localiza dois gargalos principais no V1. A geração síncrona de relatórios compete por CPU, memória, conexões e operações de entrada e saída com a navegação comum. O download de BLOBs faz o banco transacional atuar também como servidor de arquivos e disputar recursos com usuários, cursos e notas. O material não comprova que bloqueios do banco sejam a causa específica; a conclusão segura é a existência de contenção em recursos compartilhados.

Para relatórios, a resposta combina cache seletivo, fila durável, múltiplos *workers* e armazenamento do resultado. A fila absorve variações de entrada e separa a velocidade das solicitações da capacidade de processamento. Vários *workers* oferecem paralelismo; adicionar instâncias amplia horizontalmente a capacidade. Para conteúdos, Object Storage mantém a origem persistente e a CDN distribui e armazena cópias próximas aos usuários.

Essas técnicas não comprovam desempenho ou disponibilidade isoladamente. Elas exigem políticas de invalidação, durabilidade da fila, autorização dos ativos, métricas e testes de carga com navegação concorrente.

## Alta disponibilidade e degradação controlada

A meta global de 99,9% equivale, como referência, a cerca de 43,2 minutos de indisponibilidade em 30 dias. A proposta conceitual combina redundância, balanceamento, health checks, failover e fallback; nenhum desses mecanismos garante a meta sozinho.

Grades e Videos são os componentes críticos analisados. Quando puderem ser executados em múltiplas instâncias, o modelo proposto é ativo/ativo atrás de um Load Balancer, que deixa de encaminhar tráfego a instâncias não saudáveis. A própria camada de balanceamento também precisa ser redundante ou fornecida por uma infraestrutura de alta disponibilidade. No monólito modular inicial, essa redundância pode exigir replicar a unidade de implantação; a escala isolada de uma API pressupõe uma evolução arquitetural explicitamente justificada.

Os *workers* de relatórios também podem ser redundantes sobre uma fila durável. Se um deles falhar, o trabalho não confirmado permanece disponível para processamento posterior. Caso Grades adquira persistência independente, a Semana 5 avalia uma topologia *primary/standby* com replicação e promoção da réplica. Essa opção não está fechada: RPO, RTO, consistência, perda admissível de dados e procedimento de reconexão ainda precisam ser definidos e testados.

Falhas externas exigem degradação elegante. Se o provedor de vídeo estiver indisponível, cursos, conteúdos, fóruns e notas continuam acessíveis, enquanto as operações dependentes da sala exibem indisponibilidade temporária. Se o GRA falhar, a nota confirmada localmente permanece registrada e o Grades Bridge tenta sincronizá-la posteriormente.

## Saúde e observabilidade

Componentes críticos devem expor verificações que distingam processo em execução de instância pronta para receber tráfego. O monitoramento deve acompanhar disponibilidade, latência, erros, CPU, memória, instâncias saudáveis, tamanho e idade das filas, duração dos relatórios e falhas das integrações externas.

Prometheus Monitoring, Grafana e Alertmanager aparecem no estudo como ferramentas possíveis para coleta, visualização e alertas. Esses produtos não foram selecionados como implementação definitiva. Uma verificação externa também deve medir a disponibilidade pela perspectiva do usuário, complementando as métricas internas. O nome **Prometheus Monitoring** deve ser usado quando necessário para evitar confusão com o LMS Prometheus V2.

## Modularidade interna

As atividades da Semana 4 refinam os cinco limites de alto nível em Identidade, Usuários, Cursos, Matrículas, Conteúdos, Avaliações e Notas e Comunicação. Esses nomes representam subdomínios e serviços internos, não novas implantações.

Alta coesão exige que cada módulo reúna regras relacionadas ao mesmo propósito. Baixo acoplamento exige que consumidores conheçam contratos, não detalhes internos. Encapsulamento significa que regras como validação de matrícula ou restrição de alteração permaneçam sob controle do domínio proprietário.

## Modernização incremental

A evolução começa pelo inventário e proteção dos dados do V1, estabelece os limites modulares, retira binários do banco, isola a carga de Grades e automatiza o GRA. Portais e novas capacidades entram sobre essa base. A migração de cada função deve possuir validação, período de convivência e caminho de retorno antes da retirada do legado.

## Referências acadêmicas consolidadas

- BASS, Len; CLEMENTS, Paul; KAZMAN, Rick. *Software Architecture in Practice*. 4. ed. Addison-Wesley, 2021.
- BROWN, Simon. *The C4 Model for Visualising Software Architecture*. c4model.com.
- FOWLER, Martin. *Patterns of Enterprise Application Architecture*. Addison-Wesley, 2002.
- MARTIN, Robert C. *Clean Architecture*. Pearson, 2017.
- NEWMAN, Sam. *Building Microservices*. 2. ed. O'Reilly Media, 2021.
- PARNAS, David L. On the Criteria To Be Used in Decomposing Systems into Modules. *Communications of the ACM*, 1972.
- RICHARDS, Mark; FORD, Neal. *Fundamentals of Software Architecture*. O'Reilly Media, 2020.
- RICHARDS, Mark; FORD, Neal. *Software Architecture: The Hard Parts*. O'Reilly Media, 2021.
- JALA UNIVERSITY. *Arquitetura de Software II — Projeto Capstone Prometheus V1/V2*. Material didático, 2026.
