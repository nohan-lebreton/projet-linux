# Documentation Complète Installation et Configuration VPS
userName: adminuser
password: admin
public_key: 4kMFlp5svVX2KWPirqZESIDkHmeWVM7JcOS6Lvyf38MzDlXQpj71OEFSpZb0xhfyQRLJluony/Cf/kI8qD7wYrsDuG5NsIj1NewhTLYQwby62ARYyZ4D2UzZ9gz0HsIT6PKzncSZ3OvmYIzq0rJDJTadI1rviYJT16ntSl+urORrWSQnMuYVuaTbEny40uu/lDTEAaCqeyoHuNMtixFQuaaHkcgmQnH1daUL8JCltmyQzivSkFU/cYFtqxLKpuvvV+Yz7hTOlj6R5qlaQIcLhFVIlRWp/wCO4LKkSMbRzFBbra7nmt158xao3cks8cBa5eQ/WCkV4cQ4oZrcq2AfGtJ8sv5oxQv0PBUTwf1NzrpmQ5p0ObkuJDWuuGlTX4ajMDanBPXy88L40oMVSnmpPGW1TjD3G0KrQfgbjcIClSsEJONuXSbSfUr0uJR+KpJ4VQink/5iiKzY7YC4DeMScwlRU5nAihBamdBmJA2znxM0tohKp5TjL8njBhY4mrpu9MjlFlqMK1bgBpbvTRGIaLYA2PhXCcdU2K5NxRHt4KhameKZ8kecgtbJocIdzGGNG7MfMvS8BzNO+4wSL2XNwZba37TosMPpzCO2Ym9WlLXnimUs4X2VtEB1e3wgRJHdmOo0z589BhY1VkJw== lebreton.nohan@gmail.com

## Étape 0 - Installation des Outils de Base

### Outils Système
1. **htop**
   - Installation : `sudo apt install htop`
   - Rôle : Moniteur système interactif pour visualiser les processus
   - Usage : `htop`

2. **curl**
   - Installation : `sudo apt install curl`
   - Rôle : Transfert de données via différents protocoles
   - Usage : `curl https://example.com`

3. **wget**
   - Installation : `sudo apt install wget`
   - Rôle : Téléchargement de fichiers depuis le web
   - Usage : `wget https://example.com/file.zip`

4. **zsh & oh-my-zsh**
   - Installation : 
     ```bash
     sudo apt install zsh
     sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
     ```
   - Rôle : Shell amélioré avec plus de fonctionnalités
   - Usage : Définir comme shell par défaut : `chsh -s $(which zsh)`

### Outils de Développement
5. **git**
   - Installation : `sudo apt install git`
   - Rôle : Système de contrôle de version
   - Usage : `git clone https://github.com/user/repo.git`

6. **nano**
   - Installation : `sudo apt install nano`
   - Rôle : Éditeur de texte simple
   - Usage : `nano fichier.txt`

7. **vim**
   - Installation : `sudo apt install vim`
   - Rôle : Éditeur de texte avancé
   - Usage : `vim fichier.txt`

8. **tree**
   - Installation : `sudo apt install tree`
   - Rôle : Affichage arborescent des répertoires
   - Usage : `tree /chemin/du/dossier`

9. **lsof**
   - Installation : `sudo apt install lsof`
   - Rôle : Liste les fichiers ouverts
   - Usage : `lsof -i :80`

10. **ncdu**
    - Installation : `sudo apt install ncdu`
    - Rôle : Analyseur d'utilisation du disque
    - Usage : `ncdu /`
    - 
### Outils de Sécurité
11. **ufw**
    - Installation : `sudo apt install ufw`
    - Rôle : Pare-feu simple
    - Usage : `sudo ufw enable`

12. **fail2ban**
    - Installation : `sudo apt install fail2ban`
    - Rôle : Protection contre les tentatives de connexion abusives
    - Usage : `sudo systemctl status fail2ban`
      
13. **sudo**
    - Installation : `sudo apt install fail2ban`
    - Rôle : Protection contre les tentatives de connexion abusives
    - Usage : `sudo systemctl status fail2ban`

14. **cron**
    - Installation : `sudo apt install cron`
    - Rôle : Planification de tâches automatisées
    - Usage : `crontab -e pour éditer les tâches planifiées`

