# Auditoría QA del Spec Pack — Muuvi
**Versión:** 1.0.0
**Fecha:** 12/11/2026
**Autor:** Agente QA (BMAD)
**Alcance revisado:** `00-constitucion.md` · `01-project-brief.md` · `02-alcance.md` v1.0.0 · `03-reglas-de-negocio.md` v1.0.0 (RN-001 a RN-068) · `04-glosario.md` v1.0.0
**Regla de esta auditoría:** se señala lo que falta o está mal; **no se inventan reglas de negocio**. Las sugerencias dicen *qué* hay que decidir, no *qué* decidir.

---

## Resumen
| Severidad | Hallazgos |
|---|---|
| 🔴 CRÍTICA | 7 |
| 🟠 ALTA | 22 |
| 🟡 MEDIA | 24 |
| 🟢 BAJA | 13 |
| **Total** | **66** |

**Puntaje de madurez: 3 / 5.** Ver la [sección 7](#7-puntaje-de-madurez).

**Las 5 acciones que más reducen el riesgo:**
1. Definir el **ciclo de vida de una entrega**: cuándo nace y qué estados tiene (QA-002).
2. Asegurar que un ciclo **no se cobre dos veces** y resolver los **pagos pendientes** (QA-003 y QA-004).
3. Decidir qué pasa con la leche ya entregada de un **lote invalidado en plena ruta** (QA-005).
4. Que un humano **apruebe o corrija las 9 reglas 📌 de cobro y reembolso** (QA-007).
5. Incluir los **ADR-002 y ADR-003** en el Spec Pack que recibe el agente (QA-001).

---

## 1. 🔴 Hallazgos CRÍTICOS

| ID | Tipo | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| QA-001 | Contradicción | `00-constitucion.md` PRINCIPIO-001 y 009 · RN-041 · `02-alcance.md` §5.1 | La constitución exige interfaz bilingüe y 2 aprobaciones por PR. RN-041 y el alcance dicen solo es-PE y 1 aprobación. Las excepciones viven en ADR-002 y ADR-003, que **no forman parte del Spec Pack** entregado. Un agente que lea solo el pack ve una contradicción y, según el PRINCIPIO-008, debe detenerse. | Incluir ADR-001, 002 y 003 en el pack. Citar el ADR en cada regla afectada (RN-041 ya cita ADR-002; falta una regla o nota para ADR-003). |
| QA-002 | Regla faltante / Ambigüedad | RN-012, RN-013, RN-047, RN-050, RN-066 · Glosario | **"Entrega" y "programar una entrega" no están definidos.** RN-013 dice "mantener programada", lo que implica que la entrega existe antes del corte. RN-047 dice "no programar" después del cobro, lo que implica que se crea después. No hay estados de entrega previos al cierre (¿`programada`, `en_ruta`, `reprogramada`, `cancelada`?). | Definir en qué momento se crea una entrega y su máquina de estados completa, en una regla y en un diagrama Mermaid como el de la suscripción. |
| QA-003 | Regla faltante | RN-045, RN-046 | Ninguna regla impide **cobrar dos veces el mismo ciclo**: si el proceso de las 18:00 se ejecuta dos veces, si la pasarela envía un webhook duplicado o si llega una confirmación tardía después del reintento. | Agregar una regla de unicidad: un ciclo tiene como máximo un cobro confirmado, con una clave de idempotencia por ciclo. |
| QA-004 | Regla faltante | RN-023, RN-046, RN-047 | Las reglas cubren el pago **confirmado** y el **rechazado**, pero no el **pendiente**. RN-023 lo menciona en un ejemplo: con Yape y Plin un pago puede quedar pendiente varios minutos. No se dice qué pasa con la entrega si el pago sigue pendiente al cerrar la lista, ni si lo que llega tarde se acepta. | Decidir cuánto tiempo puede esperar un pago pendiente y qué ocurre con la entrega al vencer ese plazo. |
| QA-005 | Regla faltante (sanitaria) | RN-025, RN-027, RN-054 | Si una lectura en ruta invalida el lote: (a) no se dice si **esa botella se entrega o no** al cliente; (b) no hay regla para las **entregas ya realizadas** con ese lote (aviso o retiro); (c) no se dice qué pasa con las entregas del mismo lote **que están en ruta** con otros repartidores. | Decidir el protocolo de lote invalidado en ruta: entrega o no de la botella, aviso a clientes ya servidos y suspensión de las entregas en curso. |
| QA-006 | Contradicción (legal) | RN-004 · PRINCIPIO-005.3 · Dependencias D1, D2 y D5 | El consentimiento para envío a terceros es separado y, por ser separado, parece opcional. Pero **Culqi, el OSE/PSE y el proveedor de correo reciben datos personales** para cobrar, emitir la boleta y enviar correos. Si el cliente no lo marca, no se sabe si puede pagar. | Que un asesor legal determine si esos proveedores son *encargados de tratamiento* (cubiertos por el consentimiento de tratamiento) o *terceros*, y escribir la regla resultante. |
| QA-007 | Contradicción (proceso) | RN-045, 046, 049, 055, 057, 061, 066, 067, 068 (📌) | Nueve reglas **MUST** de cobro, reembolso y operación son interpretaciones del agente que nadie ha aprobado. El PRINCIPIO-008.9 prohíbe que un agente implemente lógica de cobro sin revisión humana. | Aprobar o corregir cada 📌 y quitar la marca. Hasta entonces, marcar esas reglas como **bloqueadas para implementación**. |

---

## 2. 🟠 Hallazgos ALTOS

| ID | Tipo | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| QA-008 | Ambigüedad | RN-046 | El reintento también es un "cobro rechazado", así que podría volver a activar RN-046 y repetirse sin fin. | Limitar la regla al **primer** rechazo de cada ciclo. |
| QA-009 | Regla faltante | RN-013, RN-014 | Una **cancelación después del corte** mantiene la entrega del día siguiente (RN-013), pero ninguna regla dice que se cancelen las entregas **posteriores**. RN-014 solo cubre la cancelación antes del corte. | Escribir una regla para lo que pasa con las entregas futuras cuando la cancelación llega después del corte. |
| QA-010 | Contradicción | RN-008 frente a RN-032 y PRINCIPIO-008.2 | RN-008 obliga a un proceso automático a anonimizar datos en 30 días. RN-032 y el PRINCIPIO-008.2 exigen doble confirmación del administrador para alterar suscripciones, pagos y comprobantes. Un agente no sabe si anonimizar cuenta como "alterar". | Aclarar que anonimizar campos personales no es eliminar ni anular registros, o decidir que la anonimización también requiere confirmación. |
| QA-011 | Ambigüedad | RN-032 | No se define qué es la **"doble confirmación"**: dos clics, dos sesiones, una segunda contraseña o 2FA. | Definir el mecanismo en una regla verificable. |
| QA-012 | Regla faltante | RN-024, RN-025, RN-028 | La **temperatura no es obligatoria**: RN-028 exige litros, pero no temperatura. Sin lectura, el lote nunca se invalida y el KPI de cadena de frío al 100 % no se puede medir. Además, no se fijan la **precisión** (¿1 decimal?) ni la unidad. | Hacer obligatoria la temperatura al recibir el lote y al entregar, con su precisión. |
| QA-013 | Regla faltante | RN-024, RN-025 | Nada impide que alguien **corrija una temperatura ya registrada** para evitar que el lote se invalide. | Decidir si las lecturas son inmutables o si cada corrección deja rastro en la auditoría. |
| QA-014 | Regla faltante | S14 · RN-028 | No hay regla que calcule **cuántos litros pedir a la finca** después del corte, ni cómo se le comunica. "Litros comprometidos" aparece en RN-028 sin decir de dónde sale. | Escribir la regla de cálculo y de envío del pedido a la finca. |
| QA-015 | Regla faltante | RN-028 · Riesgo R2 | **Escasez:** si llegan menos litros de los necesarios, no se dice qué entregas se cubren ni con qué causa de fallo se marcan las que no. | Decidir el criterio para repartir la leche disponible y la causa que se asigna a las entregas sin cubrir. |
| QA-016 | Ambigüedad | RN-054 | La frase "no puede realizarse porque su lote fue invalidado" no dice si antes hay que **buscar otro lote** del día. El ejemplo dice "sin otro lote disponible", pero la regla no lo exige. | Decidir si existe una reasignación a otro lote y, si existe, convertirla en regla. |
| QA-017 | Contradicción | RN-021 | La regla dice que se guarde **únicamente** el token y los últimos 4 dígitos. Pero hace falta saber **qué medio** es (Culqi, Yape o Plin), y Yape y Plin no tienen "últimos 4 dígitos". | Permitir explícitamente el tipo de medio y definir qué se guarda para cada billetera. |
| QA-018 | Regla faltante | RN-056, RN-057 | **Saldo a favor:** no se cubre el saldo que es menor o mayor que el ciclo, el cliente que cancela con saldo pendiente, el cobro de S/ 0.00 (¿se llama a la pasarela?) ni la boleta de S/ 0.00. RN-057 tampoco dice si la nota de crédito se emite al **generar** el saldo o al **aplicarlo**. | Decidir cada caso y validar el tratamiento tributario con el contador. |
| QA-019 | Ambigüedad | RN-062 | **"Pausa de N semanas"**: no se dice si equivale a N entregas omitidas o a N × 7 días desde que se pausa, ni cuándo exactamente se considera terminado el plazo respecto del corte. | Definir la pausa en número de entregas o en una fecha de fin, con una hora exacta. |
| QA-020 | Regla faltante | F-M03 | **Alta de la suscripción:** si un cliente se suscribe el viernes a las 19:00, no se dice cuándo es su primera entrega ni cuándo se hace el primer cobro. | Escribir la regla de primera entrega y primer cobro según la hora de alta. |
| QA-021 | Regla faltante | F-M03 | **Modificar una suscripción:** no hay reglas para cambiar los litros, el día o la dirección, ni para fijar a partir de qué ciclo aplica el cambio. | Decidir si se puede modificar y con qué corte. |
| QA-022 | Regla faltante | RN-008 | **Baja de cuenta con pendientes:** no se cubre la baja con una suscripción activa, una entrega ya programada, saldo de botellas, saldo a favor o un reclamo abierto. | Decidir el orden de cierre antes de anonimizar. |
| QA-023 | Regla faltante | Todo el pack | **No hay reglas de autenticación ni de autorización:** inicio de sesión, quién puede configurar el precio (RN-011), quién marca entregas o quién anula registros. | Escribir una matriz de permisos por rol (Cliente, Operador, Repartidor, Administrador titular). |
| QA-024 | Regla faltante | RN-061 | **Feriados en días de reparto:** el martes 08/12/2026 es feriado nacional (Inmaculada Concepción) y no se dice si hay reparto. | Decidir la política de feriados y la fuente de la lista de feriados. |
| QA-025 | Regla faltante | RN-011 · PRINCIPIO-008.9 | **Cambio de precio:** no se dice desde qué ciclo se aplica a las suscripciones vigentes, si se avisa al cliente ni si queda auditado. Además, la regla tiene el valor S/ 14.50 escrito dentro, así que un cambio legítimo de precio rompería el test. | Separar el valor (que va a configuración) de la regla, y decidir cómo entra en vigencia un precio nuevo. |
| QA-026 | Regla faltante | RN-022, RN-053, RN-055 | **Fallas de terceros:** no se dice qué pasa si el OSE/PSE no emite el comprobante o si la pasarela rechaza el reembolso. | Decidir los reintentos y las alertas para cada caso. |
| QA-027 | Regla faltante | RN-066 · P10 | Queda abierto qué pasa si **la entrega reprogramada vuelve a fallar**, lo que permite reprogramaciones sin fin. Tampoco se dice qué pasa si la suscripción está pausada, cancelada o suspendida el día de la reprogramación. | Responder P10 y escribir la regla para esos estados. |
| QA-028 | Regla faltante (legal) | RN-033, RN-034 | No se dice si el Libro de Reclamaciones exige **tener cuenta**; la norma protege a cualquier consumidor. Tampoco hay regla para **registrar y enviar la respuesta**. | Verificar con el asesor legal el acceso sin cuenta y escribir la regla de respuesta. |
| QA-029 | Inconsistencia de glosario | Brief §5.1 · Glosario | **"Suscriptor activo"**, el KPI principal del PMV, no está definido: no se sabe si cuentan las suscripciones pausadas o `suspendida_por_pago`. | Definir el término en el glosario con su fórmula. |

---

## 3. 🟡 Hallazgos MEDIOS

| ID | Tipo | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| QA-030 | Ambigüedad | RN-005 | No se dice **cómo se determina el distrito**: si el cliente lo elige de una lista, si se geocodifica la dirección o qué pasa con las direcciones en el límite entre distritos. | Fijar el método. |
| QA-031 | Formato | RN-007 | La regla mezcla comportamiento en ejecución ("rechazar el registro") con una condición de test. Además, detectar datos personales en ejecución no es verificable tal como está escrita. | Separarla en una regla de diseño (qué campos puede registrar un log) y un test que la verifique. |
| QA-032 | Regla faltante | RN-009 | El registro DIGESA no tiene **vigencia**, a diferencia de SENASA en RN-010. | Decidir si el registro DIGESA tiene vencimiento y escribir la regla equivalente a RN-010. |
| QA-033 | Ambigüedad | RN-019 | La regla mezcla una **pasarela** (Culqi) con **billeteras** (Yape y Plin), así que si Yape se procesa a través de Culqi la validación es ambigua. Además, "durante el PMV" no tiene un fin verificable. | Separar "medio de pago" de "pasarela" y usar un parámetro habilitado por ADR en lugar de "durante el PMV". |
| QA-034 | Contradicción | Glosario "PMV" frente a ADR-002 "Vigencia" | El glosario define el PMV como "hasta 50 suscriptores"; el ADR-002 dice "más de 50 suscriptores **o** mes 3". | Usar una sola definición. |
| QA-035 | Ambigüedad | RN-026 | No se dice **por qué canal** llega la alerta crítica: panel, correo o teléfono. Si un lote se invalida a las 05:00 y el operador no tiene el panel abierto, la alerta no sirve. | Definir el canal y la confirmación de lectura. |
| QA-036 | Ambigüedad | RN-025 | No se dice **qué se mide**: la botella, la caja térmica o el vehículo. | Definir el punto de medición. |
| QA-037 | Ambigüedad | RN-035, RN-036 | No se dice si el plazo se cuenta desde el registro o desde el día siguiente (el ejemplo cuenta desde el siguiente), ni cuál es la fuente de los feriados. | Precisarlo en la regla. |
| QA-038 | Ambigüedad | RN-049 | Cada rechazo dispara un correo, así que el cobro y el reintento generan **2 correos**. | Decidir si se envía un correo por rechazo o uno por ciclo. |
| QA-039 | Regla faltante | RN-048, RN-050 | La suscripción `suspendida_por_pago` no tiene **fin**: puede quedar suspendida para siempre. | Decidir si se cancela automáticamente pasado un plazo. |
| QA-040 | Ambigüedad | RN-051 | No se dice si el nuevo medio se **valida** antes de reactivar, ni qué pasa si se registra después del corte. | Precisarlo. |
| QA-041 | Regla faltante | RN-058, RN-059 | La **factura** necesita razón social y domicilio fiscal. El RUC no valida su dígito verificador. Además, emitir facturas a empresas roza el **B2B**, que está fuera del alcance (F-W02). | Decidir qué datos tributarios se piden y si se permite facturar a empresas. |
| QA-042 | Regla faltante | RN-017 | Sin un máximo de litros, una suscripción puede superar la capacidad de la finca (supuesto S4). | Decidir si existe un tope por suscripción o por capacidad. |
| QA-043 | Inconsistencia de glosario | RN-044 · Glosario "Lote" | RN-044 usa la **fecha de ordeño**, pero el glosario y RN-028 no la incluyen entre los atributos del lote. Tampoco figuran la finca ni el producto. | Completar los atributos del lote. |
| QA-044 | Inconsistencia | RN-006 · Glosario "Dato personal" | El **DNI** debe cifrarse, pero ninguna regla dice si se recoge ni cuándo. | Decidir si se pide el DNI y, si se pide, en qué paso. |
| QA-045 | Ambigüedad | F-M02 · RN-010 | No se dice cómo se muestra la ficha de finca si un mismo producto viene de **varias fincas** (Cañete y Huaral). | Definir la relación entre producto y finca. |
| QA-046 | Contradicción | `02-alcance.md` §3 MVP y F-M03 frente a RN-061 y RN-012 | El alcance dice que el cliente recibe la leche "el día que elija" y puede pausar "hasta las 18:00". Las reglas limitan el reparto a martes, jueves y sábado y fijan el corte **antes** de las 18:00:00. | Actualizar el alcance. |
| QA-047 | Contradicción | Brief §4 y S3 frente a `02-alcance.md` §7 | El Brief sigue con el rango de S/ 13.50 – S/ 16.00 y S/ 3.50 – S/ 6.00 a la finca. Los precios decididos son S/ 14.50 y S/ 5.50. | Actualizar el Brief o marcarlo como histórico. |
| QA-048 | Supuesto sin evidencia | RN-053 · S13 | El reembolso a Yape o Plin y el cobro recurrente con billetera están "confirmados por el fundador", pero sin una prueba técnica que lo respalde. | Hacer una prueba con la pasarela antes de implementar. |
| QA-049 | Regla faltante | RN-030, RN-031, RN-065 | Faltan casos de botellas: **rotas o perdidas**, botellas **devueltas sin una entrega** (por ejemplo, tras cancelar) y qué se hace con la leche de una entrega fallida. | Decidir esos casos. |
| QA-050 | Regla faltante | RN-016 | No se dice si un cliente puede tener **varias suscripciones** para recibir leche más de un día por semana. | Decidirlo. |
| QA-051 | Formato | RN-008, RN-011, RN-013, RN-017, RN-034, RN-059, RN-068 | Son **reglas compuestas**: cada una tiene dos acciones o una aclaración aparte. Por ejemplo, RN-013 dice "pausa **o** cancela", RN-034 dice "asignar código **y** enviar constancia" y RN-068 dice "DEBE asignar **y** NO DEBE reutilizar". | Partir cada una en reglas atómicas. |
| QA-052 | Formato | RN-056 | Usa el patrón **Optional** ("DONDE … esté habilitada") para una **elección del cliente**, cuando ese patrón es para funcionalidades configurables. | Reescribirla como State-driven, por ejemplo "MIENTRAS el cliente tenga elegido saldo a favor…". |
| QA-053 | Inconsistencia de glosario | Glosario "Ciclo" | El glosario dice "una entrega y un cobro", pero una reprogramación deja 2 entregas y 1 cobro, y el saldo a favor genera un cobro de S/ 0.00. | Ajustar la definición. |

---

## 4. 🟢 Hallazgos BAJOS

| ID | Tipo | Ubicación | Problema | Sugerencia |
|---|---|---|---|---|
| QA-054 | Inconsistencia | `00-constitucion.md` PRINCIPIO-004 y 008.2 | El nombre aparece como "Muuuvi". | Corrección humana (el PRINCIPIO-008.10 impide que la haga un agente). |
| QA-055 | Inconsistencia | PRINCIPIO-011 y PRINCIPIO-004, ejemplos | Los ejemplos muestran la franja 07:00–09:00 (la real es 07:00–10:00) y una "suscripción mensual" (el MVP es semanal). | Corrección humana de los ejemplos. |
| QA-056 | Error en ejemplo | RN-013, contraejemplo | Dice "la finca ya despachó" el viernes a las 21:00, pero la finca sale hacia las 04:00 del sábado. | Corregir el ejemplo. |
| QA-057 | Caso borde | RN-001 | No se dice en qué zona horaria se calcula la edad ni qué pasa con quien nació un 29/02. | Precisarlo. |
| QA-058 | Formato | RN-033, ejemplo | El ejemplo añade "en todas las pantallas", que la regla no exige. | Alinear el ejemplo con la regla, o la regla con el ejemplo. |
| QA-059 | Formato | RN-040, ejemplo | El timestamp del ejemplo no tiene fecha (`13:15:00Z`). | Usar un ISO 8601 completo. |
| QA-060 | Caso borde | RN-043 | Un aviso postergado puede quedar **obsoleto**, por ejemplo "tu entrega llega en 1 hora" enviado al día siguiente. | Decidir si los avisos vencen. |
| QA-061 | Caso borde | RN-037 | No se define el formato de los **montos negativos**, que aparecen en notas de crédito y reembolsos. | Definirlo. |
| QA-062 | Ambigüedad | RN-034, RN-049 · PRINCIPIO-008.4 | La ventana de 09:00 a 20:00 nombra WhatsApp, SMS y push, pero no el **correo**. Una constancia enviada a las 23:00, ¿la viola? | Aclarar si la ventana aplica al correo. |
| QA-063 | Caso borde | RN-060 | Solo se cifran los RUC que empiezan con `10`; puede haber otros prefijos de persona natural. | Verificarlo con SUNAT o con el contador. |
| QA-064 | Formato | `03-reglas-de-negocio.md` | La numeración no sigue el orden de las secciones (RN-045 a 068 están intercaladas). | Aceptable mientras los IDs no cambien; agregar una tabla de IDs ordenada. |
| QA-065 | Inconsistencia | Spec Pack pegado frente al proyecto | En el glosario pegado, el término 47 aparece antes del 46 y el encabezado está cortado. En el proyecto ya está corregido. | Generar el pack siempre desde el proyecto. |
| QA-066 | Inconsistencia de nombres | `02-alcance.md` frente al glosario | El alcance usa "MVP" y el glosario usa "PMV". | Elegir un solo término. |

---

## 5. Inconsistencias de glosario (detalle)

### 5.1 Términos usados en las reglas pero no definidos
| Término | Dónde aparece | Severidad |
|---|---|---|
| **Entrega** y **programar una entrega** | RN-012 a RN-016, RN-047, RN-050, RN-066 | 🔴 (QA-002) |
| **Suscriptor activo** | KPI del Brief §5.1 | 🟠 (QA-029) |
| **Doble confirmación** | RN-032 | 🟠 (QA-011) |
| **Lista de entregas** | RN-027, RN-047 | 🟡 |
| **Litros comprometidos / recibidos** | RN-028 | 🟡 |
| **Fecha de ordeño** | RN-044 | 🟡 (QA-043) |
| **Producto** | RN-009 | 🟡 |
| **Medio de pago** | RN-019, RN-021, RN-051 | 🟡 |
| **Pago pendiente / rechazado** | RN-023, RN-046 | 🟡 (QA-004) |
| **Estado `activa`** de la suscripción | RN-045, RN-051, RN-062 | 🟢 |
| **Ficha de finca** | F-M02 | 🟢 |
| **Hoja de reclamación** y **respuesta al reclamo** | RN-034, RN-035 | 🟢 |

### 5.2 Términos definidos que no coinciden con su uso
| Término | Glosario dice | Las reglas dicen |
|---|---|---|
| **Cancelación** | "No se programan más entregas" | RN-013: tras un corte vencido se mantiene una entrega más. |
| **Pago confirmado** | "Única condición para emitir un comprobante" | RN-057: la nota de crédito es un comprobante y no nace de un pago confirmado. |
| **Ciclo** | "Una entrega y un cobro" | RN-066 y RN-067 (2 entregas, 1 cobro) y RN-056 (cobro de S/ 0.00). |
| **Cliente** | "Persona con cuenta activa" | El alcance dice "hogar suscrito". ¿Alguien registrado pero sin suscripción es cliente? |

### 5.3 Definidos en la constitución, sin término en el glosario
- **"Saldos de garantía de botellas"** (PRINCIPIO-008.3): no existe en el MVP porque F-S04 es Should. Conviene aclarar que no aplica.

---

## 6. Debilidades de formato (resumen)
| Tipo | Reglas |
|---|---|
| Compuestas (más de una idea) | RN-008, RN-011, RN-013, RN-017, RN-034, RN-059, RN-068 |
| Patrón EARS mal elegido | RN-056 (Optional usado para una elección del cliente) |
| Valor fijo dentro de la regla | RN-011 (S/ 14.50), RN-005 (distritos), RN-061 (días) → conviene pasarlos a configuración aprobada por un humano |
| Comportamiento en ejecución mezclado con condición de test | RN-007 |
| Interpretaciones sin aprobar (📌) en reglas MUST | RN-045, 046, 049, 055, 057, 061, 066, 067, 068 |
| Ejemplo que no coincide con la regla | RN-013, RN-033, RN-040 |

**Lo que está bien:** las 68 reglas tienen ID, categoría, prioridad, patrón EARS, ejemplo y contraejemplo con valores concretos. Las duraciones están en segundos, los montos en S/, los límites (4.5 °C, 18:00:00) tienen casos de borde, y las reglas legales citan su principio.

---

## 7. Puntaje de madurez

### **3 / 5: estructurado, pero no listo para implementación autónoma**

| Criterio | Estado |
|---|---|
| Formato EARS, IDs y trazabilidad a la constitución | ✅ Sólido |
| Ejemplos y contraejemplos con valores concretos | ✅ Sólido |
| Flujo feliz de registro, suscripción, cobro y entrega | ✅ Cubierto |
| Ciclo de vida de la entrega e idempotencia del cobro | ❌ Falta (QA-002 y QA-003) |
| Errores y casos borde de pagos, terceros y escasez | ❌ Incompleto (QA-004, 015 y 026) |
| Seguridad sanitaria en ruta | ❌ Falta (QA-005) |
| Autorización por rol | ❌ Falta (QA-023) |
| Decisiones humanas de cobro aprobadas | ⚠️ 9 pendientes (QA-007) |

**Para llegar a 4/5:** resolver los 7 críticos y al menos los ALTOS QA-008 a QA-016 y QA-023.
**Para llegar a 5/5:** resolver todos los ALTOS, unificar el glosario y partir las reglas compuestas.

---

## 8. Estado tras las decisiones del 14/11/2026 (actualizado con la aprobación de las reglas pendientes)
Las decisiones se incorporaron en `03-reglas-de-negocio.md` v0.3.0, `04-glosario.md` v0.3.0, `02-alcance.md` v0.3.1 y `docs/adr/README.md`.

| Estado | Significado | Hallazgos |
|---|---|---|
| ✅ | Resuelto | 33 |
| 🟨 | Resuelto, pero depende de un conflicto (K) o de una pregunta abierta (P) | 11 |
| ⏳ | Abierto | 22 |

| Hallazgo | Estado |
|---|---|
| QA-001 | ✅ |
| QA-002 | ✅ K4 resuelto |
| QA-003 | ✅ |
| QA-004 | 🟨 P11 |
| QA-005 | ✅ |
| QA-006 | ✅ encargados de tratamiento; RN-114 a RN-118 |
| QA-007 | ✅ aprobadas el 14/11/2026 |
| QA-008 | ✅ K2 resuelto |
| QA-009 | ✅ |
| QA-010 | ✅ |
| QA-011 | 🟨 P18 (K1 resuelto) |
| QA-012 | ✅ |
| QA-013 | 🟨 P17 (K3 resuelto) |
| QA-014 | ✅ |
| QA-015 | 🟨 P12 |
| QA-016 | ✅ |
| QA-017 | ✅ |
| QA-018 | 🟨 P15, P16 |
| QA-019 | ✅ |
| QA-020 | ✅ |
| QA-021 | ✅ |
| QA-022 | ✅ K5 resuelto: botellas y saldo no bloquean la baja |
| QA-023 | ✅ |
| QA-024 | 🟨 P13 |
| QA-025 | 🟨 P14 |
| QA-026 | ✅ |
| QA-027 | ✅ |
| QA-028 | ✅ |
| QA-029 | ✅ |
| QA-030 | ⏳ |
| QA-031 | ⏳ |
| QA-032 | ⏳ |
| QA-033 | 🟨 tipo de medio separado (RN-095) |
| QA-034 | ✅ |
| QA-035 | ⏳ |
| QA-036 | ⏳ |
| QA-037 | 🟨 feriados (RN-101) |
| QA-038 | ⏳ |
| QA-039 | ⏳ |
| QA-040 | ⏳ |
| QA-041 | ⏳ |
| QA-042 | ⏳ |
| QA-043 | ✅ |
| QA-044 | ⏳ |
| QA-045 | ⏳ |
| QA-046 | ✅ |
| QA-047 | ⏳ |
| QA-048 | 🟨 sandbox recomendado |
| QA-049 | ⏳ |
| QA-050 | ⏳ |
| QA-051 | 🟨 faltan RN-011 y RN-059 |
| QA-052 | ✅ |
| QA-053 | ✅ |
| QA-054 | ⏳ corrección humana |
| QA-055 | ⏳ corrección humana |
| QA-056 | ✅ |
| QA-057 | ⏳ |
| QA-058 | ✅ |
| QA-059 | ✅ |
| QA-060 | ⏳ |
| QA-061 | ✅ |
| QA-062 | ⏳ |
| QA-063 | ⏳ |
| QA-064 | ⏳ aceptado como está |
| QA-065 | ✅ |
| QA-066 | ✅ |

**Conflictos K1 a K5 (todos resueltos) y preguntas P11 a P20 (P19 sin efecto):** ver las secciones finales de `03-reglas-de-negocio.md`.

### Puntaje actualizado: **4 / 5**, sin hallazgos críticos abiertos
Los 7 hallazgos críticos quedaron resueltos el 14/11/2026: QA-006 con el dictamen sobre los encargados de tratamiento, QA-007 con la aprobación de las reglas pendientes y los conflictos K1 a K5 con tus confirmaciones. El Spec Pack ya puede guiar la implementación de los flujos principales.

**Para llegar a 5 / 5 falta:**
1. Responder P11 a P18: pago confirmado tarde, escasez, feriados, aviso de cambio de precio, saldo a favor al darse de baja, comprobante con saldo, corrección de temperatura y mecanismo de doble confirmación. Las reglas que dependen de estas preguntas siguen sin poder implementarse.
2. Resolver los hallazgos MEDIOS y BAJOS abiertos (⏳).
3. Firmar los DPA de cada encargado y registrarlos en `docs/proveedores-datos.md` (RN-118).
4. Validar en el sandbox de Culqi, Yape y Plin el cobro recurrente, la idempotencia y los reembolsos.
