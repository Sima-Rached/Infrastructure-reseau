# Projet Réseau d'Entreprise sous GNS3

## Présentation

Ce projet consiste à concevoir, configurer et simuler un réseau d'entreprise complet sous **GNS3**.

L'objectif est de mettre en œuvre plusieurs concepts fondamentaux des réseaux IP :

* Conception d'une architecture hiérarchique
* Adressage IPv4 avec VLSM
* Routage dynamique avec OSPF
* NAT et accès Internet
* Tunnels UDP
* ACL (Access Control Lists)
* DHCP
* Routage statique

Le réseau est composé de plusieurs départements interconnectés via un backbone assurant la communication entre toutes les zones de l'entreprise.


## Architecture du Réseau

### Départements

| Département              | Routeur |
| ------------------------ | ------- |
| Ressources Humaines (RH) | RZ-1    |
| Finance                  | RZ-2    |
| Vente / Achat            | RZ-3    |

### Backbone

Le backbone est constitué de quatre routeurs :

* R0
* R1
* R2
* R3

### Fonctions du Backbone

* Routage dynamique OSPF
* Interconnexion des départements
* Redondance des chemins
* Accès Internet
* Transport des tunnels sécurisés

---

## Plan d'Adressage

### Réseau global attribué

```text
192.168.0.0/17
```

### Répartition VLSM

| Département | Nombre d'hôtes | Réseau       | Masque |
| ----------- | -------------- | ------------ | ------ |
| RH          | 5170           | 192.168.0.0  | /19    |
| Finance     | 1230           | 192.168.32.0 | /21    |
| Vente/Achat | 432            | 192.168.40.0 | /23    |

````

### Interfaces Départementales

| Routeur | Interface | Adresse |
|----------|-----------|----------|
| RZ-1 | Fa0/0 | 192.168.0.1/19 |
| RZ-1 | Fa0/1 | 10.0.1.2/30 |
| RZ-2 | Fa0/0 | 192.168.32.1/21 |
| RZ-2 | Fa1/0 | 10.0.2.2/30 |
| RZ-3 | Fa0/0 | 192.168.40.1/23 |
| RZ-3 | Fa0/1 | 10.0.3.2/30 |

---

## Accès Internet

Le routeur **R0** joue le rôle de passerelle vers Internet.

Fonctionnalités :

* NAT dynamique
* PAT (Port Address Translation)
* Traduction des adresses privées vers une adresse publique

---

## Sécurité

### ACL

Les ACL permettent :

* Le contrôle du trafic entre départements
* La limitation des accès non autorisés
* Le filtrage des flux réseau

### Tunnels UDP

Des tunnels UDP sont mis en place entre le backbone et les départements afin de sécuriser les communications.

---

## Services Configurés

| Service       | Description                             |
| ------------- | --------------------------------------- |
| DHCP          | Attribution automatique des adresses IP |
| OSPF          | Routage dynamique                       |
| NAT/PAT       | Accès Internet                          |
| ACL           | Contrôle d'accès                        |
| UDP Tunnel    | Sécurisation des échanges               |
| Static Routes | Connectivité des réseaux locaux         |

---

## Tests Réalisés

Les tests suivants ont été validés :

* Ping entre postes d'un même département
* Ping entre départements
* Connectivité Backbone ↔ Départements
* Attribution DHCP
* Fonctionnement OSPF
* Validation des ACL
* Validation des tunnels UDP
* Accès Internet via NAT

---

## Problèmes Rencontrés

### Réseaux départementaux invisibles dans OSPF

**Cause :**

Les réseaux locaux n'étaient pas annoncés automatiquement dans le backbone.

**Solution :**

Ajout de routes statiques puis redistribution dans OSPF.

```cisco
ip route <reseau> <masque> <next-hop>

router ospf 1
 redistribute static subnets
```

---

### Problèmes de connectivité UDP

**Cause :**

Pare-feu Windows bloquant les ports UDP.

**Solution :**

* Désactivation temporaire du pare-feu
* Ouverture des ports nécessaires
* Redémarrage des tunnels UDP


## Résultats

Le projet permet :

-Communication entre tous les départements

-Routage dynamique via OSPF

-Attribution automatique des adresses IP

-Accès Internet via NAT/PAT

-Sécurisation des échanges grâce aux tunnels UDP

-Contrôle d'accès via ACL

---

## 📚 Compétences Mises en Œuvre

* Réseaux IP
* Subnetting & VLSM
* Routage statique
* OSPF
* NAT/PAT
* DHCP
* ACL Cisco
* GNS3
* Architecture réseau d'entreprise
* Dépannage réseau
