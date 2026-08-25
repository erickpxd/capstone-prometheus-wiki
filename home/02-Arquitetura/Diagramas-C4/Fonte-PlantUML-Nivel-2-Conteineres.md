# Fonte PlantUML — C4 Nível 2: Contêineres

> **Estado documental:** fonte editável equivalente ao diagrama canônico. Representa o monólito modular inicialmente implantado como uma unidade e não comprova implementação.

[Voltar ao C4 Nível 2 — Contêineres](/home/02-Arquitetura/Diagramas-C4/Nivel-2-Conteineres)

```plantuml
title Prometheus V2 — C4 Nível 2: Contêineres
left to right direction
skinparam shadowing false
skinparam defaultTextAlignment center
skinparam rectangle {
  RoundCorner 8
  BorderThickness 2
}

actor "Usuário autorizado" as Usuario
rectangle "GRA\nSistema externo" as GRA #EEEEEE
rectangle "Video Provider\nSistema externo; fornecedor aberto" as Video #EEEEEE

rectangle "Prometheus V2" as Limite #FFFFFF {
  rectangle "Aplicação modular\n<<Container>>\nMonólito modular; unidade inicial de implantação" as App #D9EAF7
  database "Banco SQL principal\nCompartilhado fisicamente;\npropriedade lógica por módulo" as SQL #E8F5E9
  queue "Fila / Message Broker\nAbstração; produto aberto" as Fila #FFF4E5
  storage "Object Storage / Report Storage\nAbstrações; fornecedor aberto" as Storage #FFF4E5
  rectangle "CDN\nAbstração; fornecedor aberto" as CDN #FFF4E5
}

note right of App
Módulos lógicos internos:
Users, Courses, Content, Grades e Videos

Responsabilidades lógicas internas:
Grades Bridge e Report Workers

Contratos não implicam comunicação de rede,
microsserviços ou implantação independente.
end note

Usuario --> App : usa o Portal Web e seus contratos
App --> SQL : persiste dados e estados
App --> Fila : publica relatórios e sincronizações tolerantes a atraso
Fila --> App : entrega trabalhos aos Report Workers\ne ao Grades Bridge internos
App --> Storage : grava e recupera conteúdos e PDFs autorizados
CDN --> Storage : obtém conteúdo da origem
CDN --> Usuario : entrega conteúdo autorizado
App --> GRA : sincroniza notas pelo Grades Bridge
GRA --> App : envia webhooks ao Grades Bridge
App --> Video : agenda e gerencia sessões por adaptador

note bottom of Storage
Conteúdo e relatórios podem exigir produtos e políticas distintas.
O agrupamento representa função, não uma escolha de produto único.
end note

legend bottom
  Azul: único contêiner de aplicação da implantação inicial
  Verde: persistência relacional vigente por abstração
  Laranja: capacidades de infraestrutura sem produto selecionado
  Cinza: sistemas externos
endlegend
```
