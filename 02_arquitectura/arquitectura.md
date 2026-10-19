# Arquitectura · Fase 3

Se entrega por bloques; cada bloque queda **propuesto** hasta que Aida lo aprueba.

| Bloque | Tema | Estado |
|---|---|---|
| 1 | Backups y migración a SSD | ✅ **Aprobado** (2026-10-19, con cambio de frecuencia) |
| 2 | Zigbee: ZHA vs Zigbee2MQTT | ✅ **Aprobado** (2026-10-19): ZHA |
| 3 | Acceso remoto con dominio propio | **Propuesto** |
| 4 | Estructura de configuración (YAML + Git) y acceso de Claude | Pendiente |
| 5 | Usuarios y permisos | Pendiente |
| 6 | Decisiones de integración (Tuya, tado nube) | Pendiente |
| 7 | Análisis de fallos | Pendiente |

---

## Bloque 1 · Backups y migración a SSD

**Por qué primero:** R-01 (microSD) y R-11 (sin copia externa) son los dos riesgos rojos, y la regla del proyecto es no tocar nada sin plan de recuperación.

### 1.1 Política de backups

| Parámetro | Propuesta | Motivo |
|---|---|---|
| Tipo | Backup automático **completo** del sistema de HA ✅ | Incluye configuración, historial y complementos instalados |
| Frecuencia | **Cada dos días aprox.: lunes, miércoles, viernes y domingo**, de madrugada (p. ej. 04:00) | Decisión de Aida (la diaria le parece excesiva). HA permite elegir días concretos ✅. Coste: se pueden perder hasta 2 días de cambios → backup manual antes de cualquier cambio grande |
| Antes de actualizar | **Sí, siempre** (opción por defecto activada) ✅ | Vuelta atrás inmediata si una actualización rompe algo |
| Retención | **7 copias** (≈ 2 semanas con 4 copias/semana) | Margen para detectar un problema y volver atrás. El tamaño hoy es mínimo (3,3 MB) |
| Ubicaciones | **Local (SSD) + Google Drive** (cuenta dedicada) ✅ | Copia fuera del dispositivo, recomendada por la documentación ✅ |
| Cifrado | **Activado en todas las ubicaciones** (por defecto) ✅ | Drive es un tercero; una sola clave para todo es más simple |
| Clave de cifrado | **Kit de emergencia** guardado en **KeePass** + una copia offline (impresa o USB) | Sin la clave, el backup no sirve ✅. **Nunca** en el repo ni en la misma cuenta de Drive |
| Medios excluidos | Excluir `media` si crece | Recomendación oficial para reducir tamaño ✅ |
| Supervisión | Notificación si falla el backup automático (entidad `event.backup_automatic_backup`) ✅ | Se configura en la Fase 8 (automatizaciones) |
| Prueba de restauración | **La migración a SSD es la primera prueba real** (ver 1.2). Después, prueba trimestral documentada | Generada ≠ conservada ≠ restaurable |

### 1.2 Migración de microSD a SSD (restaurando un backup)

Método recomendado por la comunidad y alineado con la documentación oficial ✅: backup completo → instalación limpia de HAOS en el SSD → restaurar en el asistente de bienvenida.

**Ventaja clave:** al restaurar en el SSD estamos **probando de verdad que el backup funciona**. Si algo falla, la microSD sigue intacta.

**Ventaja de hacerlo ahora:** Zigbee todavía no está configurado, así que no hay red Zigbee que migrar.

Orden propuesto (se detallará paso a paso en la Fase 5):

1. Grabar el «antes» (capturas pendientes).
2. **Reserva DHCP** de la Pi en el router (la tarjeta de red es la misma, así que la IP se mantendrá tras la migración 🔍).
3. Crear la cuenta de Google dedicada y el proyecto de Google Cloud (OAuth **publicado**, no en pruebas) ✅.
4. Configurar backups automáticos + Google Drive + **descargar el kit de emergencia**.
5. Backup completo manual y **descarga al PC** (tercera copia).
6. Comprobar que el bootloader de la Pi 5 arranca desde USB 🔍 (puede requerir actualizarlo con Raspberry Pi Imager y una microSD de repuesto).
7. Instalar HAOS en el SSD, arrancar **sin la microSD** y restaurar el backup con la clave del kit ✅.
8. Verificar: login, integraciones (tado HomeKit, Google Cast…), historial.
9. Guardar la microSD original **sin borrar** al menos 2 semanas como vuelta atrás.
10. Después, y solo después: actualizar a HA 2026.10 (con backup previo automático), retirar Roborock (D-009) y renombrar «Estudio» (D-010).

