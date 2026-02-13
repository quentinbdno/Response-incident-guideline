# Description

Une fuite de données correspond à la divulgation non autorisée d'informations confidentielles en dehors de l'organisation. Elle peut survenir intentionnellement (par un collaborateur malveillant ou compromis) ou accidentellement (configuration mal sécurisée, suppression mal exécutée, partage erroné). Les données compromises peuvent être des documents confidentiels, des secrets commerciaux, des données client, ou des données personnelles.

Les fuites de données peuvent entraîner :

- Avantage concurrentiel perdu
- Perte de confiance des clients et partenaires
- Sanctions légales et pénalités (notamment RGPD pour données personnelles)
- Obligations légales de notification aux personnes concernées
- Impact réputationnel et financier
- Responsabilité civile et pénale

# Mesures préventives

La prévention des fuites de données repose sur plusieurs piliers : classification des données selon leur sensibilité, mise en place de contrôles d'accès basés sur le principe du moindre privilège, chiffrement des données en transit et au repos, audits réguliers des accès et téléchargements, politique claire de partage et d'export de données, et sensibilisation des collaborateurs aux risques.

# Détection

Les fuites de données peuvent être détectées de plusieurs manières : signalement direct d'une personne ayant découvert les données en ligne, alerte d'un service de monitoring (Google Alerts, services de détection de fuites), découverte lors d'une enquête interne, ou notification d'un tiers ayant accès aux données.

### Indicateurs de fuite potentielle

1. Les données commencent à circuler sur le dark web ou les forums publics
2. Un service de monitoring signale la présence des données exposées
3. Un client ou partenaire signale l'accès à des données confidentielles
4. Un collaborateur signale un accès anormal ou un téléchargement massif
5. Découverte d'un serveur ou d'une base de données exposée sans authentification

# Qualification

La gravité dépend du volume de données, de leur sensibilité, et du nombre de personnes concernées.

### Classification de la gravité

Critique : Données personnelles de clients, secrets commerciaux majeurs, données financières ou stratégiques, compromise de plus de 1000 personnes

Grave : Données métier confidentielles, impact de 100 à 1000 personnes

Modérée : Données non-critiques, petit nombre concerné

# Analyse

Une investigation rapide est nécessaire pour contenir la fuite et évaluer son ampleur.

### Communication immédiate aux parties prenantes

1. Notifier le manager de la personne si suspicion vers un collaborateur
2. Notifier le DPO si données personnelles sont impliquées
3. Notifier la direction générale et les responsables métier des données concernées
4. Isoler le système ou le collaborateur si fuite intentionnelle suspectée

### Déterminer la nature des données

1. Lister les données qui ont fui (volume, types, sensibilité)
2. Identifier si des données personnelles sont présentes (date de naissance, numéros d'identification, adresses email, numéros de téléphone, données biométriques, etc.)
3. Vérifier si d'autres données sensibles sont présentes (mots de passe, tokens, secrets commerciaux, données financières)

### Déterminer le périmètre de la fuite

1. Quand la fuite s'est-elle produite ? Date approximative ou confirmée ?
2. Combien de temps les données sont-elles restées exposées ou accessibles ?
3. Combien de personnes sont concernées ? (collaborateurs internes, clients, fournisseurs ?)
4. Qui a pu accéder aux données ? (authentification requise ou complètement publicly exposée ?)

### Déterminer le vecteur de fuite

1. Export accidentel par un collaborateur
2. Partage de fichier avec permissions trop larges
3. Serveur ou base de données exposée sans authentification
4. Email envoyé à une mauvaise adresse
5. Accès par un compte compromis
6. Suppression malveillante par un utilisateur interne
7. Incident technique (sauvegarde accessible publiquement, logs exposés)
8. Fuite depuis un fournisseur tiers ou prestataire

### Collecte de preuves et d'indices

1. Si donné sont sur le web : Capturer les preuves (URL, screenshots, date de découverte)
2. Si données sont sur un serveur interne : Vérifier les logs d'accès et de partage
3. Vérifier l'historique des permissions et des modifications
4. Analyser les logs réseau pour identifier les téléchargements ou exports massifs
5. Si collaborateur suspect : Vérifier ses accès, ses actions récentes, ses communications

### Notification du DPO en cas de données personnelles

Si des données personnelles sont impliquées, notifier immédiatement le DPO (Délégué à la Protection des Données). Le DPO est responsable de :

1. Évaluer l'obligation légale de notifier la CNIL
2. Évaluer l'obligation de notifier les personnes concernées
3. Coordonner la réponse juridique et complète

Fournir au DPO : la liste des données personnelles fuitées, le nombre de personnes concernées, la date de découverte, le vecteur de la fuite.

# Traitement

Les actions de traitement dépendent du type de fuite et de son urgence.

### Contention immédiate

1. Si serveur exposé : Sécuriser l'accès (activer authentification, bloquer l'IP, supprimer les données)
2. Si compte compromis : Réinitialiser le mot de passe et l'isoler
3. Si collaborateur malveillant : Mettre en place des restrictions d'accès immédiatement
4. Si données partagées par erreur : Révoquer les accès et demander la suppression

