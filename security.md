---
layout: home
title: Procédure de Sécurité 
---

# Procédure de Sécurité

Notre plateforme utilise les méthodes de sécurité suivantes :

## JSON Web Tokens (JWT)

Nous utilisons JWT pour l'authentification des utilisateurs. Chaque utilisateur authentifié reçoit un jeton JWT qui est inclus dans les en-têtes de chaque requête subséquente pour vérifier l'identité de l'utilisateur.

## Clés API

Les requêtes entrantes vers notre API nécessitent l'utilisation de clés API. Chaque client autorisé reçoit une clé API unique qui doit être incluse dans le header `Authorization` des requêtes HTTP. Cela garantit une authentification sécurisée et contrôlée.

## Empreinte HMAC (Hash-based Message Authentication Code)

Pour garantir l'intégrité des données, nous utilisons une empreinte HMAC. Chaque requête inclut une empreinte HMAC générée à partir de son contenu. Le serveur peut ainsi vérifier que les données n'ont pas été altérées en transit.

## Authentification des Requêtes

Avant d'envoyer des requêtes à notre API, assurez-vous de suivre les procédures d'authentification appropriées :

## Utilisation de la Clé API

Incluez votre clé API dans le header `Authorization` de toutes les requêtes vers notre API. Assurez-vous que la clé API est générée et gérée correctement depuis la section dédiée de votre compte.


### 1. Générer une Clé API

Commencez par générer une clé API depuis votre application web. Accédez à la section "API Paramètres"  et suivez les instructions pour créer une nouvelle clé.

### 2. Utiliser la Clé API dans les Requêtes

Une fois que vous avez généré la clé API, vous pouvez l'utiliser pour authentifier vos requêtes API. Voici comment inclure la clé dans vos requêtes :

### Requête GET

```http
GET /api/exemple
Authorization: Bearer VotreCleApiKey
```




### Empreinte HMAC

Chaque requête doit inclure une empreinte HMAC dans le header `x-signature`. Assurez-vous de générer cette empreinte correctement à partir du contenu de votre requête pour garantir l'intégrité des données.
