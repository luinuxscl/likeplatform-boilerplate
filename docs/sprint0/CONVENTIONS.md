# Convenciones — LikePlatform

Reglas obligatorias para todos los repositorios del ecosistema.

---

## 1. Naming

| Elemento | Convención | Ejemplo |
|---|---|---|
| Repositorio GitHub | `likeplatform-{nombre}` | `likeplatform-core` |
| Paquete Composer | `likeplatform/{nombre}` | `likeplatform/contracts` |
| Namespace PHP | `LikePlatform\{Nombre}` (PascalCase) | `LikePlatform\Webhooks` |
| Modelos Eloquent | Singular, PascalCase | `ApiKey`, `WebhookEndpoint` |
| Migraciones | `{fecha}_create_{tabla}_table.php` | `2026_05_01_create_api_keys_table.php` |
| Componentes Livewire | kebab-case en vista, PascalCase en clase | `user-list` → `UserList` |
| Componentes Blade | kebab-case con prefijo `ui.` | `<x-ui.modal>`, `<x-ui.table>` |
| Archivos de traducción | snake_case | `api_keys.php`, `dashboard.php` |
| Rutas | kebab-case, plurales | `/api-keys`, `/webhooks` |

## 2. Estructura de Archivos PHP

```
src/                          # (packages) o app/ (core)
├── Models/                   # Modelos Eloquent
├── Http/
│   ├── Controllers/          # Controllers
│   ├── Middleware/            # Middleware custom
│   └── Requests/             # Form Requests
├── Providers/                # Service Providers
├── Services/                 # Lógica de negocio
├── Actions/                  # Acciones (single-purpose classes)
├── Contracts/                # Solo en core: implementaciones locales
├── Registries/               # Registros de widgets, sidebar, etc.
└── Enums/                    # PHP 8.4 enums
```

## 3. PHP Moderno (8.4+)

Usar las siguientes features cuando aplique:

```php
// ✅ Readonly properties
public readonly string $key;

// ✅ Constructor promotion
public function __construct(
    private readonly string $name,
    private readonly string $email,
) {}

// ✅ Enums
enum UserRole: string {
    case Admin = 'admin';
    case Editor = 'editor';
    case User = 'user';
}

// ✅ Named arguments
$widget->register(
    key: 'welcome',
    component: 'widgets.welcome',
);

// ✅ Match expressions (en vez de switch)
$label = match($status) {
    'active' => __('Active'),
    'inactive' => __('Inactive'),
    default => __('Unknown'),
};

// ✅ PHP Attributes (Laravel 13)
#[Middleware('auth')]
#[Authorize('admin')]
class UserController extends Controller {}
```

## 4. Strict Types

Todos los archivos PHP DEBEN comenzar con:

```php
<?php

declare(strict_types=1);
```

## 5. Git

### Branches
| Tipo | Formato | Ejemplo |
|---|---|---|
| Feature | `feature/{descripción}` | `feature/user-management` |
| Fix | `fix/{descripción}` | `fix/api-key-validation` |
| Release | `release/v{versión}` | `release/v0.1.0` |
| Hotfix | `hotfix/{descripción}` | `hotfix/auth-bypass` |

### Commits (Conventional Commits)
```
feat(scope): descripción breve
fix(auth): corregir validación de 2FA
docs(readme): actualizar instrucciones de instalación
refactor(models): extraer lógica a Action class
test(api-keys): agregar tests de revocación
chore(deps): actualizar spatie/laravel-permission a v7.1
```

### Versionado Semántico
- **MAJOR:** Cambios incompatibles en API/contratos
- **MINOR:** Funcionalidad nueva compatible
- **PATCH:** Correcciones de bugs compatibles
- Actualizar archivo `VERSION` (patch +1) en cada commit

## 6. Testing (Pest 4)

```php
// Estructura de tests
tests/
├── Unit/           # Tests unitarios (sin framework)
├── Feature/        # Tests de integración (con framework)
└── Pest.php        # Configuración global de Pest
```

### Convenciones:
```php
// ✅ Usar it() con descripción clara
it('creates an api key for authenticated user', function () {
    // arrange, act, assert
});

// ✅ Usar datasets para variaciones
it('validates required fields', function (string $field) {
    // ...
})->with(['name', 'email', 'password']);

// ✅ Usar expect() para assertions
expect($user->apiKeys)->toHaveCount(1);
```

## 7. Internacionalización (i18n)

- **SIEMPRE** usar `__('key')` para strings en UI
- Mantener archivos en `lang/en/` y `lang/es/`
- Español es idioma por defecto de la plataforma
- Inglés disponible como segunda opción
- Labels de formularios, mensajes, tooltips: todo traducible

```php
// ✅ En Blade/Livewire
{{ __('dashboard.welcome') }}

// ✅ En PHP
return __('api_keys.created_successfully');
```

## 8. Estilo de Código

- **Formatter:** Laravel Pint (config en dev-tools)
- **Análisis estático:** PHPStan (config en dev-tools)
- Ejecutar antes de cada commit:
  ```bash
  vendor/bin/pint --dirty --format agent
  php artisan test --compact
  ```

## 9. Documentación

| Archivo | Contenido | Idioma |
|---|---|---|
| `README.md` | Descripción, instalación, uso básico | Español |
| `ARCHITECTURE.md` | Estructura, decisiones, extensibilidad | Español |
| PHPDoc | Parámetros, retornos, ejemplos | Inglés |
| Comentarios inline | Solo cuando el "por qué" no es obvio | Inglés |

## 10. Componentes UI Custom (Core)

Para componentes que reemplazan Flux Pro:

```
resources/views/components/ui/
├── modal.blade.php
├── table.blade.php
├── sidebar.blade.php
├── input.blade.php
├── textarea.blade.php
├── select.blade.php
├── switch.blade.php
├── tabs.blade.php
├── card.blade.php
├── badge.blade.php
├── toast.blade.php
└── slideover.blade.php
```

Cada componente debe:
- Usar Tailwind CSS 4 para estilos
- Usar Alpine.js para interactividad
- Aceptar props similares a la API de Flux UI
- Ser accesible (ARIA attributes)
- Soportar dark mode

---

*Like Innovación — Powered by LikePlatform*
