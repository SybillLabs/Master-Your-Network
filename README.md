# 🧠 Master your network — Projet complet d’infrastructure réseau virtualisée et sécurisée

🔹💼 **Informations de présentation du projet**  
![Statut du projet](https://img.shields.io/badge/statut-en%20cours-yellow)
![Licence](https://img.shields.io/badge/Licence-MIT-blue)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sybill%20Gribonval-blue?logo=linkedin)](https://www.linkedin.com/in/sybill-gribonval)
![Langue](https://img.shields.io/badge/langue-français-blue)
![Contributions](https://img.shields.io/badge/contributions-bienvenues-brightgreen)
[![Trello Board](https://img.shields.io/badge/Trello-Master%20Your%20Network-blue?logo=trello)](https://trello.com/b/GfEDKtpd/🧠-master-your-network-projet-complet-dinfrastructure-reseau-virtualisee-et-securisee)


🔹🧭 **Badges de conception et de planification**  
![Documentation](https://img.shields.io/badge/Documentation-Technique-blue)
![Analyse](https://img.shields.io/badge/Analyse-Infrastructure-green)
![Architecture](https://img.shields.io/badge/Architecture-Infrastructure-orange)
![Topology](https://img.shields.io/badge/Topology-Schéma-blue)

🔹🛠️ **Outils et composants principaux**  
![VMware Workstation](https://img.shields.io/badge/VMware-Workstation-blue?logo=vmware)
![VyOS](https://img.shields.io/badge/VyOS-Router-orange)
![Debian](https://img.shields.io/badge/Debian-Linux-A81D33?logo=debian)
![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D6?logo=windows)
![Active Directory](https://img.shields.io/badge/AD--DS-Annuaire-0078D6?logo=windows)
![Zabbix](https://img.shields.io/badge/Zabbix-Monitoring-red?logo=zabbix)
![GLPI](https://img.shields.io/badge/GLPI-ITSM-yellow)
![Bareos](https://img.shields.io/badge/Bareos-Backup-blue)
![Nextcloud](https://img.shields.io/badge/Nextcloud-Cloud-blue?logo=nextcloud)
![Apache](https://img.shields.io/badge/Apache-Web-red?logo=apache)
![3CX](https://img.shields.io/badge/3CX-VoIP-blue)
![PingCastle](https://img.shields.io/badge/PingCastle-Audit-blue)
![Lynis](https://img.shields.io/badge/Lynis-Security-green)
![Vaultwarden](https://img.shields.io/badge/Vaultwarden-Password%20Manager-yellow)
![iRedMail](https://img.shields.io/badge/iRedMail-Mail-red)

## 📝 Contexte
**Master Your Network** est un projet **professionnel et personnel** visant à concevoir, configurer et administrer une **infrastructure réseau d’entreprise complète**, virtualisée et sécurisée.  
Il s’agit d’une évolution individuelle du projet collaboratif « *Build Your Infrastructure* », réalisé dans le cadre de ma formation en **Technicien Supérieur Systèmes & Réseaux (Bac +2)**.

Ce projet a pour but de mettre en pratique l’ensemble des compétences attendues d’un **TSSR**, tout en intégrant certaines notions avancées issues du niveau **Administrateur d’Infrastructure Sécurisée (Bac +3)**, sans pour autant dépasser ce périmètre.

L’environnement reproduit une **entreprise de 50 utilisateurs**, avec une segmentation réseau complète (LAN, VLAN, DMZ) et des services essentiels tels que l’Active Directory, le DNS/DHCP, la supervision, la sauvegarde, la messagerie, la VoIP ou encore le partage de fichier interne.  
L’ensemble est pensé pour être **réaliste, pédagogique et totalement maîtrisable**, afin de permettre une compréhension concrète des mécanismes d’un système d’information d’entreprise.

L’infrastructure est entièrement virtualisée sous **VMware Workstation Pro**, sur un système hôte **Linux (Kubuntu)** disposant des ressources suivantes :
- **Processeur** : 8 cœurs / 16 threads
- **Mémoire vive** : 64 Go RAM
- **Virtualisation** : VMware Workstation Pro (réseaux LAN Segments, VLANs, DMZ, NAT)

Ce projet constitue à la fois une **mise en situation professionnelle** et un **laboratoire d’apprentissage personnel**, combinant rigueur technique, sécurité, documentation et méthodologie projet.

## 🔧 Objectifs du projet
L’environnement est basé sur une infrastructure hybride Linux/Windows :
- **Linux (Debian/Ubuntu)** pour les services réseau, applicatifs et de supervision
- **Windows Server** pour l’annuaire Active Directory, les politiques de groupe et la gestion des postes clients
- **Windows 11 Pro** pour les ordinateurs clients & administrateurs

👉 Le but du projet est de développer une maîtrise pratique et terrain de la mise en place, l’exploitation et la sécurisation d’un réseau d’entreprise complet.

### 🧩 Réseau & Infrastructure
- 🔐 **Routeur sécurisé** : configuration d'un routeur sécurisé avec pare-feu intégré pour filtrer et sécuriser le trafic
- 🌐 **Réseau VLANs** : segmentation logique du réseau avec des VLANs pour isoler les différentes zones
- 🔑 **Serveur VPN** : configuration d'un accès distant sécurisé pour les administrateurs et les collaborateurs
- 📡 **Serveur NTP** : mise en place d'un serveur NTP pour la synchronisation horaire de toute l'infrastructure

### 🧠 Services & Systèmes
- 📦 **Serveur Linux** : déploiement de serveurs DHCP & DNS pour gérer les adresses IP et la résolution des noms
- 🪟 **Serveur Windows** : mise en place d'un Active Directory avec GPO et un système de partage de fichiers SMB pour les utilisateurs
- 🛰️ **Serveur Updates** : mise en place d'un système de mise à jour avec WSUS
- 📧 **Serveur de messagerie** : installation d'un serveur de messagerie interne pour la communication entre utilisateurs
- 🌍 **Serveur web** : création de serveur web pour l'intranet et l'extranet de l'entreprise
- ☁️ **Plateforme de service Cloud** : déploiement d’un Cloud externe en DMZ pour les échanges sécurisés

### 🔎 Supervision, Sécurité & Maintenance
- 📊 **Supervision** : déploiement d'un outil de supervision pour surveiller l'état des serveurs et du réseau
- 📜 **Serveur central de journalisation** : centralisation et analyse des logs système pour faciliter le diagnostic et les audits
- 💾 **Stockage & sauvegarde** : mise en place d'un espace de stockage sécurisé avec RAID, sauvegardes automatiques et restauration pour les serveurs
- 🧰 **Gestion d’incident** : déploiement d'une solution de gestion des tickets et d'assistance à distance selon les bonnes pratiques ITIL
- 📞 **Serveur de VoIP** : déploiement d'un service de téléphonie IP pour les communications internes
- 🔒 **Serveur de mot de passe** : intégration d'un coffre-fort numérique pour stocker et partager les identifiants de façon sécurisée
- 🛡️ **Audit de l’infrastructure** : validation de la sécurité, de la disponibilité et des performances de l'ensemble du réseau

## 🎯 Les étapes de conception
### 🗂️ [Étape 1 : Préparation et planification](/Installations/Etape1/0-index.md)
- 🏢 **Présentation de l'entreprise** : Définir le contexte, le secteur, les besoins métier et les enjeux IT.
- 🧾 **Inventaire et nomenclature de l'infrastructure** : Lister les serveurs, postes, équipements réseau et services attendus ; traduire en machines virtuelles selon les ressources disponibles avec un nommage clair et logique.
- 💽 **La virtualisation avec VMware Workstation** : Caractrériques des VMs, création des VMs par rapport aux étapes de conception et création d'une VM avec VMware Workstation
- 🗺️ **Arborescence Windows de l'infrastructure** : Organisation de l’Active Directory (OU, groupes, GPO).
- 🌐 **Plan réseau de l'infrastructure** : Définir les VLANs, LAN/DMZ et le plan d’interconnexion global.

### 🌐 [Étape 2 : Configuration du réseau et de la sécurité](/Installations/Etape2/0-index.md)
- 🚦 **Configuration du routeur VyOS** : Interfaces, VLANs, routage, NAT, pare-feu de base.
- 🔐 **Mise en place des VLANs et interconnexions** : Attribution des sous-réseaux et tests de communication inter-VLANs.
- 🧱 **Mise en place de la DMZ et filtrage** : Isolation du réseau externe et définition des règles d’accès.
- 🧭 **Mise en place du serveur NTP (Chrony)** : Synchronisation temporelle des serveurs.
- 🧪 **Tests de connectivité et de sécurité réseau de base** : Ping, traceroute, pare-feu, contrôle des flux LAN/DMZ.

### 🖥️ [Étape 3 : Services d’infrastructure de base](/Installations/Etape3/0-index.md)
- 🧾 **Installation du serveur DHCP/DNS (Linux)** : Attribution dynamique et résolution interne.
- 🪟 **Installation du serveur Windows AD/DC** : Configuration du domaine, GPO, utilisateurs et groupes.
- 💾 **Partage SMB et gestion des permissions** : Mise en place des dossiers partagés pour les services internes.
- 🧩 **Mise en place du serveur WSUS** : Gestion des mises à jour internes.
- 🧪 **Tests de jointure au domaine et de résolution DNS** : Vérification des services fondamentaux.

### 🔒 [Étape 4 : Services de sécurité et supervision](/Installations/Etape4/0-index.md)
- 🧠 **Installation du serveur Vaultwarden** : Gestion centralisée et sécurisée des mots de passe.
- 📜 **Serveur Logs (Syslog + LogAnalyzer)** : Centralisation et analyse des journaux systèmes.
- 🧍‍♂️ **Mise en place du serveur Audit (PingCastle + Lynis)** : Installation, première analyse intermédiaire et rapport de mi-parcours.
- 📈 **Installation du serveur Monitoring (Zabbix)** : Suivi des performances et alertes.
- 🔎 **Tests de supervision complète** : Vérification des alertes et du bon reporting.

### 🧰 [Étape 5 : Services applicatifs et utilisateurs](/Installations/Etape5/0-index.md)
- 🌐 **Serveur WebExterne (Nextcloud + Nginx)** : Mise à disposition d’un cloud et d’un extranet.
- 📞 **Serveur VoIP (3CX)** : Installation et configuration du système téléphonique interne.
- 💌 **Serveur Mail (iRedMail)** : Gestion du courrier électronique interne/externe.
- 🧾 **Serveur IT (GLPI + Intranet)** : Gestion de parc et portail interne.
- 👥 **Création des postes clients et du poste Tech DSI** : Configuration des environnements utilisateurs.

### 💽 [Étape 6 : Sauvegarde, restauration et validation](/Installations/Etape6/0-index.md)
- 🗄️ **Installation du serveur Backup (Bareos + NAS/RAID)** : Configuration des stratégies de sauvegarde.
- 🧰 **Sauvegarde et restauration de test** : Simulation d’un sinistre et validation de la procédure.
- 📋 **Documentation des procédures de restauration** : Création de fiches pratiques.
- 🔄 **Automatisation des sauvegardes planifiées** : Mise en place de scripts ou de tâches planifiées.
- ✅ **Audit final de disponibilité et résilience** : Vérification globale de la tolérance aux pannes.

### 📘 [Étape 7 : Documentation et finalisation](/Installations/Etape7/0-index.md)
- 🧾 **Rédaction du Guide Administrateur** : Procédures internes, configurations et dépannage.
- 📗 **Rédaction du Guide Utilisateur** : Utilisation des services (mail, Nextcloud, Intranet, VoIP…).
- 🏁 **Présentation finale du projet** : Rapport complet et captures d’écran des tests fonctionnels.


## 📌 Avancement du projet
- [x] Étape 1 : Préparation et planification
- [ ] Étape 2 : Configuration du réseau et de la sécurité
- [ ] Étape 3 : Services d’infrastructure de base
- [ ] Étape 4 : Services de sécurité et supervision
- [ ] Étape 5 : Services applicatifs et utilisateurs
- [ ] Étape 6 : Sauvegarde, restauration et validation
- [ ] Étape 7 : Documentation et finalisation

## 🛠️ Outils et technologies

| Catégorie                     | Outils / Logiciels / Rôles techniques |
| ----------------------------- | ------------------------------------- |
| **Hyperviseur**               | VMware Workstation                    |
| **Pare-feu / Routeur**        | VyOS                                  |
| **Serveur Linux**             | Debian/Ubuntu                         |
| **Serveur Windows**           | Windows Server, AD-DS, WSUS, GPO, SMB |
| **Sauvegarde & stockage**     | Bareos, NAS, RAID                     |
| **Journalisation**            | LogAnalyzer, Syslog                   |
| **Gestion des mots de passe** | Vaultwarden                           |
| **Supervision**               | Zabbix                                |
| **Gestion IT**                | GLPI, Apache                          |
| **Serveur de temps**          | Chrony                                |
| **Services externes**         | Nextcloud, Nginx                      |
| **Serveur de messagerie**     | iRedMail                              |
| **VoIP**                      | 3CX                                   |
| **Audit de sécurité**         | PingCastle, Lynis, Nmap, Kaisen Linux |
| **Poste administrateur**      | Windows 11 Pro                        |
| **Poste utilisateur**         | Windows 11 Pro                        |

## 🗂️  Organisation du dépôt
Ce dépôt sera organisé comme suit :
- Un dossier **Installations** où sera répertorié les fiches d'installations de chacune des étapes de conception de l'infrastructure
- Un dossier **Guides** où sera répertorié les fiches **Guide de l'utilisateur** & **Guide de l'administrateur**

Pour une meilleure lisibilité, voici un visuel de la structure du dépôt :
```text
/
├── README.md
├── Installations
│   └── Etape1
│       └── Ressources
│           └── Files
│       └── index.md
│       └── task1.md
│       └── task2.md
│       └── task3.md
│       └── task4.md
│       └── task5.md
│   └── Etape2
│       └── ...
│   └── Etape7
│       └── Ressources
│           └── Files
│       └── index.md
│       └── task1.md
│       └── task2.md
│       └── task3.md
│       └── task4.md
│       └── task5.md
├── Guides
│   └── USER_GUIDE.md
│   └── ADMIN_GUIDE.md
```

## 👤 Auteur
Projet réalisé par **Sybill Gribonval**  
🔗 [LinkedIn](https://www.linkedin.com/in/sybill-gribonval) | 📧 sybillgribonval@gmail.com

## ⚠️ Disclaimer
> Ce projet est un **laboratoire personnel** : il a pour but de me former aux infrastructures réseau hybrides (Linux/Windows), de documenter mes pratiques, et de constituer un support concret dans mon portfolio.  
> Il ne représente pas une infrastructure de production, mais une maquette réaliste et reproductible en environnement virtualisé.
