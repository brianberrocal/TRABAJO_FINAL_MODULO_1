# Auditoría del Spec Pack — Muuvi (ciclo 3)

**Versión:** 0.1.0 (borrador)
**Fecha:** 14/11/2026
**Autor:** Agente QA Architect (BMAD)
**Estado:** Pendiente de revisión humana

**Alcance:** el Spec Pack tal como quedó tras la validación del prototipo y la aprobación del fundador del 14/11/2026 a las 20:47. Son estos documentos:
- [`03-reglas-de-negocio.md`](03-reglas-de-negocio.md) v0.9.1 (RN-001 a RN-185);
- [`04-glosario.md`](04-glosario.md) v0.7.0;
- [`06-user-stories-refinadas.md`](06-user-stories-refinadas.md) v0.4.1 (HU-001 a HU-018);
- [`07-architecture.md`](07-architecture.md) v1.2.1;
- [`08-backlog.md`](08-backlog.md) v0.4.1 (T-001 a T-090);
- [`09-ui-spec.md`](09-ui-spec.md) v0.2.1 (34 pantallas);
- [`10-validation-log.md`](10-validation-log.md) v0.2.1;
- la constitución y los ADR-001 a ADR-009.

**Criterio:** se busca lo que haría que un agente implemente algo incorrecto, contradictorio o sin autorización. Ninguna sugerencia cambia el stack, las dependencias, los precios ni los textos legales; las que tocan lo comercial o lo legal quedan como decisión humana (PRINCIPIO-008).

**Severidad:**
- **CRÍTICA:** un agente que siga la especificación al pie de la letra causaría un daño irreversible o violaría la constitución.
- **ALTA:** produciría un resultado incorrecto en dinero, producto o datos, implementaría algo sin aprobación, o bloquea la salida a producción.
- **MEDIA:** produciría una ambigüedad o una contradicción que el agente tendría que resolver por su cuenta, y debería detenerse.
- **BAJA:** consistencia documental.

---

## Hallazgos

### ALTA

#### AUD-01 · Cancelar una de varias suscripciones reembolsa todo el saldo
- **Severidad:** ALTA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-124, frente a RN-160 y RN-162. También T-059 en `08-backlog.md` y P-010 en `09-ui-spec.md`.
- **Problema:**
  - RN-124 dice: «CUANDO un cliente con saldo a favor… **cancela su suscripción** o se da de baja, el sistema DEBE reembolsar el saldo íntegro». Se escribió cuando había una sola suscripción por cliente.
  - Con RN-160, un cliente con suscripciones de los martes y de los sábados que cancela la del martes recibe el reembolso de **todo** su saldo, aunque la del sábado sigue activa y RN-162 iba a aplicar ese saldo a su próximo ciclo.
  - T-059 y P-010 («Te devolvemos tu saldo a favor…») implementan la lectura literal.
- **Sugerencia:** que el fundador decida y se reescriba RN-124. La propuesta es reembolsar el saldo solo al cancelar la **última** suscripción no cancelada, o al darse de baja; si quedan otras, el saldo se conserva para ellas (RN-162). Después hay que ajustar el criterio de T-059, la simulación de cancelación de T-072 y P-010.

#### AUD-02 · Ninguna regla garantiza que el cliente reciba el producto que eligió
- **Severidad:** ALTA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-161, frente a RN-093, RN-122 y RN-159. `07-architecture.md` · algoritmo de asignación (§4.13). T-056 y T-077.
- **Problema:**
  - RN-161 asocia cada suscripción a un producto (es decir, a una finca). Pero el algoritmo de asignación reparte las entregas entre «los lotes `recibido` del mismo día con litros libres» sin mirar el producto.
  - RN-093 reasigna las entregas de un lote invalidado a «otro lote válido del mismo día». RN-122 cubre la escasez sumando todos los litros del día.
  - Un cliente que eligió Finca Santa Rosa puede recibir leche de La Ponderosa, y la demanda por producto puede no coincidir con el pedido.
  - Además, RN-159 deja que el operador reparta **a mano** la demanda entre fincas. Con RN-161, la demanda de cada finca ya está determinada por sus suscripciones, así que el reparto manual puede contradecirla.
