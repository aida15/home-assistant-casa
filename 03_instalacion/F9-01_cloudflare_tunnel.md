# F9-01 · Acceso remoto con Cloudflare Tunnel (D-020)

| Campo | Valor |
|---|---|
| Estado | **Implementado y probado** (2026-10-22): acceso desde PC y móviles; regla de país activa |
| Objetivo | HA accesible desde fuera por HTTPS en un subdominio propio, sin abrir puertos |
| Piezas | App comunitaria **Cloudflared** (repo `https://github.com/homeassistant-apps/repository`, v7.0.18) · túnel gestionado desde el panel de Cloudflare (token) · `http:` con proxy de confianza e `ip_ban` |
| Datos sensibles | Token del túnel y dominio: **solo en Cloudflare y en la app**, nunca en Git ni en el chat |

## Pasos
1. (Claude) `configuration.yaml` → `http:` con `use_x_forwarded_for`, `trusted_proxies: 172.30.33.0/24`, `ip_ban_enabled`, `login_attempts_threshold: 5`. Requiere reinicio.
2. (Claude) Repositorio de apps añadido e instalada **Cloudflared** (parada).
3. (Aida) Cloudflare → Zero Trust → Networks → Tunnels → crear túnel *Cloudflared* «ha-casa» → copiar el **token** → *Public hostname*: subdominio + dominio, servicio **HTTP** `homeassistant:8123`.
4. (Aida) App Cloudflared → Configuración → `tunnel_token` → Guardar → Iniciar.
5. (Claude) Comprobar túnel, reinicio, URL externa en HA.
6. (Aida) Regla WAF: bloquear países distintos de España. Bot Fight Mode **desactivado** (rompe la app móvil).
7. Prueba desde el móvil con datos (Wi-Fi apagado): login + MFA.

## Recuperación
- Si te bloquea `ip_ban` (5 intentos fallidos): Terminal → `rm /config/ip_bans.yaml` y reiniciar.
- Cortar el acceso remoto al instante: parar la app Cloudflared o borrar el túnel en Cloudflare.
