# Análise de Lacunas

## Estado documental

Esta página é a fonte canônica do diagnóstico do Prometheus V1 e das necessidades que motivam o V2. Os fatos sobre o legado não comprovam implementação do V2; as respostas arquiteturais vigentes estão nas páginas especializadas.

## Contexto do Prometheus V1

O Prometheus V1 é o LMS utilizado pela School X há aproximadamente quinze anos. Administradores gerenciam usuários e cursos, professores publicam conteúdos e registram notas, e estudantes consultam materiais e resultados acadêmicos. O tempo de operação produziu um acervo histórico relevante, razão pela qual a substituição imediata do sistema não é considerada viável.

O sistema foi construído como uma aplicação web monolítica restrita à rede interna. A interface utiliza ASP clássico, a lógica de negócio está reunida em uma biblioteca/DLL C# e um banco MySQL centralizado armazena dados estruturados e arquivos binários. A aplicação também mantém estado de sessão no servidor.

## Lacunas de desempenho

A geração de relatórios de notas em PDF é lenta, principalmente quando envolve grande número de estudantes. O processamento ocorre no mesmo ambiente que atende às demais funções e, portanto, pode degradar a navegação. A recuperação de apresentações e documentos também é demorada porque os arquivos são armazenados como binários no MySQL, fazendo o banco atender simultaneamente às operações relacionais e à transferência de ativos grandes.

## Lacunas de escalabilidade

A escalabilidade vertical por aumento de CPU e memória tornou-se insuficiente. A arquitetura monolítica pouco modular e o estado de sessão local dificultam a execução em múltiplas instâncias, o balanceamento de carga e a ampliação apenas da função mais exigida. Relatórios, conteúdo, usuários e cursos concorrem pelos mesmos recursos.

## Lacunas de modularidade

A análise consolidada identificou responsabilidades misturadas e acesso direto entre domínios. Usuários reúne cadastro, autenticação, autorização e vínculos acadêmicos; Cursos acumula turmas, matrículas e conteúdos; Notas concentra avaliações, lançamento, cálculo, publicação, histórico e relatórios.

Essa organização produz baixa coesão, porque um módulo possui diferentes motivos para mudar, e alto acoplamento, porque alterações internas podem atingir funcionalidades não relacionadas. O compartilhamento direto de lógica e dados também reduz encapsulamento e dificulta testes isolados.

## Lacunas funcionais e de integração

O V1 não oferece acesso externo, portal de pais, avaliações on-line, fóruns ou videoconferência. Também não possui integração automática com o GRA. Embora o GRA disponibilize API REST e *webhooks*, professores ainda transcrevem notas manualmente, aumentando esforço e risco de erro.

## Necessidades de modernização

| Lacuna | Consequência | Necessidade para o V2 | Fonte arquitetural |
|---|---|---|---|
| Monólito pouco modular | Mudanças de alto impacto | Estabelecer limites claros de responsabilidade sem reproduzir o acoplamento do legado | [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) |
| Estado no processo | Dificuldade de balanceamento | Reduzir a dependência de estado mantido em uma única execução | [ADR-008 — Redução de Estado Local](/home/02-Arquitetura/ADRs/ADR-008-Reducao-de-Estado-Local) |
| Binários no MySQL | Banco sobrecarregado e conteúdo lento | Separar os dados relacionais da persistência e da entrega de arquivos | [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) |
| PDFs no fluxo principal | Degradação das demais funções | Isolar a geração pesada da navegação e permitir a conclusão posterior | [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios) |
| Ausência de integração GRA | Transcrição manual | Automatizar a integração sem bloquear a confirmação local das operações acadêmicas | [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) |
| Restrição à intranet | Acesso limitado | Permitir acesso web externo com os controles de segurança requeridos | [Requisitos Funcionais](/home/01-Requisitos/Requisitos-Funcionais) e [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais) |

Essas necessidades formam a motivação inicial do projeto. A estratégia de transição está registrada no [ADR-001 — Modernização Incremental](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental), mas seu estado documental não comprova que a modernização tenha sido executada.

## Navegação

- [Requisitos Funcionais](/home/01-Requisitos/Requisitos-Funcionais)
- [Requisitos Não Funcionais](/home/01-Requisitos/Requisitos-Nao-Funcionais)
- [Priorização](/home/01-Requisitos/Priorizacao)
- [Visão Geral da Arquitetura](/home/02-Arquitetura/Visao-Geral)
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades)
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes)
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios)
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais)
