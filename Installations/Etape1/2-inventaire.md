# 🧾 Inventaire et nomenclature de l'infrastructure

## 📝 Contexte
Avant de concevoir une **infrastructure réseau**, il faut réaliser un inventaire complet de toutes les machines nécessaires à cette infrastructure :
- Serveurs 
- Postes administrateurs
- Postes clients
- Equipements réseaux (switch, routeur,...)

Cet inventaire permet de **définir les rôles, la hiérarchie et la criticité** de chaque composant du futur système d'information.

Une infrastructure comprend généralement :
- 🛜 **Routeur principal** : qui fait le routage entre le **réseau privé de l'entreprise** (DMZ & LAN) et le **réseau public** (WAN) avec un aspect sécurité (règle de pare-feu)
- 🧱 **DMZ (zone démilitarisée)** : zone tampon entre **Internet et le réseau interne**, isolée par un pare-feu. Elle héberge les **services accessibles depuis l’extérieur**, tout en protégeant le LAN.
- 🏠 **LAN (réseau interne)** : zone interne, protégée, qui héberge les **services métiers, d’administration et de stockage**. Le LAN est segmenté en **VLANs** pour séparer les utilisateurs, les administrateurs DSI et les serveurs.

## 📦 Inventaire des équipements par zone
| #  | Nom de l'équipement           | Zone | Fonctions                                                                       |
| -- | ----------------------------- | ---- | ------------------------------------------------------------------------------- |
| 01 | 🛜 Routeur / Pare-feu (pfSense)  | —    | Point central d'interconnexion et de sécurité entre le WAN, le LAN et la DMZ    |
|    |                               |      | Assure le routage inter-zones, le NAT et le filtrage des flux réseaux           |
| 02 | 🐧 Serveur Linux primaire     | LAN  | Attribution des adresses IP et résolution des noms                              |
| 03 | 🪟 Serveur Windows primaire   | LAN  | Gestion de l'Active Directory, des GPO et des partages de fichier SMB           |
| 04 | 🪟 Serveur Windows secondaire | LAN  | Réplication du serveur Windows primaire                                         |
| 05 | 🖥️ Poste utilisateur DSI      | LAN  | Poste client pour les employés de la DSI (Windows 11 Pro)                       |
| 06 | 🖥️ Poste utilisateur          | LAN  | Poste client pour les employés hors DSI (Windows 11 Pro)                        |
| 07 | 🛰️ Serveur Windows update     | LAN  | Gestion des mises à jour Windows avec WSUS pour les PC clients                  |
| 08 | 📞 Serveur de téléphonie IP   | LAN  | Téléphonie IP interne                                                           |
| 09 | 🧍‍♂️ Ordinateur d'audit Windows | LAN  | Tests, vérification, maintenance avec PingCastle                                |
| 10 | 🗄️ Serveur de backup          | LAN  | Sauvegarde Bareos, RAID et stockage NAS                                         |
| 11 | 🧰 Serveur gestion IT         | LAN  | Gestion d'incident avec GLPI, intranet (Apache) & cloud interne (Seafile)       |
| 12 | 🌐 Serveur web externe        | DMZ  | Extranet (Nginx) & cloud externe (Nextcloud)                                    |
| 13 | 🕳️ Serveur d'accès à distance | DMZ  | Serveur OpenVPN pour une connexion par tunnel sécurisée                         |
| 14 | 🧭 Serveur de temps           | LAN  | Serveur de temps (Chrony) pour une synchronisation horaire sur l'infrastructure |
| 15 | 📈 Serveur de monitoring      | LAN  | Serveur de supervision Zabbix pour surveiller l'état de l'infrastructure        |
| 16 | 🔐 Serveur de coffre fort     | LAN  | Serveur de coffre fort (Vaulwarden) des mots de passe de l'infrastructure       |
| 17 | 🔎 Serveur de journalisation  | LAN  | Journalisation centralisée avec Graylogs pour les serveurs Windows & Linux      |
| 18 | 💌 Serveur de messagerie      | DMZ  | Gère les mails internes et la réception depuis l'extérieur                      |
| 19 | 🧍 Ordinateur d'audit Linux   | LAN  | Tests, vérification, maintenance avec Lynis                                     |

