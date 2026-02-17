# 💽 La virtualisation avec VMware Workstation

![Virtualisation](https://img.shields.io/badge/Virtualisation-VMware-white?style=flat-square&logo=vmware)

## 📝 Contexte
Avant de concevoir et déployer une infrastructure réseau complète, il est essentiel de disposer d'un **environnement de test flexible et isolé**.  
C'est dans ce cadre que la **virtualisation** entre en jeu : elle permet de **simuler une entreprise fictive** et de **reproduire une architecture réseau professionnelle** sur une seule machine physique.

Grâce à **VMware Workstation**, cahque composant du projet (serveur, poste client, routeur, ...) est virtualisé sous forme de **machine virtuelle indépendante**, facilitant ainsi :
- la **gestion centralisée** des différents systèmes dans une interface unique;
- la **création rapide de snapshots** pour tester et revenir en arrière facilement;
- la **mise en place d'un réseau virtuel complet** (LAN, DMZ, WAN) sans impacter le système hôte.

Cette approche offre un **laboratoire d'expérimentation idéal** pour la conception, la configuration et la maintenance d'une infrastructure simulée.

## 📦 Caractéristiques des VMs

| #  | Tag VM         | Hostname        | Réseaux VMWare                                  | vCPU | RAM      | Système (Go) / Data (Go)  | Système d’exploitation  |
| -- | -------------- | --------------- | ----------------------------------------------- | ---- | -------- | ------------------------- | ----------------------- |
| 01 | **ns-router**  | `GoGunHee`      | WAN (NAT), LAN (LAN Segment), DMZ (LAN Segment) | 2    | 1 Go     | Système : 8  / Data : 2   | **VyOS - Debian**       |
| 02 | **ns-lnx**     | `Tank`          | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2 Go     | Système : 20 / Data : 5   | **Debian Server Core**  |
| 03 | **ns-ad01**    | `SungJinwoo`    | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4 Go     | Système : 60 / Data : 20  | **Windows Server GUI**  |
| 04 | **ns-ad02**    | `YooJinho`      | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 3 Go     | Système : 40 / Data : 10  | **Windows Server Core** |
| 05 | **ns-user01**  | `Monarch`       | LAN (LAN Segment) – VLAN DSI Users              | 2    | 4 Go     | Système : 64 / Data : 20  | **Windows 11 Pro**      |
| 06 | **ns-user02**  | `Hunter`        | LAN (LAN Segment) – VLAN Users                  | 2    | 4 Go     | Système : 64 / Data : 20  | **Windows 11 Pro**      |
| 07 | **ns-wsus**    | `NormaSelner`   | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4 Go     | Système : 60 / Data : 120 | **Windows Server GUI**  |
| 08 | **ns-voip**    | `BaekYoonHo`    | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2 Go     | Système : 20 / Data : 10  | **3CX - Debian**        |
| 09 | **ns-audit01** | `Igris`         | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4 Go     | Système : 50 / Data : 10  | **Windows Server GUI**  |
| 10 | **ns-backup**  | `Beru`          | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4 Go     | Système : 30 / Data : 100 | **Debian Server Core**  |
| 11 | **ns-it**      | `Bellion`       | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 3 Go     | Système : 30 / Data : 20  | **Ubuntu Server Core**  |
| 12 | **ns-web**     | `EsilRadiru`    | DMZ (LAN Segment)                               | 2    | 1 Go     | Système : 20 / Data : 10  | **Ubuntu Server Core**  |
| 13 | **ns-vpn**     | `AdamWhite`     | DMZ (LAN Segment)                               | 2    | 2 Go     | Système : 15 / Data : 5   | **Debian Server Core**  |
| 14 | **ns-ntp**     | `Rulers`        | LAN (LAN Segment) – VLAN DSI Servers            | 1    | 1 Go     | Système : 8  / Data : 2   | **Debian Server Core**  |
| 15 | **ns-moni**    | `Kandiaru`      | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4 Go     | Système : 30 / Data : 30  | **Debian Server Core**  |
| 16 | **ns-safe**    | `Kamish`        | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 1 Go     | Système : 20 / Data : 5   | **Debian Server Core**  |
| 17 | **ns-logs**    | `AbsoluteBeing` | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4 Go     | Système : 30 / Data : 40  | **Ubuntu Server Core**  |
| 18 | **ns-mail**    | `Tusk`          | DMZ (LAN Segment)                               | 2    | 4 Go     | Système : 40 / Data : 40  | **Ubuntu Server Core**  |
| 19 | **ns-audit02** | `Kaisel`        | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2 Go     | Système : 30 / Data : 10  | **Kaisen Linux**        |


> La machine hôte utilisée pour ce projet repose sur **un processeur 10 cœurs / 16 threads** et **32 Go de mémoire**.  
> Bien que le total des ressources allouées aux machines virtuelles dépasse théoriquement ces valeurs, **toutes les VMs ne sont pas destinées à fonctionner simultanément**. Certaines, comme les serveurs de mise à jour ou d’audit, ne seront démarrées que ponctuellement.  
> Cette approche permet de **simuler une infrastructure complète** tout en **préservant la stabilité et les performances du système hôte**. Les ressources nécessaires à Kubuntu et à VMware Workstation sont également **prises en compte** dans la planification, garantissant ainsi un équilibre optimal entre réalisme et efficacité

## 🛰️ Création des VMs à chaque [étape de conception](/README.md#-les-étapes-de-conception)
Chaque étape de conception active uniquement les machines virtuelles nécessaires à sa réalisation.  
Cette approche séquentielle permet de **reproduire une montée en complexité réaliste**, tout en garantissant la **stabilité et les performances** de l’environnement de virtualisation sous VMware Workstation.  
Les listes ci-dessous détaillent, pour chaque phase, les **VMs en service**, celles à l’arrêt, ainsi que la **répartition des ressources** (vCPU et RAM) utilisées.

> Les ressources de la machine hôte (10 cœurs / 16 threads – 32 Go RAM) sont partagées avec les machines virtuelles.  
> 
> Le système hôte dispose d’un espace de swap de 16 Go, configuré comme une soupape de sécurité mémoire.
Cet espace n’est pas destiné à se substituer à la mémoire vive, mais à absorber les pics temporaires de consommation afin de préserver la stabilité du système hôte et des machines virtuelles, notamment lors des phases de démarrage, de snapshot ou de charges ponctuelles.  
>
> Une réserve fonctionnelle équivalente à 2 vCPU et 8 Go de RAM est implicitement préservée pour le système hôte (Kubuntu) et VMware Workstation, grâce à un dimensionnement maîtrisé des machines virtuelles et à une activation séquencée des services.

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

## 🌐 Simulation des VLANs sous VMware Workstation
Dans le projet collaboratif *Build Your Infrastructure*, l’environnement d’hypervision reposait sur **Proxmox VE**, qui permettait d’activer ou non le **mode trunk VLAN (802.1q)** directement au niveau des interfaces virtuelles.  

Dans **Master Your Network**, l’environnement repose sur **VMware Workstation**, qui **ne prend pas en charge nativement la gestion des VLANs 802.1q** au niveau de l’hyperviseur.  

Ainsi :
- Les **LAN Segments** utilisés dans VMware ne sont pas des ports “access” ou “trunk” : ils servent simplement de **liens Ethernet non filtrés** entre les VMs.  
- Le **trunk VLAN** est donc **entièrement simulé dans VyOS**, via des sous-interfaces (`vif`) correspondant à chaque VLAN logique :  
  - VLAN 10 → Users  
  - VLAN 20 → DSI Users  
  - VLAN 30 → DSI Servers  

Toutes les VMs internes (serveurs et clients) sont connectées au même **LAN Segment nommé “LAN”**, ce qui permet à VyOS de gérer :
- le **routage inter-VLAN**,  
- le **tagging/detagging VLAN**,  
- et la **segmentation logique** du réseau.  

⚙️ Cette approche reproduit fidèlement la logique d’un réseau d’entreprise, tout en restant **compatible avec les contraintes d’un laboratoire VMware Workstation**.  
Elle permet donc de simuler un **trunk VLAN réaliste**, même sans infrastructure d’hypervision avancée comme Proxmox ou ESXi.  

> 💡 **C’est quoi le mode trunk VLAN 802.1Q ?**  
> - Le **VLAN** (*Virtual Local Area Network*) est un **réseau logique** à l'intérieur d'un **réseau physique**. Il permet de **segmenter** un réseau local en plusieurs *sous-réseaux* sans ajouter de matériel supplémentaire.  
> - Chaque **VLAN** peut exister sur le même **switch physique**, mais ils ne peuvent **pas communiquer entre eux** sans passer par un **routeur**.  
> - Le **mode trunk** est un type de port de switch qui permet de transporter **plusieurs VLANs simultanément** sur **un seul lien physique**.  
>   👉 Contrairement au **mode access**, qui ne transporte **qu’un seul VLAN** à la fois.  
> - Pour que les équipements différencient les trames de chaque VLAN, le mode trunk utilise un système de **tag VLAN**, défini par la norme **IEEE 802.1Q**.  
>   Ce tag identifie chaque trame réseau selon son VLAN d’origine — c’est ce qu’on appelle le **VLAN tagging**.

> 💡 **C’est quoi une sous-interface `vif` ?**  
> - Le routeur **VyOS** possède une **interface physique `eth1`** pour le réseau LAN.  
> - Pour gérer plusieurs VLANs sur cette même interface, on crée des **sous-interfaces `vif` (Virtual Interface)**, chacune associée à un VLAN.  
> - Chaque sous-interface correspond à un VLAN distinct et transporte uniquement le trafic de ce VLAN.  
> - Leur nom suit la convention **`eth1.X`**, où **X** correspond au numéro du VLAN (par ex. `eth1.10`, `eth1.20`, `eth1.30`).  
> - Ces sous-interfaces permettent à VyOS de **faire du routage inter-VLAN** et de **simuler un véritable trunk VLAN** dans un environnement virtualisé.


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

🔹🧰 **Comment faire des snapshots sur VMware Workstation ?**

### 🖼️ Créer un snapshot
1. Démarrer la **VM cible**.  
2. Dans la barre de menu : `VM` → `Snapshot` → `Take Snapshot...`  
3. Une fenêtre s’ouvre :  
   - Donner un **nom** et une **description**.  
     > Exemple : `Avant_AD_Config` – VM avant la configuration de l’Active Directory.  
4. Cliquer sur **Take Snapshot**.  
5. Si la VM est allumée, VMware propose d’inclure **l’état de la mémoire** :  
   - ✅ **Oui** : la VM sera restaurée exactement dans son état actuel (fenêtres ouvertes, sessions connectées, etc.).  
   - 🚫 **Non** : seul l’état du disque est sauvegardé (la VM redémarrera proprement).  

### 🔁 Restaurer un snapshot
1. Sélectionner la **VM** dans VMware.  
2. Aller dans : `VM` → `Snapshot` → `Snapshot Manager...`  
3. Sélectionner le snapshot souhaité → cliquer sur **Go To** → confirmer.  

### 🧹 Supprimer un snapshot
1. Ouvrir le **Snapshot Manager**.  
2. Sélectionner le snapshot à supprimer.  
3. Cliquer sur **Delete**.  
   > ⚠️ “Delete” ne supprime pas les données : cela **fusionne les changements** dans le disque principal et **libère de l’espace**.  

> 💡 **Astuce :**  
> Il existe des **icônes raccourcis** (petit appareil photo 📸) dans la barre d’outils supérieure de VMware,  
> ce qui permet de **créer, restaurer ou supprimer un snapshot rapidement** sans passer par les menus.

---

[![STEP1](https://img.shields.io/badge/Back%20to-Etape%201%20%3A%20Pr%C3%A9paration%20et%20planification-blue?style=social&logo=github)](/Installations/Etape1/0-index.md)
  
[![README](https://img.shields.io/badge/Back%20to-Master%20your%20network-blue?style=social&logo=github)](/README.md) 