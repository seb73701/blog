---
title: SSO entre Azure et AWS
description: 
published: true
date: 2023-04-21T10:25:54.629Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:25:50.508Z
---

# SSO entre Azure et AWS
Voici la procédure permettant l’authentification SSO sur AWS avec votre compte Azure AD.

Tout d’abord, il faut activer AWS SSO sur votre portail AWS puis cliquer sur Choisir votre Source d’identité

![image-1.png](/assets/img/clouds/tempo/image-1.png)

Cliquer ensuite sur Modifier au niveau de Source d’identité

![image-2.png](/assets/img/clouds/tempo/image-2.png)

Télécharger le fichier de Métadata

![image-3.png](/assets/img/clouds/tempo/image-3.png)

Aller ensuite sur Azure puis cliquer sur Active Directory puis cliquer sur New Application

![image-4.png](/assets/img/clouds/tempo/image-4.png)

Sélectionner Non-Gallery application

![image-5.png](/assets/img/clouds/tempo/image-5.png)

Donner lui un nom puis cliquer sur Add

![image-6.png](/assets/img/clouds/tempo/image-6.png)

Cliquer sur Assign users and group

![image-7.png](/assets/img/clouds/tempo/image-7.png)

Assignez-lui des utilisateurs

![image-8.png](/assets/img/clouds/tempo/image-8.png)

Cliquer ensuite sur SAML

![image-9.png](/assets/img/clouds/tempo/image-9.png)

Uploader le fichier des Métadata téléchargé précédemment depuis AWS

![image-10.png](/assets/img/clouds/tempo/image-10.png)

Pour cela, cliquer sur Upload métadata file puis valider par Add

![image-11.png](/assets/img/clouds/tempo/image-11.png)

Télécharger celui de Azure pour l’uploader sur AWS en cliquant sur Federation metadata XML

![image-12.png](/assets/img/clouds/tempo/image-12.png)

Retourner sur AWS

![image-13.png](/assets/img/clouds/tempo/image-13.png)

Cliquer sur Parcourir pour importer le fichier de Metadata puis cliquer sur Suivant.

Retourner sur Azure

![image-14.png](/assets/img/clouds/tempo/image-14.png)

Dans l’application, cliquer sur Provisionning puis cliquer sur Get Started

![image-15.png](/assets/img/clouds/tempo/image-15.png)

Sélectionner Automatique

Pour récupérer les informations Tenant URL et Secret Token, il faut aller sur AWS

![image-16.png](/assets/img/clouds/tempo/image-16.png)

Au niveau du portail AWS SSO, cliquer sur Mise en service SCIM

![image-17.png](/assets/img/clouds/tempo/image-17.png)


Le champ TENANT URL du portail AZURE correspond à l’URL du point de terminaison du SCIM

Le secret Token correspond au jeton d’accès à générer

![image-18.png](/assets/img/clouds/tempo/image-18.png)

Activer ensuite le Provisionning


![image-19.png](/assets/img/clouds/tempo/image-19.png)

Attention, si vous avez une erreur de formatage, faites bien attention à ce que les noms et prénoms de vos utilisateurs soient correctement renseignés comme ci-dessous


![image-20.png](/assets/img/clouds/tempo/image-20.png)

Ceux -ci sont automatiquement créés sur AWS

![image-21.png](/assets/img/clouds/tempo/image-21.png)

Vous pouvez tester l’authentification Azure en utilisant le lien généré sur votre portail AWS SSO

![image-22.png](/assets/img/clouds/tempo/image-22.png)

source = https://blog.ippon.fr/tag/azure/