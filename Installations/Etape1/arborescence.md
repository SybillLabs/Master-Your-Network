# 🗺️ Arborescence Windows de l'infrastructure

## 📝 Contexte
Maintenant que l'infrastructure est bien finalisé, je peux concevoir **l'arborescence Windows** de l'infrastructure [NovaPlay Studio](./entreprise.md).  

🎯 **Pourquoi structurer une arborescence ?**  
Une arborescence bien pensée permet de :
- 🔐 **Renforcer la sécurité**
    - Appliquer le *principe du moindre privilège*.
    - Séparer clairement les utilisateurs, les admins, les serveurs, les postes, etc.
    - Cibler précisément les *GPO* (stratégies de groupe) selon les rôles.
- 🛠️ **Simplifier la gestion des accès**
    - Donner des droits via des *groupes*, et non utilisateur par utilisateur.
    - Appliquer des *permissions spécifiques* aux bons endroits.
- 🧑‍💼 **Représenter l’organisation réelle**
    - Reproduire la structure RH (services, équipes, directions).
    - Faciliter la gestion des flux d’informations et des responsabilités.
- 📁 **Uniformiser les politiques**
    - Appliquer automatiquement les bonnes *GPO* selon l’OU (ex : postes DSI ≠ postes utilisateurs).
    - Éviter les erreurs humaines et garantir la cohérence.
- 📊 **Faciliter l’audit et la traçabilité**
    - Savoir *qui a fait quoi, où et quand*.
    - Mieux réagir face à une cyberattaque ou un audit RGPD.
- ⚙️ **Préparer l’automatisation**
    - Une arborescence claire permet des scripts d’ajout/suppression automatisés.
    - Elle est indispensable pour accompagner la croissance de l’organisation.

## 🗂️ Structure de l'Active Directory (`ns-ad01`)
### 🧩 L'arborescence
- **Nom de domaine** : `novaplaystudio.lan`
- **Nombre de domaine** : 1, un seul domaine est suffisant pour une entreprise de 50 employés.
> 💡 Une entreprise comme **Siemens** qui est une multinational aura elle besoin de plusieurs domaines, et qui dit plusieurs domaines dit **forêt**.  
> Une fôret contient des domaines qui eux contiennes des unités d'organisations.  
> **Exemple** :
> - **Fôret** : `siemens.corp`
> - **Domaine** : `fr.siemens.corp`, `eng.siemens.corp`, `esp.siemens.corp`, ...
> - **OU** : `OU=IT`, `OU=Finance`, ...
- **Arborescence logique** : Basé sur les départements, les utilisateurs, les ordinateurs et les serveurs.
```powershell
DC=novaplaystudio,DC=lan
|
+--OU=Users
|   +--OU=DG    # Direction et administration
|       +--CN=Alexandre Morel
|       ...
|
|   +--OU=DEV   # Développement Jeux Vidéo
|       +--CN=Nicolas Dubois
|       ...
|
|   +--OU=COM   # Streaming & Communication
|       +--CN=Camille Durand
|       ...
|
|   +--OU=SUPP  # Support & Qualité
|       +--CN=Philippe Noël
|       ...
|
|   +--OU=IT    # Infrastructure & IT
|       +--CN=François Delauney
|       ...
|
|   +--OU=EXT   # Prestataires et intervenant externes (Comptes temporaires)
|
+--OU=Admins
|   +--OU=Comptes_Admins    # Comptes à privilèges (admin du domaine/serveurs)
|       +--CN=adm.f.delauney
|       ...
|
|   +--OU=Comptes_Services  # Comptes techniques utilisés par les services
|       +--CN=svc_glpi          # Lecture LDAP
|       +--CN=svc_backup        # Sauvegarde automatique (authentification AD + accès aux partages SMB)
|       +--CN=svc_log           # Archivage et rotation des logs (authentification AD + accès aux logs du serveur Windows)
|       +--CN=svc_vault         # Sauvegarde du coffre-fort des identifiants
|       +--CN=svc_zabbix        # Agent d'interrogation des serveurs
|       +--CN=svc_nextcloud     # Synchronisation LDAP des utilisateurs externes
|       +--CN=svc_mail          # Authentification des comptes iRedMail via LDAP
|       +--CN=svc_voip          # Authentification des utilisateurs VoIP (3CX)
|       +--CN=svc_audit         # Audit de sécurité nécessitant des requêtes LDAP de vérification de comptes et de permissions AD
|
+--OU=Ordinateurs
|   +--OU=Postes
|       +--CN=ns-admin01
|       +--CN=ns-user01
|   +--OU=Serveurs
|       +--CN=ns-fw01
|       +--CN=ns-lnx01
|       +--CN=ns-bkp01
|       +--CN=ns-log01
|       +--CN=ns-secrets01
|       +--CN=ns-it01
|       +--CN=ns-dmz01
|       +--CN=ns-voip01
|       +--CN=ns-audit01
|
+--OU=Tests     # Un terrain d'essai pour tester les GPO sans casser le domaine
```

