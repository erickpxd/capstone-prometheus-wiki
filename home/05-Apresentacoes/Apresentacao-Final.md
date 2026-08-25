# Apresentação Final — Prometheus V2

> Documento de preparação da apresentação oral do Projeto Capstone Prometheus V2.  
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
|---|---|
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
|---|---|---|
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
|---|---|
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
|---|---|
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
|---|---|
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
|---|---|
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
|---|---|---|
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
|---|---|---|
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
|---|---|
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
|---|---|---|
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
|---|---|---|
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

# 5. Roteiro oral completo

## Abertura

“O Prometheus é o LMS usado pela School X há aproximadamente quinze anos. O sistema acumulou dados muito valiosos, mas também limitações estruturais. Meu objetivo não foi simplesmente trocar tecnologias. Eu analisei onde responsabilidades, dados e processamento estavam acoplados e propus uma evolução que preserva o legado enquanto cria limites claros.”

## Prometheus V1

“O V1 é uma aplicação web monolítica com páginas ASP clássico, lógica concentrada em uma DLL C# e MySQL centralizado. O mesmo banco atende dados relacionais e arquivos binários. A sessão fica no servidor, relatórios PDF são gerados de forma síncrona e as notas são transcritas manualmente ao GRA. Isso faz conteúdos e relatórios competirem com a navegação e torna mudanças mais abrangentes.”

“Meu diagnóstico é que o principal problema não era só tecnologia antiga. Era o acoplamento entre responsabilidades, dados e processamento. Quando uma carga pesada ou uma mudança atingia uma parte, o impacto podia se propagar pelo sistema.”

## Princípios e estilo

“A proposta aplica separação de responsabilidades, SRP, encapsulamento, alta coesão, baixo acoplamento e DIP. Grades, por exemplo, não deve alterar diretamente Matrículas. Ele consulta um contrato do contexto responsável. Integrações como GRA e vídeo também ficam atrás de adaptadores.”

“Escolhi monólito modular porque precisamos modernizar gradualmente e preservar quinze anos de dados. Microsserviços desde o início adicionariam implantação distribuída, observabilidade, consistência e operação antes de termos métricas para justificá-los. Os módulos são lógicos e permanecem inicialmente na mesma unidade de implantação.”

## Arquitetura

“No C4 Nível 1, o Prometheus V2 aparece como um único sistema usado por Administrador, Professor, Estudante e Responsável. Ele se relaciona com GRA, provedor de vídeo e infraestrutura de conteúdo.”

“No Nível 2, existe uma aplicação modular com Users, Courses, Content, Grades e Videos. A base SQL é inicialmente compartilhada, mas cada módulo controla logicamente seus dados. Binários vão para Object Storage; conteúdos apropriados podem ser distribuídos por CDN. Grades Bridge isola o GRA, e Report Workers representam a geração de PDFs em segundo plano.”

## Qualidade

“O melhor exemplo de desempenho é o relatório. No V1, a requisição espera o PDF. No V2, o trabalho é registrado, publicado em fila, processado por um worker, armazenado e consultado depois. Isso responde ao RNF05 de até quinhentos PDFs simultâneos sem degradação significativa da navegação. A arquitetura responde ao requisito, mas ainda precisamos do cenário completo e do teste.”

“Para o GRA, a nota local é confirmada antes da sincronização. Se o sistema externo cair, o trabalho fica pendente, com idempotência, auditoria e reconciliação. Para vídeo, aplicamos degradação controlada: uma falha de transmissão não deve derrubar cursos, conteúdos e notas.”

“A meta de disponibilidade é 99,9%, mas eu não afirmo que ela está garantida. Ainda faltam janela de medição, SLI, topologia, RPO, RTO e evidência operacional. Retry, Circuit Breaker, Bulkhead, Timeout e Fallback foram estudados, mas seus parâmetros e bibliotecas continuam abertos.”

## DDD

“DDD foi aplicado para definir linguagem e limites, não para criar microsserviços. A hipótese classifica Oferta Acadêmica/Matrículas e Avaliações/Resultados como Core. Na apresentação eu uso Grades como exemplo, porque concentra avaliações, notas, publicação e integração oficial. Matrícula, Avaliação, Nota, Publicação e Relatório precisam significar a mesma coisa em requisitos, código e comunicação da equipe.”

