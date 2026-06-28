---
layout: default
nav_order: 3
title: Objectifs du projet
---

# Objectifs du projet
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Introduction

Pour ce projet, nous avons décidé de réaliser une **machine cartésienne qui dessine**, fonctionnant sur deux axes X et Y. La machine devait être capable de tracer automatiquement tout dessin vectorisable à l'aide d'un stylo piloté par des moteurs pas-à-pas contrôlés via une carte Arduino et le firmware GRBL-Servo.

---

## Contexte du Projet

Ce projet a été réalisé dans le cadre du **Makerspace d'UniLaSalle Amiens**, avec des contraintes de matériel et de temps. Nous avons dû faire face à plusieurs difficultés techniques qui ont ralenti l'avancement du projet.

| Semestre | Objectif visé | Résultat |
|----------|---------------|---------|
| S1 | Finaliser au moins un axe fonctionnel | Conception et début d'assemblage |
| S2 | Machine complète, pilotage G-code, premiers dessins | Non atteint par manque de temps |

---

## Objectifs pédagogiques

- Concevoir une machine cartésienne de A à Z en autonomie
- Maîtriser la modélisation 3D sur **Onshape**
- Concevoir un PCB sur **KiCad**
- Comprendre l'architecture Arduino + CNC Shield
- Travailler en équipe et s'organiser efficacement
- Apprendre à documenter un projet technique de manière rigoureuse

---

## Existant

Nous nous sommes appuyés sur des projets déjà réalisés dans la base de données du Makerspace, ainsi que sur les machines construites par les anciens étudiants. Nous avons adapté notre conception au matériel fourni.

---

## Cahier des Charges

### Contraintes fonctionnelles

- La machine doit pouvoir dessiner de manière fluide tout dessin vectorisable
- Déplacement précis sur les axes **X** et **Y**
- Pilotage du stylo (levée et abaissement) via servo-moteur
- Lecture de fichiers **G-code** générés depuis une image numérique

### Contraintes techniques

| Domaine | Contrainte |
|---------|------------|
| Matériaux | Utiliser obligatoirement la planche contreplaqué fournie |
| Électronique | Maîtriser la carte Arduino et ses composants |
| Logiciel | Arduino IDE, GitHub, Processing |
| Dimensions | Ne pas dépasser significativement la planche bois |
| Précision | Traits précis et soignés |

### Contraintes temporelles

{: .warning }
> **Budget-temps :** 30 heures par semestre de travail en makerspace. Ce budget s'est avéré insuffisant pour finaliser la machine dans son intégralité.
