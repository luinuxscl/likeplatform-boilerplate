# Contenido de Skills — likeplatform-dev-tools

Descripción detallada del contenido esperado para cada skill.

---

## 1. skill-package-init.md

**Título:** Crear un nuevo package para LikePlatform

**Contenido:**
1. Requisitos previos (PHP 8.4, Composer, Pest 4)
2. Clonar/crear estructura base:
   ```
   likeplatform-{name}/
   ├── src/Providers/{Name}ServiceProvider.php
   ├── src/Http/Controllers/
   ├── src/Models/
   ├── routes/{name}.php
   ├── database/migrations/
   ├── tests/
   ├── composer.json
   ├── README.md
   └── ARCHITECTURE.md
   ```
3. Configurar `composer.json`:
   - name: `likeplatform/{name}`
   - type: `library`
   - require: `php ^8.4`, `likeplatform/contracts ^0.1.0`
   - autoload PSR-4: `LikePlatform\{Name}\`
4. Crear ServiceProvider vacío que:
   - Registre rutas desde `routes/{name}.php`
   - Registre migraciones desde `database/migrations/`
   - Publique configuración si aplica
5. Inicializar git con branch `main`
6. Registrar en `likeplatform-registry` (packages.json)
7. Checklist de validación

---

## 2. skill-contract-implementation.md

**Título:** Implementar un contrato de likeplatform-contracts

**Contenido:**
1. Identificar el contrato a implementar desde `CONTRACTS_SPEC.md`
2. Crear la clase concreta en el namespace del package
3. Implementar todos los métodos con type hints estrictos
4. Registrar la implementación en el ServiceProvider:
   ```php
   $this->app->bind(WidgetContract::class, ConcreteWidget::class);
   ```
5. Crear test unitario que verifique:
   - La clase implementa la interfaz
   - Cada método retorna el tipo correcto
   - Lógica de negocio funciona correctamente
6. Actualizar `contracts-usage.json` en el registry
7. Ejemplo completo con WidgetContract

---

## 3. skill-registration.md

**Título:** Registrar extensiones en el core de LikePlatform

**Contenido:**
1. Sistema de registries del core:
   - `WidgetRegistry`: registrar widgets para dashboard
   - `SidebarRegistry`: registrar items de navegación
   - `ApiPermissionRegistry`: registrar permisos de API
   - `ThemeRegistry`: registrar temas de color
   - `WebhookEventRegistry`: registrar eventos de webhook
2. Cómo registrar desde un ServiceProvider:
   ```php
   public function boot(): void
   {
       $this->app->make(WidgetRegistry::class)->register(
           new MyWidget()
       );
       
       $this->app->make(SidebarRegistry::class)->register(
           new MySidebarItem()
       );
   }
   ```
3. Registrar rutas adicionales
4. Registrar migraciones
5. Registrar traducciones
6. Ejemplo completo: package ficticio que registra todo

---

## 4. skill-testing.md

**Título:** Estándares de testing con Pest 4

**Contenido:**
1. Estructura de tests: `Unit/`, `Feature/`, `Pest.php`
2. Convenciones de naming: `it('does something specific')`
3. Testing de contratos (mock de interfaces):
   ```php
   it('dispatches webhook', function () {
       $dispatcher = Mockery::mock(WebhookDispatcherContract::class);
       $dispatcher->shouldReceive('dispatch')
           ->once()
           ->with('user.created', ['id' => 1]);
       // ...
   });
   ```
4. Testing de Livewire components con Pest
5. Testing de API endpoints
6. Cobertura mínima esperada: 80% en lógica de negocio
7. Ejecutar tests: `vendor/bin/pest --compact`
8. Browser testing con Pest 4 (Playwright)
9. Datasets y lazy datasets

---

## 5. skill-versioning.md

**Título:** Versionado semántico para packages LikePlatform

**Contenido:**
1. Reglas de MAJOR.MINOR.PATCH
2. Cuándo incrementar cada nivel:
   - MAJOR: cambio en interfaz de contrato, eliminar método público
   - MINOR: nuevo contrato, nueva funcionalidad compatible
   - PATCH: bugfix, mejora de rendimiento, docs
3. Archivo VERSION en raíz de cada repo
4. Declarar compatibilidad con core en composer.json
5. Tags de git: `git tag v0.1.0`
6. Relación con contracts (si contracts sube MAJOR, packages deben adaptar)
7. Pre-releases: `0.1.0-dev`, `0.1.0-beta.1`
8. Flujo de release completo

---

## 6. skill-laravel-conventions.md

**Título:** Convenciones Laravel para LikePlatform

**Contenido:**
1. PHP 8.4 features obligatorias:
   - `declare(strict_types=1)`
   - Readonly properties, constructor promotion
   - Enums, match expressions, named arguments
   - PHP Attributes (Laravel 13)
2. Estructura de carpetas (Models, Actions, Services, Enums)
3. Naming de clases y archivos
4. Uso de Livewire 4 + Volt:
   - Single-file components cuando sea apropiado
   - Full components para lógica compleja
5. Uso de Flux UI (free tier) + componentes custom
6. Internacionalización con `__()` y archivos `lang/`
7. Form Requests para validación
8. Policies para autorización
9. Ejemplo: crear un CRUD completo siguiendo convenciones

---

## 7. skill-git-workflow.md

**Título:** Flujo de Git y colaboración

**Contenido:**
1. Branch principal: `main` (protegida)
2. Crear branch: `feature/`, `fix/`, `release/`, `hotfix/`
3. Conventional Commits obligatorios
4. Flujo de PR:
   - Crear branch desde main
   - Desarrollar con commits pequeños
   - Ejecutar quality gate: `vendor/bin/pint --dirty --format agent` + `php artisan test --compact`
   - Actualizar VERSION (patch +1)
   - Push y crear PR
5. Merge: squash merge a main
6. Release: crear tag, actualizar registry
7. Herramientas: usar `gh` CLI para PRs, issues, releases
8. `.gitignore` base para packages y core

---

## Scripts (contenido resumido)

### scripts/init-package.sh
Script interactivo que:
- Recibe nombre del package como argumento
- Crea la estructura de carpetas completa
- Genera composer.json con datos correctos
- Genera ServiceProvider vacío
- Genera README.md y ARCHITECTURE.md plantilla
- Inicializa git con branch main
- Copia configs (pint.json, pest.xml) al package

### scripts/validate-contracts.sh
Script que:
- Recorre todos los archivos PHP en likeplatform-contracts/src/
- Verifica que cada archivo declara una interfaz (no clase)
- Verifica que tiene `declare(strict_types=1)`
- Verifica que tiene PHPDoc
- Reporta resultados

### scripts/update-registry.sh
Script que:
- Recibe nombre del package y versión
- Actualiza packages.json en el registry
- Actualiza contracts-usage.json si hay nuevos contratos implementados
- Valida contra JSON schemas
- Muestra diff de cambios

---

*Like Innovación — Powered by LikePlatform*