## Implementação

“A prova de conceito implementa somente Users/Identity. O backend usa ASP.NET Core, EF Core e SQLite; o frontend usa React e TypeScript. Senhas usam BCrypt, o login emite JWT e o backend autoriza por role. O Admin cria Professor e Aluno, e cada perfil possui rotas e endpoints protegidos.”

“SQLite, JWT e localStorage são escolhas didáticas do recorte, não decisões finais do V2. Courses, Content, Grades, relatórios, GRA e vídeo não estão implementados. Users também não virou microsserviço; o código apenas demonstra um limite lógico e seus contratos.”

## Decisões e encerramento

“As decisões principais foram modernização incremental, monólito modular, binários fora do banco, relatórios assíncronos, Grades Bridge, vídeo externo, redução de estado local e assincronia seletiva. Cada uma resolve um problema e aceita um custo. Por exemplo, assincronia melhora isolamento, mas o resultado deixa de ser imediato e exige estados, idempotência e observabilidade.”

“O aprendizado central foi que arquitetura não é escolher a maior quantidade de tecnologias. É decidir limites, consequências e pontos que ainda não devem ser fechados. O Prometheus V2 cria uma base para evoluir com segurança, sem distribuir o sistema antes de existir evidência.”

# 6. Versão principal — 10 a 12 minutos

| Tempo | Parte | Foco |
|---:|---|---|
| 0:00–0:40 | Abertura | Contexto, objetivo e diagnóstico em uma frase |
| 0:40–2:00 | V1 | Legado, modularidade, desempenho, escala e GRA |
| 2:00–3:20 | Princípios e estilo | SoC, encapsulamento e por que monólito modular |
| 3:20–4:15 | C4 Nível 1 | Atores, sistema e dependências externas |
| 4:15–5:45 | C4 Nível 2 | Módulos, SQL, Object Storage/CDN, GRA e vídeo |
| 5:45–7:10 | Desempenho e escala | Relatórios assíncronos, RNF05, conteúdo e RNF03 |
| 7:10–8:25 | Resiliência | GRA, vídeo, RNF01 e padrões estudados |
| 8:25–9:15 | DDD | Core, supporting, generic e exemplo Grades |
| 9:15–10:15 | Implementação | Escopo Users/Identity, stack e limites |
| 10:15–11:15 | Decisões e trade-offs | Quatro compromissos principais e pontos abertos |
| 11:15–12:00 | Conclusão | Aprendizados e frase final |

Se houver demonstração ao vivo, usar 1–2 minutos depois da implementação e reduzir a explicação de decisões para os quatro itens principais.

# 7. Versão curta — 5 a 7 minutos

## O que agrupar

- unir contexto do V1 e diagnóstico;
- explicar princípios e estilo em um único bloco;
- mostrar C4 Nível 1 rapidamente e concentrar a explicação no Nível 2;
- combinar desempenho, escalabilidade e resiliência;
- resumir DDD em classificação e um exemplo;
- fazer demonstração de no máximo um minuto ou usar screenshot;
- apresentar somente quatro decisões e quatro trade-offs.

## Roteiro curto

### 0:00–0:50 — Problema

“O V1 preserva quinze anos de dados, mas combina ASP clássico, DLL C#, MySQL com BLOBs, sessão local, PDF síncrono e GRA manual. O problema principal é o acoplamento entre responsabilidades, dados e processamento.”

### 0:50–1:35 — Decisão estrutural

“A modernização é incremental e o estilo escolhido é monólito modular. Users, Courses, Content, Grades e Videos são limites lógicos, não microsserviços. Isso reduz complexidade inicial e permite extrair somente quando métricas justificarem.”

### 1:35–2:25 — Arquitetura

“O C4 mostra um único Prometheus conectado a GRA e vídeo. Internamente há uma aplicação modular, SQL compartilhado com propriedade lógica, Object Storage/CDN, Grades Bridge, fila e Report Workers.”

### 2:25–3:35 — Qualidade

