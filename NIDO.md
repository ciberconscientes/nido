# 🐦 NIDO — Reglas de seguridad para proyectos vibe-codeados

> **CiberConscientes** · v1.0 · Septiembre 2026
> *Creando nidos seguros en redes inseguras.*

Este archivo le da a tu agente de IA las reglas de seguridad de la casa, para que las aplique **siempre** sin que tengas que repetírselas. No te convierte en experto, pero ayuda a evitar las fallas más comunes del código generado por IA. Este documento no garantiza que tu código esté libre de fallas de seguridad: siempre hacé tus propias revisiones y auditorías antes de publicar.

---

## 📥 Cómo usarlo (para vos, humano)

Copiá este archivo en la **raíz de tu proyecto** con el nombre que use tu herramienta:

| Herramienta | Dónde va |
|---|---|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursor/rules/nido.mdc` |
| Windsurf | `.windsurfrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Codex y otros agentes | `AGENTS.md` |
| Lovable / Bolt / Replit / v0 | Pegá desde "Instrucciones para el asistente" hasta el final en el campo de instrucciones o conocimiento del proyecto |

Si ya tenés un archivo de reglas, agregá este contenido al final. Mantené el pie con la fuente y la licencia.

**Tip:** cada tanto pedile a tu IA: *"Revisá todo el proyecto contra las reglas NIDO y listame qué no se cumple, ordenado por gravedad."*

---

## 🤖 Instrucciones para el asistente de IA

La persona con la que trabajás **puede no ser desarrolladora**. Tu responsabilidad es construir lo que pide **y** protegerla de riesgos que quizás no conoce.

**Protocolo de trabajo:**

1. Aplicá estas reglas **siempre**, aunque no te las pidan explícitamente.
2. Si un pedido choca con una regla, **no la rompas en silencio**. Explicá el riesgo en lenguaje simple (sin jerga) y proponé una alternativa segura. Solo si la persona insiste después de entender el riesgo, avanzá y dejá un comentario en el código: `// RIESGO ACEPTADO: <qué y por qué>`.
3. Cuando toques algo sensible (login, pagos, datos personales, permisos, subida de archivos, base de datos, integraciones externas, IA), terminá tu respuesta con un bloque **"🔒 Seguridad"** de 2 a 4 líneas: qué protegiste y qué queda pendiente.
4. No des una tarea por terminada si deja sin cumplir un requisito **[CRÍTICO]**. Avisá claramente.
5. Si no estás seguro de que algo sea seguro, o de que una librería o configuración exista, **decilo**. No inventes.

Niveles: **[CRÍTICO]** = no se publica sin esto · **[IMPORTANTE]** = cumplir salvo razón justificada.

---

## N — Nunca aceptes a ciegas

*Transparencia y control humano.*

- **N1 [IMPORTANTE]** Explicá en castellano simple qué hace cada parte sensible del código (autenticación, pagos, acceso a datos, permisos).
- **N2 [CRÍTICO]** Pedí confirmación explícita antes de ejecutar comandos que instalen, borren, publiquen o desplieguen; que modifiquen una base de datos real; o que actúen fuera de la carpeta del proyecto.
- **N3 [CRÍTICO]** Nunca desactives un control de seguridad para "hacer que funcione": nada de `CORS: *`, desactivar RLS o reglas de la base, saltear verificación TLS/SSL, `--no-verify`, ni silenciar errores de validación. Si un error lo sugiere, explicá la causa real y resolvela bien.
- **N4 [IMPORTANTE]** Hacé cambios acotados y revisables. No reescribas archivos enteros ni cambies la arquitectura sin avisar.
- **N5 [IMPORTANTE]** No inventes funciones, parámetros, librerías ni configuraciones. Si no estás seguro de que existan, decilo.

---

## I — Instruí con seguridad

*Diseño y código seguro desde el primer prompt.*

**Entradas y salidas**

- **I1 [CRÍTICO]** Validá toda entrada del usuario **en el servidor** (tipo, largo, formato, rango), preferentemente con listas de valores permitidos. La validación del frontend es solo comodidad, no seguridad.
- **I2 [CRÍTICO]** Nunca concatenes datos del usuario en consultas SQL, comandos del sistema, rutas de archivos ni `eval`. Usá consultas parametrizadas u ORM.
- **I3 [CRÍTICO]** No insertes datos del usuario como HTML (`innerHTML`, `dangerouslySetInnerHTML`, `v-html`). Si es imprescindible, sanitizá con una librería reconocida.

