# Arquitectura · Fase 3

Se entrega por bloques; cada bloque queda **propuesto** hasta que Aida lo aprueba.

| Bloque | Tema | Estado |
|---|---|---|
| 1 | Backups y migración a SSD | ✅ **Aprobado** (2026-10-19, con cambio de frecuencia) |
| 2 | Zigbee: ZHA vs Zigbee2MQTT | ✅ **Aprobado** (2026-10-19): ZHA |
| 3 | Acceso remoto con dominio propio | ✅ **Aprobado** (2026-10-19): Cloudflare Tunnel |
| 4 | Estructura de configuración (YAML + Git) y acceso de Claude | ✅ **Aprobado** (2026-10-19) |
| 5 | Usuarios y permisos | ✅ **Aprobado** (2026-10-19) |
| 6 | Decisiones de integración (Tuya, tado nube) | ✅ **Aprobado** (2026-10-19) |
| 7 | Análisis de fallos | **Propuesto** |

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

- [x] Opción A: Cloudflare Tunnel (D-020)
- [x] Dominio elegido por Aida. **No se escribe en el repo**: en la configuración irá en `secrets.yaml` y en la documentación como `<dominio>`

### Fuentes (consultadas 2026-10-19)

- Cloudflared para HA: https://github.com/homeassistant-apps/app-cloudflared
- Tailscale + HA: https://www.tailscale.com/blog/remotely-access-home-assistant
- Cloudflare Access con la app (problema abierto): https://community.simon42.com/t/cloudflare-tunnel-cloudflare-access-fuer-home-assistant-sichere-app-nutzung-ohne-30-tage-reauth/76857
- Local Push: https://companion.home-assistant.io/docs/notifications/notification-local/
- MCP Server (requiere acceso público para clientes en la nube): https://home-assistant.io/integrations/mcp_server/
- Google Assistant manual (requiere HTTPS externo): https://www.home-assistant.io/integrations/google_assistant/

---

## Bloque 4 · Estructura de configuración, Git y forma de trabajar

### 4.1 Dos repositorios privados

| Repo | Contenido | Quién escribe |
|---|---|---|
| `home-assistant-casa` (ya existe) | Documentación, decisiones, inventario, diseño, contenido, curso | Claude (y Aida) |
| `home-assistant-config` (**nuevo**) | La configuración YAML de HA: su raíz es la carpeta `/config` | Claude propone, HA la descarga |

**Por qué separados:** la herramienta de sincronización (app oficial «Git pull») espera que el repo **sea** la carpeta de configuración ✅. Mezclar documentación, imágenes del curso y YAML en `/config` ensucia la instalación.

### 4.2 Qué se versiona y qué no

| Se versiona | NO se versiona |
|---|---|
| `configuration.yaml` | `secrets.yaml` (contraseñas, dominio, claves) |
| `packages/` (lógica por función) | `.storage/` (integraciones, dispositivos, usuarios, registro de entidades): lo cubren los backups |
| `dashboards/` (YAML) | Base de datos del historial y logs |
| `themes/` | Backups |
| `www/plano/` (SVG del plano real; repo privado) | `automations.yaml` / `scripts.yaml` de la UI (borradores, ver 4.4) |
| `blueprints/` | |

Se usará un `.gitignore` en modo **lista blanca** (se ignora todo salvo lo que se nombra) para que nunca se cuele un secreto por accidente.

**Git ≠ backup:** Git guarda la historia del YAML; el backup guarda el sistema entero (incluido `.storage`). Se necesitan los dos.

### 4.3 Estructura de carpetas propuesta

