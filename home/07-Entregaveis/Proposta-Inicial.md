---
title: Proposta-Inicial
---
# Proposta Inicial

## Objetivo

Modernizar a arquitetura do Prometheus V1 preservando suas funcionalidades e melhorando atributos de qualidade essenciais para sua evolução.

## Principais Problemas Identificados

- Forte acoplamento entre módulos.
- Baixa escalabilidade.
- Dificuldade de manutenção.
- Arquitetura pouco modular.

## Solução Proposta

Foi proposta uma arquitetura baseada em **Monólito Modular**, organizada por domínios de negócio e documentada utilizando o modelo C4.

Essa abordagem reduz o acoplamento, melhora a organização da aplicação e permite evolução incremental com menor risco durante a migração do sistema.