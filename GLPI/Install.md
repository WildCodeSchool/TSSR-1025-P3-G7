# SRVLX02 - Installation

## Serveur GLPI (Ubuntu + Docker)

---

## Informations

| Paramètre | Valeur |
|-----------|--------|
| Nom | SRVLX02 |
| OS | Ubuntu Server 22.04 LTS |
| IP | 172.16.10.3/28 |
| Passerelle | 172.16.10.1 |
| DNS | 172.16.10.2 |
| Zone | LAN_SRV |

---

## Préparation VirtualBox

- Type : Linux  
- Version : Ubuntu (64-bit)  
- RAM : 2048 MB  
- Disque : 20 GB  
- Réseau : Internal Network → LAN_SRV  

---

## Installation Ubuntu Server

1. Démarrer sur l'ISO Ubuntu Server 22.04  
2. Langue : English  
3. Keyboard : French  
4. Type : Ubuntu Server (minimized)  
5. Nom machine : srvlx02  
6. Username : safi / Azerty1*  
7. Cocher : Install OpenSSH server  
8. Ne rien cocher dans Featured snaps  
9. Reboot  

---

## Configuration réseau (Netplan)


sudo nano /etc/netplan/00-installer-config.yaml



network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      addresses:
        - 172.16.10.3/28
      routes:
        - to: default
          via: 172.16.10.1
      nameservers:
        addresses:
          - 172.16.10.2
        search:
          - tssr.lan



sudo netplan apply


---

## Installation Docker


sudo apt update && sudo apt upgrade -y

sudo apt install -y ca-certificates curl gnupg lsb-release

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo systemctl enable docker
sudo usermod -aG docker $USER


Se déconnecter/reconnecter pour appliquer le groupe docker.

---

## Installation GLPI avec Docker


sudo mkdir -p /opt/glpi
sudo chown -R $USER:$USER /opt/glpi
cd /opt/glpi
nano docker-compose.yml


Contenu :  

version: '3.8'

services:  
  mariadb:  
    image: mariadb:10.11
    container_name: glpi-mariadb
    restart: always
    environment:  
      MARIADB_ROOT_PASSWORD: Azerty1*
      MARIADB_DATABASE: glpi
      MARIADB_USER: glpi
      MARIADB_PASSWORD: Azerty1*
    volumes:  
      - mariadb_data:/var/lib/mysql
    networks:  
      - glpi-network

  glpi:  
    image: diouxx/glpi:latest
    container_name: glpi-app
    restart: always
    ports:  
      - "80:80"
    environment:  
      TIMEZONE: Europe/Paris
    volumes:  
      - glpi_data:/var/www/html/glpi
    depends_on:  
      - mariadb
    networks:  
      - glpi-network

volumes:  
  mariadb_data:  
  glpi_data:  

networks:    
  glpi-network:  
    driver: bridge


Lancer :    
docker compose up -d


---

## Configuration web GLPI

1. Accéder à http://172.16.10.3  
2. Langue : Français  
![GLPI/Ressources/gpli_setup.png](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/784892a4b5bfd69c46500ffa121db3b4b8ded8a1/GLPI/Ressources/gpli_setup.png)
4. Installer 
![GLPI/Ressources/gpli_setup2.png](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/784892a4b5bfd69c46500ffa121db3b4b8ded8a1/GLPI/Ressources/gpli_setup2.png)
5. Base de données :    
   - Serveur : mariadb    
   - Utilisateur : glpi    
   - Mot de passe : Azerty1*    
   - Base : glpi    
6. Terminer l'installation    
![GLPI/Ressources/gpli_setup3.png](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/784892a4b5bfd69c46500ffa121db3b4b8ded8a1/GLPI/Ressources/gpli_setup3.png)

---

## Configuration DNS sur SRVWIN01

Ajouter enregistrement A : srvlx02 → 172.16.10.3  

---

## Comptes par défaut

| Utilisateur | Mot de passe | Profil |
|-------------|--------------|--------|
| glpi | glpi | Super-Admin |
| tech | tech | Technicien |
| normal | normal | Utilisateur |
| post-only | postonly | Post-Only |

Changer les mots de passe après installation.

## linterface WEB
![GLPI/Ressources/capture_de_surface.png](https://github.com/WildCodeSchool/TSSR-1025-P3-G7/blob/784892a4b5bfd69c46500ffa121db3b4b8ded8a1/GLPI/Ressources/capture_de_surface.png)

---

## Vérification


docker ps
docker logs glpi-app


---

Auteur : Safiullah
Projet : Ekoloclast
