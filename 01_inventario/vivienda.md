# Vivienda y distribución

Datos aportados por Aida (2026-10-19): plano de HomeByMe y, como referencia definitiva, **plano de obra** con puntos de luz marcados. El plano real **no se publica** (D-003); el original queda fuera de Git hasta decidir si se versiona en el repo privado.

## Estructura

- **Plantas:** 1
- **Superficie interior útil** (suma del plano de obra): ≈ 86 m²

## Estancias y áreas en HA

Superficies del **plano de obra** (aportado por Aida el 2026-10-19), que sustituye al de HomeByMe como referencia. Nombres de área sin tildes ni ñ (D-004).

| Estancia | ID de área | m² (obra) | Nombre en el plano de obra | Puntos de luz de techo | Dispositivos conocidos | Notas |
|---|---|---|---|---|---|---|
| Entrada | `entrada` | 5,13 | Vestíbulo | 1 | Nest Cam, Google Home | Distribuidor: puerta de la calle + acceso a cocina, salón, pasillo y habitación principal |
| Pasillo | `pasillo` | 3,51 | Pasillo | 1 | — | Desde la entrada; da a estudio, hab. Aroa y baño Aroa |
| Cocina | `cocina` | ≈ 8,7 | Cocina | 1 (ZBMINIL2 previsto) | Regleta GHome, 2 sensores de agua | Sin ventanas; separación con el cuarto de limpieza totalmente acristalada. Cabezal previsto |
| Cuarto de limpieza | `cuarto_limpieza` | 3,57 | Tendedero | 1 | Fingerbot MOES (calefactor) | Ventana en toda la pared que da al jardín; puerta a la cocina |
| Salón | `salon` | 19,88 | Salón | 2 (ZBMINIL2 previstos) | Nest Cam, Nest Mini, 2 regletas, enchufe (lámpara), TV LG, sofá motorizado, válvula + termostato tado | Ventanales norte y este; **puerta a la Terraza 1** en el lado este. **Primeras persianas a motorizar** |
| Estudio | `estudio` | 11,34 | Dormitorio 2 | 1 | Válvula tado, ventilador Create, tado Internet Bridge | Antes «Zona gaming» (D-010) |
| Habitación Aroa | `habitacion_aroa` | 12,45 | Dormitorio 3 | 1 (ZBMINIL2 previsto) | Válvula tado | |
| Baño Aroa | `bano_aroa` | 4,36 | Baño | 1 | — | Ducha de pared a pared, sin bidé. Cabezal previsto |
| Habitación principal | `habitacion_principal` | 13,42 | Dormitorio 1 | 1 | Válvula tado | Baño principal dentro |
| Baño principal | `bano_principal` | 4,05 | Baño 1 | 1 | — | Ducha de pared a pared, sin bidé. Cabezal previsto |
| Jardín | `jardin` | — | — | — | Sensor de lluvia | Zona de **césped** al norte + zona **con suelo** al este (incluye «Terraza 1» junto al salón) |
| Terraza | `terraza` | — | (no se ve entera) | — | — | Junto a la habitación principal, con suelo |
| Garaje | `garaje` | — | — | — | — | Fuera de alcance por ahora (poca cobertura) |
| — | — | — | — | — | Deshumidificador Midea | **Móvil**: sin área fija |

**Puntos de luz de techo:** 11 en total; 4 se domotizarán con los ZBMINIL2 (salón ×2, cocina, hab. Aroa). El resto quedan dibujados en el plano como referencia.

## Persianas

- Ninguna motorizada todavía. **Se empieza por el salón.**
- **Persianas de cinta**: las primeras en motorizar serán las del salón; a futuro, todas las de la casa.
- Sin toma de corriente junto a las cajas: por eso no están motorizadas. **Presupuestos de instalación eléctrica pendientes.**

## Tablet de control

- **Huawei BAH-W09** (MediaPad M3 Lite 10, 2017, Android 7 de serie — H). Compatibilidad con la Companion App actual **pendiente de verificar**.
- Ubicación: probablemente la entrada (no decidido).
