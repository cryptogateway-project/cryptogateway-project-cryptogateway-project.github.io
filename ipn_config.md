---
layout: home
title: Configuration d'ipn 
weight: 3
---

# Configuration d'ipn
L'IPN est un mécanisme essentiel dans le domaine des transactions en ligne, permettant une transmission instantanée de données entre notre plateforme et votre système lorsqu'une transaction financière a lieu et lorsqu'elle change d'état. Cette documentation détaille les étapes nécessaires pour configurer l'IPN. La configuration de l'IPN requiert deux éléments cruciaux:

premièrement, l'URL de callback, qui est l'adresse vers laquelle nos serveurs enverront des notifications instantanées de chaque transaction. Assurez-vous de définir précisément cette URL pour garantir une réception efficace des notifications.

Deuxièmement, un secret, qui est nécessaire pour signer les données échangées pendant le processus d'IPN. Voici comment faire :

1. Connectez-vous à votre [dashboard](https://pay.izichange.com/login).
2. Accédez à la section "Paramètres API" puis sur "Information générale".
3. Cliquez sur paramétrer au niveau de la section Paramétrage IPN".
![image du dashboard pour la configuration de l'ipn](/cryptogateway-project/assets/images/ipn1.png)
4. Remplissez les champs et enregistrez les:
![image du dashboard pour la configuration de l'ipn](/cryptogateway-project/assets/images/ipn2.png)
