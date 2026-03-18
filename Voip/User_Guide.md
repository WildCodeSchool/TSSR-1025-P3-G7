# IPBX01 - Guide d'utilisation

## Serveur VoIP FreePBX

---

## Connexion

### SSH  


ssh safi@172.16.10.5  
Password : *********  

# Pour root  
su -  


### Interface web

URL : http://172.16.10.5   
User : admin  
Password : Azerty1*   
![ipbx_web](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/6d9dc4f1cf7210423ef829a5d57827ab3f0f3075/Voip/Ressources/4_instal_freePBX.png)

---

## Extensions disponibles

| Extension | Nom | Secret |
|-----------|-----|--------|
| 20001 | Safi WILDER | 1234 |
| 20002 | Admin IT | 1234 |
| 20003 | Support | 1234 |
| 20004 | Direction | 1234 |

![Extension_add](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/11_cree_extension.png)

---

## Créer une extension

1. Applications → Extensions  
![Extension_add](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/8_conf_initiale.png)

2. SIP [chan_pjsip] Extensions  
3. + Add New SIP [chan_pjsip] Extension  
![Extension_add](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/25ffb521f1e4a80030c66a02c222cf07955ebfd8/Voip/Ressources/10_cree_extension.png)

4. Remplir : User Extension, Display Name, Secret
5. Submit → Apply Config


---

## Configuration MicroSIP

1. Télécharger : https://www.microsip.org/downloads
2. Clic droit → Account → Add
3. Configurer :

| Champ | Valeur |
|-------|--------|
| Account Name | Nom utilisateur |
| SIP Server | 172.16.10.5 |
| Username | Numéro extension |
| Domain | 172.16.10.5 |
| Login | Numéro extension |
| Password | 1234 |
| Transport | UDP |

4. Sauvegarder

![conf_micro_SIP](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/61c62098a40514be5a1697d6aa273e27a8418a1c/Voip/Ressources/12.1_installation_MicroSIP.png)
---

## Passer un appel

1. Ouvrir MicroSIP
2. Vérifier statut "Online"
3. Taper le numéro (ex: 20003)
4. Appuyer Entrée  
![conf_micro_SIP](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/25ffb521f1e4a80030c66a02c222cf07955ebfd8/Voip/Ressources/14_installation_MicroSIP.png)
---

## Vérifier les extensions enregistrées

### Via interface web

1. Reports → Asterisk Info
2. PJSIP Endpoints
3. Status "Avail" = enregistré

### Via CLI

asterisk -rvvv
CLI> pjsip show endpoints
CLI> quit

---

## Commandes utiles

# Statut Asterisk
systemctl status asterisk

# Console Asterisk
asterisk -rvvv

# Voir les appels en cours
asterisk -rx "core show channels"

# Redémarrer Asterisk
systemctl restart asterisk

---

## Dépannage

| Problème | Solution |
|----------|----------|
| Extension non enregistrée | Vérifier Username/Password |
| Pas d'audio | Vérifier ports RTP (10000-20000) |
| MicroSIP "Registration failed" | Vérifier SIP Server = 172.16.10.5 |

---

## Sauvegarde

# Configuration
tar -czvf /root/backup_asterisk.tar.gz /etc/asterisk

# Via interface
Admin → Backup & Restore → Run Backup

---

Auteur : Safiullah  
Projet : Ekoloclast
