# 🧠 Master Your Network
_Master Your Network_ est un projet personnel de virtualisation que je réalise sous Linux. L’idée, c’est de concevoir, configurer et superviser une infrastructure réseau complète, sécurisée et fonctionnelle, pour apprendre concrètement les bases des métiers liés à l’administration système et réseau.

Ce projet reproduit un réseau d’entreprise modulaire, structuré et surveillé, entièrement virtualisé via ~~VirtualBox~~ -> **VMware Workstation Pro**. L’objectif, c’est d’avoir un environnement réaliste, pédagogique et surtout totalement maîtrisable.

## 🔧 Objectifs finaux du projet :

- 🔐 **Routeur sécurisé** : configuration d’un routeur virtuel avec un pare-feu intégré (pfSense).
- 🌐 **Switchs & VLANs** : segmentation logique du réseau avec des VLANs pour isoler les différentes machines.
- 📦 **Services réseau** : déploiement de serveurs DHCP et DNS sous Linux pour gérer les adresses IP et les noms de domaine.
- 🪟 **Services Windows** : mise en place d’un Active Directory, WSUS, GPO, et configuration d’un système de partage de fichiers via les dossiers réseau.
- 📊 **Supervision** : installation et configuration de Zabbix (ou équivalent) pour garder un œil sur l’état de toute l’infra.
- 📜 **Journalisation** : centralisation et analyse des logs avec Syslog et Systemd journal.
- 💾 **Stockage & sauvegarde** : solution de stockage sécurisée combinant, RAID et Bareos. Pour résumé, c'est le coffre-fort de l'entreprise.
- 🌐 **Portail web & outils IT** : création d’un serveur web pour centraliser l’accès aux outils comme GLPI, Zabbix, iRedMail, etc.

L’environnement est principalement basé sur Linux (Debian/Ubuntu), avec aussi des machines Windows pour simuler au mieux une vraie infrastructure d’entreprise hybride. Le but, c’est de développer une vraie maîtrise terrain de la gestion d’un réseau sécurisé.

---

# 🎯 Les objectifs du projet

