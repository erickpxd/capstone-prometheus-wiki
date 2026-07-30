---
title: Estrategias-Avaliadas
---
# Estratégias Arquiteturais Avaliadas

Durante o reprojeto do Prometheus V2 foram estudadas diferentes estratégias arquiteturais antes da definição da solução final.

| Estratégia | Vantagens | Desvantagens |
|------------|-----------|--------------|
| Arquitetura em Camadas | Simples implementação e boa organização | Maior acoplamento entre módulos |
| Monólito Modular | Alta modularidade, menor complexidade operacional e fácil evolução | Escalabilidade limitada em relação aos microsserviços |
| Arquitetura Orientada a Serviços | Escalabilidade e independência dos serviços | Maior complexidade operacional |

## Estratégia Selecionada

Após a análise dos requisitos e do contexto do projeto, foi escolhida a arquitetura de **Monólito Modular**.

Essa estratégia oferece um equilíbrio entre simplicidade, organização do código e facilidade de manutenção, permitindo futura evolução para uma arquitetura distribuída caso necessário.