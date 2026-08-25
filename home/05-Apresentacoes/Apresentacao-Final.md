# Apresentação Final — Prometheus V2

> Documento de preparação da apresentação oral do Projeto Capstone Prometheus V2.\
> A arquitetura descrita é proposta e vigente na documentação; a implementação comprovada limita-se ao recorte referencial de Users/Identity.

# 1. Resumo executivo

O Prometheus V1 é um LMS utilizado pela School X há aproximadamente quinze anos. Ele reúne páginas ASP clássico, lógica de negócio concentrada em uma DLL C#, banco MySQL centralizado, arquivos binários armazenados como BLOBs, sessão mantida no servidor, relatórios PDF síncronos e integração manual de notas com o GRA. O acervo histórico impede uma substituição imediata sem risco.

O diagnóstico mostrou que o problema não era apenas a idade das tecnologias. Responsabilidades, dados e processamento estavam acoplados: conteúdos, relatórios, usuários e cursos competiam pelos mesmos recursos; mudanças possuíam impacto amplo; o estado local dificultava múltiplas instâncias; e falhas compartilhadas podiam atingir diversas funções.

O Prometheus V2 foi proposto como um **monólito modular inicialmente implantado como uma unidade**, modernizado de forma incremental. Users, Courses, Content, Grades e Videos são módulos lógicos com responsabilidades e contratos explícitos. Módulo, API REST, bounded context, microsserviço e unidade de implantação não são sinônimos.

As principais respostas arquitetônicas são:

- retirar binários do banco transacional e usar as abstrações Object Storage e CDN;
- processar relatórios PDF de forma assíncrona por registro de trabalho, fila e Report Workers;
- integrar o GRA por Grades Bridge, sem bloquear a confirmação local de notas;
- delegar a transmissão de vídeo a um provedor externo, mantendo o contexto acadêmico no Prometheus;
- reduzir a dependência de estado local;
- aplicar assincronia somente aos efeitos tolerantes a atraso.

Tecnologias como SGBD definitivo, Message Broker, cache, Identity Provider, Video Provider, observabilidade e plataforma de implantação continuam abertas. Redis, RabbitMQ, MongoDB, Kubernetes e fornecedores estudados não são decisões vigentes.

A implementação realmente desenvolvida é um recorte fullstack de **Users/Identity**: backend ASP.NET Core/.NET 8, frontend React/TypeScript, persistência SQLite, senhas com BCrypt, autenticação JWT e autorização por roles Admin, Professor e Aluno. Ela comprova contratos, autenticação e autorização nesse recorte; não comprova o LMS completo nem transforma Users em microsserviço.

## Regra de interpretação durante a apresentação

| Categoria | Como apresentar |
|-----------|-----------------|
| Fato do V1 | Característica ou problema documentado do legado |
| Arquitetura proposta | Direção vigente na documentação, sem afirmar implantação |
| Decisão arquitetônica | Escolha registrada por DA/ADR e suas consequências |
| Tecnologia aberta | Produto, fornecedor ou mecanismo ainda não selecionado |
| Evolução condicionada | Possibilidade futura dependente de métricas e nova decisão |
| Implementação referencial | Código realmente existente no recorte Users/Identity |

## Diagnóstico em uma frase

> **O problema central do Prometheus V1 não era somente a tecnologia antiga, mas o acoplamento entre responsabilidades, dados e processamento, que fazia mudanças e picos de carga se propagarem por todo o sistema.**

# 2. História da apresentação

A apresentação deve seguir uma narrativa de causa e consequência:

1. **Problema:** o V1 preserva quinze anos de dados, mas possui responsabilidades misturadas, banco sobrecarregado, relatórios síncronos, sessão local e integrações manuais.
2. **Análise:** os sintomas apontam para baixa coesão, alto acoplamento, contenção de recursos e dificuldade de evolução.
3. **Decisão:** modernizar incrementalmente e adotar um monólito modular, evitando a distribuição prematura.
4. **Arquitetura:** estabelecer Users, Courses, Content, Grades e Videos como módulos lógicos, com propriedade de regras e dados.
5. **Qualidade:** retirar binários do banco, isolar relatórios, desacoplar o GRA, degradar vídeo de forma controlada e reduzir estado local.
6. **DDD:** modelar contextos e linguagem do negócio sem confundir bounded context com microsserviço.
7. **Trade-offs:** aceitar implantação conjunta, consistência posterior e novas responsabilidades operacionais em troca de simplicidade, isolamento e evolução gradual.
8. **Implementação:** demonstrar Users/Identity com autenticação e autorização reais.
9. **Aprendizado:** arquitetura é decidir limites e compromissos com base em atributos de qualidade, não apenas escolher produtos.

