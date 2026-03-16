# 💽 La virtualisation avec VirtualBox

![Virtualisation](https://img.shields.io/badge/Virtualisation-VirtualBox-white?style=flat-square&logo=virtualbox)

## 📝 Contexte
Avant de concevoir et déployer une infrastructure réseau complète, il est essentiel de disposer d'un **environnement de test flexible et isolé**.  
C'est dans ce cadre que la **virtualisation** entre en jeu : elle permet de **simuler une entreprise fictive** et de **reproduire une architecture réseau professionnelle** sur une seule machine physique.

Grâce à **VirtualBox**, cahque composant du projet (serveur, poste client, routeur, ...) est virtualisé sous forme de **machine virtuelle indépendante**, facilitant ainsi :
- la **gestion centralisée** des différents systèmes dans une interface unique;
- la **création rapide de snapshots** pour tester et revenir en arrière facilement;
- la **mise en place d'un réseau virtuel complet** (LAN, DMZ, WAN) sans impacter le système hôte.

Cette approche offre un **laboratoire d'expérimentation idéal** pour la conception, la configuration et la maintenance d'une infrastructure simulée.

## 📦 Caractéristiques des VMs

| #  | Tag VM         | Hostname        | Réseaux VirtualBox                                    | vCPU | RAM      | Système (Go) / Data (Go)  | Système d’exploitation  |
| -- | -------------- | --------------- | ----------------------------------------------------- | ---- | -------- | ------------------------- | ----------------------- |
| 01 | **ns-router**  | `GoGunHee`      | WAN (NAT), LAN (Réseau interne), DMZ (Réseau interne) | 1    | 1 Go     | Système : 16 / Data : 0   | **pfSense - FreeBSD**   |
| 02 | **ns-lnx**     | `Tank`          | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 1 Go     | Système : 30 / Data : 0   | **Debian Server Core**  |
| 03 | **ns-ad01**    | `SungJinwoo`    | LAN (Réseau interne) – VLAN DSI Servers               | 2    | 4 Go     | Système : 50 / Data : 30  | **Windows Server GUI**  |
| 04 | **ns-ad02**    | `YooJinho`      | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 2 Go     | Système : 50 / Data : 30  | **Windows Server Core** |
| 05 | **ns-user01**  | `Monarch`       | LAN (Réseau interne) – VLAN DSI Users                 | 2    | 3 Go     | Système : 80 / Data : 0   | **Windows 11 Pro**      |
| 06 | **ns-user02**  | `Hunter`        | LAN (Réseau interne) – VLAN Users                     | 2    | 3 Go     | Système : 80 / Data : 0   | **Windows 11 Pro**      |
| 07 | **ns-wsus**    | `NormaSelner`   | LAN (Réseau interne) – VLAN DSI Servers               | 2    | 3 Go     | Système : 60 / Data : 120 | **Windows Server GUI**  |
| 08 | **ns-voip**    | `BaekYoonHo`    | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 1.5 Go   | Système : 30 / Data : 0   | **3CX - Debian**        |
| 09 | **ns-audit01** | `Igris`         | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 2 Go     | Système : 50 / Data : 0   | **Windows Server Core** |
| 10 | **ns-backup**  | `Beru`          | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 2 Go     | Système : 30 / Data : 100 | **Debian Server Core**  |
| 11 | **ns-it**      | `Bellion`       | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 2 Go     | Système : 30 / Data : 10  | **Ubuntu Server Core**  |
| 12 | **ns-web**     | `EsilRadiru`    | DMZ (Réseau interne)                                  | 1    | 1 Go     | Système : 30 / Data : 0   | **Ubuntu Server Core**  |
| 13 | **ns-vpn**     | `AdamWhite`     | DMZ (Réseau interne)                                  | 1    | 1 Go     | Système : 30 / Data : 0   | **Debian Server Core**  |
| 14 | **ns-ntp**     | `Rulers`        | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 512 Mo   | Système : 30 / Data : 0   | **Debian Server Core**  |
| 15 | **ns-moni**    | `Kandiaru`      | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 2 Go     | Système : 30 / Data : 20  | **Debian Server Core**  |
| 16 | **ns-safe**    | `Kamish`        | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 1 Go     | Système : 30 / Data : 0   | **Debian Server Core**  |
| 17 | **ns-logs**    | `AbsoluteBeing` | LAN (Réseau interne) – VLAN DSI Servers               | 2    | 3 Go     | Système : 30 / Data : 50  | **Ubuntu Server Core**  |
| 18 | **ns-mail**    | `Tusk`          | DMZ (Réseau interne)                                  | 2    | 3 Go     | Système : 30 / Data : 50  | **Ubuntu Server Core**  |
| 19 | **ns-audit02** | `Kaisel`        | LAN (Réseau interne) – VLAN DSI Servers               | 1    | 1 Go     | Système : 30 / Data : 0   | **Debian Server Core**  |


> La machine hôte utilisée pour ce projet repose sur **un processeur 10 cœurs / 16 threads** et **32 Go de mémoire**.  
> Bien que le total des ressources allouées aux machines virtuelles dépasse théoriquement ces valeurs, **toutes les VMs ne sont pas destinées à fonctionner simultanément**. Certaines, comme les serveurs de mise à jour ou d’audit, ne seront démarrées que ponctuellement.  
> Cette approche permet de **simuler une infrastructure complète** tout en **préservant la stabilité et les performances du système hôte**. Les ressources nécessaires à Windows 11 Professionnel et à VirtualBox sont également **prises en compte** dans la planification, garantissant ainsi un équilibre optimal entre réalisme et efficacité

## 🛰️ Création des VMs à chaque [étape de conception](/README.md#-les-étapes-de-conception)
Chaque étape de conception active uniquement les machines virtuelles nécessaires à sa réalisation.  
Cette approche séquentielle permet de **reproduire une montée en complexité réaliste**, tout en garantissant la **stabilité et les performances** de l’environnement de virtualisation sous VirtualBox.  
Les listes ci-dessous détaillent, pour chaque phase, les **VMs en service**, celles à l’arrêt, ainsi que la **répartition des ressources** (vCPU et RAM) utilisées.

> Les ressources de la machine hôte (10 cœurs / 16 threads – 32 Go RAM) sont partagées avec les machines virtuelles.  
> 
> Le système hôte dispose d’un espace de swap de 16 Go, configuré comme une soupape de sécurité mémoire.
Cet espace n’est pas destiné à se substituer à la mémoire vive, mais à absorber les pics temporaires de consommation afin de préserver la stabilité du système hôte et des machines virtuelles, notamment lors des phases de démarrage, de snapshot ou de charges ponctuelles.  
>
> Une réserve fonctionnelle équivalente à 2 vCPU et 8 Go de RAM est implicitement préservée pour le système hôte (Windows 11 Professionnel) et VirtualBox, grâce à un dimensionnement maîtrisé des machines virtuelles et à une activation séquencée des services.

### 🧩 Matrice d’activité des machines virtuelles selon les étapes de conception
> - 🟢 : La VM reste **allumée en continu** pendant toute l'étape.
> - 🔴 : La VM est **éteinte** pendant toute l'étape.
> - 🟠 : La VM est**utilisée ponctuellement** durant l'étape (*démarrée pour une action précise, puis éteinte afin d'économiser les ressources*).
>
> Étant donné que les étapes 1 et 9 ne sont des **étapes que de documentation**, aucune VM ne sera donc allumé.

