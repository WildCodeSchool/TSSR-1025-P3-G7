# CLIWIN01 / CLIWIN02 - Installation et Utilisation

## Postes Clients Windows 10

---

## Informations

| Parametre | CLIWIN01 | CLIWIN02 |
|-----------|----------|----------|
| OS | Windows 10 Pro | Windows 10 Pro |
| IP | DHCP ou statique | DHCP ou statique |
| Passerelle | 172.16.20.1 | 172.16.20.1 |
| DNS | 172.16.10.2 | 172.16.10.2 |
| Zone | LAN_TEST | LAN_TEST |
| Domaine | tssr.lan | tssr.lan |

---

## Preparation VirtualBox

| Parametre | Valeur |
|-----------|--------|
| Type | Microsoft Windows |
| Version | Windows 10 (64-bit) |
| RAM | 2048 MB |
| Disque | 50 GB |
| Reseau | Internal Network - LAN_TEST |

---

# PARTIE 1 : INSTALLATION

## Installation Windows 10

1. Demarrer sur l'ISO Windows 10
2. Langue : Francais
3. Installer maintenant
4. Je n'ai pas de cle de produit (ou entrer une cle)
5. Choisir : Windows 10 Pro
6. Accepter licence
7. Installation personnalisee
8. Selectionner le disque - Suivant
9. Attendre l'installation
10. Redemarrer

## Configuration initiale

1. Region : France
2. Clavier : Francais
3. Ignorer le deuxieme clavier
4. Compte local : Utiliser un compte hors connexion
5. Nom utilisateur : Admin
6. Mot de passe : Azerty1*
7. Questions de securite : repondre
8. Parametres de confidentialite : tout desactiver
9. Terminer

---

## Configuration reseau

### Option 1 : DHCP (recommande)

Le serveur DHCP sur SRVWIN01 attribue automatiquement :
- IP : 172.16.20.x
- Masque : 255.255.255.240
- Passerelle : 172.16.20.1
- DNS : 172.16.10.2

### Option 2 : IP statique

1. Parametres - Reseau et Internet - Ethernet
2. Modifier les options d'adaptateur
3. Clic droit sur Ethernet - Proprietes
4. IPv4 - Proprietes

| Parametre | CLIWIN01 | CLIWIN02 |
|-----------|----------|----------|
| IP | 172.16.20.2/DHCP | 172.16.20.3/DHCP |
| Masque | 255.255.255.240 | 255.255.255.240 |
| Passerelle | 172.16.20.1 | 172.16.20.1 |
| DNS | 172.16.10.2 | 172.16.10.2 |

---

## Joindre le domaine tssr.lan

1. Parametres - Systeme - A propos
2. Renommer ce PC (avance)
3. Modifier
4. Nom de l'ordinateur : CLIWIN01 (ou CLIWIN02)
5. Membre d'un : Domaine
6. Domaine : tssr.lan
7. Entrer les identifiants :
   - Utilisateur : Administrator
   - Mot de passe : Azerty1*
8. Bienvenue dans le domaine tssr.lan
9. Redemarrer

### Verification

systeminfo | findstr /B /C:"Domain"


Doit afficher : Domain: tssr.lan

---

## Installation logiciels

### MicroSIP (VoIP)

1. Telecharger : https://www.microsip.org/downloads
2. Installer MicroSIP
3. Configuration (voir partie utilisation)

### Thunderbird (Mail)

1. Telecharger : https://www.thunderbird.net/
2. Installer Thunderbird
3. Configuration (voir partie utilisation)

---

Auteur : Safiullah  
Projet : Ekoloclast
