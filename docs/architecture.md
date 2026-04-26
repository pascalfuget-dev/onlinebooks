# Architecture du projet OnlineBooks

## Vue d'ensemble

Ce projet suit l'architecture API-Led Connectivity de MuleSoft avec une séparation claire entre les spécifications d'API et leurs implémentations.

## Structure du projet

### Shared Resources (`shared/`)
- **onlinebooks-types/** : Bibliothèque RAML contenant tous les types de données partagés (Book, Author, Customer, Order, Invoice, Stock)

### API Specifications (`apis/`)
- **system/** : System APIs (SAPI) - APIs de niveau système pour accéder aux données
  - **books-catalog-sapi/** : API pour la gestion du catalogue de livres

### Implementations (`implementations/`)
*À venir* - Implémentations Mule des spécifications d'API

## Conventions de nommage

- **APIs** : `[domain]-[layer]-api` (ex: books-catalog-sapi)
- **Implémentations** : `[domain]-[layer]-api-impl` (ex: books-catalog-sapi-impl)
- **Types partagés** : `[domain]-types` (ex: onlinebooks-types)

## Couches API

1. **System APIs (SAPI)** : Accès direct aux systèmes backend
2. **Process APIs (PAPI)** : Orchestration de processus métier
3. **Experience APIs (EAPI)** : APIs orientées expérience utilisateur

## Gestion des versions

- Versioning sémantique (Major.Minor.Patch)
- Versions indépendantes entre spécifications et implémentations
- Publication sur Anypoint Exchange