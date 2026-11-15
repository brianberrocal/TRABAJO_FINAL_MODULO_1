# Backlog técnico — Muuvi (iteración 1)
**Versión:** 0.4.1
**Fecha:** 14/11/2026
**Autor:** Agente Backlog Builder (BMAD)
**Historial:** 0.1.0 (14/11/2026) borrador · 0.2.0 (14/11/2026) liberación de G0 y G1 · 0.3.0 (14/11/2026) 27 tareas nuevas (T-045 a T-071) y ajustes a T-003, T-004, T-016, T-034 a T-037 y T-042 para resolver los hallazgos CRÍTICA y ALTA de `09-auditoria-trazabilidad.md` · 0.4.0 (14/11/2026) 19 tareas nuevas (T-072 a T-090) para subsanar las observaciones de `10-validation-log.md`, nueva compuerta G7 y ajustes a T-025, T-030, T-032, T-036, T-046 y T-055 · 0.4.1 (14/11/2026) el fundador aprueba las reglas propuestas: G7 liberada; T-080 pasa a esperar la compuerta nueva G8 (contador)
**Fuentes:** [`07-architecture.md`](07-architecture.md) v1.2.0 · [`06-user-stories-refinadas.md`](06-user-stories-refinadas.md) v0.4.0 · [`02-alcance.md`](02-alcance.md) · [`03-reglas-de-negocio.md`](03-reglas-de-negocio.md) v0.9.0 · [`09-ui-spec.md`](09-ui-spec.md) v0.2.0 · [`09-auditoria-trazabilidad.md`](09-auditoria-trazabilidad.md) · [`10-validation-log.md`](10-validation-log.md) · [`00-constitucion.md`](00-constitucion.md)

> ✅ **14/11/2026:** el fundador aprobó los ADR-005 a ADR-009 y las dependencias, y `07-architecture.md` pasó a la versión 1.0.0. Las compuertas **G0** y **G1** están liberadas.
>
> ✅ **14/11/2026 (v0.3.0):** se incorporan las correcciones de la auditoría de trazabilidad (`09-auditoria-trazabilidad.md`), con `07-architecture.md` v1.1.0, `03-reglas-de-negocio.md` v0.8.0 y `06-user-stories-refinadas.md` v0.3.0. El fundador aprobó las dependencias de desarrollo de QA-C2-002, adelantó la HU-013, definió la doble confirmación (P18) y resolvió SR-02. Siguen cerradas G2 a G6.
>
> ✅ **14/11/2026 (v0.4.0):** se incorporan las subsanaciones de la validación del prototipo (`10-validation-log.md`). El fundador decidió la hoja de ruta impresa (K9), la recuperación de contraseña, la aprobación de Yape y Plin en cada ciclo y las suscripciones múltiples. Las tareas que implementan reglas ⏳ propuestas esperaban la compuerta nueva **G7**.
>
> ✅ **14/11/2026 (v0.4.1):** el fundador aprobó las reglas propuestas (RN-156 a RN-159, RN-161 a RN-163 y RN-166 a RN-185) y, con RN-185, resolvió K8. **G7 queda liberada.** T-080 sigue bloqueada por **G8**: la validación del contador de RN-168 a RN-172.

## Resumen