# 3. Estrutura da apresentação

| Parte | Assunto | Mensagem principal |
|-------|---------|--------------------|
| A | Análise arquitetônica do V1 | O acoplamento estrutural ampliava o impacto de mudanças, cargas e falhas |
| B | Princípios arquitetônicos | Separar responsabilidades e proteger os limites antes de distribuir |
| C | Arquitetura do V2 | Monólito modular, implantação inicial unificada e integrações por abstrações |
| D | Desempenho, escala e resiliência | Isolar cargas pesadas e falhas externas com assincronia seletiva |
| E | DDD e negócio | Contextos possuem linguagem e regras próprias; não implicam microsserviços |
| F | Implementação referencial | Users/Identity foi implementado como prova de conceito fullstack |
| G | Decisões e trade-offs | Cada benefício arquitetônico possui custo, risco ou limitação |
| H | Conclusões pessoais | Métricas e limites claros devem preceder tecnologia e distribuição |

## Visuais existentes recomendados

Usar apenas os visuais que ajudam a narrativa:

1. **C4 Nível 1 — Contexto:** pessoas, Prometheus V2 e dependências externas.
2. **C4 Nível 2 — Contêineres:** uma aplicação modular, persistência, abstrações de infraestrutura e sistemas externos.
3. **Fluxo assíncrono de relatórios:** requisição até consulta posterior.
4. **Screenshot do Portal Web:** somente durante a explicação ou demonstração da implementação.

O mapa DDD completo pode ficar como material de apoio. Na fala principal, uma tabela curta de contextos é mais clara.

# 4. Conteúdo técnico da apresentação

## Parte A — Análise arquitetônica do Prometheus V1

### Sistema base

| Elemento | Situação documentada no V1 |
|----------|----------------------------|
| Aplicação | Aplicação web monolítica e pouco modular |
| Interface | Páginas ASP clássico |
| Negócio | Lógica concentrada em biblioteca/DLL C# |
| Persistência | MySQL centralizado para dados relacionais e BLOBs |
| Estado | Sessão mantida no servidor |
| Relatórios | PDF gerado de forma síncrona no fluxo web |
| Integração GRA | Transcrição manual de notas |
| Acesso | Originalmente restrito à rede interna |

### Principais problemas

#### Modularidade

- alto acoplamento entre responsabilidades;
- baixa coesão dentro dos agrupamentos existentes;
- acesso direto a estruturas internas;
- mudança em uma área com risco de impacto em outras;
- dificuldade para testar e evoluir partes isoladamente.

#### Desempenho

- geração síncrona de PDF compete com a navegação;
- BLOBs fazem o banco atender consultas relacionais e transferência de arquivos grandes;
- relatórios, conteúdos e operações comuns disputam CPU, memória, I/O e conexões;
- não há métricas documentadas do V1 que autorizem inventar throughput ou latências específicas.

#### Escalabilidade

- forte dependência de escala vertical;
- sessão local dificulta balanceamento e recuperação em outra instância;
- banco central concentra carga;
- a arquitetura não permite ampliar facilmente apenas a capacidade mais exigida.

#### Disponibilidade

- componentes compartilhados ampliam o raio de impacto das falhas;
- degradação de carga pesada pode afetar funções não relacionadas;
- topologia e evidência operacional do V1 não permitem prometer números não medidos.

#### Integrações

- notas são transcritas manualmente para o GRA;
- o processo aumenta esforço, atraso e risco de erro;
- uma integração futura precisa evitar que a indisponibilidade externa bloqueie a operação local.

## Parte B — Princípios arquitetônicos aplicados

| Princípio | Aplicação no Prometheus V2 |
|-----------|----------------------------|
| Separation of Concerns — SoC | Separa Users, Courses, Content, Grades, Videos e adaptadores externos |
| SRP | Cada módulo possui uma razão principal para mudar e controla seu conjunto de regras |
| Encapsulamento | Grades não altera diretamente Matrículas; usa um contrato para validar o vínculo |
| Baixo acoplamento | Módulos colaboram por contratos explícitos, não por acesso irrestrito a tabelas e estruturas |
| Alta coesão | Regras de avaliações, notas, publicação e relatórios permanecem próximas em Grades |
| DIP | GRA, Video Provider e armazenamento são acessados por adaptadores e abstrações |
| DRY | Regras acadêmicas e de autorização não devem ser reproduzidas em vários módulos |

