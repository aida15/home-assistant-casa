# Registro de cambios

Cambios aplicados a la instalación real (no a la documentación). Formato: fecha · fase · cambio · estado · reversión.

| Fecha | Fase | Cambio | Estado | Reversión |
|---|---|---|---|---|
| 2026-10-19 | 5 | Reserva DHCP de la Pi en el router (misma IP que tenía) · `03_instalacion/F5-02_reserva_dhcp.md` | Probado | Borrar la reserva en el router |
| 2026-10-20 | 5 | Backups automáticos L-X-V-D de madrugada, 7 copias, cifrados, solo en la Pi (Drive aplazado). Kit de emergencia guardado. Backup manual de 6,72 MB descargado al PC | Implementado · restauración pendiente de probar (paso SSD) | Desactivar copias automáticas en Ajustes → Sistema → Copias de seguridad |
| 2026-10-20 | 5 | D-010: área `estudio` creada (Claude, vía navegador con sesión de Aida); movidos a ella la zona tado «Zona gaming», el tado Internet Bridge, el dispositivo «Router» (UPnP) y `automation.aviso_humedad_estudio`; área `zona_gaming` borrada | Probado (registro de áreas verificado) | Crear área «Zona gaming» y devolver los 3 dispositivos y la automatización |
| 2026-10-20 | 5 | Aida: actualiza Core 2026.9.3 → **2026.10.2**, Supervisor → 2026.10.2, OS 16.3 → **18.3** (verificado por Claude) | Probado | Restaurar el backup previo a la actualización |
| 2026-10-20 | 5 | D-030 (Claude, vía navegador): áreas `habitacion_darlings` y `bano_darlings` creadas sustituyendo a `dormitorio_darlings` y `bano_habitacion` (movidos: zona tado «Dormitorio darlings» y `automation.aviso_humedad_habitacion_darling`); nuevas `cuarto_de_limpieza`, `jardin`, `terraza`, `garaje`. Comprobado antes que ninguna automatización usaba IDs de área | Probado | Recrear las áreas antiguas y devolver dispositivo y automatización |
| 2026-10-20 | 5 | D-009 y D-023: integraciones **Roborock** y **UPnP/IGD** eliminadas (Claude, vía navegador). Reparaciones queda sin avisos | Probado | Volver a añadir las integraciones |
| 2026-10-20 | 5 | Presencia (Claude, vía navegador, tras limpieza de Aida en la app tado): integración tado recargada; `device_tracker.poco_f7_ultra` → `device_tracker.mvaida`; eliminadas entidades huérfanas `device_tracker.aida` y `aida_2`; `person.aida` pasa a usar `device_tracker.mvaida` | Implementado · los rastreadores tado siguen sin datos (`restored`) | Renombrar de vuelta y reasignar en Ajustes → Personas |