### 1.3 Riesgos de este bloque

| Riesgo | Mitigación |
|---|---|
| Perder la clave de cifrado | Kit de emergencia en dos sitios distintos antes de borrar nada |
| La Pi no arranca desde USB | Paso 6; la microSD sigue disponible |
| Alimentación insuficiente del SSD por USB 🔍 | Fuente oficial de 27 W (ya disponible); si da problemas, caja con alimentación propia |
| Credenciales de Google en modo pruebas caducan a los 7 días ✅ | Publicar la app OAuth |
| Google Drive no disponible a la hora del backup | Se reintenta; la copia local se mantiene ✅ |

### 1.4 Decisiones que pide este bloque

- [x] Política aprobada con frecuencia L-X-V-D, 7 copias, cifrado en todo (D-017)
- [x] Gestor de contraseñas: KeePass
- [x] microSD de repuesto: disponible

### Fuentes (consultadas 2026-10-19)

- Backups en HA (frecuencia, retención, cifrado, restauración): https://www.home-assistant.io/common-tasks/general/
- Integración Backup: https://www.home-assistant.io/integrations/backup/
- Google Drive como destino: https://www.home-assistant.io/integrations/google_drive/
- Migración SD → SSD: https://community.home-assistant.io/t/best-guide-to-move-ha-to-ssd/985510

---

## Bloque 2 · Zigbee: ZHA vs Zigbee2MQTT

**Propuesta: ZHA** (integración nativa), con Zigbee2MQTT como plan B documentado.

### Comparativa

| Criterio | ZHA | Zigbee2MQTT |
|---|---|---|
| Qué es | Integración oficial dentro de HA | Complemento externo + broker MQTT (Mosquitto) |
| Piezas a mantener | 1 | 3 (Z2M, Mosquitto, integración MQTT) |
| Compatibilidad con el ZG-808Z (CC2652P) | ✅ Adaptador probado (tipo `znp`, firmware Z-Stack) | ✅ |
| ZBMINIL2 | ✅ Confirmado | ✅ Confirmado |
| Backups de la red Zigbee | ✅ Automáticos, dentro del backup de HA | Propios del complemento |
| Migrar a otro coordinador | ✅ Asistente integrado | Manual |
| Ajustes finos por dispositivo | Menos | Más (y soporte más rápido de dispositivos raros) |
| Para el curso | Más fácil de enseñar a principiantes | Más popular entre usuarios avanzados |

### Por qué ZHA

- Regla del proyecto: **nativo primero**. Con 4 relés y futuros cabezales no hay nada que exija Z2M.
- Menos piezas = menos que actualizar y menos que puede romperse.
- La red Zigbee entra en el backup normal de HA.
- **Plan B:** si un dispositivo futuro no está bien soportado en ZHA, se migra a Z2M. Implica volver a emparejar los dispositivos, asumible con una red pequeña.

### Requisitos y riesgos

| Punto | Detalle |
|---|---|
| Firmware del dongle | Debe ser Z-Stack **coordinador**. HA no actualiza el firmware de estos adaptadores ✅ → comprobar versión en la Fase 6 🔍 |
| Interferencias USB 3 | La documentación recomienda puerto USB 2.0 y alargador apantallado ✅. En la Pi 5: **SSD en un puerto USB 3 (azul), dongle en un USB 2 (negro) con alargador** |
| Malla Zigbee | Los ZBMINIL2 **no actúan como repetidores** ✅ → toda la red depende del coordinador. En 102 m² y una planta probablemente basta 🔍; si hay zonas sin cobertura, se añade un enchufe Zigbee (los enchufes con corriente sí repiten) |
| Thread | El ZG-808Z menciona Thread, pero se usará **solo para Zigbee** |
| ZBBridge-P | Sin papel en esta arquitectura; se puede reutilizar o vender |

### Decisión que pide este bloque

- [x] ZHA aprobado (D-019)

### Fuentes (consultadas 2026-10-19)

- ZHA: https://www.home-assistant.io/integrations/zha/
- ZBMINIL2: https://zigbee.blakadder.com/Sonoff_ZBMINIL2.html

---

## Bloque 3 · Acceso remoto

**Propuesta: Cloudflare Tunnel con dominio propio + endurecimiento**, sin abrir ningún puerto en el router.

### Qué necesita el acceso remoto en esta casa

