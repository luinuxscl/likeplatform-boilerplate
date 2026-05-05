# Workflow de Desarrollo — LikePlatform

## Ciclo de Desarrollo por Fases

El desarrollo se organiza en fases. Cada fase contiene tareas atómicas listadas en [`PROGRESS.md`](./PROGRESS.md). Las tareas se ejecutan en el orden definido allí.

```
1. Leer PROGRESS.md → identificar tareas pendientes
2. Desarrollar tarea por tarea con commits atómicos
3. Al terminar la fase → gate de finalización
```

## Estrategia de Git

Una sola rama activa: `main`. No se usan feature branches ni PRs durante el desarrollo inicial. Los cambios van directo a `main` en cada repo.

```bash
# Ejemplo de commits
feat(core): add api-key index, create, and show views
feat(webhooks): add WebhookEndpointController
fix(ui): handle empty slot in sidebar layout
chore(deps): update spatie/laravel-permission to v7.1
```

### Repos y su responsabilidad

| Repo | Se commitea cuando... |
|------|----------------------|
| `likeplatform-core` | Cambios en modelos, controladores, vistas, rutas, config |
| `likeplatform-webhooks` | Cambios en el package de webhooks |
| `likeplatform-ai` | Cambios en el package de IA |
| `likeplatform-contracts` | Cambios en interfaces (MAJOR bump) |
| `likeplatform-dev-tools` | Nuevas skills, scripts, o configs |
| `likeplatform-registry` | Metadata de packages o releases |

## Gate de Finalización de Fase

Al completar **todas** las tareas de una fase, el agente DEBE:

1. **Preguntar al usuario:**
   > "Fase X — [nombre]: completada. ¿Commiteo, mergeo y documento?"

2. **Si el usuario acepta:**
   - Commits pendientes en cada repo modificado:
     ```bash
     cd likeplatform-{core,webhooks,ai} && git add . && git commit -m "feat(scope): descripción" && git push origin main
     ```
   - Actualizar `PROGRESS.md`:
     - Marcar tareas completadas como `completado`
     - Agregar nuevas tareas pendientes si surgieron
     - Actualizar la fecha de la fase
   - Commitear `PROGRESS.md` en el repo `likeplatform-boilerplate`

3. **Si el usuario rechaza:**
   - Continuar desarrollando las tareas restantes
   - No actualizar `PROGRESS.md` todavía

## Actualización de PROGRESS.md

`PROGRESS.md` es la fuente de verdad del avance del proyecto.

### Cuándo se actualiza
- **Solo al final de cada fase** — no en cada commit individual
- Al inicio de cada sesión, el agente lo lee para saber dónde quedó

### Formato
```markdown
## Fase N: Nombre de la fase

### Sección — Subsección

| Tarea | Estado |
|-------|--------|
| Descripción de la tarea | completado |
| Otra tarea pendiente | pendiente |
```

### Estados válidos
- `pendiente` — no empezada
- `en progreso` — actualmente en desarrollo
- `completado` — terminada y commiteada

## Ejemplo de Flujo Completo

```
[Agente] Leo PROGRESS.md → Fase 2 tiene 5 tareas pendientes
         Desarrollo tareas 1, 2, 3 → commits a likeplatform-core
         Desarrollo tarea 4 → commit a likeplatform-webhooks
         Desarrollo tarea 5 → commit a likeplatform-ai

[Agente] Fase 2 — Vistas y Controladores: completada (5/5).
         ¿Commiteo, mergeo y documento?

[Usuario] Sí

[Agente] > cd likeplatform-core && git add . && git commit -m "feat(api-keys): add index, create, show views" && git push
         > cd likeplatform-webhooks && git add . && git commit -m "feat(webhooks): add WebhookEndpointController" && git push
         > cd likeplatform-ai && git add . && git commit -m "feat(ai): add PromptTemplateController" && git push
         > Actualiza PROGRESS.md: marca 5 tareas como completado
         > cd likeplatform-boilerplate && git add PROGRESS.md && git commit -m "docs: update progress — Fase 2 done" && git push

[Agente] Listo. Fase 2 documentada. La Fase 3 tiene 8 tareas pendientes.
         ¿Continuamos?
```

## Convenciones de Commits

```
tipo(scope): descripción breve en español

feat(core): agregar vistas de api-keys
fix(ui): corregir error de variable $slot en layout
refactor(webhooks): extraer lógica de firma a trait
test(api-keys): agregar tests de revocación
chore(deps): actualizar livewire/flux a v2.1
docs(progress): actualizar PROGRESS.md — Fase 2
```

### Tipos válidos
| Tipo | Cuándo usar |
|------|-------------|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `refactor` | Reestructuración sin cambio de comportamiento |
| `test` | Tests |
| `chore` | Tareas de mantenimiento, dependencias |
| `docs` | Documentación (README, PROGRESS.md, ARCHITECTURE.md) |
| `style` | Formato, linting |

### Scopes válidos
`core`, `webhooks`, `ai`, `contracts`, `dev-tools`, `registry`, `ui`, `auth`, `api-keys`, `deps`, `progress`, `layout`

---

*Like Innovación — Powered by LikePlatform*
