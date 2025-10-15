# 💽 La virtualisation avec VMware Workstation
## 📝 Contexte
Avant de concevoir et déployer une infrastructure réseau complète, il est essentiel de disposer d'un **environnement de test flexible et isolé**.  
C'est dans ce cadre que la **virtualisation** entre en jeu : elle permet de **simuler une entreprise fictive** et de **reproduire une architecture réseau professionnelle** sur une seule machine physique.

Grâce à **VMware Workstation**, cahque composant du projet (serveur, poste client, routeur, ...) est virtualisé sous forme de **machine virtuelle indépendante**, facilitant ainsi :
- la **gestion centralisée** des différents systèmes dans une interface unique;
- la **création rapide de snapshots** pour tester et revenir en arrière facilement;
- la **mise en place d'un réseau virtuel complet** (LAN, DMZ, WAN) sans impacter le système hôte.

Cette approche offre un **laboratoire d'expérimentation idéal** pour la conception, la configuration et la maintenance d'une infrastructure simulée.

## 📦 Caractéristiques des VMs

| #  | Tag VM         | Hostname        | Réseaux VMWare                                  | vCPU | RAM (Go) | Système (Go) / Data (Go)  | Système d’exploitation  |
| -- | -------------- | --------------- | ----------------------------------------------- | ---- | -------- | ------------------------- | ----------------------- |
| 01 | **ns-router**  | `GoGunHee`      | WAN (NAT), LAN (LAN Segment), DMZ (LAN Segment) | 2    | 1        | Système : 8  / Data : 2   | **VyOS - Debian**       |
| 02 | **ns-lnx**     | `Tank`          | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2        | Système : 20 / Data : 5   | **Debian Server Core**  |
| 03 | **ns-ad01**    | `SungJinwoo`    | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4        | Système : 60 / Data : 20  | **Windows Server GUI**  |
| 04 | **ns-ad02**    | `YooJinho`      | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 3        | Système : 40 / Data : 10  | **Windows Server Core** |
| 05 | **ns-user01**  | `Monarch`       | LAN (LAN Segment) – VLAN DSI Users              | 2    | 4        | Système : 64 / Data : 20  | **Windows 11 Pro**      |
| 06 | **ns-user02**  | `Hunter`        | LAN (LAN Segment) – VLAN Users                  | 2    | 4        | Système : 64 / Data : 20  | **Windows 11 Pro**      |
| 07 | **ns-wsus**    | `NormaSelner`   | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4        | Système : 60 / Data : 120 | **Windows Server GUI**  |
| 08 | **ns-voip**    | `BaekYoonHo`    | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2        | Système : 20 / Data : 10  | **3CX - Debian**        |
| 09 | **ns-audit01** | `Igris`         | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4        | Système : 50 / Data : 10  | **Windows Server GUI**  |
| 10 | **ns-backup**  | `Beru`          | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4        | Système : 30 / Data : 100 | **Debian Server Core**  |
| 11 | **ns-it**      | `Bellion`       | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 3        | Système : 30 / Data : 20  | **Ubuntu Server Core**  |
| 12 | **ns-web**     | `EsilRadiru`    | DMZ (LAN Segment)                               | 2    | 2        | Système : 20 / Data : 10  | **Ubuntu Server Core**  |
| 13 | **ns-vpn**     | `AdamWhite`     | DMZ (LAN Segment)                               | 2    | 1.5      | Système : 15 / Data : 5   | **Debian Server Core**  |
| 14 | **ns-ntp**     | `Rulers`        | LAN (LAN Segment) – VLAN DSI Servers            | 1    | 1        | Système : 8  / Data : 2   | **Debian Server Core**  |
| 15 | **ns-moni**    | `Kandiaru`      | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4        | Système : 30 / Data : 30  | **Debian Server Core**  |
| 16 | **ns-safe**    | `Kamish`        | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2        | Système : 20 / Data : 5   | **Debian Server Core**  |
| 17 | **ns-logs**    | `AbsoluteBeing` | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 4        | Système : 30 / Data : 40  | **Ubuntu Server Core**  |
| 18 | **ns-mail**    | `Tusk`          | DMZ (LAN Segment)                               | 2    | 4        | Système : 40 / Data : 40  | **Ubuntu Server Core**  |
| 19 | **ns-audit02** | `Kaisel`        | LAN (LAN Segment) – VLAN DSI Servers            | 2    | 2        | Système : 30 / Data : 10  | **Kaisen Linux**        |


> La machine hôte utilisée pour ce projet repose sur **un processeur 8 cœurs / 16 threads** et **64 Go de mémoire DDR4**.  
> Bien que le total des ressources allouées aux machines virtuelles dépasse théoriquement ces valeurs, **toutes les VMs ne sont pas destinées à fonctionner simultanément**. Certaines, comme les serveurs de mise à jour ou d’audit, ne seront démarrées que ponctuellement.  
> Cette approche permet de **simuler une infrastructure complète** tout en **préservant la stabilité et les performances du système hôte**. Les ressources nécessaires à Kubuntu et à VMware Workstation sont également **prises en compte** dans la planification, garantissant ainsi un équilibre optimal entre réalisme et efficacité

## 🛰️ Création des VMs à chaque [étape de conception](/README.md#-les-étapes-de-conception)
Chaque étape de conception active uniquement les machines virtuelles nécessaires à sa réalisation.  
Cette approche séquentielle permet de **reproduire une montée en complexité réaliste**, tout en garantissant la **stabilité et les performances** de l’environnement de virtualisation sous VMware Workstation.  
Les listes ci-dessous détaillent, pour chaque phase, les **VMs en service**, celles à l’arrêt, ainsi que la **répartition des ressources** (vCPU et RAM) utilisées.

> Les ressources de la machine hôte (8 cœurs / 16 threads – 64 Go RAM DDR4) sont partagées avec les machines virtuelles.  
> Une réserve fixe de **2 vCPU** et **8 Go RAM** est conservée pour **Kubuntu** et **VMware Workstation** afin de garantir la stabilité.

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

## 🖥️ Création d'une machine virtuelle VMware Workstation