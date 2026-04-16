# Caso de Estudio: Sistema AVL de Alta Disponibilidad

## 1. Contexto del Problema

El sistema debe gestionar la ubicación de múltiples unidades de transporte en tiempo real. A medida que la flota crece, el servicio de recepción de datos (Gateway) se convierte en un cuello de botella.

## 2. Atributos de Calidad Prioritarios

Como arquitecto, debo elegir qué es lo más importante. Ordena estos 3 del 1 al 3 (siendo 1 el más importante):

- **Disponibilidad:** El sistema nunca debe dejar de recibir datos, aunque tarde un poco en procesarlos.
- **Latencia:** El dato debe verse en el mapa en menos de 2 segundos desde que se generó.
- **Escalabilidad:** El sistema debe soportar pasar de 100 a 5,000 unidades sin cambios en el código.

## 3. Restricciones Técnicas

- Protocolo de comunicación (¿UDP, TCP, HTTP?).
- Base de datos actual (¿Soportará 1,000 inserts por segundo?).

## 4. Estrategia de Persistencia

- **Decisión Táctica:** Implementación de persistencia por lotes (Batch Insert).
- **Atributo Priorizado:** Rendimiento de la Base de Datos (Throughput).
- **Lógica:** El Worker acumulará mensajes del Broker y ejecutará un INSERT masivo cada 200 registros o cada 2 segundos. Esto reduce la carga de IOPS en SQL Server y evita la fragmentación excesiva de índices.

### Gestión de Fallos mediante Dead Letter Queues (DLQ)

- **Contexto:** Errores de red en SQL Server o datos malformados pueden bloquear el procesamiento de la flota.

- **Decisión:** Implementar una política de reintentos (ej. 3 intentos). Si el mensaje sigue fallando, se mueve automáticamente a una flota-positions-dlq.

- **Beneficio**: Evita el bloqueo del Worker y permite auditoría posterior de los datos fallidos.
