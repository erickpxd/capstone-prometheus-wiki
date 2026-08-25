# Dados e Integrações

> **Estado documental:** esta página descreve decisões e abstrações da arquitetura proposta do Prometheus V2. Ela não comprova implantação, contratação de fornecedor ou seleção de produto.

## Classificação usada

| Classificação | Significado |
|---|---|
| **Decisão vigente** | Direção aceita pela documentação arquitetural, ainda que a implementação não esteja comprovada |
| **Sistema externo** | Sistema fora do limite do Prometheus e com o qual o V2 precisa interagir |
| **Abstração arquitetural** | Capacidade exigida pela solução sem vinculação a produto ou fornecedor |
| **Tecnologia ainda não selecionada** | Escolha de implementação que permanece aberta |
| **Evolução condicionada** | Alternativa futura dependente de métricas, validação e nova decisão |

## Visão consolidada

| Elemento | Classificação | Situação vigente |
|---|---|---|
| Banco SQL principal | Decisão vigente e abstração arquitetural | Base relacional inicialmente compartilhada para os dados do V2, incluindo notas no desenho de referência; o SGBD ainda não foi selecionado |
| Propriedade lógica dos dados | Decisão vigente | Cada módulo controla suas regras e seus dados mesmo quando a persistência física é compartilhada |
| Object Storage | Decisão vigente e abstração arquitetural | Armazena binários de conteúdo e pode armazenar gravações, portfólios e PDFs concluídos; fornecedor ainda não selecionado |
| CDN | Decisão vigente e abstração arquitetural | Distribui conteúdos apropriados e apoia o RNF03; fornecedor ainda não selecionado |
| Fila ou Message Broker | Abstração arquitetural necessária | Desacopla relatórios e sincronização com o GRA; produto ainda não selecionado |
| GRA | Sistema externo confirmado | Sistema oficial externo de notas, com REST e *webhooks* disponíveis; regras completas de autoridade e conflito permanecem abertas |
| Grades Bridge | Decisão vigente | Adaptador lógico que isola Grades dos contratos do GRA e coordena sincronização, auditoria e reconciliação |
| Provedor de videoconferência | Sistema externo previsto | Transmite áudio e vídeo; o fornecedor e o contrato ainda não foram selecionados |
| E-mail, SMS e *push* | Integrações externas previstas | Canais necessários para recuperação de senha e notificações; fornecedores e contratos permanecem abertos |

## Banco SQL principal e propriedade lógica

O Prometheus V1 utiliza um banco MySQL centralizado tanto para dados relacionais quanto para arquivos binários. No V2, a arquitetura de referência mantém uma **base SQL principal inicialmente compartilhada** para usuários, cursos, fóruns, sessões e, inicialmente, notas. Essa é uma decisão documental; não há evidência de implantação e o SGBD do V2 é uma tecnologia ainda não selecionada.

Compartilhar a persistência física não autoriza acesso irrestrito. Users, Courses, Content, Grades e Videos devem controlar logicamente seus dados e publicar operações pelos contratos definidos. Um módulo consumidor não deve tratar tabelas ou estruturas internas de outro módulo como interface pública.

O compartilhamento físico do banco também não constitui, por si só, um *Shared Kernel* de DDD. A documentação não registra a decisão de manter um único modelo de domínio deliberadamente compartilhado; registra propriedade lógica e contratos entre limites.

Propriedade lógica também não significa banco próprio. A persistência exclusiva de Grades, o uso de NoSQL, réplicas de leitura e topologias *primary/standby* são **evoluções condicionadas**, não o estado vigente.

## Arquivos, Object Storage e CDN

Retirar os binários do banco principal é uma decisão vigente da arquitetura proposta. Content conserva metadados, referências, autorização e associação com o curso; o Object Storage conserva o arquivo. O mesmo tipo de armazenamento pode atender gravações, portfólios e relatórios concluídos, respeitando políticas e permissões próprias.

A CDN distribui conteúdos apropriados a partir da origem persistente e participa da resposta ao [RNF03](/home/01-Requisitos/Requisitos-Nao-Funcionais). Object Storage e CDN são abstrações arquiteturais: fornecedor, produto, regiões, expiração e detalhes de acesso ainda não foram selecionados.

Retirar um arquivo do banco não o torna público. Autenticação, autorização, vínculo acadêmico, retenção, exclusão e consistência entre metadado e objeto continuam sob controle do Prometheus. Uma CDN privada para relatórios é apenas uma proposta condicionada a volume, latência e segurança; ela não integra o fluxo canônico atual de PDFs.

## GRA e Grades Bridge

O GRA é um sistema externo usado para a gestão oficial de notas e disponibiliza REST e *webhooks*. No V1, a transcrição é manual. A proposta do V2 registra a integração automatizada por meio do **Grades Bridge**.

Grades Bridge pertence logicamente à arquitetura do Prometheus e atua como adaptador entre Grades e o contrato externo. A sincronização de escrita é assíncrona para que lentidão ou indisponibilidade temporária do GRA não bloqueie a nota já validada e registrada localmente. O fluxo deve considerar idempotência, novas tentativas controladas, auditoria e reconciliação.

Em termos de DDD, essa função corresponde ao papel de uma *Anti-Corruption Layer* (ACL): traduz o modelo e os contratos externos para impedir que conceitos próprios do GRA contaminem o modelo de Grades. O nome ACL descreve a responsabilidade da fronteira; não cria um contexto delimitado, processo ou serviço independente.

O Grades Bridge não é documentado como microsserviço nem unidade de implantação independente. Produto de mensageria, autenticação do GRA, regras de conflito, fonte de autoridade durante a convivência e política detalhada de reconciliação continuam abertos.

## Videoconferência

O Prometheus administra o contexto acadêmico das videoconferências: agendamento, autorização, links, associação a cursos e tratamento das gravações. A transmissão de áudio e vídeo é delegada a um **provedor externo de videoconferência**.

A delegação é uma decisão vigente; o fornecedor é tecnologia ainda não selecionada. O contrato, o SLA, os limites de timeout e as políticas de resiliência precisam ser definidos e medidos. Uma falha do provedor deve limitar as operações dependentes de vídeo, sem indisponibilizar cursos, conteúdos e notas.

## Outras integrações externas

Notificações *push*, recuperação de senha por e-mail ou SMS e outros canais de entrega exigem adaptadores externos. Os requisitos registram essas capacidades, mas não determinam fornecedores, contratos ou limites finais de responsabilidade de Comunicação e Notificações.

O mecanismo de identidade também permanece aberto. Autenticação, autorização e MFA do Portal de Pais são requisitos; Identity Provider, protocolos, algoritmos e gestão de chaves são tecnologias ainda não selecionadas.

## Evoluções condicionadas

As alternativas abaixo foram estudadas, mas não fazem parte do estado vigente:

- persistência própria ou NoSQL para Grades;
- Primary + Read Replicas para ampliar leituras;
- *primary/standby* para failover;
- implantação independente ou escala isolada de Grades;
- CDN privada para relatórios;
- cache distribuído para sessão, consultas ou referências de relatório;
- topologia ativa/ativa de módulos específicos.

Qualquer adoção depende de métricas, requisitos de consistência, RPO/RTO, migração, segurança, observabilidade e uma decisão posterior. O inventário de produtos e escolhas abertas está em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos).
