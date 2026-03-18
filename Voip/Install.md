# IPBX01 - Installation

## Serveur VoIP FreePBX

---

## Informations

| Paramètre | Valeur |
|-----------|--------|
| Nom | IPBX01 |
| OS | FreePBX Distro 16 (CentOS) |
| IP | 172.16.10.5/28 |
| Passerelle | 172.16.10.1 |
| DNS | 172.16.10.2 |
| Zone | LAN_SRV |
| Port SIP | 5060 |
| Port RTP | 10000-20000 |

---

## Préparation VirtualBox

- Type : Linux
- Version : Red Hat (64-bit)
- RAM : 1024 MB
- Disque : 20 GB
- Réseau : Internal Network → LAN_SRV

---

## Installation FreePBX Distro

1. Télécharger ISO : https://www.freepbx.org/downloads/
2. Démarrer sur l'ISO
3. Choisir version recommandée  
![image_of_options](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/instal_freePBX.png)

4. Graphical Installation - Output to VGA  
![image_of_options](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/2_instal_freePBX_.png)

5. FreePBX Standard et choit de langue  
![image_of_options](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/3instal_freePBX.png)

6. Mot de passe root : Azerty1* (attention clavier QWERTY)
![image_of_options](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/4_instal_freePBX.png)

7. Attendre fin installation  
![image_of_options](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/5_instal_freePBX.png)

8. Retirer ISO → Redémarrer

---

## Configuration réseau

Se connecter en root.

nano /etc/sysconfig/network-scripts/ifcfg-eth0  


DEVICE=eth0  
BOOTPROTO=static  
ONBOOT=yes  
IPADDR=172.16.10.5  
NETMASK=255.255.255.240  
GATEWAY=172.16.10.14  
DNS1=172.16.10.2  


systemctl restart network

---

## Configuration clavier français


localectl set-locale LANG=fr_FR.utf8  
localectl set-keymap fr  
localectl set-x11-keymap fr  

![lang_et_keyboard_conf](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/6_freePBX_chng_lan.png)

---

## Création utilisateur SSH  

adduser safi    
passwd *************Mot de passe : Azerty1*

vi /etc/ssh/sshd_config

Modifier :

PermitRootLogin no  
AllowUsers safi  
PasswordAuthentication yes  


systemctl restart sshd

---

## Configuration web

1. Accéder à http://172.16.10.5  
![ipbx_web](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/6d9dc4f1cf7210423ef829a5d57827ab3f0f3075/Voip/Ressources/4_instal_freePBX.png)

2. Créer compte admin :  
   - Username : admin  
   - Password : Azerty1*  
   - Email : admin@tssr.lan  

![ipbx_web_conf](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/7_conf_initiale.png)

![ipbx_web_conf](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/8_conf_initiale.png)
---

## Création des extensions

1. Applications → Extensions  
2. SIP [chan_pjsip] Extensions  
3. + Add New SIP [chan_pjsip] Extension  

| Extension | Nom | Secret |
|-----------|-----|--------|
| 20001 | Safi WILDER | 1234 |
| 20002 | Admin IT | 1234 |
| 20003 | Support | 1234 |
| 20004 | Direction | 1234 |

4. Submit → Apply Config  
![Extension_add](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/11_cree_extension.png)

---

## Configuration DNS sur SRVWIN01

Ajouter enregistrement A : ipbx01 → 172.16.10.5

---

## Softphone MicroSIP

Télécharger : https://www.microsip.org/downloads

Configuration :

| Champ | Valeur |
|-------|--------|
| SIP Server | 172.16.10.5 |
| Username | 20001 |
| Domain | 172.16.10.5 |
| Login | 20001 |
| Password | 1234 |
| Transport | UDP |

![conf_micro_SIP](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/12.1_installation_MicroSIP.png)

---

Auteur : Safiullah  
Projet : Ekoloclast
