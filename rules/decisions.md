# Cuándo preguntar vs. avanzar solo

Preguntar al usuario antes de avanzar cuando:

- La historia es ambigua en alcance o criterios de aceptación.
- Hay más de un enfoque válido con trade-offs reales (arquitectura, UX, performance).
- La decisión afecta a más de un rol de usuario o a datos ya existentes (migraciones, cambios de esquema).
- La acción es difícil de revertir (borrar datos, cambiar contratos de API, tocar auth/seguridad).

Avanzar sin preguntar cuando:

- La tarea ya está acotada en `tasks.md` y sigue un patrón existente en el repo.
- Es un detalle de implementación dentro del enfoque ya aprobado en `proposal.md`.
