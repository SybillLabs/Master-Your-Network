# 🧾 Inventaire des machines nécessaires

Ce fichier répond à une question simple mais essentielle :  
**"Qu’est-ce qu’il me faut comme machines ? Et pourquoi ?"**  
Tu trouveras ici un aperçu clair de chaque VM prévue dans mon infrastructure virtuelle, avec leurs rôles, leurs specs, et le raisonnement derrière leur mise en place.

| Machine                           | Rôle dans l’infrastructure                                                                                                          | Quantité | Caractéristiques principales                                                                        | Type |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------------- | ---- |
| **Routeur pfSense**               | Cœur de la sécurité réseau : il fait le pont entre WAN, LAN, et DMZ tout en gérant le routage, le firewall et le NAT.               | 1        | 2 threads • 2 Go RAM • 20 Go disque • 3 interfaces réseau (WAN, LAN, DMZ) • pfSense 2.7.x (FreeBSD) | VM   |
| **Switch VLANs (intégré)**        | Permet de créer plusieurs sous-réseaux logiques dans le LAN (clients, serveurs, IT…) sans matériel dédié, via les VLANs de pfSense. | –        | Géré directement dans pfSense, donc pas de VM séparée.                                              | VM   |
| **Serveur Linux (DHCP/DNS)**      | Fournit les services de base du réseau (adressage IP et résolution de noms) avec des outils open-source légers et efficaces.        | 1        | 1 thread • 1 Go RAM • 10 Go disque • 1 interface réseau • Ubuntu Server (console uniquement)        | VM   |
| **Serveur Windows (AD/WSUS/GPO)** | Joue le rôle de contrôleur de domaine Windows : gestion des utilisateurs, GPO, mises à jour WSUS et fichiers partagés.              | 1        | 3 threads • 6 Go RAM • 250 Go disque • 1 interface réseau • Windows Server 2019/2022                | VM   |
| **Machine Admin (Linux)**         | Permet de tester, monitorer, et interagir avec l’infrastructure en tant qu’administrateur.                                          | 1        | 1 thread • 2 Go RAM • 50 Go disque • 1 interface réseau • Ubuntu 22.04 Desktop                      | VM   |
| **Machine Utilisateur (Windows)** | Simule un poste client dans le LAN. Idéal pour tester les GPO, le DNS, l’authentification, l’accès aux fichiers, etc.               | 1        | 2 threads • 2 Go RAM • 50 Go disque • 1 interface réseau • Windows 11                               | VM   |
| **Machine de gestion IT**         | Centralise les outils de gestion IT : GLPI (helpdesk), Zabbix (supervision), logging, serveur NTP et dashboard web interne.         | 1        | 1 thread • 3 Go RAM • 50 Go disque • 1 interface réseau • Ubuntu Server (console uniquement)        | VM   |
| **Machine mail (iRedMail)**       | Gère les emails dans la DMZ, pour simuler un vrai service de messagerie sécurisé (SMTP, IMAP, Webmail, etc.).                       | 1        | 1 thread • 4 Go RAM • 60 Go disque • 1 interface réseau (DMZ) • Ubuntu Server (console uniquement)  | VM   |
| **Machine de sauvegarde & NAS**   | Sert de serveur de stockage et de sauvegarde avec gestion RAID et automatisation via Bareos.                                        | 1        | 1 thread • 4 Go RAM • 100 Go disque • 1 interface réseau • Ubuntu Server (console uniquement)       | VM   |

## 🔧 Bilan des ressources utilisées

**Total alloué aux VMs :**
- **Threads** : 12
- **RAM** : 24 Go

**Specs de ma machine hôte (Kubuntu) :**
- **CPU** : AMD Ryzen 7 5800X – 8 cœurs / 16 threads
- **RAM** : 32 Go DDR4 3200 MHz    

👉 Je me base sur les **threads** car mon processeur gère deux threads par cœur physique. Je garde donc 4 threads pour l’hôte, ce qui me permet de lancer Obsidian, Opera ou d’autres outils locaux sans ralentissement.

👉 Il me reste aussi **8 Go de RAM libres** pour l’OS hôte, ce qui garantit une bonne stabilité pendant que les VMs tournent.

---
# 💬 FAQ
## ▸ Le routeur, c’est quoi exactement ?
Un **routeur** permet de faire communiquer différents réseaux entre eux : typiquement, il relie ton réseau local (LAN) à Internet (WAN).  
Dans mon cas, il sert aussi à isoler une **DMZ**, une zone tampon où je mets les machines à risque (comme le serveur mail). Le tout est sécurisé par un firewall intégré dans pfSense.

### ▸ Pourquoi un switch VLANs dans pfSense ?
Un **VLAN** permet de segmenter logiquement un réseau : une bonne pratique pour séparer les clients, les serveurs, l’admin, etc.  
Plutôt que d’utiliser un switch physique, j’utilise directement les fonctionnalités VLAN intégrées à pfSense : plus simple, plus flexible, parfait pour un laboratoire.

## ▸ Pourquoi DHCP/DNS sur un Linux et pas sur le serveur Windows ?
J’ai volontairement **séparé les rôles** pour mieux apprendre chaque composant individuellement.  
Voici pourquoi j’ai mis les services **DHCP** et **DNS** sur **Ubuntu Server** :

- ✂️ **Découplage d’Active Directory** : En environnement pro, c’est souvent centralisé. Ici, je veux voir chaque brique.
- 🐧 **Léger et open-source** : dnsmasq, bind9 ou isc-dhcp-server tournent nickel en console avec très peu de RAM.
- 🎓 **Pédagogique** : Je veux comprendre comment configurer ces services à la main, étudier les logs, manipuler les fichiers conf, etc.
- 🔐 **DevOps-friendly** : Savoir faire ça sur Linux est essentiel dans les métiers orientés infrastructure.

> Bien sûr, **un contrôleur de domaine Windows** aurait très bien pu gérer le DHCP et le DNS. Mais comme je l’ai déjà fait sur Windows Server, cette fois je veux m’entraîner à tout configurer sur Linux, à la main, en partant de zéro.

## ▸ C’est quoi la “gestion IT” ?
C’est le pilotage de l’infrastructure au quotidien.  
Ma **VM de gestion IT** regroupe plusieurs outils clés :
- **GLPI** pour les tickets et la gestion du parc.
- **Dashboard web** pour centraliser tous les accès à mes outils.
- **Zabbix** pour surveiller la charge CPU, la mémoire, les pings, etc.
- **Un serveur de logs centralisés** pour tout ce qui touche à la sécurité.
- **Un serveur NTP** pour synchroniser l’heure de toutes les machines du réseau.

### ▸ Et la sauvegarde, elle marche comment ?

Je mets en place une solution complète de **sauvegarde et de stockage**, composée de :
1. 📦 **Stockage (NAS)** : Partage de fichiers centralisé pour tout le réseau.
2. 🛡️ **RAID** : Redondance des disques pour ne pas perdre de données en cas de panne.
3. 💾 **Sauvegarde (Bareos)** : Automatisation des copies de fichiers importants pour pouvoir les restaurer si besoin.

Même si c’est un projet de laboratoire, je m’efforce de reproduire des **bonnes pratiques pros** en matière de disponibilité et de sécurité.

---

📁 *[Retour au fichier README.md](/README.md)*
