# Plano de la vivienda

**PRIVADO (D-003):** este plano es la distribución real de la casa. No se publica en redes ni en el producto; para contenido se hará una versión alterada.

| Archivo | Qué es |
|---|---|
| `plano_casa.svg` | Plano vectorial **v0.3**, trazado sobre el **plano de obra** (≈ 59 px/m, medidas aproximadas). Incluye todas las puertas y ventanas como elementos individuales y los 11 puntos de luz de techo. Destino final: `www/plano/` en `home-assistant-config` |

## Convenciones del SVG

- Estancias: `id="area-<area_id>"` (los mismos IDs de área de HA: `sala`, `cocina`, `habitacion_aroa`…).
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

## Evolución prevista: plano 3D con luces realistas (fase posterior)

Objetivo de Aida (2026-10-19): un plano 3D renderizado de la casa en el que las luces se «encienden» de forma realista, como las referencias de HA en tablet.

**Técnica (habitual en HA):**
1. Modelar la casa en 3D (Sweet Home 3D, gratuito) a partir del plano de obra.
2. Renderizar desde **la misma cámara fija**: una imagen base de día, una base de noche (todo apagado) y **una imagen por luz** encendida.
3. En HA, superponer esas imágenes sobre la base y mostrar cada una solo cuando su luz está encendida.
4. Encima, una capa invisible con las zonas táctiles: reutiliza los `id` de este SVG (estancias, luces, puertas, ventanas).

**Encaje en el plan:** prioridad 7 (efectos visuales) → se hace **después** de tener el dashboard funcional con el SVG (Fase 7) y se valida el rendimiento en la tablet de 2017 (imágenes optimizadas a su resolución, formato WebP).

**Privacidad:** un render 3D revela aún más la casa que el plano → mismo tratamiento que D-003 (versión alterada para redes).