| #  | Tag VM         | Hostname        | Étape 2 | Étape 3 | Étape 4 | Étape 5 | Étape 6 | Étape 7 | Étape 8 |
| -- | -------------- | --------------- | :-----: | :-----: | :-----: | :-----: | :-----: | :-----: | :-----: |
| 01 | **ns-router**  | `GoGunHee`      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      |
| 02 | **ns-lnx**     | `Tank`          | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      |
| 03 | **ns-ad01**    | `SungJinwoo`    | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      | 🟢      |
| 04 | **ns-ad02**    | `YooJinho`      | 🟢      | 🟢      | 🟠      | 🔴      | 🟠      | 🔴      | 🟠      |
| 05 | **ns-user01**  | `Monarch`       | 🔴      | 🟠      | 🟠      | 🟠      | 🟠      | 🟠      | 🟠      |
| 06 | **ns-user02**  | `Hunter`        | 🔴      | 🟠      | 🟠      | 🟠      | 🔴      | 🔴      | 🔴      |
| 07 | **ns-wsus**    | `NormaSelner`   | 🔴      | 🔴      | 🟠      | 🔴      | 🔴      | 🔴      | 🟠      |
| 08 | **ns-voip**    | `BaekYoonHo`    | 🔴      | 🔴      | 🟠      | 🔴      | 🔴      | 🔴      | 🔴      |
| 09 | **ns-audit01** | `Igris`         | 🔴      | 🔴      | 🟠      | 🔴      | 🔴      | 🔴      | 🟠      |
| 10 | **ns-backup**  | `Beru`          | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      | 🔴      | 🔴      |
| 11 | **ns-it**      | `Bellion`       | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      | 🟠      | 🔴      |
| 12 | **ns-web**     | `EsilRadiru`    | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      | 🟠      | 🔴      |
| 13 | **ns-vpn**     | `AdamWhite`     | 🔴      | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      | 🔴      |
| 14 | **ns-ntp**     | `Rulers`        | 🔴      | 🔴      | 🔴      | 🔴      | 🟢      | 🟢      | 🟢      |
| 15 | **ns-moni**    | `Kandiaru`      | 🔴      | 🔴      | 🔴      | 🔴      | 🟢      | 🟢      | 🟢      |
| 16 | **ns-safe**    | `Kamish`        | 🔴      | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      | 🔴      |
| 17 | **ns-logs**    | `AbsoluteBeing` | 🔴      | 🔴      | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      |
| 18 | **ns-mail**    | `Tusk`          | 🔴      | 🔴      | 🔴      | 🔴      | 🔴      | 🟠      | 🔴      |
| 19 | **ns-audit02** | `Kaisel`        | 🔴      | 🔴      | 🔴      | 🔴      | 🔴      | 🔴      | 🟠      |

