# 🧠 Master Your Network
Master Your Network est un projet personnel réalisé en environnement virtualisé sous Linux, ayant pour objectif de concevoir, configurer et superviser une infrastructure réseau sécurisée complète et fonctionnelle.

Ce projet simule un réseau d’entreprise modulaire, structuré et supervisé, intégralement hébergé dans VirtualBox, afin de reproduire un environnement réaliste et pédagogique pour l’apprentissage des métiers de l’administration système et réseau.

## 🔧 Objectifs techniques du projet :
- 🔐 Routeur sécurisé : configuration d’un routeur virtuel avec pare-feu intégré (pfSense).
- 🌐 Switchs & VLANs : segmentation du réseau via VLANs pour isoler logiquement les machines.
- 📦 Services réseau : déploiement de serveurs DHCP et DNS sur Linux pour la gestion des adresses et des noms.
- 🪟 Services Windows : intégration d’un Active Directory, WSUS et GPO sur une machine Windows Server.
- 📊 Supervision : installation et configuration de Zabbix (ou équivalent) pour surveiller l’état de l’infrastructure.
- 📜 Journalisation : centralisation et analyse des logs système avec Syslog et Systemd journal.
- 💾 Stockage & Sauvegarde : mise en place d’un système de stockage sécurisé combinant RAID, LVM et Bareos.
- 🌐 Portail web & services IT : création d’un serveur web centralisant l’accès aux outils (GLPI, Zabbix, iRedMail...).

L’ensemble du projet est déployé dans un environnement virtualisé principalement sous Linux (Debian/Ubuntu), intégrant également des machines Windows pour une simulation complète et réaliste d’une infrastructure réseau d’entreprise.
L’objectif est d’acquérir une maîtrise pratique des fondamentaux de l’administration d’infrastructures sécurisées hybrides.

---

# 🎯 Les objectifs du projet

## 🗂️ Objectif 1 : Préparation et planification
- 🧾 Inventaire des machines nécessaires (réseau et système)
- 🏷️ Nomenclature détaillée des machines et plan d’adressage IP
- 🗺️ Arborescence et schéma global de l’infrastructure
- 🗓️ Élaboration du planning détaillé du projet

## 🛠️ Objectif 2 : Mise en place de l'infrastructure réseau de base
- 🔐 Installation et configuration du routeur pfSense
- 🧬 Configuration du switch et des VLANs
- 🐧 Déploiement du serveur Linux (services DHCP & DNS)
- 🪟 Déploiement du serveur Windows (Active Directory, WSUS, GPO)
- 💻 Mise en place d’une machine cliente Windows et d’une machine cliente Linux, intégrées au réseau

## 🔧 Objectif 3 : Finalisation des services réseaux critiques
- ✅ Affinement et tests du serveur DHCP
- 🧠 Affinement et tests du serveur DNS
- 🗃️ Configuration avancée d’Active Directory
- 📥 Configuration et gestion de WSUS

## 🔐 Objectif 4 : Sécurisation de l’infrastructure réseau
- 🧱 Mise en place des règles firewall sur le routeur pfSense
- 🛡️ Création et déploiement des GPO de sécurité et GPO standard

## 📡 Objectif 5 : Supervision, journalisation et sauvegarde
- 📊 Mise en place du système de supervision (Zabbix ou PRTG)
- 📜 Implémentation de la centralisation des logs (journalisation)
- 💾 Installation et configuration du système de sauvegarde et stockage (Bareos, RAID, LVM)

## 🌐 Objectif 6 : Portail d’accès et gestion IT
- 🧭 Mise en place d’un serveur Web portail (dashboard d’accès aux systèmes)
- 📧 Déploiement d’une solution mail complète (iRedMail)
- 🧰 Installation et configuration de GLPI (gestion de parc & helpdesk)

## ✅ Objectif 7 : Tests et validation
- 📶 Vérification de la connectivité et de l’isolation réseau (ping, DNS, accès VLAN)
- 🔒 Tests de sécurité (règles firewall, GPO, accès restreints)
- ⚙️ Tests fonctionnels de tous les services installés (Zabbix, GLPI, Bareos, Webmail…)
- 📈 Tests de performance (bande passante, charge CPU/RAM des serveurs)
- 🗃️ Documentation des résultats et création d’une checklist de validation finale

---
