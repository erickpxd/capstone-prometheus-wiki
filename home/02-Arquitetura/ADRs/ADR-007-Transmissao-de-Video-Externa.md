# ADR-007 — Transmissão de Vídeo Externa

## Status

Aceito na documentação arquitetural.

## Contexto

O Prometheus precisa oferecer videoconferências vinculadas ao contexto acadêmico, incluindo agendamento, autorização, acesso e associação das gravações aos cursos. Transmitir áudio e vídeo, porém, exige uma capacidade especializada e sujeita o LMS à latência, indisponibilidade e mudanças de contrato de um serviço externo.

## Decisão

O Prometheus mantém o contexto acadêmico das sessões no módulo lógico Videos e delega a transmissão de áudio e vídeo a um **Video Provider externo**. O fornecedor permanece aberto.

A integração deve preservar os limites do Prometheus: agendamento, autorização, links e associação das gravações continuam sob seu controle. Falhas do provedor devem limitar as operações dependentes de vídeo, sem indisponibilizar cursos, conteúdos e notas.

## Alternativas consideradas

- **Transmitir áudio e vídeo dentro do Prometheus:** não adotada, pois reproduziria uma capacidade especializada e ampliaria a responsabilidade operacional do LMS.
- **Delegar a transmissão a um provedor externo:** adotada no nível arquitetural, sem selecionar produto ou fornecedor.

## Consequências

### Benefícios

- concentra o Prometheus nas regras e no contexto acadêmico;
- utiliza uma capacidade especializada para transmissão de mídia;
- permite isolar falhas externas das demais capacidades do LMS.

### Custos e riscos

- cria dependência do contrato, da latência e da disponibilidade do provedor;
- exige autorização, tratamento de falha e degradação controlada na integração;
- requer definição posterior de SLA, limites de tempo e tratamento das gravações.

Os trade-offs gerais estão consolidados em [Análise de Trade-offs](/home/03-Trade-offs).

## Relação com requisitos

- [RF-BASE-08](/home/01-Requisitos/Requisitos-Funcionais): agendamento e participação em videoconferências de cursos;
- [RF04](/home/01-Requisitos/Requisitos-Funcionais): disponibilização das gravações vinculadas ao curso;
- [RF07](/home/01-Requisitos/Requisitos-Funcionais): agendamento integrado ao calendário e criação externa do link.

## Relação com outras decisões

- complementa a decomposição por domínio do [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios), que atribui o contexto acadêmico ao módulo lógico Videos;
- aplica a direção de isolamento de falhas registrada na DA-10, sem confirmar uma topologia independente para Videos;
- não altera o monólito modular e a implantação inicialmente unificada definidos no [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular).

O índice completo está em [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais).

## Pontos ainda abertos

- fornecedor, contrato e SLA do Video Provider;
- autenticação, autorização e gestão dos links externos;
- limites de tempo, políticas de resiliência e critérios de degradação;
- armazenamento, retenção e disponibilização das gravações.

## Evidência de implementação

Não confirmada no material disponível.
