# Vivienda y distribución

Datos aportados por Aida (2026-10-19) + plano exportado de HomeByMe (aprox. fiel a la realidad, no 100 % amueblado). El plano real **no se publica** (D-003); el original queda fuera de Git hasta decidir si se versiona en el repo privado.

## Estructura

- **Plantas:** 1
- **Superficie interior aproximada** (suma de estancias del plano): ~102 m²
- Superficies tomadas del plano de HomeByMe: **aproximadas**

## Estancias y áreas en HA

Nombres de área sin tildes ni ñ (D-004).

| Estancia | ID de área | m² (plano) | Dispositivos conocidos | Notas |
|---|---|---|---|---|
| Entrada | `entrada` | 6,0 | Nest Cam, Google Home | Ubicación probable de la tablet |
| Pasillo | `pasillo` | 4,3 | — | Distribuidor hacia baño principal, estudio, habitación Aroa y baño Aroa |
| Cocina | `cocina` | 10,2 | Regleta GHome, 2 sensores de agua, ZBMINI (luz techo, previsto) | Cabezal previsto |
| Cuarto de limpieza | `cuarto_limpieza` | 4,3 | Fingerbot MOES (calefactor) | En el plano figura como «Lavandería» |
| Salón | `salon` | 23,4 | Nest Cam, Nest Mini, 2 regletas, enchufe (lámpara), TV LG, sofá motorizado, válvula + termostato tado, 2 ZBMINI (luces techo, previsto) | Grandes ventanales (norte y este, H). **Primeras persianas a motorizar** |
| Estudio | `estudio` | 13,4 | Válvula tado, ventilador Create, tado Internet Bridge | Antes «Zona gaming» (D-010) |
| Habitación Aroa | `habitacion_aroa` | 14,6 | Válvula tado, ZBMINI (luz techo, previsto) | |
| Baño Aroa | `bano_aroa` | 5,3 | — | Cabezal previsto |
| Habitación principal | `habitacion_principal` | 15,7 | Válvula tado | En el plano figura como «Dormitorio» |
| Baño principal | `bano_principal` | 4,9 | — | Cabezal previsto |
| Jardín | `jardin` | ~67 (29,5 + 37,6) | Sensor de lluvia | «Exterior 1» + «Exterior 6». Acceso único desde el salón. Contiene una columna («habitación 15») |
| Terraza | `terraza` | 17,1 | — | «Exterior 5», accesible desde la habitación principal |
| Garaje | `garaje` | — | — | Fuera de alcance por ahora (poca cobertura) |
| — | — | — | Deshumidificador Midea | **Móvil**: sin área fija |

Elementos del plano **sin uso en HA**: «habitación 10/12/14» (tabiques interiores), «habitación 15» (columna del jardín) y «Exterior 2/4» (restos del modelado).

## Persianas

- Ninguna motorizada todavía. **Se empieza por el salón.**
- **Persianas de cinta**: las primeras en motorizar serán las del salón; a futuro, todas las de la casa.
- Sin toma de corriente junto a las cajas: por eso no están motorizadas. **Presupuestos de instalación eléctrica pendientes.**

## Tablet de control

- **Huawei BAH-W09** (MediaPad M3 Lite 10, 2017, Android 7 de serie — H). Compatibilidad con la Companion App actual **pendiente de verificar**.
- Ubicación: probablemente la entrada (no decidido).
