# 2. Étude des Solutions Existantes

## 2.1. Objectifs de l'Étude

L'analyse des solutions existantes vise à étudier les architectures et fonctionnalités de plateformes commerciales proposant des services de partage de ressources physiques ou de réservation d'espaces. Cette démarche répond à trois objectifs principaux :

1. **Comprendre les mécanismes fonctionnels et opérationnels** régissant la gestion de ressources partagées dans des contextes réels.
2. **Distinguer le socle fonctionnel générique** (commun à tout système de réservation) des **spécificités métiers** propres à chaque domaine d'application.
3. **Identifier les points de variabilité** (métamodèle, règles de gestion, workflows) qui alimenteront la conception du noyau générique, ainsi que les points d'extension requis pour l'adjonction de modules spécifiques.

Dans le cadre de ce projet, l'étude s'appuie sur deux domaines applicatifs distincts :
* **La mobilité urbaine partagée** (flottes de véhicules en libre-service).
* **La réservation d'espaces professionnels** (salles privées pour entretiens et réunions).

---

## 2.2. Étude des Solutions de Mobilité Urbaine Partagée

### 2.2.1. Présentation des Solutions
Les services de mobilité partagée permettent à des utilisateurs d'accéder à la demande à des véhicules légers (vélos, trottinettes, scooters) répartis sur une zone géographique donnée.

