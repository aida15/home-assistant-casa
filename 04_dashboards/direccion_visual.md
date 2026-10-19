# Dirección visual (Bloque D) — borrador

Estado: **en definición** (Fase 1). No es todavía el sistema de diseño; eso se cierra en Fase 4.

## Preferencias declaradas (Aida, 2026-10-19)

- **Colores neutros** con **un «punch» de color** que resalte.
- Referencias en `referencias/` (uso interno; son diseños de terceros, **no se publican ni se reutilizan en el producto**).

## Análisis de referencias

| Ref | Qué es | Lo que se toma | Lo que no |
|---|---|---|---|
| 01 · Salvia y tarjetas | Concepto (no HA) claro, verde salvia | Paleta neutra cálida con **un único acento oscuro**; lista de estancias; termostato en arco; miniatura de cámara | Gráfica de energía (no hay medidores: sin datos reales, no se muestra) |
| 02 · Plano 3D seguridad | Concepto (no HA), gris claro | **Plano como protagonista**: vista cenital con volumen, estancia activa coloreada, iconos de dispositivo sobre el plano | Cerraduras y miembros (no aplica) |
| 03 · Pestañas + termostato | Concepto (no HA), blanco/azul | Navegación por **pestañas de estancia arriba**; **contraste**: una tarjeta oscura (clima) sobre fondo claro como «punch» | Consumos (sin datos) |
| 04 · HA real, teal | Instalación real en tablet | Arco de termostato grande, escenas como botones, accesos por estancia | Efecto cristal/desenfoque: pesado para una tablet de 2017 |
| 05 · HA real, oscuro | Instalación real en tablet | Reloj grande, rejilla de estancias, meteo | Densidad excesiva |

## Patrones comunes

1. Fondo neutro + **un solo color de acento** para «encendido/activo».
2. **Termostato en arco** como pieza central.
3. **Accesos por estancia** siempre visibles.
4. **Cámara en miniatura** integrada.
5. Tarjetas grandes, redondeadas, mucho aire.

## Restricciones

- **Tablet Huawei de 2017**: sin desenfoques ni animaciones costosas; el impacto viene de composición, contraste y plano.
- **Sin datos ficticios**: nada de gráficas de energía hasta que haya medidores.
- Las referencias 01-03 son maquetas de Dribbble: en HA se aproximan con tarjetas personalizadas, no se replican al píxel.

## Idea para el plano (a validar en Fase 4)

- Usar un **render cenital 3D exportado de HomeByMe** como fondo (estilo ref 02) y superponer un **SVG** con las estancias y dispositivos vinculados a entidades.
- La versión para redes usaría un render alterado (D-003).

## Decisiones pendientes

- [x] Color de acento: **terracota** (D-011)
- [x] Tema: claro de día, **oscuro automático de noche** (D-012)
