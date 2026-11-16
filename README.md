# Harness-Template

Plantilla genérica y reutilizable para instanciar un "harness" de Claude Code en un proyecto nuevo: el repo de referencia compartido (reglas de trabajo, skills, slash commands, specs de historias) desde el que se abre Claude Code para trabajar sobre el código de producto de ese proyecto — sin mezclar esa configuración con el propio repo de producto.

Extraída de `Gestion.Servicio.Harness`, el primer harness real construido con este patrón. No contiene nada específico de ningún proyecto: todo lo que varía está marcado con placeholders `{{ASI}}`.

## Qué resuelve

Un harness le da a Claude Code, al abrirse parado en él:

1. **Acceso sin fricción a los repos de producto hermanos**, vía `permissions.additionalDirectories` en `.claude/settings.json` — se editan igual que siempre, solo cambia desde dónde se invoca `claude`.
2. **Un `CLAUDE.md` clasificador** que manda a leer la ficha (`repos/<repo>.md`) del repo correspondiente según el pedido, en vez de asumir la estructura de memoria.
3. **Reglas de proceso siempre activas** (`rules/*.md`): flujo de historias, commits, ramas/PRs, versionado, cuándo preguntar vs. avanzar solo, límites de comentarios, criterio de tests.
4. **Skills y slash commands reales de Claude Code** (`.claude/skills/`, `.claude/commands/`) que Claude Code escanea automáticamente por ser el cwd — sin instalación aparte que sincronizar.
5. **Un ciclo de vida de historias/incidencias** (`specs/active/<TICKET>-<slug>/` → `proposal.md` → `tasks.md` → ejecución con `notes.md` → `specs/archive/`), formalizado en los comandos `/historia-iniciar`, `/historia-aplicar`, `/historia-cerrar`.
6. **Una barrera real, no solo texto**: `permissions.ask`/`permissions.deny` en `settings.json` fuerzan confirmación en operaciones destructivas y bloquean la lectura de `.env*`.

## Estructura de la plantilla

```
Harness-Template/
├── CLAUDE.md                    ← clasificador + imports de rules/ (con placeholders)
├── README.md                    ← este archivo
├── ESTADO.md · VERSION · CHANGELOG.md   ← placeholders, completar al instanciar
├── repos/
│   └── _template.md             ← base para la ficha de cada repo/paquete real
├── rules/
│   ├── workflow.md               ← flujo determinístico proposal → tasks → ejecución
│   ├── commits.md
│   ├── git-flow.md               ← ramas y PRs (Git Flow)
│   ├── versioning.md             ← cuándo y dónde bumpear versión
│   ├── decisions.md              ← cuándo preguntar vs. avanzar solo
│   ├── comments.md
│   └── testing.md
├── specs/
│   ├── _template/{proposal,tasks,notes}.md
│   ├── active/.gitkeep
│   ├── archive/.gitkeep
│   └── README.md
└── .claude/
    ├── settings.json             ← additionalDirectories + ask/deny (placeholders)
    ├── ARCHITECTURE.md           ← documento de arquitectura del harness, con placeholders
    ├── skills/
    │   ├── README.md
    │   ├── _template/SKILL.md    ← base para skills específicas de dominio/tecnología
    │   ├── self-review/SKILL.md
    │   ├── pr-review/SKILL.md
    │   └── developer/SKILL.md
    └── commands/
        ├── historia-iniciar.md
        ├── historia-aplicar.md
        └── historia-cerrar.md
```

Qué **no** incluye la plantilla, porque es específico de cada proyecto y se agrega al instanciar: `deploy/README.md` (proceso de despliegue) y `herramientas/README.md` (conectores externos como Jira/Sheets/Drive) — ambos opcionales, usados en `Gestion.Servicio.Harness` pero no esenciales al patrón.

## Placeholders a reemplazar

