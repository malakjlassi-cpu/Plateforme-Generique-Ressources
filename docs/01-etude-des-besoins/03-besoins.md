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
* ==>**Principe SOLID : S.**
Pourquoi : une partie du système s'occupe uniquement de gérer les ressources, sans faire les prix ni les réservations. Si le prix change, on ne touche pas à cette partie.

* **BF02 — Gestion multi-domaines (ou multi-types) :** Supporter plusieurs catégories de ressources (vélos, salles) avec des attributs dynamiques (ex: position GPS et batterie pour un vélo, capacité et équipements pour une salle).
* ==>**Principe SOLID : O (ouvert/fermé).**
Pourquoi : « Vélo » et « Salle » sont des données de configuration, pas des classes du noyau. Pour ajouter « Ordinateur portable », on ajoute une configuration (type + attributs) et on ne modifie pas le noyau.

* **BF03 — Recherche et consultation :** Permettre aux utilisateurs de rechercher des ressources selon des critères ciblés et de consulter leurs caractéristiques, conditions et disponibilités.
* ==>**Principe SOLID : I.**
Pourquoi : celui qui cherche utilise une petite interface de recherche qui contient seulement ce dont il a besoin (critères, résultats), sans les fonctions de modification des ressources.

* **BF04 — Gestion de la disponibilité :** Suivre les périodes de disponibilité et empêcher les conflits ou doubles réservations sur une même ressource.
* ==>**Principe SOLID : S.**
Pourquoi : vérifier la disponibilité est une responsabilité à part, confiée à une seule partie du système. Si la règle de conflit change, on modifie un seul endroit, sans toucher à la réservation ni à la tarification.

* **BF05 — Réservation et location :** Assurer la prise de réservation (créneaux horaires pour les salles) et la gestion de location (début/fin de trajet pour les vélos).
* ==>**Principe SOLID : S.**
Pourquoi : réserver un créneau (salle) et utiliser une ressource tout de suite (vélo) n'ont pas les mêmes raisons de changer. On sépare la partie qui gère la réservation de celle qui gère le début et la fin d'utilisation : si la règle des créneaux change, la location d'un vélo n'est pas touchée.

* **BF06 — Suivi des réservations :** Enregistrer les demandes, consulter leur état (en cours, validée, annulée) et gérer leur cycle de vie.
* ==>**Principe SOLID : O.**
Pourquoi : les états d'une réservation sont gérés comme des données et non écrits en dur dans le code. Si on veut ajouter l'état « en attente de paiement », on ajoute une configuration, sans modifier le noyau.

* **BF07 — Gestion de la tarification :** Définir des règles de calcul de prix flexibles (basées sur la durée, le créneau ou des formules spécifiques).
* ==>**Principe SOLID : O.**
Pourquoi : chaque règle de prix (durée, créneau, horaire…) est une petite règle qu'on ajoute à une liste, et le calculateur ne change pas. Pour ajouter une réduction « abonné », on ajoute une règle, sans modifier ce qui existe.

* **BF08 — Gestion des règles d'utilisation :** Appliquer des contraintes spécifiques par catégorie (zones de circulation, durées maximales, horaires d'accès).
* ==>**Principe SOLID : L.**
Pourquoi : zone, durée maximale et horaire sont des règles différentes, mais toutes répondent à la même question : « l'utilisation est-elle autorisée ou refusée ? ». On peut donc remplacer ou ajouter une règle sans que le reste du système change, car toutes respectent le même contrat.

* **BF09 — Gestion du cycle de vie des ressources :** Modéliser les états d'une ressource (ex: *Disponible*, *Réservé*, *En maintenance*) et les transitions associées.
* ==>**Principe SOLID : O.**
Pourquoi : les états et les transitions sont des données de configuration, interprétées par un mécanisme générique, et non une classe de code par état. Pour ajouter l'état « Nettoyage » aux salles, on modifie la configuration et on ne modifie pas le noyau.

* **BF10 — Évolutivité par configuration :** Intégrer de nouvelles catégories de ressources (ex: matériel informatique) principalement via la configuration du métamodèle, sans réécrire le noyau.
* ==>**Principe SOLID : O.**
Pourquoi : c'est le principe ouvert/fermé appliqué à toute la plateforme : le noyau est fermé à la modification et ouvert à l'ajout. Intégrer le matériel informatique se fait en ajoutant une configuration, comme pour les vélos et les salles.

* **BF11 — Support Multi-organisations (Multi-tenant) :** Permettre à plusieurs structures ou sites d'exploiter la même plateforme tout en isolant leurs configurations et leurs ressources.
* ==>**Principe SOLID : S.**
Pourquoi : la responsabilité d'isoler les données de chaque organisation est confiée à une partie dédiée, au lieu d'être dispersée dans chaque fonction. Une organisation ne voit jamais la configuration ni les ressources d'une autre.

* **BF12 — Notifications :** Informer les utilisateurs des événements clés liés à leurs réservations (confirmation, modification, annulation).
* ==>**Principe SOLID : D.**
Pourquoi : le noyau demande « envoyer une notification » à une abstraction, sans savoir si c'est un e-mail ou un SMS. Changer de canal se fait sans modifier le noyau.

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
