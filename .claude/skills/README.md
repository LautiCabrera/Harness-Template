# Skills

Acá se versionan las skills de Claude Code específicas de este producto (sin prefijo "{{project-slug}}-" en el nombre: el repo ya es de este producto, el nombre lo da por sentado):

- `developer` — implementación de funcionalidades/fixes.
- `self-review` — autochequeo del desarrollador sobre su propio cambio, antes de darlo por terminado o abrir el PR.
- `pr-review` — revisión formal cuando a alguien le asignan un PR para revisar.

Sumar skills específicas de tecnología/dominio (ej. `database`, `ui`) a medida que haga falta — ver `_template/SKILL.md`.

Esta carpeta **es** la instalación activa, no solo la fuente de verdad: Claude Code la escanea automáticamente porque se abre parado en la raíz del harness (ver `../ARCHITECTURE.md`). Editar un `SKILL.md` acá cambia el comportamiento en la próxima sesión, sin sincronizar nada a mano.

Para agregar una skill nueva: carpeta `.claude/skills/<nombre>/` con su `SKILL.md` en el formato estándar de Claude Code (ver "Cómo extender el harness" en `../../README.md`).
