# Glosario — Muuvi
**Versión:** 1.0.0 (borrador)
**Fecha:** 12/11/2026
**Autor:** Agente Business Analyst (BMAD)
**Uso:** Cuando un término de este glosario aparece en el código, en los documentos o en los tests, significa exactamente lo que dice aquí. El nombre en código sigue el PRINCIPIO-001 (español).

| # | Término | Nombre en código | Definición |
|---|---|---|---|
| 1 | **Cliente** | `cliente` | Persona mayor de 18 años, con cuenta y dirección de entrega en un distrito atendido, tenga o no una suscripción. No confundir con *suscriptor activo* (término 48). Quien registra un reclamo sin cuenta es un **consumidor**, no un cliente (RN-109). |
| 2 | **Operador** | `operador` | Persona de Muuvi que registra fincas, lotes y precios, arma la lista de entregas y atiende reclamos. |
| 3 | **Administrador titular** | `administrador_titular` | El fundador de Muuvi. Es el único que puede dar la doble confirmación para anular datos críticos (RN-032). |
| 4 | **Repartidor** | `repartidor` | Persona de Muuvi que hace la última milla y cierra cada entrega como "entregada" o "fallida". |
| 5 | **Finca proveedora** | `finca` | Productor de Cañete o Huaral que ordeña, enfría y envasa la leche en botellas de vidrio y la lleva al punto de acopio. No usa el sistema en el MVP. |
| 6 | **Punto de acopio** | `punto_acopio` | Local refrigerado de Muuvi en Lima donde se reciben los lotes antes de repartirlos. |
| 7 | **Lote** | `lote` | Conjunto de botellas de 1 L que una finca entrega en una fecha. Atributos: código (por ejemplo `L-2026-1114-01`), finca de procedencia, producto, fecha de ordeño, fecha de recepción, litros comprometidos (RN-091), litros recibidos y temperatura de recepción (RN-085). |
| 8 | **Lote invalidado** | `lote.estado = 'invalidado'` | Lote que pasó 900 s (15 minutos) `en_revision` sin que el administrador titular lo devolviera a `recibido` (RN-134). Sus botellas no pueden asignarse a entregas (RN-027). Es un estado definitivo. |
| 9 | **Cadena de frío** | `cadena_frio` | Mantener la leche a 4.5 °C o menos desde la recepción en el punto de acopio hasta la entrega al cliente. |
| 10 | **Alerta crítica** | `alerta_critica` | Aviso al operador que se genera cuando un lote se invalida (RN-026). |
| 11 | **Suscripción semanal** | `suscripcion` | Acuerdo por el que el cliente recibe una cantidad fija de litros en 1 día fijo cada semana, al precio por litro vigente. |
| 12 | **Ciclo** | `ciclo` | Cada semana de una suscripción. Tiene como máximo un cobro confirmado (RN-073), que puede ser menor o no existir si hay saldo a favor (RN-096 y RN-097), y una entrega, que puede generar como máximo una entrega reprogramada (RN-107). |
| 13 | **Hora de corte** | `hora_corte` | 18:00:00 del día previo a una entrega, en hora de Lima. Pausar o cancelar esa entrega solo es posible **antes** de ese instante, y en ese mismo instante se cobra el ciclo (RN-012 a RN-014 y RN-045). |
| 14 | **Franja de entrega** | `franja_entrega` | Rango horario en que se entrega la leche. En el PMV es único: de 07:00 a 10:00 (RN-015). |
| 15 | **Pausa** | `suscripcion.estado = 'pausada'` | Suspensión de las entregas que decide el propio cliente, sin cancelar la suscripción. Puede ser **con plazo** (N semanas, se reanuda sola al terminar; RN-062) o **indefinida** (hasta que el cliente la reactiva; RN-063 y RN-064). |
| 16 | **Cancelación** | `suscripcion.estado = 'cancelada'` | Fin definitivo de una suscripción. Si se pide antes del corte, no se crean más entregas (RN-014); si se pide después, se mantiene solo la entrega de ese ciclo (RN-081 y RN-082). |
| 17 | **Precio por litro vigente** | `precio_litro` | Valor configurado por el operador, que ningún proceso automático modifica. S/ 14.50 en el PMV (RN-011). |
| 18 | **Botella retornable** | `botella` | Envase de vidrio en que se entrega la leche y que el cliente devuelve al repartidor. |
| 19 | **Saldo de botellas** | `saldo_botellas` | Botellas entregadas menos botellas devueltas por un cliente. Nunca es negativo (RN-030 y RN-031). |
| 20 | **Módulo de pagos** | `pagos` | Único componente que se comunica con las pasarelas (Culqi, Yape y Plin en el MVP) mediante una interfaz común (PRINCIPIO-002). |
| 21 | **Token de pasarela** | `token_pasarela` | Identificador que devuelve la pasarela en lugar de los datos de la tarjeta. Es lo único que se guarda del medio de pago, junto con los 4 últimos dígitos (RN-021). |
| 22 | **Pago confirmado** | `pago.estado = 'confirmado'` | Pago que la pasarela aprobó, o pago con origen `saldo_favor` cuando el saldo cubre todo el ciclo (RN-125). Es la única condición para emitir una boleta o una factura (RN-022 y RN-023). La nota de crédito tiene otra condición (RN-057). Los otros estados de un pago son `pendiente` y `rechazado` (término 52). |
| 23 | **Comprobante electrónico** | `comprobante` | Boleta o factura electrónica SUNAT emitida por cada pago confirmado. |
| 24 | **OSE / PSE** | `proveedor_facturacion` | Operador o proveedor de servicios electrónicos autorizado por SUNAT, a través del cual Muuvi emite los comprobantes. |
| 25 | **Registro sanitario DIGESA** | `registro_digesa` | Autorización sanitaria obligatoria para publicar un producto en el catálogo (RN-009). |
| 26 | **Certificación SENASA vigente** | `certificacion_senasa` | Certificación de la finca con fecha de vencimiento igual o posterior a la fecha actual. Es la condición para mostrar "Orgánico" (RN-010). |
| 27 | **Distrito atendido** | `distrito_atendido` | Distrito con servicio de entrega. En el PMV: Miraflores, San Isidro y La Molina (RN-005). |
| 28 | **Consentimiento** | `consentimiento` | Aceptación expresa del cliente, en casillas separadas y sin premarcar. Hay tres: **principal** (obligatorio; tratamiento para ejecutar el servicio, incluidos los encargados de tratamiento; RN-003, RN-114 y RN-115), **envío a terceros** (opcional; RN-004 y RN-116) y **fines comerciales** (opcional, si se habilitan campañas; RN-117). |
| 29 | **Dato personal** | `dato_personal` | DNI, nombre completo, fecha de nacimiento, teléfono, correo, dirección exacta y coordenadas. Se guarda cifrado con AES-256-GCM (RN-006). |
| 30 | **Baja de cuenta** | `fecha_baja` | Solicitud del cliente para cerrar su cuenta. Inicia el plazo de 2 592 000 s (30 días) para anonimizar sus datos (RN-008). |
| 31 | **Anonimización** | `anonimizar()` | Reemplazo irreversible de los datos personales para que ya no identifiquen al cliente, conservando los comprobantes que exige SUNAT. |
| 32 | **ANPDP** | — | Autoridad Nacional de Protección de Datos Personales, ante la que se inscriben los bancos de datos de clientes (PRINCIPIO-005). |
| 33 | **Libro de Reclamaciones virtual** | `libro_reclamaciones` | Formulario en la PWA para registrar reclamos y quejas, conforme a la Ley 29571 (RN-033). |
| 34 | **Reclamo** | `reclamo.tipo = 'reclamo'` | Disconformidad del cliente con el producto o el servicio. Se responde en un máximo de 15 días hábiles (RN-035). |
| 35 | **Queja** | `reclamo.tipo = 'queja'` | Disconformidad del cliente con la atención recibida, no con el producto. Tiene el mismo plazo de respuesta que el reclamo. |
| 36 | **Día hábil** | `dia_habil` | Día de lunes a viernes que no es feriado nacional en el Perú. Se usa para calcular los plazos del Libro de Reclamaciones. |
| 37 | **Ventana de comunicación** | `ventana_comunicacion` | Horario permitido para enviar avisos al cliente: de 09:00 a 20:00, hora de Lima (PRINCIPIO-008.4, RN-042). |
| 38 | **PMV** | — | Producto mínimo viable. **Término único del proyecto**: "MVP" es su sinónimo en inglés y no se usa en documentos nuevos. Son los 6 Must de `02-alcance.md`. El PMV termina cuando ocurre lo primero entre (a) superar los 50 suscriptores activos y (b) cumplirse el mes 3 desde el lanzamiento, igual que la vigencia del ADR-002. |
| 39 | **Día de reparto** | `dia_reparto` | Día de la semana con entregas: martes, jueves y sábado en el PMV (RN-061). |
| 40 | **Suspendida por pago** | `suscripcion.estado = 'suspendida_por_pago'` | Estado en el que entra una suscripción cuando fallan el cobro y su reintento. No genera entregas hasta que el cliente registra un nuevo medio de pago (RN-048 a RN-051). A diferencia de la pausa, no la decide el cliente. |
| 41 | **Reintento de cobro** | `reintento_cobro` | Segundo intento automático de cobrar un ciclo, 60 s después del rechazo (RN-046). |
| 42 | **Causa de entrega fallida** | `entrega.causa_fallo` | Motivo obligatorio de una entrega "fallida": `imputable_muuvi` (error de la operación o lote invalidado) o `imputable_cliente` (RN-052). |
| 43 | **Reembolso** | `reembolso` | Devolución de un importe al medio de pago original. Ocurre cuando el cliente lo elige para un importe a devolver (RN-120), cuando no elige en 72 h (RN-130), o sin elección en los casos de RN-108 y RN-124. Se solicita en 3 600 s como máximo (RN-055). |
| 44 | **Saldo a favor** | `saldo_favor` | Importe que el cliente eligió conservar en lugar del reembolso y que se descuenta del cobro de su siguiente ciclo (RN-056). No confundir con la nota de crédito, que es el comprobante SUNAT. |
| 45 | **Nota de crédito electrónica** | `nota_credito` | Comprobante SUNAT que anula o reduce un comprobante emitido. Se emite por cada reembolso o saldo a favor (RN-057). |
| 46 | **RUC** | `ruc` | Registro Único de Contribuyentes, de 11 dígitos, que se exige para emitir factura (RN-058). El que empieza con `10` (persona natural) contiene el DNI y se guarda cifrado (RN-060). |
| 47 | **Entrega reprogramada** | `entrega.reprogramada_de` | Nueva entrega que se crea cuando una entrega falla por causa `imputable_cliente`. Va al siguiente día de reparto, no se cobra de nuevo y se arma con un lote del día nuevo (RN-066 a RN-068). |
| 48 | **Suscriptor activo** | — | Cliente con una suscripción en estado `activa` y sin pagos rechazados pendientes. **No** cuentan las suscripciones `pausada`, `suspendida_por_pago` ni `cancelada`. Es el KPI principal del PMV (Brief §5.1). |
| 49 | **Entrega** | `entrega` | Envío de los litros de un ciclo a la dirección del cliente, en un día de reparto y en la franja de 07:00 a 10:00. Se crea al cumplirse la hora de corte (RN-069) o al reprogramarse (RN-066). |
| 50 | **Programar una entrega** | — | Crear la entrega en estado `programada` (RN-069). Una entrega "no programada" es una que nunca se creó. |
| 51 | **Estados de entrega** | `entrega.estado` | `programada`, `en_ruta`, `entregada`, `fallida`, `reprogramada`, `cancelada` y `suspendida` (RN-070). Las transiciones permitidas están en el diagrama del ciclo de vida de `03-reglas-de-negocio.md`. |
| 52 | **Pago pendiente / rechazado** | `pago.estado` | *Pendiente*: la pasarela aún no confirma ni rechaza; con Yape o Plin tiene un plazo de 1 800 s (RN-074). *Rechazado*: la pasarela lo denegó (RN-046). |
| 53 | **Motivo de suspensión** | `entrega.motivo_suspension` | `pago` (RN-047 y RN-074) o `lote_invalidado` (RN-076 y RN-077). |
| 54 | **Clave de idempotencia** | `clave_idempotencia` | Identificador del ciclo más identificador del cliente. Impide cobrar dos veces el mismo ciclo (RN-072 y RN-073). |
| 55 | **Medio de pago** | `medio_pago` | Forma de pago del cliente. Su tipo es `tarjeta` (por Culqi), `yape` o `plin` (RN-095). De una tarjeta se guardan el token y los últimos 4 dígitos (RN-021); de una billetera, la referencia de la transacción (RN-094). |
| 56 | **Lista de entregas** | — | Entregas `programada` de un día de reparto, asignadas a lotes válidos, que el operador entrega a los repartidores. |
| 57 | **Litros demandados** | `litros_demandados` | Suma de los litros de las entregas `programada` de un día de reparto, calculada al cumplirse el corte (RN-090). Es lo que se pide a la finca. |
| 58 | **Falla de abastecimiento de la finca** | `motivo = 'falla_abastecimiento_finca'` | Motivo de una entrega `fallida` por llegar menos litros de los demandados. Su causa es `imputable_muuvi` y genera un importe a devolver (RN-092 y RN-053). |
| 59 | **Reasignación de lote** | — | Paso de las entregas `programada` de un lote invalidado a otro lote válido del mismo día (RN-093). |
| 60 | **Lectura de temperatura** | `lectura_temperatura` | Valor en °C con 1 decimal, tomado al recibir el lote o al entregar. No se sobrescribe (RN-088); una corrección es un registro nuevo con justificación (RN-089). |
| 61 | **Aviso de disculpa** | — | Correo a los clientes que ya recibieron leche de un lote que luego se invalidó (RN-078). |
| 62 | **Feriado nacional** | `feriado` | Día no laborable declarado por norma publicada en El Peruano, cargado por el operador en el calendario (RN-100). |
| 63 | **Doble confirmación** | — | Aprobación explícita y repetida del administrador titular antes de anular registros críticos o cambiar precios o límites de crédito (RN-032). ❓ El mecanismo exacto está pendiente (P18). |
| 64 | **Consumidor** | — | Cualquier persona que registra un reclamo o una queja, tenga o no cuenta (RN-109). |
| 65 | **Encargado de tratamiento** | `encargado_tratamiento` | Proveedor que trata datos personales por cuenta y bajo las instrucciones exclusivas de Muuvi, sin usarlos para fines propios. En el PMV: Culqi, Yape, Plin, el OSE/PSE y el proveedor de correo transaccional (QA-006). Está cubierto por el consentimiento principal. |
| 66 | **Tercero** | — | Quien recibe datos personales para usarlos en beneficio propio, como fines comerciales propios o un proveedor de IA. Requiere el consentimiento opcional de envío a terceros (RN-004). Un encargado de tratamiento **no** es un tercero. |
| 67 | **Contrato de encargo de tratamiento (DPA)** | — | Contrato entre Muuvi y un encargado de tratamiento que lo obliga a usar los datos solo para el servicio y con las medidas de seguridad exigidas. Se registra en `docs/proveedores-datos.md` (RN-118). |
| 68 | **Registro de proveedores de datos** | `docs/proveedores-datos.md` | Lista de los encargados de tratamiento y terceros que reciben datos personales, con el estado de su DPA. Ningún proveedor fuera de la lista recibe datos (RN-118; PRINCIPIO-005.4). |
| 69 | **Prioridad por antigüedad** | — | Orden en que se cubren las entregas cuando falta leche: primero las suscripciones creadas antes. Una entrega se cubre solo si alcanzan todos sus litros (RN-122). |
| 70 | **Traslado por feriado** | — | Paso de las entregas de un día de reparto que es feriado nacional al siguiente día de reparto que no lo sea, con corte y cobro el día previo a la nueva fecha (RN-123). |
| 71 | **Origen del pago** | `pago.origen` | De dónde sale el dinero de un pago confirmado: `pasarela` (tarjeta, Yape o Plin) o `saldo_favor` (RN-125). |
| 72 | **Elección ante pago tardío** | — | Decisión del cliente entre reembolso y saldo a favor cuando un pago se confirma después de suspender su entrega (RN-119 a RN-121). |
| 73 | **Lote en revisión** | `lote.estado = 'en_revision'` | Estado de un lote con una lectura mayor de 4.5 °C durante los primeros 900 s (15 minutos). Sus entregas quedan retenidas y solo el administrador titular puede devolverlo a `recibido` (RN-024, RN-025 y RN-127 a RN-134). |
| 74 | **Importe a devolver** | `importe_devolver` | Dinero que Muuvi debe al cliente por una entrega fallida imputable a Muuvi, una entrega suspendida por lote invalidado o un pago confirmado tarde. El cliente elige reembolso o saldo a favor; si no elige en 72 h, se reembolsa (RN-129 y RN-130). |
| 75 | **Periodo de gracia** | — | Los 900 s (15 minutos) que un lote pasa `en_revision` antes de invalidarse (RN-134). |