* **ShareBike :** Solution logicielle et matérielle dédiée à la gestion de flottes de vélos (mécano-stables ou électriques). L'expérience utilisateur repose sur l'inscription, la géolocalisation sur carte interactive, le déverrouillage par scan de code QR et la clôture assistée de la session de location. *(Source : Documentation officielle ShareBike & Guide d'utilisation).*
* **Lime :** Opérateur global de micro-mobilité électrique. La plateforme intègre la recherche de véhicules à proximité, l'application dynamique de règles locales (ex. zones de stationnement obligatoire, limitations de vitesse), l'affichage de la tarification avant course et la gestion automatique du paiement post-trajet. *(Source : Centre d'aide et documentation Lime).*

### 2.2.2. Synthèse des Fonctionnalités Observées
L'analyse de ces plateformes met en évidence un ensemble de fonctionnalités clés :

* **Gestion de parc et cycle de vie :** Identification unique des unités, suivi de l'état de fonctionnement (batterie, maintenance) et géolocalisation.
* **Consultation et recherche en temps réel :** Visualisation cartographique des ressources disponibles à proximité.
* **Moteur d'allocation / Session d'usage :** Processus de réservation instantanée, déverrouillage distant, suivi de session et clôture d'usage.
* **Gestion des contraintes spatiales :** Contrôle des zones d'évolution autorisées (*geofencing*), zones d'interdiction de stationnement (*no-parking zones*) et bornes virtuelles.
* **Tarification dynamique :** Calcul du coût d'usage combinant des frais fixes de déverrouillage et un tarif à la minute ou à la distance parcourue.
* **Gestion des événements et incidents :** Signalement par l'usager de pannes, dégradations ou anomalies géographiques.

### 2.3.3. Besoins Spécifiques au Domaine
La micro-mobilité se caractérise par une forte dynamique temporelle et spatiale. Elle requiert la gestion continue de données télémétriques (GPS, niveau de charge), la gestion de pannes à la volée, ainsi que la vérification de contraintes géographiques lors des transitions d'état (ex. interdiction de clôturer la location hors zone).

> **Implication d'architecture :** Ces attributs (coordonnées GPS, niveau de batterie) et contraintes (zones géographiques) ne doivent pas être codés dans le noyau générique. Ils constitueront une extension de type *Télémétrie & Geofencing* greffée sur le modèle d'état des ressources.

---

## 2.3. Étude des Solutions de Réservation de Salles

### 2.3.1. Présentation des Solutions
Les plateformes de réservation d'espaces gèrent la mise à disposition temporaire de lieux physiques pour des besoins professionnels (réunions, entretiens, postes de coworking).

* **Netveep :** Plateforme de réservation d'espaces de travail et de salles de réunion en Tunisie. Le parcours utilisateur s'articule autour d'un catalogue multi-critères (capacité, localisation, équipements), d'un calendrier de réservation temps réel et d'un système de confirmation de réservation. *(Source : Site officiel Netveep).*
* **Skedda :** Solution SaaS de gestion d'espaces partagés. Elle intègre un moteur de réservation basé sur des grilles d'emplois du temps, des règles de restriction d'accès personnalisées (quotas par utilisateur, délais d'annulation) et la synchronisation avec des calendriers externes (Google Calendar, Outlook). *(Source : Documentation Skedda).*

### 2.3.2. Synthèse des Fonctionnalités Observées
* **Catalogue d'espaces configurables :** Fiches détaillées présentant la capacité, les aménagements (vidéoprojecteur, visio, tableau) et l'emplacement.
* **Gestion des grilles d'emplois du temps :** Réservation par créneaux horaires fixes ou sur-mesure.
* **Moteur de détection de conflits :** Contrôle des chevauchements temporels (*anti-overbooking*) en temps réel.
* **Gestion des politiques de réservation :** Délais minimaux de préavis, durée maximale de réservation, quotas par utilisateur.
* **Tarification temporelle :** Modèle de facturation à l'heure, à la demi-journée ou à la journée.

### 2.3.3. Besoins Spécifiques au Domaine
Dans le cas particulier de la réservation de salles privées pour entretiens confidentiels, des critères spécifiques émergent : niveau d'isolation phonique, niveau de confidentialité, dispositifs de contrôle d'accès physique (digicodes éphémères, badges) et intégration aux agendas des recruteurs.

> **Implication d'architecture :** La dimension temporelle discrète (créneaux/calendriers) et les attributs d'équipement constituent des configurations particulières du métamodèle, complétées par des extensions pour les sous-systèmes tiers (ex. contrôle d'accès réseau/serrure connectée).

---

## 2.4. Comparaison des Domaines et Analyse de Variabilité

Le tableau comparatif ci-dessous résume les divergences et convergences entre les deux cas d'usage :

| Critère d'analyse | Mobilité Urbaine Partagée | Réservation de Salles d'Entretien |
| :--- | :--- | :--- |
| **Type de Ressource** | Mobile (Vélo, Trottinette) | Immobile / Fixe (Salle, Bureau) |
| **Mode d'Allocation** | Instantané / Spontané (Sur place) | Anticipé / Plannifié (Créneaux horodatés) |
| **Critère Majeur de Disponibilité** | Proximité spatiale (Coordonnées GPS) | Disponibilité temporelle (Disponibilité d'agenda) |
| **Attributs Métier** | Charge batterie, état d'usure, GPS | Capacité d'accueil, équipements, isolation phonique |
| **Règles de Gestion Clés** | Limites géographiques, retour en station | Non-chevauchement, quotas d'heures, règles d'annulation |
| **Modèle Tarification** | Prise en charge + Coût à la minute/km | Forfait par créneau (Heure / Journée) |
| **Déclencheur d'Usage** | Scan QR / Communication IoT | Validation de réservation / Badge d'accès |

---

## 2.5. Synthèse et Directives pour l'Architecture Générique

L'analyse comparative confirme la possibilité de concevoir un **noyau applicatif générique unique**, capable d'absorber ces deux cas d'usage à condition de bien isoler la configuration du métamodèle et les points d'extension.

```
+-----------------------------------------------------------------------+
|                         NOYAU GÉNÉRIQUE                               |
|  - Entité "Ressource" abstraite   - Moteur de Réservation / Session   |
|  - Moteur de règles (Policy)      - Cycle de Vie & États (FSM)        |
+-----------------------------------------------------------------------+
                                   |
         +-------------------------+-------------------------+
         |                                                   |
         v                                                   v
+-------------------------------+   +-------------------------------+
|  DOMAIN CONFIG: MOBILITÉ      |   |   DOMAIN CONFIG: SALLES       |
| - Attributs: battery, lat/lon |   | - Attributs: capacity, noise  |
| - Cycle: Locked -> Unlocked   |   | - Cycle: Draft -> Confirmed   |
| - Extensions: IoT, GPS, Maps  |   | - Extensions: Outlook, Door   |
+-------------------------------+   +-------------------------------+
```

### 1. Socle Métier Abstrait (Noyau)
Le noyau dur contiendra exclusivement les concepts invariants :
* **Gestion générique des Ressources :** Déclaration d'identifiants et association d'attributs clé-valeur typés.
* **Moteur d'allocations et sessions :** Gestion abstraite des opérations `Réserver`, `Démarrer`, `Terminer`, `Annuler`.
* **Automate d'états (FSM - Finite State Machine) :** Moteur de transition d'états gérant le cycle de vie des ressources et des réservations.
* **Moteur de règles (Policy Engine) :** Évaluation dynamique de conditions d'éligibilité avant d'autoriser une action.

### 2. Éléments Définis par Configuration (Métamodèle)
Toute variabilité d'organisation ou de domaine doit être configurée sans recompilation :
* Définition des types de ressources et de leur schéma d'attributs.
* Définition des diagrammes d'états et transitions autorisées pour chaque type.
* Barèmes et structures de tarification.
* Conditions d'accès (durées min/max, délais de préavis, quotas).

### 3. Points d'Extension (Plugins & Hooks)
Les comportements nécessitant un code spécifique seront intégrés via des interfaces d'extension (*Open/Closed Principle*) :
* **Connecteurs IoT & Télémétrie :** Réception des coordonnées GPS et état de batterie (Mobilité).
* **Connecteurs Systèmes d'Accès :** Génération de codes à usage unique ou ouverture de serrures intelligentes (Salles).
* **Paiement & Facturation :** Intégration de passerelles de paiement tierces (Stripe, Flouci, etc.).
* **Notification & Synchronisation :** Webhooks pour agendas externes (Google Calendar, Microsoft Outlook).
