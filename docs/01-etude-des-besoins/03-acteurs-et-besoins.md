# 3. Étude des Besoins

## 3.1. Présentation du Besoins Général

Le projet consiste à concevoir une **plateforme générique** permettant de gérer, partager et réserver différents types de ressources à partir d'une application unique.

Dans un premier temps, la plateforme prend en charge deux cas d'usage distincts :
* **La réservation de salles privées** destinées aux entretiens dans un environnement calme.
* **La location de vélos** pour les déplacements urbains.

À l'avenir, le système doit pouvoir être étendu à d'autres catégories de ressources (ordinateurs portables, vidéoprojecteurs, matériel électronique, etc.). L'objectif principal est de proposer une architecture flexible et évolutive, capable de s'adapter à des domaines variés sans nécessiter de modifications structurelles de son noyau central.

---

## 3.2. Besoins Fonctionnels

Les besoins fonctionnels décrivent les services et les comportements que la plateforme doit offrir aux utilisateurs et aux administrateurs.

* **BF01 — Gestion des ressources :** Permettre l'ajout, la consultation, la modification et la désactivation des ressources proposées à la location ou à la réservation.
* **BF02 — Gestion multi-domaines (ou multi-types) :** Supporter plusieurs catégories de ressources (vélos, salles) avec des attributs dynamiques (ex: position GPS et batterie pour un vélo, capacité et équipements pour une salle).
* **BF03 — Recherche et consultation :** Permettre aux utilisateurs de rechercher des ressources selon des critères ciblés et de consulter leurs caractéristiques, conditions et disponibilités.
* **BF04 — Gestion de la disponibilité :** Suivre les périodes de disponibilité et empêcher les conflits ou doubles réservations sur une même ressource.
* **BF05 — Réservation et location :** Assurer la prise de réservation (créneaux horaires pour les salles) et la gestion de location (début/fin de trajet pour les vélos).
* **BF06 — Suivi des réservations :** Enregistrer les demandes, consulter leur état (en cours, validée, annulée) et gérer leur cycle de vie.
* **BF07 — Gestion de la tarification :** Définir des règles de calcul de prix flexibles (basées sur la durée, le créneau ou des formules spécifiques).
* **BF08 — Gestion des règles d'utilisation :** Appliquer des contraintes spécifiques par catégorie (zones de circulation, durées maximales, horaires d'accès).
* **BF09 — Gestion du cycle de vie des ressources :** Modéliser les états d'une ressource (ex: *Disponible*, *Réservé*, *En maintenance*) et les transitions associées.
* **BF10 — Évolutivité par configuration :** Intégrer de nouvelles catégories de ressources (ex: matériel informatique) principalement via la configuration du métamodèle, sans réécrire le noyau.
* **BF11 — Support Multi-organisations (Multi-tenant) :** Permettre à plusieurs structures ou sites d'exploiter la même plateforme tout en isolant leurs configurations et leurs ressources.
* **BF12 — Notifications :** Informer les utilisateurs des événements clés liés à leurs réservations (confirmation, modification, annulation).

---

## 3.3. Besoins Non Fonctionnels (Qualités du Système)

Les besoins non fonctionnels définissent les critères de qualité de l'architecture logicielle :

* **BNF01 — Évolutivité :** Faciliter l'intégration de nouveaux types de ressources et de nouvelles règles métier.
* **BNF02 — Modularité :** Organiser le code en modules indépendants aux responsabilités bien définies (couplage faible, cohésion forte).
* **BNF03 — Maintenabilité :** Assurer un code propre et structuré pour simplifier la correction des bugs et l'évolution future.
* **BNF04 — Sécurité :** Assurer la protection des données et le contrôle des accès selon les rôles des utilisateurs.
* **BNF05 — Fiabilité :** Garantir l'intégrité des données de réservation et éviter rigoureusement les conflits d'attribution.
* **BNF06 — Ergonomie (Facilité d'utilisation) :** Proposer une interface claire et intuitive pour la recherche et la réservation rapide.
* **BNF07 — Réutilisabilité :** Mutualiser les composants communs (gestion des utilisateurs, moteur de réservation) pour éviter la duplication de code.
* **BNF08 — Extensibilité :** Respecter le principe ouvert/fermé (OCP) via des points d'extension documentés (plugins/hooks) pour préserver la stabilité du noyau.

---

## 3.4. Besoins Techniques et Objectifs d'Apprentissage

* **BT01 — Mise en pratique du Génie Logiciel :** Mettre en œuvre concrètement les concepts fondamentaux de la conception logicielle, à savoir :
  * La modélisation orientée objet et les diagrammes UML.
  * L'application rigoureuse des principes **SOLID**.
  * L'utilisation de **patrons de conception (Design Patterns)** adaptés (ex: *Strategy* pour la tarification, *State* pour les cycles de vie, *Factory* pour l'instanciation des ressources).
  * La structuration d'une architecture modulaire et extensible.
