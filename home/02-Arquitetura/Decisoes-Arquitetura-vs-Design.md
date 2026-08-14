# Decisões de Arquitetura vs. Design

## Distinção

Decisões arquiteturais definem estruturas de alto impacto, influenciam atributos de qualidade e são difíceis de reverter. Decisões de design detalham a implementação interna de um componente e podem ser alteradas com impacto mais localizado.

| Decisão | Classificação | Justificativa |
|---|---|---|
| Adotar monólito modular | Arquitetura | Define implantação, limites e estratégia de evolução. |
| Separar o sistema por domínios | Arquitetura | Altera responsabilidades e dependências principais. |
| Retirar binários do banco | Arquitetura | Modifica persistência e distribuição de conteúdo. |
| Sincronizar o GRA de forma assíncrona | Arquitetura | Determina integração, disponibilidade e consistência. |
| Processar relatórios em fila e *worker* | Arquitetura | Introduz componentes para desempenho e escala. |
| Reduzir estado local | Arquitetura | Habilita balanceamento e múltiplas instâncias. |
| Usar redundância ativa/ativa em componentes críticos | Arquitetura | Altera implantação, roteamento e tratamento de falhas. |
| Avaliar réplica *primary/standby* para Grades | Arquitetura | Afeta persistência, recuperação, consistência, RPO e RTO. |
| Definir endpoints de saúde e telemetria | Arquitetura | Sustenta roteamento, diagnóstico e resposta operacional. |
| Usar padrão Repository internamente | Design | Organiza o acesso aos dados de um componente. |
| Escolher linguagem ou biblioteca visual | Design | Não altera necessariamente os limites do sistema. |
| Nomear classes e métodos | Design | Afeta apenas a organização interna do código. |

Uma escolha pode mudar de classificação conforme seu alcance. Um repositório costuma ser design, mas uma abstração obrigatória para múltiplas fontes pode adquirir relevância arquitetural. Uma técnica de autenticação também se torna arquitetural se determinar a dependência de sessão local.

## Decisões consolidadas

### DA-01 — Modernização incremental

O V1 não será substituído de uma única vez. Dados e funções migram progressivamente, com validação e possibilidade de retorno.

### DA-02 — Monólito modular

O V2 mantém inicialmente implantação unificada, com módulos e contratos definidos. Microsserviços não são o estilo de referência.

### DA-03 — Decomposição por domínio

Users, Courses, Content, Grades e Videos formam os limites principais. Identidade, matrículas e comunicação refinam sua organização interna.

### DA-04 — Conteúdos fora do banco principal

Binários passam para armazenamento dedicado e CDN; o banco conserva metadados e referências.

### DA-05 — Isolamento da carga de Grades

PDFs são processados em segundo plano, e Grades é preparada para escala horizontal. Persistência dedicada continua como possibilidade condicionada.

### DA-06 — Integração automatizada com o GRA

Grades Bridge concentra REST, *webhooks* e adaptação. A sincronização de escrita ocorre de forma assíncrona.

### DA-07 — Videoconferência externa

O Prometheus administra contexto e acesso; uma plataforma especializada transmite áudio e vídeo.

### DA-08 — Redução de estado local

A aplicação deve evitar sessão exclusivamente na memória de uma instância. A tecnologia não foi selecionada.

### DA-09 — Assincronia seletiva

Eventos e filas são usados para relatórios, notificações, auditoria e integrações tolerantes a atraso. Operações acadêmicas que exigem confirmação permanecem síncronas no contexto principal.

### DA-10 — Disponibilidade seletiva e isolamento de falhas

Grades, Report Workers e Videos recebem prioridade na proposta de redundância. Instâncias preparadas para execução concorrente podem operar em modo ativo/ativo atrás de balanceamento com health checks. A falha de vídeo ou GRA deve limitar apenas a capacidade dependente, preservando o restante do LMS por fallback e processamento posterior.

Essa decisão descreve uma direção arquitetural. Ela não confirma que cada API já possua implantação independente nem transforma automaticamente o monólito modular em microsserviços.

### DA-11 — Observabilidade orientada à operação

Disponibilidade e fluxos assíncronos devem ser avaliados por métricas, logs, alertas, saúde das instâncias e verificações externas. Prometheus Monitoring, Grafana e Alertmanager são opções estudadas, não produtos contratados ou implementados.

## Pontos ainda abertos

SGBD do V2, produto de mensageria, autenticação, cache, fornecedor de vídeo, Load Balancer, plataforma de observabilidade e persistência especializada para Grades não são decisões fechadas. RPO e RTO da persistência acadêmica também precisam ser definidos. JWT, Redis, OAuth/OpenID Connect, NoSQL, 100ms.live, Prometheus Monitoring, Grafana e Alertmanager aparecem nos estudos apenas como exemplos ou técnicas avaliadas.
