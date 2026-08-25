# Atributos de Qualidade

## Estado documental e regra de precedência

Esta página interpreta os requisitos não funcionais da arquitetura proposta do Prometheus V2 como cenários de qualidade verificáveis. Ela não cria ou redefine RNFs, não substitui decisões arquiteturais e não comprova implementação.

A precedência documental é:

- o RNF define a obrigação ou meta;
- esta página traduz a obrigação em estímulo, ambiente, resposta esperada e medida de verificação;
- DA e ADR registram a decisão arquitetural;
- Trade-offs registra a relação entre benefício e custo;
- Tecnologias e Pontos Abertos registra produtos e mecanismos ainda não selecionados.

Na tabela, **VIGENTE** indica um comportamento ou cenário sustentado pelos requisitos ou pela arquitetura aceita na documentação; não significa implementação comprovada. **CONDICIONADO** identifica um cenário cuja estratégia concreta depende de decisões futuras.

## Cenários de qualidade

| Atributo | Estímulo | Ambiente | Resposta esperada | Medida/verificação | RNF | DA/ADR | Status |
|---|---|---|---|---|---|---|---|
| Desempenho | Usuário autorizado solicita conteúdo acadêmico em acesso externo. | Não especificado | O conteúdo é entregue respeitando a autorização e a arquitetura de armazenamento e entrega definida. | Menos de dois segundos, conforme o RNF03. | [RNF03](/home/01-Requisitos/Requisitos-Nao-Funcionais) | [DA-04](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco) | **VIGENTE** |
| Escalabilidade e desempenho | São solicitados até 500 PDFs simultâneos. | Não especificado | A geração é processada de forma assíncrona, com isolamento da carga em relação à navegação. | Verificar que a navegação não sofre degradação significativa, conforme o RNF05; observar, sem metas ainda definidas, percentis da navegação, fila, throughput, erros e saturação das dependências. | [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) | [DA-05 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios); [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) | **VIGENTE** |
| Resiliência e interoperabilidade | O GRA está lento ou indisponível durante a sincronização de uma nota já registrada localmente. | Não especificado | O registro local confirmado não é bloqueado; a sincronização posterior preserva idempotência, auditoria, tentativas controladas e reconciliação. | Verificar a confirmação local, a retomada da sincronização e a ausência de notas duplicadas. | [RNF04](/home/01-Requisitos/Requisitos-Nao-Funcionais) | [DA-06 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge); [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) | **VIGENTE** |
| Confiabilidade e resiliência | Um Report Worker falha durante o processamento. | Não especificado | O trabalho permanece recuperável para reprocessamento sem corromper o estado do relatório. | Verificar reprocessamento seguro, integridade do estado e ausência de duplicação visível. | [RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) | [DA-05 e DA-09](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios); [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) | **VIGENTE** |
| Resiliência | O Video Provider fica indisponível. | Não especificado | A indisponibilidade da transmissão externa é isolada da navegação e das demais capacidades independentes do LMS. | Verificar que cursos, conteúdos e notas permanecem disponíveis quando a transmissão externa falha. | Não há RNF específico documentado | [DA-07](/home/02-Arquitetura/Decisoes-Arquiteturais); [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa); [DA-10 — direção relacionada](/home/02-Arquitetura/Decisoes-Arquiteturais) | **VIGENTE** |
| Disponibilidade e resiliência | Ocorre uma falha isolada de execução. | Não especificado | Uma falha isolada de execução não deve comprometer desnecessariamente as capacidades independentes do LMS. | Verificação ainda não definida; depende de decisões futuras de implantação, RPO/RTO, redundância e operação. | [RNF01](/home/01-Requisitos/Requisitos-Nao-Funcionais) | [DA-10 e DA-11 — direções relacionadas](/home/02-Arquitetura/Decisoes-Arquiteturais) | **CONDICIONADO** |

## Lacunas de medição

Os ambientes necessários aos seis cenários não estão documentados; por isso, permanecem como “Não especificado”. As lacunas abaixo não criam novos requisitos e não devem ser preenchidas por inferência.

| Atributo | Estado atual | Lacuna |
|---|---|---|
| Desempenho de conteúdo | O RNF03 define a meta e a arquitetura registra a forma de armazenamento e entrega. | Tamanho e tipo do arquivo, localização, região, condições de rede, estado do cache e percentil. |
| Escalabilidade de relatórios | O RNF05 define a carga; processamento assíncrono e isolamento são respostas vigentes. | Duração, mix e tamanho dos relatórios, limite quantitativo de degradação, taxa de erro, throughput, ambiente e capacidade dos Report Workers. O protocolo candidato deve comparar um baseline de navegação, observar p50/p95/p99, profundidade e idade da fila, CPU, memória, conexões e saturação do SQL e variar a quantidade de workers sem presumir ganho linear. |
| Resiliência | Os comportamentos de GRA, Report Worker e Video Provider são sustentados por requisitos ou decisões. | GRA: autenticação, autoridade, conflitos, timeout e limites de tentativas; relatórios: semântica de entrega e reprocessamento; vídeo: fornecedor, SLA, timeout e políticas de resiliência. |
| Disponibilidade | O RNF01 define a obrigação; o cenário de falha isolada permanece condicionado. | Ambiente e estratégia concreta de implantação, redundância e operação. |
| Segurança | O RNF02 define controles verificáveis, mas o cenário operacional completo não está definido. | Protocolos e algoritmos, gestão de chaves, identidade e sessão, fluxo de MFA, matriz de autorização, modelo de ameaças, ambiente e evidência de teste. |
| Usabilidade | Não há RNF formal de usabilidade no catálogo atual. | Tarefas críticas, acessibilidade, dispositivos, perfis de usuário, métricas, cenário e critérios de aceitação ainda precisam ser descobertos; esta lacuna não cria um novo RNF. |
| Manutenibilidade | É um atributo importante e possui decisões arquiteturais relacionadas. | Cenário e medida verificável de manutenção e evolução. |
| Modularidade | DA-02 e DA-03 registram o estilo e os limites lógicos. | Critério verificável de governança dos limites e contratos entre módulos. |
| Observabilidade | DA-11 define uma direção arquitetural, mas a estratégia concreta permanece aberta. | SLI, retenção, escopo de rastreamento, limiares de alerta, plataforma e modelo operacional. |
| Recuperação | O [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) registra migração incremental, validação e possibilidade de retorno. | RPO, RTO, backup, restauração, replicação, failover e recuperação de desastre. |

## Referências relacionadas

- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais)
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais)
- [Architecture Decision Records](/home/02-Arquitetura/ADRs)
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes)
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios)
- [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos)
- [Análise de Trade-offs](/home/03-Trade-offs)
