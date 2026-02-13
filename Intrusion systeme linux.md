# Description

Une intrusion Linux correspond à l'accès non autorisé à un serveur ou une station Linux, avec ou sans élévation de privilèges, pouvant entraîner :

- Exécution de code malveillant (malware, backdoor, RAT),
- Vol ou exfiltration de données,
- Mouvement latéral vers d'autres systèmes,
- Mise en place de mécanismes de persistance,
- Compromission de comptes (dont comptes root ou avec privilèges sudo).

# Mesures préventives

La prévention des intrusions sur Linux repose sur plusieurs piliers : durcissement des configurations (désactivation de SSH root, clés SSH plutôt que mots de passe), firewalls configurés correctement (UFW, firewalld), mis à jour réguliers du système et des applications, utilisation de sudo avec restrictions appropriées, centralisation des logs, et sauvegardes régulières et testées des données critiques.

# Détection

Les intrusions sur Linux peuvent être détectées de trois manières : par déclaration d'un administrateur ou d'un utilisateur constatant un comportement anormal, par alerte d'une solution de monitoring (Wazuh, Osquery, etc.), ou par analyse des logs système.

**Journaux Linux à surveiller :**

Les fichiers de logs critiques incluent **`/var/log/auth.log`** (tentatives d'authentification SSH, connexions sudo, échecs d'accès), **`/var/log/syslog`** ou **`/var/log/messages`** (événements système généraux), **`/var/log/audit/audit.log`** (if auditd is enabled – activités kernel détaillées), et **`~/.bash_history`** ou **`~/.zsh_history`** (historique des commandes par utilisateur). Sur les systèmes avec journald, utilisez `journalctl` pour consulter les logs de manière centralisée.

# Qualification

La gravité de l'incident dépend du nombre de serveurs compromis, du niveau d'accès obtenu (utilisateur standard vs root) et du type de compromission. Cependant, quelle que soit la gravité, **la première mesure critique est l'isolation immédiate du système compromis** : isolez la machine du réseau (débranchez le câble Ethernet ou déactivez la carte réseau), ou mettez en place une règle de firewall bloquant tout trafic entrant/sortant. Cette isolation doit être faite avant toute investigation pour limiter la propagation et l'exfiltration de données.

# Analyse

⚠️ **Règle d'or : capturer la mémoire avant toute action destructive.** Un simple redémarrage ou arrêt du système supprimera des informations précieuses sur les processus malveillants et les connexions actives.

L'analyse forensique se déroule en deux phases : d'abord l'analyse mémoire qui capture l'état volatil du système au moment de l'incident, puis l'analyse disque qui recherche les traces persistantes de compromission.

## Analyse de la mémoire

La mémoire vive (RAM) contient des informations volatiles très utiles pour la forensique : les processus actifs, les connexions réseau ouvertes, le code injecté en mémoire, les clés de chiffrement, et potentiellement des malwares fileless. ⚠️ **Un redémarrage du système détruit définitivement ces éléments**, d'où l'importance de capturer le dump mémoire rapidement.

### LiME (Linux Memory Extractor)

**LiME** est l'outil open source de référence pour capturer la mémoire sur Linux. Il charge un module kernel temporaire pour extraire l'intégralité de la mémoire physique dans un fichier RAW exploitable par Volatility.

- **GitHub** : https://github.com/504ensicsLabs/LiME
- **Documentation** : https://github.com/504ensicsLabs/LiME/wiki

**Bonnes pratiques lors de la capture :** exécutez LiME depuis un support externe ou via SSH depuis une autre machine (jamais depuis la machine compromise), notez l'heure exacte du dump et conservez le hash SHA256 du fichier généré pour l'intégrité forensique.

### Volatility 3

Une fois le dump mémoire acquis, transférez-le sur une station dédiée sécurisée pour l'analyse. **Volatility 3** est le framework open source moderne pour analyser les dumps mémoire Linux.

- **Documentation** : https://volatility3.readthedocs.io/
- **Repository GitHub** : https://github.com/volatilityfoundation/volatility3

**Analyses prioritaires à réaliser :**

Commencez par lister les processus actifs :

```bash
volatility3 -f dump_memory.raw linux.pslist
```

Recherchez les processus cachés :

```bash
volatility3 -f dump_memory.raw linux.psscan
```

Cherchez les injections de code :

```bash
volatility3 -f dump_memory.raw linux.malfind
```

Analysez les connexions réseau ouvertes :

```bash
volatility3 -f dump_memory.raw linux.netstat
```