“PDF deixa a requisição web e passa por registro, fila, worker, armazenamento e consulta posterior. O GRA não bloqueia a nota local. Vídeo falha de forma isolada. Essas decisões respondem aos RNFs, mas ainda precisam de cenários e medições completas.”

### 3:35–4:15 — DDD

“DDD define linguagem e responsabilidade. Avaliações e Resultados é um Core candidato; Grades controla nota, publicação e relatório. Bounded context não significa microsserviço.”

### 4:15–5:10 — Implementação

“Foi implementado Users/Identity com ASP.NET Core, React, SQLite, JWT, BCrypt e roles. O Admin cadastra Professor e Aluno e o backend recusa acessos incompatíveis. É uma prova de conceito, não o LMS completo.”

### 5:10–6:10 — Trade-offs

“Ganhamos simplicidade com o monólito modular, mas limitamos escala independente. Ganhamos isolamento com assincronia, mas aceitamos resultado posterior. Object Storage reduz carga no banco, mas exige consistência. Provedor externo reduz responsabilidade, mas cria dependência.”

### 6:10–6:40 — Conclusão

“O projeto mostrou que arquitetura é definir limites, consequências e métricas. O V2 não distribui por tendência; ele cria condições para evoluir com segurança.”

# 8. Roteiro da demonstração

## Recomendação

Realizar a demonstração imediatamente depois de explicar a implementação referencial. Duração máxima: **1–2 minutos**.

## Preparação

- backend em `http://localhost:5000`;
- frontend em `http://localhost:5173`;
- Admin local: `admin@prometheus.local`;
- senha acadêmica: `Admin@123`;
- deixar uma aba com Portal Web e outra com Swagger;
- criar antecipadamente um plano B com duas capturas de tela.

## Fluxo recomendado

1. Acessar o login como Admin.
2. Explicar que a API verifica o hash BCrypt e emite JWT com role.
3. Mostrar a listagem administrativa.
4. Cadastrar rapidamente um Professor ou Aluno.
5. Encerrar a sessão e autenticar com o perfil criado.
6. Tentar acessar `/admin` e mostrar o redirecionamento por perfil.
7. Se houver tempo, usar Swagger com token de Professor e chamar `GET /api/demo/admin` para demonstrar o `403 Forbidden` real do backend.

## O que dizer durante a demo

“A proteção de rota melhora a experiência, mas não é a segurança efetiva. O backend valida o token e a role. O frontend pode esconder uma função; somente a API pode negar a operação.”

## O que evitar

- não explicar CSS ou detalhes visuais;
- não criar muitos usuários;
- não navegar por todos os endpoints;
- não tentar corrigir ambiente durante a apresentação;
- se a demo falhar, usar screenshots e continuar a narrativa.

# 9. Perguntas prováveis

1. Por que escolher monólito modular em vez de microsserviços?
2. Por que Grades não foi criado como serviço independente?
3. Como o sistema garante 99,9% de disponibilidade?
4. Por que retirar os arquivos do banco?
5. O que acontece se o GRA ficar indisponível?
6. Como funciona a consistência eventual nessa integração?
7. Por que JWT foi usado?
8. Por que SQLite foi usado na implementação?
9. Guardar JWT em localStorage é seguro?
10. Como Grades poderia escalar no futuro?
11. Qual é o Core Domain?
12. Qual é a diferença entre módulo lógico e bounded context?
13. Onde o DIP foi aplicado?
14. Qual é o principal trade-off da proposta?
15. O que não foi implementado?
16. Como evitar que o monólito modular volte a ficar acoplado?
17. Como seria feita a migração dos dados do V1?
18. Que métricas justificariam extrair Grades?
19. Por que não selecionar RabbitMQ ou Redis agora?
20. Qual é o maior risco da arquitetura?

# 10. Respostas

## 1. Por que monólito modular e não microsserviços?

Porque a modernização precisa preservar dados e avançar gradualmente. O monólito modular cria limites com menor complexidade operacional. Serviços independentes só seriam considerados quando métricas mostrarem benefício superior ao custo da distribuição.

## 2. Por que Grades não virou serviço independente?

