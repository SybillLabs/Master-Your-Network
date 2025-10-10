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

| #  | 🏷️ Tag VM       | 🌐 Réseaux VMware                                       | ⚙️ vCPU | 🧠 RAM | 💾 Disque & stockage           | 💻 Système d’exploitation                      |
| -- | ---------------- | ------------------------------------------------------- | ------- | ------ | ------------------------------ | ---------------------------------------------- |
| 1  | **ns-fw01**      | WAN (NAT), LAN (LAN Segment), DMZ (LAN Segment)         | 2       | 4 Go   | Système : 20 Go / Data : —     | **VyOS** (routeur / pare-feu)                  |
| 2  | **ns-lnx01**     | LAN (LAN Segment) – VLAN DSI Servers                    | 2       | 4 Go   | Système : 20 Go / Data : 10 Go | **Debian 12** (DHCP, DNS)                      |
| 3  | **ns-ad01**      | LAN (LAN Segment) – VLAN DSI Servers                    | 2       | 8 Go   | Système : 40 Go / Data : 20 Go | **Windows Server 2022**                        |
| 4  | **ns-wsus01**    | LAN (LAN Segment) – VLAN DSI Servers                    | 2       | 6 Go   | Système : 40 Go / Data : 40 Go | **Windows Server 2022**                        |
| 5  | **ns-bkp01**     | LAN (LAN Segment) – VLAN DSI Servers, DMZ (LAN Segment) | 2       | 6 Go   | Système : 30 Go / Data : 70 Go | **Debian 12** (Bareos NAS)                     |
| 6  | **ns-log01**     | LAN (LAN Segment) – VLAN DSI Servers                    | 1       | 3 Go   | Système : 20 Go / Data : 10 Go | **Ubuntu Server 24.04** (LogAnalyzer / Syslog) |
| 7  | **ns-secrets01** | LAN (LAN Segment) – VLAN DSI Servers                    | 1       | 2 Go   | Système : 15 Go / Data : 5 Go  | **Debian 12** (Vaultwarden)                    |
| 8  | **ns-it01**      | LAN (LAN Segment) – VLAN DSI Servers                    | 2       | 4 Go   | Système : 25 Go / Data : 15 Go | **Ubuntu Server 24.04** (GLPI / Apache)        |
| 9  | **ns-mon01**     | LAN (LAN Segment) – VLAN DSI Servers                    | 2       | 6 Go   | Système : 30 Go / Data : 10 Go | **Debian 12** (Zabbix)                         |
| 10 | **ns-ntp01**     | LAN (LAN Segment) – VLAN DSI Servers                    | 1       | 1 Go   | Système : 10 Go / Data : —     | **Debian 12** (Chrony)                         |
| 11 | **ns-voip01**    | LAN (LAN Segment) – VLAN DSI Servers, DMZ (LAN Segment) | 2       | 4 Go   | Système : 25 Go / Data : 15 Go | **Ubuntu Server 24.04** (3CX / Asterisk)       |
| 12 | **ns-audit01**   | LAN (LAN Segment) – VLAN DSI Servers                    | 2       | 4 Go   | Système : 30 Go / Data : 10 Go | **Kaisen Linux (Rolling)**                     |
| 13 | **ns-admin01**   | LAN (LAN Segment) – VLAN DSI Users                      | 2       | 4 Go   | Système : 40 Go / Data : —     | **Windows 10 Pro**                             |
| 14 | **ns-user01**    | LAN (LAN Segment) – VLAN Users                          | 2       | 4 Go   | Système : 40 Go / Data : —     | **Windows 10 Pro**                             |
| 15 | **ns-web01**     | DMZ (LAN Segment), LAN (LAN Segment) – VLAN DSI Servers | 2       | 4 Go   | Système : 25 Go / Data : 25 Go | **Ubuntu Server 24.04** (Nginx / Nextcloud)    |
| 16 | **ns-vpn01**     | DMZ (LAN Segment), LAN (LAN Segment) – VLAN DSI Servers | 1       | 3 Go   | Système : 15 Go / Data : 5 Go  | **Debian 12** (OpenVPN)                        |
| 17 | **ns-mail01**    | DMZ (LAN Segment), LAN (LAN Segment) – VLAN DSI Servers | 2       | 6 Go   | Système : 40 Go / Data : 40 Go | **Ubuntu Server 24.04** (iRedMail)             |

