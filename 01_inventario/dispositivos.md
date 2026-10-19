# Inventario · Dispositivos

Versión 0.1 (2026-10-19) — lista aportada por Aida. **Protocolos e integraciones son hipótesis (H)** hasta conocer el modelo exacto; nada se da por compatible por funcionar con Google Home.

Leyenda integración: **Oficial-local** · **Oficial-nube** · **HACS** (comunitaria) · **Matter** · **Zigbee** (vía coordinador propio, local).

| ID | Dispositivo | Marca | Cant. | Ubicación | Protocolo (H) | Integración candidata (H) | Nube | Falta |
|---|---|---|---|---|---|---|---|---|
| DEV-01 | Robot aspirador | iRobot Roomba | 1 | — | Wi-Fi | Oficial «iRobot Roomba and Braava» (local push). Modelos x05 no soportados; j7 no entrega contraseña local | No para HA (la app sí) | Modelo |
| DEV-02 | Regleta | GHome | 1 | Cocina | Wi-Fi (ecosistema Tuya, H) | Tuya oficial (nube) · tuya-local (HACS, local) · Matter si el modelo lo soporta | Según vía | Modelo, app que usas |
| DEV-03 | Sensor de agua | Smart Life | 2 | Cocina | Wi-Fi o Zigbee (H) | Si Zigbee → coordinador propio (local). Si Wi-Fi → Tuya (nube) | Según protocolo | Modelo, ¿lleva hub? |
| DEV-04 | Pulsador Fingerbot (acciona calefactor) | MOES | 1 | Cuarto de limpieza | Bluetooth o Zigbee (H) | Si Zigbee → coordinador propio. Si BLE → Tuya vía gateway o HACS | Según protocolo | Modelo/versión |
| DEV-05 | Sensor de lluvia | eWeLink | 1 | Jardín | Wi-Fi o Zigbee (H) | Si Zigbee → coordinador propio. Si Wi-Fi → eWeLink/SONOFF (nube o LAN) | Según protocolo | Modelo, ¿va por el bridge Sonoff? |
| DEV-06 | Regletas (TV, cámara, sofá, asistente) | GHome | 2 | Salón | Wi-Fi (H) | Igual que DEV-02 | Según vía | Modelo, qué toma alimenta qué |
| DEV-07 | Deshumidificador | Midea | 1 | — | Wi-Fi | Sin integración oficial. HACS «midea LAN» (local) — a evaluar | No (LAN) | Modelo, ubicación |
| DEV-08 | Ventilador | Create | 1 | Estudio | Wi-Fi (Tuya, H) | Tuya oficial (nube) · tuya-local (HACS) | Según vía | Modelo |
| DEV-09 | Cámara Nest Cam (wired) | Google | 2 | Entrada, salón | Wi-Fi | Oficial «Google Nest» (SDM API, nube, requiere proyecto en Device Access) | **Sí** | Generación exacta |
| DEV-10 | Asistente de voz | Google | 2 | Entrada, salón | Wi-Fi | Oficial «Google Cast» (local: TTS, multimedia) | Voz: sí | Modelo (Nest Mini / Nest Hub…) |
| DEV-11 | Enchufes inteligentes | — | 4 (1 en uso, 3 libres) | — | — | — | — | Marca, modelo, dónde está el que se usa |
| DEV-12 | Termostatos/válvulas | tado | 4 + 1 | Dorm. principal, dorm. Aroa, estudio, salón + termostato en salón | Propietario tado / Thread (si tado X) | V3+: oficial «tado» (nube, API no oficial, **cuota diaria desde 01/2026**). tado X: **Matter** (local) | Sí (V3+) | Generación (V3+ o X), tipo de cada unidad, ¿suscripción Auto-Assist? |
| DEV-13 | Televisor | — | 1 | Salón | — | — | — | Marca, modelo, ¿smart TV? |
| DEV-14 | Sofá (en regleta) | — | 1 | Salón | — | — | — | ¿Motorizado? |

## Notas para la auditoría

- La cuota diaria de tado afecta a la frecuencia de sondeo y a la detección de presencia vía tado.
- Si algún asistente es un **Nest Hub (2.ª gen) o Nest Hub Max**, actúa como border router Thread: relevante si los tado son tado X.
- Las regletas del salón alimentan la cámara y el asistente → ver riesgo R-03.
- El Fingerbot acciona un calefactor → ver riesgo R-04.
