# Description

L'attaque téléphonique regroupe le vishing (voice phishing), le smishing (SMS phishing) et les faux messages sur les applications de messagerie (WhatsApp, Telegram, etc.). L'attaquant se fait passer pour un tiers de confiance (support informatique, direction, banque, prestataire externe) pour obtenir des informations sensibles ou faire effectuer une action compromettant la sécurité.

# Mesures préventives

La prévention des attaques téléphoniques repose sur plusieurs piliers : une sensibilisation régulière des collaborateurs aux tactiques courantes, une politique de vérification des demandes sensibles, une procédure formalisée pour les demandes d'accès ou de modification système, et l'encouragement de signaler les appels ou messages suspects sans peur de représailles.

# Détection

Les tentatives d'attaque téléphonique présentent souvent des signaux d'alerte que les collaborateurs doivent apprendre à reconnaître.

### Vishing (Appels téléphoniques)

1. Urgence et pression temporelle : "Il y a une urgence sécurité", "Votre système va être verrouillé", "J'ai besoin de vérifier votre accès maintenant". L'attaquant crée un stress pour court-circuiter la réflexion.

2. Usurpation d'identité : L'appelant dit venir du support IT, de la direction, d'un prestataire connu ou même de la DSI. Avec les numéros VoIP, l'affichage peut être falsifié (spoofing) pour ressembler à un numéro interne.

3. Demande d'identifiants ou accès : "Pouvez-vous me donner votre mot de passe pour vérifier ?", "Dites-moi votre login pour tester l'accès", "J'ai besoin de votre code VPN". Aucun support légitime ne demande cela.

4. Prétexte plausible : L'attaquant mentionne des détails vrais obtenus publiquement (votre nom, votre service, un projet en cours, un fournisseur réel) pour gagner la confiance.

5. Demande d'accès ou action système : "Pouvez-vous relancer un service ?", "Pouvez-vous accepter une demande d'accès qui vient d'arriver ?", "Pouvez-vous télécharger cet outil de diagnostic ?"

6. Refus de vérification : Si vous demandez à vérifier l'identité ou à raccrocher pour rappeler le support IT officiel, l'attaquant refuse ou devient agressif.

### Smishing (SMS)

1. Message inattendu : Vous recevez un SMS d'une entité avec laquelle vous n'interagissez pas habituellement (banque inconnue, service de livraison non commandé).

2. Lien court ou URL raccourcie : Les messages contiennent des URL réduites (`bit.ly`, `tinyurl`) qui masquent la destination réelle. Les vrais services envoient rarement des SMS avec des liens courts.

3. Urgence et menace : "Votre colis n'a pu être livré, cliquez ici", "Votre compte bancaire est bloqué, confirmez votre identité", "Mettez à jour votre Apple ID maintenant". La pression temporelle est forte.

4. Demande de confirmation d'informations : "Confirmez votre numéro de sécurité sociale", "Entrez votre code secret", "Vérifiez votre identité sur ce lien". Les SMS ne demandent jamais cela.

5. Numéro de sender suspect : Le SMS vient d'un numéro aléatoire, d'un code court inhabituel, ou prétend venir d'une entité officielle mais provient d'un mobile standard.

6. Demande d'action sur une plateforme tierce : "Cliquez sur ce lien pour vérifier", "Téléchargez cette app pour confirmer", "Répondez avec votre identifiant".

### WhatsApp et messagerie instantanée

1. Contact "ami" avec numéro inconnu : L'attaquant prétend être quelqu'un que vous connaissez ("C'est ton cousin", "C'est de la part de Paul"). Vérifiez avec la personne par un autre canal.

2. Lien inattendu : Un ami vous envoie un lien en disant cliquer rapidement sans explication. Les vrais amis expliquent le contexte.

3. Message reçu sur une plateforme peu utilisée : Vous recevez un message important sur Telegram alors que vous utilisiez Signal. Les vrais contacts vous contactent sur votre canal habituel.

4. Demande de partage d'écran ou d'accès : L'attaquant demande de partager votre écran pour "aider" ou "vérifier" quelque chose. C'est un vecteur d'accès direct.

# Qualification

La gravité dépend du nombre de collaborateurs touchés et du type de compromission. Cependant, tout incident doit être pris au sérieux et signalé immédiatement.

### Procédure de signalement pour les collaborateurs

Pour les appels téléphoniques :

- Rester calme et ne pas se laisser intimider par l'urgence
- Ne pas donner d'informations sensibles (identifiants, codes, détails système)
- Raccrocher et rappeler le numéro officiel du support IT ou de l'entité prétendante
- Signaler l'appel à l'équipe de sécurité avec l'heure, le numéro affiché, et le contenu

Pour les SMS et WhatsApp :

