# Historias de Usuario refinadas — Muuvi
**Versión:** 0.4.1 (borrador)
**Fecha:** 14/11/2026
**Autor:** Agente QA Funcional (BMAD)
**Estado:** Pendiente de revisión humana
**Cambios en v0.4.1:** el fundador aprueba el 14/11/2026 las reglas propuestas de `03-reglas-de-negocio.md` v0.9.1. Se retiran las marcas ⏳ de los criterios; los de RN-168 a RN-172 conservan ⚠️ porque falta la validación del contador. K8 queda resuelto con RN-185.
**Cambios en v0.4.0:** validación del prototipo (`10-validation-log.md`, 14/11/2026). Decisiones del fundador: hoja de ruta impresa (K9), recuperación de contraseña (nueva HU-018), aprobación de Yape y Plin en cada ciclo y varias suscripciones por cliente. Se agregan criterios **[NUEVO v0.4.0]** en HU-003 a HU-005, HU-007, HU-009 a HU-011 y HU-013 a HU-017, que citan RN-155 a RN-185 de `03-reglas-de-negocio.md` v0.9.0. SR-04, SR-05, SR-07 y SR-10 pasan a reglas propuestas (RN-178 a RN-181). Los criterios que dependían de una regla ⏳ propuesta quedaron habilitados con la aprobación del 14/11/2026 (v0.4.1).
**Cambios en v0.3.0:** decisiones del 14/11/2026 sobre la auditoría de trazabilidad (`09-auditoria-trazabilidad.md`): SR-02 resuelto (RN-154); P18 resuelta (RN-151 y RN-152), que desbloquea HU-013 y HU-014; HU-013 adelantada a la iteración 1 para cargar el precio inicial (RN-153); nueva HU-017 para registrar fincas y productos (QA-C2-004); RN-118 citada en HU-001 y HU-007 (QA-C2-015).
**Cambios en v0.2.0:** decisiones del fundador del 14/11/2026: C-01 resuelto con ADR-004; SR-09, SR-12 y SR-13 convertidos en reglas (RN-135 a RN-145); HU-013 a HU-016 aprobadas; HU-016 con reglas propias (RN-146 a RN-149).
**Base:** [`06-user-stories.md`](06-user-stories.md) v0.3.0 · [`03-reglas-de-negocio.md`](03-reglas-de-negocio.md) v0.9.1 · [`04-glosario.md`](04-glosario.md) v0.7.0 · [`00-constitucion.md`](00-constitucion.md) · [`adr/README.md`](adr/README.md)