### Notification CNIL (avec DPO)

Si données personnelles, le DPO coordonnera la notification CNIL :

1. Déclaration obligatoire dans les 72 heures si risque pour les droits et libertés
2. Information aux personnes concernées si risque élevé
3. Assistance juridique pour la formulation correcte des notifications
4. Suivi des obligations légales selon le RGPD

### Notification des personnes concernées (si applicable)

Si requis légalement, envoyer une communication clara aux personnes dont les données ont fui :

"Nous devons vous informer qu'une fuite de données nous affecte. Les données concernées incluent [liste des informations]. Nous avons mis en place les actions suivantes : [mesures prises]. Vous avez des droits sur vos données : [exercer les droits]. Si vous avez des questions, contactez [contact DPO ou responsable]."

### Réparation ou offre de compensation

Selon le contexte et la législation :

1. Offrir un crédit de monitoring de crédit ou une assurance si données bancaires fuitées
2. Offre des services de sécurisation (audit, sensibilisation)
3. Compensation financière en cas de dommages identifiés

### Communication à l'organisation

Alerter l'organisation sur l'incident (sans révéler tous les détails sensibles) et les mesures prises pour éviter une récurrence.

# Actions post-incident

Une fois la fuite contenue, analyser les causes et points d'amélioration.

### Audit post-mortem

1. Comment la fuite s'est-elle produite ? Manque de contrôle d'accès ? Erreur humaine ? Malveillance ?
2. Pourquoi n'a-t-elle pas été détectée immédiatement ? Absence de monitoring ? Manque de logs ?
3. Cela aurait-il pu être évité ? Quelles protections manquaient ?

### Améliorations techniques

1. Implémenter le chiffrement des données sensibles
2. Améliorer le monitoring et les alertes en cas de téléchargement massif
3. Revoir les permissions et les contrôles d'accès (appliquer le moindre privilège)
4. Auditer régulièrement les partages de fichiers et supprimers les accès inutiles
5. Mettre en place une solution Data Loss Prevention (DLP) pour prévenir les exports non autorisés
6. Améliorer les logs et l'audit trail pour tracer les modifications

### Renforcement de la sensibilisation

1. Sensibiliser les collaborateurs sur la classification des données
2. Former sur les risques de partages et d'export imprudents
3. Mettre en place une procédure claire pour exporter ou partager des données confidentielles
4. Effectuer des audits de sensibilisation

### Suivi avec le DPO

1. Documenter complètement l'incident pour le DPO
2. Coordonner le suivi des notifications CNIL si applicables
3. Mettre à jour le registre des traitements si nécessaire
4. Évaluer la nécessité d'une évaluation d'impact pour les traitements futurs