| Placeholder | Dónde aparece | Qué poner |
|---|---|---|
| `{{PROJECT_NAME}}` | `CLAUDE.md`, `.claude/ARCHITECTURE.md`, skills | Nombre del proyecto/producto (ej. `Crape`, `Gestion.Servicio`) |
| `{{TICKET_PREFIX}}` | `rules/*.md`, `.claude/commands/*.md`, `specs/_template/*` | Prefijo de ticket (ej. `GSTRI`, `CRAPE`) |
| `{{REPOS_DESCRIPTION}}` | `CLAUDE.md` | Descripción breve de qué repos/paquetes componen el ecosistema (ej. "back, front, y futuros servicios" o "monorepo npm workspaces") |
| `{{SIBLING_REPOS_NOTE}}` | `CLAUDE.md` | Si hay repos hermanos: nota tipo " — Back y Front son repos hermanos, accesibles sin fricción vía `additionalDirectories`"; si es monorepo, puede quedar vacío o adaptarse |
| `{{CLASIFICACION_TABLA}}` | `CLAUDE.md` | Tabla markdown "pedido → qué leer" (ver `repos/_template.md` y el ejemplo de `Gestion.Servicio.Harness/CLAUDE.md`) |
| `{{VERSIONING_TARGETS}}` | `rules/versioning.md` | Lista de dónde vive la versión en cada repo/paquete (ej. `pom.xml`, `package.json`) |
| `{{TESTING_SETUP}}` | `rules/testing.md` | Qué test runner usa cada repo/paquete, si ya está configurado o falta elegirlo |
| `{{STACK_RESUMEN}}` | `.claude/skills/developer/SKILL.md` | Lista corta del stack tecnológico real |
| `{{RUTA_REPO_HERMANO_1}}` (y sucesivos) | `.claude/settings.json` | Ruta relativa (`../NombreRepo`) de cada repo hermano a sumar en `additionalDirectories` — en un monorepo puede ser una sola ruta al repo del monorepo |

## Cómo instanciar un harness nuevo

1. **Copiar esta carpeta** como `<Proyecto>.Harness/`, repo hermano (no dentro del repo de producto).
2. **Reemplazar los placeholders** de la tabla anterior en todos los archivos que los usan (`grep -rl "{{" .` para encontrarlos todos).
3. **Completar `repos/<nombre>.md`** a partir de `repos/_template.md` con el stack/estructura real de cada repo o workspace del ecosistema — uno por repo si son repos separados, o uno solo si es un monorepo describiendo sus workspaces.
4. **Ajustar `CLAUDE.md`**: completar la tabla de clasificación según cuántos repos/áreas haya realmente (si es un solo monorepo, puede no necesitar clasificar por repo sino por área/paquete interno).
5. **Configurar `.claude/settings.json`**: `additionalDirectories` con la ruta real a cada repo hermano (vacío si todo vive en un único repo ya accesible como cwd).
6. **Revisar las skills de `.claude/skills/`**: renombrar menciones de `{{PROJECT_NAME}}` y sumar skills de dominio/tecnología específicas (usar `_template/SKILL.md` como base) — ej. `database`, `ui`, o algo propio del dominio del proyecto.
7. **Completar `ESTADO.md`, `VERSION`, `CHANGELOG.md`** con el estado inicial real del harness instanciado.
8. **Agregar `deploy/README.md` y/o `herramientas/README.md`** si el proyecto los necesita (no son parte de la plantilla base).
9. En la raíz de cada repo de producto, agregar un `CLAUDE.md` de dos líneas:
   ```
   @../<Proyecto>.Harness/CLAUDE.md
   @../<Proyecto>.Harness/repos/<nombre>.md
   ```
10. **Probar en una sesión real**: abrir Claude Code parado en `<Proyecto>.Harness/` y confirmar que las skills y los slash commands (`/historia-iniciar`, `/self-review`, etc.) aparecen y funcionan.

## Cómo sumar un repo/paquete nuevo al ecosistema ya instanciado

1. Crear `repos/<nombre>.md` a partir de `repos/_template.md` con su stack/estructura real.
2. Si es un repo separado (no un workspace dentro de un monorepo ya cubierto), sumarlo a `additionalDirectories` en `.claude/settings.json` y agregarle su `CLAUDE.md` de dos líneas como en el paso 9 de arriba.

## Cómo se vive una historia/incidencia

Ver `rules/workflow.md`. En corto: spec en `specs/active/<TICKET_PREFIX>-XXX-<slug>/` con `proposal.md` → `tasks.md` → ejecución con `notes.md`, y se cierra moviéndola a `specs/archive/`.

## Cómo extender el harness instanciado

| Quiero… | Pasos |
|---|---|
| Una skill nueva | `.claude/skills/<nombre>/SKILL.md` con `name` y una `description` que diga *cuándo* se usa (ver `_template/SKILL.md`). |
| Un slash command nuevo | `.claude/commands/<nombre>.md` con `description` y `argument-hint` en el frontmatter; usar `$ARGUMENTS` en el cuerpo. |
| Una regla nueva (aplica siempre) | Archivo en `rules/` + agregar el `@rules/<nombre>.md` en `CLAUDE.md`. |
| Imponer una regla (no solo texto) | Entrada en `.claude/settings.json` (`permissions.ask`/`permissions.deny`/`additionalDirectories`). |
| Un repo/paquete nuevo en el ecosistema | Ver "Cómo sumar un repo/paquete nuevo" arriba. |
| Documentar una herramienta externa | `herramientas/README.md`. |
| Cerrar un cambio del harness | Actualizar `ESTADO.md`, subir `VERSION`, agregar entrada en `CHANGELOG.md`, y `.claude/ARCHITECTURE.md` si cambió una pieza estructural. |
