# ADR-002 — Monólito Modular

## Status

Aceito na documentação arquitetural.

## Contexto

O Prometheus V1 possui responsabilidades misturadas e alto acoplamento, mas a modernização também precisa preservar dados e evoluir gradualmente. Serviços independentes foram estudados, porém introduziriam desde o início distribuição, coordenação operacional e maior complexidade de implantação.

## Decisão

Adotar um **monólito modular inicialmente implantado como uma unidade**. A aplicação deve possuir limites lógicos e contratos explícitos, sem presumir processos, bancos ou ciclos de implantação independentes para seus módulos.

Módulo lógico e contrato de API não significam microsserviço. Uma extração futura somente será considerada quando métricas demonstrarem que necessidades próprias de escala, disponibilidade ou evolução justificam o custo da distribuição.

## Alternativas consideradas

- **Serviços com implantação independente desde o início:** alternativa estudada e não adotada como estilo de referência, devido ao custo operacional e de coordenação durante a modernização.

## Consequências

### Benefícios

- combina limites de responsabilidade com implantação inicial mais simples;
- reduz a complexidade distribuída durante a modernização incremental;
- permite evoluir contratos e modularidade antes de decidir por extrações.

### Custos e riscos

- os módulos possuem menor independência de implantação e escala;
- ampliar uma capacidade pode exigir inicialmente replicar a aplicação inteira;
- limites mal governados podem reproduzir o acoplamento do legado dentro do monólito.

## Relação com requisitos

- [RF-BASE-05](/home/01-Requisitos/Requisitos-Funcionais) exige preservação dos dados durante a modernização;
- [RNF01 e RNF05](/home/01-Requisitos/Requisitos-Nao-Funcionais) motivam escalabilidade e disponibilidade, sem exigir serviços independentes na implantação inicial.

## Relação com outras decisões

- [DA-01 / ADR-001 — Modernização incremental](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) define a estratégia de transição;
- [DA-03 / ADR-003 — Decomposição por domínios](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) define os limites lógicos dentro da unidade de implantação;
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) é a fonte canônica da distinção entre módulo, API, microsserviço e unidade de implantação.

## Pontos ainda abertos

As métricas e os limiares que justificariam uma futura extração, assim como a topologia dessa evolução, ainda não foram definidos. Nenhum módulo possui implantação independente confirmada.

## Evidência de implementação

Não confirmada no material disponível.
