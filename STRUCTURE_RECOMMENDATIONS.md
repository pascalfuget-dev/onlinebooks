# Recommandations pour la structure du workspace onlinebooks

## Structure recommandée pour un écosystème MuleSoft complet

```
onlinebooks/
├── onlinebooks.code-workspace
├── STRUCTURE_RECOMMENDATIONS.md
├── docs/                              # Documentation globale
│   ├── architecture.md
│   └── api-guidelines.md
│
├── shared/                            # Ressources partagées
│   ├── onlinebooks-types/            # Types RAML partagés ✅ EXISTANT
│   ├── common-policies/              # Politiques communes
│   └── shared-configs/               # Configurations partagées
│
├── apis/                             # Spécifications d'API
│   ├── system/                       # System APIs (SAPI)
│   │   ├── books-catalog-sapi/       # ✅ EXISTANT
│   │   ├── customers-sapi/
│   │   ├── orders-sapi/
│   │   └── inventory-sapi/
│   ├── process/                      # Process APIs (PAPI)
│   │   ├── order-management-papi/
│   │   └── customer-service-papi/
│   └── experience/                   # Experience APIs (EAPI)
│       ├── mobile-eapi/
│       └── web-portal-eapi/
│
└── implementations/                  # Applications Mule
    ├── system/                       # Implémentations SAPI
    │   ├── books-catalog-sapi-impl/
    │   ├── customers-sapi-impl/
    │   └── orders-sapi-impl/
    ├── process/                      # Implémentations PAPI
    │   ├── order-management-papi-impl/
    │   └── customer-service-papi-impl/
    └── experience/                   # Implémentations EAPI
        ├── mobile-eapi-impl/
        └── web-portal-eapi-impl/
```

## Actions recommandées

### 1. Réorganisation immédiate (optionnelle)
- Déplacer `onlinebooks-types/` vers `shared/onlinebooks-types/`
- Déplacer `books-catalog-sapi/` vers `apis/system/books-catalog-sapi/`

### 2. Structure pour les nouveaux projets

#### Pour une nouvelle API (exemple: customers-sapi) :
```
apis/system/customers-sapi/
├── customers-sapi.raml
├── exchange.json
├── examples/
│   ├── customer-request.json
│   └── customer-response.json
└── traits/
    └── common-traits.raml
```

#### Pour une implémentation (exemple: books-catalog-sapi-impl) :
```
implementations/system/books-catalog-sapi-impl/
├── pom.xml
├── mule-artifact.json
├── src/
│   ├── main/
│   │   ├── mule/
│   │   │   ├── global.xml
│   │   │   └── books-catalog-flow.xml
│   │   └── resources/
│   │       ├── application.yaml
│   │       ├── application-dev.yaml
│   │       └── application-prod.yaml
│   └── test/
│       └── munit/
└── exchange.json
```

### 3. Configuration workspace mise à jour

Le fichier `onlinebooks.code-workspace` devrait inclure tous les projets :
```json
{
  "folders": [
    { "path": "." },
    { "path": "shared/onlinebooks-types" },
    { "path": "apis/system/books-catalog-sapi" },
    { "path": "implementations/system/books-catalog-sapi-impl" }
  ]
}
```

## Avantages de cette structure

### ✅ Séparation claire des responsabilités
- **APIs** : Spécifications uniquement
- **Implementations** : Code Mule séparé
- **Shared** : Ressources réutilisables

### ✅ Scalabilité
- Facilite l'ajout de nouvelles APIs et implémentations
- Structure par couche (System/Process/Experience)

### ✅ Maintenance
- Gestion indépendante des versions
- Tests et déploiements séparés
- Collaboration équipe optimisée

### ✅ Conformité MuleSoft
- Respect de l'architecture API-Led Connectivity
- Bonnes pratiques Exchange et Anypoint Platform

## Prochaines étapes recommandées

1. **Garder la structure actuelle** (elle fonctionne bien)
2. **Ajouter progressivement** selon le modèle recommandé
3. **Créer la première implémentation** : `books-catalog-sapi-impl`
4. **Établir des conventions** de nommage et de versioning