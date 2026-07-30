---
title: Visao-Geral
---
# Visão Geral da Arquitetura

A arquitetura proposta para o Prometheus V2 foi definida com o objetivo de modernizar o sistema preservando compatibilidade com a versão anterior e permitindo evolução incremental.

A solução organiza o sistema em módulos independentes por domínio de negócio, reduzindo o acoplamento entre funcionalidades e facilitando manutenção, testes e futuras expansões.

Os principais domínios identificados são:

- Usuários
- Cursos
- Conteúdo
- Notas
- Vídeos

Cada domínio possui responsabilidades bem definidas e comunicação por meio de APIs, favorecendo maior organização da aplicação.

A documentação arquitetural utiliza o modelo C4 para representar o sistema em diferentes níveis de abstração.