### Por que monólito modular?

A escolha busca corrigir o acoplamento do legado sem introduzir imediatamente a complexidade de um sistema distribuído. A migração precisa preservar aproximadamente quinze anos de dados e permitir validação e retorno em etapas menores.

**Benefícios:**

- menor complexidade de implantação e operação inicial;
- modernização gradual;
- limites lógicos e contratos explícitos;
- preservação do acervo e possibilidade de convivência;
- base para uma extração futura orientada por evidência.

**Custos:**

- implantação inicialmente conjunta;
- escala independente limitada;
- falha da unidade pode possuir raio de impacto maior;
- exige disciplina para impedir dependências internas indevidas.

> **Regra para falar:** módulo lógico não é microsserviço. Uma extração futura só faz sentido quando métricas de carga, disponibilidade, ritmo de mudança ou custo operacional justificarem a distribuição.

## Parte C — Arquitetura proposta do Prometheus V2

### C4 Nível 1 — Contexto

O C4 Nível 1 mostra o Prometheus V2 como **um único sistema**. Seus atores são Administrador, Professor, Estudante e Pai ou Responsável. As principais dependências são:

- **GRA:** sistema oficial externo para notas, com REST e webhooks;
- **Video Provider:** transmissão externa de áudio e vídeo, com fornecedor ainda aberto;
- **Object Storage/CDN:** infraestrutura necessária para armazenamento e entrega de conteúdo.

Banco SQL, módulos, fila e workers não devem ser explicados nesse nível porque são detalhes internos.

### C4 Nível 2 — Contêineres

O C4 Nível 2 possui um contêiner principal de aplicação: a **aplicação modular inicialmente implantada como uma unidade**. Dentro dela existem módulos e responsabilidades lógicas. Banco SQL, fila, armazenamento e CDN são apresentados por função, sem produto escolhido.

| Módulo lógico | Responsabilidade principal |
|---------------|----------------------------|
| Users | Usuários, perfis, ciclo cadastral, autenticação e autorização |
| Courses | Cursos, turmas, períodos, calendário e contexto dos vínculos acadêmicos |
| Content | Metadados, autorização, associação ao curso e ciclo de vida de materiais |
| Grades | Avaliações, validação, notas, cálculo, publicação, histórico, relatórios e exportações |
| Videos | Agendamento, autorização, links, contexto acadêmico e associação de gravações |

Responsabilidades internas complementares:

- **Identidade:** autenticação e autorização, com tecnologia arquitetural ainda aberta;
- **Matrículas:** vínculo estudante–curso–turma, associado a Courses;
- **Comunicação:** fóruns, avisos, notificações e mensagens, com fronteira final ainda aberta;
- **Grades Bridge:** adaptador/ACL para o GRA;
- **Report Workers:** responsabilidade de processamento de PDFs.

### Dados e integrações

#### Banco SQL principal

A referência vigente é uma base relacional inicialmente compartilhada fisicamente. Cada módulo, entretanto, possui **propriedade lógica** sobre suas regras e seus dados. Compartilhar o banco não autoriza acesso direto às estruturas internas de outro módulo e não constitui automaticamente Shared Kernel.

O SGBD do V2 ainda não foi selecionado. MySQL é fato do V1; SQLite pertence apenas à implementação referencial; PostgreSQL ou outras alternativas estudadas não são decisões.

#### Object Storage e CDN

Content mantém metadados, referências, associação acadêmica e autorização. O arquivo binário fica em Object Storage. Uma CDN distribui conteúdos apropriados a partir dessa origem. Fornecedor, região, política de expiração e forma de acesso continuam abertos.

Retirar o arquivo do banco não o torna público. Autenticação, autorização, retenção, exclusão e consistência metadado–objeto continuam necessárias.

#### Grades Bridge e GRA

Grades Bridge isola o modelo de Grades do contrato externo do GRA, exercendo o papel de Anti-Corruption Layer. A nota é validada e registrada localmente antes da sincronização assíncrona. Idempotência, auditoria, tentativas controladas e reconciliação são comportamentos requeridos.

