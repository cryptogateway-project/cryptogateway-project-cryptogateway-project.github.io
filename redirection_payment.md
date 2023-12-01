---
layout: home
title: Payement par redirection
nav_order: 6
---

# Payement par redirection

Un paiement par redirection désigne un processus sécurisé où un utilisateur est dirigé vers une plateforme externe de traitement des paiements pour finaliser une transaction. Ce processus vous permet entant que marchand sur notre plateforme d'externaliser la gestion de la transaction à notre services de paiement spécialisés. L'utilisateur est redirigé vers notre interface de paiement sécurisé où il saisit ses informations, une fois les informations validées, notre système met à disposition de ce dernier une addresse de payement sur lequel il pourra vous payer en une ou plusieurs partie. dès lors que nous recevons un payement sur cet addresse une notification temps réelle lui est envoyé sur son interface de payement.
Une fois le paiement obtenue et confirmée, l'utilisateur est renvoyé vers le site d'origine pour recevoir une confirmation de la transaction. 

### Comment obtenir un lien de paiement ?

L'obtention d'un lien de payement se fait via l'Api, vous devez effectuer une requête HTTP POST à l'endpoint spécifié `/api/payements/generate_url`. Assurez-vous d'inclure les en-têtes nécessaires tels que `l'x-api-key` pour l'authentification API, et `l'x-signature` pour garantir l'intégrité des données.


**Description des paramètres**

| variables | type | description |
| --- | --- | --- |
| coin | string | le code de la cryptomonnaie dans laquelle vous souhaitez être payer par défaut |
| amount | decimal | montant de la transaction |
| acceptedCoins | array | Liste des autres cryptomonnaies dans lesquelles vous souhaiter être payer |
| successUrl | string | lien de redirection vers votre plateforme en cas de success |
| canceledUrl | string | lien de redirection vers votre plateforme en cas d'annulation de l'operation |
| failedUrl | string | lien de redirection vers votre plateforme en cas d'échec |

**Méthode HTTP :** POST

**Exemple de requête :**

``` http
curl --location --globoff '{{base_url}}/payements/generate_url' \
--header 'x-api-key: 6|skqZd0TvuSSfVluXVZd7wh3WN1pdAZFgImsLizKU289daa5d' \
--header 'x-signature: adae036371e5cb7ab6dc875b8febdca8561918b64d0a52a00c04b1cbd9b9dba6' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--data '{
    "coin":"trx",
    "amount":"2",
    "acceptedCoins":["TRX"],
    "successUrl":"https://google.com",
    "canceledUrl":"https://google.com",
    "failedUrl":"https://google.com"
}'

 ```

Assurez-vous de calculer la signature correctement à partir des données  
de la requête en utilisant l'algorithme SHA-256 avec le secret de  
signature approprié.