> 💡 Je n'ai pas créé de compte de service `svc_wsus` car :
> - **WSUS** se trouve sur le même serveur que mon **Active Directory**. Dans le cas inverse, j'aurais créé le compte de service `svc_wsus`
> - J'ai une supervision standard avec **Zabbix** qui va simplement demander si le service est **actif**. Dans ce cas, j'utilise le compte de service `svc_zabbix` mais pas de compte de service `svc_wsus`.
> 
> 🔒 Tous les comptes `svc_*` sont :
> - Membre du groupe **Domain Users** uniquement
> - Interdits de connexion interactive (pas de RDP/console)
> - Mots de passe forts, non expirants
> - Affectés de droits spécifiques uniquement si nécessaire

### 👥 Les groupes
Les **groupes** permettent de **regrouper les utilisateurs, les ordinateurs et les comptes de service** afin de leur attribuer des **droits communs**, des **règles de sécurité** ou des **stratégies spécifiques** (GPO, accès SMB, RDP, etc.).

Ils constituent un **outil central de gestion dans Active Directory**, car ils permettent d’administrer les autorisations et les stratégies de **manière collective**, plutôt qu’utilisateur par utilisateur.

Cette organisation suit une logique simple et hiérarchisée :
- 🧩 **Un groupe par service métier** (Direction, Développement, Communication, Support, IT)
- 📁 **Un groupe par partage de fichier** pour gérer les accès aux répertoires réseau
- 🛠️ **Des groupes administratifs et techniques** dédiés à la gestion de l’infrastructure (administration, supervision, sauvegarde, VPN, etc.)
- 💬 **Des groupes de communication** utilisés pour la messagerie interne et les listes de diffusion
- 💻 **Des groupes d’ordinateurs** permettant d’appliquer des GPO ou des configurations spécifiques à certains postes ou serveurs

> Cette structuration facilite la gestion des permissions, la cohérence des politiques de sécurité, et la maintenabilité de l’infrastructure à long terme.

```powershell
DC=novaplaystudio,DC=lan
|
+--Groupes basiques        # Regroupement par service / département
|   +--GRP_DG              # Direction & Administration
|   +--GRP_DEV             # Développement Jeux Vidéo
|   +--GRP_COM             # Communication & Streaming
|   +--GRP_SUPP            # Support & Qualité
|   +--GRP_IT              # Infrastructure & IT
|
+--Groupes de partage      # Droits d'accès aux dossiers partagés SMB
|   +--GRP_SHARE_DG        # Accès au dossier \\jinwoo\Partage\Direction
|   +--GRP_SHARE_DEV       # Accès au dossier \\jinwoo\Partage\Dev
|   +--GRP_SHARE_COM       # Accès au dossier \\jinwoo\Partage\Com
|   +--GRP_SHARE_SUPP      # Accès au dossier \\jinwoo\Partage\Support
|   +--GRP_SHARE_IT        # Accès au dossier \\jinwoo\Partage\IT
|
+--Groupes administratifs  # Gestion et supervision du domaine
|   +--GRP_IT_Admins       # Accès complet aux serveurs / GPO / RDP
|   +--GRP_RDP_Users       # Autorisation de connexion RDP sur serveurs
|   +--GRP_GPO_Editors     # Gestion des stratégies de groupe
|
+--Groupes techniques      # Accès spécifiques à certains services
|   +--GRP_Backup_Read     # Lecture des partages pour sauvegarde (svc_backup)
|   +--GRP_Log_Read        # Lecture des journaux Windows (svc_log)
|   +--GRP_VPN_Users       # Autorisés à se connecter via VPN
|   +--GRP_Audit_Team      # Lecture rapports de sécurité et logs
|
+--Groupes de communication # Listes de diffusion (iRedMail / Nextcloud)
|   +--GRP_All_Staff       # Tous les employés internes
|   +--GRP_IT_Team         # Équipe technique (IT)
|   +--GRP_Direction       # Membres de la direction
|
+--Groupes ordinateurs        # Groupes contenant des postes ou serveurs
|   +--GRP_PC_Serveurs        # Tous les serveurs membres du domaine
|   +--GRP_PC_Admins          # Postes d'administration
|   +--GRP_PC_Clients         # Postes utilisateurs standards
|   +--GRP_PC_Tests           # Machines de test / sandbox
```

---

👉 Retour à la [page index de l'étape](/Etape1/index.md).  
👉 Retour à la [page principale du projet](/README.md).  