- **Sugerencia:** que el fundador decida si, ante una invalidación o una escasez, se puede sustituir el producto de una finca por el de otra (con aviso al cliente) o si la entrega falla. Luego:
  - agregar una RN: «una entrega solo se asigna a lotes del producto de su suscripción, salvo la sustitución aprobada»;
  - calcular la demanda **por producto** en el corte (RN-090), en lugar de repartirla a mano en RN-159;
  - aplicar RN-122 por producto;
  - ajustar §4.13, T-056 y T-077.

#### AUD-03 · Tres textos todavía mandan aplicar la escasez al registrar el lote
- **Severidad:** ALTA
- **Ubicación:**
  - `08-backlog.md` · criterio de completitud de T-030;
  - `06-user-stories-refinadas.md` · HU-010, sexto criterio original;
  - `03-reglas-de-negocio.md` · ejemplo de RN-091.
- **Problema:** RN-157 (aprobada) dice que la escasez se aplica **al cerrar la recepción**, y RN-159, que lo comprometido de cada lote es el pedido de su finca. Sin embargo:
  - el criterio de T-030 exige que «5 L para A, B y C cubren A y C y dejan B `fallida`» al registrar el lote;
  - el criterio de HU-010 dice «CUANDO registro el lote, ENTONCES… B queda `fallida`»;
  - el ejemplo de RN-091 dice que «el lote se registra con 100 L comprometidos».

  El backlog solo agrega una nota de «ajuste» que remite a T-077, pero un agente que ejecute T-030 cumplirá su criterio literal. Así implementaría un comportamiento ya rechazado, que T-077 tendrá que deshacer, y un test escrito para T-030 tendría que modificarse después (PRINCIPIO-008.8).
- **Sugerencia:**
  - reescribir el criterio de T-030 para que la falta de litros sea solo una advertencia;
  - mover el caso A, B y C a T-077;
  - marcar como reemplazado el criterio de HU-010 y escribir el nuevo con «CUANDO cierro la recepción»;
  - corregir el ejemplo de RN-091 a «el pedido del día suma 100 L, repartido entre las fincas».

#### AUD-04 · El contrato de la §4.14 no está aprobado y sus tareas ya no tienen compuerta
- **Severidad:** ALTA
- **Ubicación:** `07-architecture.md` · Estado y §4.14 («esperan la aprobación humana del contrato»). `08-backlog.md` · T-072 a T-084 y T-089, con G7 liberada.
- **Problema:**
  - Al liberar G7, las tareas que implementan los endpoints nuevos (simulaciones, direcciones, recuperación, recepción, hoja de ruta, alertas y feriados) quedaron sin ninguna compuerta.
  - Pero la arquitectura sigue diciendo que el contrato espera la aprobación humana (D-04 de `10-validation-log.md`), y el principio D11 (contrato primero) exige que el endpoint exista en `openapi.yaml` antes de implementarse.
  - Un agente podría implementar un contrato que nadie aprobó.
- **Sugerencia:** agregar una compuerta **G9**, la aprobación del contrato de la §4.14, que bloquee T-072 a T-084 y T-089, o que el fundador apruebe ahora la §4.14 y que `07-architecture.md` pase a la versión 1.3.0.

#### AUD-05 · El pago `en_verificacion` no tiene desenlace (P23)
- **Severidad:** ALTA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-137 (P23). `06-user-stories-refinadas.md` · HU-007, criterio ⛔ P23. `08-backlog.md` · T-026 (`test.todo`). `07-architecture.md` · tarea `consultar_pago_en_verificacion`.
- **Problema:**
  - Si la pasarela no responde a las 18:00:00, el pago queda `en_verificacion` y el sistema consulta una sola vez.
  - No está definido cada cuánto se repite la consulta, hasta cuándo, ni qué pasa con la entrega de las 07:00 del día siguiente si a los 1 800 s el pago sigue sin resolverse: no se sabe si sale, si se suspende o si se reprograma.
  - Es el flujo de dinero central del PMV y un caso realista: cualquier caída de Culqi a las 18:00 afecta a todos los ciclos del día.
- **Sugerencia:** que el fundador responda P23 y que el BA escriba sus reglas. La propuesta: consultas cada 60 s hasta los 1 800 s; sin respuesta, la entrega pasa a `suspendida` con motivo `pago_en_verificacion` y, si el cargo se confirma después, se trata como pago tardío (RN-119 y RN-131). Con eso se cierra el `test.todo` de T-026.