- Ne pas cliquer sur le lien et ne pas télécharger de fichier
- Ne pas répondre ni confirmer d'informations
- Bloquer le numéro ou le contact
- Signaler le message à l'équipe de sécurité

# Analyse

Lors de la détection d'une attaque téléphonique, une investigation structurée est nécessaire.

### Communication aux collaborateurs

Dès la détection, envoyer une communication interne demandant aux collaborateurs de signaler les incidents similaires :

"Une tentative de [phishing par SMS / appel de vishing / arnaque WhatsApp] a été détectée. Si vous avez reçu un message ou un appel similaire, nous vous demandons de le signaler immédiatement à l'équipe de sécurité avec : l'heure, le numéro ou le contact, le contenu du message ou les détails de l'appel."

Cette communication aide à identifier l'étendue de l'attaque.

### Vérification des logs téléphoniques

Pour les appels (vishing) :

1. Vérifier auprès du prestataire télécom les numéros utilisés pour l'appel
2. Identifier tous les appels similaires reçus pendant la même période
3. Bloquer le(s) numéro(s) source au niveau du PABX ou auprès de l'opérateur
4. Vérifier les logs d'accès IT pour voir si quelqu'un a utilisé les identifiants compromis

### Téléphones personnels des collaborateurs

Important : Les SMS et WhatsApp sont reçus sur les téléphones personnels des collaborateurs. L'entreprise ne réalisera pas d'investigation numérique sur ces appareils personnels.

À la place, envoyer une communication aux collaborateurs affectés avec les actions à prendre sur leurs téléphones personnels :

"Une campagne de smishing / arnaque WhatsApp nous affecte. Si vous avez reçu ce type de message, veuillez : 1) Ne pas cliquer sur le lien, 2) Bloquer le contact, 3) Supprimer le message, 4) Signaler le message à votre opérateur et à l'équipe de sécurité interne."

# Traitement

En fonction de l'analyse, mettre en place les actions suivantes.

### Si un identifiant a été divulgué par téléphone

- Réinitialiser immédiatement le mot de passe du collaborateur
- Forcer une authentification multi-facteurs si non active
- Vérifier les logs d'accès de ce compte pour détecter toute utilisation malveillante
- Si des accès suspects sont détectés, suivre la procédure d'intrusion

### Si un lien a été cliqué

Pour les SMS et WhatsApp sur téléphone personnel:

Envoyer une communication au collaborateur : "Vous avez cliqué sur un lien de smishing. Veuillez : 1) Changer votre mot de passe depuis un autre appareil sécurisé, 2) Vérifier votre compte bancaire pour toute activité suspecte, 3) Signaler tout paiement non autorisé, 4) Activer une authentification multi-facteurs si ce n'est pas fait."

L'équipe de sécurité n'effectuera pas d'investigation numérique sur le téléphone personnel mais peut demander une description des actions effectuées par le collaborateur après avoir cliqué.

### Communication à l'organisation

Alerter l'organisation sur la tentative sans culpabiliser les victimes, en décrivant les signaux d'alerte et les actions prises.

### Actions légales si nécessaire

Si l'attaque tentait d'usurper l'identité d'une banque, d'une administration, ou contenait du chantage ou des menaces, envisager une plainte auprès de la police ou de la gendarmerie en fournissant tous les détails (numéros, timestamps, captures d'écran).

# Actions post-incident

Une fois l'incident résolu, analyser les causes et points d'amélioration.

### Analyse post-mortem

- Pourquoi le collaborateur a-t-il cliqué ou divulgué des informations ? Manque de sensibilisation ? Urgence trop crédible ? Absence de procédure de vérification ?
- Quels détails l'attaquant a-t-il utilisés ? Cela indique le niveau de reconnaissance (recherche publique, données de breach, informations de l'annuaire interne).
- Comment l'attaquant s'est-il procuré le numéro de téléphone ? Annuaire public ? Données de breach ? Fuite interne ?

### Améliorations techniques

- Bloquer le(s) numéro(s) source auprès de l'opérateur ou du PABX
- Ajouter un filtre sur les SMS longs ou contenant des URLs pour les appareils d'entreprise
- Configurer une authentification multi-facteurs avec l'application mobile (pas par SMS) pour les systèmes sensibles
- Auditer la présence du numéro de l'organisation dans les annuaires publics

### Renforcement de la sensibilisation

- Mettre à jour le programme de sensibilisation avec cet exemple concret
- Former les collaborateurs sur les tactiques de vishing (urgence, prétextes courants)
- Établir une procédure claire : "Qui peut demander des mots de passe ? Jamais par téléphone ou email"
- Effectuer des simulations d'appels téléphoniques pour évaluer la vigilance (si légalement permis selon la législation locale)
