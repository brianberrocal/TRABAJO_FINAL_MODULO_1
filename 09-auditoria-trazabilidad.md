# Auditoría de trazabilidad y consistencia del Spec Pack — Muuvi
**Versión:** 1.2.0
**Historial:** 1.0.0 (14/11/2026) auditoría inicial · 1.1.0 (14/11/2026) severidad en 4 niveles, formato Ubicación / Problema / Sugerencia y puntaje de madurez. El Spec Pack auditado es idéntico en las dos versiones. · 1.2.0 (14/11/2026) estado de las correcciones de los hallazgos CRÍTICA y ALTA (ver [Estado de las correcciones](#estado-de-las-correcciones-14112026)).
**Fecha:** 14/11/2026
**Autor:** Agente QA de Backlog (BMAD)
**Alcance auditado:** reglas de negocio (RN-001 a RN-150), historias de usuario (HU-001 a HU-016), documento de arquitectura (C1 a C10, §4 API, ADR-005 a ADR-009, §6.1 a §6.3), backlog (T-001 a T-044) y documento de alcance, tal como se entregaron en el pedido.
**Método:** extracción automática de las referencias `RN-NNN` y `HU-NNN` de cada documento, revisión manual de la cobertura implícita (tareas que implementan una regla sin citarla) y recálculo con script de 26 fechas, plazos y montos.

---

## Resumen ejecutivo
El Spec Pack es sólido en su núcleo: el stack respeta la constitución, las 44 tareas forman un grafo sin ciclos y todas trazan a una HU o a infraestructura, todos los endpoints trazan a una HU, y los 26 valores numéricos que recalculé (cortes, feriados, días hábiles, backoff, 900 s, 72 h, 30 días, importes) coinciden en todos los archivos. De las 146 reglas Must, **112 tienen tarea**, **7 la tienen solo en parte** y **27 no tienen ninguna**. El problema principal no es de calidad sino de cobertura: la iteración 1 **crea importes a devolver y avisa al cliente para que elija, pero no implementa ni la elección ni el reembolso**, y no hay forma de cargar en producción el precio, las fincas ni los productos que un agente no puede insertar. Hay **4 hallazgos críticos, 14 mayores y 7 sugerencias**, y el Spec Pack obtiene un puntaje de madurez de **3 sobre 5**.

| Categoría | Severidad | Cantidad | IDs |
|---|---|---|---|
| 🔴 Crítico | CRÍTICA | 4 | QA-C2-001 a QA-C2-004 |
| 🟡 Mayor | ALTA | 9 | QA-C2-005 a 008, QA-C2-012 a 016 |
| 🟡 Mayor | MEDIA | 5 | QA-C2-009 a 011, QA-C2-017, QA-C2-018 |
| 🟢 Sugerencia | BAJA | 7 | QA-C2-019 a QA-C2-025 |

**Puntaje de madurez: 3 / 5** al momento de la auditoría; **4 / 5 después de las correcciones del 14/11/2026** (ver [Estado de las correcciones](#estado-de-las-correcciones-14112026)). El contenido está bien especificado y es consistente, pero el backlog todavía no permite que los agentes construyan un PMV que pueda salir a producción. El detalle está al final del reporte.

**Cómo leer cada hallazgo:** la **severidad** usa cuatro niveles (CRÍTICA, ALTA, MEDIA y BAJA) junto a su categoría (🔴, 🟡 o 🟢). La **ubicación** indica el archivo y la regla, HU, tarea o sección. El **problema** dice qué está mal, y la **sugerencia**, cómo corregirlo.

**Lo que está bien y no requiere acción:**
- **Stack:** React 18 PWA, Node.js 20 con Express y PostgreSQL 16, sin Redis ni otros motores (PRINCIPIO-002, ADR-007). Las dependencias de la §6.3 están aprobadas.
- **Trazabilidad hacia atrás:** las 44 tareas citan una HU o son de infraestructura; no hay tareas huérfanas. Los 21 endpoints de la arquitectura citan una HU.
- **MoSCoW:** las 42 tareas Must trazan a HU Must, y las 2 Should (T-043, T-044) son de soporte. No hay tareas de funciones Won't ni Could.
- **Valores numéricos:** S/ 14.50 por litro, 3 L = S/ 43.50, corte a las 18:00:00 = 23:00:00 UTC, feriado del martes 08/12/2026 → jueves 10/12/2026, 15 días hábiles desde el 16/11/2026 → 07/12/2026 (desde el 24/11 → 16/12; desde el 20/11 y el 21/11 → 14/12), 10.º día hábil → 30/11/2026, backoff 18:01:05 … 18:31:05, 900 s (06:10 → 06:25 = 11:25Z), 1 800 s, 72 h y 30 días: todo consistente.
- **Sin conflictos de fondo entre HU:** las HU no se contradicen entre sí. Las contradicciones encontradas están entre una regla y una HU (QA-C2-009) o entre el alcance y las reglas (QA-C2-011).

---

## Hallazgos

### 🔴 Críticos

**QA-C2-001: El flujo del importe a devolver queda abierto: se avisa al cliente, pero no puede elegir ni recibir el reembolso**
Severidad: **CRÍTICA** (🔴 Crítico)
Ubicación: `08-backlog.md` (T-027, T-028, T-030, T-031, T-032), `07-architecture.md` §4.10, `06-user-stories-refinadas.md` (HU-007, HU-009).
Problema: En la iteración 1, T-027 (pago tardío), T-030 (escasez) y T-031 (lote invalidado) crean un `importe_devolver` y envían el aviso de elección de RN-129. Pero ninguna tarea implementa:
- `POST /importes-devolver/{id}/eleccion` (§4.10 de la arquitectura; RN-120 y RN-121);
- la tarea `reembolso_automatico_72h` (RN-130);
- la llamada a `reembolsar` de la pasarela y su reintento (RN-055 y RN-105; T-019 solo define la interfaz);
- el reembolso sin elección de una entrega cancelada (RN-108) y el del saldo al cancelar o darse de baja (RN-124);
- la nota de crédito (RN-057): T-028 solo emite boletas y facturas.

Además, T-032 no genera importe a devolver para una entrega `fallida` con causa `imputable_muuvi` (RN-053), y T-031 no lo genera para las entregas `en_ruta` suspendidas por lote invalidado (RN-079). Resultado: el sistema cobra, avisa al cliente de una opción que no existe y retiene el dinero sin plazo, lo que contradice RN-130 y expone a Muuvi ante INDECOPI.
Sugerencia: Agregar a la iteración 1 estas tareas:
- **T-045** `POST /importes-devolver/{id}/eleccion` (HU-009);
- **T-046** servicio `devoluciones` con `solicitarReembolso` y el manejador `reintento_reembolso` (RN-055, RN-105, RN-108, RN-120 y RN-124);
- **T-047** tarea `reembolso_automatico_72h` (RN-130);
- **T-048** nota de crédito en `comprobantes` (RN-057).

Extender T-032 con RN-053 y T-031 con RN-079. Si no caben en la iteración 1, hay que retirar de T-027, T-030 y T-031 el envío del aviso de elección, para no prometer una opción inexistente.

---

**QA-C2-002: Varias tareas necesitan dependencias que no están aprobadas, y el agente debería detenerse**
Severidad: **CRÍTICA** (🔴 Crítico)
Ubicación: `07-architecture.md` §6.3, `08-backlog.md` (T-004, T-034 a T-037, T-042).
Problema: Según el PRINCIPIO-008.1, un agente no puede agregar dependencias sin aprobación humana. Sin embargo:
- **T-004** pide "lint", pero ningún linter (`eslint` o similar) está en la §6.3.
- **T-004** exige que la cobertura falle por debajo del 80 % y del 60 %:
  - en el backend, hasta donde sé, el ejecutor `node:test` de Node.js 20 mide la cobertura en modo experimental, pero no admite umbrales (llegaron en versiones posteriores);
  - en el frontend, `vitest` necesita `@vitest/coverage-v8` para medir la cobertura.
- **T-034 a T-037** piden "tests de componente" con `axe-core`. Eso requiere un DOM simulado (`jsdom` o `happy-dom`) y, en la práctica, `@testing-library/react`. Ninguno de los tres está aprobado.

Un agente que siga la constitución se detendrá en T-004 y en T-034 a T-037. T-042 depende de esas tareas, así que también queda bloqueada.
Sugerencia: Que el fundador apruebe estas dependencias de desarrollo, o las rechace explícitamente, y que se agreguen a la §6.3:
- un linter (`eslint`);
- `@vitest/coverage-v8`;
- `jsdom` o `happy-dom`;
- `@testing-library/react`;
- en el backend, `c8` o un script propio que lea la salida de `--experimental-test-coverage` y aplique los umbrales.

Si se rechazan, hay que reescribir los criterios de T-004 y de T-034 a T-037 para que no las necesiten.

---

**QA-C2-003: La baja y la anonimización (PRINCIPIO-005) no tienen ninguna tarea**
Severidad: **CRÍTICA** (🔴 Crítico)
Ubicación: `08-backlog.md` ("Fuera de esta iteración"), `06-user-stories-refinadas.md` (HU-002).
Problema: HU-002 solo aparece en T-011, que es el conector con el KMS. Ninguna tarea implementa:
- `POST /clientes/me/baja` ni los bloqueos de RN-098;
- la tarea `anonimizar_bajas` (RN-008), que debe correr dentro de los 30 días;
- la conservación de comprobantes al anonimizar (RN-111).

El PRINCIPIO-005 y la Ley 29733 obligan a eliminar los datos en un máximo de 30 días tras la baja. Si la iteración 1 llega a producción tal como está, Muuvi incumple la constitución y la ley desde la primera baja.
Sugerencia: Agregar dos tareas:
- **T-049** `POST /clientes/me/baja` con los bloqueos de RN-098 y el reembolso del saldo (RN-124, que depende de T-046);
- **T-050** tarea `anonimizar_bajas` con borrado criptográfico (ADR-006), conservando los comprobantes.

También hay que declarar en el backlog que **ninguna versión sale a producción sin T-049 y T-050**, igual que la inscripción ante la ANPDP.

---

**QA-C2-004: No hay forma de cargar en producción el precio, las fincas ni los productos, así que el catálogo y la suscripción responden 503**
Severidad: **CRÍTICA** (🔴 Crítico)
Ubicación: `07-architecture.md` §4.12, `06-user-stories-refinadas.md` (HU-003, HU-013), `08-backlog.md`, `03-reglas-de-negocio.md` (matriz de permisos).
Problema: T-009 prohíbe con razón insertar `precio_litro` (PRINCIPIO-008.9), y T-017 y T-024 responden `503` si falta el precio. Pero el único mecanismo para que un humano lo cargue es HU-013, que no tiene tareas y espera P14 y P18. Lo mismo pasa con el catálogo:
- la matriz de permisos dice que el operador "registra fincas, productos, registro DIGESA y certificación SENASA";
- HU-003 tiene el criterio "el operador intenta publicarlo";
- pero **no hay endpoint en la §4, ni HU propia, ni tarea** para registrar una finca o un producto.

Solo la semilla de desarrollo (T-044) crea esos datos, y no puede correr en producción. Resultado: el PMV no puede abrir al público aunque se completen todas las tareas.
Sugerencia:
1. Crear la **HU-017 "Registrar fincas y productos"** (F-M02, RN-009, RN-010 y RN-099) con los endpoints `POST /operacion/fincas`, `PATCH /operacion/fincas/{id}`, `POST /operacion/productos` y `POST /operacion/productos/{id}/publicacion`, y agregarlos a la §4.12 y al backlog.
2. Para el precio, elegir una de estas dos vías:
   - adelantar a la iteración 1 la parte de HU-013 que no depende de P14 (proponer y confirmar), con el mecanismo de P18 decidido;
   - documentar en `docs/configuracion-entorno.md` un procedimiento manual que ejecuta el fundador, no un agente, para la carga inicial de `precio_litro`, con registro en `auditoria`.

---

### 🟡 Mayores

#### Severidad ALTA

**QA-C2-005: La arquitectura no tiene endpoints para asignar las entregas ni para pasarlas a `en_ruta`**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `07-architecture.md` §4 y §2.2, `08-backlog.md` (T-030, T-032).
Problema: El diagrama de estados de la entrega tiene la transición `programada → en_ruta` ("el repartidor sale con la entrega"), y RN-077 y RN-133 dependen del estado `en_ruta`. Pero ningún endpoint de la §4 produce esa transición. Tampoco hay un endpoint ni una regla de servicio para:
- asignar un `repartidor_id` a cada entrega ("Generar la lista de entregas del día", en la matriz de permisos);
- asignar las entregas a un lote en el caso normal. T-030 solo reparte cuando hay escasez.

Sin esa asignación no se pueden cumplir RN-027 (no asignar botellas de un lote invalidado), RN-068 (reprogramada con un lote del mismo día) ni RN-113 (no reutilizar el lote original).
Sugerencia: Agregar a la §4 estos endpoints y su tarea:
- `POST /operacion/dias/{fecha}/asignaciones`, con el que el operador asigna lote y repartidor;
- `POST /reparto/salidas`, con el que el repartidor marca `en_ruta`, también sin conexión, en la misma cola de T-036.

Detallar en la tarea el algoritmo de asignación de lotes, con RN-027, RN-068, RN-093 y RN-113.

---

**QA-C2-006: El componente C2 (PWA, módulo operación) no tiene ninguna tarea**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `08-backlog.md`, `07-architecture.md` §2.1.
Problema: C2 es el único de los 10 componentes sin tareas. Sin él, el operador solo puede usar la API directamente para:
- registrar lotes (HU-010) y corregir temperaturas;
- cargar feriados (HU-015) y ver la lista de entregas;
- atender reclamos (HU-012).

Con 50 clientes y un fundador único (S9), eso no es operable.
Sugerencia: Agregar al menos tres tareas de C2:
- registro de lote y corrección de temperatura (HU-010);
- lista del día, asignación de entregas y saldo de botellas (QA-C2-005);
- carga de feriados y bandeja de reclamos con su fecha límite (HU-015 y HU-012).

---

**QA-C2-007: HU-006, HU-013, HU-014 y HU-016 (todas Must) no tienen tareas**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `08-backlog.md`, `06-user-stories-refinadas.md`.
Problema: El backlog no cubre estas HU, que dependen de reglas Must:
- **HU-006** (modificar o cancelar): RN-014, RN-081, RN-082 y RN-083 sin tarea. Esta HU **no** está bloqueada por ninguna decisión, y T-029 (corte) debería aplicar los cambios pendientes de `cambio_suscripcion` y las cancelaciones.
- **HU-013** (precio): bloqueada por P14 y P18; ver QA-C2-004.
- **HU-014** (anulaciones): bloqueada por P18. La parte "un proceso automático no puede borrar" (RN-032) ya la cubren los *triggers* de T-007 y T-009.
- **HU-016** (respuesta a reclamos): RN-146 a RN-149 son un borrador pendiente del asesor legal, pero la alerta del día 10 (RN-036, de la HU-012) no está bloqueada y tampoco tiene tarea.
Sugerencia: Agregar a la iteración 1:
- una tarea para HU-006, con `PATCH /suscripciones/{id}` y `POST /suscripciones/{id}/cancelacion`);
- una para `alerta_reclamo_dia_10` (RN-036).

Extender T-029 para que aplique los cambios de RN-083. Dejar HU-013, HU-014 y HU-016 con la nota explícita de su bloqueo.

---

**QA-C2-008: Los efectos de invalidar un lote están incompletos en el backlog**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `08-backlog.md` (T-030, T-031, T-032, T-040).
Problema: T-031 cubre la invalidación, la reasignación (RN-093), las entregas fallidas sin reasignación (RN-054) y la suspensión de las entregas `en_ruta` (RN-077). Le faltan:
- RN-078: aviso de disculpa, en 3 600 s, a los clientes que ya recibieron botellas del lote;
- RN-079: importe a devolver de las entregas suspendidas (ver QA-C2-001);
- RN-133: aviso a cada **repartidor** con entregas `en_ruta` del lote, en 60 s.

T-030 y T-032 alertan al operador y al administrador, pero no a los repartidores. RN-133 es la única protección sanitaria en la puerta del cliente.
Sugerencia: Agregar RN-078 y RN-079 a la descripción y al criterio de T-031, y RN-133 a T-030 y T-032, con `alertarInterno({ rol: 'repartidor', … })` a los repartidores de las entregas `en_ruta` del lote. Incluir los tres casos en T-040.

---

**QA-C2-012: Algunos ejemplos de las reglas no reflejan la revisión de 900 s y producirían tests equivocados**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `03-reglas-de-negocio.md` (RN-054, RN-077, RN-078, RN-093).
Problema: Los tests de aceptación se escriben a partir de los ejemplos (T-038 a T-040), pero estos son anteriores a la versión 0.6.0 y no reflejan la revisión de 900 s:
- **RN-054:** "el lote se recibe a 5.0 °C; las 12 entregas… quedan como fallida", sin los 900 s de revisión.
- **RN-093:** "el lote L-2026-1114-01 se invalida a las 06:10". La HU-010 dice que entra en revisión a las 06:10 y se invalida a las 06:25.
- **RN-077 y RN-078:** "se invalida a las 08:20". La HU-011 dice que entra en revisión a las 08:20 y se invalida a las 08:35.

Un agente que escriba el test a partir del ejemplo esperaría la invalidación en el acto.
Sugerencia: Reescribir los cuatro ejemplos con la secuencia completa: lectura alta a las 06:10:00, invalidación a las 06:25:00 y efecto a partir de ese momento.

---

**QA-C2-013: El contrato de la API no permite registrar el RUC, y el registro revela si un correo existe**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `07-architecture.md` §4.2 y §4.12, `08-backlog.md` (T-003, T-016).
Problema:
- La HU-008 pide "ingreso el RUC… lo guardo" y RN-058 y RN-060 lo validan y cifran. Pero `POST /clientes` (§4.2) solo trae `tipo_comprobante`, sin `ruc`, y no hay ningún endpoint para editar los datos de facturación. Con el contrato actual no se puede emitir una factura.
- La §4.2 responde `409` cuando el correo ya está registrado y, en la misma fila, dice "no debe revelar que el correo existe". Un `409` distinto de la respuesta de éxito revela justamente eso.
Sugerencia:
- Agregar `PATCH /clientes/me/facturacion` con `{ "tipo_comprobante", "ruc" }`, que valida RN-058 y cifra según RN-060, y su tarea.
- Para SR-02, que el BA escriba la regla. La propuesta es responder igual que un registro exitoso (`202`) y enviar al correo un aviso de "ya tienes cuenta".

---

**QA-C2-014: El contrato OpenAPI no cubre todos los endpoints de la iteración 1 (PRINCIPIO-010)**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `08-backlog.md` (T-003, T-015), `07-architecture.md` §4.12.
Problema: T-003 genera 12 *paths*, pero la iteración 1 implementa además estos endpoints:
- `POST /sesiones/renovar` y `DELETE /sesiones` (T-013);
- `POST /suscripciones/{id}/reactivacion` (T-025);
- `POST /operacion/feriados` y `GET /operacion/feriados` (T-015);
- `GET /operacion/dias/{fecha}/entregas` (T-032);
- `GET /api/v1/salud` (T-001).

Además, `GET /operacion/feriados` no figura en la arquitectura. El principio D11 ("contrato primero") y el PRINCIPIO-010 exigen que cada endpoint exista en `openapi.yaml` antes de implementarse.
Sugerencia: Ampliar T-003 a todos los endpoints de la iteración 1 y agregar `GET /operacion/feriados` a la §4.12. Agregar a T-004 una verificación que falle si una ruta de Express no está en `openapi.yaml`.

---

**QA-C2-015: RN-118 (no enviar datos a proveedores sin DPA) no tiene ni HU ni tarea**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `06-user-stories-refinadas.md`, `08-backlog.md` (T-019 a T-022, T-028).
Problema: RN-118 es Must, y hoy **todos** los encargados figuran con el DPA ⏳ pendiente en `docs/proveedores-datos.md`. Ninguna HU la cita y ninguna tarea impide que un adaptador (Culqi, Yape, Plin, OSE o correo) envíe datos a un proveedor sin DPA vigente.
Sugerencia: Agregar una tarea con una lista de proveedores habilitados, `proveedores_habilitados` en `configuracion`, que carga un humano. Los adaptadores reales fallan si su proveedor no está en la lista, y los simulados no se ven afectados. Citar RN-118 en HU-001 y HU-007.

---

**QA-C2-016: Faltan tareas de despliegue, respaldo y monitoreo**
Severidad: **ALTA** (🟡 Mayor)
Ubicación: `08-backlog.md`.
Problema: El backlog cubre la configuración, el CI y los tests, pero no tiene tareas de:
- despliegue y entornos (desarrollo, staging y producción);
- configuración del proxy solo con TLS 1.3 (PRINCIPIO-003, §6.1);
- respaldo y prueba de restauración de PostgreSQL, que guarda pagos y comprobantes que SUNAT exige conservar (RN-111);
- monitoreo del worker: tareas `agotadas`, outbox atascado y retraso del corte de las 18:00;
- gestión de secretos (la hace un humano, pero la tarea debe existir).

Casi todas dependen de G3 (hosting), pero tienen que estar en el backlog para que la ruta crítica sea real.
Sugerencia: Agregar tareas de infraestructura bloqueadas por G3:
- entornos y despliegue continuo;
- proxy con TLS 1.3 y un test que rechace TLS 1.2;
- respaldo diario con restauración probada;
- métricas y alertas del worker.

---

#### Severidad MEDIA

**QA-C2-009: RN-049 contradice a la HU-007 sobre cuándo se pide actualizar el medio de pago**
Severidad: **MEDIA** (🟡 Mayor)
Ubicación: `03-reglas-de-negocio.md` (RN-049), `06-user-stories-refinadas.md` (HU-007), `08-backlog.md` (T-026).
Problema:
- RN-049 dice "CUANDO la pasarela rechaza el cobro de un ciclo", y su ejemplo envía el correo a las 18:02:00, después del primer rechazo y antes de que se resuelva el reintento.
- La HU-007 (criterio 4) y T-026 envían el correo **después del segundo rechazo**.

Con RN-049 tal como está escrita, un cliente cuyo reintento de las 18:01:00 se aprueba igual recibe a las 18:02:00 un correo diciendo que su pago falló.
Sugerencia: Que el fundador decida en qué momento se envía. La recomendación es reescribir RN-049 como "CUANDO el **reintento** de cobro de un ciclo es rechazado…", con el ejemplo "reintento rechazado a las 18:01:00, correo a las 18:03:00".

---

**QA-C2-010: RN-042 y RN-043 están como SHOULD, pero son una obligación constitucional**
Severidad: **MEDIA** (🟡 Mayor)
Ubicación: `03-reglas-de-negocio.md` (RN-042, RN-043 e índice por funcionalidad).
Problema: La ventana de 09:00 a 20:00 para los avisos al cliente viene del PRINCIPIO-008.4, que es obligatorio. Sin embargo, RN-042 y RN-043 están clasificadas como SHOULD, dentro de F-S02. T-022 las implementa como Must, lo que es correcto, pero un agente que priorice por la etiqueta de la regla podría omitirlas.
Sugerencia: Pasar RN-042 y RN-043 a MUST y moverlas a "Transversales" en el índice. RN-044 (trazabilidad, F-S03) sigue como SHOULD.

---

**QA-C2-011: El alcance sigue diciendo que una lectura alta invalida el lote al instante y no lista al administrador titular**
Severidad: **MEDIA** (🟡 Mayor)
Ubicación: `02-alcance.md` (§1, F-M05 y §3).
Problema:
- F-M05 y el párrafo del PMV (§3 del alcance) dicen que "una lectura mayor de 4.5 °C… genera una alerta crítica e **invalida el lote**". Desde la versión 0.6.0, las reglas pasan el lote a `en_revision` durante 900 s y solo después lo invalidan (RN-024, RN-127 y RN-134).
- La tabla de usuarios del alcance tiene solo Cliente, Operador y Repartidor. Faltan:
  - el **Administrador titular**, que es el único que confirma precios, anula registros y revierte lotes (RN-032, RN-127 y la matriz de permisos);
  - el **consumidor sin cuenta** del Libro de Reclamaciones (RN-109).
Sugerencia: Actualizar F-M05 y §3 con "pasa el lote a revisión durante 15 minutos y, si nadie lo revierte, lo invalida". Agregar los dos roles a la tabla de usuarios.

---

**QA-C2-017: Las versiones y fechas del Spec Pack entregado no coinciden con las del proyecto**
Severidad: **MEDIA** (🟡 Mayor)
Ubicación: `07-architecture.md`, `08-backlog.md`, `06-user-stories-refinadas.md`, más el paquete que se entrega a los agentes.
Problema:
- **Arquitectura:** el documento entregado dice "v1.0.1, 28/10/2026", pero incluye los ADR aceptados el **14/11/2026**, una fecha imposible. En el proyecto es la v1.0.0 del 14/11/2026. Además, le faltan la §6.4 y la §6.5, y T-043 cita la §6.4.
- **Backlog:** el entregado dice "v1.0.0, 28/10/2026, Sprint 1 & 2, Aprobado para Ejecución", pero en el proyecto es la v0.2.0 del 14/11/2026. Al entregado le faltan el resumen, las **compuertas humanas** (G2 a G6) y la sección "Fuera de esta iteración", así que un agente no puede saber que T-004, T-011, T-020 y T-021 están bloqueadas.
- **Historias de usuario:** las entregadas dicen v1.1.0, y el proyecto tiene la v0.2.0.
- El encabezado del pedido dice "Proyecto: [NOMBRE_DEL_PRODUCTO]".
Sugerencia: Entregar a los agentes siempre la versión del proyecto, completa e incluida la sección de compuertas, y no una copia editada. Si las versiones 1.x son intencionales, registrarlas en el historial de cada documento con su fecha real.

---

**QA-C2-018: Ninguna tarea crea las máquinas de estado explícitas (principio D8)**
Severidad: **MEDIA** (🟡 Mayor)
Ubicación: `08-backlog.md`, `07-architecture.md` (D8).
Problema: El principio D8 y RN-070 y RN-071 piden máquinas de estado explícitas para la suscripción, la entrega, el lote, el pago y el reclamo, y que se rechace toda transición no prevista. Hoy las transiciones quedan repartidas en T-025, T-026, T-027, T-031 y T-032, sin un módulo común ni un test que recorra cada diagrama. El criterio de la HU-011 "`entregada` → `en_ruta` se rechaza" no está en ninguna tarea.
Sugerencia: Agregar una tarea temprana, después de T-001, que cree `backend/src/modulos/*/estados.js` con las tablas de transiciones de los cuatro diagramas de `03-reglas-de-negocio.md` y una función `transicionar(entidad, de, a)`. Su test debe probar cada transición permitida y al menos una prohibida por estado. Las tareas que cambian estados pasan a depender de ella.

---

### 🟢 Sugerencias

**QA-C2-019: Hay siete reglas Must que no se citan en ninguna HU**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `06-user-stories-refinadas.md`.
Problema: RN-038 (fechas `DD/MM/AAAA`), RN-040 (UTC), RN-041 (`es-PE.json`), RN-069 (crear la entrega en el corte), RN-110 (montos negativos), RN-150 (alertas internas) y RN-118 (ver QA-C2-015) no figuran en las "Reglas de negocio relacionadas" de ninguna HU. Las seis primeras están cubiertas de forma implícita (T-002, T-007, T-022, T-029, T-035), así que la trazabilidad se pierde solo en papel.
Sugerencia:
- RN-038 y RN-110 → HU-003 y HU-008;
- RN-040 y RN-041 → HU-001;
- RN-069 → HU-004 y HU-010;
- RN-150 → HU-010 y HU-011.

---

**QA-C2-020: Algunas reglas cubiertas no tienen un test explícito en el backlog**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `08-backlog.md` (T-016, T-032, T-035, T-038).
Problema: Estas reglas están cubiertas de forma implícita, pero ningún criterio de completitud las verifica:
- RN-116: registrarse y pagar sin el consentimiento de terceros;
- RN-067: no cobrar una entrega reprogramada;
- RN-015: franja única 07:00–10:00;
- RN-110: montos negativos;
- RN-065: 1 botella por litro, verificada solo de forma indirecta.

El PRINCIPIO-009 pide cobertura ≥ 80 % de las reglas.
Sugerencia: Agregar estos casos a los criterios:
- a T-016: "con `terceros: false` → 201";
- a T-032: "la reprogramada no crea ciclo ni pago";
- a T-024: "`franja` es siempre `07:00-10:00`";
- a T-035: "`-43.50` se muestra como `-S/ 43.50`".

---

**QA-C2-021: Las HU tienen notas desactualizadas**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `06-user-stories-refinadas.md`.
Problema:
- La HU-003 dice "el cambio de precio no tiene HU propia; ver la HU-013 sugerida", y la HU-012 dice "ver HU-016 sugerida". Las dos ya están aprobadas.
- El encabezado dice "Ámbito: Must y Should", pero las 16 HU son Must.
- Las particiones sugeridas de HU-007, HU-009, HU-010 y HU-011 (007a/b/c, etc.) no se aplicaron, y el backlog traza a la HU entera.
Sugerencia: Actualizar las notas y el encabezado. Aplicar las particiones o descartarlas de forma explícita, y si se aplican, hacer que las tareas citen la subhistoria (por ejemplo, T-026 → HU-007a y HU-007b).

---

**QA-C2-022: La arquitectura resuelve en silencio dos escenarios que siguen sin regla**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `07-architecture.md` §4.4 y §4.5, `03-reglas-de-negocio.md`.
Problema:
- La §4.4 exige `medio_pago_id` para crear una suscripción, lo que decide SR-05 (¿se exige un medio de pago al suscribirse?) sin regla.
- La §4.5 responde `409` al pausar una suscripción `suspendida_por_pago`, lo que decide SR-07, aunque lo marca con ❓.

T-025 lo deja, con razón, como `test.todo`.
Sugerencia: Que el BA escriba las reglas de SR-05 y SR-07, y marcar en la §4.4 que el campo obligatorio depende de SR-05.

---

**QA-C2-023: Dos reglas son ambiguas para implementarlas**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `03-reglas-de-negocio.md`.
Problema:
- **RN-124:** reembolsa el saldo "al medio de pago con que se pagó el ciclo que generó ese saldo". Si el saldo se formó con varios ciclos pagados con medios distintos, hay varios medios posibles.
- **RN-098:** bloquea la baja con una "suscripción activa", pero no dice qué pasa con una suscripción `pausada` o `suspendida_por_pago`.
Sugerencia: Agregar a RN-124 que se reembolsa por partes a cada medio de origen, según los movimientos de `movimiento_saldo`. Agregar a RN-098 qué estados de la suscripción bloquean la baja (propuesta: cualquiera distinto de `cancelada`).

---

**QA-C2-024: RN-150 usa "PUEDE", que no es un patrón EARS**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `03-reglas-de-negocio.md` (RN-150).
Problema: El catálogo define los patrones EARS con "DEBE". RN-150 dice "El sistema PUEDE enviar a cualquier hora…", que no es verificable como requisito.
Sugerencia: Reescribirla como "El sistema DEBE enviar las alertas internas de operación… en el plazo de la regla que las origina, a cualquier hora, incluso fuera de la ventana de 09:00 a 20:00 (ADR-004)".

---

**QA-C2-025: El alcance y las HU citan leyes distintas para el Libro de Reclamaciones**
Severidad: **BAJA** (🟢 Sugerencia)
Ubicación: `02-alcance.md`, `06-user-stories-refinadas.md`.
Problema: El alcance (F-M06 y §5.2) cita la Ley 29571 (Código de Protección y Defensa del Consumidor), y la HU-016 cita la Ley 31435, que fija el plazo de 15 días hábiles. Ninguno de los dos documentos menciona la otra ley.
Sugerencia: Citar las dos leyes en F-M06 y en la HU-012. Que el asesor legal confirme la base normativa vigente, junto con la validación de RN-146 a RN-149.

---

## Matriz de trazabilidad

### Por funcionalidad: reglas → HU → componentes → tareas
| Funcionalidad | Reglas Must | HU | Componentes | Tareas | Reglas Must sin tarea | Estado |
|---|---|---|---|---|---|---|
| F-M01 Registro y cuenta | RN-001 a RN-008, RN-098, RN-111, RN-114 a RN-116, RN-118, RN-124 | HU-001, HU-002 | C1, C5, C6 `cuentas`, C8, C9 | T-006, T-010, T-011, T-016, T-034, T-041 | RN-008, RN-098, RN-111, RN-118, RN-124 (parcial: RN-115, texto humano) | 🟨 Registro ✅, baja ❌ (QA-C2-003) |
| F-M02 Catálogo y precio | RN-009 a RN-011, RN-102, RN-103 | HU-003, HU-013 | C1, C2, C6 `catalogo` y `configuracion` | T-006, T-017, T-029, T-035 | RN-103 (⛔ P14) | 🟨 Sin alta de fincas y productos ni precio (QA-C2-004) |
| F-M03 Suscripción | RN-012 a RN-018, RN-061 a RN-065, RN-081 a RN-084, RN-123 | HU-004, HU-005, HU-006 | C1, C6 `suscripciones`, C7 | T-007, T-014, T-024, T-025, T-029, T-035 | RN-014, RN-081, RN-082, RN-083 | 🟨 Alta y pausa ✅, modificar y cancelar ❌ (QA-C2-007) |
| F-M04 Cobro, comprobante y reembolso | RN-019 a RN-023, RN-045 a RN-060, RN-072 a RN-075, RN-079, RN-080, RN-094 a RN-097, RN-104 a RN-106, RN-108, RN-119 a RN-121, RN-125, RN-126, RN-129 a RN-131, RN-135 a RN-138 | HU-007, HU-008, HU-009 | C6 `pagos`, `comprobantes` y `devoluciones`, C7, C10 | T-008, T-019 a T-021, T-023, T-026 a T-028, T-039 | RN-055, RN-057, RN-079, RN-108, RN-120, RN-121, RN-130 (parcial: RN-053, RN-105, RN-137 ⛔ P23) | 🟨 Cobro ✅, devoluciones ❌ (QA-C2-001) |
| F-M05 Operación diaria | RN-024 a RN-032, RN-066 a RN-071, RN-076 a RN-078, RN-085 a RN-093, RN-107, RN-113, RN-122, RN-127, RN-128, RN-132 a RN-134, RN-139 a RN-145 | HU-010, HU-011, HU-014, HU-015 | C2, C3, C6 `operacion`, C7 | T-007, T-015, T-029 a T-032, T-036, T-040 | RN-068, RN-078, RN-113, RN-133 (parcial: RN-032, RN-070, RN-071) | 🟨 Sin C2, sin asignación ni `en_ruta` (QA-C2-005, 006, 008) |
| F-M06 Libro de Reclamaciones | RN-033 a RN-036, RN-109, RN-112, RN-146 a RN-149 | HU-012, HU-016 | C4, C6 `reclamos` | T-009, T-033, T-037, T-038 | RN-036, RN-146 a RN-149 (borrador legal) | 🟨 Registro ✅, alerta y respuesta ❌ (QA-C2-007) |
| Transversales | RN-037 a RN-041, RN-099 a RN-101, RN-110, RN-150 | Todas | C1, C5, C6 `calendario` y `permisos` | T-002, T-012 a T-015, T-022, T-035 | RN-110 | ✅ |
| F-S02 y F-S03 (Should) | RN-042 a RN-044 (SHOULD) | — | C6 `notificaciones` | T-022 (RN-042 y RN-043) | — | ✅ (subir a MUST, QA-C2-010) |

**Totales de reglas Must:** 146 en total. **112 cubiertas** (77 %), **7 parciales** (RN-032, RN-053, RN-070, RN-071, RN-105, RN-115, RN-137) y **27 sin tarea**:
- RN-008, RN-014, RN-036, RN-055, RN-057, RN-068, RN-078, RN-079, RN-081;
- RN-082, RN-083, RN-098, RN-103, RN-108, RN-110, RN-111, RN-113, RN-118;
- RN-120, RN-121, RN-124, RN-130, RN-133, RN-146, RN-147, RN-148, RN-149.

### Por historia de usuario: HU → componentes → tareas
| HU | Componentes (arquitectura) | Tareas | Estado | Hallazgo |
|---|---|---|---|---|
| HU-001 Registro | C1, C5, C6, C8, C9 | T-006, T-010, T-016, T-034, T-041, T-042 | ✅ | QA-C2-013 (SR-02) |
| HU-002 Baja | C1, C6, C7, C9 | T-011 (solo el KMS) | ❌ | QA-C2-003 |
| HU-003 Catálogo | C1, C6 | T-006, T-017, T-035 | 🟨 | QA-C2-004 |
| HU-004 Suscripción | C1, C6, C7 | T-007, T-014, T-024, T-029, T-035, T-038, T-042 | ✅ | — |
| HU-005 Pausa | C1, C6 | T-014, T-025, T-038 | ✅ | QA-C2-022 (SR-07) |
| HU-006 Modificar y cancelar | C1, C6 | — | ❌ | QA-C2-007 |
| HU-007 Cobro | C6, C7, C10 | T-019 a T-023, T-026, T-027, T-029, T-039, T-042 | 🟨 | QA-C2-001, QA-C2-009 |
| HU-008 Comprobante | C6, C10 | T-008, T-028 | 🟨 | QA-C2-001 (nota de crédito), QA-C2-013 (RUC) |
| HU-009 Reembolso | C1, C6, C7, C10 | T-008, T-027, T-039 | 🟨 | QA-C2-001 |
| HU-010 Lotes | C2, C6, C7 | T-029, T-030, T-031, T-040 | 🟨 | QA-C2-006 (sin pantalla) |
| HU-011 Cierre en ruta | C3, C6 | T-032, T-036, T-040, T-042 | 🟨 | QA-C2-005, QA-C2-008, K8, K9 |
| HU-012 Reclamo | C4, C6, C7 | T-033, T-037, T-038 | 🟨 | QA-C2-007 (RN-036) |
| HU-013 Precio | C2, C6 | — | ❌ ⛔ P14, P18 | QA-C2-004, QA-C2-007 |
| HU-014 Anulaciones | C2, C6 | — (los *triggers* de T-007 y T-009 cubren RN-032 contra los borrados automáticos) | ❌ ⛔ P18 | QA-C2-007 |
| HU-015 Feriados | C2, C6 | T-009, T-014, T-015 | ✅ | — |
| HU-016 Respuesta al reclamo | C2, C6, C10 | — | ❌ ⛔ asesor legal | QA-C2-007 |

**Resumen por HU:** 4 ✅ · 7 🟨 · 5 ❌ (3 de ellas bloqueadas por decisiones humanas).

### Por componente: componente → tareas
| Componente | Tareas | Estado |
|---|---|---|
| C1 · PWA módulo cliente | T-002, T-034, T-035, T-042, T-043 | 🟨 Faltan las pantallas de baja, modificación y elección de reembolso |
| C2 · PWA módulo operación | — | ❌ QA-C2-006 |
| C3 · PWA módulo reparto | T-036 | ✅ (K9 pendiente) |
| C4 · PWA Libro de Reclamaciones | T-037 | ✅ |
| C5 · API REST | T-001, T-003, T-004, T-012, T-013 | ✅ (QA-C2-014) |
| C6 · Módulos de dominio | T-014 a T-017, T-023 a T-028, T-030 a T-033, T-038 a T-040 | 🟨 `devoluciones` y `configuracion` sin tareas; `auditoria` parcial |
| C7 · Worker | T-018, T-029 (más los manejadores de T-026, T-028, T-031) | 🟨 Faltan `reembolso_automatico_72h`, `alerta_reclamo_dia_10` y `anonimizar_bajas` |
| C8 · Base de datos | T-005 a T-009, T-041, T-044 | ✅ |
| C9 · Cifrado + KMS | T-010, T-011 | ✅ (T-011 ⛔ G3) |
| C10 · Adaptadores externos | T-019 a T-022, T-028 (OSE simulado) | 🟨 El OSE y el correo reales esperan G4 y G6 |

### Trazabilidad hacia atrás
| Verificación | Resultado |
|---|---|
| Tareas que no trazan a una HU ni a infraestructura | Ninguna (44 de 44 trazan) |
| Endpoints de la arquitectura sin HU | Ninguno (21 de 21 trazan) |
| Endpoints de la arquitectura sin tarea | 10: `POST /importes-devolver/{id}/eleccion`, `POST /clientes/me/baja`, `PATCH /suscripciones/{id}`, `POST /suscripciones/{id}/cancelacion`, `GET /comprobantes`, los dos de precio, los dos de anulaciones y `POST /operacion/reclamos/{codigo}/respuesta`. Todos son de HU sin tareas o bloqueadas |
| Endpoints del backlog que no están en la arquitectura | `GET /operacion/feriados` y `GET /api/v1/salud` (QA-C2-014) |
| Tareas huérfanas | Ninguna |
| Dependencias con ciclos | Ninguna: cada tarea depende solo de tareas con número menor |

---

## Próximos pasos sugeridos
1. **Decisiones humanas:**
   - aprobar o rechazar las dependencias de QA-C2-002;
   - decidir RN-049 (QA-C2-009) y P18, que desbloquea HU-013, HU-014 y la carga del precio;
   - elegir la vía para cargar el precio inicial (QA-C2-004).
2. **BA y PO:**
   - crear la HU-017 de fincas y productos;
   - corregir los ejemplos de las reglas (QA-C2-012);
   - pasar RN-042 y RN-043 a MUST;
   - actualizar el alcance (QA-C2-011);
   - escribir las reglas de SR-02, SR-05 y SR-07.
3. **Architect:** agregar a la §4 los endpoints de asignación, `en_ruta`, facturación, fincas y productos, y `GET /operacion/feriados`.
4. **Backlog Builder:** incorporar las tareas propuestas para QA-C2-001, 003, 005 a 008, 014 a 016 y 018, y reordenar la ruta crítica.

---

## Puntaje de madurez del Spec Pack

### **3 / 5: sólido en la especificación, pero todavía no está listo para agentes de punta a punta**

| Dimensión | Puntaje | Por qué |
|---|---|---|
| Reglas de negocio | 4 / 5 | Las 150 reglas usan EARS, con ejemplo y contraejemplo, y sus valores son consistentes. Restan un punto cuatro ejemplos desactualizados (QA-C2-012), la contradicción de RN-049 (QA-C2-009), dos ambigüedades (QA-C2-023) y las decisiones abiertas P14, P18, P23, P24 y K8 a K10. |
| Historias de usuario | 4 / 5 | Las 16 HU tienen criterios con valores concretos y bordes, y no se contradicen entre sí. Faltan una HU de fincas y productos (QA-C2-004) y siete citas de reglas (QA-C2-019). |
| Arquitectura | 4 / 5 | El stack cumple la constitución, hay 9 ADR aceptados y el modelo de datos y la seguridad están bien resueltos. Faltan endpoints de asignación, `en_ruta`, facturación y catálogo (QA-C2-005, 013 y 004). |
| Backlog | 2 / 5 | Las tareas son atómicas, verificables y sin ciclos, pero el 18 % de las reglas Must no tiene tarea. Además, el flujo de devoluciones queda abierto, faltan la baja de cuenta y el despliegue, y varias tareas necesitan dependencias sin aprobar (QA-C2-001 a 003, 007 y 016). |
| Consistencia entre documentos | 3 / 5 | Los números cuadran (26 de 26), pero el alcance está desactualizado y las versiones y fechas entregadas no coinciden con las del proyecto (QA-C2-011 y 017). |

**Qué falta para llegar a 4/5:** cerrar los 4 críticos. Eso significa agregar las tareas de devoluciones y de la baja de cuenta, aprobar o reemplazar las dependencias de desarrollo y resolver la carga del precio y del catálogo. También hay que corregir QA-C2-009, 011 y 012.

**Qué falta para llegar a 5/5:** cerrar además los hallazgos ALTA, en especial el módulo de operación (C2), la asignación y el estado `en_ruta`, OpenAPI completo, RN-118 y el despliegue. También deben tomarse las decisiones humanas P14, P18, P23, K8, K9 y K10, y validarse lo que está pendiente del contador y del asesor legal.

---

## Estado de las correcciones (14/11/2026)
Por pedido del fundador, se corrigieron los 4 hallazgos CRÍTICA y los 9 ALTA. Se tomaron estas decisiones humanas: aprobar las dependencias de desarrollo, adelantar la HU-013, fijar la doble confirmación con contraseña y código por correo (P18) y rechazar con un 409 explícito un correo ya registrado (SR-02).

**Documentos actualizados:**
- `03-reglas-de-negocio.md` v0.8.0: RN-151 a RN-154 y ejemplos corregidos;
- `06-user-stories-refinadas.md` v0.3.0: HU-013 y HU-014 desbloqueadas, HU-017 nueva y SR-02 resuelto;
- `07-architecture.md` v1.1.0: §3.2, §4.13, §6.2, §6.3 y §6.6;
- `08-backlog.md` v0.3.0: T-045 a T-071.

| Hallazgo | Severidad | Estado | Cómo se corrigió |
|---|---|---|---|
| QA-C2-001 | CRÍTICA | ✅ Resuelto | Tareas nuevas: servicio de devoluciones con reintento (T-045), elección del cliente (T-046), reembolso automático a las 72 h (T-047), nota de crédito (T-048) y devoluciones de entregas fallidas, canceladas y suspendidas (T-049, con RN-053, RN-079 y RN-108). La pantalla de elección es T-065 |
| QA-C2-002 | CRÍTICA | ✅ Resuelto | El fundador aprobó `eslint`, `@vitest/coverage-v8`, `jsdom`, `@testing-library/react` y `c8` (§6.3 de la arquitectura). T-004 y T-034 a T-037 las usan |
| QA-C2-003 | CRÍTICA | ✅ Resuelto | Baja de cuenta (T-050), anonimización con borrado criptográfico y sin `DELETE` (T-051) y pantalla (T-066). En producción, la anonimización necesita el KMS real (T-011, compuerta G3) |
| QA-C2-004 | CRÍTICA | ✅ Resuelto (⏳ falta aprobar HU-017) | La **HU-017** nueva, con sus endpoints de fincas y productos (§4.13) y T-052. El precio inicial se carga con la HU-013 adelantada, usando RN-153 y T-054, más la doble confirmación de RN-151 y RN-152 (T-053). ⚠️ En producción, el código por correo depende del proveedor de correo (G6) |
| QA-C2-005 | ALTA | ✅ Resuelto | Endpoints de asignación y de salida a ruta, con el algoritmo de asignación (§4.13). Tareas T-056 y T-057; la salida también funciona sin conexión |
| QA-C2-006 | ALTA | ✅ Resuelto | Pantallas del módulo de operación (T-063 y T-064), más los listados de lotes, fincas y reclamos (§4.13) |
| QA-C2-007 | ALTA | 🟨 Parcial | Tienen tarea: HU-006 (T-059, que además aplica los cambios en el corte), HU-013 (T-054), HU-014 (T-055) y la alerta del día 10 (T-060). **La HU-016 sigue bloqueada** por la validación del asesor legal y por P24 |
| QA-C2-008 | ALTA | ✅ Resuelto | T-058 cubre el aviso a los repartidores en 60 s (RN-133) y la disculpa a los clientes en 3 600 s (RN-078). T-049 cubre el importe de las entregas suspendidas (RN-079) |
| QA-C2-012 | ALTA | ✅ Resuelto | Se reescribieron los ejemplos de RN-054, RN-077, RN-078 y RN-093 con la secuencia completa: revisión, invalidación a los 900 s y efecto |
| QA-C2-013 | ALTA | ✅ Resuelto | `PATCH /clientes/me/facturacion` y `GET /comprobantes` (T-061). SR-02: el fundador eligió el `409` explícito (RN-154), en lugar de la respuesta neutra recomendada, y aceptó el riesgo de revelar qué correos tienen cuenta |
| QA-C2-014 | ALTA | ✅ Resuelto | T-071 completa el contrato OpenAPI, T-004 falla si una ruta no está en el contrato, y `GET /operacion/feriados` ya está en la arquitectura |
| QA-C2-015 | ALTA | ✅ Resuelto | Tabla `proveedor_habilitado`, que carga un humano, y verificación obligatoria en todo adaptador real (T-062, §6.2). HU-001 y HU-007 citan RN-118. *Nota: la versión del proyecto de `06-user-stories-refinadas.md` ya trataba RN-118 como requisito previo a producción, en su Definición de Terminado; la copia que se pegó para la auditoría no tenía esa sección* |
| QA-C2-016 | ALTA | 🟨 Parcial | El monitoreo del worker y del outbox no tiene compuertas (T-067). El despliegue continuo (T-068), el TLS 1.3 (T-069) y el respaldo con restauración probada (T-070) están definidos, pero esperan que se elija el hosting (G3) |

**Cobertura de reglas Must después de las correcciones:** hay 150 reglas Must (146, más RN-151 a RN-154):
- **144 cubiertas (96 %)**.
- **3 parciales**, que esperan una decisión:
  - RN-103 (P14);
  - RN-115 (el texto legal lo redacta un humano);
  - RN-137 (P23).
- **3 sin tarea:** RN-146 a RN-148, pendientes del asesor legal.

**Puntaje de madurez después de las correcciones: 4 / 5.** Ya no hay hallazgos CRÍTICA, y el backlog sube de 2 a 4 sobre 5. Para llegar a 5 faltan cuatro cosas:
- aprobar la HU-017;
- cerrar los hallazgos MEDIA: QA-C2-009 (RN-049), QA-C2-010 (RN-042 y RN-043 a MUST), QA-C2-011 (alcance), QA-C2-017 (versiones) y QA-C2-018 (máquinas de estado);
- tomar las decisiones P14, P23, P24 y K8 a K10;
- obtener las validaciones del contador (RN-125 y RN-126) y del asesor legal (RN-146 a RN-149).
