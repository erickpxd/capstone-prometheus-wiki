# Análise de Trade-offs

## Objetivo e escopo

Esta página é a fonte canônica para a relação entre os benefícios obtidos pelas decisões arquiteturais do Prometheus V2 e os custos, riscos ou limitações aceitos em troca. “Vigente” significa aceito na documentação arquitetural; não comprova implementação nem implantação em produção.

A síntese abaixo não substitui os requisitos, os cenários de qualidade ou os ADRs. Produtos, mecanismos e evoluções sem decisão explícita permanecem em Tecnologias e Pontos Abertos.

## Trade-offs das decisões vigentes

| Decisão / tensão | Benefício obtido | Custo ou risco aceito | Atributos favorecidos | Atributos impactados | Referência |
|---|---|---|---|---|---|
| Modernização incremental | Reduz a exposição a uma substituição total, preserva o acervo histórico e permite validação gradual com possibilidade de retorno. | A convivência temporária aumenta a complexidade e pode prolongar o período de modernização; cada etapa exige migração, validação, tratamento de divergências e retorno. | Confiabilidade; recuperação; manutenibilidade | Complexidade operacional durante a transição | [DA-01](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) |
| Monólito modular | Combina modularidade lógica com implantação inicial mais simples, menor custo de distribuição e evolução posterior baseada em métricas. | A implantação compartilhada limita o isolamento físico e a escala independente; os limites internos exigem disciplina arquitetônica. | Modularidade; manutenibilidade | Escalabilidade; isolamento de falhas | [DA-02](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) |
| Decomposição por domínios | Aumenta coesão, encapsulamento e testabilidade, explicita responsabilidades e favorece mudanças localizadas. | Exige contratos e governança; fronteiras inadequadas ou dependências cruzadas podem recriar o acoplamento do legado. | Manutenibilidade; modularidade | Complexidade de evolução | [DA-03](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) |
| Binários fora do banco | Reduz a carga de armazenamento e entrega sobre o banco transacional, separa responsabilidades e favorece a distribuição de conteúdo. | Introduz dependências operacionais de armazenamento e entrega; exige autorização, migração dos BLOBs e consistência entre metadados e arquivos. | Desempenho; escalabilidade | Segurança; consistência; complexidade operacional | [DA-04](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco) |
| Processamento assíncrono de relatórios | Isola a geração pesada da navegação, desacopla a entrada da capacidade de processamento e favorece o cenário de concorrência do RNF05. | O resultado deixa de ser imediato; estados intermediários, idempotência, reprocessamento e operação adicional tornam-se necessários. | Desempenho; escalabilidade; isolamento de carga | Imediatismo do resultado; complexidade operacional | [DA-05](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios) |
| Integração GRA por Grades Bridge | Retira a transcrição manual do fluxo pretendido, isola o registro local da lentidão ou indisponibilidade do GRA e favorece auditoria e reconciliação. | Aceita atualização posterior entre os sistemas e depende de contratos externos, controle de estado, tratamento de falhas e reconciliação; autoridade e conflitos permanecem abertos. | Interoperabilidade; resiliência; auditabilidade | Consistência entre sistemas; complexidade operacional | [DA-06](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge) |
| Transmissão de vídeo externa | Evita construir infraestrutura especializada de mídia, reduz a responsabilidade operacional do Prometheus e usa capacidade externa especializada. | Aceita dependência de contrato, latência e disponibilidade externas e exige isolamento de falhas; fornecedor, SLA, custos e limites permanecem abertos. | Manutenibilidade; foco de domínio | Disponibilidade; resiliência | [DA-07](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa) |
| Redução de estado local | Reduz a dependência de afinidade de sessão, facilita a distribuição de tráfego e a recuperação por outra execução e prepara a evolução da implantação. | O estado precisa ser representado de outra forma e pode introduzir uma dependência compartilhada, com requisitos de segurança, expiração, revogação e consistência; o mecanismo concreto permanece aberto. | Escalabilidade; disponibilidade | Segurança; complexidade operacional | [DA-08](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-008](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local) |
| Assincronia seletiva | Desacopla efeitos tolerantes a atraso, preserva a responsividade do fluxo principal e melhora a resiliência diante de tarefas demoradas ou dependências externas. | Nos fluxos aprovados, efeitos terminam depois e exigem estados explícitos, idempotência, reconciliação e maior capacidade de diagnóstico. | Desempenho; resiliência; disponibilidade | Consistência imediata; observabilidade; complexidade operacional | [DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) |

