# Cybersecurity-linux-fundamentals
# **Project #1: Linux Security Basics / Bases de la Sécurité Linux**

> **Overview / Présentation:**
> **[EN]** This repository documents foundational Linux commands, access control mechanisms, SSH hardening, process management, and log analysis from a cybersecurity perspective.
> **[FR]** Ce dépôt documente les commandes Linux fondamentales, les mécanismes de contrôle d'accès, la sécurisation SSH, la gestion des processus et l'analyse des journaux (logs) sous l'angle de la cybersécurité.

---

## **1. Essential Linux Commands & Security Triage / Commandes Essentielles & Triage de Sécurité**

### **File Navigation & Inspection / Navigation & Inspection de Fichiers**

* **Commands / Commandes:** `ls`, `cd`, `pwd`, `cat`, `less`
* **[EN]** Used to navigate system directories and inspect configuration files (e.g., `/etc/passwd`, `/etc/shadow`) to review user accounts. During triage, an analyst checks web roots and recently modified files.
* **[FR]** Utilisées pour naviguer dans les répertoires et examiner les fichiers de configuration (ex. `/etc/passwd`, `/etc/shadow`). Lors du triage, un analyste vérifie la racine des serveurs web et les fichiers récemment modifiés.



### **Searching & Filtering / Recherche & Filtrage**

* **Commands / Commandes:** `grep`, `find`
* **[EN]** `grep` searches for specific strings inside large log files to identify Indicators of Compromise (IOCs). `find / -mmin -60` locates files modified in the last 60 minutes.
* **[FR]** `grep` recherche des chaînes spécifiques dans de grands fichiers de logs pour identifier des indicateurs de compromission (IOC). `find / -mmin -60` localise les fichiers modifiés au cours des 60 dernières minutes.



### **Network & Port Inspection / Inspection Réseau & Ports**

* **Commands / Commandes:** `ip`, `ss`, `curl`, `wget`
* **[EN]** `ip a` checks network interfaces, while `ss -tulnp` displays listening ports and associated processes. This helps detect unauthorized listening services or malicious command-and-control (C2) connections.
* **[FR]** `ip a` vérifie les interfaces réseau, tandis que `ss -tulnp` affiche les ports à l'écoute et les processus associés. Cela permet de détecter des services non autorisés ou des connexions de commande et contrôle (C2) malveillantes.

---

### **`ls` (List / Lister)**

* **[EN]** Lists directory contents (files and folders). Adding flags like `-la` reveals hidden files (starting with `.`, such as `.ssh` configurations) and detailed permissions.
* **[FR]** Liste le contenu des répertoires (fichiers et dossiers). L'ajout d'indicateurs comme `-la` révèle les fichiers cachés (commençant par `.`, comme les configurations `.ssh`) et les permissions détaillées.

### **`cd` (Change Directory / Changer de Répertoire)**

* **[EN]** Changes the current working shell directory. Analysts use this to navigate to critical log directories (e.g., `/var/log`) or configuration paths (e.g., `/etc/ssh`).
* **[FR]** Change le répertoire de travail actuel de l'interpréteur de commandes. Les analystes l'utilisent pour naviguer vers les répertoires de journaux critiques ou les chemins de configuration.

### **`pwd` (Print Working Directory / Afficher le Répertoire Courant)**

* **[EN]** Outputs the full absolute path of the current directory you are standing in, preventing mistakes when running powerful administrative actions.
* **[FR]** Affiche le chemin absolu complet du répertoire actuel dans lequel vous vous trouvez, évitant les erreurs lors de l'exécution d'actions administratives.

### **`cat` (Concatenate / Concaténer et Afficher)**

* **[EN]** Reads a file sequentially and prints its entire content to the terminal screen. Useful for quickly viewing small configuration files (like `/etc/passwd`).
* **[FR]** Lit un fichier séquentiellement et affiche son contenu entier sur l'écran du terminal. Utile pour visualiser rapidement de petits fichiers de configuration.

### **`less` (Page through text / Affichage paginé)**

* **[EN]** Opens a text file interactively page-by-page, allowing you to scroll up and down. Essential for reading large files without flooding or crashing the terminal.
* **[FR]** Ouvre un fichier texte de manière interactive page par page, permettant de faire défiler vers le haut et le bas. Essentiel pour lire des fichiers volumineux.

### **`grep` (Global Regular Expression Print / Recherche de motifs)**

* **[EN]** Searches text files for matching lines using regular expressions. In security, `grep "Failed password" /var/log/auth.log` immediately isolates failed SSH login attempts.
* **[FR]** Recherche des lignes correspondantes dans des fichiers texte à l'aide d'expressions régulières. En sécurité, il permet d'isoler immédiatement les échecs de connexion SSH.

### **`find` (Find files / Rechercher des fichiers)**

