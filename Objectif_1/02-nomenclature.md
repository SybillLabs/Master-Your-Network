# 🏷️ Nomenclature détaillée des machines et plan d’adressage IP avec schéma

## 🖥️ Nomenclature détaillée des machines

| Type | Nom            | Fonction & Rôle                                                 | Hostname | Justification                                         | Réseaux                 |
| ---- | -------------- | --------------------------------------------------------------- | -------- | ----------------------------------------------------- | ----------------------- |
| VM   | `firewall`     | Routeur / Pare-feu / Switch VLANs                               | `beru`   | Fidèle, gardien protecteur                            | `WAN`<br>`LAN`<br>`DMZ` |
| VM   | `linux-core`   | Serveur Linux (DHCP & DNS)                                      | `igrit`  | Le 1er shadow loyal, stoïque, base solide du système  | `LAN`                   |
| VM   | `windows-core` | Serveur Windows (AD, WSUS, GPO, Partage de fichier)             | `tusk`   | Mage stratège (GPO/AD = stratégie et magie)           | `LAN`                   |
| VM   | `pc-admin`     | PC administrateur sous Linux                                    | `jinwoo` | Maitre du réseau                                      | `LAN`                   |
| VM   | `pc-client`    | PC client sous windows                                          | `jinah`  | Soeur du maitre du réseau, connecté au coeur du monde | `LAN`                   |
| VM   | `gestionIT`    | Serveur de gestion IT : GLPI, Dashboard, Zabbix, Journalisation | `kamish` | Dragon mythique : pouvoir + surveillance              | `LAN`                   |
| VM   | `mail-core`    | Serveur de messagerie iRedMail                                  | `kaisel` | Monture volante (rapide, communication)               | `DMZ`                   |
| VM   | `datavault`    | Serveur de sauvegarde et de stockage                            | `greed`  | Gardien du trésor, stockage sans fin                  | `LAN`                   |

---

## 🌐 Plan d'adressage IP

### 🗂️ Les réseaux

|Réseau|Plage d'adresse|Masque de réseau|Adresse de réseau|Adresse de broadcast|Passerelle|Nombre d'adresse|
|---|---|---|---|---|---|---|
|WAN|NAT (externe)|fourni par hôte / FAI|x|x|Fourni par hôte / FAI|x|
|LAN|192.168.1.0|255.255.255.0 (/24)|192.168.1.0|192.168.1.255|Voir chacun des sous-réseaux|254 - Adresse utilisables de 1 à 254|
|DMZ|192.168.0.0|255.255.255.0 (/24)|192.168.0.0|192.168.0.255|192.168.0.254|254 - Adresse utilisables de 1 à 254|
### 🔖 Les VLANs

**Comment est-ce que j'ai découpé ce réseau ?**  
J'ai découpé mon _LAN_ en trois sous-réseau distinct :

- **Users** pour les utilisateurs lambda, post-bureautique
- **DSI Users** pour les utilisateurs de la DSi (Administrateur, Technicien, etc...)
- **DSI Servers** pour les serveurs et machines de l'infrastructure

Maintenant que j'ai découpé mon _LAN_ en trois sous-réseau, il faut que je détermine les plages d'adresse par sous-réseau; et pour cela il faut que je sache combien chaque sous-réseau aura d'adresse :

- **Users**
    - Nombre d'utilisateur : 42
    - Passerelle par défaut +1
    - Adresse de réseau +1
    - Adresse de brodcast +1
    - Total d'adresse minimum : 45
    - 2⁶ = 64, mais pour avoir plus de liberté j'ai pris 2⁷ = 128, ce qui donne un masque de sous réseau de /25
- **DSI Users**
    - Nombre d'utilisateur : 8
    - Passerelle par défaut +1
    - Adresse de réseau +1
    - Adresse de brodcast +1
    - Total d'adresse minimum : 11
    - 2⁴ = 16, mais pour avoir plus de liberté j'ai pris 2⁵ = 32, ce qui donne un masque de sous réseau de /27
- **DSI Servers**
    - Nombre de machines : 8
    - Passerelle par défaut +1
    - Adresse de réseau +1
    - Adresse de brodcast +1
    - Total d'adresse minimum : 11
    - 2⁴ = 16, mais pour avoir plus de liberté j'ai pris 2⁵ = 32, ce qui donne un masque de sous réseau de /27

Pour rappel, mon réseau _LAN_ a au total 253 adresse disponible (en enlevant, l'adresse de réseau, l'adresse de broadcast et l'adresse de passerelle par défaut).

Avec ce découpage, je n'ai pas utilisé l'intégralité de mon réseau _LAN_, je conserve donc le reste pour une _réserve future_ (agrandissement de la boîte, besoin d'une extension pour l'un des VLANs existant, etc...)

Donc chacun de mes _VLANs_ auront comme adresse de réseau :

|VLANs ID|Nom du sous-réseau|Adresse de réseau|Masque|Broadcast|Passerelle|Plage d’adresses utilisables|Nb d’adresses utilisables|
|---|---|---|---|---|---|---|---|
|10|**Users**|192.168.1.0|255.255.255.128 (/25)|192.168.1.127|192.168.1.126|192.168.1.1 → 192.168.1.126|126|
|20|**DSI Users**|192.168.1.128|255.255.255.224 (/27)|192.168.1.159|192.168.1.158|192.168.1.129 → 192.168.1.158|30|
|30|**DSI Servers**|192.168.1.160|255.255.255.224 (/27)|192.168.1.191|192.168.1.190|192.168.1.161 → 192.168.1.190|30|
|x|**(Réserve future)**|192.168.1.192|255.255.255.192 (/26)|192.168.1.255|192.168.1.254|192.168.1.193 → 192.168.1.254|62|

---

## 🖧 Schéma réseau de l'entreprise

![schemareseau](/Objectif_1/Ressources/schema_reseau.png)


---
*[Retour au fichier README.md](/README.md)*
