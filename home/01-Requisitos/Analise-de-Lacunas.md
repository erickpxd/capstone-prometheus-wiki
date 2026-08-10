# Análise de Lacunas

## Contexto do Prometheus V1

O Prometheus V1 é o LMS utilizado pela School X há aproximadamente quinze anos. Administradores gerenciam usuários e cursos, professores publicam conteúdos e registram notas, e estudantes consultam materiais e resultados acadêmicos. O tempo de operação produziu um acervo histórico relevante, razão pela qual a substituição imediata do sistema não é considerada viável.

O sistema foi construído como uma aplicação web monolítica restrita à rede interna. A interface utiliza ASP clássico, a lógica de negócio está reunida em uma biblioteca/DLL C# e um banco MySQL centralizado armazena dados estruturados e arquivos binários. A aplicação também mantém estado de sessão no servidor.

## Lacunas de desempenho

A geração de relatórios de notas em PDF é lenta, principalmente quando envolve grande número de estudantes. O processamento ocorre no mesmo ambiente que atende às demais funções e, portanto, pode degradar a navegação. A recuperação de apresentações e documentos também é demorada porque os arquivos são armazenados como binários no MySQL, fazendo o banco atender simultaneamente às operações relacionais e à transferência de ativos grandes.

## Lacunas de escalabilidade

A escalabilidade vertical por aumento de CPU e memória tornou-se insuficiente. A arquitetura monolítica pouco modular e o estado de sessão local dificultam a execução em múltiplas instâncias, o balanceamento de carga e a ampliação apenas da função mais exigida. Relatórios, conteúdo, usuários e cursos concorrem pelos mesmos recursos.

## Lacunas de modularidade

As atividades mais recentes identificaram responsabilidades misturadas e acesso direto entre domínios. Usuários reúne cadastro, autenticação, autorização e vínculos acadêmicos; Cursos acumula turmas, matrículas e conteúdos; Notas concentra avaliações, lançamento, cálculo, publicação, histórico e relatórios.

Essa organização produz baixa coesão, porque um módulo possui diferentes motivos para mudar, e alto acoplamento, porque alterações internas podem atingir funcionalidades não relacionadas. O compartilhamento direto de lógica e dados também reduz encapsulamento e dificulta testes isolados.

## Lacunas funcionais e de integração

O V1 não oferece acesso externo, portal de pais, avaliações on-line, fóruns ou videoconferência. Também não possui integração automática com o GRA. Embora o GRA disponibilize API REST e *webhooks*, professores ainda transcrevem notas manualmente, aumentando esforço e risco de erro.

## Direções propostas

| Lacuna | Consequência | Direção para o V2 |
|---|---|---|
| Monólito pouco modular | Mudanças de alto impacto | Monólito modular com limites por domínio |
| Estado no processo | Dificuldade de balanceamento | Redução do estado local |
| Binários no MySQL | Banco sobrecarregado e conteúdo lento | Armazenamento dedicado e CDN |
| PDFs no fluxo principal | Degradação das demais funções | Fila e processamento em segundo plano |
| Ausência de integração GRA | Transcrição manual | REST, *webhooks* e sincronização assíncrona |
| Restrição à intranet | Acesso limitado | Portais web externos com controles de segurança |

Essas direções formam a proposta inicial do projeto. Sua implementação deve ser incremental para preservar dados e continuidade de serviço.
