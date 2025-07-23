# 🧾 Inventaire des machines nécessaires (réseau et système)

Ce fichier va répondre au question *qu'est-ce qu'il me faut ? Pourquoi ?* concernant les machines nécessaires à mon infrastructure réseau et système.

| Machines                             | Rôles & fonctionnalités prévues                                                                                                                                            | Quantité prévues     | Caractéristiques techniques nécessaires (RAM, CPU, Stockage)                                                                         | Type de machine |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------- |
| Routeur pfSense                      | Assure la liaison sécurisée entre le réseau externe (WAN) et les réseaux internes (LAN, DMZ) via routage, pare-feu et NAT.                                                 | 1                    | CPU : 2 threads<br>RAM : 2 Go<br>Disque : 20 Go <br>Interface réseau : 3 (WAN, LAN, DMZ)<br>OS supporté : pfSense 2.7.x (FreeBSD)    | VM              |
| Switch VLANs                         | Segmentation du réseau en VLANs logiques pour isoler les différentes zones du LAN (Client, Serveur, ...)                                                                   | Intégré dans pfSense | Utilisation des fonctionnalités VLAN de pfSense (pas de VM dédiée)                                                                   | VM              |
| Serveur Linux                        | Assurer le fonctionnement des services DHCP et DNS                                                                                                                         | 1                    | CPU : 1 thread<br>RAM : 1Go<br>Disque : 10 Go<br>Interface réseau : 1 (Carte LAN)<br>OS supporté : Ubuntu Server (non graphique)<br> | VM              |
| Serveur Windows                      | Assurer le fonctionnement des services Actives Directory, WSUS, GPO et Serveur de fichier partagé                                                                          | 1                    | CPU : 3 threads<br>RAM : 6 Go<br>Disque : 250 Go<br>Interface réseau : 1 (Carte LAN)<br>OS supporté : Windows Server 2019 ou 2022    | VM              |
| Machine Administrateur               | Tester le fonctionnement de l'infrastructure depuis une machine administrateur                                                                                             | 1                    | CPU : 1 threads<br>RAM : 2 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu 22.04                    | VM              |
| Machine Windows                      | Tester le fonctionnement de l'infrastructure depuis une machine cliente                                                                                                    | 1                    | CPU : 2 threads<br>RAM : 2 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Windows 11                      | VM              |
| Machine de gestion IT                | Assurer le fonctionnement des services de GLPI (HelpDesk), du serveur web (DashBoard), du système de supervision Zabbix, de la journalisation du système et du serveur NTP | 1                    | CPU : 1 threads<br>RAM : 3 Go<br>Disque : 50 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu Serveur (non graphique)  | VM              |
| Machine de mail                      | Assurer le fonctionnement du service iRedMail                                                                                                                              | 1                    | CPU : 1 threads<br>RAM : 4 Go<br>Disque : 60 Go<br>Interface Réseau : 1 (Carte DMZ)<br>OS Supporté : Ubuntu Serveur (non graphique)  | VM              |
| Machine de sauvegarde et de stockage | Dédiée à la gestion des sauvegardes, au stockage via un NAS, assurant la protection des données avec Bareos et la redondance grâce au RAID                                 | 1                    | CPU : 1 threads<br>RAM : 4 Go<br>Disque : 100 Go<br>Interface Réseau : 1 (Carte LAN)<br>OS Supporté : Ubuntu Serveur (non graphique) | VM              |

**Résumé total des ressources allouées aux VMs :**
- Threads utilisés : 12
- RAM utilisée : 24 Go

Pour rappel, mon ordinateur hôte possède :
- CPU : AMD Ryzen 7 5800X avec 8 cœurs physiques et 16 threads (grâce à l’hyper-threading)
- RAM : 32 Go DDR4 3200 MHz

J’utilise ici le nombre de _threads_ plutôt que celui des _cœurs physiques_ car chaque cœur physique du processeur peut gérer deux threads simultanément via l’hyper-threading. Cela signifie que mon CPU, bien qu’ayant 8 cœurs physiques, peut exécuter jusqu’à 16 threads en parallèle.

C’est pourquoi il est possible d’allouer jusqu’à 12 threads aux machines virtuelles sans dépasser la capacité physique réelle du processeur, tout en gardant environ 4 threads disponibles pour l’OS hôte (Kubuntu) et mes applications locales (comme Obsidian ou Opera).

En termes de mémoire vive, j’alloue 24 Go aux VMs, ce qui me laisse 8 Go pour l’hôte et ses programmes, garantissant ainsi une bonne fluidité et réactivité globale.

---
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
Il existe plusieurs manières d’implémenter les services réseau de base comme le DHCP et le DNS dans une infrastructure. Les systèmes Linux et Windows sont tous deux capables de fournir ces services, mais le choix dépend du **contexte**, des **besoins du projet**, et de la **philosophie d’administration**.

Dans le cadre de ce projet, j’ai choisi d’utiliser un **serveur Linux** (Ubuntu Server) pour héberger les services **DHCP** et **DNS** pour plusieurs raisons :

- 🧩 **Séparation des rôles** : Dans une infrastructure Windows complète, ces services sont souvent intégrés dans le rôle **Active Directory (AD)** via le **contrôleur de domaine**. Or ici, je souhaite découpler les rôles pour mieux comprendre chaque composant individuellement.
- 🐧 **Légèreté et flexibilité** : Un serveur Linux en mode console consomme peu de ressources. Il est bien adapté pour des services réseau simples et stables comme **dnsmasq**, **isc-dhcp-server**, ou **bind9**, tout en restant très personnalisable.
- 🔐 **Formation orientée open-source** : Ce choix s’inscrit dans une volonté d’apprentissage et de maîtrise des outils Linux, très utilisés dans les environnements DevOps, système, réseau et cybersécurité.
- 🎓 **Approche pédagogique** : L’objectif étant aussi d’apprendre, l’implémentation des rôles de base sur Linux permet de manipuler les fichiers de configuration, comprendre la logique de résolution DNS ou d’attribution IP, et appréhender les logs système, ce qui est plus transparent que sur un serveur Windows.

Cela ne veut pas dire que l’approche Windows est mauvaise : un **contrôleur de domaine Windows Server** est tout à fait capable d’assurer les services **DHCP** et **DNS**, souvent de manière plus intégrée (avec la gestion des GPO, des zones DNS dynamiques, etc.). D’ailleurs, ce projet comporte un **serveur Windows distinct** pour jouer ce rôle dans un second temps.

## Qu'est-ce que la gestion IT ?
La *gestion IT* est un ensemble de services et d’outils me permettant d'organiser, de superviser et d'optimiser les ressources informatiques de l'infrastructure, afin d’assurer sa disponibilité, sa sécurité et son bon fonctionnement.  
Les outils et services que je vais utiliser dans le cadre de ce projet sont :

- **GLPI** : Pour assurer le helpdesk, le système de ticketing, ainsi que la gestion des utilisateurs, des matériels et des licences.
- **Dashboard** : Via un site web localisé sur un serveur, il permettra d’accéder facilement aux différents services et outils de mon infrastructure, offrant une vue centralisée.
- **Zabbix** : Un outil de supervision qui me permettra de surveiller l’état de santé de mon infrastructure, de détecter les anomalies et d’alerter en cas de problème (CPU, RAM, réseau, services, etc.).
- **Journalisation (logging centralisé)** : Un service qui collectera et centralisera les logs de toutes les machines, facilitant l’analyse, la recherche d’incidents et le renforcement de la sécurité.
- **Serveur NTP** : Permet de synchroniser l'heure de tout le réseau, ce qui est important pour la sécurité (logs, horodatés, authentification)


## Qu'est-ce qu'un système de sauvegarde et de stockage ?
Un système de **sauvegarde et de stockage** regroupe les solutions matérielles et logicielles permettant de **conserver, sécuriser et restaurer** les données critiques d’un système d'information.

Il repose généralement sur trois piliers :
1. **Le stockage** :  
    Permet d’héberger les données de manière organisée, accessible et performante. Cela peut être assuré par un **NAS (Network Attached Storage)** qui offre un espace centralisé pour les fichiers.
2. **La redondance** :  
    Mise en œuvre via des technologies comme le **RAID (Redundant Array of Independent Disks)**, elle protège les données contre la perte due à une défaillance matérielle en répartissant ou dupliquant les données sur plusieurs disques.
3. **La sauvegarde** :  
    Réalisée avec des outils comme **Bareos**, elle consiste à **copier régulièrement les données** afin de pouvoir les restaurer en cas de suppression accidentelle, de corruption ou d’incident grave (attaque, panne…).

**Dans un environnement pédagogique comme le mien**, même si les volumes de données sont faibles, la mise en place de ce système montre ma capacité à **gérer un environnement de production réaliste**, en intégrant **les bonnes pratiques de sécurité et de continuité de service.**

---

*[Retour au fichier README.md](./../README.md)*