Grades é um forte candidato a evolução, mas persistência, consistência com matrícula, auditoria, GRA, migração e operação ainda precisam ser definidos. Separá-lo agora seria uma decisão prematura sem métricas.

## 3. Como garante 99,9%?

A arquitetura não afirma que já garante. RNF01 define a meta, mas ainda faltam janela, SLI, exclusões, topologia, RPO/RTO e evidência operacional. Isolamento, observabilidade e redundância são respostas necessárias, não prova do resultado.

## 4. Por que Object Storage?

Porque BLOBs no banco fazem a persistência transacional também transferir arquivos grandes. Object Storage separa essa responsabilidade e a CDN pode apoiar a entrega externa. O custo é manter autorização e consistência entre metadado e objeto.

## 5. O que acontece se o GRA cair?

A nota validada é registrada localmente e confirmada ao professor. A sincronização fica pendente e deve ser retomada com idempotência, tentativas controladas, auditoria e reconciliação.

## 6. Como funciona a consistência eventual?

Prometheus e GRA podem ficar temporariamente em estados diferentes depois da confirmação local. O sistema rastreia a sincronização e reconcilia posteriormente. Fonte de autoridade e conflitos ainda são decisões abertas.

## 7. Por que JWT?

JWT foi escolhido na implementação referencial para demonstrar autenticação stateless e roles de forma simples. Ele não representa a seleção final de identidade do V2 e não resolve segurança sozinho.

## 8. Por que SQLite?

SQLite reduz dependências e facilita executar a prova de conceito localmente. O SGBD definitivo do V2 continua aberto; SQLite não é apresentado como escolha de produção.

## 9. JWT em localStorage é seguro?

É uma simplificação didática e aumenta o impacto de XSS. Uma solução real deve avaliar cookies HttpOnly, Secure e SameSite, CSRF, revogação e estratégia completa de sessão.

## 10. Como Grades escalaria no futuro?

Primeiro seriam medidos throughput, latência, backlog, CPU, memória, conexões SQL e impacto na navegação. Só depois seriam avaliados mais workers, múltiplas instâncias, persistência especializada ou extração.

## 11. Qual é o Core Domain?

A hipótese DDD identifica Oferta Acadêmica/Matrículas e Avaliações/Resultados como Core. Para explicar o projeto, uso Grades como exemplo central porque concentra regras críticas de avaliação, nota, publicação e integração oficial. A classificação ainda precisa ser validada com a School X.

## 12. Módulo e bounded context são iguais?

Não necessariamente. Bounded context é um limite de modelo e linguagem do domínio; módulo é uma forma de organizar responsabilidade no software. Eles podem se alinhar, mas nenhum deles implica microsserviço.

## 13. Onde o DIP foi aplicado?

GRA, Video Provider e armazenamento são acessados por abstrações ou adaptadores. Grades Bridge impede que o contrato externo do GRA domine o modelo interno de Grades.

## 14. Qual é o principal trade-off?

O monólito modular reduz complexidade operacional e favorece migração, mas limita implantação e escala independente. Ele exige governança para não reproduzir o acoplamento do legado.

## 15. O que não foi implementado?

Courses, Content, Grades, relatórios, GRA, vídeo, Portal de Pais, MFA, Object Storage, CDN e infraestrutura de produção. A implementação cobre somente Users/Identity e o Portal Web correspondente.

## 16. Como evitar acoplamento no monólito?

Com propriedade lógica, contratos explícitos, revisão de dependências, testes de arquitetura e proibição de acesso direto às estruturas internas de outro módulo. A disciplina é parte do custo da decisão.

## 17. Como migrar os dados do V1?

Em lotes incrementais, com inventário, cópias recuperáveis, mapeamento de identificadores, validação de contagens e relações e caminho de retorno. BLOBs só seriam removidos depois de copiar, verificar integridade, registrar referência e testar autorização.

## 18. Qual métrica justificaria extrair Grades?

Saturação persistente atribuída a Grades, backlog e idade da fila, throughput, percentis de latência, taxa de erro, conexões SQL e necessidade de ritmo de mudança ou disponibilidade própria. A decisão precisa comparar o benefício ao custo operacional.

