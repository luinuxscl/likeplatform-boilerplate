# Especificación de Contratos — LikePlatform

**Paquete:** `likeplatform/contracts`  
**Namespace:** `LikePlatform\Contracts`  
**Total:** 11 interfaces  
**Versión:** 0.1.0

---

## Reglas Generales

1. Cada interfaz vive en su propio subdirectorio de namespace
2. PHPDoc completo en inglés (nombre, descripción, parámetros, retornos)
3. Sin dependencias del framework Laravel excepto `illuminate/http` (para Request en WebhookReceiver)
4. Sin implementaciones concretas — solo interfaces
5. Usar type hints estrictos de PHP 8.4
6. `declare(strict_types=1);` en todos los archivos

---

## 1. Widgets/WidgetContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Widgets;

/**
 * Contract for dashboard widgets.
 *
 * Any package or the core can register widgets that appear on the dashboard.
 * Each widget provides a Livewire/Blade component and optional data.
 */
interface WidgetContract
{
    /**
     * Unique identifier for this widget.
     */
    public function key(): string;

    /**
     * Human-readable name of the widget.
     */
    public function name(): string;

    /**
     * Brief description of what this widget displays.
     */
    public function description(): string;

    /**
     * List of permission keys required to view this widget.
     *
     * @return array<string>
     */
    public function permissions(): array;

    /**
     * Fully qualified Livewire/Blade component name.
     *
     * Example: 'widgets.welcome' or 'likeplatform-webhooks::widgets.status'
     */
    public function component(): string;

    /**
     * Additional data passed to the component.
     *
     * @return array<string, mixed>
     */
    public function data(): array;
}
```

---

## 2. Sidebar/SidebarItemContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Sidebar;

/**
 * Contract for sidebar navigation items.
 *
 * Packages register sidebar items to extend the main navigation.
 * Items are rendered in order and filtered by user permissions.
 */
interface SidebarItemContract
{
    /**
     * Unique identifier for this sidebar item.
     */
    public function key(): string;

    /**
     * Display label (should be a translatable key).
     */
    public function label(): string;

    /**
     * Icon name (compatible with Flux UI icon set or Heroicons).
     */
    public function icon(): string;

    /**
     * Named route for this navigation item.
     */
    public function route(): string;

    /**
     * Permission keys required to see this item.
     *
     * @return array<string>
     */
    public function permissions(): array;

    /**
     * Sort order (lower numbers appear first).
     */
    public function order(): int;
}
```

---

## 3. Permissions/ApiPermissionContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Permissions;

/**
 * Contract for API key permissions.
 *
 * Each package can register additional permissions that can be assigned
 * to API keys created by users.
 */
interface ApiPermissionContract
{
    /**
     * Unique permission key (e.g., 'webhooks:read', 'ai:complete').
     */
    public function key(): string;

    /**
     * Human-readable permission name.
     */
    public function name(): string;

    /**
     * Description of what this permission grants access to.
     */
    public function description(): string;
}
```

---

## 4. Themes/ThemeContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Themes;

/**
 * Contract for color themes.
 *
 * The core ships with 5 default themes. Packages can register additional
 * themes that users can select in their appearance settings.
 */
interface ThemeContract
{
    /**
     * Unique identifier for this theme (e.g., 'ocean', 'forest').
     */
    public function key(): string;

    /**
     * Human-readable theme name.
     */
    public function name(): string;

    /**
     * CSS custom property values for this theme.
     *
     * Expected keys: 'primary', 'primary-hover', 'primary-foreground',
     * 'accent', 'background', 'surface', 'text', 'border'.
     *
     * Values should be valid CSS color values (HSL recommended).
     *
     * @return array<string, string>
     */
    public function colors(): array;
}
```

---

## 5. Webhooks/WebhookDispatcherContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Webhooks;

/**
 * Contract for dispatching outgoing webhooks.
 *
 * The core uses this contract to fire webhook events. The webhooks
 * package provides the concrete implementation.
 */
interface WebhookDispatcherContract
{
    /**
     * Dispatch a webhook event to all subscribed URLs.
     *
     * @param string $event Event name (e.g., 'user.created', 'api_key.revoked')
     * @param array<string, mixed> $payload Event payload data
     */
    public function dispatch(string $event, array $payload): void;

    /**
     * Get all URLs subscribed to a given event.
     *
     * @param string $event Event name
     * @return array<string> List of subscribed webhook URLs
     */
    public function getSubscribedUrls(string $event): array;
}
```

---

## 6. Webhooks/WebhookReceiverContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Webhooks;

use Illuminate\Http\Request;

/**
 * Contract for receiving and processing incoming webhooks.
 *
 * Implementations handle signature validation and payload processing
 * from external services.
 */
interface WebhookReceiverContract
{
    /**
     * Validate the HMAC signature of an incoming webhook request.
     */
    public function validateSignature(Request $request): bool;

    /**
     * Process a validated webhook payload.
     *
     * @param string $source Identifier of the webhook source
     * @param array<string, mixed> $payload Decoded payload data
     */
    public function process(string $source, array $payload): void;
}
```

---

## 7. Webhooks/WebhookEventContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Webhooks;

/**
 * Contract for defining webhook events that can be subscribed to.
 *
 * Each package registers the events it can emit. Users configure
 * which events trigger their webhook endpoints.
 */
interface WebhookEventContract
{
    /**
     * Unique event key (e.g., 'user.created', 'ai.completion.finished').
     */
    public function key(): string;

    /**
     * Human-readable event name.
     */
    public function name(): string;

    /**
     * Description of when this event fires.
     */
    public function description(): string;

