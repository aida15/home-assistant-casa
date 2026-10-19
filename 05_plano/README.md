# Plano de la vivienda

**PRIVADO (D-003):** este plano es la distribución real de la casa. No se publica en redes ni en el producto; para contenido se hará una versión alterada.

| Archivo | Qué es |
|---|---|
| `plano_casa.svg` | Plano vectorial v0.1, trazado a partir del plano de HomeByMe. Medidas **aproximadas** (≈ 39 px/m). Destino final: `www/plano/` en `home-assistant-config` |

## Convenciones del SVG

- Estancias: `id="area-<area_id>"` (los mismos IDs de área de HA: `salon`, `cocina`, `habitacion_aroa`…).
- Luces: `id="luz-<area_id>-<nombre>"`, grupo `<g class="luz">` con clase `on` / `no-disponible` según estado.
- Temperaturas: `id="temp-<area_id>"`.
- Ventanas del salón: `id="ventana-salon-norte|este"` (base para las persianas futuras).
- **Colores por variables CSS** (`--plano-suelo`, `--acento`, …) con valores de reserva del tema de día: el tema de noche solo redefine las variables.

## Pendiente

- Ajustar la posición de los puntos de luz a la ubicación real.
- Elegir la técnica de implementación en HA (tarjeta de imagen con elementos superpuestos vs. tarjeta de plano comunitaria) → comparativa antes de la Fase 7.