A assincronia é seletiva: o Prometheus V2 não está sendo definido como uma arquitetura totalmente orientada a eventos.

## Tensões ainda abertas

Os itens abaixo não representam decisões fechadas nem comprovam implementação.

| Tensão aberta | Por que ainda está aberta | Onde acompanhar |
|---|---|---|
| Topologia de alta disponibilidade | A DA-10 registra a direção de disponibilidade seletiva, mas topologia, redundância, recuperação, RPO, RTO e operação ainda dependem de critérios e decisão futura. | [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) e [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) |
| Cache | O uso permanece condicionado; produto, escopo, autoridade dos dados, TTL e invalidação não foram definidos. | [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) |
| Message Broker | O comportamento assíncrono é vigente para relatórios e GRA, mas produto, garantias de entrega, confirmações e tratamento de falhas permanecem abertos. | [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) e [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) |
| Elasticidade de Report Workers | Mais workers podem elevar o throughput e reduzir o backlog, mas aumentam custo e pressão sobre SQL, mensageria e armazenamento; partida fria e oscilação também precisam ser avaliadas. Métricas, limites e topologia permanecem condicionados. | [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos), [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) e [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) |
| Retry e Circuit Breaker nas integrações | Retry pode recuperar falhas transitórias, mas uma política agressiva aumenta tráfego e latência; Circuit Breaker pode conter falhas em cascata, mas parâmetros inadequados podem bloquear uma dependência saudável. Composição e limiares continuam abertos. | [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos), [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) e [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) |
| Identidade e sessão | A DA-08 exige redução da dependência de estado local, mas provedor, protocolos, credenciais, expiração, revogação e mecanismo de sessão ainda não foram selecionados. | [ADR-008](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local) e [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) |
| Observabilidade | A DA-11 registra uma direção sem ADR fechado; SLI, retenção, rastreamento, alertas, plataforma e modelo operacional permanecem abertos. | [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) e [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) |
| Persistência futura especializada | A base SQL inicialmente compartilhada é a referência; persistência própria de Grades, réplicas e topologias de recuperação dependem de métricas, consistência, migração e nova decisão. | [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) e [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) |
| Fornecedor de vídeo | Delegar a transmissão é a decisão vigente; fornecedor, contrato, SLA, custos e limites operacionais continuam abertos. | [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa) e [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) |

## Leitura e rastreabilidade

Cada linha da tabela principal resume o benefício e o custo de uma decisão aceita na documentação; não cria uma nova decisão nem comprova implementação. O índice de [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) é a fonte do estado de DA-01–DA-11, e os [Architecture Decision Records](/home/02-Arquitetura/ADRs) registram contexto, alternativas, decisão, consequências e pontos abertos de DA-01–DA-09.

Os [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais) preservam as obrigações e metas; [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade) preserva os cenários de verificação; e [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos) preserva produtos, mecanismos e evoluções ainda não selecionados. DA-10 e DA-11 são direções com definição pendente e não possuem ADR nesta etapa.

A síntese do monólito modular e da decomposição por domínios trata de limites lógicos; ela não transforma módulos em unidades independentes de implantação.

## Referências

- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais)
- [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade)
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais)
- [Architecture Decision Records](/home/02-Arquitetura/ADRs)
- [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos)
- [Visão Geral da Arquitetura](/home/02-Arquitetura/Visao-Geral)
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes)
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios)
