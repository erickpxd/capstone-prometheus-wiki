# Requisitos Não Funcionais

## Introdução

Os requisitos não funcionais estabelecem condições mensuráveis ou verificáveis para a operação do Prometheus V2. Esta página é a fonte normativa de RNF01–RNF05: as referências arquiteturais explicam como a proposta responde a cada requisito, sem substituir seus critérios nem comprovar implementação.

## RNF01 — Disponibilidade e resiliência

O Prometheus V2 deve atingir disponibilidade mínima de **99,9% de tempo de atividade**. A arquitetura deve suportar períodos críticos, como finais de semestre, e permitir ampliação horizontal das capacidades preparadas para múltiplas instâncias.

Em uma janela ilustrativa de 30 dias, 99,9% correspondem a aproximadamente **43,2 minutos de indisponibilidade permitida**. Essa conversão é somente uma ilustração matemática da meta e não constitui requisito adicional. A aceitação ainda depende da definição formal da janela de medição, das funções incluídas e das exclusões acordadas.

**Atributos relacionados:** disponibilidade, resiliência e escalabilidade.

**Critério principal:** disponibilidade mínima de 99,9% no período e nas funções definidos para a medição do LMS.

**Referências arquiteturais:** [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade), [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) e [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos).

## RNF02 — Segurança e proteção de dados

Dados pessoais e acadêmicos de estudantes e pais ou responsáveis devem permanecer criptografados em trânsito e em repouso. O acesso ao Portal de Pais deve exigir autenticação multifator (MFA), além da autorização que confirma o vínculo com o estudante consultado.

Essas obrigações abrangem dados mantidos no banco, arquivos em armazenamento dedicado, relatórios e comunicações entre componentes. O requisito não seleciona protocolo, produto de identidade, mecanismo de sessão ou algoritmo criptográfico.

**Atributos relacionados:** segurança, privacidade e confidencialidade.

**Critérios principais:** criptografia em trânsito; criptografia em repouso; MFA obrigatório no Portal de Pais.

**Referências arquiteturais:** [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade), [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) e [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).

## RNF03 — Desempenho de conteúdo

Apresentações, PDFs, documentos e demais materiais acadêmicos devem carregar em **menos de dois segundos** para usuários fora da rede interna da School X.

**Atributos relacionados:** desempenho, latência e experiência do usuário.

**Critério principal:** carregamento de conteúdo em menos de dois segundos no cenário externo de teste definido.

**Referências arquiteturais:** [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade), [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes) e [ADR-004 — Binários Fora do Banco](/home/02-Arquitetura/ADRs/ADR-004-Binarios-Fora-do-Banco).

## RNF04 — Desacoplamento da escrita de notas

A sincronização de notas entre o Prometheus e o GRA deve ocorrer de forma assíncrona. Lentidão ou indisponibilidade temporária do GRA não deve impedir que o professor conclua a operação principal no Prometheus.

A nota deve ser validada e registrada localmente antes de depender do sistema externo. A sincronização posterior deve preservar estado, idempotência, auditoria, novas tentativas controladas e reconciliação. Mensagens repetidas não podem produzir duplicação de notas.

**Atributos relacionados:** resiliência, desacoplamento, disponibilidade, interoperabilidade e desempenho.

**Critério principal:** a indisponibilidade ou lentidão do GRA não bloqueia o registro local confirmado ao professor.

**Referências arquiteturais:** [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade), [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes), [ADR-006 — Integração GRA por Grades Bridge](/home/02-Arquitetura/ADRs/ADR-006-Integracao-GRA-por-Grades-Bridge) e [ADR-009 — Assincronia Seletiva](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva).

## RNF05 — Escalabilidade de relatórios

O Prometheus V2 deve suportar a geração simultânea de até **500 relatórios PDF** sem degradar significativamente o tempo de resposta das demais funções de navegação.

**Atributos relacionados:** escalabilidade, desempenho, concorrência e isolamento de carga.

**Critério principal:** até 500 PDFs simultâneos sem degradação significativa das funções de navegação, conforme limites definidos no cenário de teste.

**Referências arquiteturais:** [Atributos de Qualidade](/home/02-Arquitetura/Atributos-de-Qualidade), [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios), [ADR-005 — Processamento Assíncrono de Relatórios](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios) e [ADR-009 — Assincronia Seletiva](/home/02-Arquitetura/ADRs/ADR-009-Assincronia-Seletiva).

## Restrições complementares

A solução deve preservar os dados existentes, reduzir a dependência de estado mantido em uma única instância, manter alta coesão e baixo acoplamento e permitir modernização incremental. APIs REST representam contratos e não transformam automaticamente o Prometheus V2 em uma arquitetura de microsserviços pura. Os limites e as decisões relacionados estão em [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) e [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais).

## Lacunas de verificação

Os itens abaixo são definições ainda necessárias aos protocolos de verificação. Eles não criam novos requisitos nem autorizam o preenchimento de valores por inferência.

- **RNF01:** janela de medição, funções críticas incluídas, SLI, fonte de medição, exclusões e cálculo formal;
- **RNF02:** protocolos e algoritmos, gestão de chaves, provedor de identidade e mecanismo de sessão, fluxo de MFA, matriz de autorização, modelo de ameaças e evidência de teste;
- **RNF03:** tamanho e tipo do arquivo, localização do usuário, condição de rede, estado do cache e percentil;
- **RNF04:** contrato e autenticação do GRA, fonte de autoridade, regras de conflito, limites de novas tentativas e política detalhada de reconciliação;
- **RNF05:** duração do teste, mix e tamanho dos relatórios, limite quantitativo de degradação ou latência da navegação, taxa de erro, ambiente e carga concorrente.
