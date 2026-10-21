# Mapa de entidades (actualizado 2026-10-20)

Convención: `<dominio>.<area>_<elemento>`, sin tildes ni ñ (D-004, D-030). Los enchufes que alimentan una lámpara se muestran como **luz**; el `switch` original queda oculto.

## Tuya (Smart Life)

| Dispositivo | Área | Entidad | Qué controla |
|---|---|---|---|
| e1 · Lámpara pie sala | sala | `light.sala_lampara_pie` | Lámpara de pie junto a la puerta de la terraza |
| e2 · Lámpara mesita Mikel | habitacion_darlings | `light.habitacion_darlings_lampara_mikel` | Lámpara de la mesita de Mikel |
| r1 · Regleta sala 1 | sala | `switch.sala_camara` | Toma 1 · **Nest Cam sala** ⚠️ no apagar por error |
| | | `switch.sala_regleta_1_toma_2` | Toma 2 · libre |
| | | `switch.sala_altavoz` | Toma 3 · **altavoz Google de la sala** |
| | | `switch.sala_regleta_1_usb` | USB · libre |
| | | `sensor.sala_regleta_1_{potencia,intensidad,tension,energia}` | Consumo |
| r2 · Regleta sala 2 | sala | `switch.sala_regleta_2_toma_1` | Toma 1 · libre |
| | | `switch.sala_sofa` | Toma 2 · sofá motorizado |
| | | `switch.sala_tv_corriente` | Toma 3 · TV LG (corriente) |
| | | `light.sala_tira_led` | USB · tira LED bajo el mueble de la TV |
| | | `sensor.sala_regleta_2_{potencia,intensidad,tension,energia}` | Consumo |

| r cocina · Regleta cocina | cocina | `switch.cocina_lavadora` | Toma 1 · lavadora ⚠️ no cortar en marcha |
| | | `switch.cocina_cafetera` | Toma 2 · cafetera |
| | | `switch.cocina_freidora_aire` | Toma 3 · freidora de aire |
| | | `switch.cocina_regleta_usb` | USB · libre |
| | | `sensor.cocina_regleta_{potencia,intensidad,tension,energia}` | Consumo |

Pendientes de re-emparejar: sensores de agua (cocina), 3.er enchufe. Deshumidificador Midea: Marketplace 2026.11.

## LG webOS

| Dispositivo | Área | Entidad | Qué controla |
|---|---|---|---|
| TV sala (50UK6470PLC) | sala | `media_player.sala_tv` | Encender/apagar, volumen, fuentes y apps |
| | | `switch.sala_tv_pantalla` | Apagar solo la pantalla |

Encender la TV desde HA requiere *Wake on LAN* + automatización (Fase 8). Pendiente: reserva DHCP de la TV en el router.

## Notas
- Las regletas **no exponen** el «estado al encender» en HA ni Aida lo encuentra en la app (H: el firmware no lo ofrece). Mitigación en Fase 8: automatización que, si la regleta vuelve tras un corte, enciende **cámara** y **altavoz**.
