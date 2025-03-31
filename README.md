# Documentation Complète Installation et Configuration VPS

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

### Outils de Sécurité
11. **ufw**
    - Installation : `sudo apt install ufw`
    - Rôle : Pare-feu simple
    - Usage : `sudo ufw enable`

12. **fail2ban**
    - Installation : `sudo apt install fail2ban`
    - Rôle : Protection contre les tentatives de connexion abusives
    - Usage : `sudo systemctl status fail2ban`

13. **openssh-server**
    - Installation : `sudo apt install openssh-server`
    - Rôle : Serveur SSH
    - Usage : `sudo systemctl status ssh`

### Outils Système Additionnels
14. **rsync**
    - Installation : `sudo apt install rsync`
    - Rôle : Synchronisation de fichiers
    - Usage : `rsync -av source/ destination/`

15. **net-tools**
    - Installation : `sudo apt install net-tools`
    - Rôle : Outils réseau classiques (netstat, etc.)
    - Usage : `netstat -tulpn`

16. **iproute2**
    - Installation : `sudo apt install iproute2`
    - Rôle : Outils de configuration réseau modernes
    - Usage : `ip a`

17. **tmux**
    - Installation : `sudo apt install tmux`
    - Rôle : Multiplexeur de terminal
    - Usage : `tmux new -s session_name`

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
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

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

### Configuration Fail2ban
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
# Configuration dans jail.local
[sshd]
enabled = true
bantime = 3600
findtime = 600
maxretry = 5
```

## Étape 3 - Monitoring

Installation de Netdata :
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

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

### Installation Certbot
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d example.com
```

## Étape 5 - Test de Charge

Résultats du test avec Apache Benchmark :
```bash
ab -n 1000 -c 10 http://localhost:3000/
```
- Requêtes par seconde : 2404.48
- Temps moyen par requête : 4.159 ms
- 95% des requêtes en moins de 6 ms
- Aucune requête échouée

## Étape 6 - Maintenance & Sauvegarde

Script de backup créé : `/home/adminuser/backup_script.sh`
```bash
# Exécution toutes les 6 heures via cron
0 */6 * * * /home/adminuser/backup_script.sh
```

### Monitoring et Maintenance
- Vérification régulière des logs : `tail -f /var/log/syslog`
- Surveillance espace disque : `df -h`
- Surveillance services : `systemctl status nginx`
