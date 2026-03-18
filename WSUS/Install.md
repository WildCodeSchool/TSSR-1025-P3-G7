# SRVWIN04 - Installation

## Serveur WSUS (Windows Server Core)

---

## Informations

| Paramètre | Valeur |
|-----------|--------|
| Nom | SRVWIN04 |
| OS | Windows Server 2022 Core |
| IP | 172.16.10.4/28 |
| Passerelle | 172.16.10.1 |
| DNS | 172.16.10.2 |
| Domaine | tssr.lan |
| Port WSUS | 8530 |

---

## Préparation VirtualBox

- Type : Microsoft Windows
- Version : Windows 2022 (64-bit)
- RAM : 4048 MB
- Disque : 60 GB
- Réseau : Internal Network → LAN_SRV

---

## Installation

1. Installer Windows Server 2022 Standard (sans Desktop Experience)
2. Mot de passe Administrator : Azerty1*  
![intal_set](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/79cc632ab50bfc0b43551989d8920d2cb06a63f9/WSUS/Resources/CoreWsusInstalation.png)

---

## Configuration via SConfig

Au démarrage, l'écran SConfig s'affiche.

### Réseau (option 8)

1. Taper 8 → Sélectionner adaptateur
2. Option 1 → S (Static)
3. IP : 172.16.10.4
4. Masque : 255.255.255.240
5. Passerelle : 172.16.10.1
6. Option 2 → DNS : 172.16.10.2  
![ping](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/79cc632ab50bfc0b43551989d8920d2cb06a63f9/WSUS/Resources/ping.png)

### Nom du serveur (option 2)

1. Taper 2 → SRVWIN04 → Redémarrer

### Bureau à distance (option 7)

1. Taper 7 → E (Enable) → 1 (NLA)

### Joindre le domaine (option 1)

1. Taper 1 → D (Domain)
2. Domaine : tssr.lan
3. Credentials : Administrator / Azerty1*
4. Redémarrer

---

### Installation WSUS (PowerShell)

Taper 15 pour sortir de SConfig, puis powershell.

### Créer le dossier WSUS
New-Item -Path "C:\WSUS" -ItemType Directory

### Installer le rôle
Install-WindowsFeature -Name UpdateServices -IncludeManagementTools

### Configuration post-installation
cd "C:\Program Files\Update Services\Tools"
.\wsusutil.exe postinstall CONTENT_DIR=C:\WSUS


---

### Configuration WSUS

Import-Module UpdateServices

### Synchronisation depuis Microsoft
Set-WsusServerSynchronization -SyncFromMU

### Synchroniser les catégories
$subscription = (Get-WsusServer).GetSubscription()
$subscription.StartSynchronizationForCategoryOnly()

### Sélectionner les produits
Get-WsusProduct | Where-Object {
    $_.Product.Title -in @('Windows 10', 'Windows 11', 'Windows Server 2022')
} | Set-WsusProduct

### Sélectionner les classifications
Get-WsusClassification | Where-Object {
    $_.Classification.Title -in @('Critical Updates', 'Security Updates')
} | Set-WsusClassification

### Créer les groupes
$wsus = Get-WsusServer
$wsus.CreateComputerTargetGroup("Serveurs")
$wsus.CreateComputerTargetGroup("Clients_Windows")

---

### Installation OpenSSH


Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic


---

### Pare-feu


New-NetFirewallRule -DisplayName "WSUS HTTP" -Direction Inbound -Protocol TCP -LocalPort 8530 -Action Allow
New-NetFirewallRule -DisplayName "WSUS HTTPS" -Direction Inbound -Protocol TCP -LocalPort 8531 -Action Allow


---
## Synchronisation AD DS et WSUS

Pour pouvoir Gérer le serveur WSUS depuis le serveur WINSRV01 AD-DS il faut installer Windows Server Update Services Tools,    
Add roles and Features Wizard   
Features -> Remote Server Administration tools  
Administration tools -> Windows Server Update Services Tools.  
![add_role](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/79cc632ab50bfc0b43551989d8920d2cb06a63f9/WSUS/Resources/add_features_win01_WSUS.png)

### Connection avec Serveur SRVWIN02 WSUS depuis update service de SRVWIN01 AD-DS  
![update_service_conec](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/79cc632ab50bfc0b43551989d8920d2cb06a63f9/WSUS/Resources/CoreWsus_AD_connectionn.png)


### GPO sur SRVWIN01

1. Group Policy Management → Créer GPO_WSUS_Config
2. Computer Configuration → Administrative Templates → Windows Update
3. Configure Automatic Updates : Enabled
4. Specify intranet update service : http://srvwin04.tssr.lan:8530

---

### Vérification


Get-Service WsusService
(Get-WsusServer).GetSubscription().GetSynchronizationStatus()

---

Auteur : Safiullah 
Projet : Ekoloclast