| Indicador | Valor |
|---|---|
| **Total de tareas** | 90: T-001 a T-044 de la v0.2.0, T-045 a T-071 (auditoría de trazabilidad) y T-072 a T-090 (validación del prototipo) |
| **Esfuerzo máximo estimado** | 116.5 h (S = 0.5 h, M = 1 h, L = 2 h, como techo) |
| **Tareas de testing dedicadas** | 5 (T-038, T-039, T-040, T-041, T-042), más los tests propios de cada tarea |
| **Bloqueadas por una compuerta humana** | 8: T-004 (G2); T-011, T-068, T-069 y T-070 (G3); T-020 y T-021 (G5); T-080 (G8). Ver [Compuertas humanas](#compuertas-humanas) |

**Por prioridad**

| Must | Should | Could |
|---|---|---|
| 88 | 2 | 0 |

**Por tamaño**

| S (< 30 min) | M (30 min – 1 h) | L (1 – 2 h) |
|---|---|---|
| 9 | 50 | 31 |

**Por componente** (componente principal de cada tarea)

| Componente | Tareas | Cantidad |
|---|---|---|
| C1 · PWA módulo cliente | T-002, T-034, T-035, T-042, T-043, T-065, T-066, T-086 | 8 |
| C2 · PWA módulo operación | T-063, T-064, T-087 | 3 |
| C3 · PWA módulo reparto | T-036, T-088 | 2 |
| C4 · PWA Libro de Reclamaciones | T-037 | 1 |
| C5 · API REST | T-001, T-003, T-004, T-012, T-013, T-068, T-069, T-071, T-089 | 9 |
| C6 · Módulos de dominio | T-014, T-015, T-016, T-017, T-023, T-024, T-025, T-026, T-027, T-028, T-030, T-031, T-032, T-033, T-038, T-039, T-040, T-045, T-046, T-048, T-049, T-050, T-052, T-053, T-054, T-055, T-056, T-057, T-058, T-059, T-061, T-072 a T-084, T-090 | 45 |
| C7 · Worker | T-018, T-029, T-047, T-051, T-060, T-067 | 6 |
| C8 · Base de datos | T-005, T-006, T-007, T-008, T-009, T-041, T-044, T-070 | 8 |
| C9 · Cifrado + KMS | T-010, T-011 | 2 |
| C10 · Adaptadores externos | T-019, T-020, T-021, T-022, T-062, T-085 | 6 |

**Ruta crítica hasta completar las Must** (14 tareas, 22.5 h como máximo, contando solo dependencias técnicas; recalculada por script en la v0.4.0):

T-001 → T-005 → T-006 → T-009 → T-018 → T-022 → T-026 → T-029 → T-030 → T-031 → T-045 → T-059 → T-072 → T-086

1. **T-001** (M) Crear el esqueleto del backend con Express y la estructura de módulos
2. **T-005** (S) Conectar con PostgreSQL y crear el ejecutor de migraciones SQL
3. **T-006** (M) Migración 001: cuentas, consentimiento y catálogo
4. **T-009** (M) Migración 004: reclamos, calendario, configuración, auditoría, cola y outbox
5. **T-018** (L) Implementar la cola de tareas y el outbox con `FOR UPDATE SKIP LOCKED`
6. **T-022** (M) Implementar el módulo de notificaciones con ventana horaria y adaptador simulado
7. **T-026** (L) Implementar el servicio de cobro del ciclo con reintento y verificación
8. **T-029** (L) Implementar la tarea `corte_dia_reparto`
9. **T-030** (L) Implementar `POST /operacion/lotes` con umbral de temperatura y escasez
10. **T-031** (L) Implementar la corrección de temperatura, la reversión y el cierre de la revisión a los 900 s
11. **T-045** (L) Implementar el servicio de devoluciones y el reintento de reembolsos
12. **T-059** (L) Implementar la modificación y la cancelación de la suscripción
13. **T-072** (L) Implementar las simulaciones de suscripción, pausa, reactivación, modificación y cancelación
14. **T-086** (L) Actualizar las pantallas del cliente: varias suscripciones, vistas previas, dirección y recuperación

> Desde la v0.4.0, la ruta crítica pasa por **T-072** (simulaciones). Con G7 liberada el 14/11/2026, ninguna tarea de la ruta crítica está bloqueada por una compuerta. T-042 también depende de T-004, que espera la protección de rama (G2). G3 bloquea el despliegue (T-068 a T-070) y el KMS real (T-011); G5, los cobros reales con Culqi, Yape y Plin; y G6, el correo real, del que depende en producción el código de la doble confirmación. Las demás tareas Must se pueden repartir en paralelo alrededor de esta ruta.

**Orden de ejecución.** Un script verificó que las dependencias no forman ciclos (DAG). El número de la tarea no es un orden válido, porque algunas tareas antiguas dependen de tareas nuevas. Este es un orden topológico válido (v0.4.0):

T-001 → T-002 → T-003 → T-004 → T-005 → T-006 → T-007 → T-008 → T-009 → T-010 → T-011 → T-012 → T-013 → T-014 → T-015 → T-016 → T-017 → T-018 → T-019 → T-020 → T-021 → T-022 → T-023 → T-024 → T-025 → T-026 → T-027 → T-028 → T-029 → T-030 → T-031 → T-032 → T-033 → T-034 → T-035 → T-036 → T-037 → T-038 → T-039 → T-040 → T-041 → T-043 → T-044 → T-045 → T-046 → T-047 → T-048 → T-049 → T-050 → T-051 → T-052 → T-053 → T-054 → T-055 → T-056 → T-057 → T-042 → T-058 → T-059 → T-060 → T-061 → T-062 → T-063 → T-064 → T-065 → T-066 → T-067 → T-068 → T-069 → T-070 → T-071 → T-072 → T-073 → T-074 → T-075 → T-076 → T-077 → T-078 → T-079 → T-080 → T-081 → T-082 → T-083 → T-084 → T-085 → T-086 → T-087 → T-088 → T-089 → T-090

---

## Compuertas humanas
Un agente **no puede** liberar estas compuertas (PRINCIPIO-008). Mientras una compuerta esté cerrada, las tareas que la citan quedan bloqueadas, aunque sus dependencias técnicas estén hechas.

| Compuerta | Qué decide o hace un humano | Tareas que bloquea |
|---|---|---|
| **G0** | ~~Aprobar los ADR-005 a ADR-009 y pasar `07-architecture.md` a la versión 1.0~~ ✅ Liberada el 14/11/2026 | — |
| **G1** | ~~Aprobar las dependencias de la §6.3, más `vite` y `@redocly/cli`~~ ✅ Liberada el 14/11/2026. Ese mismo día se aprobaron también `eslint`, `@vitest/coverage-v8`, `jsdom`, `@testing-library/react` y `c8` (QA-C2-002) | — |
| **G2** | Configurar la protección de la rama `main` con 1 aprobación obligatoria (ADR-003) | T-004 (su criterio la verifica) |
| **G3** | Elegir el hosting y el KMS, y crear las claves maestras | T-011, T-068, T-069, T-070 |
| **G4** | Elegir el OSE/PSE y firmar su DPA | Real: iteración 2. Los adaptadores simulados de T-028, T-048 y T-055 no se bloquean |
| **G5** | Abrir las cuentas sandbox de Culqi, Yape y Plin y **cargar sus credenciales en el gestor de secretos**. El agente nunca lee ni escribe secretos (PRINCIPIO-008.6) | T-020, T-021 |
| **G6** | Elegir el proveedor de correo y firmar su DPA, y cargarlo en `proveedor_habilitado` (T-062) | Real: iteración 2. El adaptador simulado de T-022 no se bloquea. ⚠️ En producción, el código de la doble confirmación (T-053) y, por lo tanto, la carga del precio (T-054), además de la recuperación de contraseña (T-075) y las plantillas de correo (T-085), dependen de G6 |
| **G7** | ~~Aprobar las reglas ⏳ propuestas en `03-reglas-de-negocio.md` v0.9.0~~ ✅ Liberada el 14/11/2026: el fundador las aprobó y resolvió K8 con RN-185 | — |
| **G8** | El contador valida RN-168 a RN-172 (efectos de las anulaciones), junto con RN-125 y RN-126, que ya estaban pendientes | T-080 |

**Decisiones abiertas que dejan partes de una tarea como `test.todo`** (el test queda escrito y marcado como pendiente; no se inventa el comportamiento):
| Decisión | Tarea afectada | Qué queda pendiente |
|---|---|---|
| P23 | T-026 | Intervalo y plazo de las consultas repetidas de un pago `en_verificacion` |
| ~~K8~~ | T-032 → T-078 | ✅ Resuelto: RN-185 aprobada el 14/11/2026 |
| ~~K9~~ | T-036 → T-079 y T-088 | ✅ Resuelto: hoja de ruta impresa (RN-155) |
| ~~SR-07~~ | T-025 → T-084 | ✅ RN-180 aprobada |
| SR-15 | T-033 | Reclamo sin correo |
| SR-14 | T-033 | Plazo de un reclamo registrado en sábado o domingo |
| SR-08 | T-059 | Modificar o cancelar una suscripción ya cancelada |
| ~~SR-10~~ | T-046 → T-084 | ✅ RN-181 aprobada |
| P14 | T-054 | Antelación del aviso de cambio de precio a los suscriptores (RN-103) |
| G4 | T-055 y T-080 | Comunicación de baja o nota de crédito ante el OSE al anular un comprobante (RN-169) |
| HU-016 y P24 | T-085 | Plantilla CO-11 de la respuesta al reclamo |

**Ajustes a tareas existentes (v0.4.0).** Estos cambios no reescriben su criterio; los aplica la tarea nueva que se indica:
- **T-030:** la escasez deja de aplicarse al registrar el lote; pasa al cierre de la recepción (T-077, RN-157).
- **T-032:** acepta lecturas altas y aplica la propuesta de K8 (T-078).
- **T-036:** el `test.todo` de K9 se reemplaza por la parada de la hoja impresa (T-088).
- **T-024:** exige `producto_id` y un medio de pago activo (T-074).
- **T-055:** la anulación gana sus efectos (T-080).
- **T-025 y T-046:** sus `test.todo` SR-07 y SR-10 los cierra T-084.

## Reglas para el agente que ejecute estas tareas
1. **Una tarea, una rama, un PR.** El nombre de la rama es `tarea/T-NNN-titulo-corto` y el PR cita la tarea y sus reglas (`RN-NNN`).
2. **Código, tablas, endpoints y tests en español** (PRINCIPIO-001). Los tests de reglas llevan el ID en el nombre: `test_RN045_cobrar_en_el_corte`.
3. **Sin textos en el código:** todo texto visible va en `frontend/src/locales/es-PE.json`, y la API devuelve claves de mensaje (D9).
4. **Sin secretos:** el código lee variables de entorno por su nombre, documentado en `docs/configuracion-entorno.md`. El agente **no crea, lee ni edita** archivos `.env` ni credenciales.
5. **Migraciones solo aditivas:** nada de `DROP`, `TRUNCATE`, ni `DELETE` o `UPDATE` sin `WHERE`. No se escribe migración de reversión destructiva; revertir lo decide un humano (PRINCIPIO-008.7).
6. **Ningún dato real de clientes** y **ningún precio de producción** en migraciones ni semillas: `precio_litro` lo carga un humano (PRINCIPIO-008.9). Los tests usan `14.50` solo como dato de prueba.
7. **Pruebas del backend con `node:test`** (incluido en Node.js 20), y del frontend con `vitest`. Solo se usan las dependencias aprobadas en la §6.3 de la arquitectura; cualquier otra necesita aprobación humana (PRINCIPIO-008.1).
8. **No se tocan, borran ni debilitan tests existentes** para que algo pase (PRINCIPIO-008.8).

---

# MUST

## C5 · API REST — repositorio y contrato

### T-001: Crear el esqueleto del backend con Express y la estructura de módulos
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: M
Dependencias: Ninguna

Descripción:
Crear `backend/` con `package.json` (Node.js 20, `"type": "module"`, dependencia única `express`), `src/app.js` (aplicación Express), `src/servidor.js` (arranque HTTP) y `src/worker.js` (arranque vacío del worker). Crear las carpetas `src/modulos/{cuentas,catalogo,suscripciones,operacion,pagos,comprobantes,devoluciones,reclamos,calendario,configuracion,notificaciones,auditoria,cifrado,permisos}/` con un `index.js` que exporte un objeto vacío, y el endpoint `GET /api/v1/salud`. Agregar el script `npm test` con `node --test`.

Criterio de completitud:
`npm test` pasa con un test que hace `GET /api/v1/salud` y recibe `200` con `{"estado":"ok"}`, y existen las 14 carpetas de módulos.

---

### T-003: Escribir el contrato OpenAPI de los endpoints del PMV
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: L
Dependencias: Ninguna

Descripción:
Crear `docs/api/openapi.yaml` (OpenAPI 3.1) con los 10 endpoints detallados en la §4 de la arquitectura (4.2 a 4.11), más `POST /sesiones` y `GET /catalogo`. Cada uno lleva sus esquemas de request y response con `additionalProperties: false`, montos como `string` con el patrón `^\d+\.\d{2}$`, los códigos de estado de su tabla y el esquema común `Error` (`codigo`, `clave_mensaje`, `campo`). Los casos marcados con ❓ en la arquitectura llevan `x-pendiente: SR-NN` y no se inventa su respuesta.

Criterio de completitud:
El archivo contiene los 12 `paths`, cada request declara `additionalProperties: false`, y cada código de estado listado en la §4 aparece en su endpoint.

---

## C1 · PWA — esqueleto

### T-002: Crear el esqueleto de la PWA React 18 con traducciones y rutas por rol
HU relacionada: Infraestructura
Componente: C1 · PWA módulo cliente
Prioridad: Must
Tamaño: M
Dependencias: Ninguna

Descripción:
Crear `frontend/` con React 18, `vite`, `react-router`, `react-i18next` y `vitest`. Agregar `public/manifest.webmanifest`, un Service Worker `src/sw.js` que solo cachea recursos estáticos, `src/locales/es-PE.json` y cuatro rutas vacías: `/` (cliente), `/operacion`, `/reparto` y `/libro-de-reclamaciones`. Las rutas `/operacion` y `/reparto` se cargan de forma diferida (`React.lazy`).

Criterio de completitud:
`npm run build` termina sin errores, el resultado incluye el manifiesto y el Service Worker, y el texto de la ruta `/` sale de `es-PE.json` (un test lo verifica).

---

### T-004: Configurar el CI con lint, tests, umbrales de cobertura y validación de OpenAPI
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: M
Dependencias: T-001, T-002, T-003 · ⛔ G2

Descripción:
Crear `.github/workflows/ci.yml`, que en cada PR haga lo siguiente:
- ejecute `eslint`;
- ejecute los tests del backend con `c8` y los del frontend con `@vitest/coverage-v8`;
- valide `docs/api/openapi.yaml` con `@redocly/cli lint`;
- falle si la cobertura es menor que el 80 % en `backend/src/modulos/**/reglas/**` o menor que el 60 % en total (PRINCIPIO-009);
- falle si alguna ruta registrada en Express no existe en `openapi.yaml` (principio D11, QA-C2-014), con un script propio que lea el *router* y el YAML.

La protección de rama es G2 y la configura un humano.

Criterio de completitud:
Un PR de prueba que baja la cobertura de reglas al 79 % hace fallar el CI; uno que rompe el YAML de OpenAPI también, y uno que agrega una ruta de Express que no está en el contrato, también.

---

## C8 · Base de datos

### T-005: Conectar con PostgreSQL y crear el ejecutor de migraciones SQL
HU relacionada: Infraestructura
Componente: C8 · Base de datos
Prioridad: Must
Tamaño: S
Dependencias: T-001

Descripción:
Crear `backend/src/bd/conexion.js` (pool de `pg` configurado con `DATABASE_URL` y `ssl` obligatorio fuera de desarrollo) y `backend/src/bd/migrar.js`, que aplica en orden los archivos `backend/migraciones/NNN_*.sql` y registra cada uno en la tabla `migracion_aplicada`. Documentar `DATABASE_URL` en `docs/configuracion-entorno.md` sin ningún valor real.

Criterio de completitud:
Ejecutar `npm run migrar` dos veces seguidas sobre una base local vacía aplica cada migración una sola vez, y la segunda ejecución no hace cambios.

---

### T-006: Migración 001: cuentas, consentimiento y catálogo
HU relacionada: HU-001, HU-003
Componente: C8 · Base de datos
Prioridad: Must
Tamaño: M
Dependencias: T-005

Descripción:
Crear `backend/migraciones/001_cuentas_catalogo.sql` con los tipos `ENUM` y las tablas `cliente`, `direccion`, `consentimiento`, `usuario_interno`, `finca` y `producto`, exactamente con las columnas y restricciones de la §3.2. Todo dato personal es `BYTEA` con el sufijo `_cifrado`, el correo lleva `correo_hmac BYTEA UNIQUE`, y `producto` tiene `CHECK (NOT publicado OR registro_digesa IS NOT NULL)` (RN-009).

Criterio de completitud:
La migración se aplica sin errores, y un test comprueba que insertar un producto con `publicado = true` y `registro_digesa` nulo falla.

---

### T-007: Migración 002: suscripciones, ciclos, entregas, lotes y temperatura
HU relacionada: HU-004, HU-005, HU-010, HU-011
Componente: C8 · Base de datos
Prioridad: Must
Tamaño: L
Dependencias: T-006

Descripción:
Crear `backend/migraciones/002_operacion.sql` con las tablas `suscripcion`, `pausa`, `cambio_suscripcion`, `ciclo` (`UNIQUE(suscripcion_id, fecha_entrega)`), `entrega`, `cierre_entrega` (clave `UUID`), `movimiento_botellas`, `lote` y `lectura_temperatura` de la §3.2. Agregar un *trigger* que rechace todo `UPDATE` y `DELETE` sobre `lectura_temperatura` (RN-088), y `CHECK (litros >= 1)` en `suscripcion` (RN-017).

Criterio de completitud:
Un test confirma que `UPDATE lectura_temperatura SET valor = 3.0 WHERE id = 1` falla por el trigger, y que una suscripción con `litros = 0` no se puede insertar.

---

### T-008: Migración 003: pagos, comprobantes, devoluciones y saldo
HU relacionada: HU-007, HU-008, HU-009
Componente: C8 · Base de datos
Prioridad: Must
Tamaño: M
Dependencias: T-007

Descripción:
Crear `backend/migraciones/003_pagos.sql` con `medio_pago` (sin ninguna columna de PAN, CVV ni vencimiento), `pago`, `evento_pasarela` (`UNIQUE(pasarela, evento_id_externo)`), `comprobante`, `importe_devolver`, `reembolso` y `movimiento_saldo`. Incluir el índice único parcial `CREATE UNIQUE INDEX pago_un_confirmado_por_ciclo ON pago(ciclo_id) WHERE estado = 'confirmado'` (RN-073) y `CHECK (importe >= 0)` en todos los montos, salvo `movimiento_saldo.importe`.

Criterio de completitud:
Un test confirma que insertar un segundo pago `confirmado` para el mismo `ciclo_id` falla, y que insertar dos veces el mismo `evento_id_externo` de una pasarela también.

---

### T-009: Migración 004: reclamos, calendario, configuración, auditoría, cola y outbox
HU relacionada: HU-012, HU-015
Componente: C8 · Base de datos
Prioridad: Must
Tamaño: M
Dependencias: T-006

Descripción:
Crear `backend/migraciones/004_sistema.sql` con `reclamo` (más una secuencia por año para el código `LR-AAAA-NNNNNN`), `respuesta_reclamo`, `feriado`, `configuracion`, `auditoria` (un *trigger* rechaza `UPDATE` y `DELETE`), `tarea_programada` (`clave_unica UNIQUE`) y `outbox` (con `no_antes_de TIMESTAMPTZ`). **No insertar ningún valor** en `configuracion` ni en `feriado`: los carga un humano (PRINCIPIO-008.9, HU-015).

Criterio de completitud:
La migración se aplica, `SELECT count(*) FROM configuracion` y `SELECT count(*) FROM feriado` devuelven 0, y un `UPDATE` sobre `auditoria` falla.

---

## C9 · Módulo `cifrado` + KMS

### T-010: Implementar el cifrado de campo AES-256-GCM y el índice ciego HMAC
HU relacionada: HU-001
Componente: C9 · Módulo cifrado + KMS
Prioridad: Must
Tamaño: L
Dependencias: T-001

Descripción:
Crear `backend/src/modulos/cifrado/` con `cifrarCampo(texto, claveDatos)`, `descifrarCampo(bytes, claveDatos)` y `indiceCiego(texto)`. Usar solo `node:crypto`: AES-256-GCM, un IV aleatorio de 12 bytes por valor, el formato `{versión | IV | texto cifrado | tag}` y HMAC-SHA-256 sobre el correo normalizado (minúsculas y sin espacios). Las claves llegan por una interfaz `ProveedorClaves` (`obtenerClaveDatos(clienteId)`, `obtenerClaveHmac()`, `destruirClaveDatos(clienteId)`). Incluir `ProveedorClavesMemoria`, solo para tests, que genera claves al vuelo y nunca las escribe en disco.

Criterio de completitud:
Pasan los tests de: descifrar lo cifrado devuelve el original, cifrar dos veces el mismo texto da bytes distintos, alterar un byte hace fallar el descifrado, el HMAC de `Ana@Example.com ` es igual al de `ana@example.com`, y tras `destruirClaveDatos` el descifrado falla.

---

### T-011: Conectar el proveedor de claves con el KMS elegido
HU relacionada: HU-001, HU-002
Componente: C9 · Módulo cifrado + KMS
Prioridad: Must
Tamaño: M
Dependencias: T-010 · ⛔ G3

Descripción:
Crear `backend/src/modulos/cifrado/proveedor-kms.js`, que implementa `ProveedorClaves` con el SDK del KMS elegido en G3. Usar cifrado de sobre: la clave de datos de cada cliente se guarda cifrada por la clave maestra y se cachea en memoria con un tiempo de vida de 300 s como máximo (ADR-006). Los identificadores de las claves se leen de variables de entorno documentadas en `docs/configuracion-entorno.md`.

Criterio de completitud:
El conjunto de tests de T-010 pasa también con `ProveedorClavesKms` contra el KMS del entorno de pruebas, y ninguna clave en claro aparece en los logs del test.

---

## C5 · API REST — transversal

### T-012: Crear los middlewares de DTO estrictos, formato de error y logs sin datos personales
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: M
Dependencias: T-001

Descripción:
Crear `backend/src/http/validar.js` (valida el cuerpo contra un JSON Schema con `additionalProperties: false` y responde `400`), `backend/src/http/errores.js` (convierte las excepciones de dominio `ErrorRegla(codigo, claveMensaje, campo)` en `{ "error": { … } }` con 422, 409 o 404) y `backend/src/registro/log.js`. El logger rechaza los campos llamados `nombre`, `correo`, `telefono`, `direccion`, `dni` o `ruc`, y los valores con forma de correo o de celular peruano (`9\d{8}`) (RN-007).

Criterio de completitud:
Pasan los tests de: un cuerpo con una propiedad extra recibe `400`, un `ErrorRegla('RN-017', …)` se convierte en `422` con `codigo: "RN-017"`, y registrar `{ correo: "a@b.pe" }` deja en el log `[redactado]`.

---

### T-013: Implementar sesiones JWT, autorización por rol y límite de peticiones
HU relacionada: Infraestructura (todas las HU)
Componente: C5 · API REST
Prioridad: Must
Tamaño: L
Dependencias: T-006, T-010, T-012

Descripción:
Crear `backend/src/modulos/permisos/` con `POST /sesiones` (busca por `correo_hmac` en `cliente` o `usuario_interno` y verifica `argon2id`), `POST /sesiones/renovar` (*cookie* `HttpOnly`, `Secure`, `SameSite=Strict`) y `DELETE /sesiones`. El JWT de acceso dura 900 s y lleva `sub`, `rol` y `exp`. Agregar el middleware `exigirRol(...roles)` según la matriz de permisos de `03-reglas-de-negocio.md` (RN-099), y `limitar({ porMinuto, porHora, clave })` con un almacén en memoria detrás de una interfaz.

Criterio de completitud:
Pasan los tests de: una contraseña incorrecta recibe `401`, un `repartidor` que llama a una ruta con `exigirRol('operador')` recibe `403`, un token con más de 900 s recibe `401`, y la sexta petición en un minuto a una ruta con `porMinuto: 5` recibe `429`.

---

## C6 · Módulos de dominio — `calendario`

### T-014: Implementar las reglas de calendario y hora de Lima
HU relacionada: HU-004, HU-005, HU-011, HU-012, HU-015
Componente: C6 · Módulos de dominio (`calendario`)
Prioridad: Must
Tamaño: M
Dependencias: T-001

Descripción:
Crear `backend/src/modulos/calendario/reglas/` con funciones puras que reciben la lista de feriados como parámetro y usan `Intl` con `America/Lima`, sin librerías. Las funciones son `instanteCorte(fechaEntrega)` (18:00:00 del día previo, en UTC), `primeraEntrega(diaEntrega, ahora, feriados)` (corte exclusivo, RN-084), `trasladarPorFeriado(fecha, feriados)` (al siguiente martes, jueves o sábado no feriado, RN-123 y RN-139), `sumarDiasHabiles(fecha, n, feriados)` (RN-101) y `edadEnLima(fechaNacimiento, ahora)` (RN-001).

Criterio de completitud:
Pasan los tests con los ejemplos de las HU: suscripción del sábado creada el 13/11/2026 a las 17:59:59 → 14/11/2026; a las 18:00:00 → 21/11/2026. El corte del 13/11/2026 es `2026-11-13T23:00:00Z`. El 08/12/2026 feriado pasa al 10/12/2026. 15 días hábiles desde el 24/11/2026 → 16/12/2026. Nacido el 12/11/2008 tiene 18 años el 12/11/2026.

---

### T-015: Implementar la carga de feriados por el operador
HU relacionada: HU-015
Componente: C6 · Módulos de dominio (`calendario`)
Prioridad: Must
Tamaño: S
Dependencias: T-009, T-013, T-014

Descripción:
Crear `POST /operacion/feriados` en `backend/src/modulos/calendario/rutas.js`, con el cuerpo `{ "fecha", "descripcion", "norma" }` y acceso solo para `operador` y `administrador_titular`. El endpoint guarda en `feriado` con `cargado_por` y registra la acción en `auditoria`. Agregar `GET /operacion/feriados?anio=2026` y un repositorio `feriadosEntre(desde, hasta)`, que es lo que usan las demás tareas.

Criterio de completitud:
Pasan los tests de: el operador carga el 08/12/2026 y recibe `201`, un `repartidor` recibe `403`, cargar dos veces la misma fecha recibe `409`, y queda una fila en `auditoria`.

---

## C6 · Módulos de dominio — `cuentas` y `catalogo`

### T-016: Implementar `POST /clientes` (registro con consentimiento y cifrado)
HU relacionada: HU-001
Componente: C6 · Módulos de dominio (`cuentas`)
Prioridad: Must
Tamaño: L
Dependencias: T-003, T-006, T-010, T-012, T-014

Descripción:
Crear `backend/src/modulos/cuentas/` con la ruta y el servicio de registro según la §4.2: valida la edad ≥ 18 en Lima (RN-001), la fecha de nacimiento obligatoria (RN-002), `consentimientos.principal === true` (RN-114) y el distrito atendido (RN-005). Luego cifra cada dato personal con el módulo `cifrado`, calcula `correo_hmac` y guarda en una transacción el `cliente`, la `direccion` y una fila de `consentimiento` por tipo, con `version_texto`. Un correo que ya tiene cuenta, comparado por `correo_hmac`, responde `409 RN-154` con `clave_mensaje: "errores.cuenta.correo_registrado"`.

Criterio de completitud:
Pasan los tests de la HU-001: 17 años → `422 RN-001`, 18 años justos → `201`, sin consentimiento principal → `422 RN-114`, Santiago de Surco → `422 RN-005`, un segundo registro con `Ana@Example.com` → `409 RN-154`, y ninguna columna `*_cifrado` contiene el texto original. También pasa un test que confirma que la respuesta coincide con el esquema de `openapi.yaml`.

---

### T-017: Implementar `GET /catalogo`
HU relacionada: HU-003
Componente: C6 · Módulos de dominio (`catalogo`)
Prioridad: Must
Tamaño: S
Dependencias: T-006, T-009, T-012

Descripción:
Crear `backend/src/modulos/catalogo/`, con un `GET /catalogo` público que devuelve los productos `publicado = true` con su finca (nombre y provincia), `precio_litro` leído de `configuracion` como texto decimal y `organico = senasa_vence >= hoy en Lima` (RN-010). Si `precio_litro` no está configurado, el endpoint responde `503` con `clave_mensaje: "errores.catalogo.precio_no_configurado"` y no inventa un precio.

Criterio de completitud:
Pasan los tests de: con vencimiento SENASA el 30/11/2026, `organico` es `true` el 30/11 y `false` el 01/12; un producto sin `registro_digesa` no aparece; y sin `precio_litro` la respuesta es `503`.

---

## C7 · Worker de tareas programadas

### T-018: Implementar la cola de tareas y el outbox con `FOR UPDATE SKIP LOCKED`
HU relacionada: Infraestructura
Componente: C7 · Worker de tareas programadas
Prioridad: Must
Tamaño: L
Dependencias: T-005, T-009

Descripción:
Crear `backend/src/worker/cola.js` con `encolar(tipo, clave_unica, ejecutar_en, payload)` (idempotente por `clave_unica`), `tomarSiguiente()` (`SELECT … WHERE ejecutar_en <= now() … FOR UPDATE SKIP LOCKED LIMIT 1`) y `reprogramarConBackoff(tarea)` (60, 120, 240, 480 y 960 s; tras el 5.º reintento queda `agotada` y encola una alerta interna, RN-104 a RN-106). Crear `backend/src/worker/reloj.js`, un reloj inyectable para los tests, y el bucle principal en `src/worker.js`, que despacha por `tipo` a un registro de manejadores.

Criterio de completitud:
Pasan los tests de: encolar dos veces la misma `clave_unica` deja una fila, dos workers en paralelo nunca toman la misma tarea, y una tarea que falla siempre se reprograma a +60, +120, +240, +480 y +960 s y luego queda `agotada` con una alerta encolada.

---

## C10 · Adaptadores externos

### T-019: Definir la interfaz `PasarelaPago` y el adaptador simulado
HU relacionada: HU-007
Componente: C10 · Adaptadores externos
Prioridad: Must
Tamaño: M
Dependencias: T-001

Descripción:
Crear `backend/src/modulos/pagos/pasarela.js` con la interfaz `tokenizar`, `cobrar({ claveIdempotencia, importe, token })`, `consultar(claveIdempotencia)` y `reembolsar({ referencia, importe })`, y con la clasificación de resultados `confirmado | rechazado | pendiente | indeterminado` (*timeout* o 5xx = `indeterminado`, RN-135). Crear `adaptadores/simulado.js`, que se programa por test para devolver cada resultado y que, como una pasarela real, no cobra dos veces la misma clave.

Criterio de completitud:
Pasan los tests de: dos `cobrar` con la misma clave devuelven la misma referencia y un solo cargo registrado en el simulador, un *timeout* simulado se clasifica `indeterminado`, y `consultar` devuelve el estado real del cargo.

---

### T-020: Implementar el adaptador de Culqi (sandbox)
HU relacionada: HU-007
Componente: C10 · Adaptadores externos
Prioridad: Must
Tamaño: L
Dependencias: T-019 · ⛔ G5

Descripción:
Crear `backend/src/modulos/pagos/adaptadores/culqi.js`, que implementa `PasarelaPago` contra la API sandbox de Culqi según su documentación oficial. Usa la clave de idempotencia en cada cargo, un *timeout* de 10 s y la normalización de errores de T-019, y verifica la firma de los webhooks. Las credenciales se leen de variables de entorno cargadas por un humano en G5.

Criterio de completitud:
Un test de integración marcado `@sandbox` hace un cargo con la tarjeta de prueba de Culqi, lo consulta por su clave de idempotencia y lo reembolsa, y los tres pasos responden con éxito.

---

### T-021: Implementar los adaptadores de Yape y Plin (sandbox)
HU relacionada: HU-007
Componente: C10 · Adaptadores externos
Prioridad: Must
Tamaño: L
Dependencias: T-019 · ⛔ G5

Descripción:
Crear `adaptadores/yape.js` y `adaptadores/plin.js`, que implementan `PasarelaPago` según la documentación oficial de cada billetera, o del agregador que se haya elegido en G5. Solo guardan la referencia de la transacción, nunca el número de celular (RN-094 y RN-095). Si el sandbox no permite el cobro recurrente o el reembolso, la tarea se **detiene** y reporta el hallazgo, porque eso invalida el supuesto S13.

Criterio de completitud:
Un test `@sandbox` por billetera confirma un cobro recurrente autorizado, su consulta y su reembolso. Si alguno de los tres no existe, se abre un reporte de hallazgo y la tarea se cierra como bloqueada.

---

### T-022: Implementar el módulo de notificaciones con ventana horaria y adaptador simulado
HU relacionada: HU-007, HU-010, HU-011, HU-012
Componente: C10 · Adaptadores externos (`notificaciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-018

Descripción:
Crear `backend/src/modulos/notificaciones/` con `avisarCliente({ canal, clienteId, plantilla, datos })` y `alertarInterno({ rol, plantilla, datos })`. Ambas escriben en `outbox` dentro de la transacción del llamador. Para el **push al cliente**, `no_antes_de` se ajusta a la próxima ventana de 09:00 a 20:00 en Lima (RN-042 y RN-043). Las alertas internas salen a cualquier hora (RN-150, ADR-004), y el correo transaccional se envía sin ventana hasta que se resuelva K10. El manejador `enviar_outbox` usa `adaptadores/correo-simulado.js` y `adaptadores/webpush-simulado.js`; el proveedor real es G6.

Criterio de completitud:
Pasan los tests de: un push al cliente pedido a las 21:30 en Lima queda con `no_antes_de` a las 09:00 del día siguiente, una alerta interna a las 06:10 sale a las 06:10, y el outbox no guarda el correo ni el nombre del cliente en `datos` (solo `cliente_id`).

---

## C6 · Módulos de dominio — `suscripciones` y `pagos`

### T-023: Implementar `POST /medios-pago`
HU relacionada: HU-007
Componente: C6 · Módulos de dominio (`pagos`)
Prioridad: Must
Tamaño: M
Dependencias: T-008, T-013, T-019

Descripción:
Crear la ruta en `backend/src/modulos/pagos/rutas.js` según la §4.3. El esquema solo acepta `tipo`, `token_pasarela` y `ultimos_4`; además, un validador explícito rechaza con `400 RN-020`, **sin registrar el cuerpo en el log**, cualquier petición que traiga `numero_tarjeta`, `cvv`, `pan` o `fecha_vencimiento`. Un `tipo` distinto de `tarjeta`, `yape` o `plin` recibe `422 RN-019`. Si el cliente tiene una suscripción `suspendida_por_pago`, vuelve a `activa` en la misma transacción (RN-051).

Criterio de completitud:
Pasan los tests de: `{ "numero_tarjeta": "4111111111111111", "cvv": "123" }` recibe `400 RN-020` y el log no contiene `4111`; `tipo: "niubiz"` recibe `422 RN-019`; y registrar un medio con una suscripción suspendida la deja `activa`.

---

### T-024: Implementar `POST /suscripciones`
HU relacionada: HU-004
Componente: C6 · Módulos de dominio (`suscripciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-008, T-013, T-014, T-015

Descripción:
Crear `backend/src/modulos/suscripciones/` con la ruta y el servicio de la §4.4. Valida que `litros` sea un entero ≥ 1 sin máximo (RN-017), que el día sea martes, jueves o sábado (RN-061) y que la dirección y el medio de pago pertenezcan al cliente (`404` si no). Calcula `primera_entrega` y `primer_corte_y_cobro` con `primeraEntrega` y `trasladarPorFeriado` de T-014, y el `importe_ciclo` con `precio_litro` de `configuracion`. Si no hay precio configurado, responde `503`, como en T-017.

Criterio de completitud:
Pasan los tests de: 3 L los sábados creada el 13/11/2026 a las 19:00 → `primera_entrega` 2026-11-21 e `importe_ciclo` `"43.50"`; 1.5 L → `422 RN-017`; lunes → `422 RN-061`; y una dirección de otro cliente → `404`.

---

### T-025: Implementar la pausa y la reactivación de la suscripción
HU relacionada: HU-005
Componente: C6 · Módulos de dominio (`suscripciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-024

Descripción:
Agregar `POST /suscripciones/{id}/pausas` (§4.5) y `POST /suscripciones/{id}/reactivacion`. La pausa es `con_plazo` (N ≥ 1, RN-062) o `indefinida`. Si se pide antes del corte de la entrega inmediata, la omite; a partir de las 18:00:00, la mantiene (RN-013). La pausa indefinida nunca se reanuda sola (RN-063), y la reactivación usa la misma regla de corte para la primera entrega. Pausar una suscripción `suspendida_por_pago` (SR-07) queda como `test.todo`.

Criterio de completitud:
Pasan los tests de la HU-005: pausa el 13/11/2026 a las 17:59:59 → se omite el 14/11; a las 18:00:00 → se mantiene; una pausa de 2 pedida el 13/11 a las 10:00 → reanuda el 28/11; N = 0 → `422 RN-062`; y la reactivación el 20/11 a las 18:00:00 → primera entrega el 28/11.

---

### T-026: Implementar el servicio de cobro del ciclo con reintento y verificación
HU relacionada: HU-007
Componente: C6 · Módulos de dominio (`pagos`)
Prioridad: Must
Tamaño: L
Dependencias: T-008, T-018, T-019, T-022

Descripción:
Crear `backend/src/modulos/pagos/reglas/cobrar-ciclo.js`. Con la clave `ciclo-{id}-cliente-{id}` (RN-072), si el saldo es ≥ el importe registra un pago `confirmado` con origen `saldo_favor` sin llamar a la pasarela (RN-097). Si no, llama a `cobrar` por el importe menos el saldo. Un `rechazado` encola `reintento_cobro` a +60 s, una sola vez (RN-046 y RN-080). Un segundo rechazo suspende la entrega y la suscripción y avisa al cliente (RN-047 a RN-050). Un `pendiente` encola `vencer_pago_pendiente` a +1 800 s (RN-074). Un `indeterminado` deja el pago `en_verificacion` y encola **una** consulta inmediata (RN-135 a RN-137); las consultas repetidas (P23) quedan como `test.todo`.

Criterio de completitud:
Pasan los tests de la HU-007 con el adaptador simulado: un rechazo a las 18:00:00 lleva a un reintento a las 18:01:00 y a ninguno más; un 503 lleva a `en_verificacion` y a ningún reintento; si la consulta da confirmado, hay un solo cargo; con S/ 58.00 de saldo y un ciclo de S/ 43.50, hay cero llamadas a la pasarela y quedan S/ 14.50.

---

### T-027: Implementar el webhook de pagos `POST /webhooks/pagos/{pasarela}`
HU relacionada: HU-007, HU-009
Componente: C6 · Módulos de dominio (`pagos`)
Prioridad: Must
Tamaño: M
Dependencias: T-026

Descripción:
Crear la ruta de la §4.6. Delega en el adaptador la verificación de la firma (`401` si falla, se audita sin el payload) y la normalización del evento. Inserta en `evento_pasarela` y, si el `evento_id` ya existía, responde `200` sin hacer nada (RN-073). `cargo.confirmado` pasa el pago a `confirmado` y encola el comprobante en el outbox en la misma transacción (RN-022). Si la entrega ya estaba `suspendida`, crea un `importe_devolver` con origen `pago_tardio` y vence en 72 h, reactiva la suscripción y avisa al cliente para que elija (RN-119, RN-129 y RN-131).

Criterio de completitud:
Pasan los tests de: el mismo webhook dos veces deja un solo pago `confirmado` y un solo comprobante en cola, una firma inválida recibe `401`, y una confirmación a las 18:40:00 de una entrega suspendida a las 18:30:00 crea un `importe_devolver` con `vence_en` 72 h después del aviso.

---

### T-028: Implementar el módulo de comprobantes con OSE simulado y reintentos
HU relacionada: HU-008
Componente: C6 · Módulos de dominio (`comprobantes`) + C10
Prioridad: Must
Tamaño: L
Dependencias: T-018, T-022, T-027

Descripción:
Crear `backend/src/modulos/comprobantes/` con la interfaz `EmisorComprobantes` y el adaptador `adaptadores/ose-simulado.js`; el OSE real es G4. Agregar el manejador `emitir_comprobante`: boleta por defecto, y factura si el cliente la eligió y tiene un RUC de 11 dígitos numéricos (RN-058 y RN-059). Con origen `saldo_favor`, la forma de pago es `aplicacion_saldo_favor` (RN-125 y RN-126, ⚠️ pendiente del contador). Si falla, sigue el backoff de T-018, y tras el 5.º reintento alerta al operador (RN-104 y RN-106). Un pago no confirmado nunca genera comprobante (RN-023).

Criterio de completitud:
Pasan los tests de la HU-008: un pago confirmado sin factura → una boleta; el RUC `2012345678A` se rechaza; un fallo a las 18:00:05 con éxito en el 2.º reintento → una sola boleta y ninguna alerta; y 5 fallos → alerta al operador.

---

## C7 · Worker — tarea del corte

### T-029: Implementar la tarea `corte_dia_reparto`
HU relacionada: HU-004, HU-007, HU-010
Componente: C7 · Worker de tareas programadas
Prioridad: Must
Tamaño: L
Dependencias: T-014, T-025, T-026

Descripción:
Crear `backend/src/worker/tareas/corte-dia-reparto.js`, que se encola con la clave `corte-AAAA-MM-DD` para el instante de corte de cada día de reparto, ya trasladado por feriado. En una transacción por suscripción `activa` (no pausada), crea el `ciclo` con `precio_litro` congelado (RN-102) y su `entrega` `programada`, y llama a `cobrarCiclo`. Al final calcula la demanda del día (entregas programadas más reprogramadas, RN-090 y RN-091) y la guarda como `litros_comprometidos` del pedido a la finca. Si la tarea se ejecuta dos veces, no duplica nada, gracias a `UNIQUE(suscripcion_id, fecha_entrega)`.

Criterio de completitud:
Pasan los tests de: un corte del 13/11/2026 con entregas por 96 L más 4 L reprogramados da una demanda de 100 L; una suscripción pausada no genera ciclo ni cobro; ejecutar el corte dos veces deja un ciclo y un cargo por suscripción; y la entrega del martes 08/12/2026 se genera para el jueves 10/12/2026, con corte el 09/12 a las 18:00:00.

---

## C6 · Módulos de dominio — `operacion`

### T-030: Implementar `POST /operacion/lotes` con umbral de temperatura y escasez
HU relacionada: HU-010
Componente: C6 · Módulos de dominio (`operacion`)
Prioridad: Must
Tamaño: L
Dependencias: T-007, T-013, T-022, T-029

Descripción:
Crear `backend/src/modulos/operacion/lotes/` con la ruta de la §4.7, para `operador` y `administrador_titular`. Exige la temperatura con exactamente 1 decimal (RN-085 y RN-087) y los litros (RN-028), y guarda la lectura en `lectura_temperatura`. Con más de 4.5 °C, el lote queda `en_revision`, con `revision_vence_en = ahora + 900 s`, y se encola `cerrar_revision_lote` con la alerta interna (RN-024 y RN-026). Si los litros recibidos no cubren la demanda, asigna por fecha de alta y luego por `id` (RN-092 y RN-122). Las entregas sin cubrir quedan `fallida` con motivo `falla_abastecimiento_finca` y generan un `importe_devolver` con aviso de elección (RN-129).

Criterio de completitud:
Pasan los tests de la HU-010: 4.5 °C → `recibido`; 4.6 °C → `en_revision` con una alerta interna en el outbox; `3.52` → `422 RN-087`; 5 L para A (3 L, alta 01/10), B (3 L, 05/10) y C (2 L, 10/10) cubren A y C y dejan B `fallida`; y un `repartidor` recibe `403`.

---

### T-031: Implementar la corrección de temperatura, la reversión y el cierre de la revisión a los 900 s
HU relacionada: HU-010
Componente: C6 · Módulos de dominio (`operacion`) + C7
Prioridad: Must
Tamaño: L
Dependencias: T-030

Descripción:
Agregar `POST /operacion/lotes/{id}/correcciones-temperatura` (§4.8): inserta una lectura nueva con `corrige_a_id` y una justificación obligatoria (RN-089). Si la hace el `administrador_titular` con el lote `en_revision` y `ahora < revision_vence_en`, el lote vuelve a `recibido` (RN-127). Si la hace el operador, no revierte (RN-128). Crear el manejador `cerrar_revision_lote`: si a los 900 s el lote sigue `en_revision`, pasa a `invalidado` y reasigna sus entregas `programada` a otros lotes del día con litros libres (RN-093). Las que no se cubren quedan `fallida` `imputable_muuvi` con un importe a devolver (RN-054). Las entregas `en_ruta` pasan a `suspendida` (RN-134).

Criterio de completitud:
Pasan los tests de: lote `en_revision` a las 05:50:00, el administrador corrige a las 06:04:59 → `recibido`; a las 06:05:00 → ya `invalidado` y la corrección responde `reversion: "fuera_de_plazo"`; la corrección del operador → `no_aplica`; y sin otro lote libre, 12 entregas quedan `fallida` con 12 avisos de elección.

---

### T-032: Implementar `POST /reparto/cierres` (sincronización por lotes y sin duplicados)
HU relacionada: HU-011
Componente: C6 · Módulos de dominio (`operacion`)
Prioridad: Must
Tamaño: L
Dependencias: T-014, T-030

Descripción:
Crear `backend/src/modulos/operacion/reparto/` con la ruta de la §4.9, solo para `repartidor`, hasta 200 cierres por petición. Cada cierre se procesa en orden de `registrado_en` y en su propia transacción. Si el `cierre_id` ya existe, el resultado es `duplicado` (RN-144). Una entrega de otro repartidor es `rechazado RN-099`. `entregada` exige temperatura (RN-086); a más de 4.5 °C rechaza, pone el lote `en_revision` y alerta (RN-025). Las botellas devueltas no pueden superar el saldo (RN-031). `fallida` exige causa (RN-052): `imputable_cliente` reprograma una vez al siguiente día de reparto no feriado y avisa por correo (RN-066, RN-107, RN-139 y RN-140); la segunda falla cancela. Un lote `en_revision` da `rechazado RN-132`. El caso K8 queda como `test.todo`. Agregar también `GET /operacion/dias/{fecha}/entregas`, que al `repartidor` le devuelve solo sus entregas, con identificadores, orden y litros, y que la usa T-036.

Criterio de completitud:
Pasan los tests de la HU-011: saldo 2, devuelve 2, entrega 3 → saldo 3; devuelve 3 con saldo 2 → `rechazado RN-031`; el mismo `cierre_id` enviado dos veces → un `aplicado` y un `duplicado` sin duplicar botellas; falla el 05/12/2026 → `reprogramada_para: "2026-12-10"`; y `cerrada_en` es el `registrado_en` enviado, no la hora de llegada; y `GET /operacion/dias/2026-11-14/entregas` con el token de un repartidor no devuelve entregas de otro.

---

## C6 · Módulos de dominio — `reclamos`

### T-033: Implementar `POST /reclamos` (Libro de Reclamaciones sin cuenta)
HU relacionada: HU-012
Componente: C6 · Módulos de dominio (`reclamos`)
Prioridad: Must
Tamaño: M
Dependencias: T-009, T-010, T-012, T-013, T-015, T-022

Descripción:
Crear `backend/src/modulos/reclamos/` con la ruta pública de la §4.11, con un límite de 5 por hora por IP y sin servicios anti-bots de terceros (RN-118). Asigna el código `LR-AAAA-NNNNNN` de la secuencia del año (RN-034) y calcula `fecha_limite = sumarDiasHabiles(fecha, 15, feriados)` (RN-035 y RN-101). Cifra el nombre, el correo y el detalle, y encola la constancia por correo (RN-112). La respuesta enmascara el correo (`l***@example.com`). SR-15 (reclamo sin correo) y SR-14 (reclamo en sábado) quedan como `test.todo`.

Criterio de completitud:
Pasan los tests de la HU-012: un reclamo del lunes 16/11/2026 tiene fecha límite el 07/12/2026; uno del 24/11/2026 con el feriado del 08/12 cargado, el 16/12/2026; dos reclamos seguidos llevan códigos consecutivos; y el sexto reclamo en una hora desde la misma IP recibe `429`.

---

## C1 · PWA — módulo cliente

### T-034: Crear la pantalla de registro accesible
HU relacionada: HU-001
Componente: C1 · PWA módulo cliente
Prioridad: Must
Tamaño: M
Dependencias: T-002, T-016

Descripción:
Crear `frontend/src/cliente/Registro.jsx` con los campos de la §4.2, todos con su `label` y sus textos tomados de `es-PE.json`. Las casillas del consentimiento principal y del envío a terceros van **separadas y desmarcadas**, y el texto legal sale de una clave de traducción que redacta un humano (PRINCIPIO-008.9). El distrito se elige de una lista cerrada de 3 opciones, las áreas táctiles miden ≥ 48 × 48 px, y los errores `422` de la API se muestran con su `clave_mensaje`.

Criterio de completitud:
Pasan los tests de componente (`vitest` con `jsdom` y `@testing-library/react`): las dos casillas aparecen desmarcadas al cargar, y un `422 RN-001` muestra el texto de `errores.cuenta.menor_de_edad`. `axe-core` no reporta violaciones WCAG 2.1 AA.

---

### T-035: Crear las pantallas de catálogo y suscripción
HU relacionada: HU-003, HU-004
Componente: C1 · PWA módulo cliente
Prioridad: Must
Tamaño: L
Dependencias: T-002, T-017, T-024

Descripción:
Crear `frontend/src/cliente/Catalogo.jsx`, que muestra el producto, la finca, la provincia y el precio con el formato `S/ 1,250.50` (RN-037), además de la etiqueta "Orgánico" si `organico` es `true`. Crear `frontend/src/cliente/NuevaSuscripcion.jsx`, con los litros como entero ≥ 1, el día como martes, jueves o sábado, y la elección de dirección y de medio de pago. Antes de confirmar se ven el importe del ciclo, la franja 07:00–10:00 y la primera entrega con el formato `DD/MM/AAAA` que devuelve la API. Los montos se muestran tal como los devuelve la API, sin cálculos en coma flotante.

Criterio de completitud:
Pasan los tests de componente: el precio `"1250.50"` se muestra como `S/ 1,250.50`, la fecha `2026-11-21` como `21/11/2026`, y la pantalla no ofrece el lunes ni el domingo como opción.

---

## C3 · PWA — módulo reparto sin conexión

### T-036: Crear el módulo de reparto con la cola de cierres en IndexedDB
HU relacionada: HU-011
Componente: C3 · PWA módulo reparto (sin conexión)
Prioridad: Must
Tamaño: L
Dependencias: T-002, T-032

Descripción:
Crear `frontend/src/reparto/` con la lista del día (`GET /operacion/dias/{fecha}/entregas`) y el formulario de cierre: estado, temperatura con 1 decimal, botellas devueltas y causa si es `fallida`. Cada cierre lleva un `crypto.randomUUID()` y su `registrado_en` en UTC. Se guarda primero en IndexedDB (`muuvi-reparto`, almacén `cierres_pendientes`) y se envía en orden con `POST /reparto/cierres` al haber señal, reintentando con backoff. En IndexedDB solo se guardan identificadores, orden, litros y los datos del cierre, **nunca** nombre, dirección ni teléfono (RN-145). La dirección sin conexión (K9) queda como `test.todo`.

Criterio de completitud:
Pasan los tests de componente: sin red, dos cierres quedan en `cierres_pendientes` y se envían en orden al volver la red; un `duplicado` del servidor los borra igual de la cola; y un recorrido de IndexedDB no encuentra ninguna clave `nombre`, `direccion`, `telefono` ni `correo`.

---

## C4 · PWA — Libro de Reclamaciones público

### T-037: Crear el formulario público del Libro de Reclamaciones
HU relacionada: HU-012
Componente: C4 · PWA Libro de Reclamaciones público
Prioridad: Must
Tamaño: M
Dependencias: T-002, T-033

Descripción:
Crear `frontend/src/libro/LibroReclamaciones.jsx` en la ruta `/libro-de-reclamaciones`, accesible sin iniciar sesión y enlazada desde el menú principal (RN-109). El formulario pide tipo (reclamo o queja), nombre, correo, detalle y pedido, y al registrar muestra el código y la fecha límite en formato `DD/MM/AAAA`. Los textos legales salen de claves de traducción que redacta un humano.

Criterio de completitud:
Pasan los tests de componente: la ruta se abre sin sesión, el menú principal tiene el enlace, y tras un `201` se muestran `LR-2026-000017` y `07/12/2026`. `axe-core` no reporta violaciones WCAG 2.1 AA.

---

## Testing · reglas Must

### T-038: Escribir los tests de aceptación de las reglas de horario y calendario
HU relacionada: HU-004, HU-005, HU-007, HU-012
Componente: C6 · Módulos de dominio (`calendario`) + C7
Prioridad: Must
Tamaño: M
Dependencias: T-029, T-033

Descripción:
Crear `backend/test/aceptacion/horarios.test.js`, que ejecuta contra la base de pruebas, con el reloj inyectado, cada ejemplo con fecha de las HU-004, HU-005, HU-007 y HU-012. Los bordes son 17:59:59 y 18:00:00 (el corte es exclusivo), el corte a las 23:00:00 UTC, el feriado del 08/12/2026 y los días hábiles con y sin feriado. Cada test lleva en su nombre el ID de la regla (`test_RN084_…`, `test_RN123_…`, `test_RN101_…`).

Criterio de completitud:
El archivo tiene al menos un test por cada ejemplo con fecha de esas 4 HU, todos pasan, y la cobertura de `modulos/calendario/reglas/` es ≥ 80 %.

---

### T-039: Escribir los tests de no doble cobro e idempotencia
HU relacionada: HU-007, HU-009
Componente: C6 · Módulos de dominio (`pagos`) + C10
Prioridad: Must
Tamaño: M
Dependencias: T-027, T-029

Descripción:
Crear `backend/test/aceptacion/idempotencia-pagos.test.js` con el adaptador simulado, que cubre estos escenarios. El corte del 13/11/2026 ejecutado dos veces. El mismo webhook de confirmación enviado dos veces. Un 503 a las 18:00:00 que queda `en_verificacion` y luego se confirma por consulta. Un webhook que llega mientras la consulta está en curso. Un pago con saldo igual al importe. En cada caso se verifica que en la base hay **como máximo un** `pago` `confirmado` por ciclo y en el simulador **un solo** cargo (RN-072, RN-073, RN-097 y RN-135 a RN-138).

Criterio de completitud:
Los 5 escenarios pasan, y cada uno comprueba de forma explícita que el simulador tiene un cargo y que la base tiene un pago confirmado por ciclo.

---

### T-040: Escribir los tests de cadena de frío y de la ventana de 900 s
HU relacionada: HU-010, HU-011
Componente: C6 · Módulos de dominio (`operacion`) + C7
Prioridad: Must
Tamaño: M
Dependencias: T-031, T-032

Descripción:
Crear `backend/test/aceptacion/cadena-frio.test.js` con estos bordes. Temperatura de 4.5 °C frente a 4.6 °C, en la recepción y en la puerta del cliente. Una reversión del administrador a 899 s frente a una a 900 s. La corrección del operador sin reversión. El intento de `UPDATE` de una lectura. Un cierre `entregada` de un lote `en_revision`. La invalidación con reasignación y sin litros libres. Cada test lleva el ID de la regla (RN-024 a RN-026, RN-088, RN-089, RN-127, RN-128, RN-132 y RN-134).

Criterio de completitud:
Todos los escenarios pasan, y la cobertura de `modulos/operacion/**/reglas/` es ≥ 80 %.

---

### T-041: Escribir los tests del esquema PCI y de los datos personales
HU relacionada: HU-001, HU-007
Componente: C8 · Base de datos + C9
Prioridad: Must
Tamaño: S
Dependencias: T-008, T-009, T-012

Descripción:
Crear `backend/test/seguridad/esquema.test.js`, que consulta `information_schema.columns` y falla si existe alguna columna cuyo nombre contenga `tarjeta`, `pan`, `cvv` o `vencimiento`, salvo `senasa_vence` (RN-020). También falla si algún campo de la lista de datos personales de la §3.4 no es `BYTEA` con el sufijo `_cifrado`. Agregar `backend/test/seguridad/logs.test.js`, que ejecuta los flujos de registro y de reclamo y revisa que la salida del logger no contenga el correo, el nombre ni el teléfono usados (RN-007).

Criterio de completitud:
Los dos archivos pasan sobre el esquema completo, y agregar en una base de prueba una columna `numero_tarjeta TEXT` hace fallar el test.

---

### T-042: Escribir el E2E de registro, suscripción, cobro y entrega
HU relacionada: HU-001, HU-004, HU-007, HU-011
Componente: C1 · PWA módulo cliente + C3 + C5
Prioridad: Must
Tamaño: L
Dependencias: T-004, T-034, T-035, T-036, T-039, T-056, T-057

Descripción:
Crear `e2e/flujo-principal.spec.js` con Playwright, que corre contra el backend con el adaptador de pago simulado, el reloj inyectado y datos sintéticos, en un viewport de 360 × 640 px (Android de gama baja). Recorre el registro de una clienta de San Isidro, una suscripción de 3 L los sábados, el corte simulado del viernes a las 18:00:00 con el cobro confirmado, la asignación del lote y del repartidor, la salida a ruta y el cierre de la entrega por el repartidor a 4.1 °C, con 0 botellas devueltas.

Criterio de completitud:
El E2E pasa en el CI de punta a punta, y al final la entrega está `entregada`, el pago `confirmado` y hay un comprobante en la cola (PRINCIPIO-009).

---

# MUST · Agregadas en v0.3.0 (correcciones de la auditoría de trazabilidad)

Estas tareas resuelven los hallazgos CRÍTICA y ALTA de [`09-auditoria-trazabilidad.md`](09-auditoria-trazabilidad.md). Cada una cita su hallazgo.

## C6 · Módulos de dominio — `devoluciones` (QA-C2-001)

### T-045: Implementar el servicio de devoluciones y el reintento de reembolsos
HU relacionada: HU-009
Componente: C6 · Módulos de dominio (`devoluciones`)
Prioridad: Must
Tamaño: L
Dependencias: T-018, T-019, T-022, T-027, T-030, T-031

Descripción:
Crear `backend/src/modulos/devoluciones/` con estas funciones:
- `registrarImporteDevolver({ clienteId, origen, entregaId, pagoId, importe })`: crea el importe, envía el aviso de elección en 300 s como máximo (RN-129) y encola `reembolso_automatico_72h` con `vence_en = avisado_en + 259 200 s`. Reemplaza las inserciones directas de `importe_devolver` que hacen T-027, T-030 y T-031.
- `solicitarReembolso({ pagoId, importe, motivo })`: llama a `reembolsar` de la pasarela en 3 600 s como máximo (RN-055) y crea la fila `reembolso`. Si falla, sigue el backoff de T-018 (RN-105) y, tras el 5.º reintento, alerta al operador (RN-106).
- `acreditarSaldo(clienteId, importe, motivo)`: inserta en `movimiento_saldo`.

Criterio de completitud:
Pasan los tests con el adaptador simulado:
- un reembolso que falla a las 09:45:00 y tiene éxito en el 2.º reintento (09:48:00) queda `confirmado`, sin alerta;
- 5 fallos generan una alerta interna;
- `registrarImporteDevolver` deja una tarea `reembolso_automatico_72h` con `ejecutar_en` exactamente 259 200 s después del aviso;
- T-027, T-030 y T-031 ya no insertan en `importe_devolver` de forma directa (lo verifica un `grep` en el test).

---

### T-046: Implementar `POST /importes-devolver/{id}/eleccion`
HU relacionada: HU-009
Componente: C6 · Módulos de dominio (`devoluciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-013, T-045

Descripción:
Crear la ruta de la §4.10 de la arquitectura, solo para el cliente dueño del importe. Con `reembolso`, llama a `solicitarReembolso` (RN-120); con `saldo_favor`, llama a `acreditarSaldo` (RN-121). En los dos casos, marca `eleccion` y `resuelto_en` y cancela la tarea `reembolso_automatico_72h`. Una elección posterior al vencimiento (SR-10) queda como `test.todo`.

Criterio de completitud:
Pasan los tests de la HU-009:
- con el aviso a las 09:42:00 del 14/11/2026, elegir saldo a favor el 17/11/2026 a las 09:41:59 suma S/ 43.50 al saldo y no hay reembolso automático;
- elegir reembolso lo solicita antes de 3 600 s;
- un importe de otro cliente responde `404`;
- `opcion: "efectivo"` responde `422`.

---

### T-047: Implementar la tarea `reembolso_automatico_72h`
HU relacionada: HU-009, HU-007
Componente: C7 · Worker de tareas programadas
Prioridad: Must
Tamaño: S
Dependencias: T-045

Descripción:
Crear `backend/src/worker/tareas/reembolso-automatico-72h.js`. Si el importe sigue sin elección al vencer las 72 h, llama a `solicitarReembolso` al medio de pago original y marca `eleccion = 'reembolso'` con origen `automatico` en la auditoría (RN-130). Si el cliente ya eligió, termina sin hacer nada.

Criterio de completitud:
Pasan los tests: con el aviso el 13/11/2026 a las 18:42:00, el 16/11/2026 a las 18:41:59 no hay reembolso y a las 18:42:00 se solicita; con una elección previa, la tarea no llama a la pasarela.

---

### T-048: Emitir la nota de crédito de cada reembolso y de cada saldo a favor
HU relacionada: HU-008, HU-009
Componente: C6 · Módulos de dominio (`comprobantes`)
Prioridad: Must
Tamaño: M
Dependencias: T-028, T-045

Descripción:
Agregar a `comprobantes` el tipo `nota_credito`. Lo emite el manejador `emitir_comprobante` cuando se confirma un reembolso o se acredita un saldo a favor que proviene de un pago con comprobante, y siempre referencia al comprobante original por `referencia_id` (RN-057). Usa el mismo backoff y la misma alerta que T-028. ⚠️ El tratamiento del saldo a favor está pendiente de validación por el contador (RN-125 y RN-126).

Criterio de completitud:
Pasan los tests:
- el reembolso de S/ 43.50 de la boleta B001-00000123 genera una nota de crédito por S/ 43.50 con `referencia_id` de esa boleta;
- un pago `rechazado` no genera nota de crédito;
- no hay dos notas de crédito para el mismo reembolso.

---

### T-049: Completar las devoluciones de las entregas fallidas, canceladas y suspendidas
HU relacionada: HU-009, HU-011
Componente: C6 · Módulos de dominio (`operacion`)
Prioridad: Must
Tamaño: M
Dependencias: T-031, T-032, T-045

Descripción:
Extender los dos lugares donde una entrega deja de entregarse:
- **`POST /reparto/cierres` (T-032):**
  - una entrega pagada que queda `fallida` con causa `imputable_muuvi` llama a `registrarImporteDevolver` (RN-053);
  - una reprogramada que pasa a `cancelada` llama a `solicitarReembolso`, sin elección (RN-108).
- **`cerrar_revision_lote` (T-031):** cada entrega `en_ruta` que pasa a `suspendida` con motivo `lote_invalidado` llama a `registrarImporteDevolver` (RN-079).

Criterio de completitud:
Pasan los tests:
- una entrega de S/ 43.50 cerrada `fallida` con causa `imputable_muuvi` crea un importe de S/ 43.50 con su aviso;
- una reprogramada que vuelve a fallar por causa del cliente queda `cancelada` y solicita un reembolso de S/ 43.50 sin aviso de elección;
- una entrega de 2 L suspendida por invalidación crea un importe de S/ 29.00.

---

## C6 · Módulos de dominio — `cuentas` (QA-C2-003)

### T-050: Implementar `POST /clientes/me/baja`
HU relacionada: HU-002
Componente: C6 · Módulos de dominio (`cuentas`)
Prioridad: Must
Tamaño: M
Dependencias: T-013, T-033, T-045

Descripción:
Crear la ruta de la baja, solo para el propio cliente (RN-099). La baja se rechaza con `409` y el pendiente que la bloquea si el cliente tiene una suscripción distinta de `cancelada`, una entrega `programada` o `en_ruta`, o un reclamo sin respuesta (RN-098). Las botellas pendientes y el saldo a favor no la bloquean. Si la acepta, pasa la cuenta a `baja_solicitada` con `fecha_baja`, reembolsa el saldo a favor mayor de S/ 0.00 con `solicitarReembolso` (RN-124) y encola la anonimización.

Criterio de completitud:
Pasan los tests de la HU-002:
- con una entrega `en_ruta` responde `409` con `clave_mensaje: "errores.baja.entrega_en_curso"`;
- con 2 botellas pendientes y nada más, se acepta;
- con S/ 14.50 de saldo, a las 10:00, el reembolso se solicita antes de las 11:00;
- con S/ 0.00 de saldo no hay reembolso;
- un operador que llama a la ruta recibe `403`.

---

### T-051: Implementar la tarea `anonimizar_bajas` con borrado criptográfico
HU relacionada: HU-002
Componente: C7 · Worker de tareas programadas + C9
Prioridad: Must
Tamaño: M
Dependencias: T-010, T-018, T-050

Descripción:
Crear `backend/src/worker/tareas/anonimizar-bajas.js`, que corre a diario a las 03:00 en Lima. Para cada cuenta en `baja_solicitada`:
- sobrescribe sus columnas `*_cifrado` (y las de sus direcciones) con `NULL`;
- llama a `destruirClaveDatos(clienteId)` y pasa la cuenta a `anonimizada` (RN-008, ADR-006);
- conserva sin cambios los comprobantes, pagos, entregas y auditoría (RN-111).

La tarea no hace `DELETE` de filas (PRINCIPIO-008.7). Además, encola una ejecución exacta a `fecha_baja + 2 592 000 s` como respaldo, para cumplir el plazo aunque falle la corrida diaria.

Criterio de completitud:
Pasan los tests:
- una baja del 12/11/2026 a las 10:00 está `anonimizada` a más tardar el 12/12/2026 a las 10:00:00;
- después, `descifrarCampo` falla para ese cliente y sus boletas siguen consultables;
- la tarea no ejecuta ningún `DELETE` (se verifica en el log de consultas de la base de pruebas).

---

## C6 · Módulos de dominio — `catalogo`, `configuracion` y `auditoria` (QA-C2-004 y QA-C2-007)

### T-052: Implementar el registro de fincas y productos
HU relacionada: HU-017
Componente: C6 · Módulos de dominio (`catalogo`)
Prioridad: Must
Tamaño: M
Dependencias: T-006, T-009, T-013

Descripción:
Crear los endpoints de la §4.13 de la arquitectura para operador y administrador titular, cada uno registrado en `auditoria`:
- `POST /operacion/fincas`: solo acepta las provincias `canete` y `huaral`;
- `PATCH /operacion/fincas/{id}`;
- `GET /operacion/fincas`;
- `POST /operacion/productos`: el producto nace `publicado = false`;
- `POST /operacion/productos/{id}/publicacion`: rechaza con `422 RN-009` si falta el registro DIGESA;
- `POST /operacion/productos/{id}/despublicacion`.

Criterio de completitud:
Pasan los tests de la HU-017:
- una finca de Huaral con SENASA vigente hasta el 30/11/2026 aparece como orgánica en `GET /catalogo` el 30/11 y no el 01/12;
- publicar sin DIGESA responde `422 RN-009`;
- provincia `lima` responde `422`;
- un `repartidor` recibe `403`;
- cada acción deja una fila en `auditoria`.

---

### T-053: Implementar la doble confirmación con contraseña y código por correo
HU relacionada: HU-013, HU-014
Componente: C6 · Módulos de dominio (`permisos` y `auditoria`)
Prioridad: Must
Tamaño: L
Dependencias: T-009, T-013, T-022

Descripción:
Crear `backend/migraciones/005_confirmacion_doble.sql`, con la tabla `confirmacion_doble` de la §3.2, y `backend/src/modulos/permisos/confirmacion-doble.js` con estas funciones:
- `solicitar(accion, entidadId, payload, usuario)`;
- `verificarContrasena(id, contrasena)`: verifica `argon2id`, genera un código de 6 dígitos con `crypto.randomInt`, guarda solo su HMAC y lo envía por correo con `avisar` del módulo de notificaciones (canal `correo`, sin ventana horaria);
- `verificarCodigo(id, codigo)`: comparación en tiempo constante. El código vence a los 300 s y la solicitud se cancela al tercer intento fallido (RN-151 y RN-152). Si es correcto, ejecuta el manejador registrado para la `accion`.

Agregar las rutas `POST /operacion/confirmaciones/{id}/contrasena` y `/codigo`, solo para `administrador_titular`, y registrar cada transición en `auditoria`.

Criterio de completitud:
Pasan los tests con el correo simulado:
- un código enviado a las 10:00:05 se acepta a las 10:05:05 y se rechaza a las 10:05:06;
- 3 códigos incorrectos cancelan la solicitud;
- un código ya usado no sirve para otra solicitud;
- un operador recibe `403`;
- el código nunca aparece en la base, en el outbox ni en los logs (solo su HMAC).

---

### T-054: Implementar la propuesta y la confirmación del precio por litro
HU relacionada: HU-013
Componente: C6 · Módulos de dominio (`configuracion`)
Prioridad: Must
Tamaño: M
Dependencias: T-017, T-053

Descripción:
Crear `POST /operacion/configuracion/precio`, para operador o administrador titular, con `{ "precio_litro": "15.00" }` (texto decimal con 2 decimales y mayor que 0). La ruta crea una `confirmacion_doble` con `accion = 'cambiar_precio'` y **no** modifica `configuracion` (RN-153). Registrar el manejador de `cambiar_precio`: al confirmarse, escribe `precio_litro` con `vigente_desde = ahora`, `propuesto_por` y `confirmado_por`. Los ciclos ya cortados conservan su precio, porque el corte lo congela en `ciclo.precio_litro` (RN-102). El aviso a los suscriptores (RN-103) queda como `test.todo` hasta que se decida P14.

Criterio de completitud:
Pasan los tests de la HU-013:
- sin precio cargado, la propuesta de S/ 14.50 confirmada con los dos pasos hace que `GET /catalogo` responda 200 con `"14.50"`;
- una propuesta sin confirmar no cambia el precio;
- un ciclo cortado el 13/11/2026 conserva S/ 14.50 tras un cambio a S/ 15.00 el 14/11/2026;
- un `repartidor` recibe `403`.

---

### T-055: Implementar las anulaciones con doble confirmación
HU relacionada: HU-014
Componente: C6 · Módulos de dominio (`auditoria`)
Prioridad: Must
Tamaño: M
Dependencias: T-008, T-053

Descripción:
Crear `POST /operacion/anulaciones`, solo para `administrador_titular`, con `{ "tipo": "pago" | "comprobante" | "suscripcion" | "entrega", "id", "motivo" }`. La ruta crea una `confirmacion_doble` y no anula nada hasta completarse (RN-032 y RN-151). Los manejadores marcan el registro como anulado, con `anulado_en`, `anulado_por` y `motivo`, en columnas nuevas de una migración aditiva `006_anulaciones.sql`, y **nunca** lo borran. Un comprobante anulado debe pasar por el OSE/PSE con su comunicación de baja o su nota de crédito; ese punto queda como `test.todo` hasta elegir el OSE (G4).

Criterio de completitud:
Pasan los tests de la HU-014:
- el pago 5521 se anula solo tras los dos pasos y la auditoría registra la solicitud, los pasos y la ejecución;
- sin el código, no se anula nada;
- un operador recibe `403`;
- después de la anulación, la fila sigue existiendo.

---

## C6 · Módulos de dominio — `operacion` (QA-C2-005 y QA-C2-008)

### T-056: Implementar la asignación de lotes y repartidores del día
HU relacionada: HU-010, HU-011
Componente: C6 · Módulos de dominio (`operacion`)
Prioridad: Must
Tamaño: L
Dependencias: T-030, T-032

Descripción:
Crear `backend/src/modulos/operacion/asignacion/` con el algoritmo de la §4.13 de la arquitectura: asigna las entregas `programada` por antigüedad de la suscripción (RN-122) a los lotes `recibido` del mismo día con litros libres. Una reprogramada nunca va al lote de su entrega original (RN-068 y RN-113), y un lote `en_revision` o `invalidado` no recibe entregas (RN-027 y RN-132). Agregar:
- `GET /operacion/dias/{fecha}/asignaciones/propuesta`, que devuelve la propuesta;
- `POST /operacion/dias/{fecha}/asignaciones`, con el que el operador la confirma o la ajusta, con lote, repartidor y orden;
- `GET /operacion/lotes?fecha=`.

Criterio de completitud:
Pasan los tests:
- con dos lotes de 30 L y 70 L y 100 L de demanda, todas las entregas quedan asignadas sin superar los litros de ningún lote;
- asignar a un lote `en_revision` responde `422 RN-132`;
- asignar una reprogramada a su lote original responde `422 RN-113`;
- asignar una entrega `entregada` responde `409`.

---

### T-057: Implementar la salida a ruta (`programada` → `en_ruta`), en línea y sin conexión
HU relacionada: HU-011
Componente: C6 · Módulos de dominio (`operacion`) + C3
Prioridad: Must
Tamaño: L
Dependencias: T-036, T-056

Descripción:
Crear `POST /reparto/salidas`, solo para el repartidor, con `{ "salida_id", "fecha", "registrado_en" }` e idempotente por `salida_id`. Pasa a `en_ruta` las entregas `programada` de esa fecha asignadas al repartidor cuyo lote esté `recibido`. Toda transición que no figure en el diagrama de la entrega se rechaza (RN-071). En la PWA (T-036), agregar el botón "Salir a ruta", que guarda la salida en la misma cola de IndexedDB, antes de los cierres, y la sincroniza en orden. Un cierre `entregada` de una entrega que no está `en_ruta` responde `rechazado RN-071`.

Criterio de completitud:
Pasan los tests:
- la salida a las 07:00 pasa las 12 entregas del repartidor a `en_ruta` y no toca las de otro repartidor;
- la misma `salida_id` enviada dos veces no cambia nada la segunda vez;
- sin red, la salida y dos cierres se sincronizan en ese orden;
- cerrar una entrega `programada` como `entregada` responde `rechazado RN-071`.

---

### T-058: Completar los avisos de lote en revisión e invalidado
HU relacionada: HU-010, HU-011
Componente: C6 · Módulos de dominio (`operacion`) + C10
Prioridad: Must
Tamaño: M
Dependencias: T-031, T-057

Descripción:
Extender el paso de un lote a `en_revision` (T-030 y T-032) para que, en 60 s como máximo, envíe con `alertarInterno` el aviso "Lote en revisión, no entregar" a **cada repartidor** con entregas `en_ruta` de ese lote (RN-133, RN-150). Extender `cerrar_revision_lote` (T-031) para que envíe por correo, en 3 600 s como máximo, el aviso de disculpa a cada cliente con una entrega `entregada` de ese lote (RN-078). Agregar los dos casos a `backend/test/aceptacion/cadena-frio.test.js` (T-040).

Criterio de completitud:
Pasan los tests:
- con el lote en revisión a las 08:20:00 y 3 repartidores con entregas `en_ruta` de ese lote, hay 3 alertas en el outbox con `no_antes_de` ≤ 08:21:00 y ninguna para un 4.º repartidor sin entregas del lote;
- con el lote invalidado a las 08:35:00 y 7 entregas ya `entregada`, hay 7 correos de disculpa con `no_antes_de` ≤ 09:35:00.

---

## C6 · Módulos de dominio — `suscripciones` y `reclamos` (QA-C2-007)

### T-059: Implementar la modificación y la cancelación de la suscripción
HU relacionada: HU-006
Componente: C6 · Módulos de dominio (`suscripciones`) + C7
Prioridad: Must
Tamaño: L
Dependencias: T-029, T-045

Descripción:
Crear `PATCH /suscripciones/{id}`, que acepta `litros`, `dia_entrega` y `direccion_id`, valida RN-005, RN-017 y RN-061 y guarda el cambio en `cambio_suscripcion`, aplicable desde el primer corte posterior (RN-083). Crear `POST /suscripciones/{id}/cancelacion`:
- **antes del corte:** no se crean más entregas (RN-014);
- **a partir de las 18:00:00:** se mantiene la entrega ya cortada y se cancela después (RN-081 y RN-082);
- **en los dos casos:** el saldo a favor se reembolsa con `solicitarReembolso` (RN-124).

Modificar `corte_dia_reparto` (T-029) para que aplique los cambios pendientes antes de crear el ciclo. Modificar o cancelar una suscripción ya `cancelada` (SR-08) queda como `test.todo`.

Criterio de completitud:
Pasan los tests de la HU-006:
- un cambio de 2 L a 3 L el 13/11/2026 a las 19:00 deja el 14/11 en 2 L y el 21/11 en 3 L;
- el mismo cambio a las 17:59:59 ya aplica al 14/11, con un cobro de S/ 43.50;
- una cancelación a las 16:00 no crea la entrega del 14/11;
- una a las 20:00 la mantiene y no crea la del 21/11;
- el domingo responde `422 RN-061`;
- Barranco responde `422 RN-005`;
- con S/ 14.50 de saldo, la cancelación a las 10:00 solicita el reembolso antes de las 11:00.

---

### T-060: Implementar la alerta del día 10 de los reclamos sin respuesta
HU relacionada: HU-012
Componente: C7 · Worker de tareas programadas
Prioridad: Must
Tamaño: S
Dependencias: T-033

Descripción:
Crear `backend/src/worker/tareas/alerta-reclamo-dia-10.js`, que corre a diario a las 08:00 en Lima. Envía con `alertarInterno` una alerta al operador por cada reclamo `abierto` que hoy cumple 10 días hábiles desde su registro, con `sumarDiasHabiles` (RN-036 y RN-101). No alerta por los reclamos `respondido` (RN-149). Agregar `GET /operacion/reclamos?estado=abierto`, ordenado por fecha límite (RN-035).

Criterio de completitud:
Pasan los tests:
- un reclamo del 16/11/2026 genera la alerta el 30/11/2026 y no el 27/11;
- uno respondido el 25/11/2026 no genera alerta el 30/11;
- con el feriado del 08/12/2026, uno del 24/11/2026 genera la alerta el 09/12/2026.

---

## C6 · Módulos de dominio — `cuentas` y `comprobantes` (QA-C2-013)

### T-061: Implementar los datos de facturación y la consulta de comprobantes del cliente
HU relacionada: HU-008
Componente: C6 · Módulos de dominio (`cuentas` y `comprobantes`)
Prioridad: Must
Tamaño: M
Dependencias: T-010, T-013, T-048

Descripción:
Crear `PATCH /clientes/me/facturacion`, con `{ "tipo_comprobante", "ruc"? }`. Rechaza con `422 RN-058` un RUC que no tenga exactamente 11 dígitos numéricos, cifra un RUC que empieza con `10` (RN-060) y guarda `ruc_tipo`. Crear `GET /comprobantes`, que devuelve los comprobantes del cliente con sus montos como texto decimal; el importe de las notas de crédito va con signo negativo, para que la PWA lo muestre como `-S/ 43.50` (RN-110).

Criterio de completitud:
Pasan los tests de la HU-008:
- `2012345678`, `2012345678A` y `201234567890` responden `422 RN-058`;
- `20123456789` se acepta y la siguiente emisión es una factura;
- `10456789012` queda en `ruc_cifrado` y su texto no aparece en la base;
- `GET /comprobantes` devuelve la nota de crédito con importe `"-43.50"`.

---

## C10 · Adaptadores externos (QA-C2-015)

### T-062: Hacer cumplir el registro de proveedores con DPA vigente
HU relacionada: HU-001, HU-007
Componente: C10 · Adaptadores externos
Prioridad: Must
Tamaño: M
Dependencias: T-009, T-019, T-022, T-028

Descripción:
Crear la migración aditiva `007_proveedor_habilitado.sql`, con la tabla de la §3.2, **sin insertar filas**: la carga un humano. Crear `backend/src/modulos/configuracion/proveedores.js` con `exigirProveedorHabilitado(proveedor, hoy)` y aplicarlo en los registros de adaptadores de `pagos`, `comprobantes` y `notificaciones`. Todo adaptador real, actual o futuro, pasa por esa verificación antes de enviar datos personales; los adaptadores simulados la omiten. Si el proveedor no está habilitado, la operación falla con `PROVEEDOR_NO_HABILITADO`, queda en el outbox para reintento y se envía una alerta interna (RN-118).

Criterio de completitud:
Pasan los tests:
- con la tabla vacía, un adaptador real de prueba no envía nada y queda la alerta;
- con `dpa_vigente_hasta = 2026-11-13`, el 14/11/2026 tampoco envía;
- con `dpa_vigente_hasta = 2027-11-14`, envía;
- el adaptador simulado envía siempre;
- la migración no inserta filas.

---

## C2 · PWA — módulo operación (QA-C2-006)

### T-063: Crear las pantallas de lotes, temperatura y asignación del día
HU relacionada: HU-010, HU-011
Componente: C2 · PWA módulo operación
Prioridad: Must
Tamaño: L
Dependencias: T-002, T-031, T-056

Descripción:
Crear `frontend/src/operacion/` (carga diferida, solo para operador y administrador titular) con tres pantallas:
- **Registro de lote:** litros recibidos y temperatura con exactamente 1 decimal.
- **Detalle del lote:** muestra su estado y, si está `en_revision`, la cuenta regresiva hasta `revision_vence_en` y el formulario de corrección con justificación. El administrador ve "Revertir"; el operador no.
- **Día:** la demanda, la propuesta de asignación editable (lote, repartidor y orden) y el saldo de botellas de cada cliente.

Los textos salen de `es-PE.json` y las fechas se muestran como `DD/MM/AAAA`.

Criterio de completitud:
Pasan los tests de componente:
- `3.52` no se puede enviar;
- un lote `en_revision` muestra la cuenta regresiva;
- el botón "Revertir" aparece solo con el rol `administrador_titular`;
- la asignación confirmada llama a `POST /operacion/dias/{fecha}/asignaciones`.

`axe-core` no reporta violaciones WCAG 2.1 AA.

---

### T-064: Crear las pantallas de feriados, reclamos, catálogo, precio y confirmaciones
HU relacionada: HU-012, HU-013, HU-014, HU-015, HU-017
Componente: C2 · PWA módulo operación
Prioridad: Must
Tamaño: L
Dependencias: T-002, T-015, T-052, T-054, T-055, T-060

Descripción:
Agregar a `frontend/src/operacion/` estas pantallas:
- **Feriados:** carga de feriados del año.
- **Bandeja de reclamos abiertos:** cada reclamo con su fecha límite en `DD/MM/AAAA`.
- **Fincas y productos:** con su botón de publicación.
- **Propuesta de precio.**
- **Confirmación doble** (contraseña, luego código), común al cambio de precio y a las anulaciones. Muestra los segundos que quedan del código y los intentos restantes.

Los montos se muestran como `S/ 1,250.50`.

Criterio de completitud:
Pasan los tests de componente:
- la bandeja ordena por fecha límite;
- publicar un producto sin DIGESA muestra `errores.catalogo.sin_digesa`;
- la confirmación muestra "Código vencido" tras un `409 RN-152`;
- el operador no ve la pantalla de anulaciones.

`axe-core` no reporta violaciones WCAG 2.1 AA.

---

## C1 · PWA — módulo cliente (QA-C2-001, 003 y 007)

### T-065: Crear las pantallas de elección de devolución, facturación y comprobantes
HU relacionada: HU-008, HU-009
Componente: C1 · PWA módulo cliente
Prioridad: Must
Tamaño: M
Dependencias: T-002, T-046, T-061

Descripción:
Crear `frontend/src/cliente/` con tres pantallas:
- **Importe a devolver:** muestra el monto, el plazo restante de las 72 h y las opciones "Reembolso" y "Saldo a favor" (RN-129).
- **Datos de facturación:** boleta o factura, con el RUC.
- **Mis comprobantes:** los montos negativos se muestran como `-S/ 43.50` (RN-110).

Criterio de completitud:
Pasan los tests de componente:
- elegir "Saldo a favor" llama a la API y muestra el saldo nuevo;
- el RUC `2012345678` muestra el error de RN-058;
- una nota de crédito de `"-43.50"` se muestra como `-S/ 43.50`.

`axe-core` no reporta violaciones WCAG 2.1 AA.

---

### T-066: Crear las pantallas de modificar o cancelar la suscripción y de baja de cuenta
HU relacionada: HU-002, HU-006
Componente: C1 · PWA módulo cliente
Prioridad: Must
Tamaño: M
Dependencias: T-002, T-050, T-059

Descripción:
Crear dos pantallas:
- **Mi suscripción:** cambiar litros, día (solo martes, jueves o sábado) o dirección, y cancelar. Antes de confirmar muestra desde qué entrega aplica el cambio, según el corte de las 18:00:00.
- **Baja de cuenta:** muestra los pendientes que la bloquean, con el mensaje de la API, y avisa que el saldo a favor se reembolsa.

Criterio de completitud:
Pasan los tests de componente:
- un cambio hecho después del corte indica la entrega siguiente;
- un `409` de la baja muestra "Tienes una entrega en curso";
- no se ofrece el lunes ni el domingo.

`axe-core` no reporta violaciones WCAG 2.1 AA.

---

## Infraestructura · despliegue y operación (QA-C2-016)

### T-067: Implementar el monitoreo del worker y del outbox
HU relacionada: Infraestructura
Componente: C7 · Worker de tareas programadas
Prioridad: Must
Tamaño: M
Dependencias: T-018, T-022, T-029

Descripción:
Crear `backend/src/worker/tareas/vigilancia.js`, que corre cada 60 s y envía con `alertarInterno` una alerta crítica en estos casos (RN-106 y RN-150):
- hay tareas en estado `agotada` sin revisar;
- hay mensajes del outbox con más de 300 s de atraso respecto de `no_antes_de`;
- el corte de un día de reparto no se ejecutó a las 18:05:00 en Lima.

Exponer `GET /api/v1/salud/worker` con la última ejecución de cada tarea, sin datos personales.

Criterio de completitud:
Pasan los tests con el reloj inyectado:
- sin corte a las 18:05:00 del 13/11/2026, hay una alerta;
- con el corte hecho a las 18:00:02, no la hay;
- un mensaje del outbox atrasado 301 s genera una alerta y uno atrasado 299 s, no.

---

### T-068: Configurar los entornos y el despliegue continuo a staging
HU relacionada: Infraestructura
Componente: C5 · API REST + C8
Prioridad: Must
Tamaño: M
Dependencias: T-004 · ⛔ G3

Descripción:
Agregar `.github/workflows/despliegue.yml`, que en cada merge a `main` despliega la API, el worker y la PWA en `staging` en el hosting elegido en G3. Primero aplica las migraciones aditivas y después el código. El pase a `produccion` queda como un *job* manual, que solo aprueba un humano. Documentar los entornos de la §6.6 en `docs/configuracion-entorno.md`, solo con nombres de variables.

Criterio de completitud:
Un merge de prueba despliega en `staging` y `GET /api/v1/salud` responde 200 allí; el *job* de `produccion` no corre sin la aprobación manual.

---

### T-069: Configurar el proxy con TLS 1.3 y la prueba de humo
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: S
Dependencias: T-068 · ⛔ G3

Descripción:
Configurar el proxy inverso del hosting para que acepte solo TLS 1.3 (PRINCIPIO-003) y agregar a `despliegue.yml` una prueba de humo con `openssl s_client` (o la herramienta del *runner*) que intente TLS 1.2 contra `staging`.

Criterio de completitud:
La prueba de humo falla si `staging` acepta TLS 1.2 y pasa cuando solo acepta TLS 1.3.

---

### T-070: Configurar el respaldo diario y probar la restauración
HU relacionada: Infraestructura
Componente: C8 · Base de datos
Prioridad: Must
Tamaño: M
Dependencias: T-068 · ⛔ G3

Descripción:
Configurar el respaldo diario cifrado de PostgreSQL en el hosting elegido, con la retención que defina un humano según la obligación de SUNAT de conservar comprobantes (RN-111). Escribir `docs/runbook-restauracion.md`, con el procedimiento de restauración, y un *job* mensual que restaure el último respaldo en una base temporal de `staging` y cuente las filas de `comprobante`. Ese *job* no toca `produccion`.

Criterio de completitud:
La restauración de prueba termina sin errores y el conteo de `comprobante` coincide con el del respaldo; el *runbook* existe y lo revisó un humano.

---

## C5 · API REST — contrato (QA-C2-014)

### T-071: Completar el contrato OpenAPI con los endpoints de la §4.12 y la §4.13
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: M
Dependencias: T-003

Descripción:
Agregar a `docs/api/openapi.yaml` estos endpoints:
- los de la §4.12 y la §4.13 de la arquitectura que implementa esta iteración: sesiones (renovar y cerrar), reactivación, baja, modificación y cancelación, comprobantes, facturación, feriados (`POST` y `GET`), fincas, productos, lotes, asignaciones, salidas, precio, anulaciones, confirmaciones, reclamos del operador y elección de importe;
- `GET /api/v1/salud` y `GET /api/v1/salud/worker`.

Usar las mismas convenciones que T-003. `POST /operacion/reclamos/{codigo}/respuesta` (HU-016) se incluye con `x-pendiente: asesor-legal`.

Criterio de completitud:
Cada ruta que registran T-001, T-013, T-015, T-025, T-032, T-046, T-050, T-052 a T-057, T-059 a T-061 y T-067 existe en `openapi.yaml`, y `@redocly/cli lint` pasa.

---

# MUST · Agregadas en v0.4.0 (subsanación de la validación del prototipo)

Estas tareas resuelven las observaciones de [`10-validation-log.md`](10-validation-log.md) con `03-reglas-de-negocio.md` v0.9.0, `07-architecture.md` v1.2.0 y `09-ui-spec.md` v0.2.0. Las reglas que implementan las aprobó el fundador el 14/11/2026 (G7 liberada); T-080 espera la validación del contador (G8).

## C6 · Módulos de dominio — `suscripciones` y `cuentas` (OBS-04, OBS-05 y OBS-18)

### T-072: Implementar las simulaciones de suscripción, pausa, reactivación, modificación y cancelación
HU relacionada: HU-004, HU-005, HU-006
Componente: C6 · Módulos de dominio (`suscripciones`)
Prioridad: Must
Tamaño: L
Dependencias: T-024, T-025, T-059

Descripción:
Crear `POST /suscripciones/simulacion` y `POST /suscripciones/{id}/simulaciones` según la §4.14 de la arquitectura (RN-184).
- Cada simulación llama a las **mismas** funciones de reglas que la acción real (`primeraEntrega`, `trasladarPorFeriado`, cálculo del importe y del saldo, RN-062, RN-083, RN-124), dentro de una transacción que siempre se revierte.
- La respuesta incluye `reembolso_saldo.medio`, el medio del ciclo que generó el saldo (OBS-29).
- Una suscripción `suspendida_por_pago` responde `409 RN-180` a la simulación de pausa o modificación.

Criterio de completitud:
Pasan los tests:
- para cada acción, la simulación y la acción confirmada en el mismo instante simulado devuelven las mismas fechas e importes (por ejemplo, el 13/11/2026 a las 19:00 → primera entrega 2026-11-21 e importe `"43.50"`);
- después de una simulación no cambió ninguna fila de la base (se cuentan las filas de `suscripcion`, `pausa`, `cambio_suscripcion` y `ciclo`);
- una pausa simulada el 13/11/2026 a las 10:00 con N = 2 devuelve `entregas_omitidas` 14/11 y 21/11 y `reanuda_el` 28/11.

---

### T-073: Implementar `GET /direcciones` y `POST /direcciones`
HU relacionada: HU-004, HU-006
Componente: C6 · Módulos de dominio (`cuentas`)
Prioridad: Must
Tamaño: M
Dependencias: T-010, T-013, T-016

Descripción:
Crear las rutas de la §4.14 para el cliente dueño. `POST /direcciones` valida el distrito (RN-005), cifra la línea, la referencia y las coordenadas (RN-006) y crea la dirección con `vigente = true`. `GET /direcciones` devuelve las direcciones vigentes del cliente, descifradas solo para él.

Criterio de completitud:
Pasan los tests: una dirección en Miraflores → `201`; una en Barranco → `422 RN-005`; las columnas `*_cifrado` no contienen el texto original; y otro cliente no ve esa dirección (`GET` devuelve solo las propias).

---

### T-074: Admitir varias suscripciones por cliente, con producto y medio de pago obligatorio
HU relacionada: HU-004, HU-007, HU-009
Componente: C6 · Módulos de dominio (`suscripciones` y `pagos`)
Prioridad: Must
Tamaño: L
Dependencias: T-024, T-026, T-052

Descripción:
Crear la migración aditiva `008_suscripcion_producto.sql`, que agrega `suscripcion.producto_id` con su clave foránea. Luego:
- `POST /suscripciones` exige un `producto_id` publicado (RN-161) y un medio de pago activo (`422 RN-179`), y permite más de una suscripción no cancelada por cliente (RN-160).
- Crear `GET /suscripciones`, con el `cobro_estimado` de cada próximo ciclo, el estado del último pago y la entrega reprogramada o cancelada, si la hay (§4.14).
- Modificar `cobrarCiclo` (T-026) para que aplique el saldo a favor por orden de instante de corte y, si coincide, por identificador de suscripción (RN-162).

Criterio de completitud:
Pasan los tests:
- un cliente crea una suscripción de los sábados y otra de los martes, y `GET /suscripciones` devuelve las dos;
- sin medio de pago activo → `422 RN-179`;
- con un producto no publicado → `422`;
- con S/ 14.50 de saldo, el ciclo del martes 17/11/2026 se cobra en S/ 14.50 y el del sábado 21/11/2026 en S/ 43.50;
- pausar una de las dos no cambia la otra.

---

## C6 · Módulos de dominio — `permisos` (OBS-08 y OBS-25)

### T-075: Implementar la recuperación de contraseña
HU relacionada: HU-018
Componente: C6 · Módulos de dominio (`permisos`)
Prioridad: Must
Tamaño: L
Dependencias: T-013, T-022, T-053

Descripción:
Crear la migración aditiva `009_recuperacion_contrasena.sql`, con la tabla de la §3.2, y las rutas `POST /sesiones/recuperacion` y `POST /sesiones/recuperacion/confirmacion` (§4.14).
- **Solicitud:** genera un token con `crypto.randomBytes(32)` y guarda solo su HMAC. Envía el correo CO-16 con `avisarCliente` o `alertarInterno`, según el tipo de usuario, y responde siempre `202`, tenga o no cuenta el correo (RN-165). Límite de 5 peticiones por hora por IP.
- **Confirmación:** compara en tiempo constante y rechaza con `410 RN-166` un token vencido (más de 1 800 s) o ya usado. Exige 10 caracteres como mínimo y guarda la contraseña con `argon2id`. Cierra todas las sesiones del usuario y cancela sus `confirmacion_doble` pendientes (RN-167), y registra el cambio en `auditoria`.

Criterio de completitud:
Pasan los tests de la HU-018:
- un correo con cuenta y uno sin cuenta reciben la misma respuesta `202`, y solo el primero genera un mensaje en el outbox;
- el token sirve a los 1 799 s y responde `410` a los 1 801 s;
- un segundo uso responde `410`;
- tras el cambio, el token de renovación anterior recibe `401` y la confirmación pendiente queda `cancelada`;
- el token no aparece en la base, en el outbox ni en los logs.

---

### T-090: Crear el comando de administración de usuarios internos
HU relacionada: HU-010, HU-011 (operación)
Componente: C6 · Módulos de dominio (`permisos`)
Prioridad: Must
Tamaño: S
Dependencias: T-010, T-013

Descripción:
Crear `backend/scripts/usuarios.js` (`npm run usuarios -- crear|rol|desactivar`) para que un humano dé de alta, cambie de rol o desactive usuarios internos (RN-176). El comando pide la contraseña inicial por la entrada estándar, sin eco y sin guardarla en el historial, cifra el nombre y el correo y registra cada acción en `auditoria` con `actor_tipo = 'consola'`. No hay ninguna ruta HTTP para esto.

Criterio de completitud:
Pasan los tests: el comando crea un repartidor que luego inicia sesión; `desactivar` hace que su siguiente `POST /sesiones` reciba `401`; cada acción deja una fila en `auditoria`; y ninguna ruta de Express permite crear usuarios internos (lo verifica un test sobre el *router*).

---

## C6 · Módulos de dominio — `pagos` (OBS-06)

### T-076: Avisar al cliente que apruebe el cobro de Yape o Plin
HU relacionada: HU-007
Componente: C6 · Módulos de dominio (`pagos`)
Prioridad: Must
Tamaño: M
Dependencias: T-022, T-026

Descripción:
En `cobrarCiclo` (T-026), cuando el medio es `yape` o `plin` y el resultado es `pendiente`, encolar la tarea `aviso_aprobacion_billetera`. La tarea envía en 60 s como máximo el correo CO-02 y deja el aviso para el banner de P-007, con el importe y la hora límite de RN-074 (RN-164). Si el pago se confirma antes, el banner desaparece. La validación real del flujo de aprobación es parte de T-021 (G5).

Criterio de completitud:
Pasan los tests con el adaptador simulado: un cobro con Yape iniciado el 13/11/2026 a las 18:00:00 deja en el outbox, con `no_antes_de` ≤ 18:01:00, un correo con el importe `"43.50"` y la hora límite 18:30; un cobro con tarjeta no genera ese aviso; y un pago confirmado a las 18:10 quita el aviso de `GET /suscripciones`.

---

## C6 · Módulos de dominio — `operacion` (OBS-01, OBS-02, OBS-03, OBS-10 y OBS-19)

### T-077: Implementar la recepción del día, el pedido por finca y la alerta de las 06:45
HU relacionada: HU-010, HU-009
Componente: C6 · Módulos de dominio (`operacion`) + C7
Prioridad: Must
Tamaño: L
Dependencias: T-018, T-030, T-031

Descripción:
Crear la migración aditiva `010_recepcion_dia.sql`, con `recepcion_dia` y `pedido_finca` (§3.2). Agregar:
- `PUT /operacion/dias/{fecha}/pedidos-finca`, que rechaza con `422 RN-159` los pedidos cuya suma no es igual a la demanda;
- `POST /operacion/dias/{fecha}/recepcion/cierre`.

Modificar `POST /operacion/lotes` (T-030):
- `litros_comprometidos` sale del pedido de la finca del lote;
- la falta de litros se informa como advertencia y **no** marca entregas como `fallida`.

La cobertura por antigüedad y las entregas `fallida` por escasez (RN-092 y RN-122) se aplican recién en el cierre de la recepción (RN-157). Crear la tarea `alerta_recepcion_abierta`, a las 06:45:00 de cada día de reparto (RN-158).

Criterio de completitud:
Pasan los tests de la HU-010:
- con una demanda de 100 L, un lote de 64 L a las 06:00 no crea ninguna entrega `fallida`;
- con otro lote de 30 L y el cierre a las 06:40, las entregas que suman 6 L quedan `fallida`, cubriendo por antigüedad (el ejemplo A, B y C de RN-122 sigue pasando, ahora en el cierre);
- pedidos de 70 L y 40 L → `422 RN-159`;
- sin cierre, a las 06:45:00 hay una alerta crítica en el outbox;
- cerrar dos veces → `409`.

---

### T-078: Registrar las lecturas altas en la puerta y la propuesta de K8 en la sincronización
HU relacionada: HU-011
Componente: C6 · Módulos de dominio (`operacion`)
Prioridad: Must
Tamaño: M
Dependencias: T-032, T-058

Descripción:
Extender `POST /reparto/cierres` (T-032) para aceptar elementos `tipo: "lectura_alta"` (§4.9 de la arquitectura).
- **Lectura alta:** guarda la lectura en `lectura_temperatura` con origen `entrega`, deja la entrega `en_ruta` con `retenida_desde`, pasa el lote a `en_revision` (RN-025) y deduplica por `evento_id` (RN-156).
- **Al revertir el lote** (T-031), las entregas se liberan (`retenida_desde = NULL`). **Al invalidarse**, pasan a `suspendida` (RN-077).
- **Cierre `entregada` registrado sin conexión** después del paso de su lote a `en_revision`: queda `aplicado` con `entregada_durante_revision = true` y una alerta crítica (RN-185, K8). 

Este cambio reemplaza el `test.todo` de K8 de T-032.

Criterio de completitud:
Pasan los tests de la HU-011:
- una lectura de 5.1 °C a las 08:20:00 deja la entrega `en_ruta` retenida y el lote `en_revision`;
- la misma lectura enviada dos veces se registra una sola vez;
- tras la reversión, la entrega se puede cerrar como `entregada`;
- un cierre `entregada` con `registrado_en` 08:25 de un lote en revisión desde las 08:20 queda `aplicado` con la marca y con una alerta crítica en el outbox.

---

### T-079: Implementar la hoja de ruta imprimible con auditoría
HU relacionada: HU-010, HU-011
Componente: C6 · Módulos de dominio (`operacion`)
Prioridad: Must
Tamaño: M
Dependencias: T-010, T-056

Descripción:
Crear la tabla `impresion_hoja_ruta`, en la migración aditiva `011_hoja_ruta.sql`, y `GET /operacion/dias/{fecha}/hoja-ruta?repartidor_id=`, solo para operador y administrador titular (RN-155).
- Devuelve el orden, el número de entrega, los litros, la dirección y la referencia descifradas, con `Cache-Control: no-store`, y registra la impresión.
- Responde `409` si la asignación del día no está confirmada y `403` al rol `repartidor`.
- No usa ninguna librería de PDF: la vista de impresión es de la PWA (T-087).

Criterio de completitud:
Pasan los tests:
- con la asignación confirmada, la hoja de Juan tiene sus 12 entregas en orden y ninguna de otro repartidor;
- la respuesta trae `Cache-Control: no-store`;
- cada llamada deja una fila en `impresion_hoja_ruta` y en `auditoria`;
- un repartidor recibe `403`;
- sin asignación confirmada → `409`;
- ni el nombre completo ni el teléfono del cliente aparecen en la respuesta.

---

## C6 · Módulos de dominio — `auditoria`, `calendario` y `notificaciones` (OBS-09, OBS-11, OBS-14 y OBS-21)

### T-080: Aplicar los efectos de cada tipo de anulación y su simulación
HU relacionada: HU-014
Componente: C6 · Módulos de dominio (`auditoria`)
Prioridad: Must
Tamaño: L
Dependencias: T-045, T-048, T-055, T-059 · ⛔ G8 (validación del contador)

Descripción:
Extender los manejadores de anulación de T-055 con los efectos de RN-168 a RN-172 (aprobadas por el fundador; ⚠️ falta el contador, G8):
- **Pago:** reembolso íntegro, nota de crédito y cancelación de su entrega `programada`.
- **Comprobante:** comunicación al OSE/PSE. Queda como `test.todo` hasta G4.
- **Suscripción:** los efectos de una cancelación en ese instante, más el reembolso del saldo.
- **Entrega `programada`:** pasa a `cancelada`, con reembolso sin elección. Cualquier otro estado → `409 RN-171`.
- **Aviso:** correo CO-13 al cliente en 300 s como máximo.

Crear `POST /operacion/anulaciones/simulacion`, que devuelve la lista de efectos que muestra P-036, sin ejecutar nada.

Criterio de completitud:
Pasan los tests de la HU-014:
- anular el pago 5521 confirmado de S/ 43.50 solicita el reembolso en 3 600 s, emite la nota de crédito de B001-00000123, cancela la entrega `programada` del 14/11/2026 y encola el aviso CO-13;
- anular una entrega `entregada` → `409 RN-171`;
- la simulación devuelve los mismos efectos que luego ejecuta la anulación, y no cambia ninguna fila.

---

### T-081: Implementar la simulación y el retiro de feriados, la carga tardía y la alerta anual
HU relacionada: HU-015
Componente: C6 · Módulos de dominio (`calendario`) + C7
Prioridad: Must
Tamaño: M
Dependencias: T-015, T-022, T-029

Descripción:
Agregar estos cuatro cambios:
- **Simulación:** `POST /operacion/feriados/simulacion`, que devuelve cuántas entregas se trasladan y a qué fecha.
- **Rechazo de la carga tardía:** `POST /operacion/feriados` rechaza con `422 RN-173` una fecha cuyo corte ya ocurrió.
- **Retiro:** `POST /operacion/feriados/{fecha}/retiro`, solo para el administrador titular. Marca `retirado_en` (no borra la fila), devuelve las entregas trasladadas a su fecha original, encola el correo CO-14 a sus clientes y registra el retiro en `auditoria` (RN-174).
- **Alerta anual:** la tarea `alerta_calendario_anual` corre a diario desde el 01/12 a las 08:00 mientras no haya feriados del año siguiente (RN-175). Reemplaza la propuesta ❓ de la HU-015.

Criterio de completitud:
Pasan los tests de la HU-015:
- cargar el 08/12/2026 el lunes 07/12/2026 a las 19:00 → `422 RN-173`;
- retirar el 15/12/2026 el 10/12/2026 devuelve sus 14 entregas al 15/12 y encola 14 avisos;
- el operador recibe `403` al retirar;
- el 01/12/2026 a las 08:00, sin feriados de 2027, hay una alerta, y con el 01/01/2027 cargado, no.

---

### T-082: Dar estado a las alertas internas y agregar los reintentos manuales
HU relacionada: HU-008, HU-009, HU-010, HU-012
Componente: C6 · Módulos de dominio (`notificaciones` y `operacion`)
Prioridad: Must
Tamaño: M
Dependencias: T-022, T-028, T-045

Descripción:
Crear la tabla `alerta_interna`, en la migración aditiva `012_alerta_interna.sql`, y hacer que `alertarInterno` la llene junto con su mensaje del outbox. Las alertas de RN-106 nacen con nivel `critica` (OBS-11). Agregar estas rutas para operador y administrador titular:
- `GET /operacion/alertas?estado=`;
- `POST /operacion/alertas/{id}/resolucion`, con una nota obligatoria;
- `POST /operacion/comprobantes/{id}/reintento` y `POST /operacion/reembolsos/{id}/reintento`, que vuelven a encolar la tarea `fallida` con el backoff de T-018.

Criterio de completitud:
Pasan los tests:
- 5 fallos de emisión crean una alerta `critica`;
- resolverla sin nota → `400`;
- con nota, pasa a resuelta con usuario y hora;
- el reintento manual de un comprobante `fallido` encola `emitir_comprobante`, y si tiene éxito queda una sola boleta;
- un repartidor recibe `403`.

---

### T-083: Avisar al cliente de la reprogramación y de la cancelación por reprogramación agotada
HU relacionada: HU-011, HU-009
Componente: C6 · Módulos de dominio (`operacion` y `notificaciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-022, T-049

Descripción:
En `POST /reparto/cierres` (T-032 y T-049):
- **Reprogramación** (RN-066): encolar en 300 s como máximo el correo CO-06 y el aviso en la PWA, con la nueva fecha y el aviso de que un segundo fallo la cancela (RN-182).
- **Cancelación por reprogramación agotada** (RN-107 y RN-108): encolar el correo CO-08 y el aviso en la PWA (RN-183).

Los avisos quedan disponibles en `GET /suscripciones` (T-074) y en `GET /operacion/dias/{fecha}/entregas` (la marca «Reprogramada · último intento»).

Criterio de completitud:
Pasan los tests:
- una falla `imputable_cliente` a las 08:10 del 14/11/2026 encola CO-06 con `no_antes_de` ≤ 08:15 y la fecha 2026-11-17;
- la segunda falla encola CO-08 con el importe `"43.50"`;
- la entrega reprogramada figura con `ultimo_intento: true` en la lista del repartidor.

---

### T-084: Implementar las reglas propuestas de catálogo, precio y suscripción (SR-04, SR-07 y SR-10)
HU relacionada: HU-003, HU-005, HU-009, HU-013, HU-017
Componente: C6 · Módulos de dominio (`catalogo`, `configuracion`, `suscripciones` y `devoluciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-017, T-025, T-046, T-052, T-054

Descripción:
Implementar las reglas aprobadas el 14/11/2026:
- **RN-163:** la despublicación responde `409` con `suscripciones_activas` si el producto tiene suscripciones no canceladas.
- **RN-177:** una sola propuesta de precio pendiente; la cancelada pasa a `descartada`.
- **RN-178:** `GET /catalogo` sin productos devuelve una lista vacía con `clave_mensaje: "catalogo.vacio"`.
- **RN-180:** una suscripción `suspendida_por_pago` no se pausa ni se modifica. Reemplaza el `test.todo` SR-07 de T-025.
- **RN-181:** una elección después de las 72 h responde `409`. Reemplaza el `test.todo` SR-10 de T-046.

Criterio de completitud:
Pasan los tests:
- despublicar un producto con 40 suscripciones → `409 RN-163` con `40`;
- una segunda propuesta de precio pendiente → `409 RN-177`;
- un código vencido deja la propuesta `descartada`;
- el catálogo vacío responde `200` con lista vacía;
- pausar una suscripción suspendida → `409 RN-180`;
- elegir a las 72 h y 1 min → `409 RN-181`.

---

## C10 · Adaptadores externos (OBS-16)

### T-085: Crear las plantillas de correo CO-01 a CO-16
HU relacionada: HU-007, HU-008, HU-009, HU-011, HU-012, HU-013, HU-014, HU-015, HU-018
Componente: C10 · Adaptadores externos (`notificaciones`)
Prioridad: Must
Tamaño: M
Dependencias: T-022

Descripción:
Crear `backend/src/modulos/notificaciones/plantillas/`, con una función por plantilla de la §8 de la UI spec que arma el asunto y el cuerpo a partir de claves de `es-PE.json` (`correo.*`) y de datos del outbox, que solo traen identificadores y montos. Los textos legales quedan como claves con el valor `[PENDIENTE DE REDACCIÓN HUMANA]` (PRINCIPIO-008.9). CO-11 (respuesta al reclamo) y CO-15 (cambio de precio) quedan como `test.todo` por el asesor legal y por P14.

Criterio de completitud:
Pasan los tests: cada plantilla se renderiza con datos sintéticos; ninguna contiene un correo, un teléfono ni una dirección (RN-007); todos sus textos salen de claves existentes en `es-PE.json` (RN-041); y los montos aparecen como `S/ 43.50`.

---

## C1, C2 y C3 · PWA (subsanación de la UI, `09-ui-spec.md` v0.2.0)

### T-086: Actualizar las pantallas del cliente: varias suscripciones, vistas previas, dirección y recuperación
HU relacionada: HU-001, HU-002, HU-004 a HU-009, HU-018
Componente: C1 · PWA módulo cliente
Prioridad: Must
Tamaño: L
Dependencias: T-035, T-065, T-066, T-072, T-073, T-074, T-075, T-076

Descripción:
Implementar en `frontend/src/cliente/` los cambios de la UI spec v0.2.0:
- **P-005:** elección de producto, vista previa con la simulación (componente E-13) y `422 RN-179`.
- **P-007:** tarjetas por suscripción y la tabla de banners (pago pendiente, en verificación, pago tardío, reprogramación, cancelación, factura sin RUC y cobro neto).
- **P-008 a P-010:** las simulaciones.
- **P-017:** agregar dirección.
- **P-018:** recuperar contraseña, con el token quitado de la barra de direcciones.
- **P-003:** el RUC al elegir factura y el parámetro `?volver=`, que solo acepta rutas internas.
- **P-014:** el 409 con los 4 bloqueos y su fecha de liberación.

Criterio de completitud:
Pasan los tests de componente:
- P-007 muestra dos tarjetas con su cobro neto (`S/ 29.00 (S/ 43.50 − S/ 14.50 de saldo a favor)`);
- el banner de Yape aparece con un pago `pendiente`;
- P-005 no calcula fechas: con la simulación simulada (*mock*) en `2026-11-21` muestra `21/11/2026` aunque el reloj del navegador diga otra cosa;
- `?volver=https://otro.sitio` se ignora;
- P-018 borra el token de `location`.

`axe-core` no reporta violaciones WCAG 2.1 AA.

---

### T-087: Actualizar las pantallas de operación: recepción, hoja de ruta, alertas, feriados y anulaciones
HU relacionada: HU-010, HU-011, HU-013, HU-014, HU-015, HU-017
Componente: C2 · PWA módulo operación
Prioridad: Must
Tamaño: L
Dependencias: T-063, T-064, T-077, T-079, T-080, T-081, T-082, T-084

Descripción:
Implementar en `frontend/src/operacion/` los cambios de la UI spec v0.2.0:
- **P-030:** pedido por finca, la advertencia de escasez y «Cerrar recepción del día» con E-08.
- **P-031:** lo comprometido de la finca.
- **P-032:** el resumen coherente.
- **P-033:** «Confirmar asignación» solo con la recepción cerrada, y «Imprimir hoja de ruta».
- **P-042:** la vista de impresión con `@media print` y `window.print()`, sin guardarla en la caché del Service Worker.
- **P-034:** el 409 RN-163.
- **P-035:** una propuesta pendiente, «descartada» y «rige desde».
- **P-036 y P-037:** la lista de efectos.
- **P-038:** la vista previa, el retiro y el 422 RN-173.
- **P-041:** nivel crítico, [Reintentar] y [Marcar resuelta…].

Criterio de completitud:
Pasan los tests de componente:
- con pedidos que suman 110 L para 100 L se muestra el error de RN-159;
- «Confirmar asignación» está deshabilitado con la recepción abierta;
- la ruta de P-042 no queda en la caché del Service Worker (lo verifica un test del SW);
- P-037 muestra los efectos de la simulación;
- [Marcar resuelta…] exige una nota;
- el operador no ve [Retirar] en P-038.

`axe-core` no reporta violaciones WCAG 2.1 AA.

---

### T-088: Actualizar las pantallas de reparto: parada de la hoja, lectura alta y avisos de sincronización
HU relacionada: HU-011
Componente: C3 · PWA módulo reparto (sin conexión)
Prioridad: Must
Tamaño: M
Dependencias: T-036, T-057, T-078

Descripción:
Implementar en `frontend/src/reparto/` los cambios de la UI spec v0.2.0:
- **P-020:** «Parada N», «Ver la parada N en tu hoja» sin señal y el diálogo de la hoja impresa antes de «Salir a ruta».
- **P-021:** la temperatura primero; con más de 4.5 °C, solo [Registrar temperatura alta], que guarda en la cola una lectura `lectura_alta`.
- **P-020 y P-021:** los estados «Retenida» y «Reprogramada · último intento».
- **P-022:** «Enviado con aviso» (RN-185) y [Avisar al operador] en los rechazos.

Los datos guardados en IndexedDB siguen la lista cerrada de RN-145. Este cambio reemplaza el `test.todo` de K9 de T-036.

Criterio de completitud:
Pasan los tests de componente:
- con 5.1 °C no se puede elegir «Entregada» ni «Fallida», y la cola recibe una `lectura_alta` con su `evento_id`;
- sin red, P-020 muestra «Ver la parada 1 en tu hoja» y ninguna dirección;
- un recorrido de IndexedDB solo encuentra las claves de la lista de RN-145;
- un ítem con `marca: "entregada_durante_revision"` se muestra como «Enviado con aviso».

---

## C5 · API REST — contrato

### T-089: Agregar al contrato OpenAPI los endpoints de la §4.14
HU relacionada: Infraestructura
Componente: C5 · API REST
Prioridad: Must
Tamaño: M
Dependencias: T-071

Descripción:
Agregar a `docs/api/openapi.yaml` cada ruta y cada cambio de la §4.14 de la arquitectura v1.2.0, con las convenciones de T-003. Las rutas de los efectos de anulación llevan `x-pendiente: G8`, y el retiro de un comprobante ante el OSE lleva `x-pendiente: G4`.

Criterio de completitud:
Cada ruta de la §4.14 existe en `openapi.yaml`, `@redocly/cli lint` pasa, y el script de T-004 no encuentra rutas de Express de T-072 a T-084 que falten en el contrato.

---

# SHOULD

### T-043: Agregar al CI el control de presupuesto de JavaScript de la PWA
HU relacionada: Infraestructura
Componente: C1 · PWA módulo cliente
Prioridad: Should
Tamaño: S
Dependencias: T-004

Descripción:
Agregar a `.github/workflows/ci.yml` un paso que mide el JavaScript comprimido con gzip de la carga inicial de la ruta `/` y falla si supera 170 KB (§6.4 de la arquitectura). La medición se hace con un script propio, `scripts/presupuesto-js.mjs`, que usa `node:zlib` y no agrega dependencias.

Criterio de completitud:
El CI falla en un PR que importa en `/` un archivo de 200 KB de JavaScript, y pasa sin ese archivo.

---

### T-044: Crear el script de datos sintéticos para desarrollo y pruebas
HU relacionada: Infraestructura (HU-004 a HU-011)
Componente: C8 · Base de datos
Prioridad: Should
Tamaño: M
Dependencias: T-009, T-010

Descripción:
Crear `backend/scripts/sembrar-desarrollo.js`, que **se niega a correr** si `NODE_ENV` es `production`. Crea con datos inventados y cifrados con `ProveedorClavesMemoria`: 2 fincas, 1 producto con DIGESA, 3 clientes (uno por distrito), suscripciones de martes, jueves y sábado, un repartidor, un operador y el administrador titular. `precio_litro = 14.50` se inserta **solo** en esta base de desarrollo. Esto permite trabajar las HU en paralelo, como sugiere el análisis INVEST.

Criterio de completitud:
El script deja los datos esperados en una base local vacía, y con `NODE_ENV=production` termina con código de salida distinto de 0 sin escribir nada.

---

# COULD
No hay tareas Could en esta iteración. Las funciones Could del alcance (F-C01 a F-C04) no están en la arquitectura del PMV, y generar tareas para ellas violaría la restricción de no salir del alcance.

---

## Fuera de esta iteración (Must para la iteración 2)
Desde la v0.3.0, todo lo que la v0.2.0 dejaba para la iteración 2 está en el backlog (T-045 a T-071). Quedan fuera solo las piezas que esperan una decisión o un proveedor:

| Pieza | HU | Espera |
|---|---|---|
| Respuesta al reclamo (`POST /operacion/reclamos/{codigo}/respuesta`, RN-146 a RN-149) | HU-016 | Validación del asesor legal y P24 (derivación) |
| Aviso de cambio de precio a los suscriptores (RN-103) | HU-013 | P14 |
| Adaptador real del OSE/PSE | HU-008 | G4 |
| Adaptadores reales de correo y Web Push | Todas | G6 |
