# Auditoría · Fase 2

Fecha: 2026-10-19 · Estado: **pendiente de validación por Aida**
Base: inventario de Fase 1, revisión en solo lectura de HA y documentación consultada ese día (fuentes al final).
Convención: ✅ verificado en documentación · 🔍 hipótesis que requiere prueba real.

## 1. Diagnóstico general

| Área | Estado | Comentario |
|---|---|---|
| Hardware de cómputo | 🟢 | Pi 5 8 GB sobrada para esta casa |
| Versión de HA | 🟡 | Core 2026.9.3; ya existe 2026.10 ✅. Actualizar **después** del primer backup y de la migración a SSD |
| Almacenamiento | 🔴 | microSD 32 GB clase 10: riesgo de corrupción y poca capacidad |
| Backups | 🔴 | Sin automáticas; única copia en la misma microSD |
| Red | 🟡 | IP dinámica; UPnP activo en el router (HA lo detecta) 🔍 |
| Zigbee | 🟢 | Coordinador CC2652P disponible, sin usar |
| Calefacción | 🟢 | tado ya en local vía HomeKit |
| Wi-Fi Tuya/eWeLink | 🟡 | Dependientes de nube salvo vía local (con matices) |
| Acceso remoto | ⚪ | No existe (correcto por ahora) |
| Tablet | 🟡 | Antigua; compatibilidad real por probar |

## 2. Matriz de integraciones

| Dispositivo | Integración propuesta | Tipo | Local/Nube | Requisitos | Riesgo | Veredicto |
|---|---|---|---|---|---|---|
| tado V3 (4 SRT + 1 ST) | **HomeKit Device** (ya activa) | Oficial | **Local** ✅ | Ninguno | Bajo | **Usar como vía principal** |
| tado V3 (presencia, modo ausencia) | tado (ya activa) | Oficial, API no oficial de tado | Nube ✅ | Cuota: **100 peticiones/día sin suscripción, 20.000 con Auto-Assist** ✅ | Medio: sin suscripción (desde abril) la cuota es muy justa | Decidir en Fase 3 si se mantiene; si HA asume presencia, se puede retirar |
| 4 × ZBMINI (luces de techo) | Zigbee (ZHA o Z2M) | Oficial / app | **Local** | **ZBMINI requiere neutro**; ZBMINI-L no ✅ | Bajo | Usar. Confirmar variante y neutro |
| Regletas GHome ×3, enchufe Smart Life, ventilador Create | Opción A: **Tuya oficial** | Oficial | Nube ✅ | Login con QR de la app Smart Life ✅ | Medio: nube; no expone todas las funciones ✅ | Recomendada para empezar |
| ídem | Opción B: **tuya-local** | HACS | Local ✅ | Login asistido con cuenta Smart Life para obtener claves ✅ | Medio: la clave cambia si se re-empareja; 1 conexión local por dispositivo; ritmo de comandos ✅ | Valorar dispositivo a dispositivo si la nube molesta |
| TV LG UK6470PLC | **LG webOS TV** (descubierta) | Oficial | Local | Aceptar en la TV | Bajo | Usar |
| Google Home + Nest Mini | **Google Cast** (ya activa) | Oficial | Local | — | Bajo | Usar para avisos de voz y multimedia |
| Nest Cam ×2 | **Google Nest** | Oficial | Nube ✅ | Proyecto Google Cloud + Device Access (**5 USD únicos**) + Pub/Sub; cuenta de Google personal (no Workspace) ✅. No exige acceso remoto ✅ | Medio: complejidad de alta; dependencia de Google | Usar (solo directo, WebRTC) |
| Deshumidificador Midea | Midea Air Appliances LAN | HACS | Local tras 1 conexión a la nube ✅ | Cuenta Midea para obtener token/clave ✅ | Medio: comunitaria; algunos modelos nuevos son Tuya ✅ | Prioridad baja; confirmar modelo |
| Roomba Plus 405 Combo | — | — | — | — | — | ❌ **Sin integración viable hoy** (gama «Prime/V4», solo nube con otra autenticación; hay un desarrollo comunitario en curso) ✅ |
| Sensores Wi-Fi (agua, lluvia), Fingerbot | Por determinar | — | — | Recuperar primero | — | Despriorizados |
| Roborock | — | — | — | — | — | Retirar (D-009) |
| UPnP/IGD | — | — | — | — | — | Retirar 🔍 (no aporta; revisar UPnP del router) |

## 3. Servicios de soporte

