# Agent Brief — LikePlatform Sprint 0

**Rol:** Agente de desarrollo. Ejecutar Sprint 0 del ecosistema LikePlatform.  
**Objetivo:** Crear 6 repositorios con estructura inicial, configuración, documentación y contratos. **Sin implementación de lógica de negocio.**

---

## Contexto del Proyecto

**LikePlatform** es una plataforma web basada en Laravel 13 que proporciona autenticación, gestión de usuarios, API keys, dashboard extensible, webhooks, portal de IA y sistema de plugins. Es el producto principal de **Like Innovación**.

**Stack definitivo:**
- PHP ^8.4, Laravel 13, Livewire 4, Flux UI 2 (free tier), Tailwind CSS 4
- Pest 4, Spatie Laravel Permission ^7.0, Laravel AI SDK (nativo)
- Autenticación: Laravel Fortify + Passkeys (nativo L13)

---

## Documentos de Referencia

Lee estos documentos ANTES de comenzar. Están en `docs/sprint0/`:

| Documento | Qué contiene |
|---|---|
| `CONVENTIONS.md` | Reglas de código, naming, commits, versionado |
| `CONTRACTS_SPEC.md` | Las 11 interfaces PHP a implementar en contracts |
| `SKILLS_OUTLINE.md` | Contenido de las 7 skills para dev-tools |
| `REGISTRY_DATA.md` | JSONs completos para el registry |
| `repos/repo-spec-*.md` | Especificación por repositorio (6 archivos) |

---

## Orden de Ejecución ESTRICTO

```
1. likeplatform-dev-tools     ← Base de herramientas
2. likeplatform-contracts     ← Interfaces compartidas
3. likeplatform-registry      ← Metadata del ecosistema
4. likeplatform-core          ← Laravel 13 fresco
5. likeplatform-webhooks      ← Package estructura
6. likeplatform-ai            ← Package estructura
```

**IMPORTANTE:** Cada repositorio se crea como directorio separado en el mismo nivel:
```
/home/luinux/Code/
├── likeplatform-boilerplate/     ← Este repo (docs y coordinación)
├── likeplatform-dev-tools/
├── likeplatform-contracts/
├── likeplatform-registry/
├── likeplatform-core/
├── likeplatform-webhooks/
└── likeplatform-ai/
```

**Propietario GitHub:** `luinuxscl` (cuenta personal).

---

## Instrucciones Generales

### Lo que DEBES hacer:
1. Leer el repo-spec correspondiente ANTES de crear cada repositorio
2. Seguir las convenciones de `CONVENTIONS.md` en todo momento
3. Usar Conventional Commits: `feat:`, `fix:`, `docs:`, `chore:`, etc.
4. Cada repo debe tener `README.md` con descripción, instalación y uso
5. Cada repo con código PHP debe tener `ARCHITECTURE.md`
6. Inicializar cada repo con `git init`, branch `main`
7. Crear repo en GitHub con `gh repo create luinuxscl/likeplatform-{name} --public`
8. Usar PHP 8.4+ features: readonly properties, enums, match expressions, named arguments
9. Usar PHP Attributes donde Laravel 13 los soporte (controllers, models, middleware)
10. Documentar PHPDoc en inglés, README/docs en español

### Lo que NO debes hacer:
1. NO implementar lógica de negocio (solo estructura y configuración)
2. NO usar `spatie/laravel-permission` v6 (DEBE ser ^7.0)
3. NO crear `TenantResolverContract` (eliminado, no hay multi-tenancy)
4. NO declarar `"php": "^8.3"` (usar `"php": "^8.4"`)
5. NO duplicar funcionalidad del AI SDK nativo de Laravel 13
6. NO usar componentes Flux UI Pro (solo free tier)
7. NO crear la organización `likeinnovacion` (se usa cuenta personal)

---

## Estrategia de Componentes UI Custom

Flux UI free solo incluye: Button, Dropdown, Icon, Separator, Tooltip.

Para componentes Pro (Modal, Table, Sidebar, Input, Select, etc.), el core DEBE crear versiones custom en `resources/views/components/ui/` usando:
- Blade Components
- Tailwind CSS 4 para estilos
- Alpine.js (incluido con Livewire) para interactividad
- API similar a Flux Pro para facilitar migración futura

**En Sprint 0 solo se crea la estructura de carpetas para estos componentes. La implementación es de Fase 1.**

---

## Criterios de Aceptación

- [ ] Los 6 repos existen como directorios con `git init` + remote en GitHub
- [ ] Cada repo tiene `composer.json` con nombre, dependencias y autoload correctos
- [ ] Cada repo tiene `README.md` descriptivo en español
- [ ] `likeplatform-contracts` tiene las 11 interfaces con PHPDoc completo
- [ ] `likeplatform-contracts` pasa `StructureTest` con Pest 4
- [ ] `likeplatform-registry` tiene `packages.json`, `contracts-usage.json` y schemas
- [ ] `likeplatform-dev-tools` tiene 7 skills y 3 scripts
- [ ] `likeplatform-core` tiene Laravel 13 instalado y composer.json configurado
- [ ] `likeplatform-webhooks` y `likeplatform-ai` tienen ServiceProvider vacío
- [ ] Todos los repos con PHP tienen configs de Pint y Pest desde dev-tools
- [ ] Todos los repos tienen ARCHITECTURE.md cuando aplica

---

## Workflow Git por Repositorio

```bash
# Para cada repositorio:
mkdir likeplatform-{name}
cd likeplatform-{name}
git init -b main
# ... crear archivos ...
git add .
git commit -m "feat: initial Sprint 0 structure"
gh repo create luinuxscl/likeplatform-{name} --public --source=. --push
```

---

*Like Innovación — Powered by LikePlatform*
