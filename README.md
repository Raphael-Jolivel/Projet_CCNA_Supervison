# Projet CCNA & Supervision : Rénovation Réseau Tac&Tic Brother

## 📌 Présentation du projet
Ce projet consiste à concevoir, déployer et superviser l'infrastructure réseau de la société **Tac&Tic Brother** dans le cadre de son déménagement. L'objectif est de passer d'un réseau hétérogène à une architecture segmentée, sécurisée et maintenable (MCO).

Notre équipe (Groupe 1) a eu la charge de la division **Commercial**.

## 🏗️ Architecture Réseau (Division Commercial)
L'infrastructure repose sur une segmentation stricte en VLANs via une architecture hybride Cisco (Physique) et OPNsense (Virtuel).

### Plan d'Adressage IPv4
| VLAN | Nom | Sous-réseau | Passerelle | Usage |
| :--- | :--- | :--- | :--- | :--- |
| **10** | ADMIN | `10.100.1.0/26` | `10.100.1.1` | Gestion OPNsense & Switch |
| **20** | USERS | `10.100.1.64/26` | `10.100.1.65` | Postes Commerciaux (DHCP) |
| **30** | SRV | `10.100.1.128/27` | `10.100.1.129` | Services internes |
| **40** | GUEST | `10.100.1.160/27` | `10.100.1.161` | Accès invités isolés  |
| **99** | MGMT | `10.100.1.192/28` | `10.100.1.193` | Management technique  |

### Topologie Technique
- **Routeur Cisco (TTB-G1-RTR) :** Gère l'interconnexion WAN inter-divisions (172.16.0.0/24).
- **Pare-feu OPNsense (TTB-G1-FW) :** Cœur de sécurité gérant le routage inter-VLAN, le filtrage, DHCP et DNS.
- **Transit L3 :** Liaison dédiée en `/30` entre le routeur Cisco et l'OPNsense (10.255.4.0/30).

## 🛡️ Politique de Sécurité (Zero Trust)
La sécurité est appliquée via des règles de filtrage stateful sur OPNsense:
- **Isolement Inter-VLAN :** Les utilisateurs (VLAN 20) ne peuvent pas accéder au réseau d'administration (VLAN 10).
- **Flux Métier Imposé :** Accès HTTPS autorisé vers le portail `intranet.ttb.local` hébergé par la division Développement.
- **Filtrage Invités :** Le VLAN GUEST est strictement limité à Internet et isolé du réseau interne.

## 📊 Stratégie de Supervision & RUN
L'exploitation est centralisée sur une stack de monitoring moderne pour garantir la visibilité du service.

### Stack Technique (NOC)
- **Grafana & Prometheus :** Hypervision en temps réel des performances (CPU, RAM, Uptime).
- **Blackbox Exporter :** Mesure de la latence et disponibilité des services DNS et HTTPS.
- **Loki :** Centralisation et analyse des logs firewall (Preuves ALLOW/BLOCK).
- **ITSM (GLPI) :** Gestion de l'inventaire des actifs et des tickets d'incidents (SLA).

### Alerting
Des alertes actionnables ont été configurées (ex: Utilisation CPU > 1% pour test de firing) afin de notifier l'équipe Ops en cas d'anomalie.

## ✅ Validation & Recette
Toutes les exigences critiques ont été validées par des tests documentés:
- **DHCP/DNS :** Attribution d'IP fonctionnelle et résolution de noms externe (google.com).
- **Connectivité :** Ping vers la passerelle et navigation web validés sur les postes clients.
- **Preuves Firewall :** Logs OPNsense confirmant le blocage des flux non autorisés.

## 👥 Équipe Projet
- **Ethan :** Chef d'équipe / Coordinateur projet.
- **Yanick & Raphaël :** Responsables Réseau & Virtualisation.
- **Edouard & Kephas :** Responsables Recette, Qualité & Documentation.
