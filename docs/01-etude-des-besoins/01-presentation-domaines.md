# 1. Présentation du projet et des domaines d’application

## 1.1. Présentation générale du projet

Le projet consiste à concevoir une **plateforme générique de partage et de réservation de ressources**. Cette plateforme doit pouvoir s’adapter à plusieurs domaines d’application sans nécessiter de modifications de son noyau principal.

L’objectif est de proposer une **architecture logicielle modulaire, configurable et extensible**, permettant de gérer :
- Différents types de ressources et leurs caractéristiques
- La disponibilité et le suivi du cycle de vie
- Les réservations et leurs règles d’utilisation

La plateforme distingue strictement :
1. **Les fonctionnalités communes** (transverses à tous les domaines)
2. **Les fonctionnalités spécifiques** (propres à chaque métier)

> **Principe clé :** Les spécificités métier sont prises en charge principalement par la **configuration** et par des **points d’extension**, conformément aux principes d'architecture logicielle.

---

## 1.2. Domaine de référence : La mobilité urbaine partagée

Le premier domaine étudié est celui de la **mobilité urbaine partagée** (mise à disposition de vélos et de trottinettes).

### Fonctionnement principal
- Consultation des ressources disponibles en temps réel.
- Sélection et réservation d'un moyen de transport.
- Utilisation sur une durée déterminée avec respect des règles associées.

### Apport pour le projet
Ce domaine constitue le **point de départ de l’analyse**. Il permet d’identifier les concepts métier fondamentaux (états des ressources, conditions de réservation, règles de tarification) pouvant être intégrés au noyau générique.

---

## 1.3. Deuxième domaine : La réservation de salles pour entretiens

Le deuxième domaine concerne la **réservation de salles privées** destinées à la réalisation d’entretiens (embauche, professionnels, académiques).

### Fonctionnement principal
- Consultation des salles disponibles et choix d'un créneau horaire.
- Prise en compte de caractéristiques spécifiques (*capacité, niveau d'insonorisation, équipements disponibles*).
- Application de règles d'accès, de durées minimales/maximales et de tarification.

### Apport pour le projet
Ce domaine présente des besoins différents de la mobilité urbaine (ex: créneaux fixes vs durée dynamique), tout en reposant sur les mêmes concepts transverses. Il permet de **valider l'adaptabilité du noyau générique** sans le modifier.

---

## 1.4. Extension potentielle : Le prêt de matériel sur un campus

Une évolution future envisagée est le **prêt de matériel universitaire** (*ordinateurs portables, vidéoprojecteurs, matériel électronique*).

- **Périmètre :** Hors périmètre initial d'étude.
- **Objectif :** Valider la réutilisation des mécanismes génériques (gestion des stocks, disponibilité, suivi du cycle de vie) dans un contexte matériel non mobile.

---

## 1.5. Synthèse et objectif de la comparaison

L’étude comparative de ces domaines vise à séparer les **invariants métier** des **spécificités locales** :

| Domaine | Élément générique (Noyau) | Élément spécifique (Extension/Config) |
| :--- | :--- | :--- |
| **Mobilité urbaine** | Gestion de disponibilité, réservation, tarification | Position géolocalisée, état de batterie, déverrouillage |
| **Réservation de salles** | Plages horaires, gestion de capacité | Niveau d'insonorisation, équipements, accès sécurisé |
| **Prêt de matériel** | Suivi du cycle de vie, calendrier | Contrôle d'état physique, numéro de série, caution |

### Conclusion architecturale
Cette démarche garantit une architecture **cohérente**, **faiblement couplée** et **ouverte à l’évolution**, capable de répondre à de nouveaux besoins sans altérer la stabilité du noyau.