```
/config
├── configuration.yaml        # mínimo: carga packages, dashboards, temas, http/proxy
├── secrets.yaml              # NO versionado
├── packages/                 # cada archivo = un tema funcional
│   ├── sistema/              # backups, notificaciones, salud del sistema
│   ├── presencia/            # quién está en casa, modo casa
│   ├── calefaccion/          # tado
│   ├── iluminacion/          # luces y escenas
│   └── ...                   # se crean solo cuando haya contenido
├── dashboards/
│   ├── principal.yaml        # tablet / general
│   ├── movil.yaml
│   └── componentes/          # plantillas de tarjetas reutilizables
├── themes/
│   └── casa.yaml             # neutros + terracota, claro/oscuro
└── www/plano/                # SVG del plano
```

- **Por función, no por habitación:** una automatización como «salir de casa» toca todas las estancias; las áreas ya viven en el registro de HA.
- Los nombres de archivo deben ser únicos aunque estén en subcarpetas ✅.
- Separar en `packages/` lo **genérico** (reutilizable para el producto) de lo **específico de esta casa** se hará con convenciones de nombres, sin abstracciones complejas (punto 17.7).

### 4.4 Reglas de convivencia YAML ↔ UI (desarrolla D-001)

- Lo que vive en YAML **solo se cambia vía Git**. Si se edita en la propia HA, el siguiente `pull` puede chocar.
- Las automatizaciones creadas en la UI se consideran **borradores de prueba**. Si se quedan, se pasan a un package.
- Integraciones, dispositivos, áreas y usuarios: **siempre en la UI** (no admiten YAML o no conviene).

### 4.5 Flujo de un cambio

1. Claude escribe el cambio en `home-assistant-config` con un commit descriptivo (y la ficha en `home-assistant-casa`).
2. Aida pulsa **Git pull** en HA (más adelante, automatizable o vía MCP).
3. HA: *Herramientas para desarrolladores → Comprobar configuración*.
4. Recargar lo afectado (o reiniciar si hace falta).
5. Probar según la ficha → resultado al registro de cambios.
6. **Si falla:** `git revert` del commit + Git pull → vuelve el estado anterior. Si algo peor falla: backup.

App Git pull configurada en modo **`pull`** (no `reset`, que machaca cambios locales ✅) y **sin reinicio automático** al principio, para controlar cada cambio.

### 4.6 Apps (complementos) necesarias en HA

| App | Para qué | Tipo |
|---|---|---|
| **Terminal & SSH** | Crear el repo inicial desde `/config`, diagnóstico (resuelve R-07) | Oficial |
| **Git pull** | Bajar los cambios del repo | Oficial ✅ |
| **cloudflared** | Túnel del Bloque 3 | Comunitaria |

Nada más por ahora: el editor de archivos no hace falta si los cambios entran por Git.

### 4.7 Acceso de Claude, por etapas (desarrolla D-007)

| Etapa | Acceso | Cuándo |
|---|---|---|
| 1 | Escritura en los dos repos (sin tocar HA directamente) | Fase 5 |
| 2 | Lectura de HA desde el navegador del PC vinculado (con tu aprobación por acción) | Ya disponible |
| 3 | MCP Server de HA con **usuario dedicado no administrador** y solo entidades expuestas | Tras el túnel (Fase 5-6) |

La clave de despliegue del repo (deploy key) la genera Aida en HA y la pega en GitHub: **Claude no ve ni maneja claves**.

### Decisiones que pide este bloque

- [x] Dos repos: `home-assistant-config` creado (privado, vacío) y conectado a la sesión de Claude
- [x] Estructura por función y reglas YAML ↔ UI
- [x] Flujo de cambio con Git pull manual
- Nota: el repo de configuración **se rellena por primera vez desde HA** en la Fase 5 (se sube la configuración real), no desde aquí, para no pisar `/config`.

### Fuentes (consultadas 2026-10-19)

- Git pull: https://github.com/home-assistant/addons/blob/master/git_pull/DOCS.md
- Packages: https://www.home-assistant.io/docs/configuration/packages/

---

## Bloque 5 · Usuarios y permisos

### 5.1 Cuentas

