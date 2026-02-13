# Description
Une intrusion Windows correspond à l’accès non autorisé à un poste de travail ou un serveur Windows, avec ou sans élévation de privilèges, pouvant entraîner :

- Exécution de code malveillant (malware, ransomware, RAT),
- Vol ou exfiltration de données,
- Mouvement latéral,
- Mise en place de mécanismes de persistance,
- Compromission de comptes (dont comptes à privilèges).

# Mesures préventives
- Charte informatique
- Antivirus
- Durcissement des systèmes
- Centralisation des logs sur Graylog
- Sauvegarde des données 
# Détection
-Déclaration par l'utilisateur.

Alerte d'un antivirus

Journaux Windows à surveiller

- Security.evtx :
 - 4624 → Logon
 - 4625 → Logon failed
 - 4672 → Privileges spéciaux
 - 4688 → Process creation
 - 7045 → Création de service
- Microsoft-Windows-PowerShell/Operational
- Microsoft-Windows-TaskScheduler
- TerminalServices-LocalSessionManager

# Qualification
La gravité de l'incident dépend du nombre d'ordinateur compromis et du type de compromission du système.

```
La première mesure à mettre en oeuvre est l'isolation du système compromis du SI : déconnexion de la carte Wifi, débrancher l'ordinateur. L'isolation peut également être réaliser par une exclusion au niveau du firewall.
```

# Analyse
```
Toujours capturer la mémoire avant toute action destructive
```
L'analyse doit se réaliser sur l'espace mémoire et sur le disque du système compromis. Puis il faudra rechercher les éléments 

## Analyse de la mémoire
La mémoire vive contient :

- Processus actifs
- Connexions réseau ouvertes
- Code injecté
- Clés de chiffrement
- Artefacts d’authentification (LSASS)
- Malware fileless
⚠️ Une extinction de la machine détruit ces éléments.



### WinPmem
WinPmem est l’outil open source de référence pour capturer la mémoire Windows.

Il charge temporairement un driver kernel afin d’extraire la mémoire physique complète dans un fichier RAW exploitable par Volatility.

Documentation officielle :
https://github.com/Velocidex/WinPmem

Guide d’utilisation (Velociraptor / Velocidex) :
https://docs.velociraptor.app/docs/forensic_collection/memory/

Bonnes pratiques :

- Exécuter depuis un support externe
- Noter l’heure exacte du dump
- Conserver le hash SHA256 du fichier généré

### Volatility 3
Une fois le dump mémoire acquis, l’analyse doit être réalisée sur une station dédiée.

Volatility 3 est le framework open source moderne d’analyse mémoire.

Documentation officielle :
https://volatility3.readthedocs.io/

Dépôt officiel :
https://github.com/volatilityfoundation/volatility3

Recherches prioritaires :
- Processus cachés (psscan)
- Processus non légitimes (pslist)
- Injection de code (malfind)
- DLL injectées (dlllist)
- Connexions réseau actives (netscan)
- Handles suspects (handles)
- Dump LSASS
- Modules kernel non légitimes

### Analyse du disque
L’image disque est requise lorsque :

- Persistance suspectée
- Exfiltration de données
- Ransomware
- Exigence judiciaire
- Investigation approfondie hors ligne

### FTK Imager (version gratuite)
FTK Imager permet de créer des images forensiques (E01 ou RAW).

⚠️ Gratuit pour un certain temps (suffisant pour l'investigation) mais non open source.

Page officielle :
https://www.exterro.com/digital-forensics-software/ftk-imager

Documentation :
https://support.exterro.com/

### Recherche de persistance
Les attaquants utilisent fréquemment les tâches planifiées pour relancer un malware.

Vérifier dans le répertoire suivant les scripts powershell qui ont été créés récemment. En particulier les créations récente, l'exécution sous compte admin et les commandes encodées.
```
C:\Windows\System32\Tasks
```
Les éléments peuvent également être présent dans les tâches planifiées. La commande suivant permet d'afficher toutes les tâches planifiées 
```
sc query type= service state= all
```
Les dossiers suivants peuvent également contenir des programmes qui peuvent être exécutés au démarrage 
```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```
Le dernier élément à vérifier sont les clefs de registre. 


Plusieurs outils sont disponibles pour faire l'analyse des éléments de persistance : 

Natif à Microsoft Autoruns (sysinternal) https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns

Velociraptor https://docs.velociraptor.app/ dossier spécifique pour les artefacts Windows https://docs.velociraptor.app/artifacts/



# Traitement
Remonter l'incident dans un ticket GLPI. Conserver les éléments récupérés dans GLPI et/ou dans le teams cybersécurité.

Si la compromission est réelle, il faut porter plainte. Si une fuite de données a été identifiée, suivre la procédure sur la fuite de données.

# Actions post-incident
Réinstaller windows sur l'ordinateur ou le serveur. 