> **Cómo leer este documento**
> - Se conservan las 12 HU y todos sus criterios originales. No se eliminó ninguno.
> - Cada criterio agregado lleva **[NUEVO]** y su tipo: **[Positivo]**, **[Negativo]** o **[Borde]**.
> - **❓ Sin RN** marca un escenario necesario cuyo resultado esperado **no está definido por ninguna regla de negocio**. Lo que dice el ENTONCES es una propuesta de QA y el BA debe convertirlo en regla antes de implementar (ver la [sección final](#escenarios-sin-regla-de-negocio)).
> - Los valores siguen las reglas de negocio vigentes: S/ 14.50 por litro, corte a las 18:00:00 del día previo, franja de 07:00 a 10:00, 4.5 °C, 900 s de revisión, 72 h para elegir, días de reparto martes, jueves y sábado, y feriado del martes 08/12/2026.

---

## Índice
1. [Hallazgos constitucionales](#hallazgos-constitucionales)
2. [E1 · Cuenta y consentimiento](#e1--cuenta-y-consentimiento-f-m01)
3. [E2 · Catálogo](#e2--catálogo-f-m02)
4. [E3 · Suscripción](#e3--suscripción-f-m03)
5. [E4 · Cobro, comprobantes y reembolsos](#e4--cobro-comprobantes-y-reembolsos-f-m04)
6. [E5 · Operación diaria](#e5--operación-diaria-f-m05)
7. [E6 · Libro de Reclamaciones](#e6--libro-de-reclamaciones-f-m06)
8. [Resumen INVEST y correcciones](#resumen-invest-y-correcciones)
9. [Escenarios sin regla de negocio](#escenarios-sin-regla-de-negocio)
10. [HUs sugeridas](#hus-sugeridas)

---

## Hallazgos constitucionales

Revisé cada criterio contra la constitución y los ADR vigentes. Ningún criterio la contradice directamente, pero hay un riesgo de interpretación que debe resolver un humano antes de implementar:

| # | Severidad | Dónde | Problema | Sugerencia |
|---|---|---|---|---|
| C-01 ✅ | Resuelto | HU-010 (alerta de lote), HU-011 (aviso "no entregar") · RN-026 y RN-133 · PRINCIPIO-008.4 | El PRINCIPIO-008.4 prohíbe enviar notificaciones **push** fuera de las 09:00 a 20:00 y **no limita esa prohibición a clientes**. Los lotes se reciben antes de las 07:00 y el reparto es de 07:00 a 10:00: un aviso push al operador a las 06:10 o al repartidor a las 07:30 lo violaría. | Decidir: (a) implementar estos avisos **dentro de la PWA, sin push** (cumple la letra del principio), o (b) registrar un ADR que exceptúe los avisos operativos internos. **Resuelto el 14/11/2026 con el [ADR-004](adr/ADR-004-excepcion-horaria-operaciones.md)**: las alertas internas de operación pueden enviarse a cualquier hora (RN-150). |
| C-02 | 🟡 Media (relacionado con K10) | HU-007 y HU-009 · RN-129 ("por correo y en la PWA") | Si el aviso "en la PWA" se implementa como push, aplica la ventana de 09:00 a 20:00. El aviso de las 18:42 está dentro, pero uno generado a las 21:00 no. | Implementarlo como aviso dentro de la app, o diferirlo a las 09:00 como en RN-043. |
| C-03 | 🟢 Baja | Todos los criterios con datos de prueba | El PRINCIPIO-008.7 prohíbe usar datos reales de clientes. | Usar datos ficticios en todos los escenarios y entornos de prueba. |

---

## E1 · Cuenta y consentimiento (F-M01)

### HU-001: Registrarme como cliente
**Prioridad:** Must

Como **persona que vive en Miraflores, San Isidro o La Molina**, quiero **crear mi cuenta en Muuvi**, para **poder suscribirme a la leche de finca**.

**Validación INVEST:** ✅ Cumple todos. Es la base del resto del flujo del cliente.
**Revisión de los criterios existentes:** tienen valores concretos y son verificables. Falta un camino feliz completo con todos los datos y faltan los casos de error de red y de fecha vacía.

**Criterios de aceptación:**
- DADO QUE hoy es 12/11/2026, CUANDO me registro con fecha de nacimiento 13/11/2008 (17 años), ENTONCES el sistema rechaza el registro.
- DADO QUE hoy es 12/11/2026, CUANDO me registro con fecha de nacimiento 12/11/2008 (18 años) y marco el consentimiento principal, ENTONCES el sistema crea mi cuenta.
- DADO QUE abro el formulario, CUANDO se carga, ENTONCES las casillas del consentimiento principal y del envío a terceros aparecen separadas y desmarcadas.
- DADO QUE no marco el consentimiento principal, CUANDO envío el formulario, ENTONCES el sistema rechaza el registro.
- DADO QUE marco el consentimiento principal y no marco el de envío a terceros, CUANDO envío el formulario, ENTONCES el sistema crea mi cuenta igual.
- DADO QUE ingreso una dirección en Santiago de Surco, CUANDO envío el formulario, ENTONCES el sistema rechaza la dirección.
- **[NUEVO][Positivo]** DADO QUE ingreso nombre, correo, teléfono, fecha de nacimiento 15/03/1990, una dirección en San Isidro y marco el consentimiento principal, CUANDO envío el formulario, ENTONCES se crea mi cuenta, y en la base de datos el nombre, el correo, el teléfono, la fecha de nacimiento y la dirección quedan cifrados con AES-256-GCM (columnas `*_cifrado`).
- **[NUEVO][Negativo]** DADO QUE dejo vacía la fecha de nacimiento, CUANDO envío el formulario, ENTONCES el sistema no crea la cuenta (RN-002).
- **[NUEVO][Negativo]** DADO QUE envío el formulario con un dato inválido, CUANDO el sistema registra el error, ENTONCES el log contiene solo identificadores técnicos y ningún dato personal en texto plano (RN-007).
- **[NUEVO][Borde]** DADO QUE hoy es 12/11/2026, CUANDO me registro con fecha de nacimiento 11/11/2008 (18 años y 1 día), ENTONCES el sistema crea mi cuenta.
- **[NUEVO][Borde]** DADO QUE ingreso una dirección en La Molina, el tercer distrito atendido, CUANDO envío el formulario, ENTONCES el sistema la acepta.
- **[NUEVO][Negativo] ❓ Sin RN** DADO QUE se corta la conexión mientras envío el formulario, CUANDO reintento, ENTONCES *(propuesta)* no se crea una cuenta duplicada.
- **[NUEVO][Negativo]** DADO QUE ya existe una cuenta con `ana@example.com`, CUANDO intento registrarme con `Ana@Example.com`, ENTONCES el sistema rechaza el registro con el mensaje "Este correo ya tiene una cuenta" y no crea una cuenta nueva (RN-154; decisión del fundador del 14/11/2026, que reemplaza la propuesta de no revelar si el correo existe).
- **[NUEVO][Borde] ❓ Sin RN (QA-057)** DADO QUE nací el 29/02/2008, CUANDO me registro el 28/02/2026, ENTONCES *(propuesta)* se definirá si ya cumplí 18 años.

**Reglas de negocio relacionadas:** RN-001, RN-002, RN-003, RN-004, RN-005, RN-006, RN-007, RN-114, RN-115, RN-116, RN-118, RN-154
**Notas:** se mantienen las notas originales (texto legal redactado por un humano, encargados nombrados en el consentimiento, ANPDP y DPA antes de producción).

---

### HU-002: Darme de baja
**Prioridad:** Must

Como **cliente**, quiero **dar de baja mi cuenta**, para **que Muuvi deje de tratar mis datos personales**.

**Validación INVEST:** ✅ Cumple todos.
**Revisión de los criterios existentes:** son correctos. Falta cubrir los demás bloqueos de RN-098 (suscripción activa, reclamo abierto), el permiso del rol y el borde del plazo de 30 días.

**Criterios de aceptación:**
- DADO QUE tengo una entrega en estado `en_ruta`, CUANDO pido la baja, ENTONCES el sistema la rechaza y muestra "Tienes una entrega en curso".
- DADO QUE no tengo suscripción activa, entregas `programada` o `en_ruta` ni reclamos sin respuesta, pero tengo 2 botellas sin devolver, CUANDO pido la baja, ENTONCES el sistema la acepta.
- DADO QUE pedí la baja el 12/11/2026 a las 10:00, CUANDO llega el 12/12/2026 a las 10:00 (2 592 000 s después), ENTONCES mis datos personales ya están anonimizados y mis boletas siguen guardadas.
- DADO QUE tengo S/ 14.50 de saldo a favor, CUANDO se acepta mi baja a las 10:00, ENTONCES antes de las 11:00 se solicita el reembolso de S/ 14.50 al medio de pago con que pagué el ciclo que generó ese saldo.
- **[NUEVO][Negativo]** DADO QUE tengo una suscripción `activa`, CUANDO pido la baja, ENTONCES el sistema la rechaza e indica que primero debo cancelar la suscripción.
- **[NUEVO][Negativo]** DADO QUE tengo un reclamo sin respuesta, CUANDO pido la baja, ENTONCES el sistema la rechaza e indica el reclamo pendiente.
- **[NUEVO][Negativo]** DADO QUE soy operador, CUANDO intento pedir la baja de la cuenta de un cliente, ENTONCES el sistema lo rechaza, porque solo el cliente gestiona su propia cuenta (RN-099).
- **[NUEVO][Borde]** DADO QUE pedí la baja el 12/11/2026 a las 10:00, CUANDO reviso el 12/12/2026 a las 09:59:59, ENTONCES el proceso de anonimización puede no haber corrido aún, pero a las 10:00:00 ya debe haber terminado.
- **[NUEVO][Borde]** DADO QUE tengo S/ 0.00 de saldo a favor, CUANDO se acepta mi baja, ENTONCES no se solicita ningún reembolso.
- **[NUEVO][Positivo]** DADO QUE mi cuenta ya se anonimizó, CUANDO SUNAT o el operador consulta mis boletas, ENTONCES siguen disponibles (RN-111).

**Reglas de negocio relacionadas:** RN-008, RN-098, RN-099, RN-111, RN-124
**Notas:** la anonimización es un proceso automático y no requiere doble confirmación (aclaración de QA-010).

---

## E2 · Catálogo (F-M02)

### HU-003: Ver la leche disponible y su finca de origen
**Prioridad:** Must

Como **cliente**, quiero **ver la leche disponible, su precio y la finca certificada de la que viene**, para **decidir si me suscribo**.

**Validación INVEST:** ✅ Cumple todos.
**Revisión de los criterios existentes:** el tercer criterio prueba el día después del vencimiento, pero no el **día mismo**, que es el borde real (el glosario dice "vencimiento igual o posterior a la fecha actual").

**Criterios de aceptación:**
- DADO QUE `precio_litro = 14.50` en la configuración, CUANDO abro el catálogo, ENTONCES veo "Leche fresca 1 L · S/ 14.50 por litro".
- DADO QUE un producto tiene `registro_digesa` vacío, CUANDO el operador intenta publicarlo, ENTONCES el sistema lo rechaza y el producto no aparece en el catálogo.
- DADO QUE la certificación SENASA de la finca vence el 30/11/2026, CUANDO abro el catálogo el 01/12/2026, ENTONCES la etiqueta "Orgánico" no aparece en esa finca ni en sus productos.
- DADO QUE abro la ficha de la finca, CUANDO se carga, ENTONCES veo su nombre, su provincia (Cañete o Huaral) y su certificación SENASA.
- **[NUEVO][Borde]** DADO QUE la certificación SENASA vence el 30/11/2026, CUANDO abro el catálogo el 30/11/2026, ENTONCES la etiqueta "Orgánico" **sí** aparece, porque la certificación sigue vigente ese día.
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento publicar un producto o editar una finca, ENTONCES el sistema lo rechaza (RN-099).
- **[NUEVO][Negativo]** DADO QUE un proceso automático intenta cambiar `precio_litro` de 14.50 a 15.00, CUANDO se ejecuta, ENTONCES el sistema lo rechaza y el catálogo sigue mostrando S/ 14.50 (RN-011).
- **[NUEVO][Borde]** DADO QUE `precio_litro = 1250.50` en un entorno de prueba, CUANDO abro el catálogo, ENTONCES el precio se muestra como "S/ 1,250.50", con coma de miles (RN-037).
- **[NUEVO][Borde] ❓ Sin RN** DADO QUE no hay ningún producto publicado, CUANDO abro el catálogo, ENTONCES *(propuesta)* veo un mensaje que dice que no hay leche disponible, cargado desde `es-PE.json`.

- **[NUEVO v0.4.0][Borde] RN-178** DADO QUE no hay ningún producto publicado, CUANDO abro el catálogo, ENTONCES veo «Por ahora no hay leche disponible. Vuelve pronto.» y no aparece el botón «Suscribirme» (reemplaza la propuesta SR-04).

**Reglas de negocio relacionadas:** RN-009, RN-010, RN-011, RN-037, RN-099, RN-178
**Notas:** el cambio de precio no tiene HU propia; ver la HU-013 sugerida.

---

## E3 · Suscripción (F-M03)

### HU-004: Suscribirme a una entrega semanal
**Prioridad:** Must

Como **cliente**, quiero **suscribirme para recibir cierta cantidad de litros cada semana**, para **no tener que acordarme de comprar leche**.

**Validación INVEST:** ⚠️ **I**: depende de HU-001. Se puede trabajar en paralelo con un cliente de prueba sembrado.
**Revisión de los criterios existentes:** son correctos. Falta el borde exacto de la hora de corte al crear la suscripción y el mínimo de 1 litro.

**Criterios de aceptación:**
- DADO QUE elijo 3 litros los sábados, CUANDO confirmo la suscripción, ENTONCES el sistema me muestra un importe por ciclo de S/ 43.50 (3 × S/ 14.50) y la franja "07:00–10:00".
- DADO QUE elijo el lunes como día de entrega, CUANDO confirmo, ENTONCES el sistema rechaza la suscripción e indica que los días de reparto son martes, jueves y sábado.
- DADO QUE ingreso 0 litros o 1.5 litros, CUANDO confirmo, ENTONCES el sistema rechaza la suscripción.
- DADO QUE creo una suscripción de los sábados el viernes 13/11/2026 a las 19:00, CUANDO se guarda, ENTONCES mi primera entrega es el sábado 21/11/2026 y mi primer cobro es el viernes 20/11/2026 a las 18:00:00.
- DADO QUE recibo los martes y el martes 08/12/2026 es feriado nacional, CUANDO se acerca esa semana, ENTONCES mi entrega pasa al jueves 10/12/2026, con corte y cobro el miércoles 09/12/2026 a las 18:00:00.
- **[NUEVO][Borde]** DADO QUE elijo 1 litro (el mínimo), CUANDO confirmo, ENTONCES la suscripción se crea con un importe de S/ 14.50 por ciclo.
- **[NUEVO][Borde]** DADO QUE creo una suscripción de los sábados el viernes 13/11/2026 a las 17:59:59, CUANDO se guarda, ENTONCES mi primera entrega es el sábado 14/11/2026, porque el corte de las 18:00:00 todavía no ocurrió.
- **[NUEVO][Borde]** DADO QUE creo una suscripción de los sábados el viernes 13/11/2026 a las 18:00:00 en punto, CUANDO se guarda, ENTONCES mi primera entrega es el sábado 21/11/2026.
- **[NUEVO][Borde]** DADO QUE elijo 50 litros por semana, CUANDO confirmo, ENTONCES la suscripción se crea, porque en el PMV no hay máximo (RN-017). ⚠️ El tope por capacidad de la finca sigue abierto (QA-042).
- **[NUEVO][Borde]** DADO QUE el corte de las 18:00 se evalúa en hora de Lima, CUANDO el servidor está en UTC, ENTONCES el corte del viernes 13/11/2026 ocurre a las 23:00:00 UTC (RN-039).
- **[NUEVO][Negativo] ❓ Sin RN** DADO QUE no tengo ningún medio de pago registrado, CUANDO confirmo la suscripción, ENTONCES *(propuesta)* el sistema me pide registrar uno antes de crearla.

- **[NUEVO v0.4.0][Positivo] RN-160** DADO QUE tengo una suscripción de 3 L los sábados, CUANDO creo otra de 2 L los martes, ENTONCES el sistema la crea y en «Mis suscripciones» veo las dos, cada una con su próxima entrega.
- **[NUEVO v0.4.0][Positivo] RN-161** DADO QUE hay dos productos publicados, CUANDO creo una suscripción, ENTONCES elijo uno de ellos y la suscripción queda asociada a ese producto.
- **[NUEVO v0.4.0][Negativo] RN-179** DADO QUE no tengo ningún medio de pago activo, CUANDO confirmo la suscripción, ENTONCES el sistema la rechaza con `422 RN-179` y me ofrece agregar un medio (reemplaza la propuesta SR-05).
- **[NUEVO v0.4.0][Positivo] RN-184** DADO QUE el viernes 13/11/2026 a las 19:00 elijo 3 L los sábados, CUANDO llego al paso de revisión, ENTONCES la vista previa calculada por el backend muestra «Primera entrega: 21/11/2026 · Primer cobro: 20/11/2026, 18:00 · S/ 43.50», sin crear la suscripción, y al confirmar se crea con esos mismos datos.

**Reglas de negocio relacionadas:** RN-015, RN-016, RN-017, RN-018, RN-039, RN-061, RN-084, RN-123, RN-160, RN-161, RN-179, RN-184
**Notas:** se mantienen las notas originales.

---

### HU-005: Pausar y reactivar mi suscripción
**Prioridad:** Must

Como **cliente**, quiero **pausar mi suscripción por un número de entregas o de forma indefinida, y reactivarla cuando quiera**, para **no recibir leche cuando viajo**.

**Validación INVEST:** ⚠️ **I**: depende de HU-004.
**Revisión de los criterios existentes:** el primero y el segundo cubren bien el borde del corte. Faltan el borde de la reactivación a las 18:00:00 y la pausa de 0 entregas.

**Criterios de aceptación:**
- DADO QUE mi entrega es el sábado 14/11/2026, CUANDO pauso el viernes 13/11/2026 a las 17:59:59, ENTONCES no se programa la entrega del sábado.
- DADO QUE mi entrega es el sábado 14/11/2026, CUANDO pauso el viernes 13/11/2026 a las 18:00:00, ENTONCES la entrega del sábado se mantiene.
- DADO QUE pido una pausa de 2 entregas el viernes 13/11/2026 a las 10:00, CUANDO pasan los sábados 14/11 y 21/11, ENTONCES la suscripción vuelve a `activa` y se programa la entrega del sábado 28/11/2026.
- DADO QUE mi suscripción está en pausa indefinida, CUANDO la reactivo el viernes 20/11/2026 a las 11:00, ENTONCES se programa la entrega del sábado 21/11/2026.
- **[NUEVO][Borde]** DADO QUE mi suscripción está en pausa indefinida, CUANDO la reactivo el viernes 20/11/2026 a las 18:00:00, ENTONCES no se programa la entrega del 21/11/2026 y la primera es la del sábado 28/11/2026.
- **[NUEVO][Negativo]** DADO QUE pido una pausa de 0 entregas, CUANDO la confirmo, ENTONCES el sistema la rechaza, porque N debe ser un entero mayor o igual a 1 (RN-062).
- **[NUEVO][Borde]** DADO QUE pido una pausa de 1 entrega el viernes 13/11/2026 a las 10:00, CUANDO pasa el sábado 14/11, ENTONCES la suscripción vuelve a `activa` y se programa la entrega del sábado 21/11/2026.
- **[NUEVO][Negativo]** DADO QUE mi suscripción lleva 8 semanas en pausa indefinida, CUANDO pasa el tiempo sin que la reactive, ENTONCES el sistema no la reanuda por su cuenta (RN-063).
- **[NUEVO][Borde]** DADO QUE estoy en pausa por 2 entregas y el sábado 21/11 cayera en feriado, CUANDO se traslada esa entrega, ENTONCES la entrega trasladada cuenta como una de las 2 omitidas. ❓ Sin RN: confirmar.
- **[NUEVO][Negativo] ❓ Sin RN** DADO QUE mi suscripción está en `suspendida_por_pago`, CUANDO intento pausarla, ENTONCES *(propuesta)* el sistema lo rechaza, porque el diagrama de estados de la suscripción no tiene esa transición.

- **[NUEVO v0.4.0][Negativo] RN-180** DADO QUE mi suscripción está en `suspendida_por_pago`, CUANDO intento pausarla, ENTONCES el sistema lo rechaza con `409 RN-180` y me ofrece actualizar mi medio de pago (reemplaza la propuesta SR-07).
- **[NUEVO v0.4.0][Positivo] RN-184** DADO QUE el viernes 13/11/2026 a las 10:00 elijo pausar 2 entregas, CUANDO veo la vista previa, ENTONCES dice «No recibirás: 14/11 y 21/11 · Vuelves a recibir: 28/11/2026», sin haber pausado nada todavía.

**Reglas de negocio relacionadas:** RN-012, RN-013, RN-062, RN-063, RN-064, RN-180, RN-184
**Notas:** el sistema nunca reanuda por su cuenta una pausa indefinida (RN-063).

---

### HU-006: Modificar o cancelar mi suscripción
**Prioridad:** Must

Como **cliente**, quiero **cambiar mis litros, mi día o mi dirección, o cancelar la suscripción**, para **ajustarla a lo que consume mi hogar**.

**Validación INVEST:** ⚠️ **I**: depende de HU-004.
**Revisión de los criterios existentes:** son correctos. Falta el borde de la modificación exactamente en el corte, el cambio a un día no permitido y la cancelación con saldo a favor.

**Criterios de aceptación:**
- DADO QUE recibo 2 L los sábados, CUANDO cambio a 3 L el viernes 13/11/2026 a las 19:00, ENTONCES la entrega del 14/11/2026 sale con 2 L y la del 21/11/2026 con 3 L.
- DADO QUE cancelo el viernes 13/11/2026 a las 16:00, CUANDO llega el corte, ENTONCES no se crean entregas el 14/11/2026 ni después.
- DADO QUE cancelo el viernes 13/11/2026 a las 20:00, CUANDO llega el sábado, ENTONCES recibo la entrega del 14/11/2026, la suscripción queda `cancelada` y no se crea la del 21/11/2026.
- DADO QUE cambio mi dirección a una en Barranco, CUANDO guardo el cambio, ENTONCES el sistema rechaza la dirección durante el PMV.
- **[NUEVO][Borde]** DADO QUE recibo 2 L los sábados, CUANDO cambio a 3 L el viernes 13/11/2026 a las 17:59:59, ENTONCES la entrega del 14/11/2026 ya sale con 3 L y su cobro es de S/ 43.50.
- **[NUEVO][Negativo]** DADO QUE recibo los sábados, CUANDO cambio mi día al domingo, ENTONCES el sistema rechaza el cambio (RN-061).
- **[NUEVO][Positivo]** DADO QUE recibo los sábados, CUANDO cambio mi día al jueves el lunes 16/11/2026, ENTONCES mi siguiente entrega es el jueves 19/11/2026, con corte el miércoles 18/11/2026 a las 18:00:00.
- **[NUEVO][Positivo]** DADO QUE tengo S/ 14.50 de saldo a favor, CUANDO cancelo mi suscripción a las 10:00, ENTONCES antes de las 11:00 se solicita el reembolso de S/ 14.50 (RN-124).
- **[NUEVO][Negativo] ❓ Sin RN** DADO QUE mi suscripción ya está `cancelada`, CUANDO intento cancelarla o modificarla otra vez, ENTONCES *(propuesta)* el sistema lo rechaza.

- **[NUEVO v0.4.0][Positivo] RN-005** DADO QUE quiero recibir en otra casa, CUANDO agrego la dirección «Av. Ficticia 456, Miraflores» desde «Cambiar tu suscripción», ENTONCES queda guardada y la puedo elegir para la entrega siguiente al próximo corte (RN-083).
- **[NUEVO v0.4.0][Positivo] RN-160** DADO QUE tengo dos suscripciones, CUANDO cancelo la de los martes, ENTONCES la de los sábados sigue `activa`.

**Reglas de negocio relacionadas:** RN-005, RN-014, RN-061, RN-081, RN-082, RN-083, RN-124, RN-160, RN-184
**Notas:** se mantiene la nota original.

---

## E4 · Cobro, comprobantes y reembolsos (F-M04)

### HU-007: Pagar cada ciclo automáticamente
**Prioridad:** Must

Como **cliente**, quiero **que cada ciclo se cobre solo a mi tarjeta, Yape o Plin**, para **recibir mi leche sin hacer nada cada semana**.

**Validación INVEST:** ⚠️ **I**: depende de HU-004. ⚠️ **E** y **S**: tiene 9 criterios y 23 reglas.
**Corrección sugerida:** partirla en **HU-007a** (cobro automático e idempotencia, criterios 1 a 3), **HU-007b** (rechazo, suspensión y regularización, criterios 4 y 5) y **HU-007c** (pago pendiente, pago tardío y elección, criterios 6 a 9).
**Revisión de los criterios existentes:** el primero usa `ciclo-{id}-cliente-{id}`, que no es un valor concreto. Sugiero usar `ciclo-7781-cliente-4821`, como en RN-072.

**Criterios de aceptación:**
- DADO QUE tengo una suscripción activa de S/ 43.50 con entrega el sábado 14/11/2026, CUANDO llega el viernes 13/11/2026 a las 18:00:00, ENTONCES el sistema ejecuta el cobro con la clave de idempotencia `ciclo-{id}-cliente-{id}` *(sugerencia: `ciclo-7781-cliente-4821`)*.
- DADO QUE el proceso de cobro de las 18:00 se ejecuta dos veces por error, CUANDO termina, ENTONCES tengo un solo cargo de S/ 43.50.
- DADO QUE el primer cobro se rechaza a las 18:00:00, CUANDO pasan 60 s, ENTONCES el sistema hace 1 reintento a las 18:01:00 y no hace ningún otro en ese ciclo.
- DADO QUE el cobro y el reintento se rechazan, CUANDO termina el reintento, ENTONCES la entrega del 14/11/2026 pasa a `suspendida` con motivo `pago`, mi suscripción pasa a `suspendida_por_pago` y recibo en 300 s como máximo un correo para actualizar mi medio de pago.
- DADO QUE mi suscripción está en `suspendida_por_pago`, CUANDO registro una tarjeta nueva el miércoles 18/11/2026, ENTONCES vuelve a `activa` y el siguiente cobro se hace el viernes 20/11/2026 a las 18:00:00.
- DADO QUE pago con Yape y a las 18:30:00 (1 800 s) el pago sigue pendiente, CUANDO se cumple el plazo, ENTONCES la entrega pasa a `suspendida` con motivo `pago`.
- DADO QUE un pago pendiente se confirma a las 18:40:00, después de suspender la entrega, CUANDO llega la confirmación, ENTONCES la entrega sigue `suspendida`, mi suscripción vuelve a `activa` sin pedirme otro medio de pago y a las 18:45:00 como máximo recibo un correo para elegir entre reembolso y saldo a favor. ⚠️ C-02
- DADO QUE elijo reembolso, CUANDO confirmo mi elección, ENTONCES se solicita el reembolso íntegro en 3 600 s como máximo; DADO QUE elijo saldo a favor, ENTONCES el importe se suma a mi saldo.
- DADO QUE recibí el aviso el 13/11/2026 a las 18:42:00 y no elijo ninguna opción, CUANDO llega el 16/11/2026 a las 18:42:00 (72 h), ENTONCES se solicita automáticamente el reembolso íntegro a mi medio de pago original.
- **[NUEVO][Negativo]** DADO QUE la PWA envía al backend `{ "numero_tarjeta": "4111111111111111", "cvv": "123" }`, CUANDO llega la petición, ENTONCES el backend la rechaza sin guardar su contenido (RN-020).
- **[NUEVO][Negativo]** DADO QUE intento pagar con Niubiz durante el PMV, CUANDO confirmo, ENTONCES el sistema rechaza el medio de pago (RN-019, ADR-001).
- **[NUEVO][Negativo]** DADO QUE la pasarela envía dos veces el mismo webhook de confirmación del ciclo 7781, CUANDO el sistema los procesa, ENTONCES registra un solo pago confirmado (RN-073).
- **[NUEVO][Borde]** DADO QUE pago con Plin y la confirmación llega a las 18:29:59 (1 799 s), CUANDO se registra, ENTONCES el pago queda confirmado y la entrega del 14/11/2026 se mantiene `programada`.
- **[NUEVO][Borde]** DADO QUE el reintento del ciclo también se rechaza, CUANDO pasan otros 60 s, ENTONCES no hay un tercer intento de cobro en ese ciclo (RN-080).
- **[NUEVO][Borde]** DADO QUE mi suscripción está `pausada` el viernes 13/11/2026 a las 18:00:00, CUANDO se cumple el corte, ENTONCES no se ejecuta ningún cobro.
- **[NUEVO][Positivo]** DADO QUE pago con Yape, CUANDO se confirma el pago, ENTONCES se guarda solo la referencia de la transacción (por ejemplo `YP-20261113-889120`) y `tipo_medio = yape`, sin el número de celular (RN-094 y RN-095).
- **[NUEVO][Borde]** DADO QUE recibí el aviso a las 18:42:00 del 13/11/2026, CUANDO elijo saldo a favor el 16/11/2026 a las 18:41:59 (1 s antes de las 72 h), ENTONCES se respeta mi elección y no hay reembolso automático.
- **[NUEVO][Negativo]** DADO QUE el cobro de las 18:00:00 recibe un error 503 de la pasarela, CUANDO se registra, ENTONCES el pago queda `en_verificacion`, no se hace el reintento de las 18:01:00 y el sistema consulta a la pasarela el estado del cargo `ciclo-7781-cliente-4821` (RN-135 a RN-137).
- **[NUEVO][Positivo]** DADO QUE el pago está `en_verificacion`, CUANDO la consulta informa que el cargo fue confirmado, ENTONCES el pago pasa a `confirmado` y tengo un solo cargo de S/ 43.50 (RN-138).
- **[NUEVO][Negativo]** DADO QUE el pago está `en_verificacion`, CUANDO la consulta informa que el cargo fue rechazado, ENTONCES se aplica el reintento único de RN-046.
- **[NUEVO][Borde] ⛔ P23** DADO QUE el pago sigue `en_verificacion` a las 18:30:00 (1 800 s), CUANDO se cumple el plazo, ENTONCES *(pendiente: P23)*.

- **[NUEVO v0.4.0][Positivo] RN-164** DADO QUE pago con Yape, CUANDO el cobro de S/ 43.50 se inicia el viernes 13/11/2026 a las 18:00:00, ENTONCES a las 18:01:00 como máximo recibo por correo y en la PWA el aviso «Aprueba tu pago de S/ 43.50 en Yape antes de las 18:30». ⚠️ Confirmar en el sandbox (G5).
- **[NUEVO v0.4.0][Positivo] RN-135** DADO QUE mi pago quedó `en_verificacion`, CUANDO abro «Mis suscripciones», ENTONCES veo «Estamos verificando tu pago con el banco» y no se me pide pagar otra vez.

**Reglas de negocio relacionadas:** RN-019, RN-020, RN-021, RN-045, RN-046, RN-047, RN-048, RN-049, RN-050, RN-051, RN-072, RN-073, RN-074, RN-075, RN-080, RN-094, RN-095, RN-119, RN-120, RN-121, RN-118, RN-129, RN-130, RN-131, RN-135, RN-136, RN-137, RN-138, RN-164
**Notas:** se mantienen las notas originales (PCI-DSS, sandbox antes de cerrar la HU).

---

### HU-008: Recibir mi comprobante electrónico
**Prioridad:** Must

Como **cliente**, quiero **recibir mi boleta o factura electrónica por cada pago**, para **tener el respaldo tributario de lo que pagué**.

**Validación INVEST:** ⚠️ **I**: depende de HU-007. ⚠️ **E**: sujeta a la elección del OSE/PSE.
**Revisión de los criterios existentes:** son correctos. Falta el RUC con letras, el RUC de persona natural, el reintento exitoso y el comprobante con saldo a favor.

**Criterios de aceptación:**
- DADO QUE no elegí factura, CUANDO se confirma mi pago de S/ 43.50, ENTONCES se emite una boleta electrónica de S/ 43.50.
- DADO QUE elegí factura con el RUC `20123456789`, CUANDO se confirma mi pago, ENTONCES se emite una factura electrónica.
- DADO QUE ingreso el RUC `2012345678` (10 dígitos), CUANDO lo guardo, ENTONCES el sistema lo rechaza.
- DADO QUE mi pago con Plin quedó `pendiente`, CUANDO todavía no se confirma, ENTONCES no se emite ningún comprobante.
- DADO QUE el OSE/PSE falla al emitir a las 18:00:05, CUANDO pasan los reintentos (18:01:05, 18:03:05, 18:07:05, 18:15:05 y 18:31:05) y todos fallan, ENTONCES el operador recibe una alerta crítica en 60 s como máximo.
- **[NUEVO][Negativo]** DADO QUE ingreso el RUC `2012345678A` (11 caracteres, uno no numérico), CUANDO lo guardo, ENTONCES el sistema lo rechaza (RN-058).
- **[NUEVO][Borde]** DADO QUE ingreso el RUC `201234567890` (12 dígitos), CUANDO lo guardo, ENTONCES el sistema lo rechaza.
- **[NUEVO][Positivo]** DADO QUE elegí factura con el RUC `10456789012`, CUANDO lo guardo, ENTONCES se guarda cifrado con AES-256-GCM, porque contiene el DNI `45678901` (RN-060).
- **[NUEVO][Borde]** DADO QUE el OSE/PSE falla a las 18:00:05 y el segundo reintento (18:03:05) emite con éxito, CUANDO termina, ENTONCES hay una sola boleta y el operador no recibe ninguna alerta.
- **[NUEVO][Negativo]** DADO QUE la pasarela rechazó mi pago, CUANDO termina el proceso, ENTONCES no se emite ningún comprobante (RN-023).
- **[NUEVO][Positivo]** DADO QUE mi ciclo de S/ 43.50 se cubrió con saldo a favor, CUANDO se registra el pago con origen `saldo_favor`, ENTONCES se emite una boleta de S/ 43.50 que dice "Forma de pago: aplicación de saldo a favor" (RN-125 y RN-126). ⚠️ Pendiente de validación por el contador.
- **[NUEVO][Borde]** DADO QUE elegí factura pero no tengo RUC registrado, CUANDO se confirma mi pago, ENTONCES se emite una boleta (RN-059).

**Reglas de negocio relacionadas:** RN-022, RN-023, RN-058, RN-059, RN-060, RN-104, RN-106, RN-125, RN-126
**Notas:** se mantiene la nota original.

---

### HU-009: Recibir un reembolso cuando Muuvi falla
**Prioridad:** Must

Como **cliente**, quiero **que me devuelvan lo pagado si Muuvi no me entrega la leche**, para **no pagar por algo que no recibí**.

**Validación INVEST:** ⚠️ **I**: depende de HU-007 y HU-011. ⚠️ **S**: tiene 7 criterios. ⚠️ **T**: el comprobante con saldo espera al contador.
**Corrección sugerida:** partirla en **HU-009a** (importe a devolver, elección y reembolso) y **HU-009b** (saldo a favor y su aplicación).
**Revisión de los criterios existentes:** el segundo criterio tiene dos DADO QUE en una línea. Sugiero separarlo en dos criterios para que cada uno sea un test.

**Criterios de aceptación:**
- DADO QUE mi entrega de S/ 43.50 queda `fallida` con causa `imputable_muuvi` a las 09:40:00, CUANDO se registra, ENTONCES a las 09:45:00 como máximo recibo un correo y un aviso en la PWA para elegir entre reembolso y saldo a favor por S/ 43.50. ⚠️ C-02
- DADO QUE elijo reembolso a las 10:00, CUANDO confirmo, ENTONCES el reembolso de S/ 43.50 se solicita antes de las 11:00 y se emite una nota de crédito que referencia la boleta original; DADO QUE elijo saldo a favor, ENTONCES mi saldo sube en S/ 43.50.
- DADO QUE mi entrega de 2 L (S/ 29.00) queda `suspendida` porque su lote se invalidó y no elijo nada, CUANDO pasan 72 h desde el aviso, ENTONCES se me reembolsan automáticamente S/ 29.00.
- DADO QUE mi entrega reprogramada vuelve a fallar por mi causa y queda `cancelada`, CUANDO se cancela, ENTONCES se me reembolsa su importe íntegro sin pedirme que elija.
- DADO QUE elegí saldo a favor, tengo S/ 14.50 de saldo y mi ciclo es de S/ 43.50, CUANDO llega el corte, ENTONCES se me cobran S/ 29.00.
- DADO QUE la pasarela rechaza un reembolso a las 09:45:00, CUANDO falla, ENTONCES se reintenta a las 09:46:00 con backoff exponencial, y si se agotan los 5 reintentos el operador recibe una alerta crítica.
- DADO QUE tengo S/ 58.00 de saldo y mi ciclo es de S/ 43.50, CUANDO llega el corte, ENTONCES no se envía cobro a la pasarela, se registra un pago confirmado de S/ 43.50 con origen `saldo_favor`, recibo una boleta de S/ 43.50 que dice "Forma de pago: aplicación de saldo a favor" y me quedan S/ 14.50 de saldo.
- **[NUEVO][Borde]** DADO QUE tengo exactamente S/ 43.50 de saldo y mi ciclo es de S/ 43.50, CUANDO llega el corte, ENTONCES no se envía cobro a la pasarela y mi saldo queda en S/ 0.00 (RN-097, "mayor o igual").
- **[NUEVO][Borde]** DADO QUE recibí el aviso el 14/11/2026 a las 09:42:00, CUANDO llega el 17/11/2026 a las 09:41:59 sin que yo elija, ENTONCES todavía no hay reembolso automático; a las 09:42:00 se solicita.
- **[NUEVO][Negativo]** DADO QUE ya pasaron las 72 h y se solicitó el reembolso automático, CUANDO intento elegir saldo a favor, ENTONCES ❓ Sin RN: *(propuesta)* el sistema rechaza la elección e indica que el reembolso ya está en curso.
- **[NUEVO][Positivo]** DADO QUE el reembolso falla a las 09:45:00 y el segundo reintento (09:48:00) tiene éxito, CUANDO termina, ENTONCES el reembolso queda hecho y el operador no recibe alerta.
- **[NUEVO][Negativo]** DADO QUE mi entrega falla con causa `imputable_cliente`, CUANDO se registra la primera vez, ENTONCES no se genera importe a devolver, sino una reprogramación (RN-066).
- **[NUEVO][Positivo]** DADO QUE en un día de escasez mi entrega de 3 L queda `fallida` con motivo `falla_abastecimiento_finca`, CUANDO se registra, ENTONCES se genera un importe a devolver de S/ 43.50 y recibo el aviso de elección (RN-092, RN-053 y RN-129).

- **[NUEVO v0.4.0][Negativo] RN-181** DADO QUE el aviso se envió el 14/11/2026 a las 09:42:00, CUANDO elijo saldo a favor el 17/11/2026 a las 09:43, ENTONCES el sistema lo rechaza con `409 RN-181` y me informa que el reembolso automático está en curso (reemplaza la propuesta SR-10).
- **[NUEVO v0.4.0][Positivo] RN-162** DADO QUE tengo S/ 14.50 de saldo y suscripciones de los martes (S/ 29.00) y de los sábados (S/ 43.50), CUANDO llega el corte del lunes 16/11/2026, ENTONCES el ciclo del martes 17/11 se cobra en S/ 14.50 y el del sábado 21/11 se cobra completo.
- **[NUEVO v0.4.0][Positivo] RN-096** DADO QUE tengo S/ 14.50 de saldo y mi ciclo es de S/ 43.50, CUANDO veo mi próxima entrega, ENTONCES dice «Cobro: S/ 29.00 (S/ 43.50 − S/ 14.50 de saldo a favor)».
- **[NUEVO v0.4.0][Positivo] RN-183** DADO QUE mi entrega reprogramada vuelve a fallar a las 08:30, CUANDO se cancela, ENTONCES a las 08:35 como máximo recibo por correo y en la PWA «Cancelamos tu entrega y te devolvemos S/ 43.50».

**Reglas de negocio relacionadas:** RN-053, RN-054, RN-055, RN-056, RN-057, RN-066, RN-079, RN-092, RN-096, RN-097, RN-105, RN-106, RN-108, RN-125, RN-126, RN-129, RN-130, RN-162, RN-181, RN-183
**Notas:** se mantienen las notas originales.

---

## E5 · Operación diaria (F-M05)

### HU-010: Recibir los lotes de la finca con control de temperatura
**Prioridad:** Must

Como **operador**, quiero **saber cuántos litros pedir a la finca y registrar cada lote que llega con su temperatura**, para **repartir solo leche que cumple la cadena de frío**.

**Validación INVEST:** ⚠️ **S**: tiene 9 criterios y 16 reglas.
**Corrección sugerida:** partirla en **HU-010a** (demanda y pedido a la finca), **HU-010b** (registro de lote, temperatura y corrección) y **HU-010c** (revisión de 15 minutos, invalidación, reasignación y escasez).
**Revisión de los criterios existentes:** son correctos. Falta el borde exacto de los 900 s, la precisión decimal, el empate en la escasez y los permisos.

**Criterios de aceptación:**
- DADO QUE el viernes 13/11/2026 a las 18:00:00 hay entregas `programada` que suman 96 L, más 2 reprogramadas de 4 L, CUANDO se cumple el corte, ENTONCES el sistema calcula una demanda de 100 L para el sábado 14/11/2026 y la registra como litros comprometidos del pedido a la finca.
- DADO QUE registro un lote sin temperatura, CUANDO lo guardo, ENTONCES el sistema rechaza el registro.
- DADO QUE registro un lote a 4.5 °C, CUANDO lo guardo, ENTONCES queda en estado `recibido`; DADO QUE lo registro a 4.6 °C a las 06:10:00, ENTONCES queda `en_revision`, el operador y el administrador reciben una alerta crítica en 60 s como máximo y sus botellas no se asignan a entregas nuevas. ✅ ADR-004
- DADO QUE el lote L-2026-1114-01 pasó a `en_revision` a las 06:10:00 y nadie lo revierte, CUANDO llegan las 06:25:00 (900 s), ENTONCES queda `invalidado` y sus entregas `programada` pasan al lote L-2026-1114-02, que tiene 30 L libres.
- DADO QUE quiero corregir una lectura de 5.1 °C, CUANDO intento sobrescribirla, ENTONCES el sistema lo rechaza; CUANDO registro una corrección a 4.1 °C con la justificación "error de digitación", ENTONCES quedan los dos registros con mi usuario y la hora.
- DADO QUE llegan 5 L para tres entregas (A, alta 01/10/2026, 3 L; B, alta 05/10/2026, 3 L; C, alta 10/10/2026, 2 L), CUANDO registro el lote, ENTONCES se cubren A y C, y B queda `fallida` con motivo `falla_abastecimiento_finca`.
- DADO QUE soy el operador y corrijo a 4.1 °C la lectura de un lote `en_revision`, CUANDO guardo la corrección, ENTONCES queda registrada pero el lote sigue `en_revision`.
- DADO QUE el lote pasó a `en_revision` a las 05:50:00, CUANDO el administrador titular corrige la lectura a 3.8 °C con justificación a las 05:58, ENTONCES el lote vuelve a `recibido` y sus entregas siguen su curso.
- DADO QUE el lote pasó a `en_revision` a las 05:50:00, CUANDO el administrador intenta revertirlo a las 06:10 (después de los 900 s), ENTONCES el lote ya está `invalidado` y el sistema rechaza la reversión.
- **[NUEVO][Borde]** DADO QUE el lote pasó a `en_revision` a las 05:50:00, CUANDO el administrador corrige la lectura a 3.8 °C con justificación a las 06:04:59 (899 s), ENTONCES el lote vuelve a `recibido`; a las 06:05:00 (900 s) ya está `invalidado`.
- **[NUEVO][Negativo]** DADO QUE registro una temperatura de `3.52` °C, CUANDO la guardo, ENTONCES el sistema la rechaza, porque debe tener exactamente 1 decimal (RN-087).
- **[NUEVO][Negativo]** DADO QUE registro un lote sin litros comprometidos, CUANDO lo guardo, ENTONCES el sistema rechaza el registro (RN-028).
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento registrar un lote, ENTONCES el sistema lo rechaza (RN-099).
- **[NUEVO][Negativo]** DADO QUE el administrador intenta revertir un lote `en_revision` sin escribir una justificación, CUANDO guarda la corrección, ENTONCES el sistema la rechaza (RN-127).
- **[NUEVO][Borde]** DADO QUE llegan 3 L para dos entregas de 3 L cuyas suscripciones se crearon el mismo instante (identificadores 101 y 102), CUANDO registro el lote, ENTONCES se cubre la de identificador 101 y la 102 queda `fallida` (RN-122).
- **[NUEVO][Borde]** DADO QUE un lote invalidado tiene 12 entregas `programada` y no hay ningún otro lote del día con litros libres, CUANDO se invalida, ENTONCES las 12 quedan `fallida` con causa `imputable_muuvi` y cada cliente recibe el aviso de elección (RN-054 y RN-129).
- **[NUEVO][Borde] ❓ Sin RN** DADO QUE el viernes 13/11/2026 a las 18:00:00 no hay ninguna entrega `programada` para el sábado, CUANDO se cumple el corte, ENTONCES *(propuesta)* la demanda es de 0 L y no se pide leche a la finca.

- **[NUEVO v0.4.0][Borde] RN-157** DADO QUE la demanda del sábado 14/11/2026 es de 100 L, CUANDO registro a las 06:00 un lote de 64 L, ENTONCES veo la advertencia «Faltan 36 L» y ninguna entrega pasa a `fallida`; CUANDO registro a las 06:30 otro lote de 30 L y cierro la recepción a las 06:40, ENTONCES recién las entregas que suman los 6 L faltantes pasan a `fallida` por escasez.
- **[NUEVO v0.4.0][Negativo] RN-158** DADO QUE el sábado 14/11/2026 a las 06:45:00 la recepción sigue abierta, CUANDO se cumple esa hora, ENTONCES el operador y el administrador reciben una alerta crítica.
- **[NUEVO v0.4.0][Negativo] RN-159** DADO QUE la demanda del día es de 100 L, CUANDO cargo pedidos de 70 L a Finca Santa Rosa y 40 L a La Ponderosa, ENTONCES el sistema los rechaza porque suman 110 L; con 70 L y 30 L los acepta y el lote de Santa Rosa se registra con 70 L comprometidos.
- **[NUEVO v0.4.0][Positivo] RN-155** DADO QUE confirmé la asignación del sábado 14/11/2026, CUANDO imprimo la hoja de ruta de Juan, ENTONCES sale con sus 12 paradas en orden, con dirección, referencia y litros, y la auditoría registra la impresión.

**Reglas de negocio relacionadas:** RN-024, RN-026, RN-027, RN-028, RN-054, RN-085, RN-087, RN-088, RN-089, RN-090, RN-091, RN-092, RN-093, RN-099, RN-122, RN-127, RN-128, RN-129, RN-134, RN-155, RN-157, RN-158, RN-159
**Notas:** se mantienen las notas originales (umbral pendiente de validación sanitaria; 1 decimal).

---

### HU-011: Cerrar mis entregas del día en ruta
**Prioridad:** Must

Como **repartidor**, quiero **registrar cada entrega con su temperatura y las botellas que recojo**, para **que Muuvi sepa qué se entregó y en qué condiciones**.

**Validación INVEST:** ⚠️ **I**: depende de HU-010. ⚠️ **S**: tiene 8 criterios y 20 reglas.
**Corrección sugerida:** partirla en **HU-011a** (entrega normal y botellas), **HU-011b** (lectura alta y lote en revisión en ruta) y **HU-011c** (entrega fallida y reprogramación).
**Revisión de los criterios existentes:** son correctos. Falta el borde de 4.5 °C exactos en la puerta, la devolución igual al saldo, el permiso sobre entregas ajenas y el trabajo sin conexión.

**Criterios de aceptación:**
- DADO QUE entrego 3 L a 4.1 °C y recojo 2 botellas de un cliente con saldo de 2, CUANDO cierro la entrega como `entregada`, ENTONCES el saldo del cliente queda en 3 botellas (2 − 2 + 3).
- DADO QUE intento cerrar una entrega como `entregada` sin temperatura, CUANDO la envío, ENTONCES el sistema rechaza el cierre.
- DADO QUE el cliente tiene saldo de 2 botellas, CUANDO registro 3 devueltas, ENTONCES el sistema rechaza el registro.
- DADO QUE registro 5.1 °C en la puerta del cliente a las 08:20:00, CUANDO lo guardo, ENTONCES no puedo cerrar la entrega como `entregada`, la botella no se entrega, el lote pasa a `en_revision` y los demás repartidores con entregas de ese lote reciben en 60 s como máximo el aviso "Lote en revisión, no entregar". ✅ ADR-004
- DADO QUE el lote sigue `en_revision` a las 08:35:00 (900 s), CUANDO se invalida, ENTONCES mis entregas `en_ruta` de ese lote pasan a `suspendida` y los clientes que ya recibieron leche de ese lote reciben un aviso de disculpa por correo en 3 600 s como máximo; DADO QUE el administrador lo revierte antes, ENTONCES puedo seguir entregando.
- DADO QUE el cliente no estaba el sábado 14/11/2026, CUANDO marco la entrega como `fallida` con causa `imputable_cliente`, ENTONCES se reprograma para el martes 17/11/2026, 07:00–10:00, sin un cobro nuevo y con botellas de un lote de ese día.
- DADO QUE la entrega reprogramada del 17/11/2026 vuelve a fallar por causa del cliente, CUANDO la marco, ENTONCES pasa a `cancelada` y no se reprograma de nuevo.
- DADO QUE marco una entrega como `fallida` sin causa, CUANDO la envío, ENTONCES el sistema rechaza el registro.
- **[NUEVO][Borde]** DADO QUE registro exactamente 4.5 °C en la puerta del cliente, CUANDO cierro la entrega, ENTONCES puedo cerrarla como `entregada` y el lote sigue `recibido`.
- **[NUEVO][Borde]** DADO QUE el cliente tiene saldo de 2 botellas, CUANDO entrego 2 L y registro 2 devueltas, ENTONCES el sistema lo acepta y el saldo queda en 2 (2 − 2 + 2).
- **[NUEVO][Borde]** DADO QUE el cliente tiene saldo de 0 botellas (primera entrega), CUANDO entrego 3 L y registro 0 devueltas, ENTONCES el saldo queda en 3.
- **[NUEVO][Negativo]** DADO QUE intento cerrar una entrega asignada a otro repartidor, CUANDO la envío, ENTONCES el sistema lo rechaza y no me muestra la dirección de ese cliente (RN-099).
- **[NUEVO][Negativo]** DADO QUE una entrega ya está `entregada`, CUANDO intento volver a pasarla a `en_ruta`, ENTONCES el sistema rechaza la transición (RN-071).
- **[NUEVO][Negativo]** DADO QUE el lote está `en_revision`, CUANDO intento cerrar como `entregada` otra entrega de ese lote a 3.9 °C, ENTONCES el sistema lo rechaza con el aviso "Lote en revisión, no entregar" (RN-132).
- **[NUEVO][Borde]** DADO QUE el sábado 05/12/2026 falla por causa del cliente una entrega cuya reprogramación caería el martes 08/12/2026 (feriado), CUANDO se reprograma, ENTONCES pasa al jueves 10/12/2026 y el cliente recibe el aviso de la nueva fecha por correo en 300 s como máximo (RN-139 y RN-140).
- **[NUEVO][Negativo]** DADO QUE estoy en ruta sin datos móviles a las 08:15, CUANDO cierro la entrega 9930 como `entregada` a 4.1 °C con 2 botellas devueltas, ENTONCES la PWA guarda el cierre en el dispositivo sin datos personales del cliente (RN-141 y RN-145).
- **[NUEVO][Positivo]** DADO QUE guardé cierres sin conexión a las 08:15 y a las 08:25, CUANDO recupero la señal a las 08:40, ENTONCES se envían en ese orden y cada entrega figura cerrada a su hora real (`2026-11-14T13:15:00Z` y `2026-11-14T13:25:00Z`) (RN-142 y RN-143).
- **[NUEVO][Borde]** DADO QUE la señal se corta a mitad de la sincronización, CUANDO la PWA reenvía el cierre de la entrega 9930, ENTONCES queda registrado una sola vez y el saldo de botellas no se duplica (RN-144).
- ~~**[NUEVO][Negativo] ⛔ K8** DADO QUE cerré sin conexión una entrega de un lote que a esa hora ya estaba `en_revision`, CUANDO se sincroniza, ENTONCES *(pendiente: K8)*.~~ Reemplazado por el criterio de RN-185.

- **[NUEVO v0.4.0][Positivo] RN-155 y RN-145** DADO QUE salgo a ruta con la hoja impresa, CUANDO pierdo la señal, ENTONCES la PWA me muestra el orden, el número de entrega y los litros de cada parada, y la dirección la leo en la hoja; el teléfono no guarda direcciones.
- **[NUEVO v0.4.0][Positivo] RN-156** DADO QUE registro 5.1 °C en la puerta a las 08:20:00, CUANDO guardo la lectura, ENTONCES la entrega queda «Retenida» (sigue `en_ruta`, sin cerrar), no entrego la botella y el lote pasa a `en_revision`; si el administrador lo revierte, puedo cerrarla; si se invalida, pasa a `suspendida`.
- **[NUEVO v0.4.0][Positivo] RN-182** DADO QUE el cliente no estaba el sábado 14/11/2026, CUANDO cierro la entrega como `fallida` con causa `imputable_cliente` a las 08:10, ENTONCES el cliente recibe a las 08:15 como máximo el aviso de que se la llevamos el martes 17/11/2026, y en mi ruta del martes esa entrega figura como «Reprogramada · último intento».
- **[NUEVO v0.4.0][Borde] RN-185 (K8, resuelto)** DADO QUE el lote pasó a `en_revision` a las 08:20:00 y yo, sin señal, cerré una entrega de ese lote a las 08:25, CUANDO sincronizo a las 08:40, ENTONCES la entrega queda `entregada` con la marca `entregada_durante_revision` y el operador recibe una alerta crítica. *(Reemplaza el criterio ⛔ K8, desde la aprobación del 14/11/2026.)*

**Reglas de negocio relacionadas:** RN-025, RN-029, RN-030, RN-031, RN-052, RN-065, RN-066, RN-067, RN-068, RN-070, RN-071, RN-076, RN-077, RN-078, RN-086, RN-099, RN-107, RN-113, RN-132, RN-133, RN-134, RN-139, RN-140, RN-141, RN-142, RN-143, RN-144, RN-145, RN-155, RN-156, RN-182, RN-185
**Notas:** se mantienen las notas originales. ✅ K9 resuelto el 14/11/2026: hoja de ruta impresa (RN-155). ✅ K8 resuelto el 14/11/2026 (RN-185).

---

## E6 · Libro de Reclamaciones (F-M06)

### HU-012: Registrar un reclamo y recibir respuesta a tiempo
**Prioridad:** Must

Como **consumidor, tenga o no cuenta**, quiero **registrar un reclamo o una queja en el Libro de Reclamaciones virtual**, para **que Muuvi me responda dentro del plazo legal**.

**Validación INVEST:** ✅ Cumple todos, pero la **V** (valor) queda incompleta mientras no exista una regla para registrar y enviar la **respuesta** al consumidor (ver HU-016 sugerida).
**Revisión de los criterios existentes:** el quinto criterio ("cruza un feriado") no tiene valores concretos. Sugiero el ejemplo del [NUEVO][Borde] con el 08/12/2026.

**Criterios de aceptación:**
- DADO QUE no he iniciado sesión, CUANDO abro "Libro de Reclamaciones" desde el menú principal, ENTONCES puedo registrar mi reclamo sin que me pida iniciar sesión.
- DADO QUE registro un reclamo, CUANDO se guarda, ENTONCES recibe un código correlativo (por ejemplo `LR-2026-000017`) y me llega la constancia con ese código al correo que indiqué.
- DADO QUE un reclamo se registra el lunes 16/11/2026, CUANDO el operador lo abre, ENTONCES ve como fecha límite de respuesta el lunes 07/12/2026 (15 días hábiles).
- DADO QUE el reclamo del 16/11/2026 sigue sin respuesta, CUANDO llega el lunes 30/11/2026 (10.º día hábil), ENTONCES el operador recibe una alerta.
- DADO QUE el plazo cruza un feriado nacional cargado en el calendario, CUANDO se calcula la fecha límite, ENTONCES ese feriado no se cuenta como día hábil.
- **[NUEVO][Borde]** DADO QUE un reclamo se registra el martes 24/11/2026 y el martes 08/12/2026 es feriado, CUANDO el operador lo abre, ENTONCES la fecha límite es el miércoles 16/12/2026, y no el martes 15/12/2026.
- **[NUEVO][Positivo]** DADO QUE se registran dos reclamos seguidos, CUANDO se guardan, ENTONCES reciben códigos correlativos consecutivos, por ejemplo `LR-2026-000017` y `LR-2026-000018` (RN-034).
- **[NUEVO][Borde]** DADO QUE un reclamo se registra el viernes 20/11/2026, CUANDO el operador lo abre, ENTONCES la fecha límite es el lunes 14/12/2026 (15 días hábiles, sin sábados, domingos ni el feriado del 08/12).
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento responder un reclamo, ENTONCES el sistema lo rechaza (RN-099).
- **[NUEVO][Borde] ❓ Sin RN (QA-037)** DADO QUE un reclamo se registra el sábado 21/11/2026, CUANDO se calcula la fecha límite, ENTONCES *(propuesta)* se cuenta desde el lunes 23/11/2026 y el límite es el lunes 14/12/2026.
- **[NUEVO][Negativo] ❓ Sin RN** DADO QUE dejo vacío el correo en el formulario, CUANDO lo envío, ENTONCES *(propuesta)* el sistema lo rechaza, porque la constancia se envía a ese correo (RN-112).

**Reglas de negocio relacionadas:** RN-033, RN-034, RN-035, RN-036, RN-099, RN-100, RN-101, RN-109, RN-112
**Notas:** se mantienen las notas originales.

---

## Resumen INVEST y correcciones

| HU | I | N | V | E | S | T | Corrección sugerida |
|---|---|---|---|---|---|---|---|
| HU-001 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Ninguna. |
| HU-002 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Ninguna. |
| HU-003 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Ninguna. |
| HU-004 | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ | Sembrar un cliente de prueba para trabajarla en paralelo. |
| HU-005 | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ | Sembrar una suscripción activa. |
| HU-006 | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ | Sembrar una suscripción activa. |
| HU-007 | ⚠️ | ✅ | ✅ | ⚠️ | ❌ | ⚠️ | **Partir** en 007a, 007b y 007c. Concretar la clave de idempotencia del primer criterio. Un criterio espera P23. |
| HU-008 | ⚠️ | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | Elegir el OSE/PSE. El criterio de saldo espera al contador. |
| HU-009 | ⚠️ | ✅ | ✅ | ⚠️ | ❌ | ⚠️ | **Partir** en 009a y 009b. Separar el segundo criterio en dos. |
| HU-010 | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | **Partir** en 010a, 010b y 010c. C-01 ya está resuelto (ADR-004). |
| HU-011 | ⚠️ | ✅ | ✅ | ✅ | ❌ | ✅ | **Partir** en 011a, 011b y 011c. K8 y K9 resueltos el 14/11/2026. |
| HU-012 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | La respuesta al reclamo queda cubierta por la HU-016 aprobada. |
| HU-018 | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ | Propuesta el 14/11/2026. En producción depende de G6. |

**❌ en S** significa que la historia no cabe razonablemente en un sprint tal como está. Con la partición sugerida pasan a ✅.

---

## Escenarios sin regla de negocio
Estos escenarios aparecen en los criterios marcados con **❓ Sin RN**. El BA debe escribir la regla antes de que un agente los implemente (PRINCIPIO-008: ante la duda, detenerse).

| # | Escenario | HU | Pregunta para el equipo |
|---|---|---|---|
| SR-01 | Corte de conexión al registrarse | HU-001 | ¿Cómo se evita una cuenta duplicada? |
| ~~SR-02~~ | Correo ya registrado | HU-001 | ✅ Resuelto el 14/11/2026: se rechaza con mensaje explícito (RN-154). |
| SR-03 | Nacido un 29/02 | HU-001 | ¿Cuándo cumple 18 años en un año no bisiesto? (QA-057) |
| ~~SR-04~~ | Catálogo vacío | HU-003 | ✅ Resuelto con RN-178 (aprobada el 14/11/2026) (14/11/2026), pendiente de aprobación. |
| ~~SR-05~~ | Suscribirse sin medio de pago | HU-004 | ✅ Resuelto con RN-179 (aprobada el 14/11/2026): se exige un medio activo. |
| SR-06 | Pausa con plazo y traslado por feriado | HU-005 | ¿La entrega trasladada cuenta como una de las omitidas? |
| ~~SR-07~~ | Pausar una suscripción `suspendida_por_pago` | HU-005 | ✅ Resuelto con RN-180 (aprobada el 14/11/2026): no se permite. |
| SR-08 | Modificar o cancelar una suscripción ya cancelada | HU-006 | ¿Se rechaza? |
| ~~SR-09~~ | La pasarela no responde (timeout) | HU-007 | ✅ Resuelto: pago `en_verificacion` (RN-135 a RN-138). Falta P23. |
| ~~SR-10~~ | El cliente elige después de las 72 h | HU-009 | ✅ Resuelto con RN-181 (aprobada el 14/11/2026): se rechaza. |
| SR-11 | Demanda de 0 L | HU-010 | ¿Se le avisa algo a la finca? |
| ~~SR-12~~ | Reprogramación a un día feriado | HU-011 | ✅ Resuelto (RN-139 y RN-140). |
| ~~SR-13~~ | Repartidor sin conexión | HU-011 | ✅ Resuelto (RN-141 a RN-145). Quedan K8 y K9. |
| SR-14 | Reclamo registrado en sábado | HU-012 | ¿Desde cuándo se cuenta el plazo? (QA-037) |
| SR-15 | Reclamo sin correo | HU-012 | ¿El correo es obligatorio? |

---

## HUs sugeridas
Reglas de negocio que no tienen una HU que las ejercite directamente:

### HU-013: Cambiar el precio por litro ✅ Aprobada (14/11/2026) · Iteración 1
**Prioridad:** Must ⛔ (un criterio depende de P14)
**Nota (14/11/2026):** se adelanta a la iteración 1 porque es la única forma de cargar el precio inicial en producción (QA-C2-004). La carga inicial se hace sin suscripciones activas, así que no la bloquea P14.

Como **operador**, quiero **proponer un nuevo precio por litro y que el administrador titular lo confirme**, para **ajustar el precio sin afectar ciclos ya cobrados**.

**Criterios de aceptación propuestos:**
- **[NUEVO][Positivo]** DADO QUE el operador propone S/ 15.00 el sábado 14/11/2026 a las 09:00, CUANDO el administrador titular reingresa su contraseña e ingresa a las 09:03 el código de 6 dígitos que recibió por correo a las 09:01, ENTONCES `precio_litro = 15.00` y la auditoría registra la propuesta, los dos pasos y la confirmación (RN-151 y RN-153).
- **[NUEVO][Positivo]** DADO QUE `precio_litro` no tiene valor y no hay suscripciones activas, CUANDO el operador propone S/ 14.50 y el administrador titular lo confirma con los dos pasos, ENTONCES el catálogo muestra "S/ 14.50 por litro" (carga inicial; RN-011 y RN-153).
- **[NUEVO][Borde]** DADO QUE el precio cambia el sábado 14/11/2026, CUANDO se entrega el pedido del 14/11/2026, cobrado el 13/11/2026, ENTONCES mantiene S/ 14.50 (RN-102).
- **[NUEVO][Negativo]** DADO QUE el operador propone S/ 15.00, CUANDO el administrador no lo confirma, ENTONCES el precio sigue en S/ 14.50 (RN-032 y RN-153).
- **[NUEVO][Negativo]** DADO QUE el código se envió a las 09:01:00, CUANDO el administrador lo ingresa a las 09:06:01 (301 s después), ENTONCES la confirmación se cancela y el precio no cambia (RN-152).
- **[NUEVO][Negativo]** DADO QUE el administrador ingresa mal el código 3 veces, CUANDO falla el tercer intento, ENTONCES la confirmación se cancela y el precio no cambia (RN-152).
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento proponer un precio, ENTONCES el sistema lo rechaza (RN-099).
- ⛔ DADO QUE se confirma el precio nuevo y hay suscripciones activas, CUANDO se avisa a los suscriptores, ENTONCES el aviso llega con *(P14)* días de anticipación (RN-103).

- **[NUEVO v0.4.0][Negativo] RN-177** DADO QUE hay una propuesta pendiente de S/ 15.00, CUANDO propongo S/ 15.50, ENTONCES el sistema la rechaza; DADO QUE el código de la propuesta de S/ 15.00 vence, ENTONCES la propuesta queda `descartada` y ya no se puede confirmar.

**Reglas de negocio relacionadas:** RN-011, RN-032, RN-099, RN-102, RN-103, RN-151, RN-152, RN-153, RN-177
**Notas:** en producción, el código por correo requiere el proveedor de correo transaccional con DPA vigente (RN-118).

### HU-014: Anular un registro crítico con doble confirmación ✅ Aprobada (14/11/2026)
**Prioridad:** Must

Como **administrador titular**, quiero **anular un pago, un comprobante, una suscripción o una entrega solo con doble confirmación**, para **evitar anulaciones por error y dejar rastro en la auditoría**.

**Criterios de aceptación propuestos:**
- **[NUEVO][Positivo]** DADO QUE pido anular el pago 5521 a las 10:00:00 y reingreso mi contraseña, CUANDO ingreso a las 10:02:10 el código que recibí por correo a las 10:00:05, ENTONCES el pago queda anulado y la tabla de auditoría registra la solicitud, los dos pasos, mi usuario y la hora (RN-151).
- **[NUEVO][Negativo]** DADO QUE soy operador, CUANDO intento anular un pago, ENTONCES el sistema lo rechaza (RN-099).
- **[NUEVO][Negativo]** DADO QUE un proceso automático intenta borrar entregas de más de 90 días, CUANDO se ejecuta, ENTONCES el sistema lo rechaza (RN-032).
- **[NUEVO][Negativo]** DADO QUE reingreso mi contraseña, CUANDO no ingreso el código, ENTONCES no se anula nada (RN-151).
- **[NUEVO][Borde]** DADO QUE el código se envió a las 10:00:05, CUANDO lo ingreso a las 10:05:05 (300 s después), ENTONCES la anulación se ejecuta; a las 10:05:06 la solicitud ya está cancelada (RN-152).
- **[NUEVO][Negativo]** DADO QUE ya usé el código para anular el pago 5521, CUANDO intento usarlo otra vez para anular el pago 5522, ENTONCES el sistema lo rechaza, porque cada código es de un solo uso (RN-151).

- **[NUEVO v0.4.0][Positivo] RN-168 y RN-172 (⚠️ falta el contador)** DADO QUE anulo el pago confirmado 5521 de S/ 43.50, CUANDO se ejecuta la anulación a las 10:02:10, ENTONCES se solicita el reembolso íntegro antes de las 11:02:10, se emite la nota de crédito de la boleta B001-00000123, la entrega del 14/11/2026 pasa a `cancelada` si seguía `programada`, y el cliente recibe el aviso por correo antes de las 10:07:10.
- **[NUEVO v0.4.0][Negativo] RN-171** DADO QUE la entrega 9930 ya está `entregada`, CUANDO intento anularla, ENTONCES el sistema lo rechaza.
- **[NUEVO v0.4.0][Positivo] RN-168 a RN-171** DADO QUE elegí anular un pago, un comprobante, una suscripción o una entrega, CUANDO llego a la doble confirmación, ENTONCES veo la lista de efectos de esa anulación antes de ingresar mi contraseña.

**Reglas de negocio relacionadas:** RN-032, RN-099, RN-151, RN-152, RN-168 a RN-172 ⚠️ (contador)

### HU-015: Cargar el calendario de feriados ✅ Aprobada (14/11/2026)
**Prioridad:** Must (tarea inicial: poblar la tabla de feriados de 2026 y 2027)

Como **operador**, quiero **cargar los feriados nacionales del año desde la fuente oficial**, para **que los repartos y los plazos de reclamos se calculen bien**.

**Criterios de aceptación propuestos:**
- **[NUEVO][Positivo]** DADO QUE cargo el martes 08/12/2026 como feriado nacional, CUANDO se calcula la semana, ENTONCES las entregas de ese martes pasan al jueves 10/12/2026 (RN-123) y el día no cuenta como hábil (RN-101).
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento cargar un feriado, ENTONCES el sistema lo rechaza (RN-099).
- **[NUEVO][Borde]** DADO QUE el calendario del año siguiente no está cargado, CUANDO llega el 01/12/2026, ENTONCES ❓ Sin RN: *(propuesta)* el operador recibe una alerta para cargarlo.

- **[NUEVO v0.4.0][Negativo] RN-173** DADO QUE es el lunes 07/12/2026 a las 19:00, CUANDO intento cargar el feriado del 08/12/2026, ENTONCES el sistema lo rechaza, porque el corte de ese día ya ocurrió.
- **[NUEVO v0.4.0][Positivo] RN-174** DADO QUE cargué por error el martes 15/12/2026, CUANDO el administrador titular lo retira el 10/12/2026, ENTONCES sus 14 entregas vuelven al 15/12/2026 y los clientes reciben el aviso por correo; si lo intenta el operador, el sistema lo rechaza.
- **[NUEVO v0.4.0][Borde] RN-175** DADO QUE el 01/12/2026 a las 08:00 no hay feriados de 2027 cargados, CUANDO se cumple esa hora, ENTONCES el operador recibe la alerta «Carga los feriados de 2027» (reemplaza la propuesta ❓ anterior).

**Reglas de negocio relacionadas:** RN-099, RN-100, RN-101, RN-123, RN-173 a RN-175

### HU-016: Responder un reclamo ✅ Aprobada (14/11/2026)
**Prioridad:** Must

Como **operador**, quiero **registrar la respuesta a un reclamo y enviarla al consumidor**, para **cumplir el plazo de 15 días hábiles de la Ley 31435**.

**Criterios de aceptación:**
- **[NUEVO][Positivo]** DADO QUE el reclamo `LR-2026-000017` se registró el lunes 16/11/2026, CUANDO lo respondo el miércoles 25/11/2026 a las 11:00:00, ENTONCES pasa a `respondido` con mi usuario y la hora, y la respuesta llega al correo del consumidor antes de las 11:05:00 (RN-146 y RN-148).
- **[NUEVO][Negativo]** DADO QUE intento guardar una respuesta sin texto, CUANDO la envío, ENTONCES el sistema la rechaza (RN-147).
- **[NUEVO][Borde]** DADO QUE el reclamo del 16/11/2026 se respondió el 25/11/2026, CUANDO llega el lunes 30/11/2026 (10.º día hábil), ENTONCES el operador no recibe ninguna alerta (RN-149).
- **[NUEVO][Borde]** DADO QUE el reclamo del 16/11/2026 se responde el lunes 07/12/2026, último día del plazo, CUANDO se guarda, ENTONCES queda dentro del plazo legal.
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento responder un reclamo, ENTONCES el sistema lo rechaza (RN-099).
- ⛔ DADO QUE un reclamo necesita **derivación**, CUANDO el operador la registra, ENTONCES *(pendiente: P24, qué es y a quién se deriva)*.

**Reglas de negocio relacionadas:** RN-035, RN-036, RN-099, RN-146, RN-147, RN-148, RN-149
**Notas:** las reglas RN-146 a RN-149 son un borrador basado en la Ley 31435 y requieren validación del asesor legal.

### HU-017: Registrar fincas y productos ✅ Propuesta (14/11/2026, QA-C2-004)
**Prioridad:** Must

Como **operador**, quiero **registrar las fincas proveedoras y publicar sus productos con su registro DIGESA y su certificación SENASA**, para **que el catálogo muestre leche legalmente vendible y su finca de origen**.

**Validación INVEST:** ✅ Cumple todos. Es requisito de HU-003 y HU-004: sin productos publicados no hay catálogo ni suscripción.

**Criterios de aceptación:**
- **[NUEVO][Positivo]** DADO QUE registro la finca "Finca Santa Rosa", de la provincia de Huaral, con certificación SENASA vigente hasta el 30/11/2026, CUANDO la guardo, ENTONCES aparece en la ficha de finca del catálogo con la etiqueta "Orgánico" (RN-010).
- **[NUEVO][Positivo]** DADO QUE registro "Leche fresca 1 L" de esa finca con registro DIGESA, CUANDO la publico, ENTONCES aparece en el catálogo (RN-009).
- **[NUEVO][Negativo]** DADO QUE el producto no tiene registro DIGESA, CUANDO intento publicarlo, ENTONCES el sistema lo rechaza y no aparece en el catálogo (RN-009).
- **[NUEVO][Negativo]** DADO QUE ingreso una provincia distinta de Cañete o Huaral, CUANDO guardo la finca, ENTONCES el sistema la rechaza (alcance §5.3).
- **[NUEVO][Borde]** DADO QUE actualizo la certificación SENASA de la finca con vencimiento el 30/11/2027, CUANDO la guardo el 01/12/2026, ENTONCES la etiqueta "Orgánico" vuelve a mostrarse (RN-010).
- **[NUEVO][Negativo]** DADO QUE soy repartidor, CUANDO intento registrar una finca o publicar un producto, ENTONCES el sistema lo rechaza (RN-099).
- **[NUEVO][Positivo]** DADO QUE publico o despublico un producto, CUANDO se guarda, ENTONCES la tabla de auditoría registra la acción con mi usuario y la hora.

- **[NUEVO v0.4.0][Negativo] RN-163** DADO QUE el producto 8 tiene 40 suscripciones no canceladas, CUANDO intento despublicarlo, ENTONCES el sistema lo rechaza con «40 suscripciones usan este producto».

**Reglas de negocio relacionadas:** RN-009, RN-010, RN-099, RN-163
**Notas:** los datos de la finca (nombre, código SENASA, vencimiento y registro DIGESA) son datos de una empresa, no datos personales; se guardan en claro. Los documentos de respaldo de DIGESA y SENASA los custodia un humano.

### HU-018: Recuperar mi contraseña ✅ Propuesta (14/11/2026, OBS-08 de `10-validation-log.md`)
**Prioridad:** Must (decisión del fundador del 14/11/2026: la recuperación entra en el PMV)

Como **cliente o miembro del personal interno**, quiero **recuperar mi contraseña por correo**, para **volver a entrar a mi cuenta sin depender de soporte**. El caso crítico es el administrador titular, porque su contraseña es el primer paso de la doble confirmación (RN-151).

**Validación INVEST:** ✅ Cumple todos. ⚠️ **T**: en producción depende del proveedor de correo (G6).

**Criterios de aceptación:**
- **[NUEVO][Positivo] RN-165** DADO QUE Ana tiene cuenta con `ana@example.com`, CUANDO pide recuperar su contraseña el 14/11/2026 a las 10:00:00, ENTONCES recibe un enlace de un solo uso válido hasta las 10:30:00.
- **[NUEVO][Negativo] RN-165** DADO QUE `nadie@example.com` no tiene cuenta, CUANDO alguien pide recuperar esa contraseña, ENTONCES ve el mismo mensaje que una persona con cuenta («Si el correo tiene una cuenta, te enviamos un enlace») y no se envía ningún correo.
- **[NUEVO][Borde] RN-166** DADO QUE el enlace se envió a las 10:00:00, CUANDO lo uso a las 10:29:59, ENTONCES puedo cambiar la contraseña; a las 10:30:01 el sistema responde «El enlace venció. Pide uno nuevo».
- **[NUEVO][Negativo] RN-166** DADO QUE ya usé el enlace, CUANDO intento usarlo otra vez, ENTONCES el sistema lo rechaza.
- **[NUEVO][Positivo] RN-167** DADO QUE el administrador titular tiene una confirmación de precio pendiente y una sesión abierta en la tableta, CUANDO restablece su contraseña, ENTONCES la sesión de la tableta se cierra, la confirmación pasa a `cancelada` y la auditoría registra el cambio.
- **[NUEVO][Negativo]** DADO QUE ingreso una contraseña nueva de 9 caracteres, CUANDO la guardo, ENTONCES el sistema la rechaza, porque el mínimo es de 10 caracteres (el mismo de P-003).
- **[NUEVO][Negativo]** DADO QUE pido 6 enlaces en una hora desde la misma IP, CUANDO pido el sexto, ENTONCES recibo `429` (el mismo límite del registro, arquitectura §4.2).

**Reglas de negocio relacionadas:** RN-007, RN-118, RN-151, RN-165, RN-166, RN-167
**Notas:** el enlace lleva un token aleatorio de un solo uso; en la base se guarda solo su hash y nunca aparece en los logs (RN-007).

### Reglas SHOULD sin HU
RN-042 y RN-043 (avisos dentro de la ventana de 09:00 a 20:00), RN-044 (trazabilidad por lote para el cliente) y RN-117 (consentimiento para fines comerciales). Se recomienda escribir sus HU al planificar F-S02, F-S03 y las campañas comerciales.

### Reglas cubiertas por la Definición de Terminado
RN-007, RN-037 a RN-041, RN-099 y RN-110 se verifican en todas las HU a través de la Definición de Terminado de `06-user-stories.md`. RN-118 (proveedores con DPA) es un requisito previo de salida a producción y no necesita una HU.
