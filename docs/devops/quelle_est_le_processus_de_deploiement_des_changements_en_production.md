# QUELLE EST LE PROCESSUS DE DEPLOIEMENT DES CHANGEMENTS EN PRODUCTION

The diagram below shows several common deployment strategies.
Le diagramme ci-dessous montre plusieurs stratégies de déploiement courantes :

![strategies deploiement courantes](../_media/devops/strategies_deploiement_courantes.jpg ':size=700')

## Big Bang Deployment

Le **Big Bang Deployment _(déploiement Big Bang)_** est une stratégie assez simple, qui consiste à déployer une nouvelle version en une seule fois, sans interruption de service. La préparation est essentielle pour cette stratégie. Si le déploiement échoue, nous revenons à la version précédente.

- No downtime ❌
- Targeted users ❌

## Rolling Deployment

Le **Rolling Deployment _(déploiement progressif)_** s'effectue par étapes et non par à-coups. L'ensemble de l'usine est mis à niveau un par un sur une période donnée.

- No downtime ✅
- Targeted users ❌

## Blue-Green Deployment

Dans le cadre d'un **Blue-Green Deployment**, deux environnements sont déployés simultanément dans la production. L'équipe d'assurance qualité effectue divers tests sur l'environnement green. Une fois que l'environnement green a passé les tests avec succès, l'équilibreur de charge y transfère les utilisateurs.

- No downtime ✅
- Targeted users ❌

## Canary Deployment

Avec le **Canary Deployment _(déploiement canarien)_**, seule une petite partie des instances est mise à niveau avec la nouvelle version, une fois que tous les tests sont réussis, une partie des utilisateurs est acheminée vers les instances canariennes.

- No downtime ✅
- Targeted users ❌

## Feature Toggle

Avec la **Feature Toggle _(bascule de fonctionnalité)_**, une petite partie des utilisateurs ayant un drapeau spécifique passe par le code de la nouvelle fonctionnalité, tandis que les autres utilisateurs passent par le code normal. Cette méthode peut être utilisée en combinaison avec d'autres stratégies : soit la nouvelle branche du code est mise à jour en une seule fois, soit seules quelques instances sont mises à jour avec le nouveau code.

- No downtime ✅
- Targeted users ✅

---

_Source : https://blog.bytebytego.com/i/98040721/what-is-the-process-for-deploying-changes-to-production_   
