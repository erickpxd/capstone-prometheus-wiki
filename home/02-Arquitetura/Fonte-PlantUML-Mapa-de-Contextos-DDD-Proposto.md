# Fonte PlantUML — Mapa de Contextos DDD Proposto

> **Estado documental:** hipótese acadêmica a validar com a School X. O mapa não redefine a arquitetura vigente nem transforma contextos em serviços, bancos ou unidades de implantação.

[Voltar a Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades)

```plantuml
title Prometheus V2 — Mapa de contextos DDD proposto (hipótese a validar)
top to bottom direction
skinparam shadowing false
skinparam componentStyle rectangle
skinparam component {
  BackgroundColor<<core>> #FFE0B2
  BorderColor<<core>> #E65100
  BackgroundColor<<supporting>> #DCEDC8
  BorderColor<<supporting>> #558B2F
  BackgroundColor<<generic>> #DBEAFE
  BorderColor<<generic>> #2563EB
  BackgroundColor<<auxiliar>> #F3E8FF
  BorderColor<<auxiliar>> #7E22CE
}

actor Administrador as Admin
actor Professor
actor Estudante
actor "Pai ou responsável" as Responsavel

rectangle "Prometheus V2 — monólito modular; implantação inicial unificada" {
  component "Portal Web\ncanal de apresentação; não é BC" as PortalWeb <<auxiliar>>
  component "Portal de Pais\ncanal de apresentação; não é BC" as PortalPais <<auxiliar>>
  component "Grades Bridge\nACL/adaptador; não é BC" as Bridge <<auxiliar>>

  rectangle "Contextos delimitados propostos" as Contextos {
    component "Pessoas e Perfis\nSupporting" as People <<supporting>>
    component "Identidade e Acesso\nGeneric" as IAM <<generic>>
    component "Oferta Acadêmica e Matrículas\nCore — hipótese" as Courses <<core>>
    component "Avaliações e Resultados\nCore — hipótese" as Grades <<core>>
    component "Conteúdo Didático\nSupporting" as Content <<supporting>>
    component "Comunicação Acadêmica\nSupporting — preliminar" as Comm <<supporting>>
    component "Sessões ao Vivo\nSupporting" as Live <<supporting>>
    component "Entrega de Notificações\nGeneric — preliminar" as Notify <<generic>>
  }
}

rectangle "GRA\nsistema externo" as GRA #EEEEEE
rectangle "Provedor de videoconferência\nsistema externo" as Video #EEEEEE
rectangle "Object Storage / CDN\ninfraestrutura" as Files #EEEEEE
rectangle "E-mail / SMS / Push\ncanais externos" as Channels #EEEEEE

Admin --> PortalWeb : administra
Professor --> PortalWeb : ensina
Estudante --> PortalWeb : aprende
Responsavel --> PortalPais : acompanha estudante vinculado

PortalWeb --> IAM : autenticação
PortalWeb --> Contextos : orquestra casos de uso autorizados
PortalPais --> IAM : login e MFA
PortalPais --> People : valida vínculo responsável–estudante
PortalPais --> Grades : consulta autorizada de notas
PortalPais ..> Content : portfólio, se confirmado

note right of IAM
Fornece identidade autenticada aos fluxos protegidos.
A autorização contextual permanece no contexto
que possui a regra de negócio.
end note
People --> Courses : U→D — identidades e vínculos
Courses --> Grades : U→D — curso, turma e matrícula
Courses --> Content : curso e escopo de acesso
Courses --> Comm : participantes e contexto
Courses --> Live : participantes e agenda
Grades --> Bridge : nota confirmada / sincronização
Bridge --> GRA : ACL — tradução e reconciliação
Grades --> Notify : nota publicada
Courses --> Notify : prazo próximo
Comm --> Notify : intenção de entrega
Content ..> Files : porta de armazenamento e distribuição
Live ..> Video : adaptador externo
Notify ..> Channels : adaptadores de canal

note as AvisoDDD
Esta classificação é uma hipótese acadêmica.
Contexto delimitado não implica API, banco,
microsserviço ou unidade de implantação.

Core/Supporting/Generic e as fronteiras de Comunicação,
Notificações e Portfólio exigem validação com a School X.
end note
```
