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

## Inventaire des équipements par zone
### 🔐 Routeur / Pare-feu central

| Équipement                       | Fonction                                                            | Remarques                                                                    |
| -------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 🛜 **Routeur / Pare-feu (VyOS)** | Point central d’interconnexion et de sécurité entre WAN, DMZ et LAN | Assure le routage inter-zones, le NAT, le VPN et le filtrage des flux réseau |

### 🧱 DMZ (zone démilitarisée)
> Les services hébergés dans la DMZ sont isolés du LAN.  
Seuls les flux strictement nécessaires (HTTPS, SMTP, VPN) sont autorisés via le pare-feu VyOS.

| Service                                  | Fonction                                                                        | Remarques                                                         |
| ---------------------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| 🔑 **Serveur VPN (OpenVPN)**             | Permet un accès distant sécurisé aux administrateurs et collaborateurs externes | Accès contrôlé vers le LAN (authentification + filtrage pare-feu) |
| 📧 **Serveur de messagerie (iRedMail)**  | Gère les mails internes et la réception depuis l’extérieur                      | Filtrage anti-spam et relais SMTP sécurisé                        |
| 🌍☁️ **Serveur Web Externe**             | Portail externe pour les clients et partenaires (Nginx)                         | HTTPS uniquement (443), proxifié si besoin                        |
|                                          | Espace de partage pour clients et partenaires (Nextcloud)                       | Accès HTTPS, synchronisation restreinte vers le Cloud interne     |

## 🏠 LAN (réseau interne d’entreprise)
🔹 VLAN DSI Servers
>Serveurs critiques et infrastructure centrale

| Service                                                  | Fonction                                                                   |
| -------------------------------------------------------- | -------------------------------------------------------------------------- |
| 📦 **Serveur Linux (DHCP, DNS)**                         | Attribution des adresses IP et résolution des noms                         |
| 🪟 **Serveur Windows (AD-DS, GPO, SMB)**                 | Gestion des comptes, des stratégies de groupe et des fichiers partagés SMB |
| 🛰️ **Serveur Updates (WSUS)**                            | Gestion des mises à jour Windows pour les PC Clients & Administrateurs    |
| 📊 **Serveur de supervision (Zabbix)**                   | Surveillance de l’état du réseau et des serveurs                           |
| 📜 **Serveur de journalisation (Syslog et LogAnalyzer)** | Centralisation des logs (pare-feu, serveurs, postes)                       |
| 💾 **Serveur de stockage et sauvegarde (Bareos / NAS)**  | Sauvegarde RAID et restauration des données critiques                      |
| 🌍🧰 **Serveur IT (Intranet : Apache & GLPI)**           | Portail interne (documentation, applications internes)                     |
|                                                          | Gestion du parc informatique, des incidents et des tickets IT              |
| 📡 **Serveur NTP (Chrony)**                              | Synchronisation horaire sur tout le réseau                                 |
| 📞 **Serveur VoIP (3CX)**                                | Téléphonie IP interne                                                      |
| 🔒 **Serveur de mot de passe (Vaultwarden)**             | Stockage sécurisé des identifiants et accès administratifs                 |
| 🧪 **Serveur d’audit / administration**                  | Tests, vérifications, maintenance et scripts d’automatisation              |

🔹 VLAN Users
> Postes utilisateurs standards (employés)

| Poste                                     | Fonction                                                |
| ----------------------------------------- | ------------------------------------------------------- |
| 💻 **Postes utilisateurs Windows 11 Pro** | Machines du personnel, jointes au domaine AD            |
| 🌐 **Accès restreint aux services**       | Accès aux partages, messagerie, intranet, cloud interne |

🔹 VLAN DSI Users
> Postes d’administration et de maintenance (équipe Infrastructure & IT)

| Poste                                                                 | Fonction                                                                                                         |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| 🖥️ **PC administrateurs (Windows 11 Pro)**                            | Postes de travail de l’équipe DSI,                                                                               |
|                                                                       | Intégrés au domaine Active Directory et préconfigurés avec des machines virtuelles Ubuntu / Kaisen / Kali        |
|                                                                       | Pour la supervision, la configuration et les tests d’administration système et réseau                            |
| 🔧 **Outils d’exploitation (AnyDesk, scripts, SSH, Hyper-V, VMware)** | Utilisés pour la maintenance à distance, l’automatisation et le support technique des serveurs et postes clients |

## 🔠 Convention de nommage

### 🖥️ Nomenclature des VM

