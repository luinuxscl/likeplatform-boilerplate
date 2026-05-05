# Repo Spec: likeplatform-webhooks

**Orden de ejecución:** Paso 5  
**Tipo:** Package  
**GitHub:** `luinuxscl/likeplatform-webhooks`  
**Namespace:** `LikePlatform\Webhooks`

---

## Estructura

```
likeplatform-webhooks/
├── src/
│   ├── Providers/
│   │   └── WebhooksServiceProvider.php
│   ├── Http/
│   │   └── Controllers/.gitkeep
│   └── Models/.gitkeep
├── config/
│   └── webhooks.php
├── routes/
│   └── webhooks.php
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
    "name": "likeplatform/webhooks",
    "description": "Package de webhooks salientes y entrantes para LikePlatform",
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
            "LikePlatform\\Webhooks\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "LikePlatform\\Webhooks\\Tests\\": "tests/"
        }
    },
    "extra": {
        "laravel": {
            "providers": [
                "LikePlatform\\Webhooks\\Providers\\WebhooksServiceProvider"
            ]
        }
    },
    "minimum-stability": "stable"
}
```

## WebhooksServiceProvider.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Webhooks\Providers;

use Illuminate\Support\ServiceProvider;

/**
 * Service provider for the LikePlatform Webhooks package.
 *
 * Registers routes, migrations, config, and translations.
 * Binds contract implementations in the container.
 */
class WebhooksServiceProvider extends ServiceProvider
{
    public function register(): void
    {
        $this->mergeConfigFrom(
            __DIR__.'/../../config/webhooks.php', 'likeplatform-webhooks'
        );
    }

    public function boot(): void
    {
        $this->loadRoutesFrom(__DIR__.'/../../routes/webhooks.php');
        $this->loadMigrationsFrom(__DIR__.'/../../database/migrations');
        $this->loadTranslationsFrom(__DIR__.'/../../lang', 'likeplatform-webhooks');

        if ($this->app->runningInConsole()) {
            $this->publishes([
                __DIR__.'/../../config/webhooks.php' => config_path('likeplatform-webhooks.php'),
            ], 'likeplatform-webhooks-config');
        }
    }
}
```

## config/webhooks.php

```php
<?php

declare(strict_types=1);

return [
    // Webhook configuration will be added in Fase 1
];
```

## routes/webhooks.php

```php
<?php

declare(strict_types=1);

// Webhook routes will be registered here in Fase 1
```

## Contratos que implementará (Fase 1)
- `WebhookDispatcherContract`
- `WebhookReceiverContract`
- `WebhookEventContract`

## Criterios de aceptación
- [ ] ServiceProvider funcional (registra config, rutas, migraciones, traducciones)
- [ ] composer.json con package auto-discovery configurado
- [ ] README.md y ARCHITECTURE.md en español
- [ ] VERSION con "0.1.0-dev"
- [ ] Estructura de carpetas completa con .gitkeep donde corresponda
