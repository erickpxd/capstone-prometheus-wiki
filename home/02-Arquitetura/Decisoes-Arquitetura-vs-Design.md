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

## Pontos ainda abertos

SGBD do V2, produto de mensageria, autenticação, cache, fornecedor de vídeo e persistência especializada para Grades não são decisões fechadas. JWT, Redis, OAuth/OpenID Connect, NoSQL e 100ms.live aparecem nos estudos apenas como exemplos ou técnicas avaliadas.
