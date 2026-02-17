# 🌐 Étape 2 – Réseau et Socle Système

![Statut](https://img.shields.io/badge/Statut-En%20cours-yellow?style=flat-square&logo=github)

## 📝 Contexte

Cette étape se concentre sur la mise en place du coeur de l’infrastructure réseau et des services essentiels. Nous allons configurer les éléments suivants :
- `ns-router` : routeur principal avec VyOS.
- `nx-lnx` : Serveur Linux pour les services de base (DNS, DHCP).
- `ns-ad01` : Serveur primaire Active Directory.
- `ns-ad02` : Serveur secondaire Active Directory.

# 🎯 Objectifs
- Configurer le routage et les VLANs sur `ns-router`.
- Installer et configurer les services DNS et DHCP sur `nx-lnx`.
- Déployer Active Directory sur `ns-ad01` et `ns-ad02` pour assurer la redondance.
- Assurer la connectivité et la communication entre tous les composants du réseau.
- Documenter les configurations et les étapes réalisées pour une maintenance future.

# 📦 Livrables et actions de l’étape
1. 🛜 [Mise en place du routeur VyOS](/Installations/Etape2/1-VyOS.md) : Configuration initial du WAN/LAN/DMZ et des règles de pare-feu de base.
2. 🖧 [Mise en place des VLANs](/Installations/Etape2/2-VLANs.md) : Configuration de trois VLANs dans le LAN via et géré par le routeur VyOS.
3. 🐧 [Mise en place du serveur Linux primaire](/Installations/Etape2/3-Linux.md) : Configuration d'un serveur debian non graphique pour les services DHCP (isc-dhcp-server) & DNS (bind9).
4. 🪟 [Mise en place des serveurs Windows primaire et secondaire](/Installations/Etape2/4-Windows.md) : Configuration d'un Windows Serveur *GUI* en tant que *Domain Controller 1* (Rôle : Active Directory, DNS intégré, GPO, SMB) et d'un Windows Serveur *Core* en tant que *Domain Controller 2* (Réplication du DC1).

---

[![README](https://img.shields.io/badge/Back%20to-Master%20your%20network-blue?style=social&logo=github)](/README.md)