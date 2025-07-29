# 🗺️ Arborescence de l’infrastructure
## 🌲 C’est quoi, une arborescence d’infrastructure ?
C’est une **structure hiérarchique logique** qui reflète l’organisation des ressources de l’entreprise dans un domaine Active Directory (AD). Elle regroupe les objets comme les **utilisateurs, groupes, ordinateurs, imprimantes, services** dans des **unités d’organisation (OU)**, selon des règles fonctionnelles, techniques ou de sécurité.

## 🎯 Pourquoi structurer une arborescence ?
Une arborescence bien pensée permet de :
- 🔐 **Renforcer la sécurité**
    - Appliquer le _principe du moindre privilège_.
    - Séparer clairement les utilisateurs, les admins, les serveurs, les postes, etc.
    - Cibler précisément les _GPO_ (stratégies de groupe) selon les rôles.
- 🛠️ **Simplifier la gestion des accès**
    - Donner des droits via des _groupes_, et non utilisateur par utilisateur.
    - Appliquer des _permissions spécifiques_ aux bons endroits.
- 🧑‍💼 **Représenter l’organisation réelle**
    - Reproduire la structure RH (services, équipes, directions).
    - Faciliter la gestion des flux d’informations et des responsabilités.
- 📁 **Uniformiser les politiques**
    - Appliquer automatiquement les bonnes _GPO_ selon l’OU (ex : postes DSI ≠ postes utilisateurs).
    - Éviter les erreurs humaines et garantir la cohérence.
- 📊 **Faciliter l’audit et la traçabilité**
    - Savoir _qui a fait quoi, où et quand_.
    - Mieux réagir face à une cyberattaque ou un audit RGPD.
- ⚙️ **Préparer l’automatisation**
    - Une arborescence claire permet des scripts d’ajout/suppression automatisés.
    - Elle est indispensable pour accompagner la croissance de l’organisation.

## 🗂️ Structure Active Directory envisagée (`windows-core`)

Voici l’arborescence prévue pour ton environnement de test :
```powershell
DC=novastream,DC=local
|
+--OU=Utilisateurs
|   +--OU=DG         # Direction Générale
|   +--OU=DSI        # Service informatique
|   +--OU=PROD       # Production audiovisuelle
|   +--OU=CREA       # Création graphique
|   +--OU=MKT        # Marketing et communication
|   +--OU=JURH       # Juridique & Ressources Humaines
|   +--OU=SUPP       # Support technique
|   +--OU=EXT        # Prestataires et intervenants externes
|   +--OU=Admins     # Comptes avec droits élevés
|
+--OU=Groupes
|   +--OU=Accès_Fichiers   # Groupes liés aux dossiers partagés
|   +--OU=Sécurité         # Groupes sensibles (admin, RDP, VPN, etc.)
|   +--OU=Applications     # Droits applicatifs spécifiques
|
+--OU=Ordinateurs
|   +--OU=PostesFixes
|   +--OU=Serveurs
|   +--OU=Mobiles
|
+--OU=Tests
```

## 🔍 OU vs Groupe AD : quelle différence ?

|Élément|Rôle principal|
|---|---|
|**OU (Unité d'organisation)**|Sert à structurer l’AD, appliquer des GPO, isoler des entités|
|**Groupe AD**|Sert à attribuer des droits sur des ressources (partages, apps, imprimantes, etc.)|

➡️ **En résumé** :  
_Une OU structure et applique des règles, un groupe gère les permissions.**_**

## ✅ Les types de groupes prévus

### 🔐 1. Groupes de sécurité (Administrateurs)
Utilisés pour restreindre l’accès aux ressources sensibles de l’environnement.

|Groupe|Rôle|Statut|
|---|---|---|
|`G_SEC_Admins_Domain`|Admins du domaine (pleins pouvoirs AD)|Obligatoire|
|`G_SEC_Admins_Servers`|Admins des serveurs (hors DC)|Obligatoire|
|`G_SEC_RDP_Acces`|Autorise le RDP vers certains postes ou serveurs|Prévisionnel|
|`G_SEC_VPN_Users`|Autorise l’accès VPN (future mise en place)|Prévisionnel|

### 📁 2. Groupes de partage de fichiers
Contrôlent l’accès aux répertoires partagés de chaque service.

|Groupe|Rôle|Statut|
|---|---|---|
|`G_SHARE_DSI`|Partage du service informatique|Obligatoire|
|`G_SHARE_CREA`|Partage des créatifs graphiques|Prévisionnel|
|`G_SHARE_MKT`|Partage marketing & communication|Prévisionnel|
|`G_SHARE_JURH_Confidentiel`|RH confidentiel (accès très restreint)|Obligatoire|
|`G_SHARE_COMMUN`|Dossier partagé accessible à tous les employés|Obligatoire|
|`G_SHARE_DG`|Répertoires internes à la direction générale|Prévisionnel|
|`G_SHARE_JURH`|RH général (non confidentiel, ex : planning congés)|Prévisionnel|
|`G_SHARE_SUPP`|Dossiers communs pour l’équipe support|Obligatoire|
|`G_SHARE_EXT`|Zone de dépôt pour les prestataires externes|Prévisionnel|
### 🧑‍🔧 3. Groupes techniques
Réservent des droits ou accès à certains outils pour les techniciens IT.

| Groupe                   | Rôle technique attribué                                           | Statut       |
| ------------------------ | ----------------------------------------------------------------- | ------------ |
| `G_TECH_Supervision`     | Accès aux outils de supervision (Zabbix, Grafana...)              | Prévisionnel |
| `G_TECH_BackupOperators` | Gestion ou supervision des sauvegardes                            | Prévisionnel |
| `G_TECH_Support`         | Droits d’accès aux postes utilisateurs pour assistance à distance | Obligatoire  |
### 💼 4. Groupes fonctionnels (GPO & rôles métier)
Permettent d’appliquer des GPO métiers ou des droits d’accès spécifiques selon le service.

|Groupe|Rôle métier associé|Statut|
|---|---|---|
|`G_FUNC_DG`|Direction Générale|Obligatoire|
|`G_FUNC_DSI`|Informatique (droits logiciels, GPO spécifiques)|Obligatoire|
|`G_FUNC_CREA`|Création graphique (accès apps Adobe, etc.)|Obligatoire|
|`G_FUNC_MKT`|Équipe marketing (accès réseau sociaux, outils mailing)|Obligatoire|
|`G_FUNC_JURH`|Juridique & RH (applications métier, SIRH, contrats)|Obligatoire|
|`G_FUNC_SUPP`|Support technique (accès aux tickets, gestion d’incidents)|Obligatoire|
|`G_FUNC_EXT`|Externes avec accès limité ou temporaire|Obligatoire|
### 🧪 5. Groupes de tests
Utilisés pour les expérimentations (GPO, scripts, déploiements…).

|Groupe|Usage prévu|Statut|
|---|---|---|
|`G_TEST_GPO`|Tester des GPO sans impacter l’environnement réel|Obligatoire|

---

📁 *[Retour au fichier README.md](/README.md)*
