# Validation Log — Prototipo de interfaz de Muuvi

**Versión:** 0.3.0 (borrador)
**Fecha:** 14/11/2026
**Autor:** Agente Prototype Reviewer (BMAD)
**Estado:** Pendiente de revisión humana

> **Rondas de este registro:**
> - **Ronda 2 (este documento, 14/11/2026, 21:11):** revisa el prototipo **vigente**, [`09-ui-spec.md`](09-ui-spec.md) v0.2.1, con sus 34 pantallas (P-001 a P-042), los flujos F-01 a F-17 y las plantillas CO-01 a CO-16. Lo contrasta con [`06-user-stories-refinadas.md`](06-user-stories-refinadas.md) v0.4.1 (HU-001 a HU-018) y [`03-reglas-de-negocio.md`](03-reglas-de-negocio.md) v0.9.1 (RN-001 a RN-185).
> - **Ronda 1 ([Anexo A](#anexo-a--ronda-1)):** revisó la v0.1.0, con las observaciones OBS-01 a OBS-34 y el estado de su subsanación.
>
> **Sobre el material recibido:** el adjunto de esta ronda es idéntico, byte a byte, al de la ronda 1. Contiene la UI spec rotulada «1.0.0», que es la v0.1.0. Revisarlo de nuevo repetiría el Anexo A, así que se revisó la versión vigente del proyecto.
>
> **Alcance:** solo gaps funcionales, no estética. Las observaciones nuevas siguen la numeración, desde **OBS-35**. Las que coinciden con hallazgos de [`11-auditoria-spec-pack.md`](11-auditoria-spec-pack.md) lo indican (AUD-NN).

**Leyenda.**
- **Severidad:**
  - **CRÍTICA:** impide completar una tarea principal.
  - **ALTA:** produce un resultado incorrecto o irreversible para el usuario, o bloquea la salida a producción.
  - **MEDIA:** la tarea se completa con fricción, o la UI promete algo que las reglas no garantizan.
  - **BAJA:** pulido o claridad.
- **Tipo:**
  - *comprensión:* la UI y la regla se pueden leer de dos maneras, o se contradicen;
  - *gap:* falta una pantalla, un estado, una acción o una regla;
  - *fricción:* la tarea se completa, pero con pasos de más o sin guía;
  - *mejora:* conviene agregarlo, aunque no falta.

---

## 1. Cobertura de HUs

Las **18 HU Must tienen al menos una pantalla**, y las 34 pantallas trazan a una HU. Revisando criterio por criterio, 7 HU quedan cubiertas por completo, 10 tienen huecos nuevos y 1 sigue bloqueada.

| HU | Pantallas | Cobertura | Qué falta |
|---|---|---|---|
| HU-001 Registro | P-003, P-004 | ✅ Completa | — |
| HU-002 Baja | P-014 | 🟡 Parcial | El 409 promete fechas de liberación que pueden no cumplirse (OBS-46), y no está claro qué suscripción bloquea la baja (OBS-47) |
| HU-003 Catálogo | P-001, P-002 | ✅ Completa | — |
| HU-004 Suscripción | P-005, P-006, P-007, P-017 | 🟡 Parcial | P-005 deja elegir una finca que las reglas no garantizan entregar (OBS-35), y el aviso de Yape y Plin da un día equivocado (OBS-37) |
| HU-005 Pausa | P-007, P-008 | ✅ Completa | — |
| HU-006 Modificar y cancelar | P-007, P-009, P-010, P-017 | 🟡 Parcial | Cancelar una de varias suscripciones anuncia el reembolso de todo el saldo (OBS-36) |
| HU-007 Cobro | P-005 a P-007, P-011 | 🟡 Parcial | Los textos de aprobación de Yape y Plin dicen «cada viernes» (OBS-37) |
| HU-008 Comprobantes | P-012, P-013, P-041 | ✅ Completa | — |
| HU-009 Reembolso | P-007, P-011, P-041 | 🟡 Parcial | El ejemplo del cobro neto aplica el saldo a la suscripción equivocada (OBS-38), la entrega reprogramada aparece en otra tarjeta y con cobro (OBS-39), y CO-05 promete una devolución sin regla (OBS-44) |
| HU-010 Lotes | P-030 a P-033, P-041, P-042 | 🟡 Parcial | El pedido se puede editar con lotes ya registrados (OBS-41), y P-033 exige la recepción cerrada sin una regla (OBS-42) |
| HU-011 Reparto | P-020 a P-022, P-042 | 🟡 Parcial | No se puede cerrar como fallida una entrega con lectura alta (OBS-40), y el saldo de botellas sin conexión falla con dos entregas al mismo cliente (OBS-43) |
| HU-012 Reclamo | P-015, P-016, P-039 | ✅ Completa (⛔ campos legales) | Los campos de P-015 esperan al asesor legal (OBS-24 de la ronda 1) |
| HU-013 Precio | P-035, P-036 | 🟡 Parcial | Con suscriptores activos se puede confirmar un precio sin el aviso de RN-103 (OBS-48) |
| HU-014 Anulaciones | P-036, P-037 | 🟡 Parcial | P-036 presenta como hechos efectos que todavía no se ejecutan (OBS-49) |
| HU-015 Feriados | P-038, P-041 | 🟡 Parcial | La vista previa no muestra el efecto en los plazos de reclamos (OBS-50) |
| HU-016 Respuesta a reclamos | P-039, P-040 ⛔ | ⛔ Bloqueada | Validación legal (OBS-07 de la ronda 1) |
| HU-017 Fincas y productos ⏳ | P-002, P-034 | ✅ Completa | La HU no está aprobada |
| HU-018 Recuperar contraseña ⏳ | P-004, P-018 | ✅ Completa | La HU no está aprobada. El token puede quedar en los logs del servidor (OBS-45) |

**HU Must sin pantalla:** ninguna.

---

## 2. Reglas de negocio

| Clase | Cantidad | Detalle |
|---|---|---|
| ✅ Reflejada | 146 | Tiene un elemento visible correcto en la UI |
| 🟡 Parcial o contradicha | 13 | RN-066, RN-078, RN-098, RN-103, RN-124, RN-156, RN-162, RN-164 y RN-168 a RN-172 (ver 2.1) |
| ❌ Falta | 0 | — |
| ⚪ Invisible por diseño | 22 | Las 21 de la ronda 1 y RN-176 (alta de usuarios por consola, sin pantalla) |
| ➖ SHOULD fuera del PMV | 4 | RN-042, RN-043, RN-044 y RN-117 |

### 2.1 Reglas que la UI refleja mal o de forma incompleta

| RN | Qué pasa en la UI | Pantalla | OBS |
|---|---|---|---|
| RN-066 (y RN-067) | La entrega reprogramada aparece en la tarjeta de otra suscripción, junto a un «Cobro», aunque no se cobra de nuevo | P-007 | OBS-39 |
| RN-078 | CO-05 promete «la devolución que corresponda», pero ninguna regla define una devolución para las entregas ya hechas de un lote invalidado | §8 | OBS-44 |
| RN-098 | P-014 dice «suscripción no cancelada» y la regla dice «suscripción activa» | P-014 | OBS-47 |
| RN-103 | P-035 permite enviar un precio a confirmar con suscriptores activos, aunque el aviso no está definido (P14) | P-035 | OBS-48 |
| RN-124 | P-010 anuncia el reembolso de todo el saldo al cancelar cualquiera de las suscripciones | P-010 | OBS-36 |
| RN-156 | La temperatura se pide antes que el resultado: con más de 4.5 °C no se puede registrar «Fallida», aunque RN-086 solo exige temperatura para «Entregada» | P-021 | OBS-40 |
| RN-162 | El ejemplo de P-007 descuenta el saldo del ciclo del sábado, pero la regla lo aplica primero al del martes | P-007 | OBS-38 |
| RN-164 | P-005, P-006, P-007 y F-17 dicen «cada viernes a las 18:00», pero el corte es el día anterior a **cada** entrega: lunes, miércoles o viernes, y otro si hay traslado por feriado | P-005 a P-007 | OBS-37 |
| RN-168 a RN-172 | P-036 lista los efectos como si fueran a ocurrir, pero mientras el contador no valide (G8), la anulación solo marca el registro | P-036 | OBS-49 |

### 2.2 Reglas invisibles por diseño
RN-006, RN-007, RN-023, RN-039, RN-040, RN-046, RN-055, RN-060, RN-067, RN-069, RN-072, RN-073, RN-080, RN-095, RN-104, RN-105, RN-118, RN-125, RN-136, RN-137, RN-138 y RN-176. Se verifican con tests de backend.

---

## 3. Flujo de usuario

### 3.1 Tareas principales

| Tarea | Flujo | ¿Se completa? | Fricción o corte |
|---|---|---|---|
| Suscribirse | F-01 | 🟡 | La finca elegida no está garantizada (OBS-35) |
| Varias suscripciones | F-01, P-007 | 🟡 | El saldo se muestra en la tarjeta equivocada (OBS-38) y la entrega reprogramada aparece en otra tarjeta (OBS-39) |
| Pagar con Yape o Plin | F-17 | ❌ Para los clientes de martes y jueves | El aviso les dice «viernes»: aprueban tarde y su entrega se suspende (OBS-37) |
| Cancelar una de varias suscripciones | F-03 | 🟡 | Se anuncia, y se ejecuta, el reembolso de todo el saldo (OBS-36) |
| Darse de baja | F-07 | 🟡 | Las fechas de liberación pueden no cumplirse (OBS-46) |
| Mañana de operación | F-09 | 🟡 | El pedido se edita con lotes ya registrados (OBS-41), y la asignación depende de una regla que no existe (OBS-42) |
| Ruta del repartidor | F-10 | 🟡 | Con un cliente ausente y una lectura alta, la entrega no se puede cerrar (OBS-40); falla con dos entregas al mismo cliente (OBS-43) |
| Recuperar la contraseña | F-16 | ✅ | El token puede quedar en los logs (OBS-45) |
| Cambiar el precio | F-11 | 🟡 | Con suscriptores, se confirma sin aviso (OBS-48) |
| Anular | F-12 | 🟡 | Los efectos mostrados no coinciden con lo que se ejecuta hoy (OBS-49) |
| Feriados | F-14 | 🟡 | El efecto en los plazos de reclamos no se ve (OBS-50) |
| Responder un reclamo | F-15 | ❌ | Bloqueado por el asesor legal (OBS-07 de la ronda 1) |
| Las demás tareas | F-02, F-04, F-05, F-06, F-08, F-13 | ✅ | — |

### 3.2 Callejones sin salida

| # | Dónde | Qué pasa | OBS |
|---|---|---|---|
| CS-10 | P-021, cliente ausente con temperatura alta | Con la temperatura ya registrada, «Fallida» se oculta y solo queda «Registrar temperatura alta». La entrega queda «Retenida» hasta que se decida el lote, aunque el cliente no estaba y la botella nunca se iba a entregar | OBS-40 |
| CS-11 | P-022, dos entregas del mismo cliente el mismo día | La segunda devolución de botellas se rechaza al sincronizar (RN-031), después de que el cliente ya entregó las botellas, y no hay forma de corregirla | OBS-43 |
| CS-12 | P-040 | Sigue bloqueada (OBS-07 de la ronda 1) | — |

Los callejones CS-1 a CS-9 de la ronda 1 quedaron resueltos (Anexo A, §A.7).

---

## 4. Observaciones

| # | Observación | Tipo | Severidad | Acción sugerida | HU afectada |
|---|---|---|---|---|---|
| OBS-35 | P-005 deja elegir «Leche fresca 1 L · Santa Rosa» (RN-161), pero ninguna regla garantiza que el cliente reciba esa finca. La asignación (§4.13 de la arquitectura), la reasignación por invalidación (RN-093) y la escasez (RN-122) reparten los litros del día sin mirar el producto. La UI promete algo que el sistema puede no cumplir (AUD-02) | comprensión | ALTA | Que el fundador decida si se puede sustituir la leche por la de otra finca, y escribir la RN. Si se permite, agregar en P-007 el banner «Esta semana tu leche viene de La Ponderosa» y su correo; si no, que la entrega falle con devolución. Mientras tanto, mostrar la finca en P-005 solo como origen habitual | HU-004, HU-003 |
| OBS-36 | P-010 dice «Te devolvemos tu saldo a favor de S/ 14.50…» al cancelar **cualquier** suscripción. Con varias suscripciones (RN-160), cancelar la del martes devolvería todo el saldo, aunque la del sábado sigue activa y RN-162 iba a usarlo (AUD-01) | comprensión | ALTA | Que el fundador corrija RN-124: reembolso solo al cancelar la última suscripción no cancelada, o al darse de baja. P-010 muestra entonces «Tu saldo de S/ 14.50 se usará en tu suscripción de los sábados» | HU-006, HU-009 |
| OBS-37 | Varios textos dicen que el cobro es el **viernes**: P-005 («Cada viernes a las 18:00 te pediremos aprobar…»), P-006 («Cada viernes a las 18:00…»), el banner de P-007 («viernes 18:00 a 18:30») y F-17. Pero el corte es a las 18:00 del día **anterior a cada entrega** (RN-045): el lunes para el martes, el miércoles para el jueves y el viernes para el sábado, y cambia si hay traslado por feriado (RN-123). Un cliente de los martes esperaría el viernes, no aprobaría el lunes y su entrega se suspendería (RN-074) | comprensión | ALTA | Reemplazar los textos por «El día anterior a cada entrega, a las 18:00, te pediremos aprobar el pago; tendrás 30 minutos». En P-007, mostrar la fecha concreta del próximo cobro de cada tarjeta («Aprueba el lunes 16/11/2026 a las 18:00») | HU-007, HU-004 |
| OBS-38 | El ejemplo de P-007 descuenta el saldo de S/ 14.50 del ciclo del **sábado** («Cobro: viernes 20/11/2026, 18:00 · S/ 29.00 (S/ 43.50 − S/ 14.50)») y cobra el del **martes** completo. RN-162 manda aplicar el saldo por orden de corte, así que va primero al martes (corte el lunes 16/11): el martes debe mostrar S/ 14.50 y el sábado, S/ 43.50. Los tests de la UI se escribirían con el ejemplo equivocado | comprensión | MEDIA | Corregir el ejemplo: tarjeta del martes «Cobro: lunes 16/11/2026, 18:00 · S/ 14.50 (S/ 29.00 − S/ 14.50 de saldo a favor)», tarjeta del sábado «S/ 43.50». Agregar a T-086 un test con este caso | HU-009 |
| OBS-39 | En P-007, la tarjeta del martes muestra «Próxima entrega: martes 17/11/2026 · Reprogramada (último intento)» junto a «Cobro: lunes 16/11/2026 · S/ 29.00». La entrega reprogramada pertenece a la suscripción **original**, la del sábado (RN-066), y no se cobra de nuevo (RN-067). Además, el martes la clienta recibe dos entregas, la reprogramada y la de su suscripción de los martes, y la UI muestra una sola | comprensión | MEDIA | Mostrar la reprogramada en la tarjeta de origen como una entrega adicional: «Entrega reprogramada: martes 17/11/2026 · sin cobro · último intento». En `GET /suscripciones`, incluir la lista de entregas próximas de cada suscripción, no solo «la próxima» | HU-009, HU-011 |
| OBS-40 | En P-021 la temperatura se mide primero y, con más de 4.5 °C, se ocultan «Entregada» y «Fallida» (RN-156). Si el cliente no está, el repartidor no puede registrar la causa `imputable_cliente` (RN-052): la entrega queda «Retenida» hasta que se decida el lote, y si el lote se invalida, pasa a `suspendida` con devolución en lugar de reprogramarse. RN-086 solo exige temperatura para «Entregada» | gap | MEDIA | Pedir primero el resultado: con «Fallida», la temperatura es opcional. Una lectura alta registrada igual pasa el lote a revisión (RN-025), pero no impide cerrar la entrega como fallida. Agregar esa precisión a RN-156 (decisión del fundador) | HU-011 |
| OBS-41 | En P-030 el pedido por finca es «editable hasta cerrar la recepción», pero P-031 copia lo comprometido del pedido en cada lote al registrarlo (RN-159). Si el operador cambia el pedido después de registrar un lote, ese lote queda con un comprometido viejo, y la UI no lo avisa. Además, con RN-161 la demanda por finca ya sale de las suscripciones (AUD-02) | comprensión | MEDIA | Bloquear la edición del pedido de una finca que ya tiene un lote registrado, o recalcular lo comprometido de sus lotes al guardar, avisándolo en P-030. Revisar RN-159 junto con OBS-35 | HU-010 |
| OBS-42 | P-033 habilita «Confirmar asignación» solo con la recepción cerrada, pero ninguna regla lo exige (RN-157 solo condiciona la escasez). Si la API no lo valida, otro cliente de la API puede confirmar antes; si lo valida, falta la regla (AUD-19) | gap | MEDIA | Agregar la RN («SI la recepción del día no está cerrada, ENTONCES el sistema DEBE rechazar la confirmación de la asignación») con su `409`, o quitar la condición de P-033 | HU-010, HU-011 |
| OBS-43 | Con varias suscripciones, un cliente puede tener dos entregas el mismo día. P-021 muestra en ambas el mismo «Saldo de botellas del cliente: 2» y limita las devueltas a ese saldo. Sin conexión, el repartidor puede registrar 2 devueltas en cada una y la segunda se rechaza al sincronizar (RN-031, RN-145 y RN-160) (AUD-11) | gap | MEDIA | Agrupar en una sola parada de P-020 y P-042 las entregas del mismo cliente y la misma dirección, y descontar en la PWA las devueltas ya registradas del saldo local de las demás entregas de ese cliente | HU-011 |
| OBS-44 | CO-05 (disculpa por lote invalidado) promete «la devolución que corresponda», pero RN-078 solo manda disculparse. RN-079 cubre las entregas `en_ruta` suspendidas y ninguna regla define una devolución para quien ya recibió leche de ese lote. El cliente recibiría un correo con una promesa vacía | gap | MEDIA | Que el fundador decida si esas entregas generan un importe a devolver (RN-129). Si no lo generan, quitar esa frase de CO-05 | HU-011, HU-009 |
| OBS-45 | En P-018 el token llega en el enlace de CO-16 y la PWA lo quita de la barra de direcciones al cargar. Pero si el enlace lleva el token en la consulta (`?token=`), la petición de la página ya lo envió al servidor que sirve la PWA y queda en sus logs de acceso, antes de que la PWA lo quite (RN-007 y RN-166) | mejora | MEDIA | Poner el token en el fragmento del enlace (`/recuperar/nueva#token=…`), que el navegador no envía al servidor, y documentarlo en P-018 y CO-16 | HU-018 |
| OBS-46 | El 409 de P-014 promete «Podrás darte de baja cuando termine, hoy antes de las 10:00». Si esa entrega falla por causa del cliente, se reprograma (RN-066) y vuelve a bloquear la baja (RN-098), y la fecha prometida no se cumple | comprensión | BAJA | Usar un texto condicional: «Podrás darte de baja cuando se cierre esta entrega. Si no podemos entregarla, la reprogramaremos y te avisaremos» | HU-002 |
| OBS-47 | P-014 y T-050 bloquean la baja por «una suscripción no cancelada», pero RN-098 dice «una suscripción activa». No está claro si una `pausada` o `suspendida_por_pago` la bloquea (AUD-09) | comprensión | MEDIA | Que el fundador confirme la lectura de P-014 y que se reescriba RN-098 con «no cancelada» | HU-002 |
| OBS-48 | P-035 muestra «⛔ El aviso a los suscriptores está pendiente de definir (P14)», pero deja enviar la propuesta, y P-036 deja confirmarla. Con suscriptores activos, el precio cambiaría sin el aviso que exige RN-103 | gap | MEDIA | Mientras P14 esté abierta, deshabilitar «Enviar para confirmar» cuando haya suscripciones activas, con el texto «No se puede cambiar el precio con suscriptores hasta definir el aviso previo (P14)». Que la API lo rechace también | HU-013 |
| OBS-49 | P-036 lista bajo «Qué va a pasar» la devolución, la nota de crédito, la cancelación de la entrega y el aviso al cliente (RN-168 a RN-172). Pero hasta que el contador valide (G8), la anulación solo marca el registro, como dice P-037. El administrador confirmaría creyendo que se devuelve el dinero | comprensión | MEDIA | Mientras G8 esté cerrada, P-036 debe decir: «Hoy la anulación solo marca el registro. La devolución y la nota de crédito se harán a mano hasta que se valide el procedimiento con el contador». Que la simulación devuelva los efectos que de verdad se ejecutan | HU-014 |
| OBS-50 | La vista previa de P-038 dice cuántas entregas se trasladan, pero no que el feriado cambia los días hábiles de los reclamos abiertos (RN-101). La `fecha_limite` ya informada al consumidor (P-016 y CO-10) no se recalcula (AUD-10) | gap | MEDIA | Agregar a la vista previa «Cambia el plazo de 3 reclamos abiertos», y escribir la RN de recálculo sin acortar nunca un plazo ya informado (validarla con el asesor legal) | HU-015, HU-012 |
| OBS-51 | P-017 permite agregar direcciones, pero no corregirlas ni retirarlas. Una dirección mal escrita queda para siempre en las listas de P-005 y P-009 | mejora | BAJA | Agregar «Editar» y «Dejar de usar» (marca `vigente = false`, sin borrar), solo si ninguna entrega `programada` o `en_ruta` la usa | HU-006 |
| OBS-52 | Las fuentes de la UI spec citan versiones viejas: HU v0.4.0, arquitectura v1.2.0 y RN v0.9.0 (AUD-20) | comprensión | BAJA | Actualizarlas a HU v0.4.1, arquitectura v1.2.1 y RN v0.9.1 en la próxima versión | Todas |

**Resumen por severidad:**

| Severidad | Cantidad | Observaciones |
|---|---|---|
| CRÍTICA | 0 | — |
| ALTA | 3 | OBS-35, OBS-36 y OBS-37 |
| MEDIA | 12 | OBS-38 a OBS-45 y OBS-47 a OBS-50 |
| BAJA | 3 | OBS-46, OBS-51 y OBS-52 |

Siguen abiertas tres observaciones de la ronda 1 que dependen del asesor legal: OBS-07 (P-040), OBS-24 (campos de P-015) y OBS-26 (derechos sobre los datos).

---

## 5. Decisiones

Cambios recomendados **antes de implementar** las tareas afectadas, en orden de prioridad:

| # | Decisión o cambio | Responde a | Quién decide | Tareas que bloquea |
|---|---|---|---|---|
| D-17 | Corregir los textos de aprobación de Yape y Plin: el día anterior a cada entrega, no «cada viernes» | OBS-37 | UI Designer (sin decisión de negocio) | T-076, T-086 |
| D-18 | Decidir si se puede sustituir la leche por la de otra finca, y escribir la RN de asignación por producto | OBS-35, OBS-41 | Fundador | T-056, T-074, T-077 |
| D-19 | Corregir RN-124 para varias suscripciones | OBS-36 | Fundador | T-059, T-072, T-086 |
| D-20 | Corregir los ejemplos de P-007: el saldo va al martes, y la reprogramada se muestra en su tarjeta de origen, sin cobro | OBS-38, OBS-39 | UI Designer | T-086 |
| D-21 | Permitir «Fallida» con lectura alta (precisión de RN-156) | OBS-40 | Fundador | T-078, T-088 |
| D-22 | Agregar la RN de recepción cerrada para confirmar la asignación, y fijar qué pasa al editar el pedido con lotes registrados | OBS-41, OBS-42 | BA y fundador | T-077, T-087 |
| D-23 | Agrupar en una parada las entregas del mismo cliente y la misma dirección | OBS-43 | BA | T-056, T-079, T-088 |
| D-24 | Decidir si quien recibió leche de un lote invalidado recibe una devolución, y ajustar CO-05 | OBS-44 | Fundador | T-058, T-085 |
| D-25 | Llevar el token de recuperación en el fragmento del enlace | OBS-45 | UI Designer y arquitecto | T-075, T-086 |
| D-26 | Bloquear el cambio de precio con suscriptores mientras P14 esté abierta | OBS-48 | Fundador | T-054, T-087 |
| D-27 | Mostrar en P-036 solo los efectos que de verdad se ejecutan mientras G8 esté cerrada | OBS-49 | UI Designer | T-087 |
| D-28 | Escribir la RN de recálculo de plazos de reclamos al cargar o retirar feriados | OBS-50 | BA y asesor legal | T-081 |
| D-29 | Precisar RN-098 («no cancelada») | OBS-47 | Fundador | T-050 |

Las observaciones BAJA (OBS-46, OBS-51 y OBS-52) pueden resolverse durante la implementación. Siguen vigentes, de la ronda 1: D-07 (asesor legal), D-16 (aprobar HU-017 y HU-018) y la aprobación del contrato de la §4.14 de la arquitectura.

---

## 6. Score de completitud

### 6.1 HU Must

| Métrica | Ronda 1 (v0.1.0) | Estimado tras subsanar (§A.7.3) | **Ronda 2 (v0.2.1)** | Cálculo |
|---|---|---|---|---|
| HU Must representadas | 100 % (17/17) | 100 % (18/18) | **100 %** (18/18) | — |
| HU Must cubiertas por completo | 41.2 % | 94.4 % | **38.9 %** (7/18) | HU-001, 003, 005, 008, 012, 017 y 018 |
| Cobertura ponderada | 67.6 % | 94.4 % | **66.7 %** | (7 × 1 + 10 × 0.5 + 1 × 0) / 18 |

### 6.2 Reglas de negocio

| Métrica | Ronda 1 | Estimado tras subsanar | **Ronda 2** | Cálculo |
|---|---|---|---|---|
| Reglas que deberían verse | 129 | 159 | **159** | 185 − 22 invisibles − 4 SHOULD |
| Reflejadas por completo | 85.3 % | 99.4 % | **91.8 %** (146) | — |
| Reflejadas, contando las parciales | 93.8 % | 100 % | **100 %** (159) | — |
| Deberían verse y no se ven | 6.2 % | 0 % | **0 %** | — |

> **Lectura:**
> - **Las cifras de §A.7.3 eran optimistas.** Medían si cada observación de la ronda 1 tenía respuesta en la especificación, no si esa respuesta era coherente con el resto. Esta ronda revisa el resultado y encuentra 18 observaciones nuevas.
> - **Las regresiones vienen sobre todo de las suscripciones múltiples**, una decisión del fundador de la misma ronda: chocan con reglas y ejemplos pensados para una sola suscripción (OBS-36, OBS-38, OBS-39, OBS-43 y OBS-47).
> - **Ninguna observación es CRÍTICA.** Las 3 ALTA tienen una corrección acotada. La de mayor impacto inmediato es OBS-37, porque haría perder entregas a los clientes de martes y jueves, y es solo de redacción.
> - **Si se resuelven D-17 a D-21**, la cobertura completa subiría a cerca del 55.6 % (10 HU: se suman HU-004, HU-006 y HU-007) y la ponderada, a cerca del 75.0 %. Si además se resuelven D-22 a D-29, llegaría a cerca del 94.4 %. Las reglas reflejadas por completo llegarían al 99.4 %. HU-016 seguiría bloqueada por el asesor legal.

---

# Anexo A · Ronda 1

Revisión de la UI spec v0.1.0, con las observaciones OBS-01 a OBS-34, y estado de su subsanación (versiones 0.1.0 a 0.2.1 de este registro). Se conserva sin cambios, salvo la numeración de las secciones.

**Leyenda.**
- **Severidad:**
  - **CRÍTICA:** impide completar una tarea principal.
  - **ALTA:** produce un resultado incorrecto o irreversible, o bloquea la salida a producción.
  - **MEDIA:** la tarea se completa con fricción o con información incompleta.
  - **BAJA:** pulido o claridad.
- **Tipo:**
  - *comprensión:* la UI o la regla se puede leer de dos maneras;
  - *gap:* falta una pantalla, un estado o una acción;
  - *fricción:* la tarea se completa, pero con pasos de más o sin guía;
  - *mejora:* conviene agregarlo, aunque no falta.

---

## A.1. Cobertura de HUs

Las 17 HU del PMV son **Must**. **Las 17 tienen al menos una pantalla**, así que ninguna HU Must falta por completo. Revisando criterio por criterio, 7 HU quedan cubiertas del todo y 10 tienen huecos.

| HU | Título | Pantallas | Cobertura | Qué falta (observación) |
|---|---|---|---|---|
| HU-001 | Registrarme como cliente | P-003, P-004 | ✅ Completa | Todos los criterios están: edad, consentimientos separados y desmarcados, distritos y correo duplicado. La recuperación de contraseña no está en ninguna HU (OBS-08) |
| HU-002 | Darme de baja | P-014 | 🟡 Parcial | Falta el bloqueo por entrega `programada` y no se dice cuándo podrá darse de baja (OBS-22). No se pueden rectificar datos ni revocar consentimientos (OBS-26) |
| HU-003 | Ver la leche disponible y su finca | P-001, P-002 | ✅ Completa | El estado de catálogo vacío lo decidió la UI y no una regla (OBS-33) |
| HU-004 | Suscribirme a una entrega semanal | P-005, P-006, P-007 | 🟡 Parcial | La vista previa del importe y de las fechas no tiene de dónde salir antes de confirmar (OBS-04). «Agregar dirección» no tiene pantalla (OBS-05). Falta decidir entre una o varias suscripciones y la elección de producto (OBS-18) |
| HU-005 | Pausar y reactivar | P-007, P-008 | ✅ Completa | La vista previa de P-008 tiene el mismo problema de origen de datos (OBS-04). SR-06 y SR-07 siguen abiertas |
| HU-006 | Modificar o cancelar | P-009, P-010 | 🟡 Parcial | Cambiar la dirección depende de «Agregar dirección», que no existe (OBS-05). Hay una vista previa sin origen de datos (OBS-04) |
| HU-007 | Pagar cada ciclo automáticamente | P-006, P-007, P-011 | 🟡 Parcial | No está definido el cobro recurrente con Yape o Plin (OBS-06). No se muestran los pagos `pendiente` ni `en_verificacion` (OBS-12) |
| HU-008 | Recibir mi comprobante | P-012, P-013, P-041 | 🟡 Parcial | Un cliente que elige factura sin RUC no recibe aviso (OBS-15). La alerta de emisión fallida tiene otra severidad y ninguna acción (OBS-11) |
| HU-009 | Recibir un reembolso | P-007, P-011, P-012 | 🟡 Parcial | No se ve el cobro neto con saldo a favor (OBS-13). La cancelación de una entrega y su reembolso automático no se avisan (OBS-14) |
| HU-010 | Recibir lotes con control de temperatura | P-030 a P-033, P-041 | 🟡 Parcial | La escasez se calcula sin un cierre de la recepción del día (OBS-03). «Litros comprometidos» se muestra por lote y no por día, y el pedido no se separa por finca (OBS-19) |
| HU-011 | Cerrar mis entregas en ruta | P-020 a P-022 | 🟡 Parcial | Sin conexión no se ve la dirección (OBS-01, K9). Una lectura alta no tiene estado de cierre (OBS-02). Un rechazo al sincronizar no tiene salida (OBS-10, K8) |
| HU-012 | Registrar un reclamo | P-015, P-016, P-039 | ✅ Completa | Hay que validar los campos mínimos de la hoja de reclamación (OBS-24) |
| HU-013 | Cambiar el precio por litro | P-035, P-036, P-041 | ✅ Completa (1 criterio ⛔ P14) | No se dice qué pasa con la propuesta si la confirmación se cancela (OBS-31) |
| HU-014 | Anular con doble confirmación | P-036, P-037 | 🟡 Parcial | No están definidos ni se muestran los efectos de cada tipo de anulación (OBS-09) |
| HU-015 | Cargar feriados | P-038 | ✅ Completa | No se puede corregir un feriado mal cargado (OBS-21) |
| HU-016 | Responder un reclamo | P-039, P-040 | ⛔ Bloqueada | P-040 está deshabilitada hasta que el asesor legal valide RN-146 a RN-149, y P24 sigue abierta (OBS-07) |
| HU-017 | Registrar fincas y productos (⏳ propuesta) | P-034, P-002 | ✅ Completa | La HU todavía no está aprobada. No se define qué pasa al despublicar un producto con suscriptores (OBS-23) |

**HU Must sin ninguna pantalla:** ninguna.
**Pantallas sin HU:** ninguna. Las 31 trazan a una HU.

---

## A.2. Reglas de negocio

Clasificación de las **154 reglas** (150 MUST y 4 SHOULD):

| Clase | Cantidad | Significado |
|---|---|---|
| ✅ Reflejada | 110 | Tiene un elemento visible en la UI: campo, validación, mensaje, estado o texto |
| 🟡 Parcial | 11 | Está citada, pero la UI la muestra incompleta o la deja ambigua |
| ❌ Falta | 8 | El usuario debería percibirla y ninguna pantalla la muestra |
| ⚪ Invisible por diseño | 21 | Es del backend (cifrado, idempotencia, reintentos, UTC); no hace falta mostrarla |
| ➖ SHOULD fuera del PMV | 4 | RN-042, RN-043, RN-044 y RN-117: sin HU, según el alcance |

### A.2.1 Reglas reflejadas (110)

Están en la UI con su elemento visible:
- RN-001 a RN-005, RN-008 a RN-022, RN-024 a RN-027, RN-029 a RN-038 y RN-041;
- RN-045, RN-047 a RN-054, RN-057, RN-058, RN-061 a RN-065, RN-068, RN-070, RN-071 y RN-076 a RN-079;
- RN-081 a RN-090, RN-093, RN-094, RN-099 a RN-102 y RN-109 a RN-116;
- RN-119 a RN-124, RN-126 a RN-130, RN-132 a RN-134, RN-139 y RN-141 a RN-144;
- RN-146 a RN-154.

Notas:
- RN-146 a RN-149 están diseñadas en P-040, pero la pantalla está **bloqueada** (OBS-07).
- RN-115 aparece solo como el marcador `[TEXTO LEGAL: consentimiento.principal]`, porque el texto lo redacta un humano.

### A.2.2 Reglas parciales (11)

| RN | Qué falta en la UI | Pantalla | OBS |
|---|---|---|---|
| RN-028 | El lote pide «litros comprometidos», pero P-031 muestra la demanda del día completo (100 L) en cada lote, sin repartirla por finca | P-031 | OBS-19 |
| RN-056 | El saldo a favor se ve, pero no cómo se descuenta del próximo cobro | P-007 | OBS-13 |
| RN-059 | Si el cliente eligió factura pero no tiene RUC, se emite boleta sin avisarle | P-003, P-007, P-013 | OBS-15 |
| RN-075 | P-011 dice el motivo «pago tardío», pero no explica que esa entrega no saldrá | P-007, P-011 | OBS-12 |
| RN-091 | No se ve el pedido por finca: la demanda solo aparece como un total | P-030 | OBS-19 |
| RN-092 | La escasez se calcula al registrar cada lote, antes de que llegue el resto | P-031 | OBS-03 |
| RN-098 | El 409 no lista la entrega `programada` como bloqueo | P-014 | OBS-22 |
| RN-103 | El aviso de precio está marcado ⛔ P14 y no tiene diseño | P-035 | — (P14) |
| RN-106 | La alerta se muestra como «ALTA» aunque la regla la define como **crítica**, y no ofrece ninguna acción | P-041 | OBS-11 |
| RN-131 | La vuelta a `activa` tras un pago tardío no tiene banner, porque el banner de éxito existe solo en el camino de P-006 | P-007 | OBS-12 |
| RN-145 | Sin conexión se guardan el orden, los litros y el saldo de botellas, que van más allá de lo que la regla permite literalmente | P-020, P-021 | OBS-20 |

### A.2.3 Reglas que deberían verse y no se ven (8)

| RN | Qué debería percibir el usuario | Quién | OBS |
|---|---|---|---|
| RN-066 | Que su entrega fallida se reprogramó para el siguiente día de reparto, y para qué fecha | Cliente (P-007) y repartidor (P-021) | OBS-14 |
| RN-074 | Que su pago con Yape o Plin está pendiente y que, si no lo aprueba en 1 800 s, la entrega se suspende | Cliente | OBS-06, OBS-12 |
| RN-096 | Cuánto se le cobrará de verdad cuando el saldo cubre solo una parte | Cliente (P-007) | OBS-13 |
| RN-097 | Que el ciclo lo cubre el saldo y que no habrá cargo | Cliente (P-007) | OBS-13 |
| RN-107 | Que una entrega reprogramada que vuelve a fallar queda cancelada y no se reprograma otra vez | Cliente y repartidor | OBS-14 |
| RN-108 | Que se le reembolsó una entrega cancelada | Cliente | OBS-14 |
| RN-135 | Que su pago está en verificación, porque la pasarela no respondió | Cliente y operador | OBS-12 |
| RN-140 | La nueva fecha de una reprogramada trasladada por feriado: la regla manda un correo, pero no hay plantilla y P-007 no la muestra | Cliente | OBS-14, OBS-16 |

### A.2.4 Reglas invisibles por diseño (21)

No hace falta mostrarlas:
- RN-006, RN-007, RN-023, RN-039, RN-040 y RN-046;
- RN-055, RN-060, RN-067, RN-069, RN-072 y RN-073;
- RN-080, RN-095, RN-104, RN-105, RN-118 y RN-125;
- RN-136, RN-137 y RN-138.

Se verifican con tests de backend (T-NNN del backlog), no en la UI.

---

## A.3. Flujo de usuario

### A.3.1 Tareas principales

| Tarea | Flujo | ¿Se completa? | Fricción o corte |
|---|---|---|---|
| Conocer la leche y suscribirse | F-01 | 🟡 Con fricción | La vista previa no tiene origen de datos (OBS-04). Falta «Agregar dirección» (OBS-05). Un cliente que ya tiene cuenta pierde la intención de suscribirse al ingresar (OBS-27) |
| Pausar o reactivar | F-02 | ✅ | El texto de la hora límite no es igual en todas las pantallas (OBS-28) |
| Modificar o cancelar | F-03 | 🟡 | Cambiar la dirección no se puede, porque falta «Agregar dirección» (OBS-05) |
| Regularizar un cobro rechazado | F-04 | ✅ | — |
| Pagar con Yape o Plin cada semana | F-01, F-04 | ❌ Indefinido | Ninguna pantalla pide aprobar el cobro de la semana (OBS-06) |
| Elegir la devolución | F-05 | ✅ | La elección tardía (SR-10) la decidió la UI, no una regla (OBS-33) |
| Comprobantes y factura | F-06 | 🟡 | Elegir factura al registrarse no lleva a cargar el RUC (OBS-15) |
| Darse de baja | F-07 | 🟡 | Con un 409 por entrega o reclamo pendiente no hay acción ni fecha estimada (OBS-22) |
| Registrar un reclamo | F-08 | ✅ | Hay que validar los campos legales (OBS-24) |
| Mañana de operación | F-09 | 🟡 | La escasez se calcula antes de tiempo (OBS-03); falta el pedido por finca (OBS-19) |
| Ruta del repartidor | F-10 | ❌ Sin señal | Sin conexión no hay dirección (OBS-01). Con temperatura alta no hay estado de cierre (OBS-02) |
| Cambiar el precio | F-11 | ✅ | El destino de la propuesta cancelada no está definido (OBS-31) |
| Anular un registro | F-12 | 🟡 | El administrador confirma sin saber los efectos (OBS-09) |
| Fincas y productos | F-13 | ✅ (⏳ HU-017) | No se define qué pasa al despublicar (OBS-23) |
| Feriados | F-14 | 🟡 | No hay corrección y el efecto se ve recién al guardar (OBS-21) |
| Atender un reclamo | F-15 | ❌ Bloqueado | P-040 está deshabilitada (OBS-07) |

### A.3.2 Callejones sin salida

| # | Dónde | Qué pasa | OBS |
|---|---|---|---|
| CS-1 | P-020 y P-021 sin conexión | El repartidor ve «Dirección no disponible sin conexión» y no sabe a dónde ir | OBS-01 |
| CS-2 | P-021 con más de 4.5 °C | «Entregada» está bloqueada y «Fallida» pide una causa que no corresponde: no hay ningún cierre válido | OBS-02 |
| CS-3 | P-022, ítem «Rechazado» | La leche ya se entregó en la puerta, pero el sistema la rechaza, y [Ver en la ruta] no ofrece ninguna acción | OBS-10 |
| CS-4 | P-004 | Ni el cliente ni el administrador titular pueden recuperar la contraseña. Sin contraseña, el administrador tampoco puede hacer la doble confirmación | OBS-08 |
| CS-5 | P-040 | El reclamo llega al día 10 (alerta de P-041), pero la respuesta no se puede enviar | OBS-07 |
| CS-6 | P-041, alerta de comprobante o reembolso agotado | La alerta no tiene ninguna acción (reintentar o marcar como resuelta) | OBS-11 |
| CS-7 | P-038, feriado mal cargado | No hay forma de editarlo o quitarlo, y ya trasladó entregas | OBS-21 |
| CS-8 | P-014, 409 por entrega `en_ruta` o reclamo abierto | El cliente no puede hacer nada y no sabe cuándo podrá darse de baja | OBS-22 |
| CS-9 | P-005 y P-009, [+ Agregar dirección] | El botón lleva a una pantalla que no está especificada | OBS-05 |

---

## A.4. Observaciones

| # | Observación | Tipo | Severidad | Acción sugerida | HU afectada |
|---|---|---|---|---|---|
| OBS-01 | Sin conexión, P-020 y P-021 no muestran la dirección ni la referencia (K9, RN-145). Como el modo sin conexión existe justamente para la ruta, la tarea principal del repartidor no se puede completar sin señal | gap | CRÍTICA | Resolver **K9** antes de implementar el módulo C3: ruta impresa desde P-033, o un ADR para guardar la dirección cifrada en el teléfono. Rediseñar P-020 y P-021 según lo que se decida | HU-011 |
| OBS-02 | En P-021, una lectura de más de 4.5 °C bloquea «Entregada» (RN-076), y RN-029 solo admite «entregada» o «fallida». La UI dice que el cierre «se envía como lectura alta», pero no existe ese estado. RN-076 y RN-132 dicen que la entrega queda **retenida**, sin decir cómo lo registra el repartidor ni qué hace si el lote se revierte | comprensión | ALTA | Definir una RN para la lectura alta en la puerta: el registro de la lectura sin cierre y el estado de la entrega mientras el lote está `en_revision`. Diseñar en P-021 la acción «Registrar temperatura alta» y el estado «Retenida» en P-020 | HU-011, HU-010 |
| OBS-03 | P-031 muestra la escasez («2 entregas sin cubrir») al guardar cada lote. Si el día tiene 2 lotes (P-030 muestra L-…01 y L-…02), las entregas pasarían a `fallida` (RN-092), con su aviso de devolución (RN-129), antes de que llegue el segundo lote. Es irreversible: el cliente ya recibió el aviso | gap | ALTA | Agregar una RN y una acción explícita: **«Cerrar recepción del día»** en P-030, solo después de la cual se aplican RN-092 y RN-122. Mientras tanto, mostrar «Faltan 6 L» solo como advertencia | HU-010, HU-009 |
| OBS-04 | P-005 (pasos 1 y 3), P-008 (vista previa), P-009 («desde cuándo aplica») y P-010 («qué pasa con la próxima entrega») muestran fechas e importes **antes** de confirmar. La UI dice que esos datos los da la API, pero el único origen de datos indicado es la respuesta de la acción ya confirmada (`POST /suscripciones`, `POST …/pausas`, `PATCH`, `POST …/cancelacion`), y la PWA no debe calcular fechas de corte | gap | ALTA | Definir en la arquitectura un endpoint de simulación (por ejemplo `?simular=true`) que devuelva `primera_entrega`, `entregas_omitidas`, `aplica_desde` e `importe` sin guardar nada. El contrato de la API lo aprueba un humano. Hasta entonces, P-005 a P-010 no se pueden implementar como están | HU-004, HU-005, HU-006 |
| OBS-05 | P-005 (paso 2) y P-009 tienen [+ Agregar dirección], pero ninguna pantalla lo especifica. El registro (P-003) captura una sola dirección, así que no hay forma de **cambiar la dirección**, que es un criterio de HU-006 | gap | ALTA | Especificar una pantalla nueva, **P-017 · Agregar dirección** (línea, referencia y distrito en lista cerrada; error 422 RN-005), con vuelta a la pantalla de origen | HU-006, HU-004 |
| OBS-06 | P-006 dice «Autoriza el cobro semanal en tu app» para Yape y Plin. Pero RN-074 (pago pendiente hasta 1 800 s) sugiere que el cliente aprueba **cada** cobro. Ninguna regla define si existe la autorización recurrente, y ninguna pantalla ni aviso pide aprobar el cobro del viernes a las 18:00. Si se necesita aprobar cada semana, el cliente no se entera y su entrega queda suspendida | comprensión | ALTA | Confirmar con Yape y Plin cómo funciona el cobro recurrente (gate G5, credenciales de sandbox). Si se aprueba cada ciclo, agregar una RN para el aviso y un banner en P-007: «Aprueba tu pago de S/ 43.50 en Yape antes de las 18:30» | HU-007 |
| OBS-07 | P-040 está deshabilitada hasta que el asesor legal valide RN-146 a RN-149, y P24 (derivación) sigue abierta. La Ley 31435 pone un plazo de 15 días hábiles, y P-041 avisa el día 10, pero el sistema no permite responder | gap | ALTA | Antes de producción, conseguir la validación legal o aprobar un procedimiento provisional fuera del sistema con registro manual. Decisión del fundador con el asesor legal | HU-016, HU-012 |
| OBS-08 | P-004 no tiene «¿Olvidaste tu contraseña?». Un cliente que la olvida no puede gestionar su suscripción. El administrador titular necesita su contraseña para la doble confirmación (RN-151), así que perderla bloquea los cambios de precio y las anulaciones. Ninguna HU cubre la recuperación | gap | ALTA | Decidir si el PMV incluye la recuperación de contraseña. Si la incluye, escribir la HU y la RN (enlace de un solo uso enviado por correo, con plazo) y la pantalla propuesta **P-018 · Recuperar contraseña**. Si no la incluye, definir un procedimiento de soporte | HU-001, HU-013, HU-014 |
| OBS-09 | P-037 deja anular un pago, un comprobante, una suscripción o una entrega, pero ninguna RN dice qué produce cada anulación: si hay reembolso o nota de crédito, si se avisa al cliente, si se cancelan las entregas. P-036 tampoco lo muestra. El administrador confirma una acción crítica sin saber su efecto | comprensión | ALTA | Escribir una RN por tipo de anulación con sus efectos. Mostrarlos en P-037 y en el recuadro «Qué vas a confirmar» de P-036. La anulación de comprobantes ante el OSE queda pendiente de G4 | HU-014 |
| OBS-10 | P-022 muestra «✕ Rechazado: Lote en revisión» para una entrega que el repartidor ya dio en la puerta sin conexión (K8). El estado físico (leche entregada) y el del sistema quedan distintos, y no hay acción para conciliarlos | gap | MEDIA | Resolver **K8**. Agregar en P-022 la acción «Reportar al operador», que cree una alerta en P-041, y un estado de entrega para conciliar | HU-011 |
| OBS-11 | En P-041, la alerta «No se emitió la boleta… tras 5 reintentos» aparece como **ALTA**, y RN-106 la define como **crítica**. Además, ni esa alerta ni la de reembolso agotado (RN-105) tienen acción: reintentar, ver el pago o marcarla como resuelta | gap | MEDIA | Usar «CRÍTICA» para RN-106. Agregar acciones por tipo de alerta: [Reintentar emisión], [Ver pago] y [Marcar resuelta con nota]. Agregar al ejemplo una alerta de reembolso agotado | HU-008, HU-009 |
| OBS-12 | P-007 lista «último pago y su estado» entre sus datos, pero ni el wireframe ni la tabla de estados muestran el pago `pendiente` de Yape o Plin (RN-074), el pago `en_verificacion` (RN-135) ni la vuelta a `activa` tras un pago tardío (RN-131). Durante hasta 30 minutos, el cliente no sabe si pagó | gap | MEDIA | Agregar banners en P-007: «Estamos esperando la confirmación de tu pago», «Estamos verificando tu pago con el banco» y «Tu pago llegó tarde: la entrega del 14/11 no saldrá. Elige qué hacer con S/ 43.50». Mostrar el estado del pago al operador | HU-007 |
| OBS-13 | P-007 muestra «S/ 43.50 por entrega» y «Saldo a favor: S/ 14.50», pero no cuánto se cobrará de verdad: S/ 29.00 con saldo parcial (RN-096), o S/ 0.00, «se paga con tu saldo», con saldo suficiente (RN-097) | comprensión | MEDIA | En la tarjeta «Próxima entrega», mostrar «Cobro: S/ 29.00 (S/ 43.50 − S/ 14.50 de saldo)», o «Sin cargo: se paga con tu saldo a favor». El dato lo da la API | HU-009 |
| OBS-14 | Al cliente no se le muestra que su entrega se **reprogramó** (RN-066) ni la nueva fecha. Tampoco que se canceló al fallar otra vez (RN-107) ni que se le reembolsó (RN-108). Sobre el traslado por feriado (RN-140) solo hay un correo. Además, P-021 no le dice al repartidor que una entrega es reprogramada y que un segundo fallo la cancela | gap | MEDIA | Agregar a P-007 el estado de la entrega: «Reprogramada al martes 17/11/2026», «Cancelada: te reembolsamos S/ 43.50». Marcar en P-020 y P-021 «Reprogramada (último intento)» | HU-011, HU-009 |
| OBS-15 | En P-003, si el cliente elige «Factura → se completa en P-013», el flujo sigue a P-005 sin pedirle el RUC. Por RN-059 se emite boleta sin avisarle | fricción | MEDIA | Pedir el RUC en el registro cuando se elige factura, o mostrar en P-007 el banner «Elegiste factura, pero falta tu RUC» [Completar] → P-013 | HU-008 |
| OBS-16 | La UI spec no especifica los **correos** que reciben los usuarios, aunque son parte de su interfaz y varias reglas dependen de ellos: RN-049, RN-078, RN-112, RN-129, RN-140, RN-148 y RN-151 (el código) | gap | MEDIA | Agregar una sección «Plantillas de correo», con asunto, contenido mínimo, clave `[t: …]` y enlace a la pantalla. Los textos legales y comerciales los redacta un humano | HU-007, HU-009, HU-011, HU-012, HU-013, HU-014, HU-016 |
| OBS-17 | La versión revisada (1.0.0) usa los componentes E-01 a E-12 (encabezado, alertas, banners, errores, sin conexión, diálogo, barra lateral), pero **no trae sus secciones 6 y 7**. Por eso no hay especificación de E-03, E-05, E-07 ni E-08, que cargan reglas como la alerta del lote y la confirmación de acciones peligrosas | gap | MEDIA | Entregar la versión completa, con §6 y §7 (están en `09-ui-spec.md` v0.1.0 del proyecto), y alinear el número de versión con el del proyecto | Todas |
| OBS-18 | No está definido si un cliente puede tener **varias suscripciones**, por ejemplo martes y sábado. RN-016 fija 1 día por suscripción, y P-007 está pensada para una sola. Tampoco está definido qué producto o finca elige el cliente en P-005 cuando P-034 tiene más de uno publicado | comprensión | MEDIA | Decidir entre una y varias suscripciones por cliente, y si hay elección de producto. Ajustar P-005, P-007 y el destino de «Suscribirme» con sesión | HU-004, HU-003 |
| OBS-19 | P-031 precarga «Litros comprometidos: 100» en **cada** lote, pero RN-090 y RN-091 hablan de la demanda **del día**. Con 2 fincas no se sabe cuánto se pidió a cada una, y P-030 no muestra el pedido por finca, que es lo que pide HU-010 («saber cuántos litros pedir a la finca») | comprensión | MEDIA | Definir si el pedido se reparte por finca (y con qué regla) o es uno solo. Mostrar en P-030 el pedido por finca, y en P-031 lo comprometido de esa finca | HU-010 |
| OBS-20 | RN-145 permite guardar sin conexión «solo los identificadores de las entregas y los datos del cierre», pero P-020 y P-021 guardan también el orden, los litros y el saldo de botellas del cliente. No son datos personales, pero la regla no los menciona | comprensión | MEDIA | Ampliar RN-145 a una lista cerrada: identificador, orden, litros, saldo de botellas y cierre. Así un test literal de la regla no falla por datos que sí se necesitan | HU-011 |
| OBS-21 | P-038 solo permite agregar feriados. Un feriado mal cargado ya trasladó entregas (RN-123) y no se puede corregir. El efecto («Traslada 14 entregas») se ve recién después de guardar, y no está definido qué pasa si el feriado se carga después del corte de ese día | gap | MEDIA | Mostrar el efecto antes de confirmar. Definir una RN para corregir o retirar un feriado y para la carga posterior al corte, y decidir si retirarlo exige doble confirmación | HU-015 |
| OBS-22 | El 409 de P-014 no incluye la entrega `programada`, que según RN-098 bloquea la baja. Tampoco dice cuándo se podrá dar de baja, y para «entrega en curso» o «reclamo sin respuesta» no ofrece ninguna acción | fricción | MEDIA | Listar los 4 bloqueos de RN-098 con su fecha estimada de liberación: «Podrás darte de baja después de tu entrega del sábado 14/11/2026» o «cuando respondamos tu reclamo LR-…» | HU-002 |
| OBS-23 | P-034 deja despublicar un producto, pero no define qué pasa con las suscripciones activas de ese producto: si se siguen cobrando y entregando o se cancelan | gap | MEDIA | Escribir una RN para la despublicación con suscriptores. Si los hay, mostrar una advertencia en P-034 con el número de afectados | HU-017, HU-004 |
| OBS-24 | P-015 pide tipo, nombre, correo, detalle y pedido. El contenido mínimo de la hoja de reclamación lo fija la normativa del Libro de Reclamaciones, pero ninguna RN lo lista, y SR-15 (correo obligatorio) sigue abierta | comprensión | MEDIA | Que el asesor legal valide los campos obligatorios del formulario y, a partir de eso, escribir una RN y ajustar P-015 | HU-012 |
| OBS-25 | No hay pantalla ni HU para dar de alta a operadores, administradores ni repartidores. P-033 asigna repartidores que nadie puede crear, salvo por siembra técnica | gap | MEDIA | Decidir si el PMV crea los usuarios internos con un script de siembra controlado por un humano o con una pantalla (y su HU). Documentarlo en el backlog | HU-010, HU-011 |
| OBS-26 | P-014 solo muestra el correo y el celular enmascarados. El cliente no puede rectificar sus datos ni revocar el consentimiento de envío a terceros (RN-004, RN-116), que la normativa de datos personales le reconoce | gap | MEDIA | Que el asesor legal confirme qué derechos deben poder ejercerse desde la PWA. Escribir la HU y la RN correspondientes; mientras tanto, mostrar un canal de contacto | HU-002, HU-001 |
| OBS-27 | En F-01, «Suscribirme» sin sesión lleva a P-003. Un cliente que ya tiene cuenta pasa a P-004, que lo redirige a P-007 (RN-099) y le hace perder la intención de suscribirse | fricción | BAJA | Conservar el destino («volver a») al pasar por P-003 y P-004, y tras el ingreso ir a P-005 | HU-004, HU-001 |
| OBS-28 | La hora límite se dice de tres maneras: P-007 dice «hasta… a las 17:59», P-005 dice «hasta las 18:00» y P-010 dice «después de hoy». El corte es a las 18:00:00, exclusivo (RN-012, RN-013 y RN-014), así que «17:59» parece excluir los segundos válidos y «hasta las 18:00» parece incluirlas | comprensión | BAJA | Usar un solo texto en todas: «antes de las 18:00 del viernes 13/11/2026». En P-010, «No recibirás la entrega del sábado 14/11/2026 ni las siguientes» | HU-005, HU-006 |
| OBS-29 | P-010 y P-014 dicen «Te devolvemos tu saldo… a tu Tarjeta •••• 4242», pero RN-124 manda reembolsar al medio con que se pagó el ciclo que generó el saldo, que puede no ser el actual | comprensión | BAJA | Que la API devuelva el medio de destino y la pantalla lo muestre tal cual | HU-006, HU-002 |
| OBS-30 | El ejemplo de «Invalidado» en P-032 no cuadra: un lote con 12 entregas tiene 12 reasignadas, 5 en ruta suspendidas y 7 avisos de disculpa. Así se especificarían mal los tests de UI | comprensión | BAJA | Corregir el ejemplo a números coherentes con RN-077, RN-078 y RN-093, por ejemplo 12 entregas: 7 reasignadas, 3 en ruta suspendidas y 2 avisos de disculpa | HU-010 |
| OBS-31 | No está definido qué pasa con la propuesta de P-035 si P-036 se cancela por RN-152: si queda pendiente o se descarta. Tampoco qué pasa si se hace una segunda propuesta con una pendiente | comprensión | BAJA | Definir en RN-153 que una cancelación descarta la propuesta y que solo puede haber una pendiente. Mostrarlo en P-035 | HU-013 |
| OBS-32 | P-035 dice «No afecta entregas ya cobradas», pero no dice desde qué cobro rige el precio nuevo (RN-102) | mejora | BAJA | Agregar «Rige desde el cobro del viernes 20/11/2026, 18:00», con la fecha que da la API | HU-013 |
| OBS-33 | La UI resolvió por su cuenta escenarios que no tienen regla: SR-04 (catálogo vacío en P-001), SR-05 («Siguiente» deshabilitado sin medio de pago en P-005), SR-07 (acciones ocultas en `suspendida_por_pago` en P-007) y SR-10 (409 por elección tardía en P-011). Si las reglas que se escriban después difieren, habrá que rehacer esas pantallas | comprensión | MEDIA | Que el BA escriba las RN de SR-04, SR-05, SR-07 y SR-10 y confirme o corrija lo que asumió la UI. Hasta entonces, mantener las marcas «pendiente» en esas pantallas | HU-003, HU-004, HU-005, HU-009 |
| OBS-34 | Después de P-016, el consumidor no tiene cómo consultar el estado de su reclamo | mejora | BAJA | Evaluar una consulta pública por código y correo, en una HU futura | HU-012 |

**Resumen por severidad:**

| Severidad | Cantidad | Observaciones |
|---|---|---|
| CRÍTICA | 1 | OBS-01 |
| ALTA | 8 | OBS-02 a OBS-09 |
| MEDIA | 18 | OBS-10 a OBS-26 y OBS-33 |
| BAJA | 7 | OBS-27 a OBS-32 y OBS-34 |

---

## A.5. Decisiones

Cambios recomendados **antes de implementar**, en orden de prioridad. Las que tocan lo comercial, lo legal, el contrato de la API o la constitución necesitan aprobación humana; este agente no las aplica.

| # | Decisión o cambio | Responde a | Quién decide | Bloquea |
|---|---|---|---|---|
| D-01 | Resolver **K9** (dirección sin conexión) y rediseñar P-020 y P-021 | OBS-01 | Fundador (privacidad, PRINCIPIO-003) | Módulo C3 |
| D-02 | Nueva RN para la lectura alta en la puerta (entrega retenida, sin cierre), con su acción en P-021 | OBS-02 | BA y fundador | HU-011 |
| D-03 | Nueva RN y acción **«Cerrar recepción del día»** antes de aplicar la escasez (RN-092 y RN-122) | OBS-03 | BA y fundador | HU-010 |
| D-04 | Endpoint de **simulación** para las vistas previas de P-005 y P-008 a P-010 | OBS-04 | Arquitecto, con aprobación humana del contrato | HU-004 a HU-006 |
| D-05 | Especificar **P-017 · Agregar dirección** | OBS-05 | UI Designer | HU-006 |
| D-06 | Confirmar con Yape y Plin el modelo de cobro recurrente; si cada ciclo se aprueba, agregar una RN y un aviso | OBS-06 | Fundador (G5) | HU-007 |
| D-07 | Validación legal de RN-146 a RN-149 y P24, o un procedimiento provisional | OBS-07, OBS-24, OBS-26 | Fundador y asesor legal | Salida a producción |
| D-08 | Decidir si el PMV incluye la recuperación de contraseña (HU nueva y **P-018**) | OBS-08 | Fundador | Operación del administrador |
| D-09 | RN de efectos por tipo de anulación, mostrados en P-036 y P-037 | OBS-09 | BA, fundador y contador | HU-014 |
| D-10 | Escribir las RN de SR-04, SR-05, SR-07 y SR-10 y confirmar lo que asumió la UI | OBS-33 | BA | HU-003 a HU-005, HU-009 |
| D-11 | Completar los estados de P-007: pago pendiente, en verificación, cobro neto con saldo, entrega reprogramada o cancelada | OBS-12 a OBS-14 | UI Designer | HU-007, HU-009 |
| D-12 | Agregar la sección de plantillas de correo y entregar la UI spec completa (§6 y §7), alineando la versión | OBS-16, OBS-17 | UI Designer (los textos legales los escribe un humano) | — |
| D-13 | Decidir entre una o varias suscripciones por cliente y la elección de producto | OBS-18 | Fundador | HU-004 |
| D-14 | Definir el pedido por finca y ampliar RN-145 a una lista cerrada de datos sin conexión | OBS-19, OBS-20 | BA y fundador | HU-010, HU-011 |
| D-15 | RN para corregir o retirar feriados, para despublicar con suscriptores y para crear usuarios internos | OBS-21, OBS-23, OBS-25 | BA y fundador | HU-015, HU-017 |
| D-16 | Aprobar la HU-017, que sigue propuesta | Cobertura | Fundador | HU-003 y HU-004 en producción |

Las observaciones BAJA (OBS-27 a OBS-32 y OBS-34) pueden resolverse durante la implementación, sin decisión previa, salvo OBS-31, que toca RN-153.

---

## A.6. Score de completitud

### A.6.1 HU Must

| Métrica | Valor | Cálculo |
|---|---|---|
| HU Must representadas en al menos una pantalla | **100 %** | 17 de 17 |
| HU Must cubiertas por completo | **41.2 %** | 7 de 17 (HU-001, 003, 005, 012, 013, 015 y 017) |
| Cobertura ponderada | **67.6 %** | (7 × 1 + 9 × 0.5 + 1 × 0) / 17. Completa = 1, parcial = 0.5, bloqueada = 0 (HU-016) |

### A.6.2 Reglas de negocio

| Métrica | Valor | Cálculo |
|---|---|---|
| Reglas que deberían verse en la UI | 129 | 154 − 21 invisibles por diseño − 4 SHOULD fuera del PMV |
| Reglas reflejadas por completo | **85.3 %** | 110 de 129 |
| Reglas reflejadas, contando las parciales | **93.8 %** | 121 de 129 |
| Reglas que deberían verse y no se ven | 6.2 % | 8 de 129 (RN-066, 074, 096, 097, 107, 108, 135 y 140) |

> **Lectura:** la UI cubre todo el mapa de HU y casi todas las reglas visibles. Lo que falta se concentra en **tres frentes**:
> - el modo sin conexión del repartidor (OBS-01 y OBS-02);
> - la recepción del día en operación (OBS-03);
> - los estados de pago y devolución que el cliente no ve (OBS-06 y OBS-12 a OBS-14).
>
> Con D-01 a D-06 y D-11 resueltas, se estima que la cobertura ponderada de HU subiría a cerca del 82 % (12 HU completas) y las reglas reflejadas por completo a cerca del 94 % (121 de 129). HU-016 seguiría bloqueada hasta la validación legal.

---

## A.7. Estado de subsanación (v0.2.0)

**Fecha:** 14/11/2026, después de las decisiones del fundador sobre K9, la recuperación de contraseña, Yape y Plin, y las suscripciones múltiples.

**Documentos actualizados:**
- [`03-reglas-de-negocio.md`](03-reglas-de-negocio.md) v0.9.0: RN-155 a RN-185 y RN-145 modificada.
- [`06-user-stories-refinadas.md`](06-user-stories-refinadas.md) v0.4.0: criterios nuevos y HU-018.
- [`07-architecture.md`](07-architecture.md) v1.2.0: §4.14 y tablas nuevas, **sin dependencias nuevas**.
- [`09-ui-spec.md`](09-ui-spec.md) v0.2.0: P-017, P-018 y P-042, y la §8 de correos.
- [`08-backlog.md`](08-backlog.md) v0.4.0: T-072 a T-090 y la compuerta G7.

**Leyenda:**
- ✅ subsanada en la especificación;
- ⏳ subsanada con una regla **propuesta** que espera aprobación (G7);
- ⛔ depende de un tercero (asesor legal);
- ➖ diferida a propósito.

### A.7.1 Observaciones

| # | Estado | Cómo quedó |
|---|---|---|
| OBS-01 | ✅ | Decisión del fundador (K9): hoja de ruta impresa. RN-155, RN-145 modificada; P-020, P-021, P-033 y nueva **P-042**; T-079 y T-088 |
| OBS-02 | ✅ | RN-156 ⏳: la lectura alta no cierra la entrega, que queda «Retenida». P-020, P-021 y P-022; T-078 y T-088 |
| OBS-03 | ✅ | RN-157 y RN-158 ⏳: «Cerrar recepción del día» y alerta a las 06:45. Antes del cierre, la escasez es solo advertencia. P-030, P-031 y P-033; T-077 y T-087 |
| OBS-04 | ✅ | RN-184 ⏳ y endpoints de simulación (arquitectura §4.14, contrato por aprobar). Componente E-13 en P-005, P-008, P-009 y P-010; T-072 y T-086 |
| OBS-05 | ✅ | Nueva pantalla **P-017**, con `GET` y `POST /direcciones`; T-073 |
| OBS-06 | ✅ | Decisión del fundador: aprobación en cada ciclo (RN-164). Textos en P-005 y P-006, banner en P-007 y correo CO-02; T-076. Falta confirmarlo en el sandbox (G5) |
| OBS-07 | ⛔ | Sin cambio: depende de la validación del asesor legal de RN-146 a RN-149 y de P24 (D-07) |
| OBS-08 | ✅ | Decisión del fundador: recuperación por correo. RN-165 a RN-167, nueva **HU-018** (propuesta) y nueva **P-018**; T-075. En producción depende de G6 |
| OBS-09 | ✅ ⚠️ | RN-168 a RN-172 ⏳, borrador que también debe validar el contador. Efectos en P-036 y P-037 con su simulación; T-080 |
| OBS-10 | ✅ | Propuesta para K8: RN-185 ⏳. En P-022, «Enviado con aviso» y [Avisar al operador]; T-078 y T-088. K8 sigue siendo decisión del fundador |
| OBS-11 | ✅ | P-041 con nivel CRÍTICA, [Reintentar emisión], [Reintentar reembolso] y [Marcar resuelta…]; tabla `alerta_interna`; T-082 |
| OBS-12 | ✅ | P-007 con banners de pago pendiente, en verificación y pago tardío; `GET /suscripciones` con el estado del pago; T-074 y T-086 |
| OBS-13 | ✅ | P-007 muestra el cobro neto («S/ 29.00 (S/ 43.50 − S/ 14.50 de saldo a favor)») o «Sin cargo» |
| OBS-14 | ✅ | RN-182 y RN-183 ⏳: avisos de reprogramación y de cancelación. P-007, P-020 y P-021 («Reprogramada · último intento»); correos CO-06 a CO-08; T-083 |
| OBS-15 | ✅ | P-003 pide el RUC al elegir factura; P-007 muestra el banner «Falta tu RUC» |
| OBS-16 | ✅ | Nueva §8 de la UI spec, con las plantillas CO-01 a CO-16; T-085 |
| OBS-17 | ✅ | `09-ui-spec.md` v0.2.0 es la versión vigente y completa, con las §6 a §8 |
| OBS-18 | ✅ | Decisión del fundador: varias suscripciones (RN-160). P-007 con tarjetas y P-005 con elección de producto (RN-161 ⏳); T-074 |
| OBS-19 | ✅ | RN-159 ⏳: pedido por finca que debe sumar la demanda. P-030 y P-031; T-077 |
| OBS-20 | ✅ | RN-145 con lista cerrada (la lista es propuesta ⏳; la hoja de ruta, decisión del fundador) |
| OBS-21 | ✅ | RN-173 a RN-175 ⏳: rechazo después del corte, retiro por el administrador titular y alerta anual. P-038 con vista previa; T-081 |
| OBS-22 | ✅ | P-014 lista los 4 bloqueos de RN-098, con su acción o su fecha de liberación |
| OBS-23 | ✅ | RN-163 ⏳: no se despublica un producto con suscriptores. P-034 muestra el 409; T-084 |
| OBS-24 | ⛔ | Sin cambio: el asesor legal debe validar los campos de la hoja de reclamación. P-015 lo marca ⛔ |
| OBS-25 | ✅ | RN-176 ⏳: los usuarios internos se crean con un comando de consola, sin pantalla; T-090 |
| OBS-26 | ⛔ | Sin cambio de fondo: P-014 muestra un canal de contacto `[TEXTO LEGAL]` hasta el dictamen legal |
| OBS-27 | ✅ | `?volver=` en P-001, P-002, P-003 y P-004 (solo rutas internas) |
| OBS-28 | ✅ | Texto único «antes de las 18:00 del…» en P-005, P-007 y P-010 |
| OBS-29 | ✅ | La simulación de cancelación y la baja devuelven el medio de destino del reembolso (P-010 y P-014) |
| OBS-30 | ✅ | Ejemplo corregido en P-032: 7 + 0 + 3 + 2 = 12 |
| OBS-31 | ✅ | RN-177 ⏳: una sola propuesta pendiente; la cancelada queda «Descartada» (P-035); T-084 |
| OBS-32 | ✅ | P-035 y P-036 muestran desde qué cobro rige el precio |
| OBS-33 | ✅ | SR-04, SR-05, SR-07 y SR-10 se convierten en RN-178 a RN-181 ⏳; las marcas de P-001, P-005, P-007 y P-011 citan la regla; T-084 |
| OBS-34 | ➖ | Diferida a una HU futura (UI spec §1.4) |

| Estado | Cantidad |
|---|---|
| ✅ Subsanada | 30 (13 de ellas pasaron de ⏳ a ✅ con la aprobación del fundador del 14/11/2026; OBS-09 todavía espera al contador) |
| ⛔ Depende del asesor legal | 3 (OBS-07, OBS-24 y OBS-26) |
| ➖ Diferida | 1 (OBS-34) |

### A.7.2 Decisiones

| # | Estado | Detalle |
|---|---|---|
| D-01 | ✅ | K9 resuelto por el fundador: hoja de ruta impresa (RN-155, P-042) |
| D-02 | ✅ | RN-156 aprobada (14/11/2026) |
| D-03 | ✅ | RN-157 y RN-158 aprobadas (14/11/2026) |
| D-04 | ✅ / ⏳ | RN-184 aprobada (14/11/2026). Falta aprobar el contrato de la API de la §4.14 de la arquitectura |
| D-05 | ✅ | P-017 especificada |
| D-06 | ✅ | Aprobación en cada ciclo (RN-164). Falta validarla en el sandbox (G5) |
| D-07 | ⛔ | Sigue pendiente: validación legal de RN-146 a RN-149, P24, campos de la hoja de reclamación y derechos sobre los datos |
| D-08 | ✅ | Recuperación incluida en el PMV. Falta aprobar la HU-018 |
| D-09 | ⚠️ | RN-168 a RN-172 aprobadas por el fundador (14/11/2026); falta la validación del contador (G8) |
| D-10 | ✅ | RN-178 a RN-181 aprobadas (14/11/2026) |
| D-11 | ✅ | Estados de P-007 especificados |
| D-12 | ✅ | §8 de plantillas y versión de la UI spec alineada |
| D-13 | ✅ | Varias suscripciones (RN-160); la elección de producto es la propuesta RN-161 |
| D-14 | ✅ | RN-159 y la lista cerrada de RN-145 aprobadas (14/11/2026) |
| D-15 | ✅ | RN-163 y RN-173 a RN-176 aprobadas (14/11/2026) |
| D-16 | ⛔ | Sigue pendiente: aprobar la HU-017, y ahora también la HU-018 |

### A.7.3 Score de completitud recalculado

**HU Must:** ahora son 18, con la HU-018 nueva.

| Métrica | Antes (v0.1.0) | Especificado (v0.2.0) | Solo con lo ya aprobado | Cálculo |
|---|---|---|---|---|
| HU Must representadas en al menos una pantalla | 100 % (17/17) | **100 %** (18/18) | 100 % | — |
| HU Must cubiertas por completo | 41.2 % (7/17) | **94.4 %** (17/18) | 27.8 % (5/18) | Solo con lo aprobado, quedan completas HU-001, HU-002, HU-007, HU-008 y HU-012; las demás dependen de una regla ⏳ o de aprobar su HU |
| Cobertura ponderada | 67.6 % | **94.4 %** | 61.1 % | Completa = 1, parcial = 0.5, bloqueada = 0. HU-016 sigue bloqueada (0) |

**Reglas de negocio:** ahora son 185.

| Métrica | Antes (v0.1.0) | Especificado (v0.2.0) | Solo con lo ya aprobado | Cálculo |
|---|---|---|---|---|
| Reglas que deberían verse en la UI | 129 | 159 | 159 | 185 − 22 invisibles por diseño (se suma RN-176) − 4 SHOULD |
| Reglas reflejadas por completo | 85.3 % (110) | **99.4 %** (158) | 83.0 % (132) | En la columna aprobada se restan las 26 reglas ⏳ visibles |
| Reglas reflejadas, contando las parciales | 93.8 % (121) | **100 %** (159) | — | La única parcial es RN-103 (⛔ P14) |
| Reglas que deberían verse y no se ven | 6.2 % (8) | **0 %** | — | — |

> **Lectura:** la especificación ya no tiene huecos funcionales propios. Lo que falta son **decisiones humanas**:
> 1. Aprobar o corregir las 27 reglas ⏳ (G7). Es lo que más mueve el score real: pasaría de 61.1 % a 94.4 % de cobertura ponderada de HU. Además, T-072, que espera G7, está en la ruta crítica del backlog.
> 2. Pedir al contador la validación de RN-168 a RN-172 (y de RN-125 y RN-126, que ya estaban pendientes).
> 3. Pedir al asesor legal la validación de RN-146 a RN-149, de los campos de la hoja de reclamación y de los derechos sobre los datos. Es lo único que deja una HU Must bloqueada (HU-016).
> 4. Aprobar la HU-017 y la HU-018, y decidir K8 (propuesta RN-185), K10, P14, P23, P24 y P25.
> 5. Abrir el sandbox de Yape y Plin (G5) y confirmar que el cobro se aprueba en la app.

### A.7.4 Aprobación del fundador (14/11/2026, 20:47)

El fundador aprobó las 27 reglas propuestas de la v0.9.0. Con eso, RN-185 resuelve K8, se libera la compuerta G7 del backlog y se retiran sus marcas ⏳ de los documentos:
- [`03-reglas-de-negocio.md`](03-reglas-de-negocio.md) v0.9.1;
- [`06-user-stories-refinadas.md`](06-user-stories-refinadas.md) v0.4.1;
- [`07-architecture.md`](07-architecture.md) v1.2.1;
- [`08-backlog.md`](08-backlog.md) v0.4.1;
- [`09-ui-spec.md`](09-ui-spec.md) v0.2.1.

RN-168 a RN-172 siguen esperando la validación del contador, que ahora es la compuerta **G8** y bloquea solo a T-080.

| Métrica | v0.1.0 | Solo lo aprobado, antes de la aprobación | **Tras la aprobación** | Cálculo |
|---|---|---|---|---|
| HU Must cubiertas por completo | 41.2 % | 27.8 % | **77.8 %** (14/18) | Faltan HU-014 (contador), HU-016 (asesor legal), y HU-017 y HU-018 (por aprobar) |
| Cobertura ponderada de HU | 67.6 % | 61.1 % | **86.1 %** | (14 × 1 + 3 × 0.5 + 1 × 0) / 18 |
| Reglas reflejadas por completo | 85.3 % | 83.0 % | **99.4 %** (158/159) | 96.2 % (153/159) sin contar RN-168 a RN-172, que esperan al contador. La única parcial es RN-103 (P14) |

**Lo que sigue pendiente:**
- aprobar la HU-017 y la HU-018;
- aprobar el contrato de la API de la §4.14;
- que el contador valide RN-125, RN-126 y RN-168 a RN-172 (G8);
- que el asesor legal valide la respuesta a reclamos (HU-016), los campos de la hoja de reclamación y los derechos sobre los datos;
- decidir K10, P14, P23, P24 y P25;
- liberar las compuertas G2 a G6.

---


---

## Historial de cambios
| Versión | Fecha | Autor | Cambio |
|---|---|---|---|
| 0.1.0 | 14/11/2026 | Agente Prototype Reviewer (BMAD) | Primera versión: 34 observaciones, 16 decisiones y score de completitud |
| 0.2.0 | 14/11/2026 | Agente Prototype Reviewer (BMAD) | §7: estado de subsanación de las 34 observaciones y las 16 decisiones, y score recalculado |
| 0.2.1 | 14/11/2026 | Agente Prototype Reviewer (BMAD) | §7.4: aprobación de las reglas propuestas (G7) y de K8; score tras la aprobación |
| 0.3.0 | 14/11/2026 | Agente Prototype Reviewer (BMAD) | Ronda 2: revisión de la UI spec v0.2.1 con 18 observaciones nuevas (OBS-35 a OBS-52), decisiones D-17 a D-29 y score recalculado. La ronda 1 pasa al Anexo A |
