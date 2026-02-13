# Description

Le phishing est une attaque par email massif adressée à de nombreux destinataires, tandis que le spear-phishing est ciblé vers des entreprises ou individus spécifiques. L'attaquant se fait passer pour une entité de confiance (banque, fournisseur IT, support interne) pour inciter le destinataire à cliquer sur un lien malveillant, télécharger une pièce jointe ou saisir des identifiants.

# Mesures préventives

La prévention des attaques par email repose sur plusieurs piliers : une sensibilisation régulière des collaborateurs aux risques et tactiques courantes, une politique de sécurité claire définissant les règles de partage d'informations, des procédures formalisées pour les demandes sensibles, la mise en place d'une culture de vérification avant d'agir, et l'encouragement de signaler les tentatives suspectes sans peur de représailles.

# Détection

Les tentatives de phishing présentent souvent des signaux d'alerte que les collaborateurs doivent apprendre à reconnaître.

### Indicateurs visuels et syntaxe

1. Adresse email de l'expéditeur : Vérifier que l'adresse est exacte. Les variations subtiles sont courantes (`nouvellebanque.fr` vs `nouve11ebanque.fr`, avec un `ll` au lieu de `ll`). Ne pas se fier au nom affiché, vérifier l'adresse réelle.

2. Urgence et menace : Les messages utilisent des formulations urgentes ("Vérifiez votre compte immédiatement", "Votre compte sera fermé", "Action requise dans les 24h"). C'est un signal d'alerte fort.

3. Demande d'identifiants ou d'informations sensibles : Aucune entité légitime ne demande par email les mots de passe, codes 2FA, numéros de compte, RIB, ou données personnelles. Cela ne doit jamais se faire par email.

4. Liens suspects : Survoler le lien (sans cliquer) pour voir l'URL de destination. Si le lien affiche `www.secure-bank-login.com` mais pointe vers une adresse IP ou un domaine inconnu, c'est un phishing. Les raccourcisseurs d'URL (`bit.ly`, `short.link`) peuvent masquer une destination malveillante.

5. Pièces jointes inattendues : Les fichiers `.exe`, `.scr`, `.vbs`, `.zip` contenant des exécutables sont des vecteurs d'infection. Les documents Office (`.docx`, `.xlsx`) peuvent contenir des macros malveillantes. Si une pièce jointe n'était pas attendue, ne pas l'ouvrir.

6. Formulation générique : Utilisation de "Cher Client" au lieu du nom, mauvaise grammaire, erreurs de français. Les vraies entreprises envoient des messages personnalisés et bien rédigés.

7. Domaine de l'entreprise incompatible : Si le message prétend venir du support IT interne mais l'email vient d'une adresse Gmail ou d'un domaine externe, c'est une arnaque.

# Qualification

La gravité dépend du nombre de collaborateurs touchés et du type de compromission. Cependant, tout incident doit être pris au sérieux et signalé immédiatement.

### Procédure de signalement pour les collaborateurs

- Ne pas cliquer sur les liens ni ouvrir les pièces jointes.
- Signaler l'email : Utiliser la fonction "Signaler le phishing" ou envoyer l'email à `securite@entreprise.fr`.
- Supprimer le message une fois signalé.
- Informer le Helpdesk si des identifiants ont déjà été saisis.

# Analyse

Lors de la détection d'une attaque de phishing, une investigation structurée est nécessaire.

### Communication aux collaborateurs

Dès la détection, envoyer une communication interne demandant aux collaborateurs de signaler les incidents similaires pendant la période d'analyse :

"Une tentative de phishing a été détectée. Si vous avez reçu un email similaire, nous vous demandons de le signaler immédiatement à l'équipe de sécurité avec les détails suivants : heure de réception, objet exact, expéditeur."

Cette communication aide à identifier l'étendue de l'attaque et les cibles prioritaires.

### Vérification sur le serveur mail

Une fois l'email signalé, l'équipe de sécurité doit analyser les traces :

1. Extraction de l'email complet côté serveur : Récupérer les en-têtes complets (From, Return-Path, Received, Authentication-Results) pour identifier la véritable source.

2. Analyse des enregistrements SMTP : Vérifier que l'email provient réellement du domaine affiché. Utiliser `SPF`, `DKIM`, `DMARC` pour authentifier l'expéditeur.

3. Recherche sur le serveur mail : Identifier tous les emails avec les mêmes caractéristiques (même domaine falsifié, même lien, même objet partiel) pour évaluer le volume.

4. Quarantaine : Bloquer le domaine source si possible, ou ajouter des règles pour filtrer les emails similaires.

5. Extraction du lien ou de la pièce jointe : Analyser l'URL ou le fichier de manière sécurisée (sandbox virtuel, outils de scanning) pour identifier le malware ou le service d'hameçonnage utilisé.

### Évaluation de l'impact

Déterminer si des collaborateurs ont :

- Cliqué sur le lien et saisi des identifiants
- Téléchargé et ouvert une pièce jointe
- Fourni des informations sensibles

# Traitement

En fonction de l'analyse, mettre en place les actions suivantes.

### Si des identifiants ont été compromis

- Réinitialiser immédiatement tous les mots de passe des comptes affectés
- Forcer une authentification multi-facteurs si non active
- Vérifier les logs d'accès de ces comptes pour détecter toute utilisation malveillante
- Si des accès suspects sont détectés, suivre la procédure d'intrusion

### Si une pièce jointe malveillante a été ouverte

- Isoler immédiatement l'ordinateur du réseau
- Effectuer un scan antivirus complet
- Analyser les processus lancés et les fichiers modifiés
- Vérifier les connexions réseau sortantes
- Suivre la procédure d'intrusion si un malware est confirmé

### Communication à l'organisation

Alerter l'organisation sur la tentative sans culpabiliser les victimes, en décrivant les signaux d'alerte et les actions prises.

### Actions légales si nécessaire

Si l'email tentait d'usurper l'identité d'une banque ou d'une institution, ou s'il contient des menaces, envisager une plainte auprès de la police ou de la gendarmerie.

# Actions post-incident

Une fois l'incident résolu, analyser les causes et points d'amélioration.

### Analyse post-mortem

- Pourquoi le collaborateur a-t-il cliqué ? Manque de sensibilisation ? Urgence trop crédible ? Absence de seconde vérification ?
- Quels détails l'attaquant a-t-il utilisés ? Cela indique le niveau de reconnaissance (recherche publique, données de breach, informations de l'annuaire).
- Pourquoi le serveur mail n'a-t-il pas filtré l'email ? Absence de règle DMARC/SPF efficace ? Domaine similaire non reconnu ?

### Améliorations techniques

- Bloquer le domaine source au niveau du firewall
- Ajouter des règles SPF/DKIM/DMARC si absentes
- Configurer le serveur mail pour filtrer les domaines lookalike (homograph attacks)
- Ajouter l'URL détectée aux listes noires globales

### Renforcement de la sensibilisation

- Mettre à jour le programme de sensibilisation avec cet exemple concret
- Effectuer des campagnes de phishing simulé pour évaluer la vigilance de l'équipe
- Former les collaborateurs qui ont cliqué
- Créer des alertes visuelles dans le client email ("Email provenant d'un domaine externe")
