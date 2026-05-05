# Repo Spec: likeplatform-dev-tools

**Orden de ejecución:** Paso 1 (primero de todos)  
**Tipo:** Tooling  
**GitHub:** `luinuxscl/likeplatform-dev-tools`

---

## Estructura

```
likeplatform-dev-tools/
├── skills/
│   ├── skill-package-init.md
│   ├── skill-contract-implementation.md
│   ├── skill-registration.md
│   ├── skill-testing.md
│   ├── skill-versioning.md
│   ├── skill-laravel-conventions.md
│   └── skill-git-workflow.md
├── scripts/
│   ├── init-package.sh
│   ├── validate-contracts.sh
│   └── update-registry.sh
├── configs/
│   ├── phpstan.neon
│   ├── pint.json
│   ├── pest.xml
│   └── .editorconfig
├── VERSION
├── README.md
└── composer.json
```

## composer.json

```json
{
    "name": "likeplatform/dev-tools",
    "description": "Herramientas de desarrollo y configuraciones base para el ecosistema LikePlatform",
    "type": "library",
    "license": "MIT",
    "version": "0.1.0",
    "require": {
        "php": "^8.4"
    }
}
```

## Contenido de configs/

### pint.json
```json
{
    "preset": "laravel",
    "rules": {
        "declare_strict_types": true,
        "final_class": false,
        "ordered_class_elements": true,
        "single_line_empty_body": true
    }
}
```

### phpstan.neon
```neon
includes:
    - ./vendor/larastan/larastan/extension.neon

parameters:
    level: 6
    paths:
        - app
        - src
    excludePaths:
        - vendor
```

### pest.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="vendor/phpunit/phpunit/phpunit.xsd"
         bootstrap="vendor/autoload.php"
         colors="true">
    <testsuites>
        <testsuite name="Unit">
            <directory>tests/Unit</directory>
        </testsuite>
        <testsuite name="Feature">
            <directory>tests/Feature</directory>
        </testsuite>
    </testsuites>
    <source>
        <include>
            <directory>app</directory>
            <directory>src</directory>
        </include>
    </source>
</phpunit>
```

### .editorconfig
```ini
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 4
indent_style = space
insert_final_newline = true
trim_trailing_whitespace = true

[*.md]
trim_trailing_whitespace = false

[*.{yml,yaml}]
indent_size = 2

[*.json]
indent_size = 2
```

## Skills

Ver `SKILLS_OUTLINE.md` para contenido detallado de cada skill.

## Scripts

Los 3 scripts deben ser ejecutables (`chmod +x`):

- `init-package.sh`: recibe nombre, crea estructura completa de package
- `validate-contracts.sh`: valida que todas las interfaces tienen `declare(strict_types=1)`, PHPDoc, y son interfaces (no clases)
- `update-registry.sh`: recibe nombre y versión, actualiza JSONs del registry

## README.md

Debe contener:
- Descripción del propósito del repo
- Cómo usar las skills (copiar al contexto del agente)
- Cómo ejecutar los scripts
- Cómo copiar configs a un nuevo package
- Versión actual

## Criterios de aceptación
- [ ] 7 skills como markdown completo y accionable
- [ ] 3 scripts ejecutables y probados
- [ ] 4 archivos de configuración
- [ ] composer.json válido
- [ ] README.md en español
- [ ] VERSION con "0.1.0"
