# F5-05 · Actualización a 2026.10, limpieza y MFA

| Campo | Valor |
|---|---|
| Fase | 5 · Preparación |
| Estado | **Diseñado** → pendiente de ejecutar por Aida |
| Objetivo | HA al día, sin integraciones muertas, áreas con nombres reales y cuenta admin con MFA |
| Decisiones | D-009 (Roborock), D-010 (Estudio), D-022 (MFA), D-023 (UPnP) |
| Riesgo | Bajo. Backup automático antes de actualizar |
| Fuente | Notas de 2026.10 (consultadas 2026-10-20): no se han visto cambios incompatibles para tado/HomeKit, Cast, Met, Backup ni app móvil. Sí uno de **autenticación**: los nombres de usuario se normalizan; si aparece un aviso en Reparaciones, renombrar |

## 1 · Actualizar
Ajustes → Sistema → Actualizaciones → **Home Assistant Core 2026.10.x** → marcar **«Crear copia de seguridad antes de actualizar»** → Actualizar. Después, si aparece, actualizar **Operating System**.
- Validar: Ajustes → Acerca de muestra 2026.10.x; Ajustes → Reparaciones sin avisos nuevos (o anotarlos).

## 2 · Retirar Roborock y UPnP
Ajustes → Dispositivos e integraciones → *Roborock* → ⋮ → **Eliminar**. Igual con *UPnP/IGD*.
- Reversión: volver a añadir la integración (Roborock no se reinstalará: el aparato ya no existe).

## 3 · Área «Estudio»
Renombrar un área **no cambia su ID interno** (`zona_gaming`), y ese ID se usará en YAML. Por eso se crea una nueva:
1. Ajustes → Áreas → **Crear área** «Estudio» (ID `estudio`).
2. Mover a Estudio los dispositivos que estaban en «Zona gaming» (tado Internet Bridge, válvula del estudio).
3. Borrar el área «Zona gaming» cuando esté vacía.
- Validar: ningún dispositivo queda sin área por error.

## 4 · MFA en la cuenta `aida`
1. Instalar en el móvil una app TOTP (Aegis, Google Authenticator, Microsoft Authenticator…).
2. HA → Perfil → Seguridad → **Módulos de autenticación multifactor → Aplicación de autenticación → Activar**.
3. **Antes de escanear el QR**: guardar el secreto TOTP en **KeePass** (campo TOTP de la entrada de HA). Es la vía de recuperación.
4. Escanear, introducir el código.
5. **Probar en una ventana de incógnito** que pide código y entra, **sin cerrar la sesión actual**.
- Reversión: Perfil → Seguridad → desactivar el módulo.

## Resultado real
_(pendiente)_
