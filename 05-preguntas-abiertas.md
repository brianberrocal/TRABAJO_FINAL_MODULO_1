# Preguntas Abiertas y Puntos de Decisión Pendientes

Este documento consolida las inconsistencias de arquitectura, vacíos normativos y decisiones de negocio que no se pueden asumir de manera autónoma por un agente de IA ni un desarrollador, y requieren una decisión formal por parte del equipo humano del proyecto **FiaYa**.

---

### PREG-01: Protocolo de Sincronización y Resolución de Conflictos Offline
- **Contexto:** En el Project Brief y el Documento de Alcance se especifica que la aplicación PWA debe funcionar con "conectividad intermitente / modo offline básico".
- **Incógnita:** ¿Cuál es la estrategia de sincronización autorizada cuando se recupera la red? Si dos ayudantes de bodega ingresan fiados al mismo tiempo sin conexión para el mismo cliente superando el límite de crédito, ¿cuál transacción tiene prioridad?
- **Opciones:**
  1. *First-write-wins* (la primera transacción procesada localmente prevalece).
  2. *Last-write-wins* con timestamp del cliente.
  3. Rechazo de transacciones offline que afecten el saldo contable sin previo handshake con PostgreSQL.
- **Acción requerida:** Definir la regla de negocio explícita (`RN-021`) para la sincronización PWA offline.

---

### PREG-02: Manejo de DNI Duplicados entre Bodegas
- **Contexto:** La regla `RN-001` exige cifrar el DNI con AES-256 (cifrado simétrico).
- **Incógnita:** ¿Un cliente (representado por su DNI) puede ser registrado en dos bodegas distintas? Dado que el cifrado es reversible pero no directo para búsquedas indexadas a menos que se use un hash ciego (*blind index*), ¿debe permitirse que dos bodegas registren al mismo cliente independientemente, o el sistema debe detectar duplicados globales?
- **Opciones:**
  1. Aislamiento total: El DNI es único solo dentro de la misma bodega (`bodega_id`, `dni_hash`).
  2. Detección global: Usar un HMAC / Hash determinista para evitar re-crear clientes globales sin violar la privacidad.
- **Acción requerida:** Determinar la política de unicidad de clientes a nivel de base de datos multitenant.

---

### PREG-03: Flujo y Política de Anulación/Eliminación de Transacciones
- **Contexto:** La regla `RN-017` prohíbe la eliminación definitiva sin doble confirmación y exige auditoría.
- **Incógnita:** ¿Se permitirá el borrado físico (*hard delete*) bajo algún escenario, o el sistema funcionará estrictamente como un libro contable inmutable (*soft delete* / nota de crédito de anulación)?
- **Opciones:**
  1. Implementar exclusivamente *soft deletes* mediante una columna `estado` (`ACTIVO`, `ANULADO`) para preservar trazabilidad.
  2. Permitir *hard delete* únicamente al usuario con rol "Dueño" ingresando su PIN de confirmación.
- **Acción requerida:** Aprobar el modelo de persistencia para anulación de transacciones.