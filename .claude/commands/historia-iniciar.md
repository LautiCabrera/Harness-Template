---
description: Inicia una historia/incidencia nueva (pasos 1-3 de rules/workflow.md): crea el spec y completa proposal.md y tasks.md con OK humano entre cada uno.
argument-hint: <{{TICKET_PREFIX}}-XXX> <descripción breve>
---

Argumentos: `$ARGUMENTS` (primero el ticket `{{TICKET_PREFIX}}-XXX`, después una descripción breve de la historia/incidencia).

Seguir `rules/workflow.md` pasos 1-3:

1. Crear `specs/active/{{TICKET_PREFIX}}-XXX-<slug>/` a partir de `specs/_template/` (slug corto derivado de la descripción).
2. Completar `proposal.md`: entendimiento del pedido, alcance, enfoque propuesto, preguntas/decisiones abiertas. Si hay ambigüedad real, preguntar al usuario (ver `rules/decisions.md`) en vez de asumir.
3. Mostrar el `proposal.md` completado y esperar el OK explícito del usuario antes de seguir.
4. Una vez aprobado: completar `tasks.md` con las tareas concretas derivadas de la propuesta.

No tocar código de producto en este comando — solo los archivos del spec dentro del harness. No hacer commit salvo que el usuario lo pida (ver `rules/commits.md`).