Fonte de autoridade, regras de conflito, autenticação, detalhes de webhooks e produto de mensageria continuam abertos.

#### Video Provider

O Prometheus mantém o contexto acadêmico: agenda, participantes, autorização, links e associação de gravações. A transmissão de mídia é delegada. O fornecedor e o SLA continuam abertos.

## Parte D — Escalabilidade, desempenho e resiliência

### Relatórios PDF

**Fluxo no V1:**

> requisição → processamento completo → geração do PDF → resposta

A requisição permanece ocupada durante a carga pesada e compete com a navegação.

**Fluxo proposto no V2:**

> requisição → registro do trabalho → fila → Report Workers → geração do PDF → armazenamento do resultado → consulta ou download posterior

Estados conceituais:

| Estado | Significado |
|--------|-------------|
| Pendente | Trabalho registrado e aguardando processamento |
| Em processamento | Um Report Worker assumiu o trabalho |
| Concluído | PDF armazenado e referência disponível |
| Falha controlada | Estado e diagnóstico preservados para tratamento ou nova tentativa |

O fluxo precisa ser idempotente. Reentrega ou repetição não pode produzir duplicação visível nem associar um resultado incorreto ao usuário. Falha do worker não pode ser apresentada como sucesso.

O RNF05 exige até **500 PDFs simultâneos sem degradação significativa da navegação**. Fila e workers são a resposta arquitetônica, não a prova do requisito. Ambiente, duração, combinação de relatórios, quantidade de workers e limite de degradação ainda precisam ser medidos.

### Conteúdos e CDN

O RNF03 exige conteúdo externo em menos de dois segundos. A resposta proposta é:

> metadados em Content + arquivo em Object Storage + distribuição apropriada por CDN

A meta existe, mas o cenário de aceitação ainda precisa definir tamanho e tipo de arquivo, região, rede, percentil e estado do cache.

### Estratégias de escala

**Vertical:** pode continuar sendo usada inicialmente para ampliar recursos da unidade de implantação.

**Horizontal:** a arquitetura é preparada por redução de estado local, armazenamento fora do banco, processamento em workers e separação lógica de Grades. Isso não significa que Grades já esteja em múltiplas instâncias ou possua banco próprio.

Persistência exclusiva de Grades, réplicas, NoSQL e implantação independente são evoluções futuras condicionadas a métricas, consistência, migração, auditoria, recuperação e custo.

### Resiliência do GRA

Se o GRA estiver lento ou indisponível:

1. a nota deve ser validada e registrada localmente;
2. o professor recebe a confirmação da operação principal;
3. a sincronização permanece pendente;
4. novas tentativas são controladas;
5. auditoria e reconciliação tratam divergências;
6. repetição não pode duplicar notas.

Esse fluxo aceita consistência posterior entre Prometheus e GRA em troca de disponibilidade da operação local.

### Resiliência do vídeo

Uma falha no Video Provider pode indisponibilizar a sessão de vídeo, mas não deve derrubar cursos, conteúdos ou notas. Esse comportamento é **graceful degradation**: limitar o impacto à capacidade dependente.

### Padrões estudados

Retry, Timeout, Circuit Breaker, Bulkhead e Fallback foram estudados como padrões aplicáveis. Biblioteca, composição, limiares e parâmetros dependem de telemetria e do contrato externo. Não afirmar que todos estão implementados.

### Disponibilidade

O RNF01 define meta de **99,9%**. Em trinta dias, 0,1% representa aproximadamente 43,2 minutos, mas a aceitação ainda depende da janela, das funções incluídas, do SLI, das exclusões e da fonte de medição.

Isolamento de falhas, verificação de saúde, observabilidade, degradação controlada e redundância seletiva são respostas ou capacidades necessárias. Topologias ativa/ativa, primary/standby, failover e Load Balancer específico ainda não foram decididos.

### Atributos de qualidade

| Atributo | Principal resposta arquitetônica | Limite da afirmação |
|----------|----------------------------------|---------------------|
| Segurança | Autenticação, autorização contextual, criptografia e MFA exigido para o Portal de Pais | Provedor, protocolos, sessão e MFA não estão implementados no LMS completo |
| Desempenho | Object Storage/CDN e relatórios assíncronos | RNF03 e RNF05 ainda exigem cenários completos e teste |
| Escalabilidade | Modularidade, redução de estado e isolamento de cargas | Escala isolada de Grades não está decidida |
| Disponibilidade | Isolamento, saúde, observabilidade e degradação | Topologia e evidência de 99,9% permanecem abertas |
| Manutenibilidade | Monólito modular, contratos e propriedade lógica | Governança dos limites ainda precisa de critério verificável |
| Resiliência | Assincronia seletiva e tratamento de falhas externas | Produtos e políticas concretas ainda não foram escolhidos |

