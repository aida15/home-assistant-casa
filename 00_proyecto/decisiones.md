# Registro de decisiones

Formato: ID · fecha · decisión · motivo · alternativas descartadas. 🎓 = interés didáctico.

| ID | Fecha | Decisión | Motivo | Alternativas descartadas |
|---|---|---|---|---|
| D-001 | 2026-10-19 | Lógica y dashboards en **YAML**; integraciones y ajustes internos en la **UI** | YAML es versionable, documentable y reutilizable para el producto; las integraciones las gestiona HA internamente | Todo en UI (poco trazable); todo en YAML (no soportado por muchas integraciones) 🎓 |
| D-002 | 2026-10-19 | Documentación en **repo privado de GitHub** (`aida15/home-assistant-casa`) | Historial, reversión y base del curso | Notion, Google Drive |
| D-003 | 2026-10-19 | **Plano real privado**; versión alterada/anonimizada para redes | Publicar distribución + ubicación de cámaras es un riesgo de seguridad (prioridad 1 > 8) | Publicar el plano real 🎓 |
| D-004 | 2026-10-19 | Nombres de entidades en **castellano sin tildes ni ñ** | Consistencia y compatibilidad de IDs | Inglés |
| D-005 | 2026-10-19 | Contenido y curso **solo en castellano**; en cámara aparece **solo Aida** | Decisión de la propietaria | Bilingüe |
| D-006 | 2026-10-19 | `custom:button-card` queda como **candidata a evaluar** en Fase 4, no instalada | Primero UX, después herramientas | — |
| D-007 | 2026-10-19 | Acceso de Claude por capas: (1) repo GitHub, (2) PC vinculado vía app de escritorio, (3) MCP Server de HA solo tras definir acceso remoto seguro. Siempre con **backup previo** y **usuario de HA dedicado y revocable** | Acceso rápido sin abrir la red; mínimo privilegio | Exponer HA a Internet ya 🎓 |
| D-009 | 2026-10-19 | Retirar la integración **Roborock** (aspirador antiguo, ya no existe) | Evitar entidades huérfanas y dependencias de nube inútiles | Mantenerla |
| D-010 | 2026-10-19 | El área «Zona gaming» pasa a llamarse **«Estudio»** (`estudio`) en todos los dispositivos y zonas | Nombre real de la estancia | — |
| D-008 | 2026-10-19 | Notificaciones en 3 prioridades (crítica / importante / informativa); resúmenes y mantenimiento como tipos dentro de informativa | Reconcilia puntos 17.4 y 18.4 | 5 niveles planos |
| D-011 | 2026-10-19 | Paleta: **neutros cálidos + acento terracota** | Preferencia de Aida: neutros con un «punch» de color; terracota asocia «encendido» de forma intuitiva | Salvia, menta |
| D-012 | 2026-10-19 | **Tema claro de día y oscuro automático de noche** | Evitar deslumbrar en la entrada de noche | Claro fijo, oscuro fijo |
| D-013 | 2026-10-19 | Acceso remoto **sin Nabu Casa**, con **dominio propio** (mecanismo concreto a definir en Fase 3, sin abrir puertos) | Preferencia de Aida; coste recurrente evitable | Nabu Casa |
| D-014 | 2026-10-19 | **Una sola cuenta administradora** (Aida); resto de usuarios solo consumen dashboards. Sin invitados ni mantenimiento | Mínimo privilegio | Varios admins |
| D-015 | 2026-10-19 | Cámaras: **solo directo**, sin grabación; **fuera de la portada**, en vista secundaria | Privacidad; uso puntual (comprobar encuadre) | Cámaras en portada |
| D-016 | 2026-10-19 | Backups con copia externa en **cuenta de Google Drive dedicada**; migrar la Pi a un **SSD reaprovechado** | Coste cero, separa la copia del almacenamiento que se protege | NAS (no hay) |
| D-017 | 2026-10-19 | Backups automáticos completos **L-X-V-D** de madrugada + antes de cada actualización; **7 copias**; local + Google Drive; **cifrado en todo**; kit de emergencia en **KeePass** + copia offline | Aida considera excesiva la diaria; 7 copias ≈ 2 semanas | Diaria |
| D-018 | 2026-10-19 | Migración a SSD por **instalación limpia + restauración de backup**, que sirve como primera prueba de restauración; microSD original guardada 2 semanas | Prueba real del backup sin riesgo | Clonar la tarjeta; disco de datos externo con SD |
| D-019 | 2026-10-19 | Pila Zigbee: **ZHA**, con el ZG-808Z como único coordinador (USB 2.0 + alargador; SSD en USB 3). Plan B: Zigbee2MQTT | Nativo primero, menos piezas, red Zigbee incluida en el backup de HA | Zigbee2MQTT 🎓 |
| D-020 | 2026-10-19 | Acceso remoto por **Cloudflare Tunnel** con dominio propio (no documentado en el repo; en `secrets.yaml`) + MFA, tablet solo red local, geobloqueo a España, `ip_ban`, sin Cloudflare Access | Presencia continua sin VPN, deja abiertos MCP y «Ok Google», sin abrir puertos | Tailscale, puerto 443, Nabu Casa 🎓 |
| D-021 | 2026-10-19 | **Dos repos privados**: `home-assistant-casa` (docs) y `home-assistant-config` (raíz = `/config`); YAML por función en `packages/`; `.gitignore` en lista blanca; sincronización con la app **Git pull** en modo `pull`, sin reinicio automático | La app Git pull espera que el repo sea la configuración; separar docs de config | Un único repo 🎓 |
| D-022 | 2026-10-19 | Cuentas: `aida` (única admin, MFA), `mikel` (usuario, MFA), `tablet` (usuario, solo red local, sin MFA), `claude` futura (usuario no admin, MCP). Secreto TOTP y contraseñas en KeePass. Sin exención de MFA por red local | MFA solo se pide al iniciar sesión; `trusted_networks` es arriesgado detrás del túnel | Eximir red local 🎓 |
