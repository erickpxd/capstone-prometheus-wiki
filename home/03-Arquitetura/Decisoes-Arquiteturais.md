---
title: Decisoes-Arquiteturais
---
# Decisões Arquiteturais

Durante o desenvolvimento do Prometheus V2 foram tomadas as seguintes decisões arquiteturais.

## Organização por Domínio

O sistema foi dividido em módulos responsáveis por domínios específicos:

- Usuários
- Cursos
- Conteúdo
- Notas
- Vídeos

---

## Comunicação por APIs

Os módulos comunicam-se através de APIs bem definidas, reduzindo o acoplamento entre componentes.

---

## Arquitetura Monólito Modular

Foi escolhida por equilibrar simplicidade de implementação, modularidade e facilidade de evolução.

---

## Documentação Arquitetural

Toda a documentação utiliza o modelo C4 para representar a arquitetura em diferentes níveis de abstração.