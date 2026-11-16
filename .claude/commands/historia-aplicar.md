---
description: Ejecuta las tareas de una historia/incidencia ya iniciada (paso 4 de rules/workflow.md), tildando tasks.md y anotando notes.md.
argument-hint: <{{TICKET_PREFIX}}-XXX-slug | ruta del spec en specs/active/>
---

Spec objetivo: `$ARGUMENTS` (carpeta dentro de `specs/active/`).

Seguir `rules/workflow.md` paso 4, sobre el `proposal.md`/`tasks.md` ya aprobados de ese spec:

1. Ejecutar las tareas de `tasks.md` una por una, en orden.
2. Después de cada tarea: tildarla en `tasks.md` y agregar una línea en `notes.md` (qué se hizo, qué archivos se tocaron, por qué).
3. Si la tarea tiene lógica testeable, correr o crear los tests unitarios correspondientes (`rules/testing.md`) antes de tildarla.
4. Respetar los límites de comentarios en código (`rules/comments.md`) y el formato de commits (`rules/commits.md`) si el usuario pide commitear.
5. Si aparece una decisión no trivial (cambio de alcance, ambigüedad de negocio/arquitectura, algo difícil de revertir) — parar y preguntar al usuario (`rules/decisions.md`). No decidir solo ni seguir con el resto de las tareas hasta resolverlo.

Este comando toca código de producto en los repos del ecosistema (accesibles como additional directories) — no solo archivos del harness.
