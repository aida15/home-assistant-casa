# F6-01 · Coordinador Zigbee con ZHA

| Campo | Valor |
|---|---|
| Fase | 6 · Integración |
| Estado | **Diseñado** |
| Objetivo | Red Zigbee propia en HA con el LinknLink ZG-808Z (D-019) |
| Requisitos | Alargador USB (0,5-1 m) · puerto **USB 2.0 (negro)** de la Pi · ZBBridge-P de Sonoff **desenchufado** |
| Riesgo | Bajo; no afecta a nada existente |

## Procedimiento
1. Conectar el dongle con el alargador a un puerto **negro (USB 2.0)**, lejos de la Pi, del router y de puertos USB 3.
2. HA lo detecta: Ajustes → Dispositivos e integraciones → **Descubierto: ZHA** (si no, Añadir integración → ZHA → elegir el puerto `/dev/serial/by-id/…`).
3. Tipo de radio: autodetección (**ZNP**, Texas Instruments). Opción **Crear una red nueva**; canal automático.

## Validación
- [ ] Integración ZHA cargada; dispositivo coordinador visible.
- [ ] Configuración de red de ZHA muestra canal y PAN ID.
- [ ] Siguiente backup incluye la red Zigbee.

## Reversión
Eliminar la integración ZHA y desenchufar el dongle.

## Notas
- Los ZBMINIL2 **no son routers** (sin neutro): la cobertura dependerá de que el coordinador llegue a cada caja. Si falla alguno, un enchufe Zigbee hará de repetidor.
- Instalación eléctrica de los ZBMINIL2: con el cuadro cortado o por un electricista (prioridad 1, seguridad).

## Resultado real
_(pendiente)_