## 🖥️ Nomenclature des équipements
> 🔠 **Convention de nommage**  
> Le choix des **tags VM** est effectué sur la base du *nom de l’entreprise*, du *nom* et de la *fonction de l’équipement*.  
> Ces tags sont uniquement visibles par moi, car ils correspondent aux noms attribués à mes machines virtuelles dans **VirtualBox**.  
>  
> Le choix des **hostnames** est quant à lui motivé par des considérations de sécurité : il vise à éviter que toute personne extérieure à l’entreprise ne puisse deviner la fonction d’un équipement à partir de son nom.  
> Je me suis donc inspiré du manhwa **_Solo Leveling_**, l’une de mes œuvres préférées, pour nommer les différents systèmes de l’infrastructure.
> 
> Bien évidemment, les justifications de ces choix sont présentées ici à titre explicatif pour le portfolio, mais dans un contexte réel de DSI, il n’est pas recommandé de divulguer publiquement la signification des hostnames choisis.

| #  | Tag VM         | Hostname        | Justification                                                                                                          |
| -- | -------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 01 | **ns-router**  | `GoGunHee`      | Comme **Go Gun-Hee qui** veille sur les chasseurs, ce serveur contrôle et protège tout le trafic du réseau             |
| 02 | **ns-lnx**     | `Tank`          | Fidèle et solide comme **Tank**, il assure les fondations du réseau en distribuant adresses IP et noms DNS             |
| 03 | **ns-ad01**    | `SungJinwoo`    | Centre du pouvoir comme **Sung Jinwoo**, il gère utilisateurs, groupes et politiques du domaine                        |
| 04 | **ns-ad02**    | `YooJinho`      | Loyal et dévoué à **Sung Jinwoo**, **Yoo Jinho** réplique et soutient le premier contrôleur de domaine                 |
| 05 | **ns-user01**  | `Monarch`       | Poste DSI représentant l’autorité technique, tel le **Monarque** dominant son royaume                                  |
| 06 | **ns-user02**  | `Hunter`        | Poste utilisateur standard, **chasseur actif** au sein du réseau d’entreprise                                          |
| 07 | **ns-wsus**    | `NormaSelner`   | À l’image de **Norma Selner** qui upgrade les chasseurs, il améliore les systèmes via les mises à jour                 |
| 08 | **ns-voip**    | `BaekYoonHo`    | Chaleureux et fiable comme **Baek Yoon-Ho**, il relie les équipes par la voix et la communication interne              |
| 09 | **ns-audit01** | `Igris`         | Rigoureux et discipliné, **Igris** reflète ce serveur qui contrôle et audite les environnements Windows                |
| 10 | **ns-backup**  | `Beru`          | Protecteur absolu comme **Beru**, il sauvegarde et restaure les données essentielles du royaume numérique              |
| 11 | **ns-it**      | `Bellion`       | Chef d’organisation de l’armée des ombres, **Bellion** coordonne ici la gestion IT, GLPI et l’intranet                 |
| 12 | **ns-web**     | `EsilRadiru`    | Médiatrice entre mondes, **Esil Radiru** incarne ce serveur exposé en DMZ reliant Internet et extranet                 |
| 13 | **ns-vpn**     | `AdamWhite`     | Tel un diplomate entre nations, **Adam White** établit un canal sécurisé entre utilisateurs externes et réseau interne |
| 14 | **ns-ntp**     | `Rulers`        | Les **Rulers** maintiennent l’équilibre du monde, tout comme ce serveur synchronise le temps du réseau                 |
| 15 | **ns-moni**    | `Kandiaru`      | L’**Architecte du Système** observe et évalue, à l’image de Zabbix qui supervise toute l’infrastructure                |
| 16 | **ns-safe**    | `Kamish`        | Tel le cœur scellé du dragon, **Kamish** protège et enferme les secrets dans le coffre-fort numérique                  |
| 17 | **ns-logs**    | `AbsoluteBeing` | Comme l’**Être Suprême** qui voit tout, il enregistre chaque action pour garder la mémoire du système                  |
| 18 | **ns-mail**    | `Tusk`          | **Tusk**, shaman communicateur, incarne ce serveur qui transmet fidèlement les messages internes et externes           |
| 19 | **ns-audit02** | `Kaisel`        | Monture rapide et vigilante, **Kaisel** survole l’infrastructure Linux pour en analyser la sécurité                    |