## 🌐 Simulation des VLANs avec VirtualBox, GNS3 et pfSense
Dans le projet *Master Your Network*, l'environnement repose désormais sur un trio cohérent :
- **VirtualBox** pour l'exécution des machines virtuelles (serveurs, postes clients, etc.).
- **GNS3** pour la simulation réseau (switch, trunk, topologie).
- **pfSense** comme routeur/firewall et point central du routage inter-VLAN.

Contrairement à des hyperviseurs comme **Proxmox VE** ou **VMware ESXi**, **VirtualBox ne gère pas nativement les VLANs 802.1Q** au niveau des interfaces virtuelles.  
Les cartes réseau VirtualBox ne peuvent pas être configurées en mode *access* ou *trunk* : elles transportent simplement des trames Ethernet brutes.

Pour contourner cette limitation et obtenir un réseau réaliste, la gestion des VLANs est assurée par **GNS3**, qui simule les switchs.  
Les VMs VirtualBox sont **directement intégrées dans GNS3** via le menu `File` → `Import Appliance...` → `VirtualBox VM`, ce qui permet de les connecter aux ports du switch simulé comme de véritables équipements réseau.*

### 🧩 Fonctionnement global
L'architecture repose sur une séparation claire entre les rôles :

#### 🔵 GNS3 simule l’infrastructure résea
GNS3 joue le rôle de **plan de commutation** de l'entreprise :
- Switchs L2/L3 simulés.
- Ports configurable en **access** (VLAN spécifique) ou **trunk** (tous les VLANs).
- Transport des VLANs via **802.1Q tagging**.
- Connexion directe avec pfSense et les VMs VirtualBox.

Ainsi, GNS3 devient l'équivalent d'un **commutateur physique d'entreprise**, capable de transporter plusieurs VLANs sur un même lien.

#### 🔴 pfSense assure le routage inter-VLAN
pfSense est connecté au switch GNS3 via une interface en mode **trunk 802.1Q**. Sur pfSense, chaque VLAN est déclaré comme une **sous-interface virtuelle** :
- `VLAN 10` → Users
- `VLAN 20` → DSI Users
- `VLAN 30` → DSI Servers

pfSense gère alors :
- le **tagging/detagging** des trames.
- le **routage inter-VLAN** (communication entre les VLANs).
- les **règles de firewall** entre les segments.
- les **services réseau**.

#### 🟢 VirtualBox héberge les machines, mais ne gère pas les VLANs
Les VMs VirtualBox sont simplement reliées à GNS3 via une interface réseau “brute”. Elles ne voient **qu’un seul VLAN** : celui du **port access** auquel elles sont connectées dans GNS3.  

```
                         ┌───────────────────────────┐
                         │         Internet          │
                         └────────────┬──────────────┘
                                      │
                                    [WAN]
                                      │
                         ┌───────────────────────────────┐
                         │   eth0 = WAN (NAT)            │
                         │   ns-router (pfSense)         │
                         │   eth1 = LAN (Trunk 802.1Q)   │
                         └────────────┬──────────────────┘
                                      │
                         ┌───────────────────────────────────┐
                         │   Port 0 : Trunk 802.1Q (pfSense) |
                         │   Switch principal VLANs GNS3     |
                         |   Port 1 : (Access VLAN 10)       | ─────────────────► [ Switch VLAN 10 (Users)       ] ─────────────────► [VMs Users]
                         │   Port 2 : (Access VLAN 20)       | ─────────────────► [ Switch VLAN 20 (DSI Users)   ] ─────────────────► [VMs DSI Users]
                         │   Port 3 : (Access VLAN 30)       | ─────────────────► [ Switch VLAN 30 (DSI Servers) ] ─────────────────► [VMs DSI Servers]
                         └───────────────────────────────────┘
```

VirtualBox n'a donc **aucune configuration VLAN interne** : tout est géré par le switch GNS3 et pfSense.

Cette architecture permet de :
- reproduire fidèlement un réseau d'entreprise.
- séparer proprement les rôles,
- garder une topologie **visuelle**, **réaliste** et **documentée**.
- contourner les limites de VirtualBox sans perdre en qualité technologique.