* **[EN]** Searches for files within a directory hierarchy based on criteria like name, size, or modification time (e.g., finding files modified in the last 24 hours to spot malware installation).
* **[FR]** Recherche des fichiers dans une hiérarchie de répertoires selon des critères (nom, taille, date de modification) pour repérer l'installation potentielle de logiciels malveillants.

### **`ip` (IP utility / Utilitaire réseau)**

* **[EN]** Manages and displays network interface configurations, routing, and tunneling. Running `ip a` shows all active network card IP addresses and states.
* **[FR]** Gère et affiche les configurations des interfaces réseau, le routage et le tunnelage. Exécuter `ip a` affiche toutes les adresses IP et états des cartes réseau actives.

### **`ss` (Socket Statistics / Statistiques des Sockets)**

* **[EN]** Dumps socket statistics to display active network connections, listening ports, and associated PIDs (`ss -tulnp`). Used to detect rogue open ports.
* **[FR]** Affiche les statistiques des sockets pour révéler les connexions réseau actives, les ports à l'écoute et les PID associés (`ss -tulnp`). Utilisé pour détecter les ports ouverts non autorisés.

### **`curl` / `wget` (Client URL & Web Get / Téléchargement web)**

* **[EN]** Transfer data from or to servers via command line. Defenders use them to fetch patches or test APIs; incident responders look for them in command histories to see if attackers downloaded malicious payloads.
* **[FR]** Transfèrent des données depuis ou vers des serveurs en ligne de commande. Les défenseurs les utilisent pour récupérer des correctifs ; les intervenants en incident vérifient s'ils ont servi à télécharger des charges utiles malveillantes.

---

## **2. Users & Permissions (Access Control) / Utilisateurs & Permissions (Contrôle d'Accès)**

### **Managing Permissions / Gestion des Permissions (`chmod`)**

* **[EN]** Linux uses Discretionary Access Control (DAC) with Read (4), Write (2), and Execute (1) permissions across User (u), Group (g), and Others (o).
* *Security Context:* `chmod 600` ensures only the owner can read/write sensitive files. Misconfigured permissions or improper SUID/SGID bits lead to local privilege escalation.


* **[FR]** Linux utilise un contrôle d'accès discrétionnaire (DAC) basé sur les droits Lecture (4), Écriture (2) et Exécution (1) pour l'Utilisateur (u), le Groupe (g) et les Autres (o).
* *Contexte de Sécurité :* `chmod 600` garantit que seul le propriétaire peut lire/écrire des fichiers sensibles. Des permissions mal configurées ou des bits SUID/SGID incorrects entraînent une élévation locale de privilèges.



### **File Ownership / Propriété des Fichiers (`chown`)**

* **[EN]** Changes file and group ownership (e.g., `sudo chown root:root /etc/crontab`). Ensuring system files are owned by `root` prevents unprivileged users from tampering with administrative tasks.
* **[FR]** Modifie la propriété des fichiers et des groupes (ex. `sudo chown root:root /etc/crontab`). S'assurer que les fichiers système appartiennent à `root` empêche les utilisateurs non privilégiés d'altérer les tâches administratives.

---

### **`chmod` (Change Mode / Modifier les permissions)**

* **[EN]** Modifies the read (4), write (2), and execute (1) access permissions of a file or directory for user, group, and others. (e.g., `chmod 700 script.sh`).
* **[FR]** Modifie les droits d'accès en lecture (4), écriture (2) et exécution (1) d'un fichier ou d'un dossier pour l'utilisateur, le groupe et les autres.

### **`chown` (Change Owner / Modifier le propriétaire)**

* **[EN]** Changes the user and/or group ownership of a file system object (e.g., `sudo chown root:root /etc/cron.d/`). Prevents low-privilege users from owning system automation scripts.
* **[FR]** Modifie le propriétaire utilisateur et/ou groupe d'un objet du système de fichiers pour empêcher les utilisateurs à faibles privilèges d'altérer les scripts système.

---

## **3. SSH Hardening / Sécurisation du Service SSH**

* **Authentication / Authentification:**
* **[EN]** Moving from password-based authentication to **SSH Key-Pair Authentication** protects systems against automated brute-force attacks.
* **[FR]** Passer d'une authentification par mot de passe à une **authentification par clé SSH** protège les systèmes contre les attaques par force brute automatisées.


* **Configuration Hardening (`/etc/ssh/sshd_config`) / Hardening de la Configuration:**
* **`PermitRootLogin no`**:
* **[EN]** Disables direct root login over the network, forcing attackers to compromise a standard account first.
* **[FR]** Désactive la connexion directe en tant que root via le réseau, obligeant les attaquants à compromettre d'abord un compte standard.


* **`PasswordAuthentication no`**:
* **[EN]** Forces the use of cryptographic SSH keys for remote access.
* **[FR]** Impose l'utilisation de clés SSH cryptographiques pour l'accès à distance.


---

## **4. Process & Service Monitoring / Surveillance des Processus & Services**

### **Process Inspection / Inspection des Processus (`ps`, `top`)**

