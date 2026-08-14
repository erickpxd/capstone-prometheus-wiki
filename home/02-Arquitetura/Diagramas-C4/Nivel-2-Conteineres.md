# Nível 2 — Contêineres

## Escopo do diagrama futuro

O diagrama de contêineres deverá representar os portais Principal, Estudante, Professor, Administrador e Pais; os limites Users, Courses, Content, Grades e Videos; a base SQL principal; o armazenamento de ativos/CDN; a integração GRA; a plataforma de vídeo; e o processamento assíncrono associado a Grades.

As APIs precisam permanecer visualmente dentro do limite do monólito modular, salvo quando um componente for explicitamente marcado como evolução possível. A persistência exclusiva de Grades também deve ser identificada como alternativa futura, não como estado confirmado.

## Estado

![Prometheus_V2_C4_Nivel_2](uploads/00b42cb148199a436202def1683af916/Prometheus_V2_C4_Nivel_2.png){width=900 height=454}