## 🗂️ Objectif 1 : Préparation et planification
- 🏢 [Présentation de l'entreprise](/Objectif_1/00-presentation-entreprise.md)
- 🧾 [Inventaire des machines nécessaires](/Objectif_1/01-inventaire_machines.md)
- 🏷️ [Nomenclature + plan d’adressage IP avec schéma](/Objectif_1/02-nomenclature.md)
- 🗺️ [Arborescence de l’infrastructure](/Objectif_1/03-arborescence.md)
- 🗓️ [Planning détaillé du projet](/Objectif_1/04-planning.md)

## 🛠️ Objectif 2 : Mise en place de l’infrastructure réseau de base

- 🔐 [Installation et configuration de pfSense](/Objectif_2/00-firewall.md)
- 🧬 [Configuration du switch VLANs](/Objectif_2/01_VLANs.md)
- 🐧 Déploiement d’un serveur Linux (DHCP + DNS)
- 🪟 Déploiement du serveur Windows (AD, WSUS, GPO)
- 💻 Ajout d’un poste client Windows + machine d’admin Linux intégrées au réseau

## 🔧 Objectif 3 : Finalisation des services réseau critiques

- ✅ Tests et ajustements du DHCP
- 🧠 Ajustements du DNS
- 🗃️ Config avancée d’Active Directory
- 📥 Mise en place + gestion de WSUS

## 🔐 Objectif 4 : Sécurisation du réseau

- 🧱 Règles firewall sur pfSense
- 🛡️ Déploiement des GPO de sécurité et de base

## 📡 Objectif 5 : Supervision, journalisation, sauvegarde

- 📊 Installation de Zabbix
- 📜 Mise en place de la journalisation centralisée
- 💾 Configuration de Bareos, RAID, NAS
- 📁 Partage de fichiers via les dossiers réseau Windows (intégrés à l’AD)

## 🌐 Objectif 6 : Portail web + outils IT

- 🧭 Création d’un portail d’accès centralisé (dashboard web)
- 📧 Installation de iRedMail
- 🧰 Configuration de GLPI (gestion de parc et helpdesk)
- ⏰ Serveur de temps NTP

## ✅ Objectif 7 : Tests et validation

- 📶 Vérif connectivité, DNS, VLAN
- 🔒 Audit de sécurité du routeur, du serveur linux et du serveur Windows
- ⚙️ Tests de tous les services installés
- 📈 Tests de performance (bande passante, charge des serveurs)
- 🗃️ Documentation + checklist de validation

---
# 🏗️ Cadre et contexte du projet
## 🗓️ Durée et rythme de travail
Ce projet est 100 % solo. Je bosse dessus du *lundi au vendredi*, à raison d’environ de *4 heures par jour*.

Avec ce rythme, je vise une date de fin pour le **30 août 2025**, incluant les phases de tests et de validation.

Pour suivre l’avancement, voici le lien vers le planning du projet : [Planning GoogleSheets](https://docs.google.com/spreadsheets/d/1zhlR8zkiVm_Ano6SkIDbGHE1j4LoGr4-Lp43iBQBKpQ/edit?usp=sharing).

## 🖥️ Matériel utilisé (ordinateur hôte)
Ce projet est fait sur ma machine personnelle, avec cette config :

- **CPU** : AMD Ryzen 7 5800X – 8 cœurs / 16 threads à 3,8 GHz
- **RAM** : 32 Go DDR4 3200 MHz
- **GPU** : ASUS TUF GeForce RTX 3070 – 8 Go dédiés
- **OS hôte** : Kubuntu 22.04
- **Stockage** : SSD 250 Go pour le système + HDD 1 To pour les données
- **Virtualisation** : ~~VirtualBox~~ VMware Workstation Pro
- **Réseau** : Wi-Fi
- **Périphériques** : setup multi-écrans, clavier, souris

Cette configuration me permet de faire tourner plusieurs VM sans trop galérer, même en pleine supervision ou configuration lourde.

## ⚖️ Contraintes et points forts

| ⚠️ Contraintes potentielles                                                                                                                                                                         | ✅ Avantages du projet                                                                                                                                             |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ⏱️ **Temps limité quotidien** : 4 heures par jour, ce qui est un rythme soutenu mais limite les longues sessions de travail intensif ou imprévus.                                                   | 💪 **Configuration matérielle puissante** : CPU Ryzen 7 5800X avec 16 threads et 32 Go de RAM, largement suffisant pour plusieurs VMs et charges réseau modérées. |
| 🧍‍♀️ **Travail en solo** : absence d’aide directe, donc toute difficulté technique ou bug doit être résolue seule, ce qui peut rallonger les délais.                                               | 🐧 **Environnement Linux stable** : Kubuntu 22.04 comme système hôte, réputé pour sa stabilité et performance dans la virtualisation.                             |
| 🌐 **Connexion WiFi** : peut parfois être instable ou moins performante qu’une connexion filaire, impactant les tests réseau et téléchargements.                                                    | 📦 ~~**Virtualisation légère** : VirtualBox est simple à configurer et à utiliser, facilitant la gestion quotidienne des machines virtuelles.~~                   |
| 🖥️ **Ressources matérielles partagées** : l’ordinateur personnel est utilisé à la fois pour la virtualisation et d’autres tâches, ce qui peut réduire les ressources disponibles selon les usages. | 🎛️ **Contrôle total** : projet personnel et solo, donc pleine liberté pour planifier, ajuster et expérimenter sans contraintes externes.                         |
| 🔒 **Virtualisation sur un seul hôte** : limite le nombre et la complexité des machines virtuelles que tu peux faire tourner en même temps, impactant certains scénarios ou tests.                  | 🖥️ **Multi-écrans** : facilite le multitâche, la supervision et le suivi simultané de plusieurs consoles ou interfaces.                                          |
| 📦 ~~**Dépendance logicielle** : usage exclusif de VirtualBox, qui peut avoir certaines limitations ou bugs propres comparé à d’autres solutions comme VMware ou Proxmox.~~                         | 🛠️ **Accès à une large palette de technologies** : Linux, Windows, pfSense, Zabbix, GLPI, etc. pour une expérience complète et diversifiée.                      |
| 🔄 **Multitâche et interruptions** : gérer projet et vie personnelle sur le même poste peut entraîner des interruptions ou des baisses de concentration.                                            | 📆 **Rythme régulier** : travail du lundi au samedi, ce qui assure une progression continue et une bonne dynamique d’avancement.                                  |
🔧 **Mise à jour** : J’ai finalement remplacé VirtualBox par **VMware Workstation Pro (version gratuite)**.  
VirtualBox s’est révélé trop limité pour la complexité de mon infrastructure : problèmes de performance, gestion réseau plus rigide, et moins de flexibilité sur certains scénarios de test.


---

## ⚠️ Disclaimer
Ce projet est **purement pédagogique**, réalisé sur mon PC personnel dans un environnement virtualisé et sécurisé.  
Il me permet de progresser dans mes compétences en administration système, réseau et cybersécurité, **dans un cadre 100 % légal**.

📌 **Si vous reprenez ce projet**, vous le faites **à vos risques** :
- Chaque environnement matériel est différent.
- Certaines configs peuvent causer des problèmes si elles sont mal adaptées.
- Je ne pourrai pas être tenue responsable en cas de perte de données ou mauvaise utilisation.

Aucune partie de ce projet n’est destinée à des fins illégales ou malveillantes.
