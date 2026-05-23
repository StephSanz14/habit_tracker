# AGENTS.md

Contrato operativo para agentes custom y developers humanos del proyecto
Habit Tracker. Este archivo define decisiones cerradas y libertades permitidas.

## Stack

- Next.js 15 con App Router.
- Supabase para Postgres, Auth y Storage.
- Vercel para deploy.
- TypeScript en modo estricto.
- Tailwind CSS para estilos.
- Cliente Supabase en navegador y SWR para data fetching, segun `spec.md`.

## Convenciones de TypeScript

- Mantener `strict` activo y tratar errores de tipos como bloqueantes.
- No usar `any` salvo justificacion explicita en `CONTEXT.md`.
- Preferir tipos concretos, `unknown` con narrowing, e interfaces simples.
- Modelar datos de Supabase con tipos alineados a migraciones y constraints.
- Evitar logica implicita: validar entradas antes de persistir o renderizar.
- Los componentes deben recibir props tipadas y devolver UI predecible.

## Estructura de carpetas esperada

- `app/`: rutas, layouts y paginas del App Router.
- `components/`: componentes reutilizables de UI.
- `lib/`: clientes, helpers, validaciones y calculos de dominio.
- `supabase/migrations/`: migraciones versionadas de base de datos.
- `docs/`: documentacion auxiliar del proyecto.
- `insumos/` y `prompts/`: material de trabajo, no fuente runtime.
- `.env.local`: variables locales requeridas, nunca commitear secretos.

## Politica de commits

- Un commit por unidad funcional o documental cerrada.
- Cada commit debe ser atomico, verificable y facil de revertir.
- Usar prefijos convencionales: `feat:`, `docs:`, `chore:`, `fix:`.
- No usar commits genericos como `implement everything` o `updates`.
- Antes de commitear, revisar diff y ejecutar las verificaciones acordadas.

## Flujo git

- `main` es estable y no recibe trabajo directo.
- `develop` es la rama de integracion.
- Cada unidad se trabaja en una rama tipada desde `develop`.
- Ramas permitidas: `feat/*`, `docs/*`, `chore/*`, `fix/*`.
- Al cerrar la unidad, mergear la rama tipada hacia `develop`.
- No borrar ni reescribir commits existentes sin instruccion explicita.
- Nada debe quedar sin commitear al cerrar una unidad de trabajo.

## Regla de CONTEXT.md

- Toda edicion manual de codigo debe registrarse en `CONTEXT.md`.
- El registro debe incluir que se cambio, por que se hizo y que riesgo cubre.
- No hace falta registrar cambios generados por un agente si estan en el plan.
- Si una decision se aparta del plan aprobado, documentarla antes de seguir.

## Prohibiciones explicitas

- No escribir codigo sin plan aprobado.
- No usar `any` sin justificacion documentada en `CONTEXT.md`.
- No introducir Material UI, Chakra u otra libreria pesada de componentes.
- No agregar ni exigir tests automatizados para este alcance.
- No crear backend REST propio si no esta aprobado en la spec.
- No ampliar scope con PWA, mobile nativo, gamificacion o notificaciones.
- No commitear secretos, `.env.local` ni credenciales.

## Libertades de decision

- Elegir nombres claros de funciones, componentes, variables y ramas.
- Organizar componentes pequenos si no cambia la arquitectura acordada.
- Ajustar clases Tailwind para una UI simple, legible y consistente.
- Hacer refactors minimos dentro de la unidad aprobada.
- Proponer cambios de scope, pero no implementarlos sin actualizar el plan.
