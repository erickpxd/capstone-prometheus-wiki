# 7. Implementação Referencial

## Estado documental

A implementação referencial é um entregável opcional do Capstone. Este repositório contém somente a Wiki e não possui diretório `/src` nem código-fonte do Prometheus V2.

No material disponível, também não foram confirmados:

- URL ou estrutura do repositório principal da aplicação;
- código que evidencie a implementação dos módulos documentados;
- ambientes de execução;
- pipelines de integração, entrega ou implantação;
- infraestrutura implantada.

Assim, a arquitetura registrada na Wiki não deve ser interpretada como implementação existente.

## Estrutura planejada

Uma implementação futura poderá organizar o código pelos limites lógicos Users, Courses, Content, Grades e Videos e pela camada Portal Web. Essa organização é apenas planejada: não comprova diretórios, processos, bancos ou unidades independentes de implantação.

A referência arquitetônica permanece um monólito modular inicialmente implantado como uma unidade. Separar diretórios ou contratos de API não transforma os módulos em microsserviços.

## Evidências necessárias

Esta página só deve registrar cobertura de requisitos ou decisões no código quando houver repositório, caminho e evidência verificáveis. Até lá, implementação, ambientes, pipelines e infraestrutura permanecem não confirmados.

## Referências

- [Visão Geral da Arquitetura](/home/02-Arquitetura/Visao-Geral)
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades)
- [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes)
- [Processamento Assíncrono de Relatórios](/home/02-Arquitetura/Processamento-Assincrono-de-Relatorios)
- [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais)
- [Architecture Decision Records](/home/02-Arquitetura/ADRs)