15. **sudo**
    - Installation : `apt install sudo (si non installé par défaut)`
    - Rôle : Permet d'exécuter des commandes en tant qu'administrateur
    - Usage : `sudo commande (ex: sudo apt update)`
      
16. **openssh-server**
    - Installation : `sudo apt install openssh-server`
    - Rôle : Serveur SSH
    - Usage : `sudo systemctl status ssh`

### Outils Système Additionnels
17. **rsync**
    - Installation : `sudo apt install rsync`
    - Rôle : Synchronisation de fichiers
    - Usage : `rsync -av source/ destination/`

18. **net-tools**
    - Installation : `sudo apt install net-tools`
    - Rôle : Outils réseau classiques (netstat, etc.)
    - Usage : `netstat -tulpn`

19. **iproute2**
    - Installation : `sudo apt install iproute2`
    - Rôle : Outils de configuration réseau modernes
    - Usage : `ip a`

20. **tmux**
    - Installation : `sudo apt install tmux`
    - Rôle : Multiplexeur de terminal
    - Usage : `tmux new -s session_name`

21. **lsb-release**
    - Installation : `sudo apt install lsb-release`
    - Rôle : Affichage des informations sur la distribution Linux
    - Usage : `lsb_release -a pour voir la version du système`
      
## Étape 1 - Déploiement Initial

### Mise à jour du système
```bash
sudo apt update && sudo apt upgrade -y
```

### Création utilisateur admin
```bash
sudo adduser adminuser
sudo usermod -aG sudo adminuser
```

### Configuration SSH
```bash
# Génération de clé sur le poste local
ssh-keygen -t ed25519 -C "votre@email.com"

# Configuration SSH (/etc/ssh/sshd_config)
```
![image](https://github.com/user-attachments/assets/e60c189c-7c7b-410e-9840-5b570734de7b)

## Étape 2 - Sécurisation

### Configuration UFW
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```
![image](https://github.com/user-attachments/assets/3fb604e7-0bb7-499b-9173-bb23d9988333)
(le port 12001 servira plus tard pour netdata)

### Configuration Fail2ban
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
# Configuration dans jail.local
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/journal/*/system.journal
bantime = 10m
```
![image](https://github.com/user-attachments/assets/1fc648b6-cdd8-4bf7-84ae-3e0fe464d585)
![image](https://github.com/user-attachments/assets/50b1c570-a460-4d6f-956b-e5a14ba8bda3)

## Étape 3 - Monitoring

Installation de Netdata :
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```
![netdata](https://github.com/user-attachments/assets/436ab22c-41bb-40a0-9ece-684892ac6521)
![netdata interface](https://github.com/user-attachments/assets/7cf865eb-0f3a-4fae-b920-24c6ae268fb7)

## Étape 4 - Serveur Web & Reverse Proxy

### Installation Nginx
```bash
sudo apt install nginx
sudo systemctl enable nginx
```

### Configuration Reverse Proxy
```nginx
server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
![nginx](https://github.com/user-attachments/assets/54ca7f7c-99a3-4492-993b-8a6306d9f7cd)
### Installation Certbot
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d example.com
```
![image](https://github.com/user-attachments/assets/73c2ca17-09a5-4ec4-8974-01c793691e8a)

## Étape 5 - Test de Charge

Résultats du test avec Apache Benchmark :
```bash
ab -n 1000 -c 10 http://localhost:3000/
```
- Requêtes par seconde : 2404.48
- Temps moyen par requête : 4.159 ms
- 95% des requêtes en moins de 6 ms
- Aucune requête échouée
![test](https://github.com/user-attachments/assets/39910f40-345f-4cce-b706-9aef00b15b0c)

## Étape 6 - Maintenance & Sauvegarde

Script de backup créé : `/home/adminuser/backup_script.sh`
```bash
# Exécution toutes les 6 heures via cron
0 */6 * * * /home/adminuser/backup_script.sh
```
![cron](https://github.com/user-attachments/assets/7ff77a8b-f230-4498-9976-f6c9f36af5ee)

### Monitoring et Maintenance
- Vérification régulière des logs : `tail -f /var/log/syslog`
- Surveillance espace disque : `df -h`
- Surveillance services : `systemctl status nginx`
