# Completa los puntos clave:

## **Contexto:** Al tener una arquitectura desacoplada, es difícil rastrear el flujo de un solo paquete de datos.

## **Decisión:** Implementar un ID de Correlación generado en el Gateway.

## **Consecuencias Positivas:** Reducción drástica del tiempo de depuración (MTTR - Mean Time To Repair).

## **Consecuencias Negativas:** Ligero aumento en el tamaño del mensaje (un GUID más en cada paquete).
