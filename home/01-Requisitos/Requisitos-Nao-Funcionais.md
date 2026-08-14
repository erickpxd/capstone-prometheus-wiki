# Requisitos Não Funcionais

## Introdução

Os requisitos não funcionais estabelecem condições mensuráveis para a operação do Prometheus V2. Eles direcionam decisões de segurança, distribuição de conteúdo, processamento de Grades, integração com o GRA e evolução da implantação, sem determinar antecipadamente produtos ou fornecedores.

## RNF01 — Disponibilidade e resiliência

O Prometheus V2 deve atingir disponibilidade mínima de **99,9% de tempo de atividade**. A arquitetura deve suportar períodos críticos, como finais de semestre, e permitir ampliação horizontal das capacidades preparadas para múltiplas instâncias.

A meta é global e não pode ser atribuída isoladamente a um Load Balancer, réplica ou cache. Ela depende da eliminação de pontos únicos de falha, da detecção e recuperação de falhas, do monitoramento e da degradação controlada de dependências externas. No monólito modular, escala independente exige especialização prévia do componente; Grades é o principal candidato documentado.

Em uma janela ilustrativa de 30 dias, 99,9% correspondem a aproximadamente **43,2 minutos de indisponibilidade permitida**. Esse valor ajuda a comunicar o orçamento de indisponibilidade, mas a aceitação ainda depende da definição formal da janela de medição, das funções incluídas e das exclusões acordadas.

**Atributos relacionados:** disponibilidade, resiliência e escalabilidade.

**Critério principal:** disponibilidade mínima de 99,9% no período e nas funções definidos para a medição do LMS.

## RNF02 — Segurança e proteção de dados

Dados pessoais e acadêmicos de estudantes e pais ou responsáveis devem permanecer criptografados em trânsito e em repouso. O acesso ao Portal de Pais deve exigir autenticação multifator (MFA), além da autorização que confirma o vínculo com o estudante consultado.

Os controles devem abranger os dados mantidos no banco, arquivos em armazenamento dedicado, relatórios e comunicações entre componentes. O requisito não seleciona protocolo, produto de identidade ou algoritmo criptográfico; essas escolhas dependem de decisão de design e avaliação de segurança posteriores.

**Atributos relacionados:** segurança, privacidade e confidencialidade.

**Critérios principais:** criptografia em trânsito; criptografia em repouso; MFA obrigatório no Portal de Pais.

## RNF03 — Desempenho de conteúdo

Apresentações, PDFs, documentos e demais materiais acadêmicos distribuídos por CDN devem carregar em **menos de dois segundos** para usuários fora da rede interna da School X.

O requisito justifica retirar os binários do banco principal, usar armazenamento dedicado como origem e distribuir os arquivos por CDN. A validação deverá definir tamanho representativo do arquivo, localização do usuário, condições de rede e estado do cache, pois medir apenas a resposta da Content API não comprova a entrega do conteúdo.

**Atributos relacionados:** desempenho, latência e experiência do usuário.

**Critério principal:** carregamento de conteúdo em menos de dois segundos no cenário externo de teste definido.

## RNF04 — Desacoplamento da escrita de notas

A sincronização de notas entre o Prometheus e o GRA deve ocorrer de forma assíncrona por fila ou *message broker*. Lentidão ou indisponibilidade temporária do GRA não deve impedir que o professor conclua a operação principal no Prometheus.

A nota deve ser validada e registrada no domínio Grades antes de depender do sistema externo. O Grades Bridge processa a integração posteriormente, preservando estado, auditoria, novas tentativas controladas e reconciliação. Mensagens repetidas não podem produzir duplicação de notas.

**Atributos relacionados:** resiliência, desacoplamento, disponibilidade, interoperabilidade e desempenho.

**Critério principal:** a indisponibilidade ou lentidão do GRA não bloqueia o registro local confirmado ao professor.

## RNF05 — Escalabilidade de relatórios

A Grades API deve suportar a geração simultânea de até **500 relatórios PDF** sem degradar significativamente o tempo de resposta das demais funções de navegação.

O processamento deve ser retirado da requisição síncrona, encaminhado por fila e executado por *workers*. Vários *workers* permitem processamento paralelo; a inclusão de novas instâncias amplia horizontalmente a capacidade. Fila ou paralelismo isolados não comprovam o requisito: sua aceitação depende de teste de carga que meça relatórios concluídos, tempo de fila, taxa de erro e latência da navegação concorrente.

**Atributos relacionados:** escalabilidade, desempenho, concorrência e isolamento de carga.

**Critério principal:** até 500 PDFs simultâneos sem degradação significativa das funções de navegação, conforme limites definidos no cenário de teste.

## Restrições complementares

A solução deve preservar os dados existentes, reduzir a dependência de estado mantido em uma única instância, manter alta coesão e baixo acoplamento e permitir modernização incremental. APIs REST representam contratos e não transformam automaticamente o Prometheus V2 em uma arquitetura de microsserviços pura.

## Matriz resumida

| ID | Requisito | Atributo principal | Métrica ou restrição |
|---|---|---|---|
| RNF01 | Disponibilidade e resiliência | Disponibilidade | 99,9% de tempo de atividade |
| RNF02 | Segurança e proteção de dados | Segurança | Criptografia em trânsito e repouso; MFA para pais |
| RNF03 | Desempenho de conteúdo | Desempenho | Carregamento abaixo de dois segundos |
| RNF04 | Desacoplamento da escrita de notas | Resiliência e integração | Sincronização assíncrona sem bloquear o professor |
| RNF05 | Escalabilidade de relatórios | Escalabilidade | Até 500 PDFs simultâneos sem degradar a navegação |
