---
layout: home
title: Procédure de Sécurité 
---

# Procédure de Sécurité

Notre plateforme utilise les méthodes de sécurité suivantes :

### JSON Web Tokens (JWT)

Nous utilisons JWT pour l'authentification des utilisateurs. Chaque utilisateur authentifié reçoit un jeton JWT qui est inclus dans les en-têtes de chaque requête subséquente pour vérifier l'identité de l'utilisateur.

### Clés API

Les requêtes entrantes vers notre API nécessitent l'utilisation de clés API. Chaque client autorisé reçoit une clé API unique qui doit être incluse dans le header `Authorization` des requêtes HTTP. Cela garantit une authentification sécurisée et contrôlée.

### Empreinte HMAC (Hash-based Message Authentication Code)

Pour garantir l'intégrité des données, nous utilisons une empreinte HMAC. Chaque requête inclut une empreinte HMAC générée à partir de son contenu. Le serveur peut ainsi vérifier que les données n'ont pas été altérées en transit.


