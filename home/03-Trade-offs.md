# 3. Análise de Trade-offs

## Monólito modular versus serviços independentes

Uma arquitetura distribuída permitiria implantação, escala e isolamento de falhas por serviço, mas introduziria comunicação de rede, observabilidade distribuída, coordenação de dados, falhas parciais e múltiplos processos de entrega. O monólito modular aceita menor independência inicial em troca de operação mais simples e migração menos arriscada.

O custo dessa escolha é a possibilidade de replicar toda a aplicação quando apenas uma capacidade estiver sobrecarregada. A resposta é preparar limites claros e extrair somente o componente cuja demanda justifique a complexidade. Grades é o candidato mais explícito devido ao volume de relatórios.

## Segurança versus facilidade de uso

Criptografia, expiração de sessão, controle por papel, auditoria e MFA protegem informações pessoais e acadêmicas, mas adicionam passos e tratamento de erros. O equilíbrio aplica MFA obrigatoriamente ao Portal de Pais e combina controles proporcionais com recuperação automática de senha e mensagens compreensíveis.

## Desempenho versus consistência

Cache pode reduzir consultas e latência, porém corre o risco de devolver informações desatualizadas. Dados pouco mutáveis, como descrições de cursos, são candidatos mais seguros. Notas exigem invalidação ou atualização imediata. Cache permanece uma técnica avaliada, não um produto selecionado.

## Assincronia versus resposta imediata

Fila e eventos protegem a interface de tarefas demoradas e de falhas externas. Em contrapartida, notificações, relatórios e sincronização com o GRA podem terminar depois da operação principal. Essa consistência eventual é aceitável para efeitos secundários, mas não para autenticação, confirmação local da nota ou conclusão de matrícula.

## Banco compartilhado versus persistência independente

A base SQL central simplifica migração e transações iniciais, mas pode provocar contenção e acoplamento por dados. Uma persistência exclusiva para Grades isola consultas pesadas, mas exige sincronização, auditoria, reconciliação e operação adicionais. A referência mantém inicialmente a base central e condiciona a separação a testes e medições.

## CDN versus controle central

Armazenamento dedicado e CDN reduzem a carga do banco e aproximam arquivos dos usuários. A distribuição externa, entretanto, exige manter autorização, expiração, metadados e objetos consistentes. O ganho é necessário para cumprir a meta de latência, mas nenhum fornecedor foi definido.

## Dependência externa versus especialização

GRA e plataforma de videoconferência oferecem capacidades que não devem ser reproduzidas integralmente. Essa especialização reduz esforço, mas expõe o LMS a latência, indisponibilidade e mudança de contrato. Adaptadores, filas, novas tentativas e degradação controlada limitam o impacto.

## Síntese

| Escolha | Benefício | Custo aceito |
|---|---|---|
| Monólito modular | Simplicidade e modernização gradual | Menor independência inicial de implantação |
| CDN e armazenamento dedicado | Menor latência e carga no banco | Controle distribuído dos ativos |
| Processamento assíncrono | Responsividade e isolamento | Efeitos secundários concluídos posteriormente |
| Banco central inicial | Migração e transações mais simples | Possível contenção entre domínios |
| Plataformas externas | Uso de capacidades especializadas | Dependência de contratos e disponibilidade |
