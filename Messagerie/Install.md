# SRVLX01 - Installation

## Serveur Messagerie iRedMail

---

## Informations

| Paramètre | Valeur |
|-----------|--------|
| Nom | SRVLX01 |
| OS | Debian 12 (sans GUI) |
| IP | 172.16.30.2/28 |
| Passerelle | 172.16.30.1 (FW01 DMZ) |
| DNS | 172.16.10.2 |
| Zone | DMZ |
| Domaine mail | tssr.lan |

---

## Préparation VirtualBox

- Type : Linux
- Version : Debian (64-bit)
- RAM : 4096 MB
- Disque : 40 GB
- Réseau : Internal Network → DMZ

---

## Installation Debian 12

1. Installer Debian 12 netinst
2. Nom machine : mail
3. Domaine : tssr.lan
4. Mot de passe root : Azerty1*
5. Utilisateur : admin / Azerty1*
6. Sélection logiciels : SSH + Utilitaires (pas de GUI)

---

## Configuration réseau

nano /etc/network/interfaces


auto lo
iface lo inet loopback

auto enp0s3
iface enp0s3 inet static
    address 172.16.30.2
    netmask 255.255.255.240
    gateway 172.16.30.1
    dns-nameservers 172.16.10.2

systemctl restart networking

---

## Configuration hostname

hostnamectl set-hostname mail.tssr.lan

nano /etc/hosts

127.0.0.1       localhost
172.16.30.2     mail.tssr.lan mail

Vérifier :
hostname -f
Doit afficher : mail.tssr.lan

---

## Installation iRedMail

apt update && apt upgrade -y

cd /root
wget https://github.com/iredmail/iRedMail/archive/refs/tags/1.7.4.tar.gz
tar -xzf 1.7.4.tar.gz
cd iRedMail-1.7.4
bash iRedMail.sh

### Configuration wizard

| Paramètre | Valeur |
|-----------|--------|
| Mail storage | /var/vmail |
| Web server | Nginx |
| Backend | MariaDB |
| Domain | tssr.lan |
| Admin password | Azerty1* |
| Components | Roundcube, iRedAdmin, Fail2ban |
| Firewall | Yes |

---

## Configuration DNS sur SRVWIN01

Ajouter les enregistrements :

| Type | Nom | Valeur |
|------|-----|--------|
| A | mail | 172.16.30.2 |
| MX | @ | mail.tssr.lan (priorité 10) |

---

## Règles FW01 pfSense

| Source | Destination | Port | Description |
|--------|-------------|------|-------------|
| LAN | 172.16.30.2 | 80, 443 | Webmail |
| LAN | 172.16.30.2 | 587, 993 | SMTP/IMAP |

---

## Vérification

systemctl status postfix dovecot nginx mariadb
ss -tlnp | grep -E "25|587|993|80|443"

---

## Accès

| Service | URL |
|---------|-----|
| Webmail | https://172.16.30.2/mail |
| iRedAdmin | https://172.16.30.2/iredadmin |
| Admin | postmaster@tssr.lan / Azerty1* |

---

Auteur : Safiullah
Projet : Ekoloclast