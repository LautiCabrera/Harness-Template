# Arquitectura del harness

Cómo está armado este harness: qué piezas tiene, cuándo corre cada una y cómo se conecta con el/los repo(s) de producto.

Hay otro documento sobre este repo y no es este: `../ESTADO.md` (en qué punto está, pendientes, decisiones impuestas vs. escritas).

---

## 1. La idea en tres líneas

1. **Claude se abre siempre parado en este repo** (`{{PROJECT_NAME}}.Harness/`). Los repos de producto son hermanos, habilitados vía `permissions.additionalDirectories` en `settings.json` — se editan igual que siempre, solo cambia desde dónde se invoca `claude`.
2. **`CLAUDE.md` clasifica la tarea** (¿qué repo/área toca?) y manda a leer el `repos/<repo>.md` correspondiente. El resto de las reglas (`rules/*.md`) se importan siempre.
3. **Lo que importa de verdad se impone con `settings.json`, no solo con texto.** Todo lo demás (skills, comandos, reglas en `rules/`) es guía que se lee, no se fuerza.

## 2. Layout

```
<carpeta-padre>/
├── {{PROJECT_NAME}}.Harness/        ← este repo, el que se abre para trabajo con IA
│   ├── CLAUDE.md                    ← clasificador + imports de rules/
│   ├── README.md                    ← qué es esto, estructura, cómo extenderlo
│   ├── ESTADO.md · VERSION · CHANGELOG.md
│   ├── repos/{<repo>,_template}.md
│   ├── rules/{workflow,commits,git-flow,versioning,decisions,comments,testing}.md
│   ├── deploy/README.md             ← si aplica
│   ├── herramientas/README.md       ← si aplica
│   ├── specs/{_template,active,archive}/   ← una carpeta por historia ({{TICKET_PREFIX}}-XXX)
│   └── .claude/
│       ├── settings.json            ← additionalDirectories + ask/deny (la barrera real)
│       ├── ARCHITECTURE.md          ← este archivo
│       ├── skills/{self-review,pr-review,developer,...}/SKILL.md
│       └── commands/{historia-iniciar,historia-aplicar,historia-cerrar}.md
└── <repo(s) de producto>/   (repo(s) git propio(s), con su CLAUDE.md de 2 líneas)
```

## 3. Cómo fluye una sesión

1. Se abre `claude` parado en `{{PROJECT_NAME}}.Harness/`. Carga `CLAUDE.md`, `settings.json`, las skills y los comandos.
2. `CLAUDE.md` clasifica el pedido según la tabla de `repos/<repo>.md` correspondiente.
3. Si el pedido es una historia/incidencia nueva: `/historia-iniciar {{TICKET_PREFIX}}-XXX <desc>` (crea el spec, completa `proposal.md` y `tasks.md`, con OK humano entre pasos).
4. Para ejecutar lo ya aprobado: `/historia-aplicar <spec>` — toca código de producto vía `additionalDirectories`, corre tests si aplica.
5. Antes de dar por terminado o abrir el PR: skill `/self-review`. Al asignar un PR ajeno: skill `/pr-review`.
6. Al aprobarse el PR: `/historia-cerrar <spec>` — versiona (`rules/versioning.md`), mergea (`rules/git-flow.md`), refleja el estado en el sistema de seguimiento (si aplica) y archiva el spec.
7. Cualquier decisión no trivial (ambigüedad, algo difícil de revertir) frena y se pregunta al usuario — nunca se decide solo (`rules/decisions.md`).

## 4. Piezas

### 4.1 `CLAUDE.md` (clasificador)
Importa `rules/*.md` siempre. No es una wiki: el detalle técnico de cada repo vive en `repos/<repo>.md`, y se lee según la clasificación, no de memoria.

### 4.2 Skills (`.claude/skills/`)
Viven acá porque Claude Code las escanea al abrir este repo como cwd — no hay instalación separada que sincronizar. `self-review` (autochequeo del autor), `pr-review` (revisor asignado), `developer` (implementación) — sumar skills específicas de tecnología/dominio según haga falta.

### 4.3 Slash commands (`.claude/commands/`)
`historia-iniciar` / `historia-aplicar` / `historia-cerrar` formalizan los pasos de `rules/workflow.md` como comandos explícitos en vez de prosa que hay que recordar seguir.

### 4.4 Barrera: `settings.json`
| Bloque | Contenido | Efecto |
|---|---|---|
| `permissions.additionalDirectories` | repo(s) de producto hermanos | Lee/edita esos repos sin pedir permiso de directorio |
| `permissions.ask` | `git push`, `git reset --hard`, `git branch -D`, `rm -rf`/`Remove-Item -Recurse` (Bash y PowerShell) | Toda operación destructiva/irreversible pide confirmación |
| `permissions.deny` | `**/.env`, `**/.env.*` | No se puede leer `.env` de ningún repo accesible |

### 4.5 Specs (`specs/`)
Una carpeta por ticket `{{TICKET_PREFIX}}-XXX` (ver `rules/workflow.md`), de `active/` a `archive/` al cerrar. El ID es el mismo del sistema de seguimiento de tickets real del proyecto.

## 5. Qué se ejecuta, cuándo

| Momento | Qué corre | Dónde está definido |
|---|---|---|
| Abrir sesión en el harness | `CLAUDE.md`, `settings.json`, skills, comandos | raíz y `.claude/` |
| `/historia-*` | El comando correspondiente | `.claude/commands/` |
| `/self-review`, `/pr-review`, `/developer`, ... | La skill correspondiente | `.claude/skills/` |
| `git push`, `git reset --hard`, borrado recursivo | Pide confirmación (`ask`) | `.claude/settings.json` |
| Leer un `.env*` | Denegado | `.claude/settings.json` |
| Aprobar un PR | Versionar + mergear | `rules/versioning.md` + `rules/git-flow.md` |

## 6. Límite

Todo lo de `.claude/` (skills, comandos, `settings.json`) aplica **solo si Claude se abre parado en este repo**. Si se abre directo en un repo de producto, su `CLAUDE.md` de 2 líneas sigue importando las reglas en texto (`rules/*.md`) como fallback, pero sin skills, sin comandos y sin `additionalDirectories`/enforcement.
