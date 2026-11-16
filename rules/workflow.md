# Flujo de trabajo (determinístico)

Antes de tocar código para una historia o incidencia (ticket {{TICKET_PREFIX}}-XXX):

1. Crear `specs/active/{{TICKET_PREFIX}}-XXX-<slug>/` a partir de `specs/_template/`.
2. Completar `proposal.md`: entendimiento del pedido, alcance, enfoque propuesto, preguntas/decisiones abiertas. No pasar al paso 3 si hay ambigüedad real sin que el usuario confirme la propuesta.
3. Completar `tasks.md`: lista de tareas concretas derivadas de la propuesta ya aprobada.

Los pasos 1-3 se disparan con `/historia-iniciar {{TICKET_PREFIX}}-XXX <descripción>` (ver `.claude/commands/historia-iniciar.md`).

4. Ejecutar las tareas una por una. Después de cada una: tildarla en `tasks.md` y agregar una línea en `notes.md` (qué se hizo, qué archivos se tocaron, por qué). Si la tarea tiene lógica testeable, correr/crear los tests unitarios correspondientes (ver `testing.md`) antes de tildarla.

El paso 4 se dispara con `/historia-aplicar <spec>` (ver `.claude/commands/historia-aplicar.md`).

5. Si durante la ejecución aparece una decisión no trivial (cambio de alcance, ambigüedad de negocio o arquitectura, algo difícil de revertir) — parar y preguntar al usuario. No decidir solo.
6. Al aprobar el PR de la historia: versionar el/los paquete(s) afectados y mergear la rama a su destino (ver `git-flow.md` y `versioning.md`).
7. Al cerrar la historia: marcar `status: done` en `proposal.md`, reflejar el estado en el sistema de seguimiento de tickets que use el proyecto (si aplica), y mover la carpeta a `specs/archive/`.

Los pasos 6-7 se disparan con `/historia-cerrar <spec>` (ver `.claude/commands/historia-cerrar.md`).

La revisión (autochequeo antes de dar la tarea por terminada, o revisión formal al asignar un PR) no tiene paso numerado propio ni comando: son las skills `self-review`/`pr-review` (`.claude/skills/`), invocables con `/self-review` y `/pr-review`.

El ticket `{{TICKET_PREFIX}}-XXX` es el mismo ID usado en el sistema de seguimiento de tickets del proyecto (spreadsheet, Jira, Linear, etc.) — no es un ID inventado para el harness.

Aplica a cualquier repo del ecosistema.
