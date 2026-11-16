---
description: Cierra una historia/incidencia (pasos 6-7 de rules/workflow.md): versiona y mergea al aprobarse el PR, refleja el estado en el sistema de seguimiento y archiva el spec.
argument-hint: <{{TICKET_PREFIX}}-XXX-slug | ruta del spec en specs/active/>
---

Spec objetivo: `$ARGUMENTS` (carpeta dentro de `specs/active/`).

Seguir `rules/workflow.md` pasos 6-7:

1. Confirmar con el usuario que el PR de esta historia ya fue aprobado.
2. Versionar el/los paquete(s) afectados según `rules/versioning.md` (determinar el alcance MAJOR/MINOR/PATCH antes de bumpear; si hay ambigüedad, preguntar).
3. Mergear la rama a su destino siguiendo `rules/git-flow.md`.
4. Marcar `status: done` en `proposal.md` del spec.
5. Reflejar el cierre en el sistema de seguimiento de tickets del proyecto, si aplica (ver `herramientas/README.md`).
6. Mover la carpeta del spec de `specs/active/` a `specs/archive/`.

No hacer ningún commit, push, bump de versión ni merge sin que el usuario lo confirme explícitamente en cada paso que lo requiera.