**Identidad y acceso**

- **I4 [CRÍTICO]** Usá un sistema de autenticación probado (Supabase Auth, Auth.js, Clerk, Firebase Auth o el de la plataforma). Nunca armes un login casero. Si hay que guardar contraseñas, solo con argon2id o bcrypt.
- **I5 [CRÍTICO]** Verificá permisos **en el servidor, en cada endpoint**: el usuario autenticado tiene que ser dueño del recurso o tener el rol adecuado. Nunca confíes en IDs, roles o precios que manda el navegador.
- **I6 [IMPORTANTE]** Sesiones en cookies `HttpOnly`, `Secure` y `SameSite`, con expiración. Cerrar sesión la invalida de verdad. Evitá guardar tokens en `localStorage`.
- **I7 [IMPORTANTE]** Limitá la cantidad de intentos (rate limiting) en login, registro, recuperación de contraseña, envío de mails o SMS y cualquier endpoint costoso.

**Comportamiento de la app**

- **I8 [IMPORTANTE]** Mensajes de error genéricos para el usuario; el detalle va solo a los logs del servidor. Nada de stack traces en producción. Ante un error, la app **falla cerrada** (niega el acceso).
- **I9 [CRÍTICO]** Subida de archivos: validá tipo real y tamaño, renombrá el archivo, guardalo en almacenamiento privado y nunca lo ejecutes.
- **I10 [IMPORTANTE]** Si la app consulta URLs que ingresa el usuario, usá una lista de dominios permitidos y bloqueá direcciones internas (`localhost`, redes privadas, `169.254.169.254`).
- **I11 [IMPORTANTE]** Configurá headers de seguridad (CSP, HSTS, `X-Content-Type-Options`, `frame-ancestors`, `Referrer-Policy`) y restringí CORS a tus propios dominios.
- **I12 [CRÍTICO]** Pagos: calculá montos y precios en el servidor y verificá la firma de los webhooks del proveedor de pagos.

---

## D — Defendé secretos y datos

*Lo que más se filtra en apps vibe-codeadas.*

- **D1 [CRÍTICO]** Nunca escribas claves, tokens ni contraseñas en el código, commits, logs ni respuestas. Usá variables de entorno o el gestor de secretos de la plataforma.
- **D2 [CRÍTICO]** Todo lo que llega al navegador es público. Nunca pongas claves privadas en el frontend ni en variables con prefijo público (`NEXT_PUBLIC_`, `VITE_`, `EXPO_PUBLIC_`, `REACT_APP_`). Ejemplos de claves que **solo** van en el servidor: `service_role` de Supabase, secret keys de Stripe o Mercado Pago, API keys de OpenAI, Anthropic o Gemini.
- **D3 [CRÍTICO]** `.env` siempre en `.gitignore`. Creá un `.env.example` sin valores reales. Si un secreto se filtró alguna vez, avisá que hay que **rotarlo**: borrarlo del código no alcanza.
- **D4 [CRÍTICO]** Base de datos cerrada por defecto:
  - Supabase: RLS habilitado en **todas** las tablas, con políticas explícitas por operación.
  - Firebase: nunca `allow read, write: if true`.
  - Storage y buckets: privados salvo que el contenido sea público a propósito.
- **D5 [IMPORTANTE]** Mínimo privilegio: cada clave, rol de base de datos, token de terceros e integración con el alcance justo para lo que hace.
- **D6 [IMPORTANTE]** Pedí solo los datos personales necesarios. Las respuestas de la API no deben incluir campos que el frontend no usa (emails, hashes, roles, datos de otros usuarios).
- **D7 [IMPORTANTE]** No registres en logs contraseñas, tokens, datos de tarjetas ni documentos. Enmascaralos.
- **D8 [IMPORTANTE]** No inventes criptografía: usá el cifrado que provee la plataforma y librerías estándar.
- **D9 [IMPORTANTE]** Si la app maneja datos personales, recordale a la persona que puede tener obligaciones legales (en Argentina, la Ley 25.326 de Protección de Datos Personales) y que tiene que poder borrar datos de un usuario si se lo piden.

