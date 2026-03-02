# Installation et Configuration pfSense - FW01

## Projet Ekoloclast - Infrastructure tssr.lan

---

## Informations du pare-feu

| Paramètre | Valeur |
|-----------|--------|
| Nom | FW01 |
| OS | pfSense CE 2.7.x |
| Rôle | Pare-feu / Routeur |
| Interface WAN | DHCP (vers Box Internet) |
| Interface LAN (TRANSIT) | 10.10.5.1/29 (Transit vers FW02) |
| Interface DMZ | 172.16.30.1 Vers Vlan DMZ |
| Login | admin |
| Mot de passe | pfsense (par défaut) |

---

## 1. Prérequis VirtualBox

### 1.1 Configuration VM

| Paramètre | Valeur |
|-----------|--------|
| RAM | 1024 MB |
| CPU | 1 vCPU |
| Disque | 10 GB |
| Adapter 1 | NAT ou Bridged (WAN) |
| Adapter 2 | Internal Network : Transit (LAN) |
| Adapter 3 | Internal Network : DMZ |

### 1.2 Téléchargement

- URL : https://www.pfsense.org/download/
- Architecture : AMD64
- Format : ISO Installer

---

## 2. Installation pfSense

### Démarrage et installation

1. Démarrer la VM avec l’ISO pfSense
2. Accept → Install pfSense
3. Keymap : French
4. Partitioning : Auto (ZFS ou UFS)
5. Sélection du disque
6. Confirmer l’installation
7. Reboot
8. Retirer l’ISO

---

## 3. Configuration initiale (console)

### Assignation des interfaces

    Should VLANs be set up now [y|n]? n
    Enter the WAN interface name: em0
    Enter the LAN interface name: em1
    Enter the DMZ interface name: OPT1
    Do you want to proceed [y|n]? y

### Configuration IP LAN

    Menu : 2 (Set interface IP address)
    Interface : LAN

    IPv4 address : 10.10.5.1
    Subnet bit count : 29
    Gateway : (Enter)
    IPv6 : (Enter)
    DHCP LAN : n
    Revert HTTP : y

### Vérification

    WAN : em0 → 192.168.1.176/24
    LAN : em1 → 10.10.5.1/29
    DMZ : OPT1 → 172.16.30.1/28

---

## 4. Accès Web GUI

1. Depuis une machine LAN
2. Navigateur : http://10.10.5.1
3. Login : admin
4. Password : pfsense

### Assistant initial

- Hostname : FW01
- Domain : tssr.lan
- DNS : 8.8.8.8 / 8.8.4.4
- Timezone : Europe/Paris
- WAN : 192.168.1.176
- LAN TRANSIT : 10.10.5.1 /29
- DMZ : 172.16.30.1
- Changement le mot de passe admin

---

## 5. Configuration des interfaces

### WAN

- IPv4 : DHCP
- Block private networks : désactivé
- Apply Changes

### LAN (Transit)

- Description : TRANSIT
- IP : 10.10.5.1
- Subnet : 29
- Apply Changes

### DMZ

- Description : DMZ
- IP : 172.16.30.1
- Subnet : 28
- Apply Changes



---

## 6. Routes statiques

### VLAN 10 Serveurs

| Paramètre | Valeur |
|-----------|--------|
| Réseau | 172.16.10.0/28 |
| Gateway | 10.10.5.2 |
| Description | VLAN10_Serveurs |

### VLAN 20 Clients

| Paramètre | Valeur |
|-----------|--------|
| Réseau | 172.16.20.0/28 |
| Gateway | 10.10.5.2 |
| Description | VLAN20_Clients |


### VLAN 30 DMZ

| Paramètre | Valeur |
|-----------|--------|
| Réseau | 172.16.30.0/28 |
| Gateway | 172.16.30.1/28 |
| Description | DMZ (Messagrie) |

---

## 7. Règles & Alias Firewall

## 7. Créer les Alias

Firewall → Aliases → Add :

| Nom | Type | Valeur |
|-----|------|--------|
| NET_Transit | Network | 10.10.5.0/29 |
| NET_DMZ | Network | 172.16.30.0/28 |
| NET_LAN_SRV | Network | 172.16.10.0/28 |
| NET_LAN_TEST | Network | 172.16.20.0/28 |
| NET_Internal | Network | NET_LAN_SRV, NET_LAN_TEST NET_Transit  |


## 7.1 Règles pare-feu

### Interface TRANSIT

| Action | Source | Destination | Port | Description |
|--------|--------|-------------|------|-------------|
| Pass | NET_Internal | any | any | LAN vers Internet |
| Pass | NET_Internal | NET_DMZ | 80,443,25,587,993 | LAN vers DMZ |

### Interface DMZ

| Action | Source | Destination | Port | Description |
|--------|--------|-------------|------|-------------|
| Pass | NET_DMZ | any | 80,443 | DMZ vers Internet |

---

## Route statique

System → Routing → Static Routes → Add :

| Destination | Gateway | Description |
|-------------|---------|-------------|
| 172.16.10.0/28 | 10.10.5.2 | Route vers LAN_SRV |
| 172.16.20.0/28 | 10.10.5.2 | Route vers LAN_TEST |

---

## NAT Outbound

Firewall → NAT → Outbound :

Mode : Hybrid  

Ajouter règle pour NET_Internal vers WAN.
---


Auteur : Safiullah  
Projet : Ekoloclast
Date : 02/2026
