# Repo Spec: likeplatform-registry

**Orden de ejecución:** Paso 3  
**Tipo:** Meta (solo metadata, sin código ejecutable)  
**GitHub:** `luinuxscl/likeplatform-registry`

---

## Estructura

```
likeplatform-registry/
├── packages.json
├── compatibility-matrix.json
├── contracts-usage.json
├── releases/
│   ├── core/
│   │   └── v0.1.0.json
│   ├── webhooks/
│   │   └── .gitkeep
│   └── ai/
│       └── .gitkeep
├── schema/
│   ├── packages-schema.json
│   └── compatibility-schema.json
└── README.md
```

## Contenido

Ver `REGISTRY_DATA.md` para el contenido JSON completo de todos los archivos.

## Reglas

- **SIN** composer.json (no es un paquete PHP)
- **SIN** dependencias externas
- **SIN** código ejecutable
- Solo archivos JSON y markdown
- Los schemas JSON deben validar correctamente sus archivos correspondientes

## README.md

Debe contener:
1. Descripción: "Catálogo central del ecosistema LikePlatform"
2. Cómo consultar el registry (estructura de packages.json)
3. Cómo agregar un nuevo package (pasos con ejemplo)
4. Cómo validar compatibilidad (usando compatibility-matrix.json)
5. Cómo verificar qué contratos implementa cada package (contracts-usage.json)
6. Estructura de schemas y cómo validar

## Criterios de aceptación
- [ ] packages.json con 5 packages (contracts, core, webhooks, ai, dev-tools)
- [ ] contracts-usage.json con 11 contratos mapeados
- [ ] compatibility-matrix.json consistente con packages.json
- [ ] releases/core/v0.1.0.json con datos correctos
- [ ] 2 JSON schemas que validen packages.json y compatibility-matrix.json
- [ ] README.md en español
- [ ] Repos apuntan a `luinuxscl/` (no `likeinnovacion/`)