| Cuenta | Tipo | Acceso desde fuera | MFA | Persona en HA (presencia) | Uso |
|---|---|---|---|---|---|
| `aida` | Propietaria / **única administradora** (D-014) | Sí | **TOTP obligatorio** | Sí (móvil Android) | Administración y uso diario |
| `mikel` | Usuario normal | Sí | **TOTP obligatorio** | Sí (iPhone) | Uso diario |
| `tablet` | Usuario normal | **No: «solo red local»** ✅ | No (es un panel fijo; el bloqueo de red local hace de barrera) | **No** (no representa a nadie) | Panel de la entrada |
| `claude` (futuro) | Usuario normal, **no admin** | Solo vía MCP por el túnel | — (OAuth) | No | MCP con entidades expuestas; se puede revocar en cualquier momento |

Las cuentas de usuario normal no ven Ajustes ni pueden cambiar la configuración.

### 5.2 Lo que conviene saber (autenticación ≠ permisos ≠ visibilidad)

| Capa | Qué hace | Qué NO hace |
|---|---|---|
| **Autenticación** (contraseña + MFA, «solo red local») | Decide **quién entra y desde dónde** | — |
| **Administrador vs usuario** | Un usuario normal no puede tocar la configuración | **No limita qué dispositivos puede encender o apagar**: HA no tiene permisos por entidad 🔍 |
| **Visibilidad del dashboard** | Ocultar tarjetas o vistas a ciertos usuarios | **No es seguridad**: ocultar una tarjeta no impide la acción por otras vías |

**Consecuencia de diseño:** la protección de lo sensible (calefactor del Fingerbot, cámaras) se pone en la **lógica** (límites de tiempo, condiciones, confirmaciones) y en la **red** (túnel, MFA, solo red local), no en esconder botones.

**Lección relacionada 🎓:** en 2026 se publicó una vulnerabilidad en un editor de archivos comunitario para HA que permitía a cualquier usuario no administrador ejecutar acciones de administrador ✅. Por eso este proyecto evita apps y extensiones que no sean necesarias.

### 5.3 Recuperación de acceso

| Riesgo | Medida |
|---|---|
| Aida pierde el móvil con el TOTP | Guardar el **secreto TOTP en KeePass** (admite TOTP) al activarlo → se puede regenerar el código en otro dispositivo |
| Aida olvida la contraseña | Contraseña en KeePass. Último recurso: restablecerla desde la consola de la Pi (hay teclado y pantalla) 🔍 |
| La tablet queda desatendida | Usuario sin admin, sin cámaras en portada, sin acceso desde fuera |

### 5.4 Revisión de lo que ya existe (Fase 5)

- Comprobar qué cuentas hay hoy y si la app móvil ya vinculada es la de Aida.
- Activar MFA en la cuenta de Aida **antes** de abrir el túnel.

### Decisiones que pide este bloque

- [x] Cuentas aprobadas (D-022)
- [x] MFA obligatorio para Aida y Mikel. Deseo de Aida: no tener que meter MFA en casa con los móviles habituales → se cumple sin excepciones de red, porque **el MFA solo se pide al iniciar sesión** y la app mantiene la sesión abierta. Se descarta eximir la red local (`trusted_networks`): detrás del túnel, el tráfico de Internet entra por un proceso local y un error de configuración daría acceso sin contraseña desde fuera 🔍 🎓

### Fuentes (consultadas 2026-10-19)

- MFA (TOTP): https://www.home-assistant.io/docs/authentication/multi-factor-auth/
- «Solo red local»: https://www.nabucasa.com/config/troubleshooting/user_restricted_to_local_access
- Vulnerabilidad de un editor comunitario (CVE-2026-53453): https://app.opencve.io/cve/CVE-2026-53453
- Sistema de permisos (blog de desarrolladores, 2019; a confirmar vigencia): https://developers.home-assistant.io/blog/2019/03/11/user-permissions.html

---

## Bloque 6 · Decisiones de integración

