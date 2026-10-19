# Registro de decisiones

Formato: ID · fecha · decisión · motivo · alternativas descartadas. 🎓 = interés didáctico.

| ID | Fecha | Decisión | Motivo | Alternativas descartadas |
|---|---|---|---|---|
| D-001 | 2026-10-19 | Lógica y dashboards en **YAML**; integraciones y ajustes internos en la **UI** | YAML es versionable, documentable y reutilizable para el producto; las integraciones las gestiona HA internamente | Todo en UI (poco trazable); todo en YAML (no soportado por muchas integraciones) 🎓 |
| D-002 | 2026-10-19 | Documentación en **repo privado de GitHub** (`aida15/home-assistant-casa`) | Historial, reversión y base del curso | Notion, Google Drive |
| D-003 | 2026-10-19 | **Plano real privado**; versión alterada/anonimizada para redes | Publicar distribución + ubicación de cámaras es un riesgo de seguridad (prioridad 1 > 8) | Publicar el plano real 🎓 |
| D-004 | 2026-10-19 | Nombres de entidades en **castellano sin tildes ni ñ** | Consistencia y compatibilidad de IDs | Inglés |
| D-005 | 2026-10-19 | Contenido y curso **solo en castellano**; en cámara aparece **solo Aida** | Decisión de la propietaria | Bilingüe |
| D-006 | 2026-10-19 | `custom:button-card` queda como **candidata a evaluar** en Fase 4, no instalada | Primero UX, después herramientas | — |
| D-007 | 2026-10-19 | Acceso de Claude por capas: (1) repo GitHub, (2) PC vinculado vía app de escritorio, (3) MCP Server de HA solo tras definir acceso remoto seguro. Siempre con **backup previo** y **usuario de HA dedicado y revocable** | Acceso rápido sin abrir la red; mínimo privilegio | Exponer HA a Internet ya 🎓 |
| D-009 | 2026-10-19 | Retirar la integración **Roborock** (aspirador antiguo, ya no existe) | Evitar entidades huérfanas y dependencias de nube inútiles | Mantenerla |
| D-010 | 2026-10-19 | El área «Zona gaming» pasa a llamarse **«Estudio»** (`estudio`) en todos los dispositivos y zonas | Nombre real de la estancia | — |
| D-008 | 2026-10-19 | Notificaciones en 3 prioridades (crítica / importante / informativa); resúmenes y mantenimiento como tipos dentro de informativa | Reconcilia puntos 17.4 y 18.4 | 5 niveles planos |