## 19. Por que não RabbitMQ ou Redis agora?

Porque a arquitetura exige comportamento de fila e pode futuramente exigir cache, mas ainda não definiu garantias, volume, TTL, invalidação e operação. Escolher produto antes desses critérios seria prematuro.

## 20. Qual é o maior risco?

A perda ou corrupção do acervo histórico é um risco muito alto. Outro risco estrutural é reproduzir o acoplamento do V1 dentro do novo monólito. Migração incremental, validação e governança dos limites tratam esses riscos.

# 11. Coisas que NÃO devo falar

| Não falar | Por que está errado |
|---|---|
| “Users é um microsserviço.” | É um módulo lógico; o diretório e a API não comprovam implantação independente |
| “Estamos usando Redis.” | Cache e produto continuam condicionados |
| “Usamos RabbitMQ.” | A fila é uma abstração; o broker não foi selecionado |
| “Grades já escala horizontalmente sozinho.” | Escala isolada é evolução condicionada |
| “O sistema garante 99,9% porque tem Load Balancer.” | Load Balancer não foi escolhido e disponibilidade exige medição, redundância e operação |
| “JWT torna o sistema seguro.” | JWT é apenas um mecanismo; segurança inclui sessão, autorização, chaves, XSS, CSRF e operação |
| “CDN resolve toda a disponibilidade.” | CDN ajuda conteúdo; não resolve banco, aplicação, integrações ou recuperação |
| “NoSQL é melhor para Grades.” | Não há decisão ou evidência que justifique persistência especializada |
| “O Prometheus V2 está implementado.” | Somente Users/Identity possui implementação referencial |
| “O backend de autenticação representa a arquitetura completa.” | Ele prova apenas um recorte |
| “C4 Nível 2 mostra cinco microsserviços.” | O desenho vigente mostra uma aplicação modular unificada |
| “O RNF05 já foi comprovado.” | A arquitetura responde ao requisito, mas o teste completo ainda não existe |
| “Retry e Circuit Breaker já estão configurados.” | Foram estudados; composição e parâmetros continuam abertos |
| “SQLite é o banco definitivo.” | É escolha local da prova de conceito |
| “Bounded context sempre vira serviço.” | Modelagem de domínio e topologia de implantação são decisões diferentes |

# 12. Glossário

| Termo | Definição curta |
|---|---|
| SoC | Separação de responsabilidades por preocupação |
| SRP | Um componente possui uma razão principal para mudar |
| DIP | Depender de abstrações, não de detalhes externos concretos |
| DRY | Evitar duplicação de conhecimento e regras |
| Coesão | Grau em que responsabilidades relacionadas ficam juntas |
| Acoplamento | Grau de dependência entre partes do sistema |
| Bounded Context | Limite no qual modelo e linguagem possuem significado consistente |
| Core Domain | Capacidade estratégica que diferencia o negócio |
| Supporting Domain | Capacidade importante que apoia o Core |
| Generic Subdomain | Capacidade comum que pode usar solução padronizada |
| JWT | Token assinado com claims usado para autenticação/autorização |
| Idempotência | Repetir a operação produz o mesmo efeito observável |
| Consistência eventual | Sistemas convergem depois de um intervalo |
| Fallback | Resposta alternativa quando a principal falha |
| Retry | Nova tentativa controlada de uma operação |
| Circuit Breaker | Interrompe chamadas a uma dependência que está falhando |
| Bulkhead | Isola recursos para limitar propagação de falhas |
| Horizontal Scaling | Aumentar a quantidade de instâncias ou executores |
| Vertical Scaling | Aumentar recursos de uma instância |
| CDN | Rede que distribui conteúdo próximo ao consumidor |
| Object Storage | Armazenamento de objetos/binários por chave e metadados |
| C4 | Modelo de diagramas de contexto, contêineres, componentes e código |
| ADR | Registro de contexto, decisão, alternativas e consequências |
| RPO | Perda máxima de dados aceitável em tempo |
| RTO | Tempo máximo aceitável para recuperação |

# 13. Conclusões pessoais

## O que aprendi

