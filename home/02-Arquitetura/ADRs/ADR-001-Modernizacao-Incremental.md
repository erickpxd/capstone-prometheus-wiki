# ADR-001 — Modernização Incremental

## Status

Aceito na documentação arquitetural.

## Contexto

O Prometheus V1 é utilizado pela School X há aproximadamente quinze anos e mantém um acervo relevante de usuários, cursos, vínculos, notas, históricos e conteúdos. Uma substituição integral em uma única etapa aumentaria o risco de perda de dados, interrupção e regressão funcional.

## Decisão

Modernizar o Prometheus de forma incremental. Dados e funções devem migrar gradualmente, admitindo convivência entre o V1 e as partes modernizadas enquanto cada etapa é validada. Toda etapa deve preservar uma possibilidade de retorno compatível com seu escopo.

Esta decisão define a estratégia de transição; não estabelece cronograma nem comprova que a migração tenha começado.

## Alternativas consideradas

- **Substituição integral em uma única etapa:** não adotada devido ao risco sobre o acervo histórico, a continuidade operacional e a capacidade de retorno.

## Consequências

### Benefícios

- reduz a exposição a uma substituição total;
- permite validar dados e funções em etapas menores;
- preserva a possibilidade de retorno durante a transição.

### Custos e riscos

- a convivência temporária entre versões aumenta a complexidade;
- migração, validação e retorno precisam ser definidos para cada etapa;
- divergências entre dados ou comportamentos das versões precisam ser detectadas e tratadas.

## Relação com requisitos

- [RF-BASE-05 — Migração e conservação dos dados existentes](/home/01-Requisitos/Requisitos-Funcionais) é o requisito diretamente atendido por esta estratégia.

## Relação com outras decisões

- [DA-02 / ADR-002 — Monólito modular](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) define o estilo inicial para o qual a modernização evolui;
- [DA-03 / ADR-003 — Decomposição por domínios](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios) fornece limites para organizar etapas de evolução;
- as mudanças de dados e integrações registradas nas demais decisões devem respeitar a transição incremental.

## Pontos ainda abertos

A ordem e o escopo das etapas, os critérios de validação, os mecanismos de convivência e retorno e os responsáveis por cada transição ainda não foram definidos no material disponível.

## Evidência de implementação

Não confirmada no material disponível.
