- **Decisión:** Implementar Redis como caché de estado actual y Blue-Green Deployment para actualizaciones.

- **Justificación:** Minimizar el impacto en SQL Server y garantizar disponibilidad del 99.9% durante despliegues.

- **Diagrama de Infraestructura (Mermaid):**

```mermaid
graph TD
LB[Load Balancer] --> Web1[Web Server Blue]
LB -.-> Web2[Web Server Green]
Web1 & Web2 --> Redis[(Redis: Last Position)]
Web1 & Web2 --> SQL[(SQL Server: History)]
```
