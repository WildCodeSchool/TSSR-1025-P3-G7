## UTILISATION

## Connexion au domaine

| Type | Identifiant | Mot de passe |
|------|-------------|--------------|
| Admin domaine | tssr\Administrator | Azerty1* |
| Utilisateur | tssr\prenom.nom | (defini dans AD) |

---

## Configuration MicroSIP (VoIP)

1. Ouvrir MicroSIP
2. Clic droit sur l'icone - Account - Add

| Champ | CLIWIN01 | CLIWIN02 |
|-------|----------|----------|
| Account Name | Safi | Support |
| SIP Server | 172.16.10.5 | 172.16.10.5 |
| SIP Proxy | (vide) | (vide) |
| Username | 20001 | 20003 |
| Domain | 172.16.10.5 | 172.16.10.5 |
| Login | 20001 | 20003 |
| Password | 1234 | 1234 |
| Transport | UDP | UDP |

3. Save
4. Verifier statut "Online"

### Passer un appel

1. Taper le numero (ex: 20003)
2. Appuyer Entree ou clic sur le telephone vert

### Extensions disponibles

| Extension | Nom |
|-----------|-----|
| 20001 | Safi WILDER |
| 20002 | Admin IT |
| 20003 | Support |
| 20004 | Direction |

---

## Configuration Thunderbird (Mail)

1. Ouvrir Thunderbird
2. Ajouter un compte existant
3. Nom : Votre nom
4. Email : prenom.nom@tssr.lan
5. Mot de passe : (votre mot de passe)
6. Configuration manuelle :

| Parametre | Valeur |
|-----------|--------|
| Serveur entrant | IMAP |
| Hostname | mail.tssr.lan ou 172.16.30.2 |
| Port | 993 |
| SSL | SSL/TLS |
| Serveur sortant | SMTP |
| Hostname | mail.tssr.lan ou 172.16.30.2 |
| Port | 587 |
| SSL | STARTTLS |

7. Terminer
8. Accepter le certificat (auto-signe)

---

## Acces aux services

| Service | Acces | Identifiants |
|---------|-------|--------------|
| GLPI | http://172.16.10.3 | Compte AD ou glpi/glpi |
| Webmail | https://172.16.30.2/mail | prenom.nom@tssr.lan |
| FreePBX | http://172.16.10.5 | admin / Azerty1* |

---

## Commandes utiles

### Verifier la configuration reseau


ipconfig /all


### Tester la connectivite


ping 172.16.20.1       # Passerelle
ping 172.16.10.2       # DNS (SRVWIN01)
ping 172.16.10.3       # GLPI
ping 172.16.30.2       # Mail


### Tester la resolution DNS


nslookup srvwin01.tssr.lan
nslookup mail.tssr.lan


### Verifier le domaine


systeminfo | findstr Domain
nltest /dsgetdc:tssr.lan


### Forcer la mise a jour GPO


gpupdate /force


### Forcer la detection WSUS


wuauclt /detectnow
wuauclt /reportnow


---

## Depannage

| Probleme | Solution |
|----------|----------|
| Pas d'IP | Verifier cable + DHCP sur SRVWIN01 |
| Impossible de joindre domaine | Verifier DNS = 172.16.10.2 |
| MicroSIP offline | Verifier SIP Server = 172.16.10.5 |
| Mail ne fonctionne pas | Verifier serveur = 172.16.30.1 |
| Pas d'acces Internet | Verifier passerelle + regles FW |

### Reinitialiser la configuration reseau


netsh winsock reset
netsh int ip reset
ipconfig /release
ipconfig /renew
ipconfig /flushdns


---

## Raccourcis utiles

| Raccourci | Action |
|-----------|--------|
| Win + R | Executer |
| Win + I | Parametres |
| Win + X | Menu rapide |
| Win + L | Verrouiller |
| Ctrl + Shift + Esc | Gestionnaire des taches |

### Commandes Executer (Win + R)

| Commande | Action |
|----------|--------|
| ncpa.cpl | Connexions reseau |
| sysdm.cpl | Proprietes systeme |
| control | Panneau de configuration |
| cmd | Invite de commandes |
| mstsc | Bureau a distance |

---

Auteur : Safi  
Projet : Ekoloclast
