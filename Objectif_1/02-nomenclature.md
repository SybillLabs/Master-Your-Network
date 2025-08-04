# 🏷️ Nomenclature + plan d’adressage IP avec schéma

## 🖥️ Nomenclature détaillée des machines

| Type | Nom            | Fonction & Rôle                                                                  | Hostname | Justification                                                                                         | Réseaux                 |
| ---- | -------------- | -------------------------------------------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------- | ----------------------- |
| VM   | `fw-router`    | Routeur / Pare-feu                                                               | `beru`   | Fidèle, gardien protecteur                                                                            | `WAN`<br>`LAN`<br>`DMZ` |
| VM   | `switch-VLANs` | Switch VLANs (VLAN10, 20, 30)                                                    | `tank`   | Ombre massive et stable, Tank structure les flux comme un switch : solide, silencieux, indispensable. | `LAN`                   |
| VM   | `linux-core`   | Serveur Linux (DHCP & DNS)                                                       | `igrit`  | Le 1er shadow loyal, stoïque, base solide du système                                                  | `LAN`                   |
| VM   | `windows-core` | Serveur Windows (AD, WSUS, GPO, Partage de fichiers)                             | `tusk`   | Mage stratège (GPO/AD = stratégie et magie)                                                           | `LAN`                   |
| VM   | `pc-admin`     | PC administrateur sous Linux                                                     | `jinwoo` | Maître du réseau                                                                                      | `LAN`                   |
| VM   | `pc-client`    | PC client sous Windows                                                           | `jinah`  | Sœur du maître du réseau, connectée au cœur du monde                                                  | `LAN`                   |
| VM   | `gestion-IT`   | Serveur de gestion IT : GLPI, Dashboard, Zabbix, Journalisation                  | `kamish` | Dragon mythique : pouvoir + surveillance                                                              | `LAN`                   |
| VM   | `com-core`     | Serveur de messagerie iRedMail, de transfert de fichier FTP, et de connexion VPN | `kaisel` | Monture volante (rapide, communication)                                                               | `DMZ`                   |
| VM   | `data-vault`   | Serveur de sauvegarde et de stockage                                             | `greed`  | Gardien du trésor, stockage sans fin                                                                  | `LAN`                   |

## 🌐 Plan d’adressage IP

### 🗂️ Les réseaux

*Tableau à ne plus tenir compte*

| Réseau | Plage d'adresse | Masque de réseau      | Adresse de réseau | Adresse de broadcast | Passerelle              | Nombre d’adresses utilisables |
| ------ | --------------- | --------------------- | ----------------- | -------------------- | ----------------------- | ----------------------------- |
| WAN    | NAT (externe)   | Fourni par l’hôte/FAI | x                 | x                    | Fourni par l’hôte/FAI   | x                             |
| LAN    | 192.168.1.0     | 255.255.255.0 (/24)   | 192.168.1.0       | 192.168.1.255        | Voir chaque sous-réseau | 254 (de .1 à .254)            |
| DMZ    | 192.168.0.0     | 255.255.255.0 (/24)   | 192.168.0.0       | 192.168.0.255        | 192.168.0.254           | 254 (de .1 à .254)            |

**Voici le nouveau tableau** :

| Réseau | Adresse de réseau | Masque de réseau | Adresse de broadcast | Passerelle    | Nombre d’adresses utilisables (sans la passerelle) |
| ------ | ----------------- | ---------------- | -------------------- | ------------- | -------------------------------------------------- |
| WAN    | DHCP              | -                | -                    | -             | -                                                  |
| LAN    | 192.168.0.0       | /24              | 192.168.0.255        | 192.168.0.254 | 253                                                |
| DMZ    | 192.168.1.0       | /24              | 192.168.1.255        | 192.168.1.254 | 253                                                |

Lors de ma première tentative de mise en place de VLANs via pfSense, j'ai appris que je ne pouvais pas créer des VLANs liés à l'interface LAN tout en utilisant son ancienne plage d'adresses IP : **192.168.1.0/24**.

J'ai donc décidé d'attribuer le réseau 192.168.0.0/24 pour la **LAN**, le réseau 192.168.1.0/24 pour le **DMZ**, et pour les VLANs, voir ci-dessous.

### 🔖 Les VLANs

