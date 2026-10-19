# Fase 4 · Entrega 1 — Navegación, sistema visual e Inicio

Fecha: 2026-10-19 · Estado: **pendiente de revisión por Aida**
Lienzo de diseño (privado): «HA Casa · Diseño» en Claude (artifact `Wmd8wLbLmdsYco2qYBZ5bt`).

## Contenido

| Artboard | Qué muestra |
|---|---|
| Tablet · Inicio · Día | Plano como protagonista + accesos rápidos (luces salón, sofá, TV, calefacción) + modos + clima en tarjeta oscura |
| Tablet · Inicio · Noche | La misma pantalla con el tema oscuro (mismo diseño, otro conjunto de colores) |
| Móvil · Inicio | Accesos rápidos, modos, clima compacto, estancias y barra inferior |
| Mapa de navegación | Inicio → Plano / Estancias / Clima / Cámaras (secundaria) / Sistema (solo admin); tocar un dispositivo abre su ficha |
| Sistema visual | Paleta día/noche, tipografía Figtree, medidas, 4 estados de dispositivo |

## Decisiones de diseño propuestas

- **Tablet:** el plano ocupa la mitad izquierda (D-003, plano protagonista); a la derecha lo de uso diario.
- **Tarjeta de clima oscura** sobre fondo claro: el «punch» de contraste (ref. 03).
- **Terracota = encendido / activo.** Todo lo demás, neutros.
- **Estados nunca solo por color:** icono lleno/hueco/tachado, texto y estilo de borde.
- **Tipografía Figtree** (Google Fonts): legible a distancia, cifras tabulares.
- **Ligero para la tablet de 2017:** sin desenfoques, sin sombras pesadas, sin animaciones.
- Cámaras fuera de la portada (D-015). «Sistema» no visible en la tablet (D-022).

## Limitaciones del boceto

- Valores de ejemplo; en HA solo se mostrarán datos reales.
- Plano simplificado y aproximado a partir del de HomeByMe.
- Es un prototipo visual: la implementación en HA usará tarjetas que se elegirán después (Marketplace de 2026.11 si hace falta).

## Pendiente para la Entrega 2

- Vista de estancia, ficha de control (emergente), vista Clima.
- Estados de carga, error y pérdida de conexión.
- Plano interactivo: render de HomeByMe + SVG.