## Parte E — DDD e regras de negócio

### Classificação estratégica

A classificação DDD é uma **hipótese acadêmica a validar com especialistas da School X**.

| Contexto candidato | Classificação proposta | Justificativa |
|--------------------|------------------------|---------------|
| Oferta Acadêmica e Matrículas | Core | Define cursos, turmas, períodos, matrícula e elegibilidade |
| Avaliações e Resultados Acadêmicos | Core | Concentra avaliações, notas, publicação, histórico e resultados oficiais |
| Pessoas e Perfis | Supporting | Apoia os fluxos acadêmicos com pessoas, perfis e vínculos |
| Conteúdo Didático | Supporting | Organiza materiais e seu ciclo de vida |
| Comunicação Acadêmica | Supporting | Apoia fóruns, avisos e mensagens no contexto do curso |
| Sessões ao Vivo | Supporting | Apoia sessões vinculadas ao curso |
| Identidade e Acesso | Generic | Capacidade comum de autenticação; autorização contextual permanece no domínio |
| Entrega de Notificações | Generic | Entrega eventos por canais externos sem possuir a regra acadêmica de origem |

Para a apresentação, **Grades/Avaliações e Resultados** é o melhor exemplo de Core Domain porque concentra regras acadêmicas críticas e a integração com o sistema oficial de notas. A documentação também classifica Oferta Acadêmica e Matrículas como hipótese Core; não esconder essa nuance.

### Bounded contexts

Cada contexto possui linguagem, regras, responsabilidade e propriedade lógica de dados. Um bounded context pode permanecer dentro do mesmo monólito. Ele não implica API REST, banco separado, equipe própria ou microsserviço.

Os limites arquitetônicos principais continuam Users/Identity, Courses, Content, Grades e Videos. A hipótese DDD refina esses limites, mas não os substitui automaticamente.

### Exemplo de modelagem: Grades

Termos da linguagem:

- Student/Estudante;
- Enrollment/Matrícula;
- Assessment/Avaliação;
- Grade/Nota;
- Grade Publication/Publicação da nota;
- Report/Relatório.

Regras sustentadas pela documentação:

1. operações acadêmicas dependentes de participação validam o vínculo de matrícula;
2. somente o professor autorizado registra ou altera notas no escopo permitido;
3. a nota é validada e registrada localmente antes da sincronização com o GRA;
4. publicação e consulta respeitam papel, vínculo e autorização;
5. relatórios usam dados acadêmicos autorizados e mantêm autoria e rastreabilidade.

### Linguagem ubíqua

Os mesmos termos precisam manter significado consistente em requisitos, arquitetura, código e conversas:

- **Matrícula:** vínculo entre estudante, curso e turma;
- **Avaliação:** atividade acadêmica que produz resultado;
- **Nota:** resultado validado no contexto de Grades;
- **Publicação:** disponibilização autorizada do resultado;
- **Relatório:** documento acadêmico gerado a partir de dados autorizados.

## Parte F — Implementação referencial

### Escopo realmente desenvolvido

A implementação cobre somente Users/Identity e o Portal Web necessário para demonstrá-lo.

| Camada | Implementação existente |
|--------|-------------------------|
| Backend | C#, .NET 8 e ASP.NET Core Web API |
| Persistência | Entity Framework Core, migrations e SQLite |
| Segurança | JWT assinado, validação de issuer/audience/expiração, BCrypt e roles |
| Frontend | React, TypeScript, Vite, React Router, Axios e CSS |
| Perfis | Admin, Professor e Aluno |
| Testes | Suíte versionada com 15 cenários de integração |

### Comportamentos comprovados no recorte

- login com emissão de JWT;
- restauração da sessão por `GET /api/auth/me`;
- rotas e endpoints protegidos por perfil;
- Admin lista usuários;
- Admin cadastra Professor ou Aluno;
- cadastro de outro Admin pelo fluxo comum é rejeitado;
- e-mail duplicado é tratado como conflito;
- senhas são armazenadas como hash BCrypt;
- DTOs não expõem PasswordHash;
- respostas distinguem 401 de 403;
- frontend redireciona perfil incompatível;
- backend mantém a autorização efetiva.

