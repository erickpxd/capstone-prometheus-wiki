# ADR-003 — Decomposição por Domínios

## Status

Aceito na documentação arquitetural.

## Contexto

No Prometheus V1, responsabilidades e estruturas internas compartilhadas produzem acoplamento e tornam as mudanças mais abrangentes. O V2 precisa separar razões de mudança, propriedade de regras e colaborações sem converter automaticamente essas fronteiras em serviços independentes.

## Decisão

Organizar o monólito modular pelos limites lógicos principais **Users**, **Courses**, **Content**, **Grades** e **Videos**. Cada módulo controla suas regras e a propriedade lógica de seus dados, e as colaborações ocorrem por contratos explícitos.

**Identidade**, **Matrículas** e **Comunicação** são responsabilidades internas identificadas que refinam essa organização. Seus limites finais permanecem abertos e sua identificação não cria microsserviços, processos, bancos ou unidades independentes de implantação.

## Alternativas consideradas

- **Manter as responsabilidades misturadas como no legado:** não adotada porque preservaria o acoplamento, a baixa coesão e o impacto amplo das mudanças.

## Consequências

### Benefícios

- aumenta coesão, encapsulamento e testabilidade;
- torna mais explícitas a propriedade de regras e as dependências;
- favorece mudanças localizadas e uma eventual evolução baseada em evidências.

### Custos e riscos

- exige contratos, governança e revisão das dependências legadas;
- fronteiras inadequadas podem apenas deslocar o acoplamento;
- responsabilidades internas ainda abertas exigem refinamento sem criar dependências indevidas.

## Relação com requisitos

- [RF-BASE-01 a RF-BASE-04 e RF-BASE-08](/home/01-Requisitos/Requisitos-Funcionais) fundamentam, respectivamente, os limites Users, Courses, Content, Grades e Videos;
- [RF-BASE-07, RF-BASE-12, RF09 e RF12](/home/01-Requisitos/Requisitos-Funcionais) sustentam as responsabilidades internas de Comunicação e Identidade;
- Matrículas apoia os vínculos acadêmicos exigidos nas operações de Courses e Grades, sem possuir requisito ou implantação independente confirmados.

## Relação com outras decisões

- [DA-02 / ADR-002 — Monólito modular](/home/02-Arquitetura/ADRs/ADR-002-Monolito-Modular) estabelece que esses limites permanecem inicialmente na mesma unidade de implantação;
- [Módulos e Responsabilidades](/home/02-Arquitetura/Modulos-e-Responsabilidades) detalha os limites, contratos e colaborações;
- propriedade lógica de dados não implica persistência fisicamente separada, conforme [Dados e Integrações](/home/02-Arquitetura/Dados-e-Integracoes).

## Pontos ainda abertos

Os limites finais de Identidade, Matrículas, Comunicação, Notificações, Gamificação, Analytics e Portfólio ainda precisam ser refinados. Nenhuma topologia independente decorre automaticamente desse refinamento.

## Evidência de implementação

Não confirmada no material disponível.
