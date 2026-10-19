# Inventario · Hardware e infraestructura

Datos aportados por Aida el 2026-10-19. Sin IPs ni credenciales.

## Servidor Home Assistant

| Campo | Valor | Fuente |
|---|---|---|
| Equipo | Raspberry Pi 5, 8 GB RAM | Aida |
| Almacenamiento | microSD **SanDisk 32 GB, clase 10** (sin clasificación A1/A2 indicada) | Aida |
| Alimentación | Fuente oficial | Aida |
| Conexión | Ethernet | Aida |
| Método de instalación | Home Assistant OS | Aida |
| Core | 2026.9.3 | Info del sistema |
| Supervisor | 2026.10.2 | Info del sistema |
| Operating System | 16.3 | Info del sistema |
| Frontend | 20260826.7 | Info del sistema |
| Acceso | UI por IP local ✅ · SSH ❌ (no conecta) · monitor/teclado posibles | Aida |

## Red

| Campo | Valor |
|---|---|
| Router | Sagemcom F@st 5670 (de la operadora) |
| IP de la Pi | Dinámica, **sin reserva DHCP** |
| VLAN / red de invitados | Pendiente (Bloque F) |
| Acceso remoto | Pendiente (Bloque F) |

## Coordinadores y puentes

| Equipo | Modelo | Chip | Uso actual | Rol propuesto (H) |
|---|---|---|---|---|
| Dongle USB Zigbee | LinknLink **ZG-808Z** (Zigbee 3.0) | TI CC2652P1 (según fabricante) | Ninguno | **Coordinador único** de la red Zigbee (ZHA o Zigbee2MQTT, a decidir en Fase 3) |
| Puente Zigbee Wi-Fi | Sonoff **ZBBridge-P** (Zigbee Bridge Pro) | CC2652 + ESP32 | Comprado para los ZBMINI; no instalado | Probablemente **sin rol**: con firmware original depende de eWeLink (nube); flashearlo como coordinador por red sería redundante con el dongle |

## Equipos auxiliares

| Equipo | Notas |
|---|---|
| PC Windows (`pcaida`) | En casa, misma red. Vinculado a Claude vía app de escritorio. No está encendido 24/7 |
| Otros PCs | Disponibles, ninguno 24/7 |
| Tablet (panel fijo) | Modelo pendiente |
