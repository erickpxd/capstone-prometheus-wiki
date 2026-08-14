# Nível 2 — Contêineres

## Leitura consolidada

O diagrama-base representa os portais Principal, Estudante, Professor, Administrador e Pais; os limites Users, Courses, Content, Grades e Videos; a base SQL principal; o armazenamento de ativos/CDN; a integração GRA; a plataforma de vídeo; e o processamento assíncrono associado a Grades.

As APIs precisam permanecer visualmente dentro do limite do monólito modular, salvo quando um componente for explicitamente marcado como evolução possível. A persistência exclusiva de Grades também deve ser identificada como alternativa futura, não como estado confirmado.

A análise de disponibilidade da Semana 5 acrescenta uma visão conceitual com Load Balancer, instâncias redundantes de Grades e Videos, fila durável, Report Workers, armazenamento de relatórios e monitoramento. Uma eventual base Grades *primary/standby* deve continuar identificada como cenário de evolução, com RPO e RTO ainda abertos. Esses elementos não exigem a criação de um novo diagrama nesta Wiki nem alteram a classificação atual de monólito modular.

## Estado

![Prometheus_V2_C4_Nivel_2](uploads/00b42cb148199a436202def1683af916/Prometheus_V2_C4_Nivel_2.png){width=900 height=454}
