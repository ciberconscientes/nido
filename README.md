# 🐦 NIDO — Seguridad para proyectos vibe-codeados

> *Creando nidos seguros en redes inseguras.*

**NIDO** es un conjunto de reglas de seguridad, en castellano y en lenguaje simple, para quienes construyen apps con IA (Claude Code, Cursor, Lovable, Bolt, Replit, v0, Copilot y otros), sepan programar o no.

Copiás un archivo en tu proyecto y tu asistente de IA empieza a aplicar las reglas **siempre**, sin que tengas que repetírselas.

## El método

| | | |
|---|---|---|
| **N** | Nunca aceptes a ciegas | La IA es un junior brillante, no un oráculo. |
| **I** | Instruí con seguridad | Reglas claras desde el primer prompt. |
| **D** | Defendé secretos y datos | Claves fuera del código, base de datos cerrada por defecto. |
| **O** | Observá antes de publicar | Dependencias, entornos y checklist final. |

## Cómo usarlo

1. Descargá [`NIDO.md`](NIDO.md).
2. Copialo en la raíz de tu proyecto con el nombre que use tu herramienta:

| Herramienta | Dónde va |
|---|---|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursor/rules/nido.mdc` |
| Windsurf | `.windsurfrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Codex y otros agentes | `AGENTS.md` |
| Lovable / Bolt / Replit / v0 | Pegá el contenido en las instrucciones o el conocimiento del proyecto |

3. Antes de publicar, pedile a tu IA: *"Revisá todo el proyecto contra las reglas NIDO y listame qué no se cumple, ordenado por gravedad."*

> ⚠️ NIDO reduce los errores más comunes, pero **no garantiza** que tu app sea segura. Siempre hacé tus propias revisiones y auditorías antes de publicar.

## Qué viene

- **NIDO Diseño:** un modelado de amenazas por entrevista, en lenguaje simple, que genera reglas específicas para tu proyecto.
- **NIDO Verificación:** un agente auditor independiente que revisa el código contra las reglas antes de publicar.

## Contribuir

¿Encontraste algo para mejorar o una regla que falta? Abrí un [issue](https://github.com/ciberconscientes/nido/issues).

## Licencia

[CC BY-SA 4.0](LICENSE). Podés usarlo, adaptarlo y compartirlo citando la fuente. Si publicás una versión modificada, tiene que llevar la misma licencia. Usarlo en tu proyecto no cambia la licencia de tu código.

---

Un proyecto de [CiberConscientes](https://ciberconscientes.io) · Germán Ariel Borda · Basado en OWASP

**Codeá con la vibra. Construí con el hornero.** 🐦