| Necesidad | Solución | Tipo | Requisitos | Notas |
|---|---|---|---|---|
| Backup externo | **Integración Google Drive** (destino de backups nativo) ✅ | Oficial | Proyecto Google Cloud con OAuth **publicado** (en modo prueba caduca a los 7 días) ✅ | Solo accede a su propia carpeta ✅. Cifrado: lo aporta el sistema de backups de HA 🔍 (verificar en Fase 5). El mismo proyecto de Google Cloud sirve también para Nest ✅ |
| Acceso remoto con dominio propio | **Cloudflared** (Cloudflare Tunnel) | Comunitaria (repositorio no oficial, mantenido en 2026) ✅ | **Dominio con DNS gestionado en Cloudflare** ✅; aarch64 soportado ✅ | Sin abrir puertos ✅. HA queda accesible desde Internet a través del túnel → MFA obligatorio + capa de acceso adicional (Fase 3) |
| «Ok Google» sin Nabu Casa | Google Assistant manual | Oficial | URL HTTPS externa (el túnel lo cubre), proyecto en Google Home Developer Console, cuenta de servicio ✅ | El proyecto queda en modo prueba: la sincronización puede dejar de funcionar a los ~30 días y hay que regenerarlo ✅ → **mantenimiento recurrente**. Recomendación: posponer |
| Companion App en tablet | App oficial Android | Oficial | **Android 6+** para la versión completa ✅ | La tablet (Android 7 de serie) cumple el mínimo de la app, pero el **navegador interno (WebView) puede estar congelado en una versión antigua** y no renderizar bien el frontend actual 🔍 → **prueba real temprana** |

## 4. Riesgos actualizados

| ID | Riesgo | Prob. | Impacto | Mitigación | Cuándo |
|---|---|---|---|---|---|
| R-01 | Corrupción de microSD | Alta | Alto | Migrar a SSD por USB (backup → instalar HAOS en SSD → restaurar) | Fase 5 |
| R-11 | Sin backup fuera de la Pi | — (es un hecho) | Alto | Backups automáticos + Google Drive + prueba de restauración | Fase 5, **antes de nada más** |
| R-02 | Cambio de IP de la Pi | Media | Medio | Reserva DHCP en el router | Fase 5 |
| R-10 | Tablet incapaz de mostrar el frontend actual | Media 🔍 | Medio | Probar ya; plan B: otra tablet barata o navegador alternativo | Fase 2-3 (prueba) |
| R-05 | Dependencias de nube (tado nube, Tuya, Nest, Midea) | — | Medio | Local donde sea viable; documentar qué deja de funcionar sin Internet | Fase 3 |
| R-12 | tado sin suscripción desde abril: 100 peticiones/día | Alta si no se renueva | Medio | Usar HomeKit; reducir o retirar la integración de nube | Antes de finales de marzo 2027 |
| R-03 | Regletas del salón cortan cámara y altavoz | Media | Medio | Cámara/altavoz en una toma no conmutada o automatizaciones con exclusiones | Fase 6 |
| R-04 | Fingerbot sobre calefactor | Baja | Alto | No automatizar sin límite de tiempo, condiciones y aviso | Fase 8 |
| R-06 | Interferencias USB 3 con el dongle Zigbee | Media | Medio | Alargador USB 2.0 de ~0,5-1 m, lejos del SSD | Fase 5 |
| R-08 | Roomba sin integración | — (es un hecho) | Bajo | Mantener app iRobot; revisar el desarrollo comunitario más adelante | Seguimiento |
| R-09 | Sensores de agua inoperativos | — | Medio (fugas) | Despriorizado por decisión de Aida | Fase 6 |
| R-13 | UPnP activo en el router 🔍 | Media | Medio | Valorar desactivarlo (puede afectar a consolas/apps) | Fase 3 |
| R-14 | Duplicidad tado HomeKit + nube | — (es un hecho) | Bajo | Elegir vía y limpiar entidades tras revisar dependencias | Fase 6 |

## 5. Limitaciones asumidas

- Sin persianas motorizadas hasta tener instalación eléctrica.
- Sin medición de consumo eléctrico → no habrá panel de energía real hasta añadir medidores.
- Sin sensores de presencia/movimiento → la presencia dependerá de los móviles (Companion App) 🔍.
- La Pi y el router son puntos únicos de fallo; aceptable según la tolerancia declarada (una tarde caída = molestia).

## 6. Requisitos adicionales detectados

