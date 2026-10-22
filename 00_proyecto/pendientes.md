# Pendientes

## Fase 1 · Descubrimiento (datos que faltan)

- [x] microSD: SanDisk 32 GB clase 10
- [x] Zigbee: LinknLink ZG-808Z + Sonoff ZBBridge-P
- [x] Roomba: Roomba Plus 405 Combo (no soportado por la integración oficial)
- [x] ZBMINI: 4 → luz techo dorm. Aroa, salón ×2, cocina
- [x] ZBMINI: ZBMINIL2 (sin neutro)
- [ ] Sensores Wi-Fi sin funcionar: **despriorizados por decisión de Aida** (2026-10-19); se retoman en Fase 6
- [ ] Modelos restantes (ver `01_inventario/dispositivos.md`)
- [x] Bloque C · Vivienda y distribución 
- [x] Bloque D · Preferencias de diseño (`04_dashboards/direccion_visual.md`)
- [x] Bloque E · Uso cotidiano (`00_proyecto/uso_cotidiano.md`)
- [x] Bloque F · Seguridad, privacidad y mantenimiento
- [x] SSD en desuso: montado en caja USB
- [x] Resumen de requisitos (`00_proyecto/resumen_requisitos_fase1.md`) validado por Aida → **Fase 1 cerrada (2026-10-19)**

## Fase 2 · Auditoría

- [x] Auditoría redactada (`02_arquitectura/auditoria_fase2.md`)
- [x] Prueba del frontend de HA en la tablet Huawei (R-10): **la app funciona** en Android 7 (2026-10-20)
- [x] Validación de la auditoría por Aida → **Fase 2 cerrada (2026-10-19)**

## Fechas clave

- 🗓️ **Finales de marzo 2027** — renovación de la suscripción tado Auto-Assist. Antes de esa fecha: decidir estrategia de calefacción (seguir con tado, sustituir funciones por HA o cambiar de cabezales).

## Riesgos detectados para la auditoría (Fase 2)

Hipótesis preliminares, a confirmar:

- **R-01 · microSD como almacenamiento** — SanDisk 32 GB clase 10, sin clasificación A1/A2 indicada. Riesgo alto de desgaste/corrupción y poca capacidad para histórico y backups locales. Valorar SSD (NVMe o USB) con coste justificado.
- **R-02 · IP de la Pi sin reserva DHCP** — un cambio de IP rompería accesos, la tablet y las integraciones que apunten a ella.
- **R-03 · Regletas del salón alimentan la cámara y el altavoz** — un apagado (manual o automatización) deja sin cámara.
- **R-04 · Fingerbot accionando un calefactor** — electrodoméstico potencialmente peligroso; requiere salvaguardas (límite de tiempo, condiciones, confirmación).
- **R-05 · Dependencias de nube** — tado (API con cuota diaria desde 2026), Nest (API de Google), Tuya, Midea, eWeLink.
- **R-06 · Dongle Zigbee en USB 3 de la Pi 5** — posibles interferencias; valorar alargador USB.
- **R-07 · Sin SSH configurado** — se resolverá con el complemento oficial de terminal/SSH cuando haga falta.
- **R-08 · Roomba posiblemente de la gama x05** — no soportada por la integración oficial; confirmar modelo.
- **R-09 · Dispositivos Wi-Fi que hoy no funcionan** — sensores de agua (crítico: detección de fugas) y otros.

## Cambios aprobados, pendientes de ejecutar (tras el primer backup)

- [x] D-009 · Retirar integración Roborock (2026-10-20)
- [x] D-010 · Área «Estudio» (2026-10-20)

## Acciones previas a cualquier cambio (Fase 5, adelantables)

- [ ] Grabar capturas del «antes» (`09_contenido/capturas_pendientes.md`)
- [x] Primer backup completo + copia fuera de la Pi (descarga al PC) + clave de cifrado custodiada (2026-10-20)
- [x] Reserva DHCP para la Pi en el router (2026-10-19) · R-02 resuelto
- [x] Usuario de HA dedicado para Claude (no admin salvo que sea imprescindible) — hecho 2026-10-22 (D-043) → con el MCP Server (D-007 capa 3), tras el túnel. Hoy el navegador pide aprobación por acción en direcciones privadas y no se puede ampliar

## Aplazado por decisión de Aida (2026-10-20)

- [ ] **Copia externa en Google Drive** (D-029): aplazada para priorizar tener HA funcionando. Mientras tanto: backups automáticos **solo locales** + copia manual descargada al PC. ⚠️ Riesgo aceptado temporalmente: sin copia automática fuera de la Pi. Retomar antes de la Fase 8.

## Hallazgos 2026-10-20 (registro de áreas)

