# 🗂️ Etape 1 : Préparation et planification

🔹💼 **Informations de présentation du projet**  
![Statut du projet](https://img.shields.io/badge/statut-en%20cours-yellow)
![Licence](https://img.shields.io/badge/Licence-MIT-blue)
![Langue](https://img.shields.io/badge/langue-français-blue)

🔹🧭 **Badges de conception et de planification**  
![Documentation](https://img.shields.io/badge/Documentation-Technique-blue)
![Analyse](https://img.shields.io/badge/Analyse-Infrastructure-green)
![Architecture](https://img.shields.io/badge/Architecture-Infrastructure-orange)
![Topology](https://img.shields.io/badge/Topology-Schéma-blue)

🔹🛠️ **Outils et composants principaux**  
![Draw.io](https://img.shields.io/badge/Draw.io-Design%20d'infrastructure-orange?logo=diagramsdotnet)
![Domain Controller](https://img.shields.io/badge/Domain%20Controller-DC-0078D6?logo=windows)
![Organizational Unit](https://img.shields.io/badge/Organizational%20Unit-OU-1E90FF?logo=windows)
![Common Name](https://img.shields.io/badge/Common%20Name-CN-4682B4?logo=windows)
![AD Group](https://img.shields.io/badge/AD%20Group-Groupe-005A9C?logo=windows)

## 📝 Contexte
Cette étape permet de concevoir l'infrastructure système et réseau sous deux points de vue :
- **Réelle** : L'exosquelette de l'infrastructure dans le monde physique
- **Virtuel** : L'exosquelette de l'infrastructure virtualisé avec les ressources de mon PC

## 🎯 Objectifs
- Comprendre les besoins de l'entreprise fictive
- Identifier les ressources matérielles et logicielles nécessaires
- Préparer une nomenclature claire et normalisée
- Poser les bases d'un plan réseau et d'une organisation **Active Directory** cohérente

## 📦 Livrables et actions de l’étape
- 🏢 **[Présentation de l'entreprise](./entreprise.md)**
  - Fiche entreprise et ses besoins IT
- 🧾 **[Inventaire et nomenclature de l'infrastructure](./inventaire.md)**
  - Liste des serveurs, postes, équipements réseaux, services attendus et nommage conventionnel
- 💽 **[La virtualisation avec VMware Workstation](./vmware.md)**
  - Caractrériques des VMs, création des VMs par rapport aux étapes de conception et création d'une VM avec VMware Workstation
- 🗺️ **[Arborescence Windows de l'infrastructure](./arborescence.md)**
  - Organisation de l'Active Directory (OU, groupes, GPO)
- 🌐 **[Plan réseau de l'infrastructure](./plan.md)**
  - Segmentation des différents réseaux , schéma des VLANs, LAN/DMZ et plan d'interconnexion

## ⚠️ Contraintes
- Ressources limitées (PC hôte avec 64Go de RAM, 8 cœurs & 16 threads CPU)
- Environnement virtualisé (VMware Workstation)
- Respect d'une logique de documentation

---

👉 Retour à la [page principale du projet](/README.md).