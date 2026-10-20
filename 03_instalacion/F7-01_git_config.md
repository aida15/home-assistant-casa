# F7-01 · Configuración de HA en Git (D-021)

| Campo | Valor |
|---|---|
| Estado | **En curso** |
| Objetivo | `/config` versionado en `aida15/home-assistant-config`; Claude escribe YAML en GitHub y HA lo recibe con la app Git pull |
| Riesgo | Bajo en el arranque (solo `git init` y `push`). **Alto si se instala Git pull antes de que el repo contenga la config**: la app vacía `/config` y solo restaura archivos no YAML |

## 1 · Terminal (hecho por Claude, 2026-10-20)
App oficial **Terminal & SSH** 10.5.0 instalada, iniciada y en la barra lateral. Sin puerto SSH ni contraseña (solo terminal web).

## 2 · Clave de despliegue (Aida)
```bash
mkdir -p /config/.ssh && ssh-keygen -t ed25519 -N "" -C "ha-casa" -f /config/.ssh/github_config && cat /config/.ssh/github_config.pub
```
GitHub → repo → Settings → Deploy keys → Add: título «HA Pi», pegar la **pública**, **Allow write access** ✔ (solo para el primer envío). La privada no sale de HA salvo para la app Git pull.

## 3 · Primer commit desde HA (Aida)
Contenido del `.gitignore` (lista blanca): ver `config_gitignore.txt`. Comandos: ver el mensaje de Claude del 2026-10-20 (se crean con `printf` para evitar errores de pegado).
Revisar la lista de `git status`: **no** debe aparecer `secrets.yaml`, `.storage`, `.ssh` ni `.db`.

## 4 · Revisión (Claude) → tema y paquetes en el repo
## 5 · App Git pull (Aida pega la clave privada en sus opciones): `pull`, rama `main`, sin reinicio automático
## 6 · Quitar permiso de escritura a la deploy key

## Reversión
Borrar `/config/.git` y `/config/.gitignore`; borrar la deploy key en GitHub; desinstalar las apps.

## Incidencia 2026-10-20
Al redactar esta ficha, un heredoc anidado ejecutó por error `git init/config/commit` en el **repo de documentación del sandbox de Claude** (no en HA). Detectado al momento: commit local deshecho, identidad restaurada, nada se subió. 🎓 Lección: no anidar heredocs con el mismo delimitador.
