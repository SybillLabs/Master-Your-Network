# 🧠 Master Your Network
*Master Your Network* est un projet personnel de virtualisation réalisé sous Linux, visant à concevoir, configurer et superviser une infrastructure réseau sécurisée complète et fonctionnelle, dans un but d'apprentissage pratique des métiers de l'administration système et réseau.

Ce projet simule un réseau d’entreprise modulaire, structuré et supervisé, intégralement virtualisé via VirtualBox, afin de reproduire un environnement réaliste et pédagogique et entièrement maîtrisable.

## 🔧 Objectifs finals du projet :
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
# 🏗️ Cadre et contexte du projet
## 🗓️ Durée et rythme de réalisation
Ce projet est un projet solo, non réalisé en groupe. Je prévois de travailler dessus du *lundi* au *samedi*, avec une moyenne de *4 heures par jour*.

Avec ce rythme, je vise une date de fin pour le **30 août 2025**, incluant les phases de tests et de validation.

Pour mieux visualiser l'avançement du projet dans le temps, cliquez sur ce lien pour accéder au planning du projet : [en cours de réalisation].

## 🖥️ Spécificités de l'ordinateur hôte
Ce projet solo est réalisé sur mon ordinateur personnel, dont voici les caractéristiques principales :
- **Processeur (CPU)** : AMD Ryzen 7 5800X, 8 cœurs / 16 threads, fréquence de base 3,8 GHz
- **Mémoire vive (RAM)** : 32 Go DDR4 3200 MHz
- **Carte graphique (GPU)** : ASUS TUF GeForce RTX 3070 avec 8 Go de mémoire dédiée
- **Système d'exploitation hôte** : Kubuntu 22.04 (Linux)
- **Stockage** : SSD de 250 Go dédié au système hôte, plus un disque dur HDD de 1 To pour les données
- **Logiciels de virtualisation utilisés** : VirtualBox
- **Connexion réseau** : WiFi
- **Périphériques** : écran multiple, clavier et souris

Cette configuration me permet d’héberger plusieurs machines virtuelles simultanément, avec des performances stables pour la configuration, le test et la supervision des différents services réseau du projet.

## ⚖️Contraintes et avantages du projet

| ⚠️ Contraintes potentielles                                                                                                                                                                     | ✅ Avantages du projet                                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Temps limité quotidien** : 4 heures par jour, ce qui est un rythme soutenu mais limite les longues sessions de travail intensif ou imprévus.                                                  | **Configuration matérielle puissante** : CPU Ryzen 7 5800X avec 16 threads et 32 Go de RAM, largement suffisant pour plusieurs VMs et charges réseau modérées. |
| **Travail en solo** : absence d’aide directe, donc toute difficulté technique ou bug doit être résolue seule, ce qui peut rallonger les délais.                                                 | **Environnement Linux stable** : Kubuntu 22.04 comme système hôte, réputé pour sa stabilité et performance dans la virtualisation.                             |
| **Connexion WiFi** : peut parfois être instable ou moins performante qu’une connexion filaire, impactant les tests réseau et téléchargements.                                                   | **Virtualisation légère** : VirtualBox est simple à configurer et à utiliser, facilitant la gestion quotidienne des machines virtuelles.                       |
| **Ressources matérielles partagées** : l’ordinateur personnel est utilisé à la fois pour la virtualisation et d’autres tâches, ce qui peut réduire les ressources disponibles selon les usages. | **Contrôle total** : projet personnel et solo, donc pleine liberté pour planifier, ajuster et expérimenter sans contraintes externes.                          |
| **Virtualisation sur un seul hôte** : limite le nombre et la complexité des machines virtuelles que tu peux faire tourner en même temps, impactant certains scénarios ou tests.                 | **Multi-écrans** : facilite le multitâche, la supervision et le suivi simultané de plusieurs consoles ou interfaces.                                           |
| **Dépendance logicielle** : usage exclusif de VirtualBox, qui peut avoir certaines limitations ou bugs propres comparé à d’autres solutions comme VMware ou Proxmox.                            | **Accès à une large palette de technologies** : Linux, Windows, pfSense, Zabbix, GLPI, etc. pour une expérience complète et diversifiée.                       |
| **Multitâche et interruptions** : gérer projet et vie personnelle sur le même poste peut entraîner des interruptions ou des baisses de concentration.                                           | **Rythme régulier** : travail du lundi au samedi, ce qui assure une progression continue et une bonne dynamique d’avancement.                                  |

---

