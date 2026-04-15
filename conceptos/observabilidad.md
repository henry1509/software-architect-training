# Documento de Preparación: Observabilidad en Sistemas Distribuidos

En una arquitectura monolítica (donde todo está en un solo lugar), si algo falla, miras el log del servidor y listo. En la arquitectura que diseñamos hoy para el AVL (Gateway -> Cola -> Worker -> DB), el dato viaja por diferentes "planetas". Si un dato se pierde, no sabes si fue la red, la cola, el worker o la base de datos. Aquí es donde entra la Observabilidad.

## 1. Los Tres Pilares de la Observabilidad

Para dominar este nivel, debes entender la diferencia entre estos tres conceptos:

- **Logs (Registros):** Son eventos discretos. "El Worker procesó el mensaje X a las 10:01 AM". Son buenos para saber qué pasó en un punto exacto, pero son costosos de almacenar en grandes volúmenes.

- **Metrics (Métricas):** Son datos agregados y numéricos. "El servidor de GPS tiene un 80% de uso de CPU" o "Hay 5,000 mensajes esperando en la cola". Sirven para alertas y para ver la salud general del sistema.

- **Tracing (Trazabilidad Distribuida):** Es el concepto más importante para un arquitecto de sistemas distribuidos. Es como ponerle un "chip de seguimiento" a una sola coordenada GPS desde que entra al Gateway hasta que llega a la DB. Te permite ver cuánto tiempo pasó en cada salto.

## 2. Conceptos Clave que debes investigar

- **Correlation ID (ID de Correlación):** Es un identificador único (GUID) que se genera en el momento en que el Gateway recibe el paquete. Este ID debe viajar en el mensaje de la cola y ser registrado por el Worker. Si buscas ese ID, verás toda la "vida" de ese paquete en todos los servicios.

- **Distributed Tracing (OpenTelemetry):** Es el estándar actual de la industria para que diferentes herramientas (independientemente del lenguaje de programación) puedan compartir información de trazabilidad.

- **Health Checks:** Puntos finales (endpoints) que dicen si un servicio está "vivo" y "listo" para trabajar.

## 3. El Problema que resolveremos mañana

Mañana analizaremos cómo evitar que el sistema sea una "caja negra". Si un transportista se queja de que su unidad no se ve en el mapa, ¿cómo usamos estos conceptos para darle una respuesta técnica en menos de 5 minutos?

- **Tu tarea de lectura previa:**
  Busca qué es el Stack ELK (Elasticsearch, Logstash, Kibana) o Grafana Loki. No profundices en cómo instalarlos, sino en para qué sirven en un ecosistema de microservicios o sistemas distribuidos.