> 💡 La machine hôte utilisée pour ce projet repose sur **un processeur 8 cœurs / 16 threads** et **64 Go de mémoire DDR4**.  
> Bien que le total des ressources allouées aux machines virtuelles dépasse théoriquement ces valeurs, **toutes les VMs ne sont pas destinées à fonctionner simultanément**. Certaines, comme les serveurs de mise à jour ou d’audit, ne seront démarrées que ponctuellement.  
> Cette approche permet de **simuler une infrastructure complète** tout en **préservant la stabilité et les performances du système hôte**. Les ressources nécessaires à Kubuntu et à VMware Workstation sont également **prises en compte** dans la planification, garantissant ainsi un équilibre optimal entre réalisme et efficacité.

 ## 🛰️ Création des VMs à chaque [étape de conception](/README.md#-les-étapes-de-conception)

Chaque étape de conception active uniquement les machines virtuelles nécessaires à sa réalisation.  
Cette approche séquentielle permet de **reproduire une montée en complexité réaliste**, tout en garantissant la **stabilité et les performances** de l’environnement de virtualisation sous VMware Workstation.  
Les listes ci-dessous détaillent, pour chaque phase, les **VMs en service**, celles à l’arrêt, ainsi que la **répartition des ressources** (vCPU et RAM) utilisées.

> 💡 Les ressources de la machine hôte (8 cœurs / 16 threads – 64 Go RAM DDR4) sont partagées avec les machines virtuelles.  
> Une réserve fixe de **2 vCPU** et **8 Go RAM** est conservée pour **Kubuntu** et **VMware Workstation** afin de garantir la stabilité.

### 🗂️ Étape 1 – Préparation & planification
> 📄 Aucune machine virtuelle n’est nécessaire à cette étape : il s’agit d’une phase purement documentaire (analyse, plan réseau, inventaire, etc.).

- 🟢 **VMs ON :** *Aucune*  
- 🔴 **VMs OFF :** `ns-fw01`, `ns-lnx01`, `ns-ad01`, `ns-wsus01`, `ns-bkp01`, `ns-log01`, `ns-secrets01`, `ns-it01`, `ns-mon01`, `ns-ntp01`, `ns-voip01`, `ns-audit01`, `ns-admin01`, `ns-user01`, `ns-web01`, `ns-vpn01`, `ns-mail01`  

**Utilisation totale :** 0 vCPU / 0 Go  
**Ressources restantes (VMs dispo)** : 14 vCPU / 56 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  

### 🌐 Étape 2 – Réseau & sécurité
> 🧱 Mise en place de l’environnement réseau : routage, VLANs, DMZ et synchronisation NTP.

- 🟢 **VMs ON :** `ns-fw01`, `ns-lnx01`, `ns-ntp01`, `ns-admin01`  
- 🔴 **VMs OFF :** `ns-ad01`, `ns-wsus01`, `ns-bkp01`, `ns-log01`, `ns-secrets01`, `ns-it01`, `ns-mon01`, `ns-voip01`, `ns-audit01`, `ns-user01`, `ns-web01`, `ns-vpn01`, `ns-mail01`  

**Utilisation totale :** 7 vCPU / 13 Go  
**Ressources restantes :** 7 vCPU / 43 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  

### 🖥️ Étape 3 – Services d’infrastructure de base
> 🧾 Déploiement des services fondamentaux : DHCP/DNS, AD/DC, WSUS et postes clients.

