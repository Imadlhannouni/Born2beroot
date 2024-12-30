# Born2beroot

## Description
**Born2beroot** est un projet du cursus 42 qui introduit les étudiants au domaine de l’administration système et de la virtualisation. Vous serez amené à créer une machine virtuelle en respectant des consignes précises et à configurer un serveur Linux sécurisé.

## Objectifs
- Configurer un système d'exploitation Linux (à choisir entre **Debian** ou **Rocky Linux**) sur une machine virtuelle.
- Implémenter des mesures strictes de sécurité et de configuration.
- Gérer les utilisateurs, les permissions et les services.
- Créer un script de monitoring système en bash.

---

## Prérequis

Avant de commencer ce projet, assurez-vous d’avoir :

1. **Un hyperviseur compatible**
   - [VirtualBox](https://www.virtualbox.org/) (obligatoire sauf sur Mac M1/M2 où UTM est recommandé).

2. **Une image ISO de Linux**
   - [Debian Stable](https://www.debian.org/download/).
   - [Rocky Linux](https://rockylinux.org/download/).

3. **Un environnement fonctionnel pour gérer la virtualisation**.

---

## Installation de la Machine Virtuelle

### Configuration de la VM
- **Type de système :** Linux.
- **Version :** Debian (64-bit) ou Rocky Linux (64-bit).
- **RAM :** Minimum 1024 MB (2 Go recommandé).
- **Disque dur :** Minimum 8 Go (20 Go recommandé).

### Partitionnement
- Créez au moins **2 partitions chiffrées** avec LVM.
- Exemple attendu :
  - `/boot` : Partition séparée (200-500 Mo).
  - **Swap** : 2 à 4 Go.
  - **/** : Partition principale.

---

## Configuration du Système

### 1. **Hostname et utilisateurs**
- Hostname : `[login]42` (exemple : `wil42`).
- Créez un utilisateur non-root avec votre login.
- Cet utilisateur devra :
  - Appartenir aux groupes `user42` et `sudo`.
  - Suivre une politique stricte de mot de passe.

### 2. **Politique de mot de passe**
- Expiration tous les 30 jours.
- Minimum 10 caractères (majuscule, minuscule, chiffre, aucun caractère consécutif répété plus de 3 fois).
- Avertissement 7 jours avant expiration.
- Minimum 2 jours entre deux modifications.
- Le mot de passe root doit respecter ces règles.

### 3. **Pare-feu**
- Activez `UFW` (ou `firewalld` pour Rocky Linux) et ouvrez uniquement le port **4242**.
  ```bash
  sudo apt install ufw
  sudo ufw enable
  sudo ufw allow 4242
  ```

### 4. **Service SSH**
- Activez SSH sur le port **4242**.
- Désactivez l’accès root par SSH.
  ```bash
  sudo nano /etc/ssh/sshd_config
  # Modifiez "Port 22" en "Port 4242"
  # Modifiez "PermitRootLogin" en "no"
  ```

### 5. **Configuration stricte de sudo**
- Limitez les tentatives de mot de passe à 3.
- Affichez un message en cas d’erreur.
- Archivez toutes les actions sudo dans `/var/log/sudo/`.
- Activez le mode TTY.
- Restreignez les chemins utilisables par sudo.
  ```bash
  Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
  ```

---

## Script de Monitoring

Créez un script `monitoring.sh` en bash qui affiche les informations suivantes toutes les 10 minutes sur tous les terminaux (via `wall`) :

- Architecture et version du kernel.
- Nombre de processeurs physiques et virtuels.
- Utilisation de la mémoire vive (en MB et %).
- Utilisation du disque (en MB et %).
- Charge CPU (en %).
- Date et heure du dernier redémarrage.
- Statut de LVM (actif ou non).
- Nombre de connexions actives.
- Nombre d’utilisateurs connectés.
- Adresse IPv4 et MAC.
- Nombre de commandes exécutées avec sudo.

Exemple attendu :
```text
#Architecture: Linux wil 4.19.0-16-amd64 x86_64 GNU/Linux
#CPU physical : 1
#vCPU : 1
#Memory Usage: 74/987MB (7.50%)
#Disk Usage: 1009/2Gb (49%)
#CPU load: 6.7%
#Last boot: 2021-04-25 14:45
#LVM use: yes
#Connexions TCP : 1 ESTABLISHED
#User log: 1
#Network: IP 10.0.2.15 (08:00:27:51:9b:a5)
#Sudo : 42 cmd
```

---

## Partie Bonus

Les bonus seront pris en compte uniquement si la partie obligatoire est parfaite.

### Idées de bonus
- Configuration avancée de partitions.
- Installation d’un site web WordPress avec **lighttpd**, **MariaDB** et **PHP**.
- Implémentation d’un service additionnel utile (hors NGINX/Apache2).

---

## Rendu et Soutenance

1. **Fichier signature.txt**
   - Contient la signature SHA1 de votre disque virtuel.
   - Exemple de commande pour obtenir la signature :
     ```bash
     sha1sum rocky_serv.vdi
     ```

2. **Soutenance**
   - Expliquez votre configuration et justifiez vos choix.
   - Soyez prêts à répondre à des questions techniques sur votre système et votre script.

---

## Ressources Utiles

- [Documentation officielle Debian](https://www.debian.org/doc/)
- [Documentation officielle Rocky Linux](https://docs.rockylinux.org/)
- [Guide sur SELinux](https://selinuxproject.org/)
- [Tutoriels 42 sur GitHub](https://github.com/42-Resources)

---

## Auteur
**Nom :** Imad Lhannouni  