* **[EN]** `ps aux` lists active processes, PIDs, and execution paths. Security analysts search for suspicious background binaries running out of `/tmp` or mimicking valid system names.
* **[FR]** `ps aux` liste les processus actifs, les PID et les chemins d'exécution. Les analystes cherchent des exécutables suspects fonctionnant depuis `/tmp` ou imitant des noms système légitimes.

### **Service Management / Gestion des Services (`systemctl`, `journalctl`)**

* **[EN]** `systemctl status ufw` verifies that local firewalls and logging daemons are active and running without error.
* **[FR]** `systemctl status ufw` vérifie que les pare-feux locaux et les services de journalisation sont actifs et fonctionnent sans erreur.

---

### **`ps` (Process Status / État des processus)**

* **[EN]** Provides a snapshot of the current running processes. Using `ps aux` lists every process across all users along with resource consumption and invocation paths.
* **[FR]** Fournit un instantané des processus en cours d'exécution. L'utilisation de `ps aux` liste chaque processus pour tous les utilisateurs avec leur consommation de ressources.

### **`top` (Table of Processes / Tableau des processus)**

* **[EN]** Displays a dynamic, real-time view of running system tasks, CPU utilization, and memory usage. Helpful for identifying resource-hogging cryptominers or anomalous background strains.
* **[FR]** Affiche une vue dynamique en temps réel des tâches système en cours, de l'utilisation du processeur et de la mémoire. Utile pour identifier des mineurs de cryptomonnaie clandestins.

### **`kill` (Terminate Process / Terminer un processus)**

* **[EN]** Sends a termination signal to a specified Process ID (PID) to stop a runaway or malicious process (e.g., `kill -9 1234`).
* **[FR]** Envoie un signal de terminaison à un ID de processus (PID) spécifié pour arrêter un processus incontrôlé ou malveillant.

### **`systemctl` (System Control / Contrôle du système)**

* **[EN]** Controls and inspects the `systemd` system and service manager. Used to start, stop, enable, or check the status of security daemons like firewalls (`systemctl status ufw`).
* **[FR]** Contrôle et inspecte le gestionnaire de services `systemd`. Utilisé pour démarrer, arrêter, activer ou vérifier l'état des démons de sécurité comme les pare-feux.

### **`journalctl` (Journal Control / Contrôle du journal systemd)**

* **[EN]** Queries and displays logs generated by `systemd` journald, allowing analysts to trace boot errors, service failures, and system messages.
* **[FR]** Interroge et affiche les journaux générés par le journal `systemd`, permettant aux analystes de retracer les erreurs de démarrage et les défaillances de services.

---

## **5. Log Files & Forensic Evidence / Fichiers de Logs & Preuves Inforensiques**

* **`/var/log/auth.log` (or `/var/log/secure` on RedHat/CentOS):**
* **[EN]** Records all authentication attempts, SSH logins, and `sudo` executions. Vital for detecting brute-force attacks and unauthorized privilege escalation attempts.
* **[FR]** Enregistre toutes les tentatives d'authentification, connexions SSH et exécutions de `sudo`. Essentiel pour détecter les attaques par force brute et les tentatives d'élévation de privilèges non autorisées.


* **`/var/log/syslog` / `/var/log/messages`:**
* **[EN]** Captures general system events, device drivers, and service status changes.
* **[FR]** Capture les événements généraux du système, les pilotes de périphériques et les changements d'état des services.

---

## **6. Cron Jobs & Persistence / Tâches Cron & Persistance**
* **[EN]** Cron Jobs (`crontab -l, /etc/cron`): Used to schedule automated tasks. Attackers frequently modify cron jobs or system startup scripts to achieve persistence (ensuring malware stays active after a reboot). Security analysts inspect these directories during a forensic investigation to spot unauthorized background jobs.

* **[FR]** Tâches Cron (`crontab -l, /etc/cron`): Utilisées pour planifier des tâches automatisées. Les attaquants modifient fréquemment les tâches cron ou les scripts de démarrage pour assurer leur persistance (s'assurer que le malware reste actif après un redémarrage). Les analystes de sécurité inspectent ces répertoires lors d'une investigation forensique pour repérer les tâches d'arrière-plan non autorisées.

* Here is the breakdown with **specific commands explained individually** (what each command actually does), provided in both English and French so you can copy and paste them directly into your GitHub repository.

---

### **`crontab` (Cron Table / Table des tâches planifiées)**

* **[EN]** Manages the cron table files which determine recurring automated background jobs. `crontab -l` lists scheduled tasks; attackers often inject malicious entries here to ensure their scripts execute automatically on reboot or every hour.
* **[FR]** Gère les fichiers de table cron qui déterminent les tâches d'arrière-plan automatisées et récurrentes. `crontab -l` liste les tâches planifiées ; les attaquants y injectent souvent des scripts malveillants pour s'assurer qu'ils s'exécutent au redémarrage.