| Grupo | Decisión propuesta | Motivo | Revisión |
|---|---|---|---|
| Tuya (3 regletas, enchufe, ventilador) | **Integración oficial (nube)** para todo al principio | Alta en 1 minuto con la app Smart Life; sin claves locales que se rompen al re-emparejar | Si las regletas del salón (TV/sofá, uso diario) fallan o van lentas → pasarlas a **tuya-local** (HACS), una a una |
| tado V3 | **HomeKit = vía principal** (control y lecturas, local). **Nube = temporal** | La nube aporta hoy la presencia de la app de tado y el modo ausencia | Antes de finales de marzo 2027: con la presencia de HA funcionando, decidir no renovar Auto-Assist y **retirar la integración de nube** (sin suscripción, el sondeo cada 5 min supera las 100 peticiones/día ✅) |
| Nest Cam ×2 | **Integración oficial** (pago único 5 USD) | Única vía; solo directo | Si Google cambia la API |
| LG webOS | **Añadir** (descubierta) | Local | — |
| Google Cast | **Mantener** | Local; avisos de voz | — |
| Midea | **HACS, prioridad baja** | Uso ocasional; integración comunitaria | Fase 6, al final |
| Roomba 405 | **Sin integración** | No hay vía viable | Revisar el desarrollo comunitario cada pocos meses |
| Roborock, UPnP/IGD | **Retirar** | Sin uso | Fase 5, tras el backup |
| Sensores Wi-Fi, Fingerbot | Despriorizados | Decisión de Aida | Fase 6 |

### Sobre HACS (tienda comunitaria)

HACS (Home Assistant Community Store) es el canal para instalar **extensiones hechas por la comunidad**: integraciones, tarjetas, temas. Es gratuito; su «coste» es de riesgo: lo que instala **no lo revisa ni lo prueba el equipo de HA** ✅ y puede romperse con una actualización o quedar abandonado.

**Novedad clave:** a partir de **HA 2026.11** viene **integrado de serie como «Marketplace»** ✅ (ya fusionado en desarrollo a principios de octubre de 2026). No hay que instalar nada aparte, no requiere cuenta de GitHub para el catálogo y obliga a aceptar un aviso antes de instalar contenido comunitario ✅.

**Decisión:** **no instalar HACS a mano**. Esperar a 2026.11 y usar el Marketplace integrado, solo cuando haya un primer uso justificado (Fase 4 o 6). Cada extensión pasará la ficha del punto 5 de los requisitos (problema, ventajas, dependencias, riesgos, mantenimiento, plan B).

Fuentes: https://hacs.xyz/ · https://www.matteralpha.com/news/home-assistant-embraces-hacs-into-core-as-the-marketplace

### Decisión que pide este bloque

- [x] Decisiones de integración aprobadas (D-023)

---

## Bloque 7 · Análisis de fallos

Principio rector: **si HA se cae, la casa sigue funcionando como una casa normal** (interruptores, termostatos y apps de fabricante). Tolerancia declarada: una tarde caída = molestia (Fase 1).

### 7.1 Escenarios