- 🟢 **VMs ON :** `ns-fw01`, `ns-lnx01`, `ns-ad01`, `ns-wsus01`, `ns-admin01`, `ns-user01`  
- 🔴 **VMs OFF :** `ns-bkp01`, `ns-log01`, `ns-secrets01`, `ns-it01`, `ns-mon01`, `ns-ntp01`, `ns-voip01`, `ns-audit01`, `ns-web01`, `ns-vpn01`, `ns-mail01`  

**Utilisation totale :** 12 vCPU / 30 Go  
**Ressources restantes :** 2 vCPU / 26 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  

### 🔒 Étape 4 – Sécurité & supervision
> 🧠 Installation des services de sécurité et de supervision : Vaultwarden, Syslog, Zabbix, Audit et VPN.

- 🟢 **VMs ON :** `ns-fw01`, `ns-lnx01`, `ns-vpn01`, `ns-log01`, `ns-mon01`, `ns-secrets01`, `ns-audit01`  
- 🔴 **VMs OFF :** `ns-ad01`, `ns-wsus01`, `ns-bkp01`, `ns-it01`, `ns-ntp01`, `ns-voip01`, `ns-admin01`, `ns-user01`, `ns-web01`, `ns-mail01`  

**Utilisation totale :** 11 vCPU / 26 Go  
**Ressources restantes :** 3 vCPU / 30 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  

### 🧰 Étape 5 – Services applicatifs & utilisateurs
> 🌐 Mise en ligne des services métiers : Web, Mail, VoIP, GLPI et postes clients.

- 🟢 **VMs ON :** `ns-fw01`, `ns-lnx01`, `ns-web01`, `ns-mail01`, `ns-voip01`, `ns-admin01`, `ns-user01`  
- 🔴 **VMs OFF :** `ns-ad01`, `ns-wsus01`, `ns-bkp01`, `ns-log01`, `ns-secrets01`, `ns-it01`, `ns-mon01`, `ns-ntp01`, `ns-audit01`, `ns-vpn01`  

**Utilisation totale :** 14 vCPU / 34 Go  
**Ressources restantes :** 0 vCPU / 22 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  
> 🔄 *Option séquentielle :* couper temporairement `ns-user01` (−2 vCPU / −4 Go) pour activer `ns-it01` (GLPI) à la place.

### 💽 Étape 6 – Sauvegarde, restauration & validation
> 💾 Tests de sauvegarde et restauration Bareos, vérification des stratégies et intégrité des données.

- 🟢 **VMs ON :** `ns-fw01`, `ns-bkp01`, `ns-lnx01`, `ns-ad01`, `ns-admin01`  
- 🔴 **VMs OFF :** `ns-wsus01`, `ns-log01`, `ns-secrets01`, `ns-it01`, `ns-mon01`, `ns-ntp01`, `ns-voip01`, `ns-audit01`, `ns-user01`, `ns-web01`, `ns-vpn01`, `ns-mail01`  

**Utilisation totale :** 10 vCPU / 26 Go  
**Ressources restantes :** 4 vCPU / 30 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  

### 📘 Étape 7 – Documentation & finalisation
> 📝 Étape de clôture : rédaction des guides, rapport final et mise en forme du portfolio.

- 🟢 **VMs ON :** *Aucune*  
- 🔴 **VMs OFF :** `ns-fw01`, `ns-lnx01`, `ns-ad01`, `ns-wsus01`, `ns-bkp01`, `ns-log01`, `ns-secrets01`, `ns-it01`, `ns-mon01`, `ns-ntp01`, `ns-voip01`, `ns-audit01`, `ns-admin01`, `ns-user01`, `ns-web01`, `ns-vpn01`, `ns-mail01`  

**Utilisation totale :** 0 vCPU / 0 Go  
**Ressources restantes :** 14 vCPU / 56 Go  
**Ressources réservées (hôte)** : 2 vCPU / 8 Go  

## 🖥️ Création d'une machine virtuelle VMware Workstation

---

👉 Retour à la [page index de l'étape](/Installations/Etape1/0-index.md).  
👉 Retour à la [page principale du projet](/README.md).  