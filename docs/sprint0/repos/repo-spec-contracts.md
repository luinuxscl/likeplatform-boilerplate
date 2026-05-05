# Repo Spec: likeplatform-contracts

**Orden de ejecución:** Paso 2  
**Tipo:** Shared (biblioteca de interfaces)  
**GitHub:** `luinuxscl/likeplatform-contracts`  
**Namespace:** `LikePlatform\Contracts`

---

## Estructura

```
likeplatform-contracts/
├── src/
│   ├── Widgets/WidgetContract.php
│   ├── Sidebar/SidebarItemContract.php
│   ├── Permissions/ApiPermissionContract.php
│   ├── Themes/ThemeContract.php
│   ├── Webhooks/
│   │   ├── WebhookDispatcherContract.php
│   │   ├── WebhookReceiverContract.php
│   │   └── WebhookEventContract.php
│   ├── AI/
│   │   ├── AIProviderContract.php
│   │   ├── PromptTemplateContract.php
│   │   └── AIUsageTrackerContract.php
│   └── Core/
│       └── UserResolverContract.php
├── tests/
│   ├── StructureTest.php
│   └── Pest.php
├── VERSION
├── composer.json
├── README.md
└── ARCHITECTURE.md
```

## Interfaces

Ver `CONTRACTS_SPEC.md` para el código fuente completo de las 11 interfaces, tests y composer.json.

## ARCHITECTURE.md

Debe documentar:
1. Propósito de los contratos (desacoplar core de packages)
2. Cómo se versionan (semver, cambio de interfaz = MAJOR bump)
3. Cómo un package implementa un contrato:
   - Crear clase que implementa la interfaz
   - Registrar binding en ServiceProvider
   - Actualizar contracts-usage.json en registry
4. Regla: NUNCA agregar implementaciones concretas aquí
5. Lista de contratos organizados por dominio

## README.md

Debe contener:
- Descripción: "Interfaces compartidas del ecosistema LikePlatform"
- Instalación: `composer require likeplatform/contracts`
- Lista de contratos disponibles
- Ejemplo de implementación básica
- Enlace a ARCHITECTURE.md

## Criterios de aceptación
- [ ] 11 interfaces PHP con PHPDoc completo en inglés
- [ ] `declare(strict_types=1)` en todos los archivos
- [ ] Type hints estrictos en todos los métodos
- [ ] StructureTest pasa con `vendor/bin/pest`
- [ ] composer.json válido con `illuminate/http` y `illuminate/contracts`
- [ ] README.md y ARCHITECTURE.md en español
- [ ] VERSION con "0.1.0"
