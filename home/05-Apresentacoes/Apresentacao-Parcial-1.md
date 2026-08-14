# Apresentação Parcial 1

## Estado do trabalho nas primeiras atividades

A análise inicial identificou o Prometheus V1 como uma aplicação monolítica em ASP clássico, com lógica C# em DLL, banco MySQL central, binários armazenados no banco e sessão mantida no servidor. Os principais problemas eram relatórios lentos, recuperação demorada de conteúdos, escalabilidade limitada e ausência de integração automática com o GRA.

As primeiras propostas foram retirar arquivos do banco, usar armazenamento especializado/CDN, modularizar usuários, cursos, conteúdos, notas e vídeo, integrar o GRA por REST e *webhooks* e migrar gradualmente.

Na Semana 2, a proposta ganhou portais por papel, APIs com responsabilidades definidas, Grades Bridge e processamento assíncrono de relatórios. Também foi estabelecida a diferença entre decisões arquiteturais — como módulos, persistência e integração — e decisões de design, como classes, bibliotecas e padrões internos.

## Ponto de validação

Ao final dessa etapa, a direção era um monólito modular centrado inicialmente em dados compartilhados. API REST foi entendida como contrato, não como sinônimo de microsserviço. Banco dedicado de notas, cache, autenticação por tokens e tecnologias específicas permaneciam propostas a validar.
