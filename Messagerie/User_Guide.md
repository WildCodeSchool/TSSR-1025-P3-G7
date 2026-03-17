# SRVLX01 - Guide d'utilisation

## Serveur Messagerie iRedMail (Debian 12)

---

## Connexion

### SSH


ssh admin@172.16.30.1
Password : Azerty1*

# Pour root
su -


### Interface web

| Service | URL | Identifiants |
|---------|-----|--------------|
| Webmail | https://172.16.30.1/mail/ | utilisateur@tssr.lan |
| iRedAdmin | https://172.16.30.1/iredadmin/ | postmaster@tssr.lan / Azerty1* |

---

## Gestion des utilisateurs

### Creer un utilisateur (via iRedAdmin)

1. Acceder a https://172.16.30.1/iredadmin/
2. Se connecter : postmaster@tssr.lan / Azerty1*
3. Add - User
4. Remplir : email, mot de passe, quota
5. Add

### Creer un utilisateur (via CLI)


cd /root/iRedMail-1.7.1/tools/
bash create_mail_user_SQL.sh utilisateur@tssr.lan motdepasse


### Supprimer un utilisateur

Via iRedAdmin : Users - Selectionner - Delete

---

## Configuration client mail

| Parametre | Valeur |
|-----------|--------|
| Serveur IMAP | mail.tssr.lan ou 172.16.30.1 |
| Port IMAP | 993 (SSL/TLS) |
| Serveur SMTP | mail.tssr.lan ou 172.16.30.1 |
| Port SMTP | 587 (STARTTLS) |
| Utilisateur | email complet (ex: safi@tssr.lan) |

### Thunderbird

1. Ajouter un compte
2. Email : safi@tssr.lan
3. Configuration manuelle
4. IMAP : mail.tssr.lan, 993, SSL/TLS
5. SMTP : mail.tssr.lan, 587, STARTTLS
6. Accepter le certificat auto-signe

---

## Commandes utiles

### Voir les services


systemctl status postfix
systemctl status dovecot
systemctl status nginx
systemctl status mariadb


### Redemarrer les services


systemctl restart postfix
systemctl restart dovecot
systemctl restart nginx


### Voir les logs mail


tail -f /var/log/mail.log


### Voir la file d'attente mail


postqueue -p


### Forcer l'envoi de la file


postqueue -f


### Supprimer tous les mails en attente


postsuper -d ALL


---

## Test envoi mail


apt install mailutils -y
echo "Test body" | mail -s "Test subject" admin@tssr.lan


---

## Depannage

| Probleme | Solution |
|----------|----------|
| Webmail inaccessible | systemctl restart nginx |
| Login echoue | Verifier utilisateur dans iRedAdmin |
| Mail non envoye | Verifier postqueue -p + logs |
| Certificat SSL | Accepter l'exception (auto-signe) |
| Connexion refusee | Verifier regles FW01 |

### Verifier les ports


ss -tlnp | grep -E "25|587|993|443"


---

## Sauvegarde

### Sauvegarder les mails


tar -czvf /root/backup_vmail_$(date +%Y%m%d).tar.gz /var/vmail


### Sauvegarder la config


tar -czvf /root/backup_config_$(date +%Y%m%d).tar.gz /etc/postfix /etc/dovecot /etc/nginx


### Sauvegarder la base


mysqldump -u root -p --all-databases > /root/backup_db_$(date +%Y%m%d).sql


---

## Fichiers de configuration

| Fichier | Description |
|---------|-------------|
| /etc/postfix/main.cf | Configuration Postfix |
| /etc/dovecot/dovecot.conf | Configuration Dovecot |
| /etc/nginx/sites-enabled/ | Configuration Nginx |
| /root/iRedMail-1.7.1/iRedMail.tips | Infos post-installation |
| /var/vmail | Stockage des mails |

---

## Ports utilises

| Port | Service |
|------|---------|
| 25 | SMTP (serveur a serveur) |
| 587 | SMTP Submission (clients) |
| 993 | IMAPS (SSL) |
| 995 | POP3S (SSL) |
| 80 | HTTP (redirection) |
| 443 | HTTPS (webmail) |

---

Auteur : Safi | Projet : Ekoloclast
