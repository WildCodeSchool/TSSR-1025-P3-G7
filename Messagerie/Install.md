# SRVLX01 - Installation

## Serveur Messagerie iRedMail (Debian 12)

---

## Informations

| Parametre | Valeur |
|-----------|--------|
| Nom | SRVLX01 |
| Hostname FQDN | mail.tssr.lan |
| OS | Debian 12 (Bookworm) - Sans GUI |
| IP | 172.16.30.1/28 |
| Passerelle | 172.16.30.14 (FW01 DMZ) |
| DNS | 172.16.10.2 |
| Zone | DMZ |
| Domaine mail | tssr.lan |

---

## Preparation VirtualBox

- Type : Linux
- Version : Debian (64-bit)
- RAM : 4096 MB (minimum pour iRedMail)
- Disque : 40 GB
- Reseau : Internal Network - DMZ

---

## Installation Debian 12

### Telecharger ISO

https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-12.9.0-amd64-netinst.iso

### Installation

1. Demarrer sur ISO - Selectionner Install (pas Graphical)
2. Langue : Francais | Clavier : Francais
3. Nom machine : mail | Domaine : tssr.lan
4. Mot de passe root : Azerty1*
5. Utilisateur : admin / Azerty1*
6. Partitionnement : Disque entier

### Selection des logiciels (IMPORTANT)

Decocher tout sauf :
- [x] Serveur SSH
- [x] Utilitaires usuels du systeme

Installer GRUB - Redemarrer - Retirer ISO

---

## Configuration reseau

nano /etc/network/interfaces

auto lo
iface lo inet loopback

auto enp0s3
iface enp0s3 inet static
    address 172.16.30.1
    netmask 255.255.255.240
    gateway 172.16.30.14
    dns-nameservers 172.16.10.2
    dns-search tssr.lan


systemctl restart networking

---

## Configuration hostname


hostnamectl set-hostname mail.tssr.lan

nano /etc/hosts


Contenu /etc/hosts :


127.0.0.1       localhost
172.16.30.1     mail.tssr.lan mail


Verifier : hostname -f doit afficher mail.tssr.lan

---

## Preparation systeme


apt update && apt upgrade -y
apt install -y wget curl bzip2 gzip
reboot


---

## Installation iRedMail

su -
cd /root
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.7.1.tar.gz
tar -xzf 1.7.1.tar.gz
cd iRedMail-1.7.1
bash iRedMail.sh


### Assistant installation

| Ecran | Choix |
|-------|-------|
| Welcome | y |
| Mail storage | /var/vmail |
| Web server | Nginx |
| Backend | MariaDB |
| MySQL root password | Azerty1* |
| First mail domain | tssr.lan |
| Admin password | Azerty1* |
| Components | Roundcubemail, iRedAdmin, Fail2ban |

Apres installation : reboot

---

## Configuration DNS sur SRVWIN01

| Type | Nom | Valeur |
|------|-----|--------|
| A | mail | 172.16.30.1 |
| MX | @ | mail.tssr.lan (priorite 10) |

---

## Regles FW01 pfSense

### Interface DMZ

| Source | Destination | Port | Description |
|--------|-------------|------|-------------|
| DMZ net | any | 80, 443 | Web sortant |
| DMZ net | any | 25, 587 | SMTP sortant |

### Interface TRANSIT

| Source | Destination | Port | Description |
|--------|-------------|------|-------------|
| NET_Internal | 172.16.30.1 | 80, 443 | Webmail |
| NET_Internal | 172.16.30.1 | 587, 993 | SMTP/IMAP |

---

## Verification

systemctl status postfix dovecot nginx mariadb
ss -tlnp | grep -E "25|587|993|443"


---

## Acces

| Service | URL | Identifiants |
|---------|-----|--------------|
| Webmail | https://172.16.30.1/mail/ | utilisateur@tssr.lan |
| iRedAdmin | https://172.16.30.1/iredadmin/ | postmaster@tssr.lan / Azerty1* |

---

## Creer utilisateur mail

### Via iRedAdmin

1. https://172.16.30.1/iredadmin/
2. Login : postmaster@tssr.lan
3. Add - User - Email + Password - Add

### Via CLI

cd /root/iRedMail-1.7.1/tools/
bash create_mail_user_SQL.sh utilisateur@tssr.lan motdepasse

---

## Configuration client mail

| Parametre | Valeur |
|-----------|--------|
| Serveur IMAP | mail.tssr.lan |
| Port IMAP | 993 (SSL/TLS) |
| Serveur SMTP | mail.tssr.lan |
| Port SMTP | 587 (STARTTLS) |

---

## Ports utilises

| Port | Service |
|------|---------|
| 25 | SMTP |
| 587 | Submission |
| 993 | IMAPS |
| 443 | HTTPS |

---

Auteur : Safiullah
Projet : Ekoloclast
