# Resumen de requisitos · Cierre de Fase 1

Fecha: 2026-10-19 · Estado: **pendiente de validación por Aida**

Detalle en: `requisitos.md`, `uso_cotidiano.md`, `../01_inventario/*`, `../04_dashboards/direccion_visual.md`.

## 1. Infraestructura

- Raspberry Pi 5 8 GB · HAOS 16.3 · Core 2026.9.3 · Ethernet · router de operadora Sagemcom F@st 5670.
- Almacenamiento actual: microSD SanDisk 32 GB clase 10 → **migrar a SSD** (hay un SSD en desuso reaprovechable).
- IP dinámica → **reserva DHCP**.
- Zigbee: **LinknLink ZG-808Z como coordinador único**; Sonoff ZBBridge-P sin rol previsto.

## 2. Dispositivos (prioridad de integración, a refinar en Fase 2)

| Prioridad | Grupo | Vía prevista |
|---|---|---|
| Alta | Calefacción tado V3 (4 válvulas + termostato) | Local vía HomeKit (ya funciona); nube solo si aporta algo |
| Alta | Luces de techo con 4 ZBMINI (hab. Aroa, salón ×2, cocina) | Zigbee local |
| Alta | Regletas/enchufes Tuya (sofá, TV, lámpara, cocina) | Tuya (nube) o local, a decidir |
| Alta | TV LG UK6470PLC | LG webOS (local) |
| Media | Altavoces Google (Cast) | Google Cast (local) |
| Media | Cámaras Nest ×2 | Google Nest (nube); solo directo, sin grabación |
| Baja | Ventilador Create, deshumidificador Midea | Tuya / HACS Midea LAN |
| Baja | Sensores Wi-Fi (agua, lluvia), Fingerbot | Recuperar primero; despriorizados |
| Fuera | Roomba Plus 405 Combo | Sin integración soportada; sigue en su app |
| Futuro | 5 persianas de cinta (salón) | Pendiente de instalación eléctrica y presupuestos |
| Futuro | Cabezales en baños, entrada y cocina | Decisión antes de finales de marzo 2027 (renovación tado) |

## 3. Vivienda

- 1 planta, ~102 m² interiores, 12 áreas + garaje (fuera de alcance). Plano de HomeByMe disponible.

## 4. Uso

- Usuarios: Aida (Android, **única administradora**), Mikel (iOS) y tablet familiar (Huawei BAH-W09) — ambos **solo consumo** del dashboard.
- Acciones diarias a un toque: **luces, sofá, TV, calefacción**.
- Voz: «Ok Google» opcional (indiferente mantenerlo).
- Remoto: encender cámaras, revisar luces. Futuro: modo vacaciones.

## 5. Diseño

- Claro, **neutros cálidos (piedra/arena) + acento terracota**; **oscuro automático de noche**.
- Plano como protagonista (render cenital de HomeByMe + SVG interactivo, a validar).
- Ligero para tablet de 2017: sin desenfoques ni animaciones pesadas. Sin datos ficticios.
- Cámaras **fuera de la portada**: vista secundaria para comprobar encuadre.

## 6. Seguridad, acceso y backups

- **Sin Nabu Casa** a priori. Preferencia: **dominio propio**. Propuesta (H, a validar en Fase 3): túnel saliente (sin abrir puertos) + MFA obligatorio + capa de acceso adicional.
- Sin invitados ni accesos de mantenimiento.
- Cámaras: solo directo, sin grabación.
- Backups: **automáticos**, copia en la Pi + copia en una **cuenta de Google Drive dedicada**, cifrados, con prueba de restauración.
- Tolerancia: una caída de una tarde es una molestia, no un problema → no hace falta alta disponibilidad; sí recuperación documentada.

## 7. Decisiones abiertas para Fase 2-3

- Vía para Tuya (nube oficial vs local) por dispositivo.
- ZHA vs Zigbee2MQTT.
- Mecanismo exacto de acceso remoto con dominio propio.
- Retirar o mantener la integración tado de nube.
- Compatibilidad de la tablet con la Companion App / modo quiosco.
- Mantener «Ok Google» (requiere exponer HA a Google sin Nabu Casa).

---

## Informe de fase (punto 16.12)

**A. Progreso técnico** — Implementado: nada (fase de descubrimiento). Probado: acceso de lectura a HA desde el PC vinculado. Pendiente: validación de este resumen.

**B. Documentación** — Creado: requisitos, decisiones (D-001 a D-014), pendientes, inventario de hardware/dispositivos/vivienda, estado inicial de HA, uso cotidiano, dirección visual con referencias. Falta: variante de los ZBMINI y neutro, modelos de regletas y enchufes, tipo de SSD.

**C. Oportunidades de contenido** (respaldadas por hallazgos reales)
- «Tu microSD es una bomba de relojería»: por qué migrar HA a SSD (antes/después real cuando se haga).
- «Mi termostato ya funcionaba sin nube y no lo sabía»: tado vía HomeKit en local.
- «Lo primero que hay que hacer en Home Assistant (y casi nadie hace)»: backups automáticos fuera de la Pi.
- Grabaciones pendientes del «antes»: ver `09_contenido/capturas_pendientes.md`.

**D. Producto digital**
- Lección candidata: «Inventario y auditoría antes de instalar nada» (metodología de esta fase).
- Recurso descargable candidato: **plantilla de inventario de dispositivos** con columnas de protocolo, integración y dependencia de nube.
- Diferenciador: proceso real con errores y descubrimientos (Roomba no compatible, tado local oculto, backups inexistentes).
