# Versionado

Antes de mergear una historia/incidencia a la rama principal, bumpear la versión en cada repo/paquete que la historia haya tocado:

{{VERSIONING_TARGETS}}

Escala según el alcance real del ticket (no hay convención semver estricta previa en el proyecto, pero se respeta la lógica general):

- **Patch**: fix sin funcionalidad nueva ni cambio de comportamiento visible para el usuario.
- **Minor**: funcionalidad nueva o cambio de comportamiento no disruptivo.
- **Major**: cambio que rompe compatibilidad (contrato de API externo, dato existente, flujo ya en uso).

Si el ticket toca varios repos/paquetes, bumpear en todos los afectados — no necesariamente al mismo número, cada uno versiona por separado.

Si hay ambigüedad real sobre la escala (ver `decisions.md`), preguntar antes de decidir.
