# Segmentation Réseau & Filtrage ACL d'Entreprise (Cisco Packet Tracer)

Projet pratique de conception réseau et sécurité périmétrique réalisé sous **Cisco Packet Tracer**. L'objectif est de segmenter une petite entreprise en trois services distincts reliés par un routeur central, puis de restreindre l'accès au serveur central via une liste de contrôle d'accès (ACL).

## Architecture et Topologie

La topologie repose sur un routeur Cisco 2911 (`R1-Entreprise`) interconnectant trois sous-réseaux locaux via trois commutateurs Catalyst 2960 (`SW-Admin`, `SW-Com`, `SW-IT`) :

- **Service Administratif (`192.168.10.0/24`)** : `PC-Admin1`, `PC-Admin2`, `Printer-Admin` via `SW-Admin` (`GigabitEthernet0/0`).
- **Service Commercial (`192.168.20.0/24`)** : `PC-Com1`, `PC-Com2` via `SW-Com` (`GigabitEthernet0/1`).
- **Service Informatique (`192.168.30.0/24`)** : `PC-IT1`, `Serveur-Central` via `SW-IT` (`GigabitEthernet0/2`).

## Plan d'Adressage IP

| Service | Sous-réseau | Masque | Passerelle (`R1-Entreprise`) | Équipement | Adresse IP |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Administratif** | `192.168.10.0` | `255.255.255.0` | `192.168.10.1` (`Gig0/0`) | `PC-Admin1` | `192.168.10.10` |
| **Administratif** | `192.168.10.0` | `255.255.255.0` | `192.168.10.1` (`Gig0/0`) | `PC-Admin2` | `192.168.10.11` |
| **Administratif** | `192.168.10.0` | `255.255.255.0` | `192.168.10.1` (`Gig0/0`) | `Printer-Admin` | `192.168.10.20` |
| **Commercial** | `192.168.20.0` | `255.255.255.0` | `192.168.20.1` (`Gig0/1`) | `PC-Com1` | `192.168.20.10` |
| **Commercial** | `192.168.20.0` | `255.255.255.0` | `192.168.20.1` (`Gig0/1`) | `PC-Com2` | `192.168.20.11` |
| **Informatique** | `192.168.30.0` | `255.255.255.0` | `192.168.30.1` (`Gig0/2`) | `PC-IT1` | `192.168.30.10` |
| **Informatique** | `192.168.30.0` | `255.255.255.0` | `192.168.30.1` (`Gig0/2`) | `Serveur-Central` | `192.168.30.50` |

## Règle de Sécurité (ACL Étendue)

Une ACL étendue nommée `BLOC_COM_SERVEUR` est appliquée en sortie (`out`) sur l'interface `GigabitEthernet0/2` de `R1-Entreprise` :

1. Bloque le trafic IP du réseau Commercial (`192.168.20.0/24`) vers le réseau Informatique (`192.168.30.0/24`).
2. Autorise le reste du trafic (`permit ip any any`).

## Tests de Validation

- **`PC-Admin1` -> `Printer-Admin` (`192.168.10.20`)** : `ping` réussi (`0%` de perte).
- **`PC-Admin1` -> `Serveur-Central` (`192.168.30.50`)** : `ping` réussi (`0%` de perte).
- **`PC-Com1` -> `Serveur-Central` (`192.168.30.50`)** : bloqué (`Destination host unreachable`, `100%` de perte).

## Règles de Sécurité (ACL Étendue)

Une ACL étendue nommée `BLOC_COM_SERVEUR` est appliquée en sortie (`out`) sur `GigabitEthernet0/2` (réseau Informatique) et sur `GigabitEthernet0/0` (réseau Administratif) de `R1-Entreprise` :

1. Bloque le trafic IP du réseau Commercial (`192.168.20.0/24`) vers le réseau Informatique (`192.168.30.0/24`).
2. Bloque le trafic IP du réseau Commercial (`192.168.20.0/24`) vers l'imprimante administrative `Printer-Admin` (`192.168.10.20`).
3. Autorise le reste du trafic (`permit ip any any`).

## Tests de Validation

- **`PC-Admin1` (`192.168.10.10`) -> `Printer-Admin` (`192.168.10.20`)** : `ping` réussi (`0%` de perte).
- **`PC-Admin1` (`192.168.10.10`) -> `Serveur-Central` (`192.168.30.50`)** : `ping` réussi (`0%` de perte).
- **`PC-Com1` (`192.168.20.10`) -> `Serveur-Central` (`192.168.30.50`)** : bloqué (`Destination host unreachable`, `100%` de perte).
- **`PC-Com1` (`192.168.20.10`) -> `Printer-Admin` (`192.168.10.20`)** : bloqué (`Destination host unreachable` ou `Request timed out`, `100%` de perte).

## Fichiers du Projet

- Fichier Packet Tracer (`.pkt`) de la topologie configurée et sauvegardée.
- Captures d'écran de la topologie et des tests `ping` de validation.

## Aperçu 

- [Topologie Cisco Packet Tracer][def]

[def]: Topologie.png
- [PC-Admin1 Cisco Packet Tracer][def2]
- [PC-COM1 Cisco Packet Tracer][def3]

[def2]: PC-Admin1.png
[def3]: PC-COM1.png