### 💡 Rappel : qu’est-ce qu’un trunk VLAN 802.1Q 
 - Le **VLAN** (*Virtual Local Area Network*) est un **réseau logique** à l'intérieur d'un **réseau physique**. Il permet de **segmenter** un réseau local en plusieurs *sous-réseaux* sans ajouter de matériel supplémentaire.  
 - Chaque **VLAN** peut exister sur le même **switch physique**, mais ils ne peuvent **pas communiquer entre eux** sans passer par un **routeur**.  
 - Le **mode trunk** est un type de port de switch qui permet de transporter **plusieurs VLANs simultanément** sur **un seul lien physique**.  
   👉 Contrairement au **mode access**, qui ne transporte **qu’un seul VLAN** à la fois.  
 - Pour que les équipements différencient les trames de chaque VLAN, le mode trunk utilise un système de **tag VLAN**, défini par la norme **IEEE 802.1Q**.  
   Ce tag identifie chaque trame réseau selon son VLAN d’origine — c’est ce qu’on appelle le **VLAN tagging**.

### 💡 Rappel : qu’est-ce qu’une sous-interface `vif`
 - Le routeur **pfSense** possède une **interface physique `eth1`** pour le réseau LAN.  
 - Pour gérer plusieurs VLANs sur cette même interface, on crée des **sous-interfaces `vif` (Virtual Interface)**, chacune associée à un VLAN.  
 - Chaque sous-interface correspond à un VLAN distinct et transporte uniquement le trafic de ce VLAN.  
 - Leur nom suit la convention **`eth1.X`**, où **X** correspond au numéro du VLAN (par ex. `eth1.10`, `eth1.20`, `eth1.30`).  
 - Ces sous-interfaces permettent à pfSense de **faire du routage inter-VLAN** et de **simuler un véritable trunk VLAN** dans un environnement virtualisé.

## 🧷 Les snapshots

🔹🧠 **À quoi servent les snapshots et pourquoi c’est utile ?**  
- Un **snapshot** est une **photo instantanée de l’état d’une machine virtuelle** à un moment donné.  
- Cela permet de :  
  - 🔙 Revenir en arrière après une erreur de configuration ou une mauvaise manipulation.  
  - 🧪 Tester des changements sans risques.  
  - 🧾 Documenter les étapes d’un projet.  
  - 💾 Sauvegarder une VM avant une mise à jour critique.  
  - 🧩 Travailler par version (comme un “Git” de VM).  
- ⚠️ Trop de snapshots peuvent **ralentir la VM** et **occuper beaucoup d’espace disque**, il est donc conseillé de **faire le ménage régulièrement**.

🔹🧰 **Comment faire des snapshots sur VirtualBox ?**

### 🖼️ Créer un snapshot
1. Démarrer la **VM cible**.  
2. Dans la barre de menu : `VM` → `Snapshot` → `Take Snapshot...`  
3. Une fenêtre s’ouvre :  
   - Donner un **nom** et une **description**.  
     > Exemple : `Avant_AD_Config` – VM avant la configuration de l’Active Directory.  
4. Cliquer sur **Take Snapshot**.  
5. Si la VM est allumée, VirtualBox propose d’inclure **l’état de la mémoire** :  
   - ✅ **Oui** : la VM sera restaurée exactement dans son état actuel (fenêtres ouvertes, sessions connectées, etc.).  
   - 🚫 **Non** : seul l’état du disque est sauvegardé (la VM redémarrera proprement).  

### 🔁 Restaurer un snapshot
1. Sélectionner la **VM** dans VirtualBox.  
2. Aller dans : `VM` → `Snapshot` → `Snapshot Manager...`  
3. Sélectionner le snapshot souhaité → cliquer sur **Go To** → confirmer.  

### 🧹 Supprimer un snapshot
1. Ouvrir le **Snapshot Manager**.  
2. Sélectionner le snapshot à supprimer.  
3. Cliquer sur **Delete**.  
   > ⚠️ “Delete” ne supprime pas les données : cela **fusionne les changements** dans le disque principal et **libère de l’espace**.  

> 💡 **Astuce :**  
> Il existe des **icônes raccourcis** (petit appareil photo 📸) dans la barre d’outils supérieure de VirtualBox,  
> ce qui permet de **créer, restaurer ou supprimer un snapshot rapidement** sans passer par les menus.

---

[![STEP1](https://img.shields.io/badge/Back%20to-Etape%201%20%3A%20Pr%C3%A9paration%20et%20planification-blue?style=social&logo=github)](/Installations/Etape1/0-index.md)
  
[![README](https://img.shields.io/badge/Back%20to-Master%20your%20network-blue?style=social&logo=github)](/README.md) 