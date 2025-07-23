# 🏷️ Nomenclature détaillée des machines et plan d’adressage IP

| Type | ID    | Nom            | Fonction & Rôle                                                 | Hostname | Justification                                         | Réseau                  | Adresse IP |
| ---- | ----- | -------------- | --------------------------------------------------------------- | -------- | ----------------------------------------------------- | ----------------------- | ---------- |
| VM   | VM101 | `firewall`     | Routeur / Pare-feu / Switch VLANs                               | `beru`   | Fidèle, gardien protecteur                            | `WAN`<br>`LAN`<br>`DMZ` |            |
| VM   | VM102 | `linux-core`   | Serveur Linux (DHCP & DNS)                                      | `igrit`  | Le 1er shadow loyal, stoïque, base solide du système  | `LAN`                   |            |
| VM   | VM103 | `windows-core` | Serveur Windows (AD, WSUS, GPO, Partage de fichier)             | `tusk`   | Mage stratège (GPO/AD = stratégie et magie)           | `LAN`                   |            |
| VM   | VM104 | `pc-admin`     | PC administrateur sous Linux                                    | `jinwoo` | Maitre du réseau                                      | `LAN`                   |            |
| VM   | VM105 | `pc-client`    | PC client sous windows                                          | `jinah`  | Soeur du maitre du réseau, connecté au coeur du monde | `LAN`                   |            |
| VM   | VM106 | `gestionIT`    | Serveur de gestion IT : GLPI, Dashboard, Zabbix, Journalisation | `kamish` | Dragon mythique : pouvoir + surveillance              | `LAN`                   |            |
| VM   | VM107 | `mail-core`    | Serveur de messagerie iRedMail                                  | `kaisel` | Monture volante (rapide, communication)               | `DMZ`                   |            |
| VM   | VM108 | `datavault`    | Serveur de sauvegarde et de stockage                            | `greed`  | Gardien du trésor, stockage sans fin                  | `LAN`                   |            |
