# Fonte PlantUML — Módulos e Responsabilidades

> **Estado documental:** fonte editável dos limites e das colaborações lógicas. As setas não representam chamadas de rede obrigatórias nem serviços independentes.

[Voltar a Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades)

```plantuml
title Prometheus V2 — Módulos e responsabilidades lógicas
top to bottom direction
skinparam shadowing false
skinparam componentStyle rectangle
skinparam component {
  BackgroundColor<<modulo>> #D9EAF7
  BorderColor<<modulo>> #2563A6
  BackgroundColor<<responsabilidade>> #FFF4E5
  BorderColor<<responsabilidade>> #C76B00
}

rectangle "Aplicação modular — unidade inicial de implantação" as Aplicacao {
  component "Users" as Users <<modulo>>
  component "Courses" as Courses <<modulo>>
  component "Content" as Content <<modulo>>
  component "Grades" as Grades <<modulo>>
  component "Videos" as Videos <<modulo>>

  component "Identidade\nresponsabilidade transversal;\ntecnologia aberta" as Identidade <<responsabilidade>>
  component "Matrículas\nassociada a Courses;\nfronteira técnica aberta" as Matriculas <<responsabilidade>>
  component "Comunicação\nfóruns, avisos e mensagens;\nfronteira final aberta" as Comunicacao <<responsabilidade>>
}

Users --> Courses : perfis e vínculos institucionais
Courses --> Content : curso e escopo acadêmico
Courses --> Grades : curso e turma
Courses --> Videos : curso, participantes e agenda
Courses --> Comunicacao : contexto do curso
Courses ..> Matriculas : administra vínculo acadêmico
Grades ..> Matriculas : valida participação
Grades --> Comunicacao : origina comunicações acadêmicas
Videos --> Content : associa gravações referenciadas

note right of Identidade
Aplica-se aos fluxos protegidos
de todos os módulos.
Autenticação não substitui
a autorização contextual.
end note

note bottom of Aplicacao
módulo lógico ≠ API REST ≠ microsserviço
≠ unidade independente de implantação

Todos os elementos coexistem inicialmente na mesma unidade.
As setas representam colaborações e contratos lógicos.
end note
```
