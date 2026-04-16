# Estandares

- **Trazabilidad:** Todo paquete de datos DEBE llevar un Correlation ID generado en el Gateway.

- **Resiliencia:** Todo fallo de persistencia DEBE enviarse a una Dead Letter Queue (DLQ) tras 3 reintentos.

- **Monitoreo de Salud:** Los servicios deben implementar un endpoint de /health que valide:

  Conectividad con dependencias críticas (Cola/DB).
  Umbrales de recursos (CPU < 90%, Memoria < 85%).
