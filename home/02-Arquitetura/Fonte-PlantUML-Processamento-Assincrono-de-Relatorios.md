# Fonte PlantUML — Processamento Assíncrono de Relatórios

> **Estado documental:** fonte editável do fluxo canônico de relatórios. Não seleciona broker, produto, padrão auxiliar ou topologia independente.

[Voltar a Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios)

```plantuml
title Prometheus V2 — Processamento assíncrono de relatórios
skinparam shadowing false
autonumber

actor "Usuário autorizado" as Usuario
boundary "Portal Web" as Portal
control "Grades\nmódulo lógico" as Grades
database "Banco SQL principal\nregistro e estados" as SQL
participant "Fila\n<<abstração de mensageria>>" as Fila
participant "Report Worker\n<<responsabilidade lógica>>" as Worker
participant "Report Storage / Object Storage\n<<abstração>>" as Storage

Usuario -> Portal : solicita relatório
Portal -> Grades : envia parâmetros e contexto
Grades -> Grades : valida autenticação, autorização,\nvínculo e parâmetros
Grades -> SQL : registra trabalho com identidade estável\ne estado Pendente
SQL --> Grades : trabalho registrado
Grades -> Fila : publica trabalho
Grades --> Portal : devolve referência rastreável
Portal --> Usuario : apresenta estado Pendente

note over SQL, Fila
A coordenação entre registro e publicação deve impedir
confirmação de trabalho impossível de localizar/processar.
O mecanismo técnico ainda não foi selecionado.
end note

Fila -> Worker : entrega trabalho
Worker -> SQL : marca Em processamento\ne lê dados acadêmicos autorizados
note right of Worker
Reentrega ou repetição deve ser idempotente:
a mesma identidade não produz duplicação visível.
end note

alt geração e persistência concluídas
  Worker -> Worker : gera PDF
  Worker -> Storage : grava resultado
  Storage --> Worker : confirma persistência
  Worker -> SQL : marca Concluído e salva referência
else falha antes da conclusão confirmada
  Worker -> SQL : preserva estado e diagnóstico disponível
  note over Fila, SQL
  O trabalho permanece recuperável para tentativa controlada
  ou tratamento posterior. Limites e semântica permanecem abertos.
  end note
end

== Consulta posterior ==
Usuario -> Portal : consulta estado ou solicita download
Portal -> Grades : consulta pela referência
Grades -> SQL : lê estado e autoria
alt estado Concluído e acesso autorizado
  Grades -> Storage : recupera PDF
  Storage --> Grades : resultado
  Grades --> Portal : disponibiliza download autorizado
  Portal --> Usuario : entrega PDF
else Pendente, Em processamento ou Falha controlada
  Grades --> Portal : informa o estado atual sem sucesso fictício
  Portal --> Usuario : apresenta atraso ou falha controlada
end
```
