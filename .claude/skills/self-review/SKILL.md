---
name: self-review
description: "Autochequeo que debe hacer quien implementó un cambio en {{PROJECT_NAME}} antes de darlo por terminado o abrir el PR."
---

# Self-review (autor del cambio)

Antes de marcar la tarea como terminada o abrir el PR, repasar el propio cambio:

- ¿Cumple el alcance de `proposal.md` y todas las tareas de `tasks.md` del spec?
- ¿Hay bugs potenciales, casos borde sin cubrir, errores funcionales?
- ¿Se respetó la arquitectura y convenciones existentes del repo (ver `repos/<repo>.md` correspondiente)?
- ¿Quedó código duplicado, deuda técnica evitable, complejidad innecesaria?
- ¿Los comentarios son acotados y no excesivos (`rules/comments.md`)?
- ¿Corresponde agregar o actualizar tests unitarios, y pasan (`rules/testing.md`)?
- ¿Quedó código de debug, logs de prueba, o cambios fuera de alcance?

Si algo de esto falla, corregirlo antes de dar la tarea por terminada — no delegarlo al revisor del PR.
