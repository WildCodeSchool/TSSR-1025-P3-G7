# SRVLX02 - Guide d'utilisation

## Serveur GLPI (Ubuntu + Docker)

---

## Connexion

### SSH


ssh safi@172.16.10.3
Password : Azerty1*


### Interface web


URL : http://172.16.10.3


| Utilisateur | Mot de passe | Profil |
|-------------|--------------|--------|
| glpi | glpi | Super-Admin |
| tech | tech | Technicien |

---

## Gestion des conteneurs Docker

### Voir les conteneurs

docker ps

### Redémarrer GLPI

cd /opt/glpi
docker compose restart

### Arrêter GLPI

cd /opt/glpi
docker compose down

### Démarrer GLPI

cd /opt/glpi
docker compose up -d

### Voir les logs

docker logs glpi-app
docker logs glpi-mariadb

---

## Gestion des tickets

### Créer un ticket

1. Assistance → Créer un ticket
2. Remplir : titre, description, catégorie, urgence
3. Ajouter

### Attribuer un ticket

1. Ouvrir le ticket
2. Acteurs → Attribué à → Sélectionner technicien
3. Sauvegarder

---

## Gestion du parc

### Ajouter un ordinateur

1. Parc → Ordinateurs → Ajouter
2. Remplir : nom, fabricant, modèle, numéro de série
3. Ajouter

---

## Import utilisateurs LDAP

1. Configuration → Authentification → Annuaires LDAP
2. Ajouter
3. Paramètres :
   - Serveur : 172.16.10.2
   - Port : 389
   - Base DN : dc=tssr,dc=lan
   - Utilisateur : CN=Administrator,CN=Users,DC=tssr,DC=lan
4. Tester → Sauvegarder

---

## Dépannage

| Problème | Solution |
|----------|----------|
| Page inaccessible | docker ps (vérifier conteneurs) |
| Conteneur arrêté | docker compose up -d |
| Erreur base | docker logs glpi-mariadb |

---

## Sauvegarde

cd /opt/glpi

# Sauvegarder les volumes Docker
docker run --rm -v glpi_mariadb_data:/data -v $(pwd):/backup alpine tar cvzf /backup/backup_mariadb.tar.gz /data
docker run --rm -v glpi_glpi_data:/data -v $(pwd):/backup alpine tar cvzf /backup/backup_glpi.tar.gz /data

---

Auteur : Safiullah  
Projet : Ekoloclast