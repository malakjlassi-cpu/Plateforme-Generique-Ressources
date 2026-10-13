# 2. Étude des Solutions Existantes

## 2.1. Objectifs de l'Étude

L'objectif de cette étude est d'analyser des applications existantes qui gèrent la réservation et le partage de ressources. Cette analyse permet de :
1. Comprendre le fonctionnement de ces systèmes.
2. Identifier les fonctionnalités communes (à intégrer dans le noyau générique).
3. Identifier les fonctionnalités spécifiques à chaque domaine (à gérer par configuration ou extension).

Dans le cadre de ce projet, nous étudions deux cas d'usage :
* **La mobilité urbaine partagée** (vélos et trottinettes).
* **La réservation de salles privées** (salles d'entretien et de réunion).

---

## 2.2. Étude du Domaine de la Mobilité Urbaine Partagée

### 2.2.1. Présentation des Solutions Studat
* **ShareBike :** Application de location de vélos en libre-service. L'utilisateur peut localiser un vélo sur une carte, scanner un code QR pour déverrouiller le vélo, l'utiliser puis terminer sa location.
* **Lime :** Service de location de vélos et trottinettes électriques. L'application permet de trouver un véhicule à proximité, de consulter les tarifs, de vérifier les zones de circulation et de déverrouiller le véhicule.

### 2.2.2. Fonctionnalités Principales Observées
* **Gestion des véhicules :** Suivi de chaque véhicule (position GPS, état de la batterie).
* **Recherche et Localisation :** Affichage des véhicules disponibles sur une carte en temps réel.
* **Début et fin de location :** Déverrouillage par code QR et verrouillage à la fin du trajet.
* **Règles de zones :** Définition de zones où le stationnement est autorisé ou interdit.
* **Tarification :** Calcul du prix basé sur les frais de déverrouillage et la durée du trajet.

### 2.2.3. Besoins Spécifiques au Domaine
Ce domaine dépend fortement de la **géolocalisation** (GPS) et du **suivi en temps réel** de l'état matériel du véhicule. Ces caractéristiques sont propres à la mobilité et ne doivent pas être intégrées directement dans le noyau de notre plateforme.

---

## 2.3. Étude du Domaine de la Réservation de Salles

### 2.3.1. Présentation des Solutions
* **Netveep :** Plateforme tunisienne de réservation d'espaces de travail et de salles de réunion. Elle permet de filtrer les salles selon la capacité et les équipements, de choisir un créneau horaire et de réserver en ligne.
* **Skedda :** Application de gestion d'espaces partagés. Elle propose un calendrier interactif, la gestion des disponibilités, l'application de règles de réservation et la synchronisation avec des agendas externes.

### 2.3.2. Fonctionnalités Principales Observées
* **Catalogue des salles :** Consultation de la liste des salles avec leurs photos et équipements.
* **Gestion des créneaux horaires :** Réservation par heures ou demi-journées.
* **Détection des conflits :** Empêcher la réservation d'une salle déjà occupée au même moment.
* **Règles d'accès :** Limitation du nombre d'heures réservables par utilisateur ou annulation sous conditions.
* **Tarification :** Facturation selon la durée de réservation ou la catégorie de la salle.

### 2.3.3. Besoins Spécifiques au Domaine
Pour des salles destinées aux entretiens, des contraintes particulières apparaissent : le niveau d'insonorisation, la confidentialité et la génération de codes d'accès temporaires pour la porte.

---

## 2.4. Comparaison des Deux Domaines

Le tableau suivant résume les ressemblances et les différences entre les deux domaines :

| Critère | Mobilité Urbaine | Réservation de Salles |
| :--- | :--- | :--- |
| **Ressource principale** | Vélo / Trottinette (Mobile) | Salle / Bureau (Fixe) |
| **Type de disponibilité** | Spatiale (Véhicule disponible à un endroit) | Temporelle (Salle libre sur un créneau) |
| **Mode d'utilisation** | Instantané (Sur place via QR Code) | Planifié à l'avance (Créneau horaire) |
| **Attributs spécifiques** | Niveau de batterie, position GPS | Capacité, équipements, insonorisation |
| **Règles principales** | Zones de stationnement autorisées | Pas de chevauchement d'horaires |
| **Tarification** | Prix fixe + Prix par minute | Prix par heure ou par jour |

---

## 2.5. Synthèse pour la Plateforme Générique

Cette étude nous permet de diviser le système en trois catégories pour respecter l'architecture demandée :

### 1. Concepts communs (À mettre dans le Noyau Générique)
* Gestion basique des ressources et des utilisateurs.
* Moteur de réservation (gestion des demandes et des statuts).
* Gestion des états de la ressource (ex: *Libre*, *Réservé*, *Indisponible*).
* Système générique de règles (validation des conditions d'utilisation).

### 2. Éléments à rendre configurables (Via le Métamodèle)
* Les types de ressources (ex: Type "Vélo" vs Type "Salle").
* Les attributs personnalisés (ex: "Batterie" pour un vélo, "Capacité" pour une salle).
* Les règles de tarification.
* Les durées minimales ou maximales de réservation.

### 3. Éléments à ajouter par extension (Modules externes)
* **Pour la mobilité :** Module de géolocalisation GPS et cartes interactives.
* **Pour les salles :** Module de synchronisation avec Google Calendar / Outlook et gestion des digicodes d'accès.
