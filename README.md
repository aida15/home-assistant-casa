# home-assistant-casa

Proyecto de migración de Google Home a Home Assistant: instalación domótica real, documentada desde el día 1 como base de una futura guía/curso.

> Repo **privado**. Nunca se suben contraseñas, tokens, claves de cifrado, IPs, backups ni datos sensibles (ver `.gitignore`).

## Estado

| Fase | Estado |
|---|---|
| 1. Descubrimiento | **En curso** |
| 2. Auditoría | Pendiente |
| 3. Arquitectura | Pendiente |
| 4. Diseño visual | Pendiente |
| 5. Preparación | Pendiente |
| 6. Integración de dispositivos | Pendiente |
| 7. Dashboard | Pendiente |
| 8. Automatizaciones | Pendiente |
| 9. Seguridad y mantenimiento | Pendiente |
| 10. Optimización | Pendiente |

## Estructura

Las carpetas se crean cuando tienen contenido real (no se generan vacías).

| Carpeta | Contenido |
|---|---|
| `00_proyecto/` | Requisitos, decisiones, registro de cambios, pendientes |
| `01_inventario/` | Hardware, dispositivos e integraciones |
| `02_arquitectura/` | Infraestructura y diseño técnico |
| `03_instalacion/` | Procedimientos de configuración |
| `04_dashboards/` | Sistema de diseño, interfaces y tarjetas |
| `05_plano/` | Plano real (privado) y versión alterada para redes |
| `06_automatizaciones/` | Lógica, código y pruebas |
| `07_seguridad/` | Usuarios, accesos, copias y recuperación |
| `08_mantenimiento/` | Actualizaciones y resolución de problemas |
| `09_contenido/` | Reels, guiones, capturas y material didáctico |
| `10_curso/` | Módulos, lecciones, ejercicios |
| `11_producto/` | Propuesta de valor y comercialización |
| `config/` | (Futuro) configuración YAML de HA versionada |

## Convenciones

- Nombres de entidades y archivos en castellano, **sin tildes ni ñ** (`light.salon_techo`, `bano`).
- Estados de cada elemento: `diseñado` → `implementado` → `probado` / `pendiente de prueba`.
- Hechos verificados y **hipótesis (H)** siempre separados.
