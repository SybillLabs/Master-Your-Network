# 🌐 Plan réseau de l'infrastructure

![Schématisation](https://img.shields.io/badge/Sch%C3%A9matisation-Draw.io-white?style=flat-square&logo=github)

## 📝 Contexte
Après avoir défini l’infrastructure globale de **NovaPlay Studio** (inventaire, nomenclature et arborescence Active Directory), il est maintenant essentiel de concevoir un plan réseau clair et structuré.

🎯 **Objectif du plan réseau** :  
Ce document a pour but de définir l’architecture logique du réseau de l’entreprise, d’identifier les différentes zones (LAN, WAN, DMZ) et de préciser leur rôle au sein de l’infrastructure.
L’objectif est de garantir :
- 🔐 La **sécurité** des échanges internes et externes,
- ⚙️ La **stabilité** et la **performance** des communications,
- 🧭 La **cohérence** entre l’environnement réel et virtualisé.

💡 **Contexte du projet** :  
NovaPlay Studio est une entreprise en pleine croissance d’environ 50 employés, dont les activités reposent sur des services réseau essentiels (authentification, partage de fichiers, hébergement web, messagerie, etc.).  
Jusqu’à présent, le réseau était basique — un simple accès internet et quelques postes en Wi-Fi — sans réelle segmentation ni infrastructure sécurisée.

Dans le cadre de ce projet, je mets en place une nouvelle architecture réseau complète, pensée pour :
- Séparer les flux selon leur nature (utilisateurs, serveurs, DMZ, administration),
- Introduire une couche de sécurité via le routeur pfSense,
- Optimiser la gestion des adresses IP grâce à un plan d’adressage clair et dynamique (DHCP),
- Et poser les bases des futures configurations réseau lors de la mise en œuvre.

## 🌐 Les grands réseaux

🔹 🌍 **WAN** : Wide Area Network  
C’est le **réseau étendu** fourni par le **Fournisseur d’Accès Internet (FAI)** de l’entreprise. Il représente la **partie publique** du réseau, permettant la communication avec l’extérieur.  
Dans mon projet, le réseau **WAN** est simulé par le **NAT** de VirtualBox. Son adresse IP est attribuée dynamiquement au démarrage de la machine virtuelle.

🔹 🏢 **LAN** : Local Area Network  
C’est le **réseau local interne** à l’entreprise. Il constitue la **partie privée**, dédiée aux postes de travail, serveurs internes et imprimantes.  
Dans mon projet, je configure le réseau **LAN** sur le sous-réseau suivant : `192.168.1.0/24`.  

🔹 🛡️ **DMZ** : DeMilitarized Zone  
C’est une **zone intermédiaire** entre le **LAN** et le **WAN**, utilisée pour héberger des serveurs accessibles depuis Internet (ex : site web, serveur mail) sans exposer directement le réseau interne.  
Dans mon projet, je configure le réseau **DMZ** sur le sous-réseau suivant : `192.168.0.0/24`.

## 🛜 Le routeur pfSense
Un **routeur** est un **équipement de niveau 3** du modèle OSI (*couche Réseau*).  
Il permet de **faire transiter (routage)**, **filtrer** et **sécuriser** les flux de données entre **différents réseaux ou sous-réseaux**.  

Dans mon projet, le routeur **pfSense** assure la communication entre le LAN, la DMZ et le WAN, tout en appliquant les **règles de sécurité** nécessaires.  

Le routeur **pfSense** a 3 interfaces : 
- 🌍 **WAN** : Adresse IP de la **passerelle** simulé par le **NAT** de VirtualBox
- 🏢 **LAN** : Adresse IP de la **passerelle** : `192.168.1.254/24`
- 🛡️ **DMZ** : Adresse IP de la **passerelle** : `192.168.0.254/24`

## 🧩 Les VLANs
Pour mon projet, j'ai décidé de faire **trois VLANs** au sein du réseau **LAN** :
- 👥 **VLAN Users** : destiné aux utilisateurs standard de l’entreprise
- 💻 **VLAN DSI Users** : réservé aux utilisateurs du service informatique (DSI)
- 🖥️ **VLAN DSI Servers** : dédié aux serveurs internes de l’entreprise (hors DMZ)

🔹 **Pourquoi mettre en place des VLANs ?**  
La création de VLANs (*Virtual Local Area Networks*) permet d’améliorer la **sécurité** et la **gestion du réseau**.  
Elle offre plusieurs avantages :  
- 🚫 **Isolation des flux** : un poste d’un VLAN ne peut pas communiquer directement avec un autre VLAN (ex. : *VLAN Users* ne peut pas accéder au *VLAN DSI Servers*).
- 🛡️ **Sécurité renforcée** : en cas de compromission d’un poste, l’incident reste contenu dans le VLAN concerné.
- 🧭 **Meilleure organisation** : la segmentation du LAN facilite la gestion, la supervision et le dépannage.

🔹 **Plage d'adresse des VLANs** 
| VLANs ID | Nom du VLAN      | Adresse réseau  | Masque de sous-réseau | Passerelle par défaut |
| -------: | ---------------- | --------------- | --------------------- | --------------------- |
| 10       | VLAN Users       | 192.168.10.0/24 | 255.255.255.0         | 192.168.10.254        |
| 20       | VLAN DSI Users   | 192.168.20.0/24 | 255.255.255.0         | 192.168.20.254        |
| 30       | VLAN DSI Servers | 192.168.30.0/24 | 255.255.255.0         | 192.168.30.254        |

> Chaque VLAN dispose de sa propre passerelle configurée sur le routeur pfSense, afin de permettre l’accès à Internet via le WAN tout en maintenant une isolation stricte entre les différents réseaux internes.

## 🧭 Le DHCP : Plan d'adressage réseau
🔹 **Le DHCP**  
Le **D**ynamic **H**ost **C**onfiguration **P**rotocol évite d'avoir à configurer manuellement les adresses sur chaque poste sauf si cela est nécessaire (*Exemple : les serveurs*).  
Dans mon projet, je vais avoir 3 cas :
- ⚙️ **Dynamique** : Le serveur DHCP attribue automatiquement une adresse IP depuis une **plage (pool)** définie pour un sous-réseau. L’adresse est temporaire et renouvelée périodiquement.
- 📘 **Statique (réservation)** : Le serveur DHCP attribue toujours **la même adresse IP** à une machine spécifique, basée sur son **adresse MAC**.
- 🖥️ **Manuelle** : L’adresse IP est **configurée directement sur la machine**. Elle n’est donc **pas dépendante du serveur DHCP**, ce qui est essentiel pour les **serveurs critiques** (afin qu’ils conservent leur IP même si le DHCP tombe).

💡 Le serveur **DHCP** sera configuré pour **travailler conjointement avec le routeur pfSense**, notamment pour la gestion des **VLANs** et des **plages d’adresses associées**.

🔹 **Plage d'adressage réseau**  
| #  | Tag VM         | Hostname        | VLANs ID    | DHCP      | Adresse IP                                      | Gateway        |
| -- | -------------- | --------------- |------------ | --------- | ----------------------------------------------- | -------------- |
| 01 | **ns-router**  | `GoGunHee`      | —           | NAT       | eth0 (WAN) - NAT                                | —              |
|    |                |                 | —           | Manuelle  | eth1 (LAN Trunk 10/20/30) - 192.168.1.254       | —              |
|    |                |                 | Users       | Dynamique | eth1.10 - 192.168.10.254                        | —              |
|    |                |                 | DSI Users   | Statique  | eth1.20 - 192.168.20.254                        | —              |
|    |                |                 | DSI Servers | Manuelle  | eth1.30 - 192.168.30.254                        | —              |
|    |                |                 | —           | Manuelle  | eth2 (DMZ) - 192.168.0.254                      | —              |
| 02 | **ns-lnx**     | `Tank`          | DSI Servers | Manuelle  | 192.168.30.1/24                                 | 192.168.30.254 |
| 03 | **ns-ad01**    | `SungJinwoo`    | DSI Servers | Manuelle  | 192.168.30.2/24                                 | 192.168.30.254 |
| 04 | **ns-ad02**    | `YooJinho`      | DSI Servers | Manuelle  | 192.168.30.3/24                                 | 192.168.30.254 |
| 05 | **ns-user01**  | `Monarch`       | DSI Users   | Statique  | 192.168.20.1/24                                 | 192.168.20.254 |
| 06 | **ns-user02**  | `Hunter`        | Users       | Dynamique | DHCP Pool : 192.168.10.1/24 à 192.168.10.253/24 | 192.168.10.254 |
| 07 | **ns-wsus**    | `NormaSelner`   | DSI Servers | Manuelle  | 192.168.30.4/24                                 | 192.168.30.254 |
| 08 | **ns-voip**    | `BaekYoonHo`    | DSI Servers | Manuelle  | 192.168.30.5/24                                 | 192.168.30.254 |
| 09 | **ns-audit01** | `Igris`         | DSI Servers | Manuelle  | 192.168.30.6/24                                 | 192.168.30.254 |
| 10 | **ns-backup**  | `Beru`          | DSI Servers | Manuelle  | 192.168.30.7/24                                 | 192.168.30.254 |
| 11 | **ns-it**      | `Bellion`       | DSI Servers | Manuelle  | 192.168.30.8/24                                 | 192.168.30.254 |
| 12 | **ns-web**     | `EsilRadiru`    | —           | Manuelle  | 192.168.0.1/24                                  | 192.168.0.254  |
| 13 | **ns-vpn**     | `AdamWhite`     | —           | Manuelle  | 192.168.0.2/24                                  | 192.168.0.254  |
| 14 | **ns-ntp**     | `Rulers`        | DSI Servers | Manuelle  | 192.168.30.9/24                                 | 192.168.30.254 |
| 15 | **ns-moni**    | `Kandiaru`      | DSI Servers | Manuelle  | 192.168.30.10/24                                | 192.168.30.254 |
| 16 | **ns-safe**    | `Kamish`        | DSI Servers | Manuelle  | 192.168.30.11/24                                | 192.168.30.254 |
| 17 | **ns-logs**    | `AbsoluteBeing` | DSI Servers | Manuelle  | 192.168.30.12/24                                | 192.168.30.254 |
| 18 | **ns-mail**    | `Tusk`          | —           | Manuelle  | 192.168.0.3/24                                  | 192.168.0.254  |
| 19 | **ns-audit02** | `Kaisel`        | DSI Servers | Manuelle  | 192.168.30.13/24                                | 192.168.30.254 |

> Le plan d’adressage a été conçu pour assurer une gestion claire et segmentée des adresses IP selon les rôles et VLANs. Les serveurs critiques utilisent des adresses fixes pour garantir la stabilité, tandis que les postes utilisateurs bénéficient d’une attribution automatique via DHCP.

## Schéma d'infrastructure
![schema](/Installations/Etape1/Ressources/InfraNS.drawio.png)

---

[![STEP1](https://img.shields.io/badge/Back%20to-Etape%201%20%3A%20Pr%C3%A9paration%20et%20planification-blue?style=social&logo=github)](/Installations/Etape1/0-index.md)
  
[![README](https://img.shields.io/badge/Back%20to-Master%20your%20network-blue?style=social&logo=github)](/README.md)