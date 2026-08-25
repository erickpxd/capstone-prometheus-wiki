# Requisitos Funcionais

## Introdução e convenção

Os requisitos funcionais do Prometheus V2 combinam dois conjuntos. O primeiro reúne funções já oferecidas pelo Prometheus V1 e objetivos estabelecidos desde o início de sua modernização. O segundo contém os doze requisitos adicionais do material mais recente.

Para preservar a numeração original, esta página utiliza `RF-BASE-XX` para capacidades herdadas ou objetivos iniciais e `RF01–RF12` para os novos requisitos oficialmente numerados. A preservação de dados aparece como requisito-base de transição: ela orienta a modernização, mas não significa que a migração já tenha sido executada.

## Requisitos herdados e objetivos originais

### RF-BASE-01 — Gestão de usuários

O sistema deve permitir que administradores realizem a gestão de estudantes, professores, administradores e demais perfis autorizados. Essa capacidade abrange o ciclo de vida cadastral dos usuários, respeitando as permissões de cada papel.

**Ator principal:** Administrador.

### RF-BASE-02 — Gestão de cursos

O sistema deve permitir criação, consulta, edição e administração dos cursos disponibilizados pela School X. Professores devem atuar somente nos cursos e turmas para os quais possuam autorização.

**Atores principais:** Administrador e Professor.

### RF-BASE-03 — Gestão e entrega de conteúdos

O sistema deve permitir que professores disponibilizem materiais relacionados aos cursos e que estudantes autorizados consultem ou realizem o download desses conteúdos.

**Atores principais:** Professor e Estudante.

**Referência arquitetural:** a persistência e a distribuição dos arquivos são tratadas em [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).

### RF-BASE-04 — Gestão de notas e relatórios

O sistema deve permitir o registro, o gerenciamento e a consulta das notas dos estudantes, além da geração de relatórios acadêmicos. Lançamento e alteração pertencem ao professor autorizado; estudantes e responsáveis consultam somente os resultados permitidos para seus vínculos.

**Atores principais:** Professor, Estudante e, para consulta limitada, Pais ou responsáveis.

### RF-BASE-05 — Migração e conservação dos dados existentes

O processo de modernização deve preservar os dados acumulados no Prometheus V1. A evolução arquitetural não pode resultar na perda de usuários, cursos, vínculos, notas, históricos ou referências de conteúdo. A execução da migração continua sujeita ao planejamento, à validação e ao mecanismo de retorno definidos para cada etapa.

**Atores principais:** Sistema e Administrador.

### RF-BASE-06 — Avaliações on-line

O sistema deve permitir que professores criem e publiquem avaliações on-line e que estudantes autorizados realizem essas avaliações por meio do Prometheus.

**Atores principais:** Professor e Estudante.

### RF-BASE-07 — Fóruns de discussão

O sistema deve disponibilizar fóruns relacionados aos cursos para promover discussões e interação acadêmica entre professores e estudantes.

**Atores principais:** Professor e Estudante.

### RF-BASE-08 — Videoconferências

O sistema deve permitir o agendamento e a participação em sessões de videoconferência associadas aos cursos.

**Atores principais:** Professor e Estudante.

**Referência arquitetural:** a divisão de responsabilidades e a integração necessária são tratadas em [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).

### RF-BASE-09 — Acesso web externo

O Prometheus V2 deve permitir acesso web aos usuários autorizados fora da rede interna da School X, eliminando a restrição operacional do V1 sem reduzir os controles de segurança.

**Atores principais:** Estudante, Professor, Administrador e Pais ou responsáveis.

### RF-BASE-10 — Portal de Pais e consulta de notas

O sistema deve disponibilizar um portal com acesso limitado para que pais ou responsáveis consultem as notas dos estudantes a eles vinculados. O vínculo deve ser validado pelo sistema antes de qualquer exposição de informação acadêmica.

**Atores principais:** Pais ou responsáveis.

### RF-BASE-11 — Integração com o sistema GRA

O Prometheus deve integrar-se automaticamente ao GRA para eliminar a transcrição manual de notas e reduzir atraso e erro operacional.

**Atores principais:** Professor e Sistema.

**Referência arquitetural:** os contratos e o fluxo de sincronização são tratados em [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes); o comportamento de desacoplamento permanece definido no [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais).

### RF-BASE-12 — Autenticação e controle por perfil

Todo usuário deve ser autenticado antes de acessar funções protegidas. Após a autenticação, o sistema deve disponibilizar o portal e as operações correspondentes ao papel de Administrador, Professor, Estudante ou Pai/Responsável. A autenticação não substitui a autorização específica sobre cursos, estudantes e conteúdos.

**Atores principais:** todos os usuários autenticáveis.

## Requisitos funcionais adicionais

### RF01 — Notificações *push* e mobile

