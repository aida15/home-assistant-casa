# F5-03 · Cuenta de Google y credenciales para backups en Drive

| Campo | Valor |
|---|---|
| Fase | 5 · Preparación |
| Estado | **Aplazado** (2026-10-20) · pendiente de verificación de marca (D-029) |
| Objetivo | Tener Client ID y Client Secret OAuth para la integración **Google Drive** de HA (D-016, D-017) |
| Problema que resuelve | Hoy el único backup está en la propia microSD: si la tarjeta muere, se pierde todo |
| Requisitos | Cuenta de Google **dedicada** (D-016) · KeePass abierto |
| Coste | 0 € (Google Cloud no cobra por esto; no hace falta tarjeta) |
| Dificultad | Media (muchas pantallas, ninguna difícil) |
| Riesgo | Nulo para HA: en este paso no se toca la instalación |
| Fuente | https://www.home-assistant.io/integrations/google_drive/ (consultada 2026-10-19) |

## Procedimiento

### A · Cuenta dedicada
1. Crear una cuenta de Google nueva solo para la casa (p. ej. «casa…@gmail.com»).
2. Contraseña generada en **KeePass** + verificación en 2 pasos activada.

### B · Proyecto en Google Cloud (con la cuenta dedicada)
1. `console.cloud.google.com` → crear proyecto (nombre libre, p. ej. `ha-casa`).
2. **APIs y servicios → Biblioteca → Google Drive API → Habilitar.**
3. **Google Auth Platform → Branding**: nombre de la app (p. ej. `HA Casa`), correo de soporte = la cuenta dedicada.
   - Audiencia: **External**. Correo de contacto. Aceptar la política.
   - Página de inicio, privacidad y términos: `https://home-assistant.io`. Dominio autorizado: `home-assistant.io`.
4. **Audience → Publishing status → Publicar la app** («In production»).
   - ⚠️ Si se queda en *Testing*, las credenciales caducan cada 7 días y los backups dejan de subirse sin avisar.
5. **Clients → Crear cliente** → tipo **Web application**.
   - URI de redirección autorizada, exacta: `https://my.home-assistant.io/redirect/oauth`
6. Copiar **Client ID** y **Client Secret** a **KeePass** (el secret no se puede volver a ver al cerrar el diálogo).
7. Puede tardar unos minutos (hasta 5 h según Google) en activarse.

> 🎓 `my.home-assistant.io` solo hace de «redirector»: devuelve el navegador a tu HA local. No necesita Nabu Casa ni abrir puertos.

## Validación
- [ ] Cuenta dedicada con 2FA.
- [ ] Google Drive API habilitada.
- [ ] App en **In production** (no Testing).
- [ ] Cliente Web con la URI de redirección exacta.
- [ ] Client ID y Secret guardados en KeePass.

## Reversión
Borrar el cliente OAuth o el proyecto en Google Cloud. HA no se ve afectado.

## Seguridad
- **Nunca** compartir el Client Secret (ni conmigo, ni en Git, ni en capturas).
- La integración solo puede ver los archivos que ella misma crea en la carpeta `Home Assistant` de Drive.

## Contenido
- Apto para tutorial **con difuminado**: la pantalla del cliente OAuth muestra el Client ID/Secret. Mejor grabar solo los menús y la pantalla de «Publish app».

## Resultado real
_(pendiente)_

## Incidencia 2026-10-20 · Google exige logo → verificación de marca

- La consola no deja publicar sin logo; con logo pide **verificación de marca**: página principal y política de privacidad accesibles, en un dominio **verificado a nombre de la cuenta**.
- `home-assistant.io` no sirve (no es nuestro). Errores recibidos: página no accesible / no registrada a tu nombre / política sin contenido suficiente.
- Decisión D-029: mantener Google Drive y verificar la marca con el **dominio propio** (descartados OneDrive y modo Testing).

### Procedimiento B (sustituye a los enlaces del paso B.3)
1. **Web mínima** en la raíz del dominio con `web_google/` (index + privacidad + logo). Sustituir `CORREO_CONTACTO` por el correo de la cuenta dedicada antes de subir. Alojamiento: Cloudflare Pages (subida directa), dominio personalizado = raíz del dominio.
2. **Search Console** (con la cuenta dedicada) → propiedad de tipo *Dominio* → registro TXT en el DNS de Cloudflare → Verificar.
3. **Branding**: logo, página principal `https://<dominio>/`, privacidad `https://<dominio>/privacidad.html`, dominio autorizado `<dominio>`.
4. Esperar 24 h tras verificar el dominio → **Publicar** → enviar a verificación.
5. **Mientras Google revisa**: app en Testing con la cuenta dedicada como *test user* para tener backups ya; reautorizar en HA cada 7 días hasta la aprobación.

### Riesgo asumido
La web pública asocia el dominio a «un sistema domótico». Mitigación: el subdominio del túnel no se publica en la web, MFA, geobloqueo e `ip_ban` (D-020).
