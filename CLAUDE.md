# {{PROJECT_NAME}}.Harness — reglas raíz

Este repo es el punto de referencia compartido para {{PROJECT_NAME}} ({{REPOS_DESCRIPTION}}). No contiene código de producto. **Claude se abre parado acá** para trabajo de IA{{SIBLING_REPOS_NOTE}}. Ver `.claude/ARCHITECTURE.md` para el detalle completo y `ESTADO.md` para pendientes.

@rules/workflow.md
@rules/commits.md
@rules/git-flow.md
@rules/versioning.md
@rules/decisions.md
@rules/comments.md
@rules/testing.md

## Clasificar antes de leer repos/*.md

{{CLASIFICACION_TABLA}}

No asumir la estructura de memoria genérica — siempre confirmar contra el `repos/<repo>.md` correspondiente antes de decir dónde está algo.

## Resto del repo

- Repos del ecosistema: ver `repos/*.md` (cada repo del producto además importa el suyo propio vía su propio `CLAUDE.md`, como fallback si se abre Claude directo ahí).
- Skills reales: `.claude/skills/` (`self-review`, `pr-review`, `developer` — sumar las específicas de tecnología/dominio que haga falta).
- Slash commands del ciclo de historias: `.claude/commands/` (`/historia-iniciar`, `/historia-aplicar`, `/historia-cerrar`).
- Historias/incidencias en curso: ver `specs/active/`.
- Proceso de despliegue: ver `deploy/README.md` (si aplica).
- Conectores externos (gestión de tickets, documentación, etc.): ver `herramientas/README.md` (si aplica).
