# pre-deploy-audit

> Atrapar lo que pasa todas las pruebas pero rompe o expone producción.

**Auditoría previa al despliegue** · Skill para agentes de código de la comunidad [nvvdev](https://github.com/nvv-dev). Funciona en Claude Code como plugin o como skill suelta, y en cualquier agente compatible con el formato abierto [`SKILL.md`](https://agentskills.io).

*English summary at the end.*

## Qué hace

Busca bloqueantes antes de un release: secretos en el repo o en el artefacto, configuraciones de prueba o depuración en release, servicios escuchando donde no deben, variables faltantes, migraciones destructivas y versiones sin subir. Luego arma un plan de despliegue con staging, health check y rollback listo. No despliega por su cuenta.

## Objetivo

Releases sin sorpresas y con un camino claro de vuelta atrás.

## Cuándo usarla

- Antes de compilar un release o desplegar
- Antes de promover de staging a producción

## Cuándo no

- Builds de desarrollo

## Ejemplo

```text
Haz la auditoría previa al despliegue de la versión 4.0 de la app.
```

## Cómo se activa

Esta skill **se activa sola**: Claude Code lee su descripción y la carga cuando la tarea coincide. También puedes pedirla por nombre ("usa la skill pre-deploy-audit") o invocarla con `/pre-deploy-audit:pre-deploy-audit` (plugin) o `/pre-deploy-audit` (copiada a `.claude/skills/`).

## Instalación

### Opción 1: Claude Code, como plugin (recomendada)

Dentro de Claude Code:

```text
/plugin marketplace add nvv-dev/claude-plugins
/plugin install pre-deploy-audit@nvvdev
```

La primera línea agrega el catálogo de nvvdev una sola vez; la segunda instala esta skill. Si prefieres el paquete completo, instala `dev-workflow@nvvdev` en su lugar (incluye esta skill y las demás de [dev-workflow](https://github.com/nvv-dev/dev-workflow)); no instales los dos a la vez.

### Opción 2: Claude Code, copiando la skill

Para un solo proyecto (se comparte con tu equipo por git):

```bash
git clone --depth 1 https://github.com/nvv-dev/pre-deploy-audit.git /tmp/pre-deploy-audit
mkdir -p .claude/skills
cp -R /tmp/pre-deploy-audit/skills/pre-deploy-audit .claude/skills/
```

Para todos tus proyectos, cópiala en `~/.claude/skills/` en lugar de `.claude/skills/`.

### Opción 3: otros agentes

Copia la carpeta `skills/pre-deploy-audit/` en la carpeta de skills de tu agente (cada agente documenta la suya). Con la CLI [skills](https://github.com/vercel-labs/skills) de Vercel Labs:

```bash
DISABLE_TELEMETRY=1 npx skills add nvv-dev/pre-deploy-audit
```

### Verificar

Abre una sesión nueva y pregunta "¿qué skills tienes disponibles?"; `pre-deploy-audit` debe aparecer. Con el plugin, `/plugin` la muestra como instalada.

### Actualizar o desinstalar

- Plugin: `/plugin marketplace update nvvdev` para actualizar; `/plugin uninstall pre-deploy-audit@nvvdev` para quitarla.
- Copia manual: repite la copia para actualizar, o borra la carpeta `.claude/skills/pre-deploy-audit`.

Antes de instalar cualquier skill, incluida esta, conviene leerla: es un solo archivo, [`skills/pre-deploy-audit/SKILL.md`](skills/pre-deploy-audit/SKILL.md). Ver [vetting-external-skills](https://github.com/nvv-dev/vetting-external-skills).

## Contenido

| Ruta | Qué es |
|---|---|
| `skills/pre-deploy-audit/SKILL.md` | La skill: instrucciones en inglés para que los escáneres de seguridad las analicen completas; el agente responde en tu idioma |
| `.claude-plugin/plugin.json` | Manifiesto para instalarla como plugin |
| `.github/workflows/` | Validación de estructura y escaneo con SkillSpector en cada cambio |

## Contribuir

Este repo se **genera** desde [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow), donde vive la versión canónica. Abre issues y PRs allí. La mejor contribución es un caso real en la sección *Known failures* de la skill, con la regla que lo evita.

## Origen

Escrita desde cero por nvvdev. Inspirada en: [Hainrixz/all-deploy](https://github.com/Hainrixz/all-deploy) (pre-deploy audit, preview → health check → production).

## Licencia

MIT.

---

## English

**pre-deploy-audit** — Release blockers (secrets, debug settings, missing config, unsafe migrations) and a deploy plan with health check and rollback.

Install in Claude Code with `/plugin marketplace add nvv-dev/claude-plugins` and `/plugin install pre-deploy-audit@nvvdev`, or copy `skills/pre-deploy-audit/` into `.claude/skills/` (project) or `~/.claude/skills/` (personal). Other agents: copy the folder into their skills directory, or run `DISABLE_TELEMETRY=1 npx skills add nvv-dev/pre-deploy-audit`. Loads automatically when the task matches its description. Canonical source: [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow). License: MIT.
