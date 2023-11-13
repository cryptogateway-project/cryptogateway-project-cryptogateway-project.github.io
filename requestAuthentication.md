---
layout: home
title: Authentification des requêtes
---

# Authentification des Requêtes

Avant d'envoyer des requêtes à notre API, assurez-vous de suivre les procédures d'authentification appropriées :

### Utilisation de la Clé API

Incluez votre clé API dans le header `Authorization` de toutes les requêtes vers notre API. Assurez-vous que la clé API est générée et gérée correctement depuis la section dédiée de votre compte.


1. Générer une Clé API

    Commencez par générer une clé API depuis votre application web. Accédez à la section "API Paramètres"  et suivez les instructions pour créer une nouvelle clé.

2. Utiliser la Clé API dans les Requêtes

    Une fois que vous avez généré la clé API, vous pouvez l'utiliser pour authentifier vos requêtes API. Voici comment inclure la clé dans vos requêtes :



### Empreinte HMAC

Chaque requête doit inclure une empreinte HMAC dans le header `x-signature`. Assurez-vous de générer cette empreinte correctement à partir du contenu de votre requête pour garantir l'intégrité des données.
Assurez-vous de calculer la signature correctement à partir des données de la requête en utilisant l'algorithme SHA-256 avec le secret de signature approprié.

Vous devez calculer la signature des données en utilisant l'algorithme SHA-256 avec le secret de signature de la clé API (la clé secrète définie lors de la génération de la clé API).


**Exemple de requête :**

``` 
POST {{base_url}}/monendpoint
Authorization: Bearer VotreCleAPI
x-signature: SignatureDesDonnees
Content-Type: application/json
```