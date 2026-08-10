# Requisitos Funcionais

## Capacidades base

O Prometheus V2 deve preservar a gestão de usuários, cursos, conteúdos, notas e relatórios do V1. A modernização original também acrescenta avaliações on-line, fóruns, videoconferências, acesso web externo, Portal de Pais e sincronização automática com o GRA. O material-base descreve essas capacidades sem atribuir códigos individuais a elas.

## Requisitos funcionais adicionais

Os doze requisitos abaixo mantêm a numeração original do material mais recente.

| Código | Requisito | Comportamento esperado |
|---|---|---|
| RF01 | Notificações *push* e mobile | Avisar estudantes e pais sobre notas, prazos e mensagens em fóruns. |
| RF02 | Ambiente de avaliação assistida | Detectar troca de aba ou perda de foco durante uma avaliação. |
| RF03 | *Dashboard* analítico | Mostrar a evolução da turma e identificar estudantes abaixo da média. |
| RF04 | Repositório de aulas gravadas | Disponibilizar gravações após as videoconferências. |
| RF05 | Assinatura digital de relatórios | Gerar PDFs de notas com QR Code de autenticidade. |
| RF06 | Gamificação | Conceder conquistas por conclusão de módulos ou desempenho superior a 90%. |
| RF07 | Agendamento automático de vídeo | Associar sessões ao calendário do curso e gerar links de acesso. |
| RF08 | Portfólio do estudante | Receber projetos e trabalhos visíveis a professores e responsáveis. |
| RF09 | Mural de avisos | Exibir anúncios e atualizações relevantes ao perfil autenticado. |
| RF10 | Recuperação automática de senha | Permitir recuperação por e-mail ou SMS. |
| RF11 | Exportação de dados | Exportar presença e desempenho em CSV ou Excel. |
| RF12 | Central de mensagens | Permitir comunicação assíncrona entre professores e estudantes. |

## Atores

Administradores cuidam de usuários e cursos; professores administram conteúdos, avaliações, notas e sessões; estudantes realizam atividades e consultam informações; pais ou responsáveis possuem acesso limitado aos estudantes a eles vinculados. Essa separação orienta portais e autorização, mas não determina uma tecnologia específica de autenticação.
