- **Decisión:** Usar WebSockets con una capa intermedia de Redis Pub/Sub.

- **Justificación:** Desacopla la visualización de la persistencia. Evita consultas constantes (polling) a la base de datos relacional.

- **Consecuencia:** Necesitamos un servidor de Redis en la infraestructura, pero ganamos una escalabilidad enorme.
