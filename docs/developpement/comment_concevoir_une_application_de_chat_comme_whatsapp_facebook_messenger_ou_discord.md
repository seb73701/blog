# COMMENT CONCEVOIR UNE APPLICATION DE CHAT COMME WHATSAPP, FACEBOOK MESSENGER OU DISCORD ?

Le diagramme ci-dessous montre une conception pour une application de chat 1 à 1 simplifiée.

![Logo](../_media/developpement/how_to_design_a_chat_application.jpg ':size=700')

## Flux de connexion de l'utilisateur

- **Etape 1**: Alice se connecte à l'application de chat et établit une connexion par socket web avec le serveur.
- **Etapes 2-4**: Le service de présence reçoit la notification d'Alice, met à jour sa présence et informe les amis d'Alice de sa présence.

## Flux des messages

- **Etapes 1-2**: Alice envoie un message de discussion à Bob. Le message de conversation est acheminé vers le service de conversation **A**.
- **Etapes 3-4**: Le message de chat est envoyé au service de séquençage, qui génère un identifiant unique, et est conservé dans le magasin de messages.
- **Etape 5**: Le message de chat est envoyé à la file d'attente de synchronisation des messages pour être synchronisé avec le service de chat de Bob.
- **Etape 6**: Avant de transmettre le message, le service de synchronisation des messages vérifie la présence de Bob :
  - a) Si Bob est **en ligne**, le message de chat est envoyé au service de chat **B**.
  - b) Si Bob est **non connecté**, le message est envoyé au serveur push et poussé vers l'appareil de Bob.
- **Etapes 7-8**: Si Bob est **en ligne**, le message de chat est transmis à Bob via le socket web.

---

_Source : https://blog.bytebytego.com/i/98040721/how-do-we-design-a-chat-application-like-whatsapp-facebook-messenger-or-discord_
