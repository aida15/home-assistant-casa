# Registro de cambios

Cambios aplicados a la instalación real (no a la documentación). Formato: fecha · fase · cambio · estado · reversión.

| Fecha | Fase | Cambio | Estado | Reversión |
|---|---|---|---|---|
| 2026-10-19 | 5 | Reserva DHCP de la Pi en el router (misma IP que tenía) · `03_instalacion/F5-02_reserva_dhcp.md` | Probado | Borrar la reserva en el router |
| 2026-10-20 | 5 | Backups automáticos L-X-V-D de madrugada, 7 copias, cifrados, solo en la Pi (Drive aplazado). Kit de emergencia guardado. Backup manual de 6,72 MB descargado al PC | Implementado · restauración pendiente de probar (paso SSD) | Desactivar copias automáticas en Ajustes → Sistema → Copias de seguridad |
