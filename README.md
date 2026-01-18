# TSSR-1025-P3-G7 Infrastructure Réseau Ekoloclast

Infrastructure Réseau Ekoloclast
À propos
Projet de mise en place d'une infrastructure réseau professionnelle complète pour Ekoloclast, une start-up de 183 employés spécialisée dans l'écologie.
Objectif : Remplacer le réseau actuel basique (box FAI + wifi) par une infrastructure d'entreprise sécurisée et centralisée.
Contexte
L'entreprise

183 employés répartis dans 10 départements
Start-up parisienne en pleine croissance
Fusion/acquisition prévue prochainement

Situation actuelle

Réseau wifi non sécurisé
PC en workgroup sans authentification
Aucune gestion centralisée
Stockage local uniquement

Situation cible

Infrastructure segmentée (WAN/LAN/DMZ)
Domaine Active Directory
Services centralisés (DNS, DHCP, messagerie, VoIP)
Sécurité renforcée avec pare-feu

Architecture prévue
Serveurs

FW01 : Pare-feu pfSense (WAN/LAN/DMZ)
SRVWIN01 : Windows Server 2022 (AD-DS, DNS, DHCP)
SRVWIN04 : Windows Server 2022 (WSUS)
SRVLX01 : Debian (GLPI, Messagerie)
IPBX01 : FreePBX (VoIP)
CLIWIN01/02 : Clients Windows 10/11

Services déployés

Active Directory avec 10 départements
DNS et DHCP
Pare-feu avec règle "Deny All"
GLPI (gestion de parc + ticketing)
WSUS (mises à jour Windows)
Messagerie (Zimbra ou iRedMail)
VoIP (FreePBX)

Avancement
Semaine 1 : Phase de planification (en cours)

 Tableau d'inventaire matériel
 Schéma réseau complet
 Plan d'adressage IP
 Nomenclature des noms

Semaines suivantes : Installation et configuration
Technologies

Windows Server 2022
Debian Linux
pfSense
Active Directory
GLPI
FreePBX
Zimbra/iRedMail