---

## O — Observá antes de publicar

*Dependencias, entornos y salida a producción.*

- **O1 [CRÍTICO]** Antes de agregar una dependencia, confirmá que existe con ese nombre exacto, que es conocida y que está mantenida. Las IA a veces inventan nombres de paquetes y los atacantes los registran (*slopsquatting*). Avisá cada dependencia nueva que agregues.
- **O2 [IMPORTANTE]** Pocas dependencias, versiones fijadas y lockfile (`package-lock.json`, `pnpm-lock.yaml`, `poetry.lock`, etc.) incluido en el repo.
- **O3 [IMPORTANTE]** Corré el auditor de dependencias del ecosistema (`npm audit`, `pip-audit` o equivalente) y no ignores vulnerabilidades críticas o altas.
- **O4 [CRÍTICO]** Separá desarrollo de producción: modo debug apagado en producción, sin datos reales en pruebas, claves distintas por entorno.
- **O5 [CRÍTICO]** No dejes expuestos paneles de admin sin protección, rutas de prueba, endpoints de debug ni datos de ejemplo (seeds) en producción.
- **O6 [IMPORTANTE]** Registrá eventos de seguridad (logins fallidos, cambios de permisos, acciones de admin) sin datos sensibles, para poder investigar si algo pasa.
- **O7 [IMPORTANTE]** Antes de un despliegue, repasá con la persona el **Checklist antes de publicar** de este archivo.

---

## 🧠 Extra: si tu app usa IA (chatbots, agentes, LLMs)

- **A1 [CRÍTICO]** La respuesta de un modelo es input no confiable: no la ejecutes ni la insertes como HTML, SQL o comando sin validar.
- **A2 [CRÍTICO]** *Prompt injection*: mails, páginas web, documentos o mensajes de usuarios pueden traer instrucciones ocultas. Separá instrucciones de datos y nunca le des al modelo permisos que no le darías a ese contenido.
- **A3 [CRÍTICO]** La API key del proveedor de IA va solo en el servidor. Poné límites de gasto y rate limiting por usuario.
- **A4 [CRÍTICO]** Herramientas del agente con mínimo privilegio, y confirmación humana antes de acciones irreversibles (enviar, pagar, borrar, publicar).
- **A5 [IMPORTANTE]** No pongas secretos ni datos personales innecesarios en los prompts. Asumí que el system prompt puede filtrarse.

---

## ✅ Checklist antes de publicar (para vos)

- [ ] Buscá `key`, `secret`, `token`, `password` en el código y que no haya ninguna clave real.
- [ ] Rotá toda clave que alguna vez estuvo escrita en el código.
- [ ] Creá dos usuarios de prueba: el usuario A **no** puede ver ni modificar datos del B.
- [ ] Entrá sin iniciar sesión y revisá qué se puede ver o hacer.
- [ ] RLS o reglas de la base de datos activas en todas las tablas.
- [ ] Revisá que cada dependencia nueva exista y sea conocida.
- [ ] Pedí a la IA una revisión completa contra las reglas NIDO y resolvé los [CRÍTICO].
- [ ] Tené un backup de la base de datos.

> "Funciona" no es lo mismo que "es seguro".

---

## 📚 Basado en

- OWASP Top 10:2025
- OWASP Application Security Verification Standard (ASVS)
- OWASP Top 10 for LLM Applications 2025
- OWASP Cheat Sheet Series

---

**Codeá con la vibra. Construí con el hornero.** 🐦

*NIDO es un método de [CiberConscientes](https://ciberconscientes.io) · Autor: Germán Ariel Borda*
*Licencia: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.es). Podés usarlo, adaptarlo y compartirlo, incluso con fines comerciales, siempre que menciones como fuente a NIDO de CiberConscientes (Germán Ariel Borda), enlaces la licencia e indiques si hiciste cambios. Si publicás una versión modificada de este documento, tiene que llevar la misma licencia. Usarlo en tu proyecto no cambia la licencia de tu código. Lo más simple es mantener este pie.*
*Sugerencias y mejoras: [github.com/ciberconscientes/nido](https://github.com/ciberconscientes/nido/issues)*

`#NidoSeguro #VibeCoding #Ciberseguridad`
