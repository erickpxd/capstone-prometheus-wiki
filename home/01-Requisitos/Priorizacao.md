# Priorização

## Critério

A priorização considera impacto sobre dados e operação, relação com os problemas do V1 e dificuldade de reversão das decisões arquiteturais. A atividade mais recente sobre atributos estabelece segurança, manutenibilidade, desempenho e escalabilidade como ordem principal.

| Prioridade | Grupo | Justificativa |
|---:|---|---|
| 1 | Segurança e privacidade | Uma falha pode expor dados pessoais, notas e registros institucionais. |
| 2 | Preservação e confiabilidade | O acervo histórico impede a substituição imediata do V1. |
| 3 | Manutenibilidade e modularidade | Limites claros evitam reproduzir o acoplamento do legado. |
| 4 | Desempenho | Conteúdos e relatórios já são gargalos concretos. |
| 5 | Escalabilidade e disponibilidade | A plataforma deve suportar picos e cumprir 99,9% de disponibilidade. |
| 6 | Integrações essenciais | GRA e vídeo habilitam funções centrais e removem trabalho manual. |
| 7 | Novas capacidades acadêmicas | Avaliações, fóruns, portais e requisitos adicionais entram de forma incremental. |

Prioridade não significa implantação simultânea. Segurança, preservação e limites modulares devem existir desde o início; gargalos de conteúdo e Grades são tratados nos primeiros incrementos; as novas funções entram sobre essa base.
