# Decisões Arquiteturais

> **Estado documental:** este é o índice canônico das decisões aceitas na documentação arquitetural do Prometheus V2. O status não comprova implementação.

## Inventário

| ID | Decisão | Status | ADR | Observação |
|---|---|---|---|---|
| DA-01 | Modernização incremental | Vigente na documentação | [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) | Preserva dados históricos por migração gradual, convivência, validação e possibilidade de retorno. |
| DA-02 | Monólito modular | Vigente na documentação | [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) | Define o estilo de referência e a implantação inicial unificada. |
| DA-03 | Decomposição por domínio | Vigente na documentação | [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) | Organiza regras, contratos e propriedade lógica sem criar serviços independentes. |
| DA-04 | Conteúdos e binários fora do banco principal | Vigente na documentação | [ADR-004](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco) | Mantém metadados e autorização no Prometheus e desloca os arquivos para armazenamento dedicado. |
| DA-05 | Isolamento da carga de Grades e relatórios assíncronos | Vigente na documentação | [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios) | Retira a geração de PDFs do fluxo web; persistência própria de Grades não foi decidida. |
| DA-06 | Integração automatizada com o GRA | Vigente na documentação | [ADR-006](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge) | Usa Grades Bridge e sincronização assíncrona sem fechar autoridade ou política de conflito. |
| DA-07 | Videoconferência externa | Vigente na documentação | [ADR-007](/home/02-Arquitetura/ADRs/ADR-007-Transmissao-de-Video-Externa) | O Prometheus mantém o contexto acadêmico e delega a transmissão; fornecedor aberto. |
| DA-08 | Redução de estado local | Vigente na documentação | [ADR-008](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local) | Evita dependência exclusiva da memória de uma instância; mecanismo aberto. |
| DA-09 | Assincronia seletiva | Vigente na documentação | [ADR-009](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva) | Aplica assincronia a efeitos tolerantes a atraso e preserva confirmações acadêmicas síncronas. |
| DA-10 | Disponibilidade seletiva e isolamento de falhas | Direção vigente; definição pendente | — | Ativo/ativo, *primary/standby*, failover e implantação isolada continuam propostas condicionadas. |
| DA-11 | Observabilidade orientada à operação | Direção vigente; definição pendente | — | A capacidade é requerida, mas SLI, retenção, plataforma, rastreamento e modelo operacional permanecem abertos. |

DA-10 e DA-11 permanecem rastreáveis neste índice, mas não recebem ADR nesta etapa. Suas direções são válidas; as estratégias concretas ainda precisam de alternativas comparáveis, critérios e decisão explícita.

## Arquitetura e design

Uma decisão de arquitetura altera limites, persistência, integração, implantação ou comportamento transversal e costuma ter consequências amplas ou difíceis de reverter. Uma decisão de design detalha a organização interna de uma solução com impacto mais localizado; sua classificação pode mudar se o alcance se tornar arquitetural.

A presença de uma tecnologia ou produto em laboratório, apresentação ou diagrama preliminar não constitui, por si só, uma decisão arquitetural. Exemplos estudados e escolhas pendentes permanecem em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos); benefícios e custos gerais permanecem em [Análise de Trade-offs](/home/03-Trade-offs).

Os fundamentos e as consequências específicas de DA-01 a DA-09 estão nos [Architecture Decision Records](/home/02-Arquitetura/ADRs).
