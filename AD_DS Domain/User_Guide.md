# SRVWIN01 - Guide d'utilisation

## Serveur Active Directory / DNS / DHCP

---

## Connexion
Utilisateur : tssr\Administrator
Mot de passe : Azerty1*


---

## Outils d'administration

| Outil                       | Accès                               |
|----------------------------|--------------------------------------|
| AD Users and Computers     | Tools → AD Users and Computers       |
| AD Administrative Center   | Tools → AD Administrative Center     |
| DNS Manager                | Tools → DNS                         |
| DHCP Manager               | Tools → DHCP                        |
| Group Policy Management    | Tools → Group Policy Management      |

---

## Gestion des utilisateurs

### Créer un utilisateur

1. AD Users and Computers → tssr.lan → OU concernée  
2. Clic droit → New → User  
3. Remplir les informations → Définir mot de passe  

### Réinitialiser un mot de passe

1. Trouver l'utilisateur  
2. Clic droit → Reset Password  

### Débloquer un compte

1. Clic droit → Properties → Account  
2. Décocher "Unlock account"  

---

## Gestion DNS

### Ajouter un enregistrement A

1. DNS → Forward Lookup Zones → tssr.lan  
2. Clic droit → New Host (A)  
3. Nom + IP → Add Host  

### Ajouter un enregistrement MX

1. Clic droit sur tssr.lan → New Mail Exchanger  
2. Serveur : mail.tssr.lan | Priorité : 10  

---

## Gestion DHCP

### Voir les baux

1. DHCP → IPv4 → Scope → Address Leases  

### Créer une réservation

1. Reservations → Clic droit → New Reservation  
2. Nom + IP + MAC  

---

## Commandes utiles


# Utilisateurs AD
Get-ADUser -Filter *

# DNS
nslookup mail.tssr.lan

# DHCP
Get-DhcpServerv4Lease -ScopeId 172.16.20.0

# PSO utilisateur
Get-ADUserResultantPasswordPolicy -Identity "nom.utilisateur"

Dépannage
Problème	Solution
Utilisateur bloqué	Properties → Account → Unlock
DNS ne résout pas	Vérifier enregistrements + Forwarders
Pas d'IP DHCP	Vérifier scope activé

Auteur : Safiullah
Projet : Ekoloclast
