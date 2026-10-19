# Requisitos del proyecto

Síntesis de las 19 instrucciones iniciales (19/10/2026). Fuente de verdad para el resto de documentos.

## Objetivos

1. Centralizar el control de todos los dispositivos compatibles.
2. Reducir dependencia de Google Home y de nubes externas cuando sea viable.
3. Interfaz prémium, moderna e intuitiva: dashboard general, vistas por habitación y **plano interactivo** (prioritario).
4. Uso cómodo en tablet fija, móvil y ordenador.
5. Automatizar rutinas y recibir propuestas proactivas de automatización.
6. Seguridad por capas, backups recuperables y recuperación documentada.
7. Arquitectura escalable y documentación suficiente para no depender de una IA.
8. **Objetivo secundario:** documentar el proceso para Instagram y un futuro producto digital (guía/curso/plantillas), sin comprometer los anteriores.

## Orden de prioridades (punto 19, definitivo)

1. Seguridad de personas y vivienda
2. Fiabilidad, recuperación y funcionamiento predecible
3. Funcionalidad real
4. Facilidad de uso y accesibilidad
5. Mantenibilidad, documentación y control de versiones
6. Coherencia visual y experiencia
7. Funciones avanzadas y efectos visuales
8. Contenido y comercialización

## Reglas de trabajo

- Trabajo por fases (1-10) con criterio de salida; no se avanza sin validar la anterior.
- Nada de datos inventados; hechos separados de hipótesis; consultar documentación actual si depende de versión.
- Cambios pequeños y reversibles. Nada destructivo sin explicación y confirmación.
- **Backup y plan de recuperación antes de cualquier cambio.** Copia generada ≠ conservada ≠ restaurable.
- Nativo primero; HACS/extensiones solo con justificación (problema, ventajas, dependencias, riesgos, mantenimiento, plan B).
- Código completo, con ubicación, validación, resultado esperado y reversión. Sin entidades ficticias.
- Una función solo está terminada si se ha **probado**; si no, «pendiente de prueba».
- Diseñar la UX (navegación, bocetos, sistema de diseño, estados de error) antes de implementar dashboards.
- Conservar controles físicos; HA no sustituye sistemas de seguridad certificados.
- Acciones sensibles (cerraduras, alarmas, calefactores, accesos) con controles adicionales.
- Ocultar una tarjeta ≠ restringir el acceso.
- Git (control de versiones) ≠ backup.
- No pedir ni almacenar credenciales; configuraciones compartidas siempre anonimizadas.

## Documentación y contenido

- Ficha por implementación (punto 16.2) y por reel (16.6).
- Registro de decisiones y aprendizajes, errores incluidos.
- Informe breve al cierre de cada fase: progreso técnico, documentación, contenido, producto.
- Anticipar grabaciones del «antes» sin retrasar medidas urgentes de seguridad.
- Versión comercial = derivada limpia, sin datos privados, probada en entorno separado.
