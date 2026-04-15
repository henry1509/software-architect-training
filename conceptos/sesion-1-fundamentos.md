# Conceptos de la Sesión 1: Desacoplamiento y Escalabilidad

En esta sesión transformamos un flujo lineal simple en una arquitectura distribuida básica para resolver problemas de carga masiva en un sistema AVL.

## 1. Desacoplamiento (Decoupling)

Es el principio de separar los componentes de un sistema para que no dependan directamente entre sí.

- **En nuestro caso:** Separamos el Gateway (receptor de datos) de la Persistencia (guardado en base de datos).

- **Por qué es importante:** Si la base de datos se vuelve lenta o falla, el Gateway sigue funcionando y recibiendo datos de las unidades. El sistema no se "muere" por completo.

## 2. Message Broker (Intermediario de Mensajes)

Es el componente que permite la comunicación asíncrona entre servicios.

- **Concepto clave:** Actúa como un Buffer (amortiguador). Almacena los datos temporalmente cuando la producción de datos es más rápida que la capacidad de procesamiento.

- **Herramientas comunes:** RabbitMQ, Kafka, Redis Streams.

## 3. Escalabilidad: Vertical vs. Horizontal

Como arquitecto, debes decidir cómo crecer:

- **Escalabilidad Vertical (Scaling Up):** Añadir más potencia (CPU/RAM) al mismo servidor. Es limitado y costoso a largo plazo.

- **Escalabilidad Horizontal (Scaling Out):** Añadir más servidores pequeños. Es el estándar de la arquitectura moderna porque permite un crecimiento casi infinito. Al usar una Cola, facilitamos este crecimiento porque podemos poner muchos "Workers" a leer de la misma cola.

## 4. Throughput vs. Latency (El gran Trade-off)

Esta es la balanza que siempre debes equilibrar:

- **Throughput (Caudal):** Cuántos datos puede procesar el sistema en un tiempo determinado (ej. 10,000 registros por minuto).

- **Latency (Latencia):** Cuánto tarda un solo dato en completar su camino (ej. 500ms desde el GPS al Mapa).

- **Nuestra decisión (Batching):** Elegimos agrupar datos (Batching) para maximizar el Throughput y proteger la base de datos, aceptando un ligero aumento en la Latencia.

## 5. Atributos de Calidad (Quality Attributes)

No son funciones del código, sino características del sistema:

- **Resiliencia:** Capacidad de recuperarse de fallos.

- **Elasticidad:** Capacidad de adaptarse a cambios en la carga (subir o bajar el número de unidades).

- **Consistencia Eventual:** Aceptar que el dato no estará disponible en todos lados instantáneamente, pero llegará eventualmente.

Lo que aprendiste a aplicar hoy:
Patrón: Producer-Consumer.

- **Técnica de Optimización:** Batch Insertion con Flush por tiempo/tamaño.

- **Documentación:** El uso de ADRs para dejar rastro de por qué se tomó una decisión técnica.
