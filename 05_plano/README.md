# Plano de la vivienda

**PRIVADO (D-003):** este plano es la distribución real de la casa. No se publica en redes ni en el producto; para contenido se hará una versión alterada.

| Archivo | Qué es |
|---|---|
| `plano_casa.svg` | Plano vectorial **v0.3**, trazado sobre el **plano de obra** (≈ 59 px/m, medidas aproximadas). Incluye todas las puertas y ventanas como elementos individuales y los 11 puntos de luz de techo. Destino final: `www/plano/` en `home-assistant-config` |

## Convenciones del SVG

- Estancias: `id="area-<area_id>"` (los mismos IDs de área de HA: `salon`, `cocina`, `habitacion_aroa`…).
- Luces domotizadas: `id="luz-<area_id>-<nombre>"`, grupo `<g class="luz">` con clase `on` / `no-disponible` según estado.
- Puntos de luz sin domotizar: `id="punto-luz-<area_id>"` (solo referencia).
- Puertas: `id="puerta-<area_a>-<area_b>"` y `puerta-principal`; ventanas: `id="ventana-<area_id>-<orientacion>"`. Clase `abierta` para cuando haya sensores.
- Temperaturas: `id="temp-<area_id>"`.
- **Colores por variables CSS** (`--plano-suelo`, `--acento`, …) con valores de reserva del tema de día: el tema de noche solo redefine las variables.

## Pendiente

- Confirmar: las marcas moradas del plano de obra se han interpretado como **armarios** (estudio, hab. Aroa, hab. principal).
- Elegir la técnica de implementación en HA (tarjeta de imagen con elementos superpuestos vs. tarjeta de plano comunitaria) → comparativa antes de la Fase 7.

## Cómo cambiar el plano en el futuro

- Es un archivo de texto (SVG): se puede editar pidiéndoselo a Claude, o a mano con **Inkscape** (gratis) o Figma.
- **Regla única:** conservar los `id` de estancias, luces, puertas y ventanas. El dashboard se engancha a esos `id`, no a la geometría: se pueden mover paredes o puntos sin romper nada.
- Cada versión queda en Git → cualquier cambio es reversible.
- Estado: **v0.3 aceptada por Aida como base** (2026-10-19), sabiendo que no es exacta (D-027).
