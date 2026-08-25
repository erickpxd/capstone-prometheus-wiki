# ADR-008 — Redução de Estado Local

## Status

Aceito na documentação arquitetural.

## Contexto

O Prometheus V1 mantém sessão local no servidor. A dependência exclusiva do estado de uma instância dificulta distribuir tráfego, recuperar uma requisição em outra instância e evoluir a implantação quando houver necessidade comprovada de escala ou disponibilidade.

## Decisão

O Prometheus V2 deve reduzir a dependência de estado mantido exclusivamente na memória local de uma instância. O estado necessário à continuidade de uma interação deve usar um mecanismo que permita a outra instância atender o usuário quando a topologia assim exigir.

A decisão estabelece a restrição arquitetural, mas não seleciona mecanismo de sessão, produto de cache, protocolo de identidade nem formato de credencial.

## Alternativas consideradas

- **Manter sessão exclusivamente na memória da instância:** não adotada como referência do V2, pois limita balanceamento e recuperação por outra instância.
- **Reduzir o estado local com mecanismo compartilhável ou verificável por outra instância:** adotada como direção arquitetural; a solução concreta permanece aberta.

## Consequências

### Benefícios

- facilita a distribuição de tráfego entre instâncias quando necessária;
- permite que outra instância assuma interações após uma falha;
- prepara a evolução da implantação sem transformar os módulos em microsserviços.

### Custos e riscos

- exige definir segurança, expiração, revogação e consistência do estado;
- pode introduzir dependência de infraestrutura ou de um provedor de identidade;
- demanda migração cuidadosa dos comportamentos de sessão do legado.

Os trade-offs gerais estão consolidados em [Análise de Trade-offs](/home/03-Trade-offs).

## Relação com requisitos

- [RF-BASE-09](/home/01-Requisitos/Requisitos-Funcionais): acesso web externo para usuários autorizados;
- [RNF01](/home/01-Requisitos/Requisitos-Nao-Funcionais): disponibilidade e resiliência.

## Relação com outras decisões

- preserva o monólito modular e a implantação inicialmente unificada definidos no [ADR-002](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular);
- prepara a direção de disponibilidade seletiva registrada na DA-10 sem aprovar topologia ativa/ativa, Load Balancer ou implantação independente.

O índice completo está em [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais).

## Pontos ainda abertos

- mecanismo de sessão e integração com identidade;
- Identity Provider, protocolos e formato das credenciais;
- expiração, revogação, invalidação e proteção do estado;
- topologia, operação e métricas que justificariam múltiplas instâncias.

## Evidência de implementação

Não confirmada no material disponível.