## ⚙️ Priorités des équipements
### 🔴 Priorité haute 
| #  | Tag VM         | Nom de l'équipement           |
| -- | -------------- | ----------------------------- |
| 01 | **ns-router**  | 🛜 Routeur / Pare-feu (pfSense)  |
| 02 | **ns-lnx**     | 🐧 Serveur Linux primaire     |
| 03 | **ns-ad01**    | 🪟 Serveur Windows primaire   |
| 10 | **ns-backup**  | 🗄️ Serveur de backup          |
| 16 | **ns-safe**    | 🔐 Serveur de coffre fort     |
| 17 | **ns-logs**    | 🔎 Serveur de journalisation  |
| 18 | **ns-mail**    | 💌 Serveur de messagerie      |

### 🟠 Priorité moyenne
| #  | Tag VM         | Nom de l'équipement           |
| -- | -------------- | ----------------------------- |
| 04 | **ns-ad02**    | 🪟 Serveur Windows secondaire |
| 07 | **ns-wsus**    | 🛰️ Serveur Windows update     |
| 08 | **ns-voip**    | 📞 Serveur de téléphonie IP   |
| 09 | **ns-audit01** | 🧍‍♂️ Ordinateur d'audit Windows |
| 11 | **ns-it**      | 🧰 Serveur gestion IT         |
| 12 | **ns-web**     | 🌐 Serveur web externe        |
| 13 | **ns-vpn**     | 🕳️ Serveur d'accès à distance |
| 14 | **ns-ntp**     | 🧭 Serveur de temps           |
| 15 | **ns-moni**    | 📈 Serveur de monitoring      |
| 19 | **ns-audit02** | 🧍 Ordinateur d'audit Linux   |

### 🟢 Priorité basse
| #  | Tag VM         | Nom de l'équipement           |
| -- | -------------- | ----------------------------- |
| 05 | **ns-user01**  | 🖥️ Poste utilisateur DSI      |
| 06 | **ns-user02**  | 🖥️ Poste utilisateur          |

## 🕵️ Bonus : Serveur Bastion (sécurité d’administration)
Le **serveur Bastion** est un équipement de sécurité permettant de **centraliser, tracer et contrôler** les connexions d'administration vers les serveurs internes et ceux situés en DMZ. Il agit comme un **point d'accès unique pour les administrateurs**, en enregistrant leurs connexions et en limitant les accès directs au reste du réseau.

### 📘 Utilité
- Garantir une traçabilité complète des connexions d’administration
- Renforcer la sécurité en isolant le plan de gestion du réseau
- Limiter les risques d’accès non autorisés ou de compromission d’un poste administrateur

### ⚙️ Emplacement théorique 
Dans une infrastructure réelle, il serait positionné dans le **VLAN DSI Servers (LAN)**, avec un accès restreint depuis les **postes DSI Users**, et un accès autorisé vers les **serveurs LAN et DMZ** via le pare-feu pfSense.

### 🔁 Analogie : le Collège
Le **serveur Bastion** est l'équivalent du **surveillant** dans un collège, il surveille les entrées, les sorties et les actions des intervenants en notant tout dans son calepin. Le jour d'une inspection (par exemple un audit de sécurité), il ressort son calepin pour dire qui a fait quoi et à quelle heure sur tel jour.  
Ainsi, le Bastion n’accorde pas d’accès, mais observe et journalise toutes les actions d’administration au sein du réseau interne.

Contrairement au **serveur OpenVPN** qui est l'équivalent du **portail de sécurité** du collège, il ne laisse passer que ceux qui ont reçu une autorisation de passage sinon il refuse l'accès.

### 🧩 Remarque 
Ce composant n’est pas déployé dans le cadre du laboratoire, car il nécessite des compétences avancées en sécurisation d’accès privilégiés (PAM) et une infrastructure plus conséquente en ressources.
Cependant, sa présence est évoquée pour montrer la compréhension du concept et de son importance dans une architecture d’entreprise sécurisée.

---

[![STEP1](https://img.shields.io/badge/Back%20to-Etape%201%20%3A%20Pr%C3%A9paration%20et%20planification-blue?style=social&logo=github)](/Installations/Etape1/0-index.md)
  
[![README](https://img.shields.io/badge/Back%20to-Master%20your%20network-blue?style=social&logo=github)](/README.md) 