| Necesidad | Exige que HA sea alcanzable desde fuera |
|---|---|
| Ver cámaras / revisar luces fuera de casa | Sí |
| **Presencia de los móviles** (base de las automatizaciones y de sustituir Auto-Assist de tado) | **Sí, de forma continua**: la app tiene que poder enviar su ubicación |
| Notificaciones al móvil | No necesariamente (las notificaciones estándar no dependen del acceso remoto 🔍; las «Local Push» funcionan en la red de casa ✅) |
| Acceso de Claude vía MCP (futuro) | Sí, por HTTPS público ✅ |
| «Ok Google» manual (pospuesto) | Sí, por HTTPS público ✅ |

### Opciones

| | A · Cloudflare Tunnel | B · Tailscale (Serve) | C · Abrir puerto 443 + proxy | D · Nabu Casa |
|---|---|---|---|---|
| Puertos abiertos | Ninguno ✅ | Ninguno ✅ | Sí ❌ | Ninguno |
| Dominio propio | Sí, con DNS en Cloudflare ✅ | No hace falta (nombre `*.ts.net`) ✅ | Sí | No |
| HA visible en Internet | **Sí** (detrás de Cloudflare) | **No**: solo dispositivos de tu red Tailscale ✅ | Sí, directo | Sí (detrás de Nabu Casa) |
| Móviles | App de HA normal | App de HA + app Tailscale **siempre activa** en cada móvil (también el de Mikel) | App normal | App normal |
| Presencia continua | ✅ Sin fricción | ✅ Si la VPN está siempre encendida | ✅ | ✅ |
| MCP para Claude / Google manual | ✅ | ❌ (no es público) | ✅ | ✅ |
| Coste | Dominio (anual) | 0 € (plan personal) ✅ | Dominio | Suscripción |
| Mantenimiento | App `cloudflared` (comunitaria, mantenida en 2026 ✅) | App Tailscale (mantenida por un desarrollador del núcleo de HA ✅) | Certificados, proxy, router | Mínimo |
| Veredicto | **Recomendada** | Alternativa sólida si prefieres no exponer nada | Descartada (abre puertos) | Descartada (D-013) |

### Por qué A

- Cumple tu preferencia de **dominio propio** y no abre puertos.
- La **presencia** funciona sin pedir a nadie que mantenga una VPN encendida.
- Deja la puerta abierta al acceso MCP para Claude y, si algún día lo queréis, a «Ok Google».
- El precio: la pantalla de login de HA queda accesible desde Internet → se compensa con las capas de abajo.

### Endurecimiento obligatorio

| Capa | Medida |
|---|---|
| Cuentas | **MFA (TOTP) obligatorio** para Aida y Mikel |
| Tablet | Usuario propio marcado **«solo red local»**: no puede iniciar sesión desde fuera 🔍 |
| Cloudflare | Regla de firewall: **solo tráfico desde España** + protección contra bots 🔍 (plan gratuito) |
| HA | Bloqueo de IP tras varios intentos de login fallidos (`ip_ban`) |
| Proxy | `trusted_proxies` configurado para que HA vea la IP real 🔍 |
| Dominio | Nombre que **no identifique la casa ni la familia**; subdominio para HA |
| Router | Desactivar UPnP si no rompe nada (R-13) |

**Descartado:** poner Cloudflare Access (login extra de Cloudflare) delante de HA. Rompe el uso diario de la app móvil: la comunidad no ha encontrado una solución estable 🔍.

### Requisitos

- Comprar un dominio y gestionar su DNS en Cloudflare (Cloudflare también vende dominios).
- Cuenta gratuita de Cloudflare.
- App `cloudflared` en HA.

### Decisiones que pide este bloque

- [ ] Opción A (Cloudflare Tunnel) o B (Tailscale)
- [ ] Si A: ¿dominio ya pensado? (sin relación con la casa ni la familia)

### Fuentes (consultadas 2026-10-19)

- Cloudflared para HA: https://github.com/homeassistant-apps/app-cloudflared
- Tailscale + HA: https://www.tailscale.com/blog/remotely-access-home-assistant
- Cloudflare Access con la app (problema abierto): https://community.simon42.com/t/cloudflare-tunnel-cloudflare-access-fuer-home-assistant-sichere-app-nutzung-ohne-30-tage-reauth/76857
- Local Push: https://companion.home-assistant.io/docs/notifications/notification-local/
- MCP Server (requiere acceso público para clientes en la nube): https://home-assistant.io/integrations/mcp_server/
- Google Assistant manual (requiere HTTPS externo): https://www.home-assistant.io/integrations/google_assistant/
