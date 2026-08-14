# 4. Cronograma e Gestão de Riscos

Os materiais não definem datas ou duração para a implementação. Por isso, o cronograma é organizado por marcos e critérios de conclusão, sem inventar calendário.

## Marco 1 — Conhecer e proteger o legado

Inventariar dados, regras, fluxos e integrações manuais; criar cópias recuperáveis; medir o desempenho atual de relatórios e conteúdos. O marco termina quando existe uma linha de base e um caminho de retorno testado.

## Marco 2 — Estabelecer os limites modulares

Criar os contratos de Users, Courses, Content, Grades e Videos e os controles de autenticação, autorização, auditoria e observabilidade. Os módulos podem usar a base central, mas não devem modificar diretamente os dados alheios.

## Marco 3 — Retirar binários do banco

Introduzir armazenamento dedicado, migrar ativos com verificação de integridade, conservar metadados em Content e habilitar CDN. O critério de saída inclui conteúdo representativo carregado em menos de dois segundos fora da rede interna.

## Marco 4 — Isolar avaliações, notas e relatórios

Decompor os serviços internos de Grades, retirar PDFs do fluxo síncrono e validar até 500 relatórios simultâneos sem degradar a navegação. A persistência exclusiva somente é avaliada após essas medições.

## Marco 5 — Automatizar o GRA

Implementar Grades Bridge, REST, *webhooks* e mensageria. Demonstrar que a indisponibilidade do GRA não bloqueia o professor e que repetição de mensagens não duplica dados.

## Marco 6 — Entregar portais e novas capacidades

Disponibilizar acesso por papel, avaliações, fóruns, Portal de Pais com MFA e videoconferência. Notificações, análises, exportação, gravações, portfólio e mensagens entram nos módulos correspondentes em incrementos posteriores.

## Marco 7 — Validar e decidir evoluções

Medir disponibilidade, segurança, latência, carga e manutenção. Exercitar falha de instância, worker, GRA e provedor de vídeo; validar health checks, failover, fallback, alertas e a perspectiva externa do usuário. Um módulo só é extraído para implantação independente quando os resultados demonstrarem benefício superior ao custo operacional.

Antes de adotar persistência *primary/standby* para Grades, devem ser definidos RPO, RTO, modo de replicação, critérios de promoção e procedimento de reconexão. A meta global de 99,9% também precisa de janela, funções incluídas, SLI e exclusões formalizados.

## Estratégia de migração

Usuários, cursos, vínculos e notas precisam manter identificadores ou mapeamento rastreável entre V1 e V2. Contagens, relações e amostras devem ser comparadas. Notas exigem precisão, autoria, histórico e consistência com o GRA.

Para cada binário, a migração cria o objeto no armazenamento dedicado, verifica integridade, registra a referência e testa autorização. O arquivo só é removido do banco depois de validação e período de segurança.

Durante a convivência, deve existir uma fonte de autoridade para cada dado. Uma função só assume o tráfego quando dados, segurança, comportamento, desempenho e retorno estiverem verificados.

## Riscos

| Risco | Impacto | Mitigação |
|---|---|---|
| Perda de dados históricos | Muito alto | Inventário, cópias recuperáveis, lotes validados e retorno |
| Reprodução do acoplamento | Alto | Propriedade lógica, interfaces e testes arquiteturais |
| Relatórios degradarem a navegação | Alto | Fila, *worker*, métricas e escala de Grades |
| Trabalho de relatório ser perdido ou duplicado | Alto | Fila durável, confirmação, idempotência e armazenamento rastreável |
| Cache apresentar relatório acadêmico desatualizado | Alto | Invalidação por mudança de nota, publicação e regra de cálculo |
| Indisponibilidade do GRA | Alto | Grades Bridge, mensagens persistidas e novas tentativas |
| Falha da plataforma de vídeo | Médio/alto | Timeout, fallback, mensagem explícita e continuidade das demais funções |
| Exposição de dados | Muito alto | Criptografia, autorização, MFA e auditoria |
| Inconsistência entre metadado e arquivo | Alto | Verificação de integridade e política de retenção |
| Banco central continuar como gargalo | Alto | CDN, medição e avaliação de persistência dedicada |
| Mensagens duplicadas ou perdidas | Alto | Idempotência, rastreamento e reconciliação |
| Distribuição prematura da arquitetura | Médio/alto | Critérios mensuráveis antes da extração |
| Load Balancer tornar-se ponto único de falha | Alto | Camada redundante ou serviço com alta disponibilidade comprovada |
| Failover de Grades causar perda ou recuperação lenta | Muito alto | Definir RPO/RTO, testar replicação, promoção e reconexão |
| Falha ser detectada tarde | Alto | Health checks, métricas, alertas e monitoramento externo |
| Mudança de contrato externo | Médio | Adaptadores isolados e plano de substituição |

Os riscos devem ser reavaliados a cada marco. Migração e segurança bloqueiam uma liberação quando recuperação e proteção não tiverem sido demonstradas.
