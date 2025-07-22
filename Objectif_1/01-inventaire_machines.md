# 🧾 Inventaire des machines nécessaires (réseau et système)

Ce fichier va répondre au question *qu'est-ce qu'il me faut ? Pourquoi ?* concernant les machines nécessaires à mon infrastructure réseau et système.

| Machines                             | Rôles & fonctionnalités prévues                                                                                                                                    | Quantité prévues     | Caractéristiques techniques nécessaires (RAM, CPU, Stockage)                                                                         | Type de machine |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| Routeur pfSense                      | Assure la liaison sécurisée entre le réseau externe (WAN) et les réseaux internes (LAN, DMZ) via routage, pare-feu et NAT.                                         | 1                    | CPU : 2 threads<br>RAM : 2 Go<br>Disque : 20 Go <br>Interface réseau : 3 (WAN, LAN, DMZ)<br>OS supporté : pfSense 2.7.x (FreeBSD)    | VM              |
| Switch VLANs                         | Segmentation du réseau en VLANs logiques pour isoler les différentes zones du LAN (Client, Serveur, ...)                                                           | Intégré dans pfSense | Utilisation des fonctionnalités VLAN de pfSense (pas de VM dédiée)                                                                   | VM              |
| Serveur Linux                        | Assurer le fonctionnement des services DHCP et DNS                                                                                                                 | 1                    | CPU : 1 thread<br>RAM : 1Go<br>Disque : 10 Go<br>Interface réseau : 1 (Carte LAN)<br>OS supporté : Ubuntu Server (non graphique)<br> | VM              |
| Serveur Windows                      | Assurer le fonctionnement des services Actives Directory, WSUS et GPO                                                                                              | 1                    | CPU : 3 threads<br>RAM : 6 Go<br>Disque : 250 Go<br>Interface réseau : 1 (Carte LAN)<br>OS supporté : Windows Server 2019 ou 2022    | VM              |
| Machine Administrateur               | Tester le fonctionnement de l'infrastructure depuis une machine administrateur                                                                                     | 1                    | CPU : 1 threads<br>RAM : 2 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu 22.04                    | VM              |
| Machine Windows                      | Tester le fonctionnement de l'infrastructure depuis une machine cliente                                                                                            | 1                    | CPU : 2 threads<br>RAM : 2 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Windows 11                      | VM              |
| Machine de gestion IT                | Assurer le fonctionnement des services de GLPI (HelpDesk), du serveur web (DashBoard), du système de supervision Zabbix et de la journalisation du système         | 1                    | CPU : 1 threads<br>RAM : 3 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu Serveur (non graphique)  | VM              |
| Machine de mail                      | Assurer le fonctionnement du service iRedMail                                                                                                                      | 1                    | CPU : 1 threads<br>RAM : 4 Go<br>Disque : 60 Go<br>Interface Réseau : 1 (Carte DMZ)<br>OS Supporté : Ubuntu Serveur (non graphique)  | VM              |
| Machine de sauvegarde et de stockage | Dédiée à la gestion des sauvegardes et du stockage, assurant la protection des données via Bareos, la gestion des volumes avec LVM, et la redondance grâce au RAID | 1                    | CPU : 1 threads<br>RAM : 4 Go<br>Disque : 100 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu Serveur (non graphique) | VM              |

**Résumé total des ressources allouées aux VMs :**
- Threads utilisés : 12
- RAM utilisée : 24 Go

Pour rappel, mon ordinateur hôte possède :
- CPU : AMD Ryzen 7 5800X avec 8 cœurs physiques et 16 threads (grâce à l’hyper-threading)
- RAM : 32 Go DDR4 3200 MHz

J’utilise ici le nombre de _threads_ plutôt que celui des _cœurs physiques_ car chaque cœur physique du processeur peut gérer deux threads simultanément via l’hyper-threading. Cela signifie que mon CPU, bien qu’ayant 8 cœurs physiques, peut exécuter jusqu’à 16 threads en parallèle.

C’est pourquoi il est possible d’allouer jusqu’à 12 threads aux machines virtuelles sans dépasser la capacité physique réelle du processeur, tout en gardant environ 4 threads disponibles pour l’OS hôte (Kubuntu) et mes applications locales (comme Obsidian ou Opera).

En termes de mémoire vive, j’alloue 24 Go aux VMs, ce qui me laisse 8 Go pour l’hôte et ses programmes, garantissant ainsi une bonne fluidité et réactivité globale.

# F.A.Q
## Le routeur, c'est quoi ?
Un _routeur_ est un outil de couche 3 dans le modèle *TCP/IP (Couche Internet)*  qui permet de diriger le trafic internet entre le _réseau local_ (LAN) d'une entreprise et le _réseau global_ (WAN) qui correspond à **Internet**. Il est aussi possible de rajouter une carte réseau à notre routeur, pour ajouter le _réseau DMZ_ d'une entreprise.
Certains *routeurs* intègrent aussi des fonctions de filtrage du trafic, permettant ainsi de sécuriser le *LAN* et la *DMZ*.

**Qu'est-ce qu'une DMZ ?**  
Une _DMZ_ est un réseau séparé du _LAN_ et du _WAN_. Ce réseau permet d’isoler les machines à risques qui peuvent être piratées ou infectées par des virus, afin de protéger le réseau _LAN_. Par exemple, le serveur mail, qui doit être en contact avec le _WAN_, se trouve généralement dans la DMZ.

## Pourquoi mettre en place un switch VLANs ?
**Qu’est-ce qu’un VLAN ?**  
VLAN signifie _Virtual Local Area Network_. C’est un réseau local virtuel qui permet de segmenter un réseau physique en plusieurs sous-réseaux logiques. Cette segmentation facilite la séparation des différents types de machines, par exemple les postes clients (utilisateurs) et les machines IT (réseau et système).

Pour réaliser cela, on utilise généralement des _switchs administrables_ capables de gérer les VLANs. Dans mon projet, je vais utiliser la fonctionnalité intégrée à mon routeur pfSense, qui permet de créer et gérer plusieurs VLANs directement depuis le routeur, sans avoir besoin d’un switch dédié.
## Pourquoi mettre en place un serveur Linux pour les services DHCP et DNS et pas un serveur Windows ?

## A quoi vont servir les machines clientes ?

## Qu'est-ce que la gestion IT ?

## A quoi sert la supervision et la journalisation ?

## Qu'est-ce qu'un système de sauvegarde et de stockage ?
