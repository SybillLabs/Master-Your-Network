# 🔐 Installation et configuration de pfSense

Pour l'installation et la configuration de pfSense, je me suis appuyée sur le tutoriel de chez [IT-Connect](https://www.it-connect.fr/comment-installer-pfsense-dans-virtualbox-pour-creer-un-lab-virtuel).

---

## ⚠️ Problèmes de démarrage de la VM
💻 Projet réalisé sur un PC hôte **Kubuntu** (variante d'Ubuntu) avec ~~**VirtualBox**~~ remplacé ensuite par VMware WorkStation Pro.

>📝 **Remarque importante** :
Bien que ces erreurs aient été rencontrées lors de l'utilisation de VirtualBox, elles sont **liées à la configuration de l'hôte** (le PC) et **auraient également pu survenir avec VMware Workstation Pro**.  
Ces problèmes ne sont donc **pas spécifiques au logiciel de virtualisation** utilisé.

### 🐛 Erreur n°1 – Virtualisation non activée

![erreur1](/Objectif_2/Ressources/erreurVM_SVM_Mode.png)

Cette erreur indique que la **virtualisation matérielle** n'était pas activée dans le BIOS.  
🔧 Pour corriger cela :
- Aller dans le BIOS → onglet **Advanced**
- Activer l’option **SVM Mode**

✅ Problème résolu après cette modification.

### 🐛 Erreur n°2 – Conflit avec KVM

![erreur2](/Objectif_2/Ressources/erreurVM_KVM.png)

Cette erreur indique que VirtualBox ne pouvait pas fonctionner tant que le module `KVM` était chargé au démarrage de l’hôte.

🔍 Pourquoi ?  
`KVM` monopolise AMD-V dès le démarrage de Linux. VirtualBox ne peut donc pas l'utiliser.

💡 Solution (permanente) : désactiver KVM pour éviter de refaire la manipulation à chaque redémarrage.

```bash
# Empêcher le chargement automatique des modules KVM
echo -e "blacklist kvm\nblacklist kvm_amd" | sudo tee /etc/modprobe.d/blacklist-kvm.conf

# Mettre à jour initramfs (le système temporaire de démarrage)
sudo update-initramfs -u

# Décharger immédiatement les modules sans redémarrage
sudo modprobe -r kvm_amd kvm
```

✅ Une fois cela fait, la VM démarre correctement, que ce soit avec VirtualBox ou VMware Workstation.

---
## 🛠️ Installation de la VM `firewall`
### 📝 Caractéristiques de la VM
- **Nom** : `firewall`
- **Description** : Routeur pfSense
- **Système d’exploitation** : FreeBSD 64-bit
- **Mémoire vive** : 2048 Mo (2 Go)
- **Processeurs** : 2 cœurs
- **Stockage** :
    - IDE Primary Device : Disque dur 20 Go
    - IDE Secondary Device : Lecteur ISO de pfSense
- 🌐 **Réseau :**
	- **Adapter 1** : NAT (accès Internet via IP dynamique ~~`10.0.2.15/24`~~ -> `172.16.104.128/24)
	- **Adapter 2** : Internal Network (`LAN`)
	    - IP statique : ~~`192.168.1.254`~~ -> `192.168.0.254/24`
	- **Adapter 3** : Internal Network (`DMZ`)
	    - IP statique : ~~`192.168.0.254`~~ -> `192.168.1.254/24`

### ▶️ Étapes d’installation
Voici le déroulement de l’installation de pfSense :
1. **Accept License** → `<Accept>`
2. **Welcome** → `Install`
3. **Partitioning** → `Auto (ZFS)`
4. **ZFS Configuration** :
    - Laisser les paramètres par défaut → `>>> Install`
    - Sélectionner `stripe`
    - Cocher `ada0` (barre espace), puis `Ok`
    - Confirmer l’installation sur `ada0` → `Yes`
5. **Fin d’installation** : ne pas redémarrer tout de suite. Éteindre la VM et retirer l’image ISO.
6. **Redémarrer la VM** (sans ISO) → menu de démarrage de pfSense.

![menu_pfSense](/Objectif_2/Ressources/menu_pfSense.png)

### 🌐 Configuration des interfaces réseau
1. **Interface WAN** : IP attribuée automatiquement par le DHCP
2. **Interface LAN** :
    - Aller dans le menu **2) Set interface(s) IP address**
    - Définir : ~~`192.168.1.254/24`~~ -> `192.168.0.254/24`

![LAN_IPadress](/Objectif_2/Ressources/LAN_IPadress.png)

3. **Interface DMZ** :
    - Aller dans **1) Assign Interfaces** → nommée temporairement `OPT1`
    - Puis aller dans **2) Set interface(s) IP address** → ~~`192.168.0.254`~~ -> `192.168.1.254/24`

![DMZ_IPadress](/Objectif_2/Ressources/DMZ_IPadress.png)

### ⚠️ Modification suite au changement de découpage réseau et de logiciel

Suite au changement de découpage réseau et de logiciel de virtualisation, voici la nouvelle configuration sur mon routeur pfSense.

![new_reseau](/Objectif_2/Ressources/new_reseau.png)

## 🌐 Accès Web à pfSense

### 🖥️ VM Administrateur `pc-admin`
Pour accéder à l’interface web de pfSense, création d’une VM graphique sur le réseau **LAN**.
#### 📝 Caractéristiques de la VM `pc-admin`
- **Nom** : `pc-admin`
- **Description** : PC Administrateur Linux
- **Système** : Ubuntu 64-bit
- **RAM** : 4 Go (installation), 2 Go ensuite
- **CPU** : 1 cœur
- **Disque dur** : 50 Go
- **Réseau** :
    - En NAT pour l’installation
    - Puis `Internal Network` → _LAN_  
        (IP temporaire : ~~`192.168.1.253`~~ -> `192.168.0.1/24`, DNS publics utilisés)

#### 🧩 Configuration utilisateur :
- Langue système : **anglais**
- Clavier : **français**
- Nom d’utilisateur : `administrator`
- Nom de l’ordinateur : `jinwoo`
- Mot de passe : sécurisé (minuscules, majuscules, chiffres, caractères spéciaux)

📝 _J'aurais pu utiliser la VM `linux-core`, mais dans un souci de bonnes pratiques, j’utilise une machine dédiée à l’administration._

## ⚙️ Configuration initiale via interface web

Accès via : ~~`http://192.168.1.254/`~~ -> `http://192.168.0.254`
- **Identifiants par défaut** :
    - Utilisateur : `admin`
    - Mot de passe : `pfsense`

### 🧭 Étapes du Wizard :
1. **Welcome** → `Next`
2. **Support Netgate** → `Next`
3. **Informations générales** :
    - Hostname : ~~`firewall`~~ -> `beru`
    - Domaine : `novastream.lan`
    - DNS primaire : `1.1.1.1`
    - DNS secondaire : `8.8.8.8`
    - `Override DNS` : activé  
        _(Modifiables plus tard une fois le DNS interne en place)_
4. **Heure** :
    - Serveur NTP : `fr.pool.ntp.org`
    - Fuseau : `Europe/Paris`  
        _(Sera aussi remplacé plus tard par un serveur NTP interne)_
5. **WAN Interface** :
    - _Décoché_ : `Block RFC1918` et `Block bogon networks`
6. **LAN Interface** : inchangé
7. **Mot de passe admin WebGUI** :
    - Mot de passe robuste : ≥14 caractères, avec majuscules, minuscules, chiffres et symboles
8. **Reload configuration** → `Reload`
9. **Wizard terminé** → pfSense prêt pour la configuration avancée

## 🧱 Configuration avancée

### 🔁 NAT (Network Address Translation)
📌 Le NAT permet aux machines du LAN et de la DMZ d’accéder à Internet avec l’adresse publique de l’hôte.

Pour ce projet, le **NAT automatique** est suffisant :  
➡️ toutes les machines ont accès à Internet.  
Les **restrictions** viendront plus tard via les règles de pare-feu.

### 🔥 Règles de pare-feu

#### Interface LAN
- ✅ **Anti-lockout** : empêche de se couper de l’interface web
- ✅ Autorisation du trafic IPv4 & IPv6 LAN → WAN

#### Interface WAN
- ❌ Aucune règle par défaut  
    ➡️ Tout trafic entrant est bloqué (sécurité)

#### Interface DMZ
- ❌ Aucune règle encore  
    ➡️ À configurer plus tard lors de la mise en place de la VM ~~`mail-core`~~ -> `com-core`

🔐 Les vraies règles de sécurité seront établies lors de l’**Objectif 4 – Sécurisation du réseau**.

---

📁 *[Retour au fichier README.md](/README.md)*