“Eu aprendi que arquitetura não é escolher tecnologia ou dividir o sistema na maior quantidade possível de serviços. Primeiro é necessário entender o problema, os atributos de qualidade, os limites do domínio e as consequências de cada decisão.”

“Também aprendi que modularidade não surge apenas com pastas ou APIs. Ela depende de propriedade de regras, contratos explícitos, alta coesão e controle das dependências. A assincronia pode melhorar desempenho e resiliência, mas adiciona estados, idempotência, reconciliação e necessidade de observabilidade.”

## O que mudou na minha visão

“Antes eu associava escalabilidade principalmente a microsserviços e infraestrutura. Agora entendo que reduzir estado local, retirar arquivos do banco, isolar cargas e medir gargalos pode ser mais importante do que distribuir prematuramente. Microsserviço é uma consequência possível de uma necessidade comprovada, não um objetivo por si só.”

## O que eu faria diferente

- coletaria métricas reais do V1 mais cedo;
- definiria cenários completos de RNF com ambiente, percentis e critérios de aceitação;
- validaria a classificação DDD com especialistas do negócio;
- definiria contratos do GRA e do Video Provider mais cedo;
- testaria hipóteses de relatórios e conteúdo com protótipos;
- fecharia RPO/RTO antes de escolher topologia de alta disponibilidade;
- criaria critérios automatizados para governar dependências entre módulos.

# 14. Frase final

> **O Prometheus V2 não foi desenhado para distribuir o sistema por tendência, mas para criar limites claros, preservar o legado e permitir que cada evolução futura seja decidida com segurança e evidência.**

# COLA PARA APRESENTAÇÃO

## Problema V1

- ASP clássico + DLL C# + MySQL centralizado.
- BLOBs no banco, sessão local, PDFs síncronos e GRA manual.
- Alto acoplamento, baixa coesão e competição por recursos.
- Diagnóstico: responsabilidades, dados e processamento estavam acoplados.

## Solução V2

- Modernização incremental.
- Monólito modular inicialmente implantado como uma unidade.
- Módulo lógico ≠ API ≠ bounded context ≠ microsserviço.
- Extração futura somente com métricas.

## Módulos

- Users: usuários, perfis e identidade.
- Courses: cursos, turmas, períodos e matrículas.
- Content: metadados e materiais.
- Grades: avaliações, notas, publicação e relatórios.
- Videos: contexto acadêmico das sessões.

## Três RNFs principais

- RNF01: 99,9% de disponibilidade; medição ainda precisa ser completada.
- RNF03: conteúdo externo em menos de 2 segundos; cenário incompleto.
- RNF05: até 500 PDFs simultâneos sem degradação significativa; teste pendente.

RNF04: GRA não bloqueia o registro local da nota.

## Quatro decisões

1. Monólito modular para reduzir complexidade distribuída.
2. Binários fora do banco com Object Storage/CDN.
3. Relatórios assíncronos por fila e Report Workers.
4. Grades Bridge e sincronização assíncrona com o GRA.

## Quatro trade-offs

1. Simplicidade operacional × menor escala independente.
2. Assincronia e isolamento × resultado posterior.
3. Object Storage × consistência metadado–arquivo.
4. Provedor externo × dependência de contrato e disponibilidade.

## DDD

- Core candidatos: Oferta Acadêmica/Matrículas e Avaliações/Resultados.
- Exemplo principal: Grades.
- Supporting: Pessoas/Perfis, Content, Comunicação e Sessões ao Vivo.
- Generic: Identidade/Acesso e Entrega de Notificações.
- Bounded context não implica microsserviço.

## Implementação

- Somente Users/Identity.
- ASP.NET Core + React/TypeScript + SQLite.
- JWT + BCrypt + roles Admin/Professor/Aluno.
- Admin cadastra usuários; rotas e endpoints protegidos.
- Não representa o LMS completo.

## Pontos abertos

- SGBD, broker, cache, identidade, vídeo e observabilidade.
- Alta disponibilidade, RPO/RTO e persistência futura de Grades.
- Retry/Circuit Breaker/Bulkhead estudados, não configurados como decisão.

## Fechamento

> Limites claros primeiro; distribuição e tecnologia somente quando requisitos e métricas justificarem.
