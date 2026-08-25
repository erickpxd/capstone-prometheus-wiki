# 4. Cronograma e Gestão de Riscos

## Estado documental

> **Planejamento, não execução comprovada.** Os sete marcos abaixo organizam a evolução proposta do Prometheus V2. Os materiais disponíveis não confirmam datas, duração, responsáveis, orçamento, capacidade da equipe, dependências do cronograma nem evidência de execução.

Os marcos são apresentados em sequência lógica e com critérios planejados de conclusão, sem constituir calendário. “Vigente” nas referências significa aceito na documentação arquitetural; não significa implementado ou implantado.

## Marco 1 — Conhecer e proteger o legado

Planejar o inventário de dados, regras, fluxos e integrações manuais do Prometheus V1; estabelecer cópias recuperáveis; e medir o desempenho atual de relatórios e conteúdos. O critério planejado de conclusão é existir uma linha de base documentada, dados validados e um caminho de retorno testado antes de cada migração.

**Rastreabilidade:** [RF-BASE-05](/home/01-Requisitos/Requisitos-Funcionais), [DA-01](/home/02-Arquitetura/Decisoes-Arquiteturais) e [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental).

## Marco 2 — Estabelecer os limites modulares

Planejar contratos explícitos para Users, Courses, Content, Grades e Videos e definir os controles transversais necessários de autenticação, autorização, auditoria e observabilidade. Os módulos permanecem limites lógicos do monólito modular e podem usar a base SQL principal inicialmente compartilhada, sem modificar diretamente dados sob propriedade de outro módulo.

O critério planejado de conclusão é que responsabilidades, contratos e propriedade lógica estejam documentados e sejam verificáveis, sem transformar módulos ou APIs em unidades independentes de implantação.

**Rastreabilidade:** [RF-BASE-01–04 e RF-BASE-12](/home/01-Requisitos/Requisitos-Funcionais), [RNF02](/home/01-Requisitos/Requisitos-Nao-Funcionais), [DA-02, DA-03 e DA-11](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) e [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios).

## Marco 3 — Retirar binários do banco

Planejar a introdução da abstração de Object Storage, a migração gradual dos binários com verificação de integridade, a conservação dos metadados no domínio Content e a distribuição por CDN dos conteúdos apropriados. Produto, fornecedor, região e mecanismo de acesso continuam abertos.

O critério planejado de conclusão inclui a validação da migração e da autorização e a verificação do RNF03 em cenário externo ainda a ser completado. O arquivo de origem somente pode ser retirado do banco após validação e período de segurança definidos.

**Rastreabilidade:** [RF-BASE-03 e RF-BASE-05](/home/01-Requisitos/Requisitos-Funcionais), [RNF02 e RNF03](/home/01-Requisitos/Requisitos-Nao-Funcionais), [DA-04](/home/02-Arquitetura/Decisoes-Arquiteturais) e [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco).

## Marco 4 — Isolar avaliações, notas e relatórios

Planejar a consolidação dos limites internos de Grades e retirar a geração de PDFs da requisição web original, usando registro do trabalho, fila como abstração arquitetural, Report Workers, estados explícitos, idempotência e armazenamento posterior do resultado. Isso não define produto de mensageria nem implantação independente para Grades ou Report Workers.

O critério planejado de conclusão é verificar o cenário definido pelo RNF05 depois de especificados ambiente, duração, combinação de relatórios e limite mensurável de degradação. Persistência própria de Grades permanece evolução futura condicionada a métricas e decisão específica.

**Rastreabilidade:** [RF-BASE-04 e RF05](/home/01-Requisitos/Requisitos-Funcionais), [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais), [DA-05 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios) e [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva).

## Marco 5 — Automatizar o GRA

Planejar o Grades Bridge como adaptador lógico e a sincronização assíncrona com os contratos REST e *webhooks* do GRA por meio de fila ou Message Broker como abstração. O produto, as garantias de entrega e as confirmações ainda não foram selecionados.

O critério planejado de conclusão é verificar que lentidão ou indisponibilidade temporária do GRA não bloqueia o registro local confirmado ao professor e que idempotência, auditoria, tentativas controladas e reconciliação impedem duplicações observáveis. Fonte de autoridade, regras de conflito e contrato completo continuam abertos.

**Rastreabilidade:** [RF-BASE-11](/home/01-Requisitos/Requisitos-Funcionais), [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais), [DA-06 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge) e [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva).

## Marco 6 — Entregar portais e novas capacidades

