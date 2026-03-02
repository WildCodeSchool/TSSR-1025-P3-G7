# FW01 - Guide d'utilisation

## Pare-feu pfSense

---

## Connexion

### Interface web


URL : https://10.10.5.1

User : admin
Password : pfsense


---

## Interfaces

| Interface | Nom | IP |
|-----------|-----|----|
| em0 | WAN | DHCP |
| em1 | TRANSIT | 10.10.5.1/29 |
| opt1 | DMZ | 172.16.30.1/28 |

---

## Gestion des règles

### Voir les règles

Firewall → Rules → Sélectionner interface

### Ajouter une règle

1. Firewall → Rules → Interface  
2. Add (flèche haut = début, flèche bas = fin)  
3. Configurer : Action, Source, Destination, Port  
4. Save → Apply Changes  

### Ordre des règles

Les règles sont évaluées de haut en bas.  
La première règle qui correspond est appliquée.

---

## Alias

### Voir les alias

Firewall → Aliases

### Alias actuels

| Nom | Contenu |
|-----|---------|
| NET_Transit | 10.10.5.0/29 |
| NET_DMZ | 172.16.30.0/28 |
| NET_LAN_SRV | 172.16.10.0/28 |
| NET_LAN_TEST | 172.16.20.0/28 |
| NET_Internal | NET_LAN_SRV + NET_LAN_TEST |

---

## NAT

### NAT Outbound

Firewall → NAT → Outbound

### Port Forward

Firewall → NAT → Port Forward

---

## Monitoring

### Logs

Status → System Logs → Firewall

### États des connexions

Diagnostics → States

### Trafic

Status → Traffic Graph

---

## Diagnostics

### Ping

Diagnostics → Ping

### Routes

Diagnostics → Routes

### Capture de paquets

Diagnostics → Packet Capture

---

## Commandes console

| Option | Action |
|--------|--------|
| 1 | Assigner interfaces |
| 2 | Configurer IP |
| 5 | Reboot |
| 6 | Halt |
| 8 | Shell |
| 9 | pfTop |
| 10 | Filtre logs|
| 11 | Redémarrer webConfigurator |

---

## Dépannage

| Problème | Solution |
|----------|----------|
| Pas d'accès web | Option 11 (console) |
| Pas de route | Vérifier Static Routes |
| Trafic bloqué | Vérifier logs Firewall |
| NAT ne fonctionne pas | Vérifier Outbound NAT |

---

## Sauvegarde

Diagnostics → Backup & Restore → Download configuration

---

Auteur : Safiullah 
Projet : Ekoloclast