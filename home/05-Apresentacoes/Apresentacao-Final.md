# Apresentação Final

## Problema

O Prometheus V1 atende à School X há quinze anos, mas sua estrutura monolítica pouco modular, estado local, banco central com binários e ausência de integrações limitam desempenho, manutenção e crescimento. Os dados históricos impedem uma substituição imediata.

## Decisão principal

O Prometheus V2 adota um monólito modular. A solução mantém operação e implantação mais simples que uma arquitetura distribuída, separa responsabilidades por domínio e prepara a extração seletiva de capacidades críticas quando medições justificarem.

## Arquitetura consolidada

Users, Courses, Content, Grades e Videos formam os limites principais. Portais atendem estudantes, professores, administradores e responsáveis. Conteúdos passam para armazenamento dedicado/CDN; Grades usa processamento em segundo plano para relatórios; Grades Bridge automatiza o GRA; uma plataforma externa oferece videoconferência.

As atividades finais aprofundaram coesão, acoplamento e encapsulamento. Identidade, matrículas, avaliações, notas e comunicação foram reconhecidos como responsabilidades internas mais específicas, sem serem transformados automaticamente em serviços independentes.

## Qualidade e trade-offs

Segurança, manutenibilidade, desempenho e escalabilidade orientam a solução. MFA protege o Portal de Pais; CDN atende à latência de conteúdo; fila protege a navegação da carga de relatórios e do GRA; e a meta de disponibilidade é 99,9%.

O monólito modular aceita menor independência inicial para reduzir complexidade. O banco central simplifica a migração, mas exige propriedade lógica dos dados. A persistência própria de Grades e eventual extração do módulo continuam decisões futuras condicionadas a evidência.

## Desafios remanescentes

Ainda precisam ser definidos o SGBD do V2, produtos de mensageria e armazenamento, mecanismo de autenticação, fornecedor de vídeo, regras de autoridade entre GRA e Prometheus e estratégia detalhada de migração. Essas lacunas foram mantidas abertas para não inventar decisões ausentes do material.

## Encerramento

A proposta cria uma sequência coerente entre legado, problemas, requisitos, arquitetura e plano de evolução. O V2 não descarta o passado nem distribui prematuramente o sistema: estabelece limites sustentáveis para modernizar o Prometheus de maneira gradual.