| Requisito | Coste | Estado |
|---|---|---|
| SSD reaprovechado en caja USB | 0 € | Disponible |
| Alargador USB para el dongle Zigbee | Bajo (pocos euros, 🔍 sin precio verificado) | A comprar |
| Dominio propio con DNS en Cloudflare | Coste anual del dominio (🔍 según registrador) | A comprar en Fase 3 |
| Proyecto Google Cloud (Drive + Nest) | 0 € | A crear en Fase 5 |
| Device Access de Google (Nest) | 5 USD únicos ✅ | A pagar en Fase 6 |
| Neutro en cajas de interruptores (si los ZBMINI son la versión con neutro) | — | A comprobar |
| Cuenta de Google dedicada a backups | 0 € | A crear |

## 7. Funcionamiento sin Internet (preliminar, punto 17.2)

| Función | Sin Internet | Motivo |
|---|---|---|
| Calefacción tado (lectura/ajuste) | ✅ Sigue | HomeKit local |
| Luces de techo (ZBMINI) | ✅ Sigue | Zigbee local + interruptor físico |
| TV LG, avisos por altavoz | ✅ Sigue 🔍 | Integraciones locales |
| Regletas y enchufes Tuya | ❌ Si se usa Tuya oficial · ✅ con tuya-local | Nube vs local |
| Cámaras Nest | ❌ | Nube de Google |
| Acceso remoto, notificaciones al móvil | ❌ | Requieren Internet |
| Voz «Ok Google» | ❌ | Nube de Google |
| Backups a Google Drive | ❌ (se reintentan) | Nube; la copia local sigue |

## 8. Decisiones que pasan a Fase 3

1. ZHA vs Zigbee2MQTT.
2. Tuya: oficial (nube) o tuya-local, por dispositivo.
3. Integración tado de nube: mantener, reducir o retirar.
4. Diseño del acceso remoto (Cloudflare Tunnel + MFA + capa adicional).
5. «Ok Google»: posponer (recomendado) o montar.
6. Política de backups: frecuencia, retención, cifrado, prueba de restauración.
7. Orden de la migración a SSD.

---

## Informe de fase (punto 16.12)

**A. Progreso técnico** — Implementado: nada. Probado: nada nuevo. Pendiente: validación de esta auditoría y prueba del frontend en la tablet.

**B. Documentación** — Creado: esta auditoría. Falta: variante de los ZBMINI y neutro, modelo del deshumidificador, resultado de la prueba de la tablet.

**C. Contenido**
- «Tu Roomba nuevo puede no funcionar con Home Assistant»: caso real del 405 (útil antes de comprar).
- «Lo que funciona en tu casa si se cae Internet»: tabla de la sección 7 convertida en carrusel, cuando esté verificada.
- Grabación: prueba de la tablet antigua con HA (resultado real, salga bien o mal).

**D. Producto**
- Lección candidata: «Auditoría: local vs nube y matriz de riesgos».
- Recurso candidato: **plantilla de matriz de integraciones** (dispositivo, vía, local/nube, requisitos, riesgo, veredicto).
- Diferenciador: decisiones justificadas con fuentes y fecha, no listas de «las 10 mejores integraciones».

---

## Fuentes consultadas (2026-10-19)

- tado: https://www.home-assistant.io/integrations/tado/ · https://help.tado.com/en/articles/12165739-limitation-for-rest-api-usage
- Roomba: https://www.home-assistant.io/integrations/roomba/ · https://community.home-assistant.io/t/irobot-roomba-integration-cant-add-roomba-plus-405/909770
- Tuya: https://www.home-assistant.io/integrations/tuya/ · https://github.com/make-all/tuya-local
- Midea: https://github.com/nbogojevic/homeassistant-midea-air-appliances-lan
- Nest: https://www.home-assistant.io/integrations/nest/
- Google Drive: https://www.home-assistant.io/integrations/google_drive/
- Google Assistant: https://www.home-assistant.io/integrations/google_assistant/
- Cloudflared: https://github.com/homeassistant-apps/app-cloudflared
- Companion App Android: https://www.home-assistant.io/blog/2025/07/23/companion-app-for-android/ · https://community.home-assistant.io/t/android-7-1-1-android-nougat-with-home-assistant-companion-app/997422
- ZBMINI-L sin neutro: https://www.cnx-software.com/2022/01/25/sonoff-zbmini-l-zigbee-3-0-smart-switch-works-without-neutral-wire/
- ZG-808Z: https://www.linknlink.com/products/linknlink-zg-808z-zigbee-3-0-usb-dongle-home-assistant
- ZBBridge-P: https://zigbee.blakadder.com/Sonoff_ZBBridge-P.html
- HA 2026.10: https://www.home-assistant.io/changelogs/core-2026.10/
