# 🧾 Inventaire des machines nécessaires (réseau et système)

Ce fichier va répondre au question *qu'est-ce qu'il me faut ? Pourquoi* concernant les machines nécessaires à mon infrastructure réseau et système.

| Machines                             | Rôles & fonctionnalités prévues                                                                                                                                    | Quantité prévues     | Caractéristiques techniques nécessaires (RAM, CPU, Stockage)                                                                         | Type de machine |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| Routeur pfSense                      | Assure la liaison sécurisée entre le réseau externe (WAN) et les réseaux internes (LAN, DMZ) via routage, pare-feu et NAT.                                         | 1                    | CPU : 2 threads<br>RAM : 2 Go<br>Disque : 20 Go <br>Interface réseau : 3 (WAN, LAN, DMZ)<br>OS supporté : pfSense 2.7.x (FreeBSD)    | VM              |
| Switch VLANs                         | Segmentation du réseau en VLANs logiques pour isoler les différentes zones du LAN (Client, Serveur, ...)                                                           | Intégré dans pfSense | Utilisation des fonctionnalités VLAN de pfSense (pas de VM dédiée)                                                                   | VM              |
| Serveur Linux                        | Assurer le fonctionnement des services DHCP et DNS                                                                                                                 | 1                    | CPU : 1 thread<br>RAM : 1Go<br>Disque : 10 Go<br>Interface réseau : 1 (Carte LAN)<br>OS supporté : Ubuntu Server (non graphique)<br> | VM              |
| Serveur Windows                      | Assurer le fonctionnement des services Actives Directory, WSUS et GPO                                                                                              | 1                    | CPU : 4 threads<br>RAM : 6 Go<br>Disque : 250 Go<br>Interface réseau : 1 (Carte LAN)<br>OS supporté : Windows Server 2019 ou 2022    | VM              |
| Machine cliente                      | Tester le fonctionnement de l'infrastructure depuis une machine externe                                                                                            | 2                    | CPU : 2 threads<br>RAM : 2 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Windows 11 et Ubuntu 22.04      | VM              |
| Machine de gestion IT                | Assurer le fonctionnement des services de GLPI (HelpDesk), du serveur web (DashBoard), du système de supervision Zabbix et de la journalisation du système         | 1                    | CPU : 2 threads<br>RAM : 3 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu Serveur (non graphique)  | VM              |
| Machine de mail                      | Assurer le fonctionnement du service iRedMail                                                                                                                      | 1                    | CPU : 2 threads<br>RAM : 4 Go<br>Disque : 60 Go<br>Interface Réseau : 1 (Carte DMZ)<br>OS Supporté : Ubuntu Serveur (non graphique)  | VM              |
| Machine de sauvegarde et de stockage | Dédiée à la gestion des sauvegardes et du stockage, assurant la protection des données via Bareos, la gestion des volumes avec LVM, et la redondance grâce au RAID | 1                    | CPU : 2 threads<br>RAM : 4 Go<br>Disque : 100 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu Serveur (non graphique) | VM              |

# F.A.Q
## Le routeur, c'est quoi ?

## Pourquoi mettre en place un switch VLANs ?

## Pourquoi mettre en place un serveur Linux pour les services DHCP et DNS et pas un serveur Windows ?

## A quoi vont servir les machines clientes ?

## Qu'est-ce que la gestion IT ?

## A quoi sert la supervision et la journalisation ?

## Qu'est-ce qu'un système de sauvegarde et de stockage ?
