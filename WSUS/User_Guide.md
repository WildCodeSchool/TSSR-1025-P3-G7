# SRVWIN04 - Guide d'utilisation

## Serveur WSUS (Windows Server Core)

---

## Connexion

### Console locale

Utilisateur : tssr\Administrator
Mot de passe : Azerty1*


### SSH

ssh Administrator@172.16.10.4


### PowerShell à distance (depuis SRVWIN01)

Enter-PSSession -ComputerName SRVWIN04.tssr.lan

### Console WSUS graphique (depuis SRVWIN01)

1. Tools → Windows Server Update Services
2. Action → Connect to Server
3. Server : SRVWIN04.tssr.lan | Port : 8530

---

## Interface SConfig

| Option | Action |
|--------|--------|
| 8 | Configuration réseau |
| 13 | Redémarrer |
| 14 | Arrêter |
| 15 | Sortir vers PowerShell |

Pour revenir à SConfig : taper `sconfig`

---

## Commandes WSUS

### Statut synchronisation

$wsus = Get-WsusServer
$wsus.GetSubscription().GetSynchronizationStatus()


### Lancer synchronisation


(Get-WsusServer).GetSubscription().StartSynchronization()


### Voir les ordinateurs

Get-WsusComputer -All | Select FullDomainName, LastReportedStatusTime

### Voir mises à jour non approuvées

Get-WsusUpdate -Approval Unapproved -Status Needed


### Approuver une mise à jour

Approve-WsusUpdate -Update $update -Action Install -TargetGroupName "Clients_Windows"


### Nettoyer WSUS

Invoke-WsusServerCleanup -CleanupObsoleteUpdates -CleanupUnneededContentFiles

---

## Maintenance

### Redémarrer le service

Restart-Service WsusService

### Voir l'espace disque

Get-PSDrive C

### Voir les logs

Get-EventLog -LogName Application -Source "Windows Server Update Services" -Newest 10

---

## Dépannage

| Problème | Solution |
|----------|----------|
| WSUS ne répond pas | Restart-Service WsusService |
| Client ne se connecte pas | Vérifier GPO + wuauclt /detectnow |
| Espace disque plein | Invoke-WsusServerCleanup |

---

## Forcer détection sur client

Sur le poste client :

wuauclt /detectnow  
wuauclt /reportnow  

---

Auteur : Safi   
Projet : Ekoloclast  
Date : /02/2026  
