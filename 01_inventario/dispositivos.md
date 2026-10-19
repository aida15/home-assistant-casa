# Inventario · Dispositivos

Versión 0.2 (2026-10-19). **Protocolos e integraciones marcados (H) son hipótesis** hasta verificarlos en la auditoría; nada se da por compatible por funcionar con Google Home.

Leyenda integración: **Oficial-local** · **Oficial-nube** · **HACS** (comunitaria) · **Matter** · **Zigbee** (coordinador propio, local).

| ID | Dispositivo | Marca / modelo | Cant. | Ubicación | Protocolo | Integración candidata | Nube | Estado / falta |
|---|---|---|---|---|---|---|---|---|
| DEV-01 | Robot aspirador «atrapacacas» | iRobot **Roomba Plus 405 Combo** (fw 9.3.7) | 1 | — | Wi-Fi | ❌ **No soportado** por la integración oficial (gama x05 excluida). Alternativas a investigar en la auditoría | Sí (app iRobot) | Se mantiene en la app iRobot / Google Home hasta encontrar vía viable |
| DEV-02 | Regleta | GHome | 1 | Cocina | Wi-Fi (Tuya, H) | Tuya oficial (nube) · tuya-local (HACS, local) | Según vía | Modelo |
| DEV-03 | Sensor de agua | Smart Life | 2 | Cocina | Wi-Fi (H, según Aida) | Tuya oficial (nube) | Sí (H) | **No funcionan actualmente**. Modelo |
| DEV-04 | Fingerbot (acciona calefactor) | MOES | 1 | Cuarto de limpieza | Wi-Fi (H, según Aida) | Por determinar | Por determinar | Modelo. ⚠️ R-04 |
| DEV-05 | Sensor de lluvia | eWeLink | 1 | Jardín | Wi-Fi (H, según Aida) | Por determinar | Por determinar | **No funciona actualmente** (H). Modelo |
| DEV-06 | Regletas (TV, cámara, sofá, asistente) | GHome | 2 | Salón | Wi-Fi (Tuya, H) | Igual que DEV-02 | Según vía | Modelo, qué toma alimenta qué. ⚠️ R-03 |
| DEV-07 | Deshumidificador | Midea | 1 | **Móvil** (se mueve según humedad medida por tado) | Wi-Fi | Sin integración oficial. HACS «midea LAN» (local) — a evaluar | No (LAN) | Modelo |
| DEV-08 | Ventilador | Create | 1 | Estudio | Wi-Fi (Tuya, H) | Tuya oficial (nube) · tuya-local (HACS) | Según vía | Modelo |
| DEV-09 | Cámara Nest Cam (wired) | Google | 2 | Entrada, salón | Wi-Fi | Oficial «Google Nest» (SDM API, nube, proyecto en Device Access) | **Sí** | Generación exacta |
| DEV-10 | Altavoz inteligente | Google Home (original) | 1 | Entrada | Wi-Fi | Oficial «Google Cast» (local: TTS, multimedia) | Voz: sí | Varios años de antigüedad |
| DEV-11 | Altavoz inteligente | Google Nest Mini | 1 | Salón | Wi-Fi | Oficial «Google Cast» (local) | Voz: sí | Generación no identificada (no es router Thread) |
| DEV-12 | Enchufe inteligente (lámpara) | Smart Life | 1 en uso | Salón | Wi-Fi (Tuya, H) | Tuya oficial · tuya-local | Según vía | Modelo |
| DEV-13 | Enchufes inteligentes libres | Pendiente | 3 | — | — | — | — | Marca |
| DEV-14 | Válvulas termostáticas + termostato | tado **V3** (con suscripción Auto-Assist) | 4 + 1 | Dorm. principal, dorm. Aroa, estudio, salón + termostato salón | Propietario tado vía Internet Bridge | ✅ **Ya integrado en local vía HomeKit** (bridge IB01 + 4 SRT01 + 1 ST01). También integrado por la nube (duplicado) | HomeKit: No · Integración tado: Sí | 🗓️ Renovación finales de marzo 2027 |
| DEV-15 | Televisor | LG **UK6470PLC** (webOS) | 1 | Salón | Wi-Fi/Ethernet | Oficial «LG webOS TV» (local) — **descubierta por HA, sin añadir** | No | — |
| DEV-16 | Sofá motorizado (en regleta) | — | 1 | Salón | Sin conectividad propia | Solo vía regleta (alimentación) | — | — |
| DEV-17 | Relés Zigbee | Sonoff **ZBMINI** | 4 | Sin instalar. Destino: luz de techo dorm. Aroa, salón (×2), cocina | Zigbee | Zigbee (coordinador propio, local) | No | Confirmar variante (ZBMINI requiere neutro; ZBMINI-L no) y si hay neutro en las cajas. Mantener interruptor físico (S1/S2) |

| DEV-18 | Robot aspirador antiguo | Roborock | 1 | — | Wi-Fi | Oficial «Roborock» — configurada en HA | Sí | 🗑️ **Ya no se usa** → retirar la integración (D-009) |

## Planes declarados

- **Calefacción:** cabezal en todas las estancias (faltan los 2 baños, entrada y cocina). No tiene por qué ser tado. Si HA cubre lo que hace Auto-Assist, se puede dejar de pagar la suscripción (renueva a finales de marzo 2027).

## Notas para la auditoría

- Varios dispositivos Wi-Fi no funcionan hoy: revisar uno a uno antes de integrarlos (¿batería?, ¿fuera de cobertura?, ¿desvinculados?).
- La cuota diaria de tado afecta a la frecuencia de sondeo y a la presencia vía tado.
- Ningún asistente actual actúa como router Thread.
- Regletas del salón → R-03. Fingerbot y calefactor → R-04. Roomba posible x05 → R-08.