- ~~Áreas no coincidentes~~ → resuelto con D-030.
- Existen 4 automaciones `aviso_humedad_*` (sala, estudio, hab. Aroa, hab. darlings) que notifican a `notify.mobile_app_maida`; no estaban en la auditoría inicial. Revisar en Fase 8.
- 24 dispositivos sin área.
- Entidades no disponibles: `device_tracker.aida`, `aida_2`, `mike`, `poco_f7_ultra` (rastreadores antiguos/duplicados) → en análisis con Aida. `media_player.sala` (Nest Mini): **depende de un enchufe inteligente**; si está apagado, el altavoz no está disponible (comportamiento esperado, ver R-03).
- Pendiente de F5-05: activar **MFA** (aún desactivado). Roborock y UPnP retirados.
- Personas en HA: `person.villarusategi` (rastreador: app móvil MAida), `person.aida` (rastreador tado `aida`), `person.mike` (rastreador tado `mike`). Rastreadores tado extra: `aida_2`, `poco_f7_ultra`. Solo hay 2 móviles reales (uno por persona). Reorganizar en Fase 6.
- ~~Usuarios~~ → resuelto con D-031.
- **Hallazgo 2026-10-20 · presencia**: tras recargar tado, *todos* sus rastreadores (`mike`, `mvaida`) siguen como `restored` = la integración no les envía datos (H: la integración/cuota de tado ya no los alimenta). Propuesta: presencia con la app de HA en los dos móviles.
- La app de HA «MAida» está en un Xiaomi 23078PND5G; el móvil actual de Aida es un POCO F7 Ultra → H: la app está en el móvil viejo. Confirmar.
- ✅ (resuelto 2026-10-20) La app registrada «MAida» era el **móvil viejo** (registro 2025-12-24, batería al 1 %, sin datos). Las 4 automatizaciones `aviso_humedad_*` notifican a `notify.mobile_app_maida` → **los avisos de humedad se están perdiendo**. Acción: iniciar sesión en la app del POCO (nuevo dispositivo), cambiar el destino de las 4 automatizaciones y borrar «MAida».
- Persona «Mike» → renombrar a «Mikel» y vincular a su usuario cuando se cree.

## Usuarios y presencia (2026-10-20)

- [x] Notificación de prueba recibida en `mvaida`.
- [ ] Ubicación: ni `mvaida` ni `iphone_de_mikel` han enviado aún coordenadas (sin `latitude`). Zona casa OK (radio 100 m). Revisar sensores de ubicación en las apps.
- [x] Mikel: usuario y app (2026-10-20). Pendiente: confirmar notificación y que activa su MFA. ~~Mikel: activar inicio de sesión en la persona «Mike» (renombrar a Mikel) → usuario `mikel`, no admin. Lo hace Aida (Claude no crea cuentas ni contraseñas). Después: app en su iPhone, MFA propio, ubicación «Siempre»; Claude asigna su rastreador y quita `tado_mike`.~~
- [ ] Tablet (D-032): requisitos anotados; la tablet no está operativa. Riesgo a vigilar: batería de una tablet de 2017 cargando 24/7 (posible hinchazón) → valorar ciclo de carga con enchufe inteligente.

## Tuya (2026-10-20)

- Causa de `unavailable`: **cambio de contraseña Wi-Fi** desde la configuración (dato de Aida). Acción: re-emparejar en Smart Life (2,4 GHz).
- Destino acordado tras re-emparejar:
  - Enchufe «Lampara salon» → lámpara de pie junto a la puerta de la terraza → área `sala`, mostrar como **luz** (`light.sala_lampara_pie`).
  - Enchufe «Mike» → lámpara de la mesita de noche de Mikel → área `habitacion_darlings`, mostrar como **luz** (`light.habitacion_darlings_lampara_mikel`).
  - Sensores de agua (lavadora y fregadero) → ambos en `cocina` (`binary_sensor.cocina_fuga_lavadora`, `binary_sensor.cocina_fuga_fregadero`). Aviso de fuga = prioridad crítica (D-008), en Fase 8.
- 3.er enchufe sin configurar: se añadirá cuando tenga uso.
- [ ] Tablet: crear usuario `tablet` (Aida), volver a iniciar sesión en la tablet con él y borrar el registro hecho con la cuenta de habitante.
- [ ] Midea (deshumidificador): no está en Smart Life/Tuya; integración comunitaria → esperar al Marketplace de HA 2026.11 (D-023).

## Fase 7 (2026-10-20)
- [x] (2026-10-20, datos de Aida) ~~Confirmar en la app de tado qué válvula (HomeKit `VA…`/`RU…`) está en cada habitación. Hipótesis por temperaturas: RU4020065024 sala (termostato) · VA2873572352 sala (válvula) · VA3376888832 hab. darlings · VA2252618752 estudio · VA1497840640 hab. Aroa. Después: clima del panel con HomeKit (local) en lugar de la nube.~~ Pendiente: configurar el 4.º cabezal en la sala y confirmar que es `cabezal_4` (VA2873572352).
- [ ] Posiciones en el plano de TV, tira LED y lámpara de Mikel: aproximadas, a confirmar por Aida.
- [x] Flujo Git (D-021) + tema terracota + modo noche (2026-10-20). ~~panel a YAML; plano noche~~ (hechos). Pendiente: quitar escritura a la deploy key; tarjeta de TV (la media-control se ve gris con el tema oscuro).
- [ ] Tablet: panel predeterminado `/panel-casa`, modo quiosco (D-032).
- [x] Avisos de humedad corregidos (2026-10-20). Pendiente decidir si se unifica el umbral (40 % vs 50 %).
- [ ] Tablet: aviso «La autoridad de certificación no es de confianza» al abrir la app (no bloquea). El frontend no carga nada externo (comprobado en `/panel-casa` y `/home`). H: la app intenta una URL `https://` guardada en su configuración. **Aparcado por Aida (2026-10-20)**; revisar con el modo quiosco.
- [x] Móviles: «Conexión insegura bloqueada» resuelto añadiendo la Wi-Fi de casa en la app (2026-10-20).