| Escenario | Deja de funcionar | Sigue funcionando | Cómo se detecta | Recuperación | Prevención |
|---|---|---|---|---|---|
| **Sin Internet** | Acceso remoto, cámaras Nest, Tuya (nube), voz de Google, backups a Drive, notificaciones al móvil fuera de casa | Todo lo local: luces Zigbee, tado vía HomeKit, TV LG, dashboards en casa | Sensor de conectividad en HA; aviso en el dashboard | Automática al volver Internet; los backups pendientes se reintentan ✅ | Lo esencial, en local (Bloque 6) |
| **Reinicio del router** | Todo durante 1-3 min | — | Igual que arriba | Automática; la **reserva DHCP** mantiene la IP de la Pi | Reserva DHCP (Fase 5) |
| **HA caído o reiniciando** | Automatizaciones, dashboards, presencia | **Interruptores de pared** (los ZBMINIL2 conmutan en local), tado con su propia programación, apps de fabricante, **Google Home** | Desde dentro no se puede avisar → monitor externo gratuito que vigile la URL pública 🔍 (opcional) | Reinicio; si no arranca, restaurar backup | **No borrar los dispositivos de Google Home**: es el plan B de control |
| **Fallo de la Pi o del SSD** | Todo HA | Igual que «HA caído» | HA no responde | Instalar HAOS en otro soporte y **restaurar el último backup** (procedimiento probado en la migración a SSD); objetivo ~1-2 h 🔍 | Backups cada 2 días + Drive; microSD de repuesto |
| **Dongle Zigbee desconectado** | Control de las 4 luces desde HA | Interruptores de pared | Dispositivos Zigbee «no disponibles»; aviso | Reconectar; si el dongle muere, ZHA migra a otro coordinador desde su backup ✅ | Alargador USB fijo, sin tirones |
| **Un dispositivo sin conexión** | Ese dispositivo en HA | El resto | Entidad «no disponible» > X min → aviso (Fase 8) | Revisar alimentación / Wi-Fi | Sin notificaciones por cortes breves (umbral de tiempo) |
| **Caída de la nube de un fabricante** (Tuya, Google, tado) | Los dispositivos de ese fabricante vía nube | tado vía HomeKit; todo lo local | Entidades «no disponibles» | Esperar; control manual o app del fabricante | Migrar a local lo que más se usa si se repite |
| **Una actualización rompe una integración** | Esa integración | El resto | Error tras actualizar | **Backup previo automático** ✅ → restaurar o volver a la versión anterior | Leer notas de versión; no actualizar todo a la vez; esperar unos días a la versión `.1` |
| **Tablet sin batería o sin conexión** | El panel | Móviles, interruptores | Visual | Cargar; la app de panel recarga sola al volver la red (Fase 4) | Tablet siempre enchufada; plan de batería en Fase 4 |
| **Apagón de la casa** | Todo | — | — | Al volver la luz, la Pi arranca sola; Zigbee y Wi-Fi se reconectan | Definir el estado de las luces tras un corte (ZBMINIL2: «apagado» o «último estado») 🔍 |

### 7.2 Consecuencias de diseño

1. **Google Home se mantiene** como canal de control de reserva para lo que ya controla hoy.
2. **Ninguna automatización sustituye a un interruptor físico.**
3. Los avisos de fallo distinguen cortes breves de caídas reales (umbral de tiempo), para no saturar.
4. Monitor externo de disponibilidad: **opcional**, se decide en la Fase 9.

### Decisión que pide este bloque

- [ ] Aprobar el análisis y sus consecuencias de diseño → **cierre de la Fase 3**

---

## Informe de fase (punto 16.12)

**A. Progreso técnico** — Implementado: nada (fase de diseño). Probado: nada nuevo. Pendiente: prueba de la tablet con IP manual.

**B. Documentación** — Arquitectura completa en 7 bloques; decisiones D-017 a D-023. Repo `home-assistant-config` creado (vacío hasta la Fase 5).

**C. Contenido**
- «Por qué mi casa sigue funcionando si se cae Home Assistant» (tabla 7.1 → carrusel; solo cuando esté **probado**).
- «Cloudflare Tunnel vs Tailscale: cuál elegí y por qué» (decisión real con contexto de presencia).
- «El error de seguridad que casi cometo con el túnel» (`trusted_networks`).
- «HACS ya viene de serie»: oportuno en noviembre de 2026, tras actualizar a 2026.11.

**D. Producto**
- Lecciones candidatas: «Diseñar para el fallo», «Acceso remoto seguro sin abrir puertos», «Git para tu configuración de HA».
- Recurso candidato: **matriz de fallos rellenable** (escenario, qué cae, qué sigue, detección, recuperación).
