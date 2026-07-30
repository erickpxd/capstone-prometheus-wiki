---
title: Trade-offs
---
# Análise de Trade-offs

A escolha da arquitetura considerou diferentes atributos de qualidade e seus impactos no desenvolvimento do Prometheus V2.

| Critério | Monólito Modular |
|----------|------------------|
| Modularidade | Alta |
| Escalabilidade | Média |
| Manutenibilidade | Alta |
| Facilidade de Implementação | Alta |
| Complexidade Operacional | Baixa |

## Benefícios

- Separação clara por domínio.
- Baixo custo de implantação.
- Menor complexidade de infraestrutura.
- Facilidade de manutenção.
- Evolução incremental.

## Limitações

- Escalabilidade inferior à de microsserviços.
- Implantação única da aplicação.
- Compartilhamento de recursos entre módulos.