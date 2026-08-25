# Módulos e Responsabilidades

> **Estado documental:** esta página descreve a arquitetura proposta e vigente na documentação do Prometheus V2. Ela não comprova que os módulos estejam implementados.

## Regra de interpretação

O Prometheus V2 é documentado como um **monólito modular inicialmente implantado como uma unidade**. A separação por domínios organiza responsabilidades, regras e contratos internos sem distribuir prematuramente a aplicação.

Os termos abaixo não são equivalentes:

| Conceito | Significado nesta arquitetura |
|---|---|
| Subdomínio | Parte do espaço do problema e da capacidade de negócio; sua classificação estratégica não define topologia técnica |
| Contexto delimitado | Limite no qual um modelo e uma linguagem possuem significado consistente; pode permanecer dentro da mesma aplicação |
| Módulo lógico | Limite de responsabilidade, regras e propriedade lógica de dados dentro da aplicação |
| API REST | Contrato de interação exposto por uma capacidade; não determina sua topologia de execução |
| Microsserviço | Serviço com ciclo de vida e implantação independentes; não é o estilo arquitetônico vigente |
| Unidade independente de implantação | Artefato ou processo que pode ser publicado separadamente; os módulos atuais não possuem essa condição presumida |

Portanto:

> **módulo lógico ≠ API REST ≠ microsserviço ≠ unidade independente de implantação**

Os nomes Users API, Courses API, Content API, Grades API e Videos API identificam contratos associados aos limites lógicos. Eles não significam cinco processos, cinco bancos ou cinco microsserviços. Uma extração futura somente poderá ocorrer quando métricas e necessidades próprias de escala, disponibilidade ou evolução demonstrarem benefício superior ao custo da distribuição.

Da mesma forma, identificar um subdomínio ou propor um contexto delimitado não cria uma API, um banco ou uma unidade de implantação. Modelagem de domínio e topologia de execução são decisões relacionadas, mas distintas.

## Limites lógicos principais

| Módulo | Responsabilidades consolidadas | Colaborações principais |
|---|---|---|
| **Users** | Cadastro e ciclo de vida de estudantes, professores, administradores, pais ou responsáveis; perfis e permissões | Identidade e consumidores autorizados dos dados de usuário |
| **Courses** | Cursos, disciplinas, turmas, períodos, fóruns e calendário; contexto dos vínculos acadêmicos | Users, Matrículas, Content, Grades e Videos |
| **Content** | Metadados, upload, autorização, associação ao curso e ciclo de vida dos materiais | Courses, Object Storage e CDN |
| **Grades** | Avaliações, validação, lançamento e consulta de notas, cálculo, publicação, histórico, relatórios e exportações | Identidade, Matrículas, Grades Bridge, fila, Report Workers e armazenamento de relatórios |
| **Videos** | Agendamento, autorização, links, contexto acadêmico das sessões e associação das gravações | Courses, Content e provedor externo de videoconferência |

Esses limites buscam alta coesão, baixo acoplamento e encapsulamento. Cada módulo deve controlar suas regras e oferecer contratos aos consumidores, evitando acesso direto às estruturas internas de outro domínio.

![image](uploads/b6d0beca1718c25f4c4c2a4a8d3d3478/image.png){width=900 height=438}

Uma visão editável desses limites está na [fonte PlantUML de Módulos e Responsabilidades](/home/02-Arquitetura/Fonte-PlantUML-Modulos-e-Responsabilidades). As setas do desenho representam colaborações lógicas, não chamadas de rede obrigatórias.

## Responsabilidades internas identificadas

### Identidade

Identidade reúne autenticação e autorização. Ela é uma responsabilidade interna relacionada a Users e aos fluxos protegidos de todos os módulos. A tecnologia de identidade, o provedor, os protocolos e o mecanismo de sessão ainda não foram selecionados.

Autenticar um usuário não substitui a autorização contextual. Operações sobre cursos, estudantes, conteúdos e notas também precisam validar papel, vínculo e escopo permitidos. O Portal de Pais deve cumprir o requisito de MFA e limitar a consulta aos estudantes vinculados.

Identidade não é documentada como microsserviço ou unidade independente de implantação.

### Matrículas

Matrículas representa o vínculo entre estudante, curso e turma e a validação desse vínculo. A responsabilidade está associada ao contexto acadêmico administrado por Courses e é consultada por Grades antes de operações que dependam da participação do estudante.

Grades não deve alterar diretamente os dados internos de matrícula. A fronteira técnica final dessa responsabilidade continua aberta; sua identificação não cria um serviço independente.

### Comunicação

Comunicação abrange fóruns, notificações, avisos e mensagens relacionadas ao contexto acadêmico. Ela colabora principalmente com Courses e Grades e pode exigir adaptadores para entrega por e-mail, SMS ou *push*.

Os limites finais de Comunicação e Notificações ainda não estão definidos. Por isso, a página registra a responsabilidade, mas não escolhe módulo proprietário definitivo, produto de mensageria ou unidade de implantação.

## Hipótese de modelagem estratégica com DDD

> **Proposta condicionada:** a classificação e os contextos desta seção resultam das análises acadêmicas da Semana 7. Eles precisam ser validados com especialistas da School X, não substituem Users, Courses, Content, Grades e Videos e não comprovam implementação.

A hipótese distingue capacidades presumidamente diferenciadoras, capacidades de apoio e capacidades genéricas. Criticidade técnica, sozinha, não torna um subdomínio *core*.

| Contexto delimitado candidato | Classificação proposta | Relação com os limites vigentes | Estado |
|---|---|---|---|
| Pessoas e Perfis | Supporting | Refina Users para representar pessoas, perfis institucionais e vínculo responsável–estudante | Hipótese a validar |
| Identidade e Acesso | Generic | Refina a responsabilidade Identidade; autenticação não substitui autorização contextual | Hipótese a validar; provedor e mecanismos abertos |
| Oferta Acadêmica e Matrículas | Core | Refina Courses para cursos, turmas, períodos, calendário, matrícula e elegibilidade | Hipótese de domínio central a validar |
| Avaliações e Resultados Acadêmicos | Core | Refina Grades para avaliações, tentativas, notas, publicação, histórico, relatórios e exportações | Hipótese de domínio central a validar |
| Conteúdo Didático | Supporting | Refina Content para metadados, autorização, associação ao curso e ciclo de vida | Hipótese a validar; Portfólio continua em descoberta |
| Comunicação Acadêmica | Supporting | Refina Comunicação para fóruns, avisos, conversas e mensagens do curso | Fronteira preliminar |
| Sessões de Aprendizagem ao Vivo | Supporting | Refina Videos para agendamento, participantes, links e associação de gravações | Hipótese a validar |
| Entrega de Notificações | Generic | Entrega intenções por canais externos sem possuir a regra acadêmica de origem | Separação de Comunicação ainda preliminar |

Nessa leitura proposta:

- o Portal de Pais compõe casos de uso e regras pertencentes a outros contextos; não é automaticamente um contexto delimitado próprio;
- o Grades Bridge exerce o papel de adaptador ou *Anti-Corruption Layer* para o GRA; não é automaticamente contexto, módulo independente ou serviço;
- Object Storage, CDN, Video Provider e renderização técnica de PDF são infraestrutura, serviço externo ou mecanismo de implementação, não subdomínios de negócio por si mesmos;
- compartilhar fisicamente o banco SQL não constitui *Shared Kernel*: não há decisão de compartilhar deliberadamente um único modelo de domínio entre contextos;
- relações *upstream/downstream*, linguagens publicadas, eventos e responsabilidades de equipe ainda precisam de descoberta e não são contratos vigentes por simples aparição no mapa.

O [mapa de contextos DDD proposto em PlantUML](/home/02-Arquitetura/Fonte-PlantUML-Mapa-de-Contextos-DDD-Proposto) visualiza essa hipótese dentro da implantação inicialmente unificada. Ele é complementar e não substitui os diagramas C4 vigentes.

## Organização interna de Grades

As análises de modularidade identificam dentro de Grades as responsabilidades de Avaliações, Validação, Lançamento, Cálculo, Publicação e Relatórios. Essa decomposição serve para separar regras e razões de mudança dentro do domínio.

Essas responsabilidades internas não são serviços independentes confirmados. O processamento assíncrono de PDFs possui fluxo próprio, descrito em [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios), mas não transforma automaticamente Grades em um microsserviço.

## Propriedade e colaboração

- o módulo proprietário aplica as regras do seu domínio;
- outros módulos usam contratos explícitos e não modificam diretamente suas estruturas internas;
- propriedade lógica não exige banco fisicamente separado;
- identificadores e contratos não comprovam comunicação de rede;
- operações que exigem confirmação acadêmica permanecem síncronas no contexto principal;
- efeitos tolerantes a atraso podem usar assincronia seletiva conforme a decisão registrada;
- autenticação, autorização, auditoria e observabilidade são preocupações transversais, não justificativas automáticas para novos serviços.

A base SQL inicialmente compartilhada e as integrações externas são detalhadas em [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes). Tecnologias e fornecedores em aberto são registrados em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos).

## Evolução condicionada

Grades é o candidato mais explícito a uma futura especialização por causa das cargas de notas e relatórios. Persistência própria, escala isolada ou implantação independente, entretanto, continuam como **evoluções futuras condicionadas a métricas**, consistência, auditoria, migração, observabilidade e custo operacional.

Até que uma nova decisão seja registrada, a referência permanece: **um monólito modular, inicialmente implantado como uma unidade, com módulos lógicos e contratos explícitos**.
