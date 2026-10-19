# Estado inicial de Home Assistant

Revisión en **solo lectura** desde el navegador de la app de Claude (2026-10-19). Sin cambios realizados. Números de serie omitidos a propósito.

## Integraciones configuradas

| Integración | Contenido | Tipo | Observaciones |
|---|---|---|---|
| Aplicación móvil | 1 dispositivo | Local | Companion app ya vinculada (¿qué móvil?) |
| Backup | 1 servicio | Local | **Sin copias automáticas configuradas.** 1 copia manual (3,29 MB) guardada en la propia microSD |
| Bluetooth | 1 dispositivo | Local | Adaptador de la Pi |
| Comprobador de fuente de alimentación de Raspberry Pi | 1 dispositivo | Local | — |
| **Dispositivo HomeKit** | 6 dispositivos · 26 entidades | **Local** | tado Internet Bridge (IB01) + 4 × Smart Radiator Thermostat (SRT01) + 1 × Smart Thermostat (ST01). El bridge está asignado al área **«Zona gaming»** |
| Google Cast | 2 dispositivos | Local | Previsiblemente Google Home (entrada) + Nest Mini (salón) |
| Google Translate TTS | 1 servicio | Nube | — |
| Home Assistant Supervisor | 4 servicios | Local | — |
| Lista de la compra | 1 entidad | Local | — |
| Met.no | 1 servicio | Nube | Meteorología |
| Radio Browser | 1 entrada | Nube | — |
| **Roborock** | 1 dispositivo | Nube (H) | ⚠️ No figura en el inventario aportado: ¿qué dispositivo es? |
| Sol | 1 servicio | Local | — |
| **Tado** | 10 dispositivos | **Nube** | Duplica los mismos equipos que ya llegan por HomeKit |
| UPnP/IGD | 1 dispositivo | Local | ❌ Error de configuración (reintentando) |

## Descubiertas, sin añadir

| Integración | Dispositivo |
|---|---|
| iBeacon Tracker | Balizas BLE detectadas por el Bluetooth de la Pi |
| LG webOS TV | **LG UK6470PLC** (salón) |

## Conclusiones (para la auditoría)

- **tado ya funciona en local vía HomeKit** (bridge V3). Es la vía preferente frente a la nube: sin cuota de API y sin dependencia de Internet para leer/ajustar. La integración de nube podría quedar solo para lo que HomeKit no ofrece (presencia/geofencing de tado, modo ausencia) o eliminarse si HA asume esas funciones. Decisión en Fase 3.
- **Duplicidad tado nube + HomeKit**: hay entidades repetidas para los mismos equipos → limpieza en Fase 6, sin borrar nada antes de revisar dependencias.
- **No hay estrategia de backup** y la única copia está en la misma microSD que se quiere proteger → prioridad en Fase 5.
- **UPnP/IGD** con error: probablemente innecesaria (y UPnP en el router es una superficie de ataque a revisar en el Bloque F).
- Existe un área **«Zona gaming»** no recogida en el Bloque C.
