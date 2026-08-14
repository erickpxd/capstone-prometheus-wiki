# Nível 1 — Contexto

## Leitura consolidada

O diagrama de contexto apresenta o Prometheus V2 como uma única unidade e o relaciona aos atores Administrador, Professor, Estudante e Pai ou Responsável. As dependências externas documentadas são o GRA, a plataforma de videoconferência e a infraestrutura de CDN/Object Storage usada pelos conteúdos.

O GRA troca informações de notas por API REST e *webhooks*. A plataforma externa oferece sessões e gravações. A presença da CDN/Object Storage registra uma dependência de infraestrutura do Prometheus; banco de dados, APIs, fila e workers continuam fora deste nível por serem detalhes internos da solução.

## Estado

![Captura_de_tela_de_2026-07-19_21-15-51](uploads/9d118ca185cda520cbcba2d2fcab4570/Captura_de_tela_de_2026-07-19_21-15-51.png){width=830 height=600}