### Limitações deliberadas

- SQLite é escolha da prova de conceito, não o SGBD definitivo;
- JWT é mecanismo da implementação, não a seleção final de Identity Provider;
- token em `localStorage` é didático e aumenta o impacto de XSS;
- não há refresh token, recuperação de senha, MFA ou cadastro público;
- Courses, Content, Grades e Videos não estão implementados;
- não há relatórios, GRA, Object Storage, CDN, vídeo ou observabilidade de produção;
- não há evidência de implantação do LMS completo.

### Relação implementação × arquitetura

O diretório `users-api` representa um limite de código e um contrato HTTP. Ele não prova processo independente, banco próprio ou microsserviço. A implementação mostra que autenticação, autorização, roles e contratos podem respeitar o limite lógico Users/Identity dentro da visão de monólito modular.

## Parte G — Decisões e trade-offs

### Decisões principais para apresentar

| Decisão | Motivo | Consequência principal |
|---------|--------|------------------------|
| DA-01 — Modernização incremental | Preservar quinze anos de dados e reduzir risco de substituição total | Convivência e migração ficam mais complexas |
| DA-02 — Monólito modular | Obter limites sem custo distribuído prematuro | Implantação e escala inicialmente compartilhadas |
| DA-04 — Binários fora do banco | Reduzir competição entre dados relacionais e arquivos | Exige consistência metadado–objeto e migração dos BLOBs |
| DA-05 — Relatórios assíncronos | Isolar geração pesada da navegação | Resultado posterior, estados e operação adicional |
| DA-06 — Grades Bridge | Automatizar o GRA sem bloquear a nota local | Consistência posterior e reconciliação |
| DA-07 — Vídeo externo | Evitar construir transmissão especializada | Dependência de contrato, SLA e disponibilidade externa |
| DA-08 — Redução de estado local | Preparar balanceamento e recuperação por outra execução | Sessão, revogação e segurança precisam de solução própria |
| DA-09 — Assincronia seletiva | Desacoplar efeitos tolerantes a atraso | Maior necessidade de idempotência e observabilidade |

### Trade-offs prioritários

| Tensão | Ganho | Custo aceito |
|--------|-------|--------------|
| Monólito modular × microsserviços | Simplicidade operacional e migração gradual | Menor independência de implantação e escala |
| Assincronia × imediatismo | Isolamento, responsividade e resiliência | Resultado posterior e consistência eventual |
| Object Storage × banco único | Menos carga no banco e melhor distribuição | Consistência entre metadado e arquivo |
| Provedor externo × controle interno | Capacidade especializada e foco no domínio | Dependência externa e necessidade de degradação |
| Segurança × usabilidade | Proteção de dados e escopo | Mais controles, fricção e complexidade de sessão |

### Decisões ainda abertas

- SGBD definitivo;
- Message Broker e garantias de entrega;
- uso, escopo, TTL e invalidação de cache;
- Identity Provider, protocolos e mecanismo de sessão;
- fornecedor e SLA de vídeo;
- plataforma e modelo operacional de observabilidade;
- Load Balancer e topologia de alta disponibilidade;
- RPO, RTO, backup e recuperação de desastre;
- persistência especializada, réplicas ou implantação futura de Grades;
- parâmetros de Retry, Timeout, Circuit Breaker, Bulkhead e Fallback.

Uma decisão permanecer aberta não é falha. É uma forma de evitar compromisso prematuro sem requisitos, métricas ou alternativas suficientes.

## Parte H — Síntese e aprendizados

O projeto não procurou distribuir o sistema por tendência. Primeiro definiu limites, responsabilidades, atributos de qualidade e decisões reversíveis. A arquitetura prepara evolução, mas exige métricas antes de aprovar extrações, replicação ou produtos.

As principais lições são:

- tecnologia não substitui limite de domínio;
- modularidade depende de propriedade e contratos;
- disponibilidade exige medição e operação, não apenas componentes;
- assincronia melhora isolamento, mas cria estados e consistência posterior;
- toda decisão possui benefício, custo e risco;
- uma prova de conceito deve ser apresentada pelo que comprova, não pelo sistema inteiro.

# 