| #  | 🏷️ Tag VM        | 🖥️ Hostname    | ⚙️ Fonctions & rôles principaux                                          |
| -- | ---------------: | :------------: | ------------------------------------------------------------------------ |
|  1 | **ns-fw01**      | `igris`        | Routeur, Pare-feu, VLANs                                                 |
|  2 | **ns-lnx01**     | `tusk`         | DHCP, DNS                                                                |
|  3 | **ns-ad01**      | `sungjinwoo`   | Domain Controler, Active Directory, GPO, SMB                             |
|  4 | **ns-wsus01**    | `woojinchul`   | Mises à jour Windows Updates WSUS pour les PC clients & Administrateurs  |
|  5 | **ns-bkp01**     | `beru`         | Bareos Director/Storage, dépôt NAS/RAID logiciel                         |
|  6 | **ns-log01**     | `iron`         | LogAnalyzer (web), Syslog                                                |
|  7 | **ns-secrets01** | `kamish`       | Vaultwarden (coffre identifiants admin)                                  |
|  8 | **ns-it01**      | `bellion`      | GLPI, Intranet (Apache)                                                  |
|  9 | **ns-mon01**     | `baran`        | Serveur de supervision Zabbix                                            |
| 10 | **ns-ntp01**     | `sillad`       | Serveur de temps Chrony                                                  |
| 11 | **ns-voip01**    | `tank`         | 3CX (SIP/RTP), trunks opérateur                                          |
| 12 | **ns-audit01**   | `greed`        | nsaudit de sécurité des différents serveurs                              |
| 13 | **ns-admin01**   | `shadow-admin` | Poste administrateur                                                     |
| 14 | **ns-user01**    | `hunter`       | Poste utilisateur type joint au domaine                                  |
| 15 | **ns-web01**     | `kaisel`       | Nginx RP (Extranet), Nextcloud (externe)                                 |
| 16 | **ns-vpn01**     | `rakan`        | Serveur de connexion à distance OpenVPN                                  |
| 17 | **ns-mail01**    | `querehsha`    | Serveur de messagerie iRedMail                                           |

> Les hôtes du système **NovaPlay Studio** utilisent des noms inspirés du manhwa *Solo Leveling*.  
> Ce choix symbolique permet d’attribuer à chaque machine une identité cohérente avec son rôle au sein de l’infrastructure : chaque personnage ou ombre représente une fonction clé, une force ou une responsabilité technique.

🔹**Pourquoi ce nommage pour les `Hostname` ?**  
Le choix des **noms d’hôtes** est directement inspiré de l’univers du manhwa *Solo Leveling*, où chaque personnage, ombre ou monarque incarne un rôle bien défini au sein d’une hiérarchie de puissance et de responsabilités.  
De la même manière, chaque serveur de l’infrastructure **NovaPlay Studio** se voit attribuer un nom reflétant sa **fonction technique**, son **importance dans le réseau** et son **degré de criticité**.

Cette approche a trois objectifs :
- 🎯 **Donner du sens aux machines** : chaque nom évoque immédiatement le rôle du serveur (ex. igris → protecteur du domaine, comme un pare-feu).
- 🧠 **Créer une cohérence thématique** : le thème des ombres et des monarques renforce la logique d’une architecture hiérarchisée et maîtrisée.
- 💼 **Allier technique et identité** : le réseau devient un écosystème vivant, où chaque composant a sa personnalité et sa mission.

| Exemple          | Référence Solo Leveling                  | Symbolique réseau                               |
| :--------------- | :--------------------------------------- | :---------------------------------------------- |
| **`igris`**      | Ombre loyale et chevalier protecteur     | Pare-feu, défense du périmètre réseau           |
| **`tusk`**       | Mage orc, stratège puissant              | DNS/DHCP, intelligence et coordination          |
| **`sungjinwoo`** | Protagoniste, Monarque des Ombres        | Contrôleur de domaine, cœur de l’infrastructure |
| **`woojinchul`** | Chef du département de surveillance      | WSUS, supervision des mises à jour              |
| **`beru`**       | Commandant des ombres, fidèle exécutant  | Sauvegarde et stockage                          |
| **`iron`**       | Ombre puissante et robuste               | Collecte et archivage de logs                   |
| **`kamish`**     | Dragon légendaire, rare et précieux      | Coffre-fort des identifiants administratifs     |
| **`bellion`**    | Premier général d’Ashborn                | Serveur central DSI (GLPI, intranet)            |
| **`baran`**      | Monarque des flammes blanches            | Supervision (Zabbix), vigilance constante       |
| **`sillad`**     | Monarque du givre                        | Stabilité et régularité du temps (NTP)          |
| **`kaisel`**     | Wyvern du roi des ombres                 | Reverse Proxy, lien entre interne et externe    |
| **`rakan`**      | Monarque des crocs, roi des bêtes        | VPN, gardien des connexions sécurisées          |
| **`querehsha`**  | Reine des insectes, souveraine du réseau | Messagerie, communication et diffusion          |

> 💬 Ainsi, la nomenclature ne se limite pas à une convention technique : elle raconte une histoire cohérente entre la sécurité, la hiérarchie et la maîtrise du réseau, à l’image du royaume des ombres dans Solo Leveling.

## ⚙️ Priorités des équipements
### 🔺 Priorité haute
> Services essentiels au fonctionnement, à la sécurité et à l’identité du réseau.
> Leur indisponibilité provoque une panne globale ou un arrêt du SI.

