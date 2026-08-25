# Fonte PlantUML — C4 Nível 1: Contexto

> **Estado documental:** fonte editável reconstruída a partir da leitura canônica do C4 Nível 1. Não é a fonte original do PNG e não comprova implementação ou implantação.

[Voltar ao C4 Nível 1 — Contexto](/home/02-Arquitetura/Diagramas-C4/Nivel-1-Contexto)

```plantuml
title Prometheus V2 — C4 Nível 1: Contexto
left to right direction
skinparam shadowing false
skinparam defaultTextAlignment center
skinparam rectangle {
  RoundCorner 8
  BorderThickness 2
}

actor "Administrador" as Admin
actor "Professor" as Professor
actor "Estudante" as Estudante
actor "Pai ou responsável" as Responsavel

rectangle "Prometheus V2\nSistema de gestão educacional da School X" as Prometheus #D9EAF7
rectangle "GRA\nSistema externo de gestão de notas" as GRA #EEEEEE
rectangle "Provedor de videoconferência\nSistema externo; fornecedor aberto" as Video #EEEEEE
rectangle "Infraestrutura de conteúdo\nObject Storage e CDN — abstrações" as Conteudo #EEEEEE

Admin --> Prometheus : gerencia usuários, perfis, cursos e turmas
Professor --> Prometheus : gerencia conteúdos, avaliações, notas,\nfóruns e sessões ao vivo
Estudante --> Prometheus : acessa conteúdos, avaliações, notas,\nfóruns e sessões ao vivo
Responsavel --> Prometheus : consulta dados autorizados\ndo estudante vinculado

Prometheus --> GRA : sincroniza notas pelo contrato externo
GRA --> Prometheus : envia webhooks / notificações
Prometheus --> Video : cria e gerencia o contexto das sessões
Prometheus --> Conteudo : armazena e distribui conteúdo autorizado

note bottom of Prometheus
Arquitetura proposta e vigente na documentação.
Não comprova implementação ou implantação.
end note

note bottom of GRA
Autenticação, fonte de autoridade e regras
de conflito ainda não estão definidas.
end note

legend bottom
  Pessoas: atores do LMS
  Azul: Prometheus V2 como um único sistema
  Cinza: dependências externas ou de infraestrutura
endlegend
```
