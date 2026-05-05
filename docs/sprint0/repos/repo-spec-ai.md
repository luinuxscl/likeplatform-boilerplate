# Repo Spec: likeplatform-ai

**Orden de ejecución:** Paso 6 (último)  
**Tipo:** Package  
**GitHub:** `luinuxscl/likeplatform-ai`  
**Namespace:** `LikePlatform\AI`

---

## Estructura

```
likeplatform-ai/
├── src/
│   ├── Providers/
│   │   └── AIServiceProvider.php
│   ├── Http/
│   │   └── Controllers/.gitkeep
│   └── Models/.gitkeep
├── config/
│   └── ai.php
├── routes/
│   └── ai.php
├── database/
│   └── migrations/.gitkeep
├── tests/
│   ├── Unit/.gitkeep
│   ├── Feature/.gitkeep
│   └── Pest.php
├── lang/
│   ├── en/.gitkeep
│   └── es/.gitkeep
├── VERSION
├── composer.json
├── README.md
└── ARCHITECTURE.md
```

## composer.json

```json
{
    "name": "likeplatform/ai",
    "description": "Portal de IA para LikePlatform — Wrapper sobre Laravel AI SDK",
    "type": "library",
    "license": "MIT",
    "version": "0.1.0-dev",
    "require": {
        "php": "^8.4",
        "likeplatform/contracts": "^0.1.0"
    },
    "require-dev": {
        "pestphp/pest": "^4.0",
        "orchestra/testbench": "^10.0"
    },
    "autoload": {
        "psr-4": {
            "LikePlatform\\AI\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "LikePlatform\\AI\\Tests\\": "tests/"
        }
    },
    "extra": {
        "laravel": {
            "providers": [
                "LikePlatform\\AI\\Providers\\AIServiceProvider"
            ]
        }
    },
    "minimum-stability": "stable"
}
```

## AIServiceProvider.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\AI\Providers;

use Illuminate\Support\ServiceProvider;

/**
 * Service provider for the LikePlatform AI package.
 *
 * Wraps Laravel 13's native AI SDK to provide LikePlatform-specific
 * features: prompt templates, usage tracking, and cost reporting.
 */
class AIServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        $this->mergeConfigFrom(
            __DIR__.'/../../config/ai.php', 'likeplatform-ai'
        );
    }

    public function boot(): void
    {
        $this->loadRoutesFrom(__DIR__.'/../../routes/ai.php');
        $this->loadMigrationsFrom(__DIR__.'/../../database/migrations');
        $this->loadTranslationsFrom(__DIR__.'/../../lang', 'likeplatform-ai');

        if ($this->app->runningInConsole()) {
            $this->publishes([
                __DIR__.'/../../config/ai.php' => config_path('likeplatform-ai.php'),
            ], 'likeplatform-ai-config');
        }
    }
}
```

## config/ai.php

```php
<?php

declare(strict_types=1);

return [
    /*
    |--------------------------------------------------------------------------
    | AI Provider Configuration
    |--------------------------------------------------------------------------
    |
    | This package wraps Laravel 13's native AI SDK. Configure providers
    | and models via Laravel's built-in AI configuration.
    |
    | LikePlatform-specific settings (templates, usage tracking) are below.
    |
    */
];
```

## routes/ai.php

```php
<?php

declare(strict_types=1);

// AI routes will be registered here in Fase 1
```

## ARCHITECTURE.md

Debe documentar:
1. Relación con Laravel AI SDK nativo (wrapper, no reimplementación)
2. Contratos que implementará:
   - `AIProviderContract` — delega al AI SDK nativo
   - `PromptTemplateContract` — CRUD de plantillas
   - `AIUsageTrackerContract` — persistencia de métricas
3. Funcionalidades objetivo (Fase 1):
   - Completar textos usando providers configurados en Laravel
   - CRUD de plantillas de prompts con placeholders
   - Panel de estadísticas de uso (por día, semana, mes, usuario)
   - Desglose de costos por provider/model
4. NO duplicar: no reimplementar llamadas a APIs de providers

## Criterios de aceptación
- [ ] ServiceProvider funcional
- [ ] composer.json con package auto-discovery
- [ ] README.md y ARCHITECTURE.md en español
- [ ] ARCHITECTURE.md documenta integración con AI SDK nativo
- [ ] VERSION con "0.1.0-dev"
- [ ] Estructura de carpetas completa con .gitkeep
