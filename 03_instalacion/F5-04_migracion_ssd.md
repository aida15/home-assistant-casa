# F5-04 · Migración de microSD a SSD (instalación limpia + restauración)

| Campo | Valor |
|---|---|
| Fase | 5 · Preparación |
| Estado | **Diseñado** → pendiente de ejecutar por Aida |
| Objetivo | HA arrancando desde el SSD USB con toda la configuración restaurada (D-018) |
| Problemas que resuelve | R-01 (desgaste y capacidad de la microSD) + **primera prueba real de restauración** del backup |
| Requisitos | Backup del 2026-10-20 (6,72 MB) en el PC · kit de emergencia · SSD en caja USB · Raspberry Pi Imager en el PC · ~45 min |
| Riesgo | Medio, **totalmente reversible**: la microSD no se toca |
| Destructivo | **Sí, para el contenido del SSD** (se borra entero) |

## Procedimiento

### A · Grabar HAOS en el SSD (en el PC)
1. Conectar el SSD al PC. Abrir **Raspberry Pi Imager**.
2. Dispositivo: **Raspberry Pi 5**. SO: *Other specific-purpose OS → Home assistants and home automation → Home Assistant* → HAOS para RPi 5.
3. Almacenamiento: **el SSD**. ⚠️ Comprobar nombre y tamaño: elegir otro disco lo borra.
4. Si pregunta por personalizar ajustes: **No**. Escribir y esperar a la verificación.

### B · Cambiar de disco
1. HA → Ajustes → Sistema → ⋮ → **Apagar el sistema**. Esperar a que se apague el LED verde de actividad y desenchufar.
2. **Sacar la microSD** y guardarla etiquetada («HA original · no borrar hasta el 2026-11-03»).
3. SSD a un puerto **USB 3 (azul)**. Enchufar la Pi.
4. Primer arranque: **10-20 min** sin tocar nada.

### C · Restaurar
1. Abrir `http://<IP de la Pi>:8123` (misma IP gracias a la reserva DHCP: la MAC de la Pi no cambia).
2. En la bienvenida: **Restaurar desde una copia de seguridad** → subir el `.tar` → clave del kit de emergencia → restaurar todo.
3. Esperar a que reinicie (puede tardar varios minutos) e iniciar sesión con la cuenta de siempre.

## Validación
- [ ] Ajustes → Sistema → Hardware/Almacenamiento: disco del tamaño del SSD (no 32 GB).
- [ ] La IP es la misma (cierra la comprobación pendiente de F5-02).
- [ ] Integraciones como antes: HomeKit (tado) responde, Cast, Met.no.
- [ ] Copias automáticas siguen configuradas (L-X-V-D, 7 copias).
- [ ] Dejar 24 h funcionando sin reinicios inesperados.

## Reversión
Apagar → desconectar el SSD → meter la microSD → encender. Vuelve exactamente al estado anterior.

## Si no arranca desde el SSD
- La Pi 5 arranca desde USB de serie (**H**: no comprobado en esta unidad). Si no lo hace: Imager → *Misc utility images → Bootloader (Pi 5) → USB Boot* en la **microSD de repuesto** → arrancar la Pi con ella hasta que el LED parpadee verde de forma continua → apagar, quitarla y repetir B.3.

## Contenido
Apto para reel («la primera vez que restauro un backup»): grabar el cambio de disco y la pantalla de restauración. No grabar la IP ni la barra de direcciones.

## Resultado real
_(pendiente)_
