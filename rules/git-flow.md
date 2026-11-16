# Ramas y Pull Requests

> Aplica a los repos de **producto** del ecosistema. El propio repo del harness no sigue este flujo todavía — ver `../README.md`.

Basado en Git Flow.

## Ramas permanentes

- `main`: versiones estables publicadas.
- `develop`: integración de lo que va a formar parte de la próxima versión.

No se eliminan.

## Ramas temporales

| Tipo | Prefijo | Origen | Destino |
|---|---|---|---|
| Feature | `feature/` | develop | develop |
| Bugfix | `bugfix/` | develop | develop |
| Release | `release/` | develop | main y develop |
| Hotfix | `hotfix/` | main | main y develop |

Nombre: `tipo/{{TICKET_PREFIX}}-XXX-DescripcionBreve` (ej. `feature/{{TICKET_PREFIX}}-012-DebtControl`). Para `release/`/`hotfix/` se usa la versión en lugar del ticket (ej. `release/2.0.0`).

El `{{TICKET_PREFIX}}-XXX` es el mismo ID de ticket usado en `specs/` (ver `workflow.md`).

## Flujo de un Pull Request

1. Crear la rama correspondiente, desarrollar, hacer commits descriptivos (ver `commits.md`).
2. Publicar la rama en el remoto y abrir el PR. Título: `{{TICKET_PREFIX}}-XXX Descripción breve`.
3. Revisión con la skill `pr-review`.
4. Al aprobar: versionar el/los paquete(s) afectados y mergear la rama a su destino **inmediatamente** (ver `versioning.md`) — no dejar el merge pendiente una vez aprobado y versionado.
5. Eliminar la rama temporal una vez integrada y confirmado que el cambio quedó en destino.

## Reglas generales

- No mergear directo a `main` durante el desarrollo habitual (solo vía `release`/`hotfix` de Git Flow).
- No crear ramas con nombre genérico si hay un ticket disponible.
- No eliminar una rama temporal antes de confirmar que se integró correctamente.
- No ejecutar operaciones destructivas (`git reset --hard`, `git push --force`, borrado de ramas) sin autorización explícita del usuario.
