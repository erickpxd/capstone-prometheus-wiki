# Apresentação Final

## Estado documental

Apesar do título acadêmico, esta página preserva o recorte da apresentação consolidada até a Semana 5. Ela não representa necessariamente o estado documental posterior, não comprova implementação e não substitui as páginas canônicas. Hipóteses de tecnologia, redundância e implantação citadas abaixo devem ser lidas no contexto daquele recorte.

## Problema

O Prometheus V1 atende à School X há quinze anos, mas sua estrutura monolítica pouco modular, estado local, banco central com binários e ausência de integrações limitam desempenho, manutenção e crescimento. Os dados históricos impedem uma substituição imediata.

## Decisão principal

O Prometheus V2 adota um monólito modular. A solução mantém operação e implantação mais simples que uma arquitetura distribuída, separa responsabilidades por domínio e prepara a extração seletiva de capacidades críticas quando medições justificarem.

## Arquitetura consolidada

Users, Courses, Content, Grades e Videos formam os limites principais. Portais atendem estudantes, professores, administradores e responsáveis. Conteúdos passam para armazenamento dedicado/CDN; Grades usa processamento em segundo plano para relatórios; Grades Bridge automatiza o GRA; uma plataforma externa oferece videoconferência.

As atividades da Semana 4 aprofundaram coesão, acoplamento e encapsulamento. Identidade, matrículas, avaliações, notas e comunicação foram reconhecidos como responsabilidades internas mais específicas, sem serem transformados automaticamente em serviços independentes.

Na Semana 5, o diagnóstico de desempenho evitou atribuir os gargalos a uma causa não comprovada: a evidência disponível demonstra contenção por recursos compartilhados. A resposta então estudada para relatórios combinava cache seletivo, fila durável, múltiplos Report Workers e armazenamento do resultado. Cache, garantias concretas da fila e quantidade ou topologia de workers eram hipóteses daquele recorte, não decisões vigentes nem evidência de implementação. Para conteúdos, Object Storage mantém a origem dos arquivos e a CDN cuida da distribuição.

## Qualidade e trade-offs

Segurança, manutenibilidade, desempenho e escalabilidade orientam a solução. MFA protege o Portal de Pais; CDN atende à latência de conteúdo; fila protege a navegação da carga de relatórios e do GRA; e a meta de disponibilidade é 99,9%.

O monólito modular aceita menor independência inicial para reduzir complexidade. O banco central simplifica a migração, mas exige propriedade lógica dos dados. A persistência própria de Grades e eventual extração do módulo continuam decisões futuras condicionadas a evidência.

## Disponibilidade e operação

A proposta conceitual de alta disponibilidade apresentada naquele recorte priorizava Grades e Videos. Modo ativo/ativo, Load Balancer, health checks, workers redundantes, fila durável e réplica *standby* eram hipóteses para avaliação, não uma topologia selecionada ou implantada. Persistência própria de Grades, failover, RPO e RTO também permaneciam abertos.

Falhas externas usam degradação elegante: indisponibilidade do vídeo não interrompe o restante do LMS, e indisponibilidade do GRA não bloqueia a nota já confirmada localmente. Métricas, alertas e verificações externas devem comprovar a meta de 99,9%. Prometheus Monitoring, Grafana e Alertmanager são alternativas estudadas, não ferramentas já adotadas.

## Desafios remanescentes

Ainda precisam ser definidos o SGBD do V2, produtos de mensageria, armazenamento e observabilidade, mecanismo de autenticação, fornecedor de vídeo, solução de balanceamento, regras de autoridade entre GRA e Prometheus, RPO/RTO e estratégia detalhada de migração. Essas lacunas foram mantidas abertas para não inventar decisões ausentes do material.

## Encerramento

A proposta cria uma sequência coerente entre legado, problemas, requisitos, arquitetura e plano de evolução. O V2 não descarta o passado nem distribui prematuramente o sistema: estabelece limites sustentáveis para modernizar o Prometheus de maneira gradual.

## Precedência documental

Para o estado vigente na documentação, consulte a [Visão Geral da Arquitetura](/home/02-Arquitetura/Visao-Geral), as [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais) e os [Architecture Decision Records](/home/02-Arquitetura/ADRs). O fluxo canônico de PDFs está em [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios); produtos, mecanismos de resiliência, topologias e fornecedores ainda não escolhidos permanecem em [Tecnologias e Pontos Abertos](/home/02-Arquitetura/Tecnologias-e-Pontos-Abertos).
