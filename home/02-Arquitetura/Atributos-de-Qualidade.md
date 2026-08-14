# Atributos de Qualidade

Os atributos de qualidade relacionam os problemas do V1 às decisões estruturais do V2.

| Atributo | Problema de origem | Resposta arquitetural |
|---|---|---|
| Segurança | Acesso externo e dados sensíveis | Criptografia, autorização por papel, MFA e auditoria |
| Manutenibilidade | Acoplamento e responsabilidades misturadas | Monólito modular, contratos e decomposição por domínio |
| Desempenho | PDFs e binários sobrecarregam o sistema | CDN, armazenamento dedicado e processamento em segundo plano |
| Escalabilidade | Escala vertical insuficiente | Redução de estado e escala horizontal seletiva |
| Disponibilidade | Picos, falhas de instância e dependências externas | Meta de 99,9%, redundância, health checks, failover e fallback |
| Confiabilidade | Notas não podem ser perdidas ou duplicadas | Validação, transações, histórico e auditoria |
| Interoperabilidade | Transcrição manual para o GRA | REST, *webhooks* e contratos de integração |
| Observabilidade | Fluxos assíncronos e falhas parciais difíceis de diagnosticar | Logs, métricas, rastreamento, health checks, alertas e testes externos |
| Recuperação | Valor dos dados históricos e falha de persistência | Cópias recuperáveis, plano de migração e estratégia de réplica condicionada |

## Cenários arquiteturais atuais

| Cenário | Resposta esperada | Verificação necessária |
|---|---|---|
| Uma instância preparada para redundância deixa de responder | O Load Balancer retira a instância não saudável e mantém o tráfego nas demais | Teste de health check e failover, incluindo a camada de balanceamento |
| Um Report Worker falha durante o processamento | O trabalho não confirmado permanece na fila durável e pode ser retomado | Teste de falha, repetição segura e ausência de relatório duplicado |
| O provedor de vídeo fica indisponível | O restante do LMS continua funcionando e a função afetada apresenta fallback | Teste de timeout e degradação elegante |
| O GRA fica indisponível após o registro de uma nota | A confirmação local é preservada e a sincronização ocorre posteriormente | Teste de novas tentativas, idempotência, auditoria e reconciliação |
| Até 500 PDFs são solicitados simultaneamente | A fila e os workers processam a carga sem degradar significativamente a navegação | Teste de carga com tempo de fila, erros e latência concorrente |
| Um conteúdo representativo é acessado externamente | Object Storage e CDN entregam o ativo em menos de dois segundos | Medição com tamanho, localização, rede e estado do cache definidos |

Uma janela de 30 dias transforma a meta de 99,9% em aproximadamente 43,2 minutos de indisponibilidade permitida. Isso é uma referência de comunicação, não substitui a definição do período, do escopo funcional, do SLI e das regras de cálculo.

## Prioridade

A consolidação das atividades coloca segurança, manutenibilidade, desempenho e escalabilidade entre as maiores preocupações. Disponibilidade e confiabilidade permanecem obrigatórias devido aos RNFs e à natureza dos registros acadêmicos. A proposta de alta disponibilidade da Semana 5 aprofunda Grades, Videos, workers e integrações, mas continua conceitual.

Os atributos geram tensões. MFA aumenta segurança, mas adiciona passos; cache melhora latência, mas pode apresentar dados antigos; assincronia preserva responsividade, mas atrasa efeitos secundários. Esses compromissos são avaliados em [Trade-offs](../03-Trade-offs.md).