1. 🧱 **Routeur / Pare-feu (VyOS)** — point d’interconnexion WAN / DMZ / LAN, sécurité réseau, NAT, filtrage
2. 📦 **Serveur Linux (DHCP, DNS)** — attribution d’adresses IP et résolution des noms (dépendance pour tous les postes)
3. 🪟 **Serveur Windows (AD-DS, GPO, WSUS, partage)** — authentification centralisée et gestion du domaine
4. 📡 **Serveur NTP (Chrony)** — synchronisation horaire (important pour cohérence, mais tolère une panne temporaire)
5. 💾 **Serveur de stockage et sauvegarde (Bareos / NAS)** — protection et restauration des données critiques
6. 📜 **Serveur de journalisation (Rsyslog & LogAnalyzer)** — centralisation des logs, indispensable pour diagnostic et sécurité
7. 🔒 **Serveur de mot de passe (Vaultwarden)** — sécurité des identifiants administratifs (accès à l’infrastructure)

💡 Ces services constituent la “colonne vertébrale” du réseau NovaStudios.

### 🟠 Priorité moyenne
> Services d’administration, de communication et de production.  
> Leur panne n’empêche pas le fonctionnement de base, mais dégrade fortement l’efficacité du SI.

1. 🧰 **Serveur GLPI** — gestion des incidents et du parc informatique
2. 🧪 **Serveur d’audit / administration** — maintenance, scripts et vérifications régulières
3. 🔑 **Serveur VPN (OpenVPN)** — accès distant pour les administrateurs ou collaborateurs
4. ☁️ **Serveur Web Externe (Nextcloud & Extranet Nginx)** — échanges de fichiers avec l’extérieur & portail externe clients/partenaires
5. 📧 **Serveur de messagerie (iRedMail)** — communication interne/externe (important mais non vital au cœur du réseau)
6. 🌍 **Serveur Web Intranet (Apache)** — portail interne et documentation
7. 📞 **Serveur VoIP (3CX)** — téléphonie interne (confort utilisateur, non critique)
8. 📊 **Serveur de supervision (Zabbix)** — détection des pannes et surveillance des services vitaux

💡 Ces services soutiennent l’activité, la collaboration et la supervision du réseau sans en être vitaux.

## 🟢 Priorité basse
> Éléments périphériques sans impact direct sur la disponibilité du SI.

1. 🖥️ **PC administrateur (Windows 11 Pro)** — postes d’administration de la DSI, équipés d’outils de gestion, supervision et maintenance (VM Ubuntu / Kaisen / Kali selon le rôle)
2. 💻 **Postes utilisateurs (Windows 11 Pro)** — postes de travail standard, non critiques pour l’infrastructure

💡 Les postes administrateurs conservent la base Windows 11 Pro pour l’intégration au domaine, tout en embarquant un environnement Linux virtualisé pour les tâches techniques.

## 🕵️ Serveur Bastion (sécurité d’administration)
Le **serveur Bastion** est un équipement de sécurité permettant de **centraliser, tracer et contrôler** les connexions d'administration vers les serveurs internes et ceux situés en DMZ. Il agit comme un **point d'accès unique pour les administrateurs**, en enregistrant leurs connexions et en limitant les accès directs au reste du réseau.

### 📘 Utilité
- Garantir une traçabilité complète des connexions d’administration
- Renforcer la sécurité en isolant le plan de gestion du réseau
- Limiter les risques d’accès non autorisés ou de compromission d’un poste administrateur

### ⚙️ Emplacement théorique 
Dans une infrastructure réelle, il serait positionné dans le **VLAN DSI Servers (LAN)**, avec un accès restreint depuis les **postes DSI Users**, et un accès autorisé vers les **serveurs LAN et DMZ** via le pare-feu VyOS.

### 🔁 Analogie : le Collège
Le **serveur Bastion** est l'équivalent du **surveillant** dans un collège, il surveille les entrées, les sorties et les actions des intervenants en notant tout dans son calepin. Le jour d'une inspection (par exemple un audit de sécurité), il ressort son calepin pour dire qui a fait quoi et à quelle heure sur tel jour.  
Ainsi, le Bastion n’accorde pas d’accès, mais observe et journalise toutes les actions d’administration au sein du réseau interne.

Contrairement au **serveur OpenVPN** qui est l'équivalent du **portail de sécurité** du collège, il ne laisse passer que ceux qui ont reçu une autorisation de passage sinon il refuse l'accès.

### 🧩 Remarque 
Ce composant n’est pas déployé dans le cadre du laboratoire, car il nécessite des compétences avancées en sécurisation d’accès privilégiés (PAM) et une infrastructure plus conséquente en ressources.
Cependant, sa présence est évoquée pour montrer la compréhension du concept et de son importance dans une architecture d’entreprise sécurisée.

---

👉 Retour à la [page index de l'étape](/Installations/Etape1/0-index.md).  
👉 Retour à la [page principale du projet](/README.md).  