O sistema deve enviar notificações em tempo real para estudantes e pais ou responsáveis sobre novas notas publicadas, prazos de tarefas próximos e novas mensagens em fóruns. A entrega da notificação complementa o registro do evento no Prometheus e não deve alterar a responsabilidade do módulo que originou a informação.

**Atores principais:** Estudante e Pais ou responsáveis.

**Áreas relacionadas:** Notificações, Courses, Grades e Fóruns.

### RF02 — Ambiente de avaliação assistida (*proctoring*)

Durante avaliações on-line, o sistema deve detectar troca de aba e perda de foco na janela da prova. Os eventos detectados devem ser associados à sessão de avaliação do estudante para posterior consulta pelo professor.

**Atores principais:** Estudante e Professor.

**Áreas relacionadas:** Avaliações e monitoramento da sessão.

### RF03 — *Dashboard* analítico para professores

O sistema deve oferecer aos professores uma interface visual para acompanhar a evolução do desempenho da turma. O *dashboard* deve identificar estudantes abaixo da média, apoiando intervenções pedagógicas antecipadas, e limitar os dados às turmas sob responsabilidade do professor.

**Ator principal:** Professor.

**Áreas relacionadas:** Grades, análise acadêmica e Courses.

### RF04 — Repositório de aulas gravadas

O sistema deve integrar-se ao serviço de videoconferência para disponibilizar automaticamente as gravações após o encerramento das sessões. As gravações devem aparecer no portal dos estudantes autorizados e permanecer associadas ao curso correspondente.

**Atores principais:** Professor e Estudante.

**Áreas relacionadas:** Videos, Content e armazenamento.

### RF05 — Assinatura digital de relatórios

O sistema deve gerar relatórios de notas em PDF contendo QR Code de autenticidade. O mecanismo deve permitir verificar a procedência do documento, reduzindo a necessidade de validação manual pela secretaria.

**Atores principais:** Professor, Estudante e Administração.

**Áreas relacionadas:** Grades e relatórios.

### RF06 — Sistema de gamificação

O sistema deve conceder automaticamente conquistas ou medalhas quando o estudante concluir módulos de cursos ou obtiver nota superior a 90% em avaliações on-line.

**Ator principal:** Estudante.

**Áreas relacionadas:** Gamificação, Courses e Avaliações.

### RF07 — Agendamento automático de videoconferências

O sistema deve permitir que professores agendem sessões integradas ao calendário do curso. Após o agendamento, o Prometheus deve solicitar à plataforma externa a criação do link de acesso e disponibilizá-lo aos participantes autorizados.

**Atores principais:** Professor e Estudante.

**Áreas relacionadas:** Videos e Courses.

### RF08 — Repositório de portfólio do estudante

O sistema deve oferecer espaço para upload de projetos e trabalhos autorais independentes das tarefas convencionais dos cursos. Professores e pais ou responsáveis autorizados devem poder acompanhar o desenvolvimento prático do estudante além de suas notas.

**Atores principais:** Estudante, Professor e Pais ou responsáveis.

**Áreas relacionadas:** Content, Portfólio e armazenamento.

### RF09 — Mural de avisos dinâmico

O sistema deve apresentar nos portais um feed de anúncios administrativos, atualizações de cursos e informações relevantes ao perfil autenticado. O recurso deve estar disponível, no mínimo, nos portais de Estudante, Professor e Pais.

**Atores principais:** Estudante, Professor e Pais ou responsáveis.

**Áreas relacionadas:** Comunicação e Courses.

### RF10 — Recuperação automática de senha

O sistema deve oferecer a função “Esqueci minha senha”, permitindo recuperação por e-mail ou SMS sem intervenção manual do administrador.

**Atores principais:** todos os usuários autenticáveis.

**Áreas relacionadas:** Users, autenticação e segurança.

### RF11 — Exportação de dados para professores

O sistema deve permitir que professores exportem listas de presença e médias de desempenho das turmas em CSV ou Excel. A exportação complementa os relatórios em PDF e deve respeitar o escopo de cursos e turmas autorizado ao professor.

**Ator principal:** Professor.

**Áreas relacionadas:** Grades, relatórios e Courses.

### RF12 — Central de mensagens interna

O sistema deve disponibilizar comunicação entre professores e estudantes por chat ou mensagens assíncronas relacionadas aos conteúdos dos cursos. O objetivo é centralizar a comunicação acadêmica dentro do Prometheus e reduzir a dependência de redes sociais externas.

**Atores principais:** Professor e Estudante.

**Áreas relacionadas:** Comunicação e Courses.

## Rastreabilidade

A associação inicial entre os requisitos, domínios e atributos de qualidade está registrada na página de [Priorização](/home/01-Requisitos/Priorizacao). Essa associação orienta a decomposição, mas não cria automaticamente novos microsserviços para cada responsabilidade.

Referências relacionadas:

- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais);
- [Priorização](/home/01-Requisitos/Priorizacao);
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades);
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes);
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios);
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais);
- [Architecture Decision Records](/home/02-Arquitetura/ADRs).
