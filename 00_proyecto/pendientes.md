# Pendientes

## Fase 1 · Descubrimiento (datos que faltan)

- [x] microSD: SanDisk 32 GB clase 10
- [x] Zigbee: LinknLink ZG-808Z + Sonoff ZBBridge-P
- [x] Roomba: Roomba Plus 405 Combo (no soportado por la integración oficial)
- [x] ZBMINI: 4 → luz techo dorm. Aroa, salón ×2, cocina
- [ ] ZBMINI: variante exacta y presencia de neutro en las cajas
- [ ] Sensores Wi-Fi sin funcionar: **despriorizados por decisión de Aida** (2026-10-19); se retoman en Fase 6
- [ ] Modelos restantes (ver `01_inventario/dispositivos.md`)
- [ ] Bloque C · Vivienda y distribución
- [ ] Bloque D · Preferencias de diseño
- [ ] Bloque E · Uso cotidiano
- [ ] Bloque F · Seguridad, privacidad y mantenimiento
- [ ] Resumen de requisitos validado por Aida → cierre de Fase 1

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

## Acciones previas a cualquier cambio (Fase 5, adelantables)

- [ ] Grabar capturas del «antes» (`09_contenido/capturas_pendientes.md`)
- [ ] Primer backup completo + copia fuera de la Pi + clave de cifrado custodiada
- [ ] Reserva DHCP para la Pi en el router
- [ ] Usuario de HA dedicado para Claude (no admin salvo que sea imprescindible)
