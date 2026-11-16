# Estado del harness

Para la próxima persona o sesión que toque este repo. Ver también `.claude/ARCHITECTURE.md` (qué piezas hay) y `CHANGELOG.md` (qué trajo cada versión).

> Esta es la plantilla genérica (`Harness-Template`), sin proyecto instanciado — al instanciar un harness nuevo, completar esta sección con el estado real.

## Qué funciona hoy

- Reglas de proceso (`rules/*.md`) importadas vía `CLAUDE.md`.
- Skills base (`self-review`, `pr-review`, `developer`) y slash commands (`/historia-iniciar`, `/historia-aplicar`, `/historia-cerrar`) — **solo si Claude se abre parado en la raíz del harness**.
- `.claude/settings.json` con placeholders para `additionalDirectories` y las reglas impuestas por permisos.

## Decisiones — impuesta vs. escrita

- **Impuesta** (`settings.json`, no se puede saltear sin tocar el archivo): acceso a repos hermanos, `ask` en git push/reset --hard/branch -D/borrado recursivo, `deny` en `.env*`.
- **Escrita** (prosa en `rules/`, depende de que se siga): todo lo demás — formato de commits, cuándo versionar, cuándo preguntar antes de avanzar, límites de comentarios, criterio de tests.

## Pendientes al instanciar

1. Reemplazar todos los placeholders `{{...}}` (ver tabla en `README.md`).
2. Completar `repos/<nombre>.md` con el stack real.
3. Decidir si hace falta `deploy/README.md` y/o `herramientas/README.md`.
4. Probar en una sesión real que skills y comandos aparecen y funcionan.
