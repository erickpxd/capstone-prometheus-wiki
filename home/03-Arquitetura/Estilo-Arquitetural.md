---
title: Estilo-Arquitetural
---
# Estilo Arquitetural

Após a análise das alternativas estudadas durante o projeto, foi adotada a estratégia de **Monólito Modular**.

Essa abordagem oferece um equilíbrio entre simplicidade de implementação e organização da aplicação, permitindo separar funcionalidades por domínio sem introduzir a complexidade operacional de uma arquitetura distribuída.

## Principais características

- Organização por módulos.
- Baixo acoplamento.
- Alta coesão.
- Evolução incremental.
- Facilidade de manutenção.

Essa decisão reduz riscos durante a migração do Prometheus V1 para o Prometheus V2 e permite futura evolução para microsserviços, caso necessário.