Planejar a disponibilização incremental do acesso por papel, das avaliações, dos fóruns, do Portal de Pais com MFA e das videoconferências. Notificações, análises, exportação, gravações, portfólio e mensagens devem permanecer associadas aos limites lógicos responsáveis e ser planejadas em incrementos próprios.

O contexto acadêmico de vídeo permanece no Prometheus, enquanto a transmissão é delegada a um Video Provider externo ainda não selecionado. O marco não altera o monólito modular nem confirma implantação independente de qualquer módulo.

**Rastreabilidade:** [Requisitos Funcionais](/home/01-Requisitos/Requisitos-Funcionais), [RNF02](/home/01-Requisitos/Requisitos-Nao-Funcionais), [DA-03, DA-07 e DA-08](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios), [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa) e [ADR-008](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local).

## Marco 7 — Validar e decidir evoluções

Planejar a medição de disponibilidade, segurança, latência, carga e manutenibilidade e a execução dos cenários de qualidade para falhas do GRA, de Report Worker, do Video Provider e de uma execução isolada. Sinais operacionais, critérios de detecção, procedimentos e plataforma de observabilidade ainda precisam ser definidos.

O critério planejado de conclusão é obter evidência suficiente para aceitar ou rejeitar evoluções. Extração de módulo, múltiplas instâncias, persistência especializada, redundância e recuperação são possibilidades condicionadas a métricas, custos e nova decisão; não fazem parte automaticamente da arquitetura vigente.

**Rastreabilidade:** [RNF01–RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais), [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade), [DA-10 e DA-11](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) e [ADR-008](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local).

## Estratégia de migração

Usuários, cursos, vínculos e notas precisam manter identificadores ou mapeamento rastreável entre V1 e V2. Contagens, relações e amostras devem ser comparadas. Notas exigem precisão, autoria, histórico e consistência com o GRA.

Para cada binário, a migração planejada cria o objeto no armazenamento dedicado, verifica sua integridade, registra a referência e testa a autorização. O arquivo somente pode ser retirado do banco depois da validação e do período de segurança definidos.

Durante a convivência, deve existir uma fonte de autoridade para cada dado. Uma função somente pode assumir tráfego depois da verificação de dados, segurança, comportamento, desempenho e caminho de retorno.

**Rastreabilidade:** [RF-BASE-05 e RF-BASE-11](/home/01-Requisitos/Requisitos-Funcionais), [DA-01, DA-04 e DA-06](/home/02-Arquitetura/Decisoes-Arquiteturais), [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental), [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco) e [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge).

## Riscos

Os tratamentos abaixo são respostas planejadas no nível arquitetural. Eles não comprovam implantação nem selecionam produtos, fornecedores ou topologias.

| Risco | Impacto | Tratamento planejado | Rastreabilidade |
|---|---|---|---|
| Perda ou corrupção de dados históricos | Muito alto | Inventariar, manter cópias recuperáveis, migrar em lotes validados e preservar caminho de retorno. | [RF-BASE-05](/home/01-Requisitos/Requisitos-Funcionais); [DA-01](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) |
| Reprodução do acoplamento do legado | Alto | Aplicar propriedade lógica, contratos explícitos e critérios verificáveis de governança entre módulos. | [DA-02 e DA-03](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular); [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) |
| Relatórios degradarem a navegação | Alto | Separar a requisição do processamento, acompanhar estados e medir o isolamento da carga no cenário completo do RNF05. | [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-05 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios) |
| Trabalho de relatório ser perdido, duplicado ou ficar inconsistente | Alto | Manter registro rastreável, estados explícitos, idempotência e recuperação para reprocessamento; definir posteriormente a semântica de entrega. | [DA-05 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios); [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) |
| Indisponibilidade ou lentidão do GRA | Alto | Confirmar a nota localmente e tratar a sincronização posterior com estado, idempotência, tentativas controladas, auditoria e reconciliação. | [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-06 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge) |
| Mensagens repetidas, perdidas ou fora de ordem nas integrações assíncronas | Alto | Definir estado, idempotência, rastreamento, reconciliação e, antes da implementação, as garantias adequadas a cada fluxo. | [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-06 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge); [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) |
| Falha do Video Provider afetar funções independentes | Médio/alto | Isolar a integração externa e preservar cursos, conteúdos e notas; contrato, SLA e políticas concretas de resiliência permanecem abertos. | [DA-07 e DA-10](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa) |
| Exposição de dados pessoais ou acadêmicos | Muito alto | Aplicar criptografia em trânsito e repouso, autorização por vínculo, MFA no Portal de Pais e auditoria; mecanismos ainda precisam ser selecionados. | [RNF02](/home/01-Requisitos/Requisitos-Nao-Funcionais); [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) |
| Inconsistência entre metadado e arquivo | Alto | Verificar integridade, autorização e referências durante migração e operação; definir políticas de retenção, exclusão e recuperação. | [DA-04](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco) |
| Banco SQL principal permanecer como gargalo | Alto | Medir o comportamento, retirar binários conforme DA-04 e avaliar especialização de persistência somente se métricas justificarem nova decisão. | [RNF03 e RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-04, DA-05 e DA-10](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco) |
| Distribuição prematura da arquitetura | Médio/alto | Exigir evidência mensurável de benefício superior ao custo antes de extrair módulos ou separar implantações. | [DA-02, DA-03 e DA-10](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular); [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) |
| Dependência compartilhada introduzir novo ponto único de falha | Alto | Avaliar disponibilidade e operação de cada dependência quando sua tecnologia e topologia forem decididas; não pressupor uma solução concreta. | [RNF01](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-08 e DA-10](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-008](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local) |
| Recuperação não atender às necessidades acadêmicas | Muito alto | Definir RPO, RTO, backup, restauração e recuperação de desastre antes de selecionar e testar mecanismos de redundância ou recuperação. | [RNF01](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-10 e DA-11](/home/02-Arquitetura/Decisoes-Arquiteturais); [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) |
| Falha ser detectada tarde | Alto | Definir SLI, sinais, fontes de medição, critérios de alerta e procedimentos operacionais antes de escolher a plataforma. | [RNF01](/home/01-Requisitos/Requisitos-Nao-Funcionais); [DA-11](/home/02-Arquitetura/Decisoes-Arquiteturais); [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) |
| Mudança de contrato externo | Médio | Isolar contratos em adaptadores lógicos, acompanhar versões e planejar substituição ou reconciliação conforme o sistema afetado. | [RF-BASE-08 e RF-BASE-11](/home/01-Requisitos/Requisitos-Funcionais); [DA-06 e DA-07](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge); [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa) |

Os riscos devem ser reavaliados a cada marco. Migração e segurança bloqueiam uma liberação quando recuperação e proteção ainda não tiverem sido demonstradas.

## Mecanismos e evoluções ainda abertos

Os seguintes itens não são compromissos deste cronograma:

- produto de Message Broker, garantias de entrega, confirmações e persistência de mensagens; a arquitetura vigente requer comportamento assíncrono, estados, idempotência e recuperação conforme cada fluxo, sem prescrever garantia de durabilidade;
- adoção de cache, produto, escopo, autoridade dos dados, TTL e invalidação; qualquer uso permanece condicionado a necessidade e decisão futura;
- Load Balancer, múltiplas instâncias, replicação, *failover*, topologias ativa/ativa ou *primary/standby* e implantação independente; são alternativas futuras condicionadas a métricas, RPO/RTO, custo e decisão associada à DA-10;
- persistência própria de Grades, réplicas de leitura ou banco especializado; a base SQL principal inicialmente compartilhada permanece a referência;
- plataforma, retenção, rastreamento, alertas e modelo operacional de observabilidade; a DA-11 registra a direção, mas não seleciona a estratégia concreta.

Esses pontos devem ser acompanhados em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) e na seção de tensões da [Análise de Trade-offs](/home/03-Trade-offs).

## Lacunas de planejamento e operação

As lacunas abaixo impedem transformar a sequência de marcos em um calendário executável ou em compromisso operacional. Elas não devem ser preenchidas por inferência.

| Lacuna | Estado atual |
|---|---|
| Datas e duração | Não informadas para os marcos. |
| Responsáveis e aprovadores | Não informados. |
| Orçamento | Não informado. |
| Capacidade da equipe | Não informada. |
| Dependências e precedências executáveis | A sequência lógica está documentada, mas dependências, bloqueios e critérios de entrada não foram confirmados com a equipe. |
| RPO e RTO | Não definidos para dados, integrações ou capacidades do Prometheus V2. |
| Backup, restauração e recuperação de desastre | Políticas, ambientes, retenção, testes e evidências não confirmados. |
| Evidência de execução | Não há evidência local confirmada de implementação, implantação ou conclusão dos marcos. |

## Referências

- [Requisitos Funcionais](/home/01-Requisitos/Requisitos-Funcionais)
- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais)
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais)
- [Architecture Decision Records](/home/02-Arquitetura/ADRs)
- [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade)
- [Análise de Trade-offs](/home/03-Trade-offs)
- [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos)