Examinez les fichiers ouverts :

```bash
volatility3 -f dump_memory.raw linux.lsof
```

Pour les incidents graves, cherchez les modules kernel chargés suspectes qui indiqueraient une compromission au niveau kernel.

### Analyse du disque

Une image complète du disque est nécessaire dans les cas suivants : persistance suspectée, exfiltration de données, exigence judiciaire, ou enquête approfondie hors ligne.

**Création d'une image disque avec dd (natif et open source)**

Utilisez **`dd`**, l'outil natif Linux pour copier les données bit par bit. Combinez-le avec `md5sum` ou `sha256sum` pour générer un hash d'intégrité forensique.

```bash
sudo dd if=/dev/sdX of=image_disque.raw bs=4M status=progress && sha256sum image_disque.raw > image_disque.raw.sha256
```

Le fichier RAW généré peut être analysé avec des outils comme Autopsy ou des scripts Python dédiés à la forensique Linux.

## Analyse post-acquisition

Après acquisition des dumps, effectuez une analyse approfondie des logs et du disque :

**Recherche dans les logs :**

1. Parcourez **`/var/log/auth.log`** pour identifier les tentatives d'accès suspectes, connexions SSH inhabituelles, commandes sudo non autorisées.

2. Vérifiez **`/var/log/syslog`** pour les messages d'erreur, chargements de modules kernel, redémarrages inattendus.

3. Consultez l'historique des commandes des utilisateurs suspects dans **`~/.bash_history`** et **`~/.zsh_history`**.

4. Utilisez `journalctl` pour une vue centralisée :

```bash
journalctl -xe                    # Affiche les erreurs récentes
journalctl --since "2 hours ago"  # Affiche les logs des 2 dernières heures
```

**Recherche de persistance :**

Les attaquants mettent en place des mécanismes de persistance pour conserver l'accès. Les crontabs, les tâches systemd, les fichiers d'initialisation et les clés SSH sont des vecteurs favoris.

**Vérifications manuelles :**

1. Examinez les crontabs :

```bash
sudo crontab -l      # pour root
crontab -l           # pour l'utilisateur courant
# ou vérifiez le répertoire
ls -la /etc/cron.d/
```

Recherchez les tâches planifiées récentes ou suspectes.

2. Vérifiez les dossiers en lecture seule : `/etc/rc.local`, `/etc/init.d/`, et les fichiers systemd dans `/etc/systemd/system/`. Recherchez les scripts lancés au démarrage.

3. Contrôlez les clés SSH autorisées : `/root/.ssh/authorized_keys` et `/home/<user>/.ssh/authorized_keys`. Une clé supplémentaire indiquerait un accès persistent.

4. Inspectez les fichiers d'initialisation shell : `/etc/bashrc`, `/etc/bash.bashrc`, `/home/<user>/.bashrc`, `/home/<user>/.bash_profile`. Les attaquants y insèrent souvent des backdoors.

5. Recherchez les fichiers binaires suspects modifiés récemment :

```bash
find / -type f -newermt "2 hours ago" ! -path "/proc/*" ! -path "/sys/*" 2>/dev/null
```

6. Vérifiez les utilisateurs cachés :

```bash
getent passwd
```

**Outils recommandés :**

**chkrootkit** (open source) : https://www.chkrootkit.org/ – Détection de rootkits et autres malwares Linux courants.

**rkhunter** (open source) : https://rkhunter.sourceforge.io/ – Scanner pour les rootkits et fichiers suspects.

**Osquery** (open source) : https://osquery.io/ – Framework d'interrogation système pour enquêtes forensiques avancées.

# Traitement

Documentez l'incident dans un ticket GLPI avec les éléments collectés (logs, captures mémoire, images disque, timeline des actions). Conservez également une copie dans le channel Teams cybersécurité.

En fonction des conclusions :
- **Si la compromission est confirmée** : déposez plainte auprès des autorités compétentes.
- **Si une exfiltration de données est détectée** : déclenchez la procédure de gestion des fuites de données selon votre politique.

Recherchez les traces de propagation vers d'autres systèmes (connexions SSH historiques, scripts de scanner réseau, listes de cibles).

# Actions post-incident

Une fois l'investigation terminée, le système ne doit pas être réparé partiellement. **Réinstallez entièrement le système Linux** sur le serveur ou la station compromise. Cette étape est essentielle pour garantir que la machine n'héberge plus aucun élément malveillant, même au niveau kernel. Avant la réinstallation, conservez une copie complète du disque original pour usage judiciaire éventuel.
