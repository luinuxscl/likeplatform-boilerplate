# Repo Spec: likeplatform-core

**Orden de ejecución:** Paso 4  
**Tipo:** Core (proyecto Laravel 13)  
**GitHub:** `luinuxscl/likeplatform-core`

---

## Instalación Base

```bash
composer create-project laravel/laravel likeplatform-core
```

Luego configurar composer.json con dependencias del ecosistema.

## Estructura (post-instalación)

Se agregan estos directorios/archivos sobre la instalación fresca de Laravel 13:

```
likeplatform-core/
├── app/
│   ├── Models/                  # (viene con User.php)
│   ├── Http/
│   │   ├── Controllers/
│   │   └── Middleware/
│   ├── Providers/
│   ├── Services/                # ← CREAR (lógica de negocio)
│   ├── Actions/                 # ← CREAR (acciones single-purpose)
│   ├── Enums/                   # ← CREAR (PHP 8.4 enums)
│   ├── Registries/              # ← CREAR (widget, sidebar, theme, etc.)
│   └── Contracts/               # ← CREAR (implementaciones de contratos)
├── resources/
│   └── views/
│       └── components/
│           └── ui/              # ← CREAR (componentes custom que reemplazan Flux Pro)
├── lang/
│   ├── en/                      # ← CREAR estructura
│   └── es/                      # ← CREAR estructura
├── config/
├── database/migrations/
├── routes/
├── tests/
│   ├── Unit/
│   └── Feature/
├── VERSION                      # ← CREAR con "0.1.0"
├── composer.json                # ← MODIFICAR
├── README.md                    # ← REESCRIBIR
├── ARCHITECTURE.md              # ← CREAR
└── phpunit.xml                  # (viene con Laravel)
```

## composer.json (completo)

```json
{
    "name": "likeplatform/core",
    "description": "LikePlatform — Plataforma web extensible con auth, gestión de usuarios, API keys, dashboard y sistema de plugins",
    "type": "project",
    "license": "MIT",
    "require": {
        "php": "^8.4",
        "laravel/framework": "^13.0",
        "laravel/fortify": "^1.0",
        "likeplatform/contracts": "^0.1.0",
        "spatie/laravel-permission": "^7.0",
        "livewire/livewire": "^4.0",
        "livewire/flux": "^2.0"
    },
    "require-dev": {
        "pestphp/pest": "^4.0",
        "pestphp/pest-plugin-laravel": "^3.0",
        "likeplatform/dev-tools": "^0.1.0",
        "laravel/pint": "^1.0"
    },
    "repositories": [
        {
            "type": "path",
            "url": "../likeplatform-contracts"
        },
        {
            "type": "path",
            "url": "../likeplatform-dev-tools"
        }
    ],
    "autoload": {
        "psr-4": {
            "App\\": "app/",
            "Database\\Factories\\": "database/factories/",
            "Database\\Seeders\\": "database/seeders/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
    "minimum-stability": "stable",
    "prefer-stable": true
}
```

## Carpetas Vacías a Crear (con .gitkeep)

```
app/Services/.gitkeep
app/Actions/.gitkeep
app/Enums/.gitkeep
app/Registries/.gitkeep
app/Contracts/.gitkeep
resources/views/components/ui/.gitkeep
lang/en/.gitkeep
lang/es/.gitkeep
```

## ARCHITECTURE.md

Debe documentar:
1. Descripción general de LikePlatform como producto
2. Stack tecnológico definitivo (con versiones correctas)
3. Sistema de extensibilidad:
   - Registries (WidgetRegistry, SidebarRegistry, ThemeRegistry, etc.)
   - Cómo un package se registra en el core
   - ServiceProvider discovery
4. Autenticación: Fortify + Passkeys (nativo L13)
5. Estrategia de componentes UI: Flux free + custom components
6. Internacionalización: ES/EN con `__()` y `lang/`
7. Diagrama de arquitectura:
   ```
   [Packages: webhooks, ai, ...]
         ↓ implements
   [likeplatform-contracts]
         ↓ defines interfaces for
   [likeplatform-core]
         ↓ extends via
   [Registries: widgets, sidebar, themes, permissions]
   ```

## README.md

Debe contener:
- Nombre: LikePlatform
- Marca: Like Innovación — Powered by LikePlatform
- Descripción del producto
- Requisitos (PHP 8.4, Composer, Node.js para Vite/Tailwind)
- Instalación paso a paso
- Configuración básica (.env)
- Stack tecnológico
- Estado actual: "Sprint 0 — Estructura inicial"

## IMPORTANTE

- **NO implementar lógica de negocio** en Sprint 0
- Solo crear la estructura, configurar dependencias y documentar
- El `composer install` debe completar sin errores
- Los contracts y dev-tools deben estar como repos path adyacentes

## Criterios de aceptación
- [ ] Laravel 13 instalado y funcional (`php artisan --version`)
- [ ] composer.json con todas las dependencias correctas
- [ ] Carpetas de extensibilidad creadas (Services, Actions, Enums, Registries, Contracts)
- [ ] Carpeta de componentes UI custom creada
- [ ] Estructura i18n (lang/en, lang/es) creada
- [ ] ARCHITECTURE.md completo
- [ ] README.md en español con marca Like Innovación
- [ ] VERSION con "0.1.0"
- [ ] `composer install` pasa sin errores (con repos path)
