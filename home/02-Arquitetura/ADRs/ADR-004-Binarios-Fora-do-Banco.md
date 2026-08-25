# ADR-004 — Binários Fora do Banco

## Status

Aceito na documentação arquitetural.

Esse status registra uma decisão para o Prometheus V2 e não comprova sua implementação.

## Contexto

O Prometheus V1 mantém dados relacionais e arquivos binários no banco MySQL centralizado. Essa combinação amplia a carga e o volume da persistência transacional e dificulta a distribuição eficiente de materiais acadêmicos. A modernização também precisa preservar metadados, vínculos, autorização e os dados históricos existentes.

## Decisão

Os binários deixam o banco transacional e passam a ser armazenados por uma abstração de **Object Storage**. O Prometheus conserva no banco SQL principal os metadados, as referências aos objetos, as associações acadêmicas e as informações necessárias à autorização.

Uma **CDN** distribui os conteúdos apropriados, mantendo o Object Storage como origem persistente. O uso de CDN não torna os arquivos públicos nem se aplica automaticamente a todo tipo de documento. Produtos, fornecedores, regiões e mecanismos de acesso ainda não foram selecionados.

## Alternativas consideradas

- **Manter os BLOBs no banco transacional:** corresponde ao desenho do V1, mas preservaria no mesmo recurso as cargas relacionais e a transferência de arquivos; não foi escolhida para o V2.
- **Separar binários em Object Storage e distribuir conteúdos apropriados por CDN:** alternativa escolhida, com metadados e autorização sob responsabilidade do Prometheus.

## Consequências

### Benefícios

- reduz a carga de armazenamento e entrega de binários sobre o banco SQL principal;
- permite que persistência de objetos e distribuição de conteúdo evoluam conforme suas próprias características;
- favorece a entrega externa de materiais associada ao RNF03;
- mantém regras acadêmicas, metadados e autorização sob controle do Prometheus.

### Custos e riscos

- exige consistência entre o metadado persistido e o objeto armazenado;
- introduz controles específicos de autorização, criptografia, retenção e exclusão de arquivos;
- cria dependências operacionais de armazenamento e, para conteúdos distribuídos, de CDN;
- requer migração e validação dos BLOBs existentes sem perda de dados ou referências.

## Relação com requisitos

- [RF-BASE-03, RF-BASE-05, RF04 e RF08](/home/01-Requisitos/Requisitos-Funcionais) relacionam a decisão à entrega de conteúdos, preservação do acervo, gravações e portfólios.
- [RNF02 e RNF03](/home/01-Requisitos/Requisitos-Nao-Funcionais) exigem proteção dos dados e desempenho na entrega externa de conteúdo.

## Relação com outras decisões

- detalha a **DA-04** do índice de [Decisões Arquiteturais](/home/02-Arquitetura/Decisoes-Arquiteturais);
- complementa a modernização gradual registrada no [ADR-001](/home/02-Arquitetura/ADRs/ADR-001-Modernizacao-Incremental) e a propriedade lógica definida no [ADR-003](/home/02-Arquitetura/ADRs/ADR-003-Decomposicao-por-Dominios);
- fornece o armazenamento de resultado usado pelo fluxo de relatórios do [ADR-005](/home/02-Arquitetura/ADRs/ADR-005-Processamento-Assincrono-de-Relatorios).

## Pontos ainda abertos

- fornecedor, produto, regiões e configuração do Object Storage e da CDN;
- estratégia de migração, validação e eventual retorno para os BLOBs existentes;
- política de consistência entre metadado e objeto;
- critérios de elegibilidade de conteúdos para CDN e mecanismos de acesso autorizado;
- retenção, exclusão, criptografia, backup e recuperação dos objetos.

## Evidência de implementação

Não confirmada no material disponível.
