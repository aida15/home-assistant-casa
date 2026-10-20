# Pendientes

## Fase 1 · Descubrimiento (datos que faltan)

- [x] microSD: SanDisk 32 GB clase 10
- [x] Zigbee: LinknLink ZG-808Z + Sonoff ZBBridge-P
- [x] Roomba: Roomba Plus 405 Combo (no soportado por la integración oficial)
- [x] ZBMINI: 4 → luz techo dorm. Aroa, salón ×2, cocina
- [x] ZBMINI: ZBMINIL2 (sin neutro)
- [ ] Sensores Wi-Fi sin funcionar: **despriorizados por decisión de Aida** (2026-10-19); se retoman en Fase 6
- [ ] Modelos restantes (ver `01_inventario/dispositivos.md`)
- [x] Bloque C · Vivienda y distribución 
- [x] Bloque D · Preferencias de diseño (`04_dashboards/direccion_visual.md`)
- [x] Bloque E · Uso cotidiano (`00_proyecto/uso_cotidiano.md`)
- [x] Bloque F · Seguridad, privacidad y mantenimiento
- [x] SSD en desuso: montado en caja USB
- [x] Resumen de requisitos (`00_proyecto/resumen_requisitos_fase1.md`) validado por Aida → **Fase 1 cerrada (2026-10-19)**

## Fase 2 · Auditoría

- [x] Auditoría redactada (`02_arquitectura/auditoria_fase2.md`)
- [ ] Prueba del frontend de HA en la tablet Huawei (R-10) — 1.er intento: la app se queda en «buscando en la red doméstica» → probar con IP manual
- [x] Validación de la auditoría por Aida → **Fase 2 cerrada (2026-10-19)**

## Fechas clave

- 🗓️ **Finales de marzo 2027** — renovación de la suscripción tado Auto-Assist. Antes de esa fecha: decidir estrategia de calefacción (seguir con tado, sustituir funciones por HA o cambiar de cabezales).

## Riesgos detectados para la auditoría (Fase 2)

Hipótesis preliminares, a confirmar:

- **R-01 · microSD como almacenamiento** — SanDisk 32 GB clase 10, sin clasificación A1/A2 indicada. Riesgo alto de desgaste/corrupción y poca capacidad para histórico y backups locales. Valorar SSD (NVMe o USB) con coste justificado.
- **R-02 · IP de la Pi sin reserva DHCP** — un cambio de IP rompería accesos, la tablet y las integraciones que apunten a ella.
- **R-03 · Regletas del salón alimentan la cámara y el altavoz** — un apagado (manual o automatización) deja sin cámara.
- **R-04 · Fingerbot accionando un calefactor** — electrodoméstico potencialmente peligroso; requiere salvaguardas (límite de tiempo, condiciones, confirmación).
- **R-05 · Dependencias de nube** — tado (API con cuota diaria desde 2026), Nest (API de Google), Tuya, Midea, eWeLink.
- **R-06 · Dongle Zigbee en USB 3 de la Pi 5** — posibles interferencias; valorar alargador USB.
- **R-07 · Sin SSH configurado** — se resolverá con el complemento oficial de terminal/SSH cuando haga falta.
- **R-08 · Roomba posiblemente de la gama x05** — no soportada por la integración oficial; confirmar modelo.
- **R-09 · Dispositivos Wi-Fi que hoy no funcionan** — sensores de agua (crítico: detección de fugas) y otros.

## Cambios aprobados, pendientes de ejecutar (tras el primer backup)

- [x] D-009 · Retirar integración Roborock (2026-10-20)
- [x] D-010 · Área «Estudio» (2026-10-20)

## Acciones previas a cualquier cambio (Fase 5, adelantables)

- [ ] Grabar capturas del «antes» (`09_contenido/capturas_pendientes.md`)
- [x] Primer backup completo + copia fuera de la Pi (descarga al PC) + clave de cifrado custodiada (2026-10-20)
- [x] Reserva DHCP para la Pi en el router (2026-10-19) · R-02 resuelto
- [ ] Usuario de HA dedicado para Claude (no admin salvo que sea imprescindible) → con el MCP Server (D-007 capa 3), tras el túnel. Hoy el navegador pide aprobación por acción en direcciones privadas y no se puede ampliar

## Aplazado por decisión de Aida (2026-10-20)

- [ ] **Copia externa en Google Drive** (D-029): aplazada para priorizar tener HA funcionando. Mientras tanto: backups automáticos **solo locales** + copia manual descargada al PC. ⚠️ Riesgo aceptado temporalmente: sin copia automática fuera de la Pi. Retomar antes de la Fase 8.

## Hallazgos 2026-10-20 (registro de áreas)

- ~~Áreas no coincidentes~~ → resuelto con D-030.
- Existen 4 automaciones `aviso_humedad_*` (sala, estudio, hab. Aroa, hab. darlings) que notifican a `notify.mobile_app_maida`; no estaban en la auditoría inicial. Revisar en Fase 8.
- 24 dispositivos sin área.
- Entidades no disponibles: `device_tracker.aida`, `aida_2`, `mike`, `poco_f7_ultra` (rastreadores antiguos/duplicados) → en análisis con Aida. `media_player.sala` (Nest Mini): **depende de un enchufe inteligente**; si está apagado, el altavoz no está disponible (comportamiento esperado, ver R-03).
- Pendiente de F5-05: activar **MFA** (aún desactivado). Roborock y UPnP retirados.
- Personas en HA: `person.villarusategi` (rastreador: app móvil MAida), `person.aida` (rastreador tado `aida`), `person.mike` (rastreador tado `mike`). Rastreadores tado extra: `aida_2`, `poco_f7_ultra`. Solo hay 2 móviles reales (uno por persona). Reorganizar en Fase 6.
- ~~Usuarios~~ → resuelto con D-031.
- **Hallazgo 2026-10-20 · presencia**: tras recargar tado, *todos* sus rastreadores (`mike`, `mvaida`) siguen como `restored` = la integración no les envía datos (H: la integración/cuota de tado ya no los alimenta). Propuesta: presencia con la app de HA en los dos móviles.
- La app de HA «MAida» está en un Xiaomi 23078PND5G; el móvil actual de Aida es un POCO F7 Ultra → H: la app está en el móvil viejo. Confirmar.
- ✅ (resuelto 2026-10-20) La app registrada «MAida» era el **móvil viejo** (registro 2025-12-24, batería al 1 %, sin datos). Las 4 automatizaciones `aviso_humedad_*` notifican a `notify.mobile_app_maida` → **los avisos de humedad se están perdiendo**. Acción: iniciar sesión en la app del POCO (nuevo dispositivo), cambiar el destino de las 4 automatizaciones y borrar «MAida».
- Persona «Mike» → renombrar a «Mikel» y vincular a su usuario cuando se cree.
