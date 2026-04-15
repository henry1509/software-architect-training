# ADR 001: Implementación de Message Broker para Ingesta de Datos AVL

## Estado

Propuesto

## Contexto

El sistema AVL debe soportar un crecimiento de 50 a 2,000 unidades. Cada unidad envía coordenadas cada 5 segundos. La escritura directa en la base de datos relacional genera bloqueos de tablas y pérdida de paquetes UDP/TCP durante picos de tráfico.

## Decisión

Implementar un Message Broker (ej. RabbitMQ o Redis Streams) entre el servicio de recepción (Gateway) y el servicio de persistencia.

## Justificación

1. **Escalabilidad Horizontal:** Podemos tener múltiples "consumidores" leyendo de la cola si la carga aumenta.
2. **Resiliencia:** Si la base de datos cae para mantenimiento, los datos del GPS no se pierden; se quedan en la cola hasta que el servicio vuelva.
3. **Costo-Beneficio:** Evita pagar por un servidor de base de datos gigante (Escalado Vertical) que estaría ocioso la mayor parte del tiempo.

## Consecuencias

- **Complejidad:** Añadimos un componente más a la infraestructura que debe ser monitoreado.
- **Latencia:** Introducimos un retraso mínimo (milisegundos) entre la recepción y la visualización final.

graph LR
subgraph Vehiculos ["Vehículos"]
U1[Unidad 1]
U2[Unidad 2]
U3[Unidad N]
end

    Gateway[Gateway Service]
    Queue((Cola de Mensajes))
    Worker[Worker de Persistencia]
    DB[(Base de Datos)]

    U1 & U2 & U3 --> Gateway
    Gateway --> Queue
    Queue --> Worker
    Worker --> DB
