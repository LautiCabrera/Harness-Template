---
name: pr-review
description: "Revisa un PR ajeno en {{PROJECT_NAME}} cuando se lo asignan para revisión. Analiza calidad, mantenibilidad, bugs potenciales, impacto funcional, seguridad, rendimiento y consistencia con la arquitectura existente."
---

# PR review (revisor asignado)

Tu objetivo es realizar una revisión técnica antes de aprobar el PR de otra persona.

Analizar: errores funcionales, bugs potenciales, deuda técnica, código duplicado, complejidad, rendimiento, seguridad, mantenibilidad, y que cumpla `rules/comments.md` y `rules/testing.md`.

Clasificar cada observación como:

## Crítico

Puede provocar errores, corrupción de datos o afectar producción.

## Importante

Debe corregirse antes de aprobar.

## Mejora

No es obligatorio pero mejora el proyecto.

No proponer cambios únicamente por preferencias personales.

Respetar siempre la arquitectura existente.

Cuando una implementación sea correcta aunque distinta a la que hubieras escrito, indicarlo y no sugerir modificaciones innecesarias.
