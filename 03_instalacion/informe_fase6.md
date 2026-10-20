# Informe de cierre · Fase 6 (2026-10-20)

## 1 · Hecho
- ZHA con ZG-808Z (canal 20, Z-Stack 20250321).
- Tuya oficial: regletas sala 1 y 2, lámpara de pie sala, lámpara mesita Mikel, tira LED (como luces).
- LG webOS: TV sala. Reserva DHCP de la TV hecha por Aida.
- App de HA: móviles de Aida y Mikel (presencia `home` verificada en ambos) y tablet (usuario `tablet`, solo red local).

## 2 · Probado
- Lámpara de pie: encender/apagar desde HA. Lectura de estado de TV, regletas y consumo. Notificaciones a ambos móviles. Presencia de ambos.

## 3 · Pendiente (se mantiene abierto)
- Re-emparejar en Smart Life: sensores de agua (cocina), regleta de la cocina, 3.er enchufe.
- ZBMINIL2: instalación eléctrica (electricista o diferencial bajado).
- Midea (Marketplace 2026.11) · Nest (aparcado: aporta poco y requiere verificación de Google).
- Encendido de la TV por Wake on LAN (Fase 8).
- Limpieza de entidades de tado (duplicados nube/HomeKit, nombres antiguos `zona_gaming`, `dormitorio_darlings`) → inicio de Fase 7.

## 4 · Riesgos
- Regletas sin «estado al encender» → cámara/altavoz pueden quedar apagados tras un corte (mitigar en Fase 8).
- Dependencia de nube Tuya para todo lo de Smart Life.