#### AUD-06 · Responder un reclamo sigue bloqueado y es requisito legal para operar
- **Severidad:** ALTA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-146 a RN-149 (borrador legal) y P24. `06-user-stories-refinadas.md` · HU-016. `09-ui-spec.md` · P-040 ⛔. `10-validation-log.md` · OBS-07 y D-07.
- **Problema:** la Ley 31435 exige responder en 15 días hábiles. El sistema registra reclamos y alerta al día 10 (RN-036), pero no puede responder. Al salir a producción, el primer reclamo dejaría a Muuvi sin cómo cumplir el plazo desde el sistema. Tampoco están validados los campos de la hoja de reclamación (OBS-24).
- **Sugerencia:** antes de producción, conseguir la validación del asesor legal de RN-146 a RN-149, P24 y los campos de P-015, o aprobar por escrito un procedimiento provisional fuera del sistema con registro manual. Agregar al backlog una compuerta de salida a producción que lo exija.

### MEDIA

#### AUD-07 · RN-132 no menciona la excepción de RN-185
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-132, frente a RN-185.
- **Problema:** RN-132 dice que el sistema NO DEBE permitir cerrar como `entregada` **ninguna** entrega de un lote `en_revision`. RN-185 (aprobada) permite exactamente eso para los cierres hechos sin conexión, y declara la excepción solo en su propio texto. Un test escrito desde RN-132 (`test_RN132_…`) fallaría con el comportamiento de RN-185, o al revés.
- **Sugerencia:** agregar a RN-132: «salvo los cierres registrados sin conexión, que se rigen por RN-185», y escribir un test que cubra los dos casos.

#### AUD-08 · No hay dónde deduplicar las lecturas altas
- **Severidad:** MEDIA
- **Ubicación:** `07-architecture.md` · §3.2, tabla `lectura_temperatura`, y §4.9 (elementos `lectura_alta` con `evento_id`). `08-backlog.md` · T-078.
- **Problema:** la §4.9 y T-078 deduplican la lectura alta por `evento_id`, pero `lectura_temperatura` no tiene esa columna, y `cierre_entrega` solo guarda cierres. Además, el trigger de RN-088 impide hacer un `UPDATE` para marcarla. Un reenvío sin conexión podría duplicar la lectura.
- **Sugerencia:** agregar `evento_id UUID UNIQUE NULL` a `lectura_temperatura` (en una migración aditiva, dentro de T-078) y documentarlo en la §3.2.

#### AUD-09 · No está claro qué suscripción bloquea la baja
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-098 («una suscripción activa»). `08-backlog.md` · T-050 («una suscripción distinta de `cancelada`»). `09-ui-spec.md` · P-014 («suscripción no cancelada»).
- **Problema:** la regla y su implementación no dicen lo mismo. Una suscripción `pausada` o `suspendida_por_pago` bloquea la baja según T-050 y P-014, pero no según el texto literal de RN-098.
- **Sugerencia:** que el fundador confirme la lectura de T-050 (cualquier suscripción no cancelada bloquea la baja) y que se reescriba RN-098 con esa precisión.

#### AUD-10 · Cargar o retirar feriados no tiene en cuenta los días sin reparto ni los plazos de reclamos
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-173 y RN-174, frente a RN-035 y RN-101.
- **Problema:**
  - RN-173 rechaza un feriado de «una fecha cuyo corte de reparto ya ocurrió», pero un lunes o un miércoles no tienen corte. No se sabe si se aceptan siempre.
  - Un feriado cargado o retirado tarde cambia los días hábiles, pero `reclamo.fecha_limite` se calcula una sola vez al registrar el reclamo (T-033). Los plazos legales de los reclamos abiertos quedarían mal.
- **Sugerencia:**
  - precisar que RN-173 solo aplica a los días de reparto;
  - agregar una RN que mande recalcular `fecha_limite` de los reclamos abiertos al cargar o retirar un feriado, sin acortar nunca un plazo ya informado al consumidor (validarlo con el asesor legal).