    /**
     * JSON Schema-like definition of the event payload.
     *
     * @return array<string, mixed>
     */
    public function payloadSchema(): array;
}
```

---

## 8. AI/AIProviderContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\AI;

/**
 * Contract for AI provider integration.
 *
 * This contract wraps/extends Laravel 13's native AI SDK to provide
 * a LikePlatform-specific interface for AI completions.
 * Implementations should delegate to the native SDK rather than
 * reimplementing provider logic.
 */
interface AIProviderContract
{
    /**
     * Generate a text completion.
     *
     * @param string $prompt The input prompt
     * @param array<string, mixed> $options Provider-specific options
     *     (model, temperature, max_tokens, etc.)
     * @return string The generated completion text
     */
    public function complete(string $prompt, array $options = []): string;

    /**
     * Get available models for this provider.
     *
     * @return array<string> List of model identifiers
     */
    public function getModels(): array;

    /**
     * Calculate estimated cost for a completion.
     *
     * @param string $model Model identifier
     * @param int $inputTokens Number of input tokens
     * @param int $outputTokens Number of output tokens
     */
    public function calculateCost(string $model, int $inputTokens, int $outputTokens): float;

    /**
     * Get the provider name (e.g., 'openai', 'anthropic', 'gemini').
     */
    public function getProviderName(): string;
}
```

---

## 9. AI/PromptTemplateContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\AI;

/**
 * Contract for reusable prompt templates.
 *
 * Templates contain placeholders that are replaced at runtime
 * to generate contextualized prompts.
 */
interface PromptTemplateContract
{
    /**
     * Unique template identifier.
     */
    public function key(): string;

    /**
     * Human-readable template name.
     */
    public function name(): string;

    /**
     * The raw template string with {{placeholder}} syntax.
     */
    public function template(): string;

    /**
     * List of placeholder names expected by this template.
     *
     * @return array<string>
     */
    public function placeholders(): array;

    /**
     * Render the template by replacing placeholders with values.
     *
     * @param array<string, string> $values Map of placeholder => value
     * @return string The rendered prompt
     */
    public function render(array $values): string;
}
```

---

## 10. AI/AIUsageTrackerContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\AI;

/**
 * Contract for tracking AI usage and costs.
 *
 * Implementations persist usage data for reporting dashboards
 * with breakdowns by period, user, and provider.
 */
interface AIUsageTrackerContract
{
    /**
     * Record a usage event.
     *
     * @param string $provider Provider name
     * @param string $model Model identifier
     * @param int $tokens Total tokens consumed
     * @param float $cost Estimated cost in USD
     */
    public function track(string $provider, string $model, int $tokens, float $cost): void;

    /**
     * Get aggregated usage for a given period.
     *
     * @param string $period One of: 'day', 'week', 'month', 'year'
     * @return array<string, mixed> Usage statistics
     */
    public function getUsage(string $period): array;
}
```

---

## 11. Core/UserResolverContract.php

```php
<?php

declare(strict_types=1);

namespace LikePlatform\Contracts\Core;

use Illuminate\Contracts\Auth\Authenticatable;

/**
 * Contract for resolving the current authenticated user.
 *
 * Packages use this contract to access the current user without
 * depending on specific auth implementations.
 */
interface UserResolverContract
{
    /**
     * Get the currently authenticated user, or null if not authenticated.
     */
    public function getCurrentUser(): ?Authenticatable;
}
```

---

## Archivo de Tests: tests/StructureTest.php

```php
<?php

declare(strict_types=1);

use LikePlatform\Contracts\AI\AIProviderContract;
use LikePlatform\Contracts\AI\AIUsageTrackerContract;
use LikePlatform\Contracts\AI\PromptTemplateContract;
use LikePlatform\Contracts\Core\UserResolverContract;
use LikePlatform\Contracts\Permissions\ApiPermissionContract;
use LikePlatform\Contracts\Sidebar\SidebarItemContract;
use LikePlatform\Contracts\Themes\ThemeContract;
use LikePlatform\Contracts\Webhooks\WebhookDispatcherContract;
use LikePlatform\Contracts\Webhooks\WebhookEventContract;
use LikePlatform\Contracts\Webhooks\WebhookReceiverContract;
use LikePlatform\Contracts\Widgets\WidgetContract;

$contracts = [
    WidgetContract::class,
    SidebarItemContract::class,
    ApiPermissionContract::class,
    ThemeContract::class,
    WebhookDispatcherContract::class,
    WebhookReceiverContract::class,
    WebhookEventContract::class,
    AIProviderContract::class,
    PromptTemplateContract::class,
    AIUsageTrackerContract::class,
    UserResolverContract::class,
];

it('has exactly 11 contracts defined', function () use ($contracts) {
    expect($contracts)->toHaveCount(11);
});

foreach ($contracts as $contract) {
    it("defines {$contract} as an interface", function () use ($contract) {
        $reflection = new ReflectionClass($contract);
        expect($reflection->isInterface())->toBeTrue();
    });
}
```

---

## composer.json

```json
{
    "name": "likeplatform/contracts",
    "description": "Contratos e interfaces compartidas del ecosistema LikePlatform",
    "type": "library",
    "license": "MIT",
    "version": "0.1.0",
    "require": {
        "php": "^8.4",
        "illuminate/http": "^13.0",
        "illuminate/contracts": "^13.0"
    },
    "require-dev": {
        "pestphp/pest": "^4.0"
    },
    "autoload": {
        "psr-4": {
            "LikePlatform\\Contracts\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "LikePlatform\\Contracts\\Tests\\": "tests/"
        }
    },
    "minimum-stability": "stable"
}
```