> *Voici ce que j’avais initialement envisagé, mais que j’ai finalement modifié car cela rentrait en conflit avec l'ancien réseau LAN.*

~~Comment j’ai découpé le LAN ?  
J’ai divisé mon réseau local en trois sous-réseaux logiques, chacun correspondant à un type d’utilisation :~~  
- ~~Users : pour les utilisateurs classiques (postes bureautiques)~~  
- ~~DSI Users : pour les utilisateurs IT (Admin, Technicien…)~~  
- ~~DSI Servers : pour tous les serveurs de l’infrastructure~~  
~~Ensuite, j’ai estimé les besoins en nombre d’adresses pour chacun, en prévoyant des marges pour éviter de me retrouver bloquée si ça évolue :~~  
- ~~Users~~  
    - ~~Utilisateurs : 42~~  
    - ~~Passerelle, adresse réseau, broadcast → total min : 45~~  
    - ~~2⁶ = 64 pas suffisant sur le long terme → j’ai pris 2⁷ = 128 adresses, soit un /25~~  
- ~~DSI Users~~  
    - ~~Utilisateurs : 8~~  
    - ~~Passerelle, adresse réseau, broadcast → total min : 11~~  
    - ~~2⁴ = 16 un peu juste → j’ai pris 2⁵ = 32 adresses, soit un /27~~  
- ~~DSI Servers~~  
    - ~~Machines prévues : 8~~  
    - ~~Passerelle, adresse réseau, broadcast → total min : 11~~  
    - ~~Même logique → j’ai pris aussi /27~~

📌 ~~Pour rappel, mon LAN de base (192.168.1.0/24) offre 254 adresses utilisables, donc je garde une grosse réserve au cas où l’organisation s’agrandit ou si j’ai besoin d’ajouter un VLAN.~~

Comme dit précédemment, le découpage de VLANs que j'avais établi n'était pas le bon car cela rentrait en conflit avec l'ancien réseau LAN. J'ai donc décidé de modifier le réseau LAN, et pour les VLANs, j'ai fait comme suit dans le tableau ci-dessous.

### 📊 Tableau récapitulatif des VLANs

*Tableau à ne plus tenir compte*

| VLAN ID | Nom                  | Adresse de réseau | Masque                | Broadcast     | Passerelle    | Plage d’adresses              | Nb adresses |
| ------- | -------------------- | ----------------- | --------------------- | ------------- | ------------- | ----------------------------- | ----------- |
| 10      | **Users**            | 192.168.1.0       | 255.255.255.128 (/25) | 192.168.1.127 | 192.168.1.126 | 192.168.1.1 → 192.168.1.126   | 126         |
| 20      | **DSI Users**        | 192.168.1.128     | 255.255.255.224 (/27) | 192.168.1.159 | 192.168.1.158 | 192.168.1.129 → 192.168.1.158 | 30          |
| 30      | **DSI Servers**      | 192.168.1.160     | 255.255.255.224 (/27) | 192.168.1.191 | 192.168.1.190 | 192.168.1.161 → 192.168.1.190 | 30          |
| x       | **(Réserve future)** | 192.168.1.192     | 255.255.255.192 (/26) | 192.168.1.255 | 192.168.1.254 | 192.168.1.193 → 192.168.1.254 | 62          |

**Voici le nouveau tableau** :

| VLAN ID | Nom             | Adresse de réseau | Adresse de broadcast | Masque de réseau | Passerelle     | Nombre d'adresse disponible (sans la passerelle) | Statique / Dynammique |
| ------- | --------------- | ----------------- | -------------------- | ---------------- | -------------- | ------------------------------------------------ | --------------------- |
| 10      | **Users**       | 192.168.10.0      | 192.168.10.255       | /24              | 192.168.10.254 | 253                                              | Dynamique             |
| 20      | **DSI Users**   | 192.168.20.0      | 192.168.20.255       | /24              | 192.168.20.254 | 253                                              | Dynamique             |
| 30      | **DSI Servers** | 192.168.30.0      | 192.168.30.255       | /24              | 192.168.30.254 | 253                                              | Statique              |

## 🖧 Schéma réseau de l'entreprise

![schemareseau](/Objectif_1/Ressources/schema_reseau.png)

---

📁 *[Retour au fichier README.md](/README.md)*
