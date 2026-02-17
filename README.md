# 🧠 Master your network — Projet complet d’infrastructure réseau virtualisée et sécurisée

![statut](https://img.shields.io/badge/Statut-En%20cours-yellow?style=flat-square&logo=github)
![FR](https://img.shields.io/badge/Langue-Fran%C3%A7ais-blue?style=flat-square&logo=github)

![pfSense](https://img.shields.io/badge/Routeur-pfSense-red?style=flat-square&logo=pfsense)
![Linux](https://img.shields.io/badge/Linux-DHCP%20--%20DNS%20(Bind9)-red?style=flat-square&logo=linux)
![Windows](https://img.shields.io/badge/Windows-DC%20--%20AD--DS%20--%20WSUS%20--%20GPO%20--%20R%C3%A9plication-blue?style=flat-square&logo=github)
![Téléphonie](https://img.shields.io/badge/T%C3%A9l%C3%A9phonie-VoIP%20--%203CX-green?style=flat-square&logo=github)
![Audit](https://img.shields.io/badge/Audit%20de%20s%C3%A9curit%C3%A9-PingCastle%20--%20Lynis-purple?style=flat-square&logo=github)

![Sauvegarde](https://img.shields.io/badge/Sauvegarde-Bareos%20--%20RAID%20--%20NAS-pink?style=flat-square&logo=github)
![GestionIT](https://img.shields.io/badge/Gestion%20IT-GLPI%20--%20Apache%20--%20Nginx-orange?style=flat-square&logo=github)
![Cloud](https://img.shields.io/badge/Cloud-Seafile%20--%20Nextcloud-green?style=flat-square&logo=nextcloud)
![VPN](https://img.shields.io/badge/VPN-OpenVPN-red?style=flat-square&logo=openvpn)
![NTP](https://img.shields.io/badge/NTP-Chrony-red?style=flat-square&logo=github)

![Monitoring](https://img.shields.io/badge/Monitoring-Zabbix-pink?style=flat-square&logo=github)
![Coffre](https://img.shields.io/badge/Coffre%20fort-Vaultwarden-purple?style=flat-square&logo=vaultwarden)
![Journalisation](https://img.shields.io/badge/Journalisation-Graylogs-pink?style=flat-square&logo=graylog)
![Messagerie](https://img.shields.io/badge/Messagerie-iRedmail-green?style=flat-square&logo=github)
![Virtualisation](https://img.shields.io/badge/Virtualisation-VMware-white?style=flat-square&logo=vmware)

## 📝 Contexte
**Master Your Network** est un projet **professionnel et personnel** visant à concevoir, configurer et administrer une **infrastructure réseau d’entreprise complète**, virtualisée et sécurisée.  
Il s’agit d’une évolution individuelle du projet collaboratif « *Build Your Infrastructure* », réalisé dans le cadre de ma formation en **Technicien Supérieur Systèmes & Réseaux (Bac +2)**.

Ce projet a pour but de mettre en pratique l’ensemble des compétences attendues d’un **TSSR**, tout en intégrant certaines notions avancées, sans pour autant dépasser le niveau attendu pour un TSSR.

L’environnement reproduit une **entreprise de 50 utilisateurs**, avec une segmentation réseau complète (LAN, VLAN, DMZ) et des services essentiels tels que l’Active Directory, le DNS/DHCP, la supervision, la sauvegarde, la messagerie, la VoIP ou encore le partage de fichier interne.  
L’ensemble est pensé pour être **réaliste, pédagogique et totalement maîtrisable**, afin de permettre une compréhension concrète des mécanismes d’un système d’information d’entreprise.

L’infrastructure est entièrement virtualisée sous **VMware Workstation Pro**, sur un système hôte **Windows 11 Professionnel** disposant des ressources suivantes :
- **Processeur** : 13th Gen Intel(R) Core(TM) i7-13620H (2.40 GHz) - 10 cœurs / 16 threads
- **Mémoire vive** : 32 Go RAM
- **Virtualisation** : VMware Workstation Pro

Ce projet constitue à la fois une **mise en situation professionnelle** et un **laboratoire d’apprentissage personnel**, combinant rigueur technique, sécurité, documentation et méthodologie projet.

## 🔧 **Objectifs du projet**
> 🎯 Déployer une infrastructure d’entreprise complète sous environnement virtualisé (VMware), mêlant systèmes Windows et Linux.  

L’objectif est d’acquérir une maîtrise pratique des domaines suivants :

- 🧩 **Réseau & Infrastructure** : routage, VLANs, pare-feu, VPN, NTP.  
- 🧠 **Systèmes & Services** : Active Directory, DNS/DHCP, WSUS, Cloud, Web, VoIP, messagerie.  
- 🔎 **Sécurité & Supervision** : Zabbix, Graylog, Vaultwarden, Bareos, audits PingCastle/Lynis.  
- 📘 **Documentation & Maintenance** : GLPI, guides d’exploitation, règles ITIL, sauvegardes.

> L’ensemble du projet est réalisé en neuf étapes, de la planification initiale à la documentation finale.

## 🎯 Les étapes de conception
### 🗂️ [Étape 1 – Planification et Préparation](/Installations/Etape1/0-index.md)
🎯 **Objectif**  
Définir la base du projet et concevoir l’architecture complète.

🧠 **Contenu**
- 🏢 **Présentation de l'entreprise** : Définir le contexte, le secteur, les besoins métier et les enjeux IT. 
- 🧾 **Inventaire et nomenclature de l'infrastructure** : Lister les serveurs, postes, équipements réseau et services attendus ; traduire en machines virtuelles avec un nommage clair et logique.
- 💽 **La virtualisation avec VMware Workstation** : Caractéristiques des VMs, création des VMs par rapport aux étapes de conception et création d'une VM avec VMware Workstation.
- 🗺️ **Arborescence Windows de l'infrastructure** : Organisation de l’Active Directory (OU, groupes, GPO).
- 🌐 **Plan réseau de l'infrastructure** : Segmentation des différents réseaux , schéma des VLANs, LAN/DMZ et plan d'interconnexion.

---

### 🌐 [Étape 2 – Réseau et Socle Système](/Installations/Etape2/0-index.md)
🎯 **Objectif**  
Mettre en place l’infrastructure réseau et les services de base.

🧠 **Contenu**
- 🛜 **Mise en place du routeur pfSense** : Configuration initial du WAN/LAN/DMZ et des règles de pare-feu de base.
- 🖧 **Mise en place des VLANs** : Configuration de trois VLANs dans le LAN via et géré par le routeur pfSense.
- 🐧 **Mise en place du serveur Linux primaire** : Configuration d'un serveur debian non graphique pour les services DHCP (isc-dhcp-server) & DNS (bind9).
- 🪟 **Mise en place des serveurs Windows primaire et secondaire** : Configuration d'un Windows Serveur *GUI* en tant que *Domain Controller 1* (Rôle : Active Directory, DNS intégré, GPO, SMB) et d'un Windows Serveur *Core* en tant que *Domain Controller 2* (Réplication du DC1).

---

### 🖥️ Étape 3 – Active Directory, GPO et Partages
🎯 **Objectif**  
Structurer et sécuriser le domaine d’entreprise.

🧠 **Contenu**
- 👥 **Configuration de l'Active Directory** : Création des unités d'organisations (OU), des groupes, et des utilisateurs.
- 🖥️ **Mise en place des postes utilisateurs** : Intégration au domaine de deux postes clients, l'un pour les employés de la DSI et l'autre pour le reste des employés.
- 📜 **Mise en place des GPO** : Configuration des GPO standard et de sécurité.
- 💾 **Mise en place des partages de fichier SMB** : Configuration du rôle SMB (droits NTFS & groupes AD).

---

### 🔒 Étape 4 – Maintenance, Mises à jour et Audit
🎯 **Objectif**  
Automatiser la maintenance et évaluer la sécurité du domaine Windows.

🧠 **Contenu**
- 📡 **Mise en place de GPO de télémétrie** : Configuration de la collecte de données de diagnostic Windows au niveau du domaine.
- 🛰️ **Mise en place d'un service de mise à  Windows** : Installation et configuration d'un serveur WSUS distinct des serveurs DC1 & DC2.
- 📞 **Mise en place de la téléphonie VoIP** : Installation et configuration de 3CX, et tests d'appels.
- 🧍‍♂️ **Audit de sécurité Windows** : Installation et configuration du PC d'audit Windows avec PingCastle, et tests d'audit des serveurs Windows.

---

### 💾 Étape 5 – Stockage, Sauvegarde et Cloud
🎯 **Objectif**  
Assurer la sauvegarde et la résilience selon la règle **3-2-2-1-0**.

🧠 **Contenu**
- 🗄️ **Mise en place du serveur de sauvegarde** : Installation et configuration de Bareos avec NAS & RAID, avec test de restauration de fichier.
- 📀 **Mise en place RAID miroir sur serveur Windows** : Installation et configuration d'un RAID sur le serveur DC1.
- ☁️ **Mise en place du cloud interne** : Installation et configuration de Seafile dans le LAN
- ☁️ **Mise en place du cloud externe** : Installation et configuration de NextCloud dans la DMZ.

---

### 🛡️ Étape 6 – Accès distant, Synchronisation, Supervision et Sécurité
🎯 **Objectif**  
Garantir la sécurité, la supervision et l’administration distante.

🧠 **Contenu**
- 🕳️ **Mise en place d'un serveur VPN** : Installation et configuration d'un accès distant sécurisé avec OpenVPN.
- 🧭 **Mise en place d'un serveur de temps** : Installation et configuration d'un serveur NTP Chrony.
- 📈 **Mise en place d'un serveur de monitoring** : Installation et configuration d'un serveur Zabbix qui surveille le LAN et la DMZ.
- 🔐 **Mise en place d'un serveur coffre-fort de mot de passe** : Installation et configuration de Vaulwarden.

---

### 🌍 Étape 7 – Services Web, Logs, Support et Messagerie
🎯 **Objectif**  
Finaliser les services internes, externes et de support utilisateur.

🧠 **Contenu**
- 🧰 **Mise en place d'un serveur de gestion d'incident** : Installation et configuration de GLPI (LDAP, Helpdesk, ITIL).
- 🔎 **Mise en place d'un serveur central de journalisation** : Installation et configuration de Graylogs pour récupérer les logs de serveur Windows, Linux & pfSense.
- 🌐 **Mise en place de l'intranet et de l'extranet** : Installation et configuration d'Apache pour l'intranet et de Nginx pour l'extranet.
- 💌 **Mise en place d'un serveur de messagerie** : Installation et configuration d'iRedMail.

---

### 🔐 Étape 8 – Renforcement de la Sécurité et Audits
🎯 **Objectif**  
Auditer, corriger et renforcer la sécurité globale du SI.

🧠 **Contenu**
- 🧱 **Configuration avancée du routeur pfSense** : Amélioration des règles de sécurité du pare-feu pfSense (filtrage, flux DMZ/LAN, journalisation).
- 🧩 **Configuration des rôles FSMO des Domain Controler Windows** : Répartition des cinq rôles FSMO entre le Domain Controller 1 et 2.
- 🧍‍♂️ **Audit de sécurité Windows** : Seconde analyse avec PingCastle et correctifs.
- 🧍 **Audit de sécurité Linux** : Installation et configuration du PC d'audit Linux avec Lynis, et tests d'audit des serveurs Linux (Debian/Ubuntu).

---

### 📘 Étape 9 – Documentation et Bilan de Projet
🎯 **Objectif**  
Clôturer le projet, documenter et transmettre les connaissances.

🧠 **Contenu**
- 🧾 **Guide administrateur** : architecture, procédures, maintenance, supervision.  
- 📗 **Guide utilisateur** : accès aux services (mail, cloud, VPN, intranet).  
- 🏁 **Compte-rendu du projet** : remarques, difficultés, réussites, apprentissages et améliorations futures.

## 📌 Avancement du projet
- ✅ 🗂️ Étape 1 – Planification et Préparation
- ⬜ 🌐 Étape 2 – Réseau et Socle Système
- ⬜ 🖥️ Étape 3 – Active Directory, GPO et Partages
- ⬜ 🔒 Étape 4 – Maintenance, Mises à jour et Audit
- ⬜ 💾 Étape 5 – Stockage, Sauvegarde et Cloud
- ⬜ 🛡️ Étape 6 – Accès distant, Synchronisation, Supervision et Sécurité
- ⬜ 🌍 Étape 7 – Services Web, Logs, Support et Messagerie
- ⬜ 🔐 Étape 8 – Renforcement de la Sécurité et Audits
- ⬜ 📘 Étape 9 – Documentation et Bilan de Projet

## 🗂️  Organisation du dépôt
Ce dépôt sera organisé comme suit :
- Un dossier **Installations** où seront répertoriées les fiches d'installations de chacune des étapes de conception de l'infrastructure
- Un dossier **Guides** où seront répertoriées les fiches **Guide de l'utilisateur** & **Guide de l'administrateur**

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
│   └── ...
│   └── Etape9
│       └── Ressources
│           └── Files
│       └── index.md
│       └── task1.md
│       └── task2.md
│       └── task3.md
├── Guides
│   └── UserGuide.md
│   └── AdminGuide.md
│   └── CompteRendu.md
```

## ⚠️ Disclaimer
> Ce projet est un **laboratoire personnel** : il a pour but de me former aux infrastructures réseau hybrides (Linux/Windows), de documenter mes pratiques, et de constituer un support concret dans mon portfolio.  
> Il ne représente pas une infrastructure de production, mais une maquette réaliste et reproductible en environnement virtualisé.

---

[![Profil](https://img.shields.io/badge/Back%20to-SybillLabs%20(Profil)-blue?style=social&logo=github)](https://github.com/SybillLabs)