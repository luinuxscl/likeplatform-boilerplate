# Datos del Registry — LikePlatform

Contenido JSON completo y corregido para `likeplatform-registry`.

---

## packages.json

```json
{
  "ecosystem": "likeplatform",
  "version": "0.1.0",
  "maintainer": "luinuxscl",
  "packages": {
    "likeplatform/contracts": {
      "repo": "luinuxscl/likeplatform-contracts",
      "current": "0.1.0",
      "type": "shared",
      "status": "active",
      "php": "^8.4"
    },
    "likeplatform/core": {
      "repo": "luinuxscl/likeplatform-core",
      "current": "0.1.0",
      "type": "core",
      "status": "development",
      "php": "^8.4",
      "requires": {
        "likeplatform/contracts": "^0.1.0",
        "laravel/framework": "^13.0",
        "spatie/laravel-permission": "^7.0",
        "livewire/livewire": "^4.0",
        "livewire/flux": "^2.0"
      }
    },
    "likeplatform/webhooks": {
      "repo": "luinuxscl/likeplatform-webhooks",
      "current": "0.1.0-dev",
      "type": "package",
      "status": "development",
      "php": "^8.4",
      "requires": {
        "likeplatform/contracts": "^0.1.0"
      },
      "compatible_core": ">=0.1.0",
      "implements_contracts": [
        "WebhookDispatcherContract",
        "WebhookReceiverContract",
        "WebhookEventContract"
      ]
    },
    "likeplatform/ai": {
      "repo": "luinuxscl/likeplatform-ai",
      "current": "0.1.0-dev",
      "type": "package",
      "status": "development",
      "php": "^8.4",
      "requires": {
        "likeplatform/contracts": "^0.1.0"
      },
      "compatible_core": ">=0.1.0",
      "implements_contracts": [
        "AIProviderContract",
        "PromptTemplateContract",
        "AIUsageTrackerContract"
      ]
    },
    "likeplatform/dev-tools": {
      "repo": "luinuxscl/likeplatform-dev-tools",
      "current": "0.1.0",
      "type": "tooling",
      "status": "active"
    }
  }
}
```

---

## contracts-usage.json

```json
{
  "version": "0.1.0",
  "contracts": {
    "WidgetContract": {
      "namespace": "LikePlatform\\Contracts\\Widgets\\WidgetContract",
      "implemented_by": ["likeplatform/core"],
      "required_by": []
    },
    "SidebarItemContract": {
      "namespace": "LikePlatform\\Contracts\\Sidebar\\SidebarItemContract",
      "implemented_by": ["likeplatform/core"],
      "required_by": []
    },
    "ApiPermissionContract": {
      "namespace": "LikePlatform\\Contracts\\Permissions\\ApiPermissionContract",
      "implemented_by": ["likeplatform/core"],
      "required_by": []
    },
    "ThemeContract": {
      "namespace": "LikePlatform\\Contracts\\Themes\\ThemeContract",
      "implemented_by": ["likeplatform/core"],
      "required_by": []
    },
    "WebhookDispatcherContract": {
      "namespace": "LikePlatform\\Contracts\\Webhooks\\WebhookDispatcherContract",
      "implemented_by": ["likeplatform/webhooks"],
      "required_by": ["likeplatform/core"]
    },
    "WebhookReceiverContract": {
      "namespace": "LikePlatform\\Contracts\\Webhooks\\WebhookReceiverContract",
      "implemented_by": ["likeplatform/webhooks"],
      "required_by": []
    },
    "WebhookEventContract": {
      "namespace": "LikePlatform\\Contracts\\Webhooks\\WebhookEventContract",
      "implemented_by": ["likeplatform/webhooks"],
      "required_by": ["likeplatform/core"]
    },
    "AIProviderContract": {
      "namespace": "LikePlatform\\Contracts\\AI\\AIProviderContract",
      "implemented_by": ["likeplatform/ai"],
      "required_by": ["likeplatform/core"]
    },
    "PromptTemplateContract": {
      "namespace": "LikePlatform\\Contracts\\AI\\PromptTemplateContract",
      "implemented_by": ["likeplatform/ai"],
      "required_by": []
    },
    "AIUsageTrackerContract": {
      "namespace": "LikePlatform\\Contracts\\AI\\AIUsageTrackerContract",
      "implemented_by": ["likeplatform/ai"],
      "required_by": ["likeplatform/core"]
    },
    "UserResolverContract": {
      "namespace": "LikePlatform\\Contracts\\Core\\UserResolverContract",
      "implemented_by": ["likeplatform/core"],
      "required_by": ["likeplatform/webhooks", "likeplatform/ai"]
    }
  }
}
```

---

## compatibility-matrix.json

```json
{
  "version": "0.1.0",
  "matrix": {
    "likeplatform/core@0.1.0": {
      "php": "^8.4",
      "laravel": "^13.0",
      "likeplatform/contracts": "^0.1.0",
      "spatie/laravel-permission": "^7.0",
      "livewire/livewire": "^4.0",
      "livewire/flux": "^2.0"
    },
    "likeplatform/webhooks@0.1.0-dev": {
      "php": "^8.4",
      "likeplatform/contracts": "^0.1.0",
      "likeplatform/core": ">=0.1.0"
    },
    "likeplatform/ai@0.1.0-dev": {
      "php": "^8.4",
      "likeplatform/contracts": "^0.1.0",
      "likeplatform/core": ">=0.1.0"
    }
  }
}
```

---

## releases/core/v0.1.0.json

```json
{
  "package": "likeplatform/core",
  "version": "0.1.0",
  "date": "2026-05-01",
  "status": "development",
  "changelog": "Sprint 0: Estructura inicial del proyecto con Laravel 13",
  "dependencies": {
    "php": "^8.4",
    "laravel/framework": "^13.0",
    "likeplatform/contracts": "^0.1.0",
    "spatie/laravel-permission": "^7.0",
    "livewire/livewire": "^4.0",
    "livewire/flux": "^2.0"
  }
}
```

---

## schema/packages-schema.json

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "LikePlatform Packages Registry",
  "type": "object",
  "required": ["ecosystem", "version", "packages"],
  "properties": {
    "ecosystem": { "type": "string", "const": "likeplatform" },
    "version": { "type": "string", "pattern": "^\\d+\\.\\d+\\.\\d+$" },
    "maintainer": { "type": "string" },
    "packages": {
      "type": "object",
      "additionalProperties": {
        "type": "object",
        "required": ["repo", "current", "type", "status"],
        "properties": {
          "repo": { "type": "string" },
          "current": { "type": "string" },
          "type": { "type": "string", "enum": ["core", "shared", "package", "tooling", "meta"] },
          "status": { "type": "string", "enum": ["active", "development", "deprecated"] },
          "php": { "type": "string" },
          "requires": { "type": "object" },
          "compatible_core": { "type": "string" },
          "implements_contracts": { "type": "array", "items": { "type": "string" } }
        }
      }
    }
  }
}
```

---

## schema/compatibility-schema.json

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "LikePlatform Compatibility Matrix",
  "type": "object",
  "required": ["version", "matrix"],
  "properties": {
    "version": { "type": "string", "pattern": "^\\d+\\.\\d+\\.\\d+$" },
    "matrix": {
      "type": "object",
      "additionalProperties": {
        "type": "object",
        "additionalProperties": { "type": "string" }
      }
    }
  }
}
```
