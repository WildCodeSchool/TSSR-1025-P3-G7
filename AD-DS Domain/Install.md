# Installation et Configuration AD / DNS / DHCP - SRVWIN01

## Projet Ekoloclast - Infrastructure tssr.lan

---

## Informations du serveur

| Paramètre | Valeur |
|-----------|--------|
| Nom | SRVWIN01 |
| OS | Windows Server 2022 (GUI) |
| Rôles | AD DS, DNS, DHCP |
| IP | 172.16.10.2 |
| Masque | 255.255.255.240 (/28) |
| Gateway | 172.16.10.1 (FW02) |
| Domaine | tssr.lan |
| Réseau VirtualBox | LAN_SRV |

---

## 1. Configuration réseau

### Via interface graphique

1. Clic droit sur l’icône réseau → Open Network & Internet settings  
2. Change adapter options  
3. Clic droit sur Ethernet → Properties  
4. Internet Protocol Version 4 (TCP/IPv4)  
5. Configurer :
   - IP address : 172.16.10.2
   - Subnet mask : 255.255.255.240
   - Default gateway : 172.16.10.1
   - Preferred DNS : 127.0.0.1  
6. Valider

### Via PowerShell

    New-NetIPAddress -InterfaceAlias "Ethernet" `
     -IPAddress 172.16.10.2 `
     -PrefixLength 28 `
     -DefaultGateway 172.16.10.1

    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" `
     -ServerAddresses 127.0.0.1

---

## 2. Renommer le serveur

### Via interface graphique

1. Win + R → sysdm.cpl  
2. Change  
3. Nom : SRVWIN01  
4. Redémarrer

### Via PowerShell

    Rename-Computer -NewName "SRVWIN01" -Restart

---

## 3. Installation des rôles

### Via Server Manager

1. Manage → Add Roles and Features  
2. Installation type : Role-based  
3. Serveur : SRVWIN01  
4. Cocher :
   - Active Directory Domain Services
   - DNS Server
   - DHCP Server  
5. Add Features  
6. Install

### Via PowerShell

    Install-WindowsFeature AD-Domain-Services, DNS, DHCP `
     -IncludeManagementTools

---

## 4. Promotion en contrôleur de domaine

### Via interface graphique

1. Server Manager → drapeau ⚠️  
2. Promote this server to a domain controller  
3. Add a new forest  
4. Root domain name : tssr.lan  
5. Forest & Domain functional level : Windows Server 2016  
6. DNS Server : activé  
7. NetBIOS : TSSR  
8. Installer → redémarrage automatique

### Via PowerShell

    Install-ADDSForest `
     -DomainName "tssr.lan" `
     -DomainNetbiosName "TSSR" `
     -ForestMode "WinThreshold" `
     -DomainMode "WinThreshold" `
     -InstallDns:$true `
     -SafeModeAdministratorPassword (ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force) `
     -Force:$true

---

## 5. Configuration DNS

### Enregistrements A

| Nom | Adresse IP |
|----|------------|
| srvwin01 | 172.16.10.2 |
| srvlx02 | 172.16.10.3 |
| srvwin04 | 172.16.10.4 |

### Via PowerShell

    Add-DnsServerResourceRecordA -ZoneName "tssr.lan" -Name "srvwin01" -IPv4Address 172.16.10.2
    Add-DnsServerResourceRecordA -ZoneName "tssr.lan" -Name "srvlx02" -IPv4Address 172.16.10.3
    Add-DnsServerResourceRecordA -ZoneName "tssr.lan" -Name "srvwin04" -IPv4Address 172.16.10.4

### Forwarders DNS

    Add-DnsServerForwarder -IPAddress 8.8.8.8
    Add-DnsServerForwarder -IPAddress 8.8.4.4

---

## 6. Configuration DHCP

### Autoriser le serveur DHCP

    Add-DhcpServerInDC -DnsName "srvwin01.tssr.lan" -IPAddress 172.16.10.2

### Créer l’étendue VLAN 20

    Add-DhcpServerv4Scope -Name "VLAN20_Clients" `
     -StartRange 172.16.20.1 `
     -EndRange 172.16.20.10 `
     -SubnetMask 255.255.255.240 `
     -State Active

    Set-DhcpServerv4OptionValue -ScopeId 172.16.20.0 `
     -DnsServer 172.16.10.2 `
     -Router 172.16.20.1

---

## 7. Structure Active Directory

### Création des OUs

    New-ADOrganizationalUnit -Name "_Ekoloclast" -Path "DC=tssr,DC=lan"

    $departments = "Communication","Direction_Financiere","Direction_Generale",
    "Direction_Marketing","DSI","RD","RH","Services_Generaux",
    "Service_Juridique","Ventes"

    foreach ($dept in $departments) {
        New-ADOrganizationalUnit -Name $dept -Path "OU=_Ekoloclast,DC=tssr,DC=lan"
    }

### Groupes AGDLP (exemple DSI)

    New-ADGroup -Name "GG_DSI" -GroupScope Global -Path "OU=DSI,OU=_Ekoloclast,DC=tssr,DC=lan"
    New-ADGroup -Name "DL_DSI" -GroupScope DomainLocal -Path "OU=DSI,OU=_Ekoloclast,DC=tssr,DC=lan"
    Add-ADGroupMember -Identity "DL_DSI" -Members "GG_DSI"

### Création utilisateur

    New-ADUser -Name "Safi Wilder" `
     -SamAccountName "s.wilder" `
     -UserPrincipalName "s.wilder@tssr.lan" `
     -Path "OU=DSI,OU=_Ekoloclast,DC=tssr,DC=lan" `
     -AccountPassword (ConvertTo-SecureString "Azerty1*" -AsPlainText -Force) `
     -Enabled $true

    Add-ADGroupMember -Identity "GG_DSI" -Members "s.wilder"

---

## 8. GPO essentielles

### Stratégie mot de passe (PSO)
Tools → Active Directory Administrative Center
tssr (local) → System → Password Settings Container
- Longueur minimale : 12
- Complexité : activée
- Expiration : 90 jours
- Verrouillage : 5 tentatives / 30 minutes

---

## 9. Vérifications

    Get-Service NTDS, DNS, DHCPServer
    Resolve-DnsName srvwin04.tssr.lan
    Get-ADDomainController
    dcdiag /v

---

Auteur : Safiullah  
Projet : Ekoloclast - Infrastructure tssr.lan
Date : 02/2026