#### AUD-11 · El saldo de botellas sin conexión falla con dos entregas al mismo cliente el mismo día
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-145, RN-031 y RN-160. `09-ui-spec.md` · P-021 («máximo = saldo»).
- **Problema:** con varias suscripciones, un cliente puede tener dos entregas el mismo día. Las dos llevan el mismo saldo de botellas descargado. Si el repartidor registra devoluciones en ambas sin conexión, la suma puede superar el saldo real y la segunda se rechaza al sincronizar (RN-031), después de que el cliente ya entregó las botellas.
- **Sugerencia:**
  - en la asignación (T-056), agrupar las entregas del mismo cliente y la misma dirección en una sola parada;
  - en la PWA (T-088), descontar las devoluciones ya registradas del saldo local de las demás entregas de ese cliente;
  - decidir si dos suscripciones del mismo día y la misma dirección se permiten.

#### AUD-12 · Varias reglas nuevas no son atómicas
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md`:
  - RN-156 (guardar la lectura, retener la entrega, y qué pasa al revertir y al invalidar);
  - RN-171 (efecto de la anulación y prohibición en otros estados);
  - RN-174 (efecto del retiro y permiso del operador);
  - RN-177 (rechazo de la segunda propuesta y paso a `descartada`);
  - RN-185 (registro con marca, alerta y aviso).
- **Problema:** las convenciones del documento piden reglas atómicas, con un test por regla (PRINCIPIO-009). Estas mezclan dos o tres comportamientos con patrones EARS distintos, así que un solo `test_RNxxx` no las cubre o queda ambiguo.
- **Sugerencia:** partirlas como se hizo con QA-051, por ejemplo RN-177 en RN-177 (Unwanted) y RN-186 (Event-driven), o declarar por escrito que cada regla compuesta tiene un test por comportamiento.

#### AUD-13 · El glosario no define los términos nuevos
- **Severidad:** MEDIA
- **Ubicación:** `04-glosario.md` v0.7.0 (12/11/2026).
- **Problema:** el glosario dice que sus términos «significan exactamente lo que dice aquí», pero no define ninguno de los términos nuevos: hoja de ruta, recepción del día, cierre de la recepción, pedido por finca, entrega retenida, lectura alta, `entregada_durante_revision`, propuesta descartada, simulación o vista previa, suscripción adicional y enlace de recuperación. Un agente que los busque no los encontrará.
- **Sugerencia:** publicar `04-glosario.md` v0.8.0 con esas entradas, cada una con su regla.

#### AUD-14 · La custodia de la hoja de ruta impresa no tiene procedimiento
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-155 («un humano define su custodia, devolución y destrucción»). `09-ui-spec.md` · P-042 (`[TEXTO LEGAL: hoja.custodia]`).
- **Problema:**
  - La hoja impresa contiene direcciones de clientes fuera del sistema y fuera del cifrado del PRINCIPIO-003.
  - No hay documento que diga quién la entrega, quién la recibe de vuelta, cuándo se destruye ni qué pasa si se pierde.
  - Tampoco se sabe si eso debe figurar en el registro ante la ANPDP (Ley 29733).
- **Sugerencia:** que un humano escriba `docs/procedimiento-hoja-ruta.md` (entrega contra firma, devolución al cierre de la ruta, destrucción diaria y aviso por pérdida) y que el asesor legal valide si hace falta declararlo.

#### AUD-15 · Ninguna compuerta impide implementar las historias sin aprobar
- **Severidad:** MEDIA
- **Ubicación:** `06-user-stories-refinadas.md` · HU-017 y HU-018 («✅ Propuesta»). `08-backlog.md` · T-052, T-064, T-075, T-085, T-086 y T-087.
- **Problema:** las dos historias siguen sin aprobar, pero ninguna compuerta bloquea sus tareas. HU-017 es además requisito del catálogo en producción.
- **Sugerencia:** aprobar HU-017 y HU-018, o agregar una compuerta «HU aprobada» a las tareas que citan una HU propuesta.

#### AUD-16 · Quedan escenarios sin regla como `test.todo`
- **Severidad:** MEDIA
- **Ubicación:** `06-user-stories-refinadas.md` · tabla de escenarios sin regla: SR-01, SR-03, SR-06, SR-08, SR-11, SR-14 y SR-15.
- **Problema:** siguen sin regla siete escenarios:
  - una cuenta duplicada por un corte de conexión (SR-01);
  - un nacido el 29/02 (SR-03);
  - una entrega trasladada dentro de una pausa (SR-06);
  - modificar o cancelar una suscripción ya cancelada (SR-08);
  - una demanda de 0 L (SR-11);
  - un reclamo registrado en sábado (SR-14);
  - un reclamo sin correo (SR-15).

  SR-14 y SR-15 tocan plazos y comunicaciones legales, y SR-06 cambia cuántas entregas omite una pausa (RN-062).
- **Sugerencia:** que el BA escriba sus reglas, con prioridad para SR-14, SR-15 y SR-06. Las dos primeras, validadas con el asesor legal.

#### AUD-17 · Faltan validaciones del contador sobre cobros y comprobantes
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-057, RN-125, RN-126 y RN-168 a RN-172. `08-backlog.md` · compuerta G8 y T-080.
- **Problema:**
  - RN-057 manda emitir una nota de crédito «cuando se aplica un saldo a favor».
  - RN-125 y RN-126 mandan emitir una boleta por el importe completo de un ciclo pagado con saldo.
  - Si las dos se aplican al mismo ciclo, la venta podría quedar anulada o el ingreso declarado dos veces.
  - Los efectos de las anulaciones también esperan al contador.
- **Sugerencia:** llevar al contador una sola consulta con los cuatro casos: saldo total, saldo parcial, anulación de un pago y anulación de un comprobante. Corregir RN-057 según su respuesta antes de implementar T-048 y T-080.

#### AUD-18 · K10 y P14 siguen abiertos y afectan avisos al cliente
- **Severidad:** MEDIA
- **Ubicación:** `03-reglas-de-negocio.md` · K10 y RN-103 (P14). `09-ui-spec.md` · §8, plantillas CO-02 y CO-15.
- **Problema:**
  - No está definido si los avisos transaccionales al cliente respetan la ventana de 09:00 a 20:00 (K10); hoy el correo sale sin ventana.
  - El aviso de un cambio de precio no tiene antelación (P14), así que no se puede cambiar el precio a nadie que ya tenga una suscripción activa.
- **Sugerencia:** que el fundador decida K10 y P14. Sin P14, dejar escrito que el precio no cambia con suscriptores activos.

#### AUD-19 · La UI impone una condición que ninguna regla pide
- **Severidad:** MEDIA
- **Ubicación:** `09-ui-spec.md` · P-033 («Confirmar asignación se habilita con la recepción cerrada») y F-09.
- **Problema:** ninguna regla exige cerrar la recepción antes de confirmar la asignación: RN-157 solo condiciona la escasez. Si la API no lo exige, la PWA y la API se comportan distinto; si la API lo exige, falta la regla.
- **Sugerencia:** agregar la RN («SI la recepción del día no está cerrada, ENTONCES el sistema DEBE rechazar la confirmación de la asignación») y el `409` correspondiente en la §4.13, o quitar la condición de P-033.

### BAJA

#### AUD-20 · Fuentes con versiones viejas
- **Severidad:** BAJA
- **Ubicación:**
  - `07-architecture.md` · Fuentes (cita `03` v0.9.0);
  - `09-ui-spec.md` · Fuentes (cita `06` v0.4.0, `07` v1.2.0 y `03` v0.9.0);
  - `08-backlog.md` · introducción de T-072 a T-090.
- **Problema:** las referencias no apuntan a las versiones vigentes (03 v0.9.1, 06 v0.4.1 y 07 v1.2.1).
- **Sugerencia:** actualizar las líneas de fuentes en la próxima versión de cada documento.

#### AUD-21 · Dos documentos con el número 09
- **Severidad:** BAJA
- **Ubicación:** `docs/09-auditoria-trazabilidad.md` y `docs/09-ui-spec.md`.
- **Problema:** el orden del Spec Pack queda ambiguo.
- **Sugerencia:** renombrar uno de los dos (por ejemplo, `09-ui-spec.md` a `09b-ui-spec.md`) y actualizar los enlaces.

#### AUD-22 · Dos posturas distintas sobre revelar si un correo tiene cuenta
- **Severidad:** BAJA
- **Ubicación:** `03-reglas-de-negocio.md` · RN-154 (el registro revela que el correo existe) y RN-165 (la recuperación no lo revela).
- **Problema:** la recuperación protege un dato que el registro ya expone. No es un error, pero el texto de RN-165 sugiere una protección que no existe.
- **Sugerencia:** agregar una nota en RN-165 que explique que la respuesta genérica evita el envío de correos a terceros y no la enumeración, que ya aceptó el fundador en RN-154.

#### AUD-23 · El índice de ADR no menciona que K9 cambió el ADR-008
- **Severidad:** BAJA
- **Ubicación:** `docs/adr/README.md` y `docs/adr/ADR-008-reparto-sin-conexion.md`.
- **Problema:** la nota sobre K9 está solo en `07-architecture.md` §5. Quien lea el ADR-008 sigue viendo «el repartidor no ve la dirección sin conexión hasta que se resuelva K9».
- **Sugerencia:** agregar al índice de ADR una línea «ADR-008: consecuencia sobre K9 resuelta por RN-155 (14/11/2026)», sin modificar el ADR aceptado.

#### AUD-24 · «Mermas» sigue sin definir
- **Severidad:** BAJA
- **Ubicación:** `03-reglas-de-negocio.md` · P25 y nota de SR-13. QA-049.
- **Problema:** la decisión sobre los cierres sin conexión menciona registrar «mermas», y ninguna regla, pantalla ni tarea las define.
- **Sugerencia:** que el fundador responda P25 o retire el término.

---

## Resumen

| Severidad | Cantidad | Hallazgos |
|---|---|---|
| CRÍTICA | 0 | — |
| ALTA | 6 | AUD-01 a AUD-06 |
| MEDIA | 13 | AUD-07 a AUD-19 |
| BAJA | 5 | AUD-20 a AUD-24 |

**Qué hace bien el Spec Pack:**
- Los IDs están verificados por script entre los seis documentos, sin referencias rotas.
- Cada pantalla traza a una HU y cada HU tiene pantalla.
- El 99.4 % de las reglas visibles está reflejado en la UI.
- El backlog es un grafo de dependencias sin ciclos, con la ruta crítica calculada.
- Las compuertas humanas son explícitas.
- No hay dependencias sin aprobar ni secretos en la especificación.

**Qué falta:** los hallazgos ALTA nacen sobre todo de dos decisiones recientes. Las suscripciones múltiples (AUD-01, AUD-02 y AUD-11) chocan con reglas escritas para una sola suscripción. La escasez al cerrar la recepción (AUD-03) no se propagó a los criterios antiguos. A eso se suman dos pendientes viejos que bloquean la producción: P23 (AUD-05) y la validación legal (AUD-06).

## Puntaje de madurez del Spec Pack

| Puntaje | Significado |
|---|---|
| 1 | Muy inmaduro: faltan reglas o HU esenciales |
| 2 | Hay hallazgos CRÍTICA: un agente causaría daño siguiendo la especificación |
| 3 | Sin CRÍTICA, pero con ALTA abiertas: un agente puede trabajar en la mayoría de las tareas, no en las afectadas |
| 4 | Solo MEDIA y BAJA: listo para agentes, con decisiones menores pendientes |
| 5 | Listo para agentes: sin ambigüedades abiertas |

### Puntaje: **3 / 5**

No hay hallazgos CRÍTICA, y la mayor parte del backlog (cuentas, catálogo, calendario, cifrado, cobro básico, comprobantes y reclamos) se puede implementar sin riesgo. Pero seis hallazgos ALTA afectan al dinero (AUD-01 y AUD-05), al producto que recibe el cliente (AUD-02), a criterios que mandan implementar algo ya rechazado (AUD-03), a la gobernanza del contrato (AUD-04) y al cumplimiento legal (AUD-06).

**Cómo llegar a 4:** que el fundador decida AUD-01, AUD-02, AUD-04 y AUD-05 (P23); corregir AUD-03, que es solo de redacción, y documentar un plan escrito para AUD-06.

**Cómo llegar a 5:** además, cerrar las 13 MEDIA, sobre todo el contador (AUD-17), K10 y P14 (AUD-18) y los escenarios sin regla (AUD-16).

---

## Historial de cambios
| Versión | Fecha | Autor | Cambio |
|---|---|---|---|
| 0.1.0 | 14/11/2026 | Agente QA Architect (BMAD) | Auditoría del Spec Pack tras la validación del prototipo: 24 hallazgos y puntaje de madurez |