## To-do (2026-10-21)
- [x] **Ring Intercom** integrado (2026-10-21). Pendiente: Reglas propuestas: abrir siempre con confirmación, nunca por automatización ni voz; decidir si se integra la apertura (HA no permite restringir entidades por usuario).
- [ ] **V2C Trydan** (garaje): integración oficial local por IP. **Bloqueado: sin cobertura Wi-Fi en el garaje.** Opciones: punto de acceso/mesh en el garaje o cable (si el cargador lo admite; verificar modelo).
- [ ] ⚠️ **R-15 · Carga de la regleta de la cocina**: lavadora + freidora de aire + cafetera en una misma regleta. Comprobar la potencia máxima de la regleta (etiqueta) frente a la suma de los tres; no cortar la lavadora en marcha desde HA.

## Rutinas (2026-10-21)
- [x] MAC de la TV y encendido por red probado (2026-10-21). Pendiente: Aida prueba «Cenar» y «Cine» completos.
- [ ] Probar en real: volumen 20:00, volumen 7:30/9:00, despertar 6:45.
- [ ] Cuando funcionen en HA: **desactivar las rutinas equivalentes en Google Home** (si no, se ejecutan dos veces).
- [ ] Voz: decidir sustituto de «Ok Google, vamos a cenar» (D-036).
- [ ] Festivos: las rutinas de días laborables no distinguen festivos (opción: integración Workday).
- [ ] Propuesta de seguridad: apagado automático de la cafetera a las X h (pendiente de decisión).
- [ ] Probar el modo Vacaciones una noche real (o adelantando las ventanas) antes del primer viaje.
- [ ] Vacaciones · futuro: luces de techo (ZBMINIL2) y persianas (bajar de noche / subir por la mañana a hora aleatoria).

## Calefacción y presencia (2026-10-21)
- [ ] Ver que a las 16:00 vuelve la programación de tado.
- [ ] Probar modo Fuera (20 ºC) y Vacaciones (17 ºC).
- [ ] Cámara de la entrada: no tiene enchufe controlable → confirmar cómo se alimenta.
- [ ] Desactivar en la app tado su geolocalización si choca con el modo Fuera de HA.
- [ ] ⚠️ Marzo 2027: sin Auto-Assist la nube de tado baja a 100 llamadas/día; revisar consumo de estas rutinas.

## Backlog del panel de la tablet
- [ ] Chip de persona: **círculo verde** cuando está en casa (ahora terracota); vacío cuando está fuera (como ahora). Pedido por Aida 2026-10-21, «para un cambio en el futuro».
- [ ] Revisar con Aida la 3.ª fila del mosaico (Tira LED, Mesita Mikel, Cafetera = elección provisional de Claude).
- [ ] Portero en el panel (aviso de «llaman» y, si se decide, botón de abrir con confirmación).
- [ ] Probar el portero con un timbrazo real (aviso en móviles + ventana en la tablet + abrir).
- [ ] **Cámaras en Sistema (tablet)**: vista previa de las 2 cámaras pedida por Aida. Bloqueado: las Nest no están en HA (Nest requiere proyecto Google + verificación, igual que Drive). Opciones a decidir: integrar Nest tras la verificación de Google, o cambiar a cámaras locales.

## Lista de Aida y Mikel (2026-10-22)
- [x] App fuera de casa: URL externa configurada en el iPhone (2026-10-22); entra con datos móviles. Comprobar el POCO.
- [ ] Presencia: ambos móviles envían ya coordenadas; el iPhone no ha registrado ninguna salida desde ayer → probar hoy.
- [ ] No molestar en el iPhone: la app de iOS no lo permite → Modo Concentración programado en el iPhone.
- [ ] Termómetros inteligentes para el mapa de calor (cocina, entrada, pasillo, limpieza, baños): comprar (Zigbee, locales).
- [ ] Nest: credenciales de aplicación creadas por Aida, integración sin terminar. Bloqueo: publicar la app OAuth exige verificación de marca → web en Cloudflare Pages + Search Console (sirve también para Drive).

## Seguridad del acceso remoto
- [ ] Antes del próximo viaje: **certificado de cliente (mTLS)** en los dos móviles + regla «fuera de España solo con certificado». Verificar antes el soporte en la app de iOS. Mientras tanto, al viajar: desactivar la regla de país.
- [ ] Quitar permiso de escritura a la deploy key de GitHub.
- [x] Usuario `claude` + MCP Server (D-007). Hecho 2026-10-22 · lectura y acción (mesita Mikel) probadas
