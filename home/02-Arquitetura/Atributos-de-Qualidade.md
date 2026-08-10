# Atributos de Qualidade

Os atributos de qualidade relacionam os problemas do V1 às decisões estruturais do V2.

| Atributo | Problema de origem | Resposta arquitetural |
|---|---|---|
| Segurança | Acesso externo e dados sensíveis | Criptografia, autorização por papel, MFA e auditoria |
| Manutenibilidade | Acoplamento e responsabilidades misturadas | Monólito modular, contratos e decomposição por domínio |
| Desempenho | PDFs e binários sobrecarregam o sistema | CDN, armazenamento dedicado e processamento em segundo plano |
| Escalabilidade | Escala vertical insuficiente | Redução de estado e escala horizontal seletiva |
| Disponibilidade | Picos e dependências externas | Meta de 99,9%, redundância e isolamento de falhas |
| Confiabilidade | Notas não podem ser perdidas ou duplicadas | Validação, transações, histórico e auditoria |
| Interoperabilidade | Transcrição manual para o GRA | REST, *webhooks* e contratos de integração |
| Observabilidade | Fluxos assíncronos difíceis de diagnosticar | Logs, métricas, rastreamento e monitoramento |
| Recuperação | Valor dos dados históricos | Cópias recuperáveis e plano de migração |

## Prioridade

A atividade mais recente ordena segurança, manutenibilidade, desempenho e escalabilidade. Disponibilidade e confiabilidade permanecem obrigatórias devido aos RNFs e à natureza dos registros acadêmicos.

Os atributos geram tensões. MFA aumenta segurança, mas adiciona passos; cache melhora latência, mas pode apresentar dados antigos; assincronia preserva responsividade, mas atrasa efeitos secundários. Esses compromissos são avaliados em [Trade-offs](../03-Trade-offs.md).
