# F5-02 · Reserva DHCP para la Raspberry Pi

| Campo | Valor |
|---|---|
| Fase | 5 · Preparación |
| Estado | **Implementado y probado** (2026-10-19) |
| Objetivo | Que la Pi tenga siempre la misma IP en la red local |
| Problema que resuelve | R-02: si el router cambia la IP, se rompen la tablet, los accesos guardados y las integraciones que apunten a la Pi |
| Situación inicial | IP dinámica, sin reserva |
| Hardware / software | Router Sagemcom F@st 5670 (operadora) · Raspberry Pi 5 con HAOS |
| Coste | 0 € |
| Dificultad | Baja |
| Riesgo | Bajo y reversible |
| Conocimientos previos | Entrar en la web del router |

## Por qué reserva DHCP y no IP fija en la Pi

- La **reserva** se hace en el router: la Pi sigue pidiendo IP como siempre, pero el router le da **siempre la misma**.
- Configurar una IP fija dentro de HA también funciona, pero si un día cambias de router o de rango, la Pi se queda sin red. Con la reserva, el control queda en un solo sitio (el router). 🎓

## Procedimiento

1. **Averiguar la IP actual de la Pi y la del router**
   - IP de la Pi: HA → Ajustes → Sistema → Red.
   - IP del router: en el PC, `Win + R` → `cmd` → `ipconfig` → «Puerta de enlace predeterminada» (suele ser `192.168.1.1`).
2. **Entrar en el router**: abrir esa IP en el navegador e iniciar sesión con la contraseña de administración (pegatina del router o la que hayas cambiado). *Este paso lo hace Aida: Claude no introduce contraseñas.*
3. **Buscar la sección** (el nombre varía según la operadora): «Red local», «LAN», «DHCP», «Reserva de direcciones», «IP estática», «Static lease» o «Dispositivos conectados».
4. **Localizar la Pi** en la lista de dispositivos: nombre `homeassistant` (o la IP del paso 1). Anotar su **MAC** por si hay que crear la reserva a mano.
5. **Crear la reserva** con la **IP que ya tiene ahora** (así nada cambia). Guardar.
6. Si el router lo pide, **aplicar/reiniciar** solo el servicio DHCP; no hace falta reiniciar la Pi.

## Validación

- [ ] En el router, la Pi aparece como «reservada» / «estática» con su IP.
- [ ] HA → Ajustes → Sistema → Red muestra la misma IP.
- [ ] (Opcional, más adelante) Tras el próximo reinicio de la Pi, sigue con la misma IP.

## Reversión

Borrar la reserva en la misma pantalla del router. La Pi vuelve a IP dinámica sin perder nada.

## Precauciones

- No cambies ningún otro ajuste del router en este paso (wifi, DNS, cortafuegos, UPnP).
- **No grabar** la pantalla del router: muestra IPs, MACs y nombres de dispositivos.

## Resultado real

- 2026-10-19 · Aida crea la reserva en el router con la IP que ya tenía la Pi. Sin incidencias.
- Comprobación pendiente (no bloquea): confirmar que la IP se mantiene tras el próximo reinicio de la Pi (será en el paso de migración al SSD).

## Contenido

- No apto para vídeo (datos de red). Como mucho, una frase en el reel de preparación: «Lo primero: que la Pi tenga siempre la misma dirección».
