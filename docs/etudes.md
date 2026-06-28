---
layout: default
nav_order: 4
title: Études et choix techniques
---

# Études et choix techniques
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Architecture de la machine

Nous avons retenu l'architecture **cartésienne XY** : une tête de dessin se déplace sur deux axes perpendiculaires pour atteindre n'importe quel point d'une zone de travail rectangulaire. D'autres architectures (CoreXY, traceur mobile, bras articulé) ont été écartées car trop complexes à réaliser dans le temps imparti.

---

## Architecture électronique

```
[PC — Processing / UGS]
         │ USB
         ▼
  [Arduino Uno + CNC Shield v3]
         │
    ┌────┴────────────────┐              │
    │                     │              │
[Driver A4988]     [Driver A4988]   [Servo FS5103B]
    │                     │              │
[Moteur pas-à-pas X] [Moteur pas-à-pas Y] [Stylo ↑↓]

        [Alimentation 12V externe]
```

---

## Composants électroniques

### Arduino Uno + CNC Shield v3

**Rôle :** L'Arduino exécute le firmware GRBL-Servo et reçoit les commandes G-code depuis le PC via USB. La CNC Shield v3 se fixe directement dessus et offre les slots pour les drivers, le bornier 12V et la pin servo.

<div style="background:#d1ecf1;border-left:4px solid #0c7abf;padding:12px 16px;margin:16px 0;font-size:13px;">
  <strong>ℹ Choix contraint :</strong> Nous avions initialement conçu un <strong>PCB custom sur KiCad</strong>. Lors des tests, le 3,3V ne passait pas dans la carte. Nous avons donc adopté Arduino Uno + CNC Shield v3.
</div>

| Caractéristique | Valeur |
|-----------------|--------|
| Microcontrôleur | ATmega328P |
| Tension de fonctionnement | 5V |
| Connexion PC | USB Type-B |
| Slots drivers | 4 (X, Y, Z, A) |
| Alimentation moteurs | 12V externe (bornier CNC Shield) |

### Drivers moteurs A4988

| Caractéristique | Valeur |
|-----------------|--------|
| Tension d'alimentation moteur | 8 à 35V |
| Courant max par phase | 2A |
| Modes micro-pas | 1 · 1/2 · 1/4 · 1/8 · 1/16 |
| Formule Vref | Vref = Imax × 8 × Rs (Rs = 0,1 Ω) |

<div style="background:#fff3cd;border-left:4px solid #ffc107;padding:12px 16px;margin:16px 0;font-size:13px;">
  <strong>⚠ Problème rencontré :</strong> Lors des premiers tests, les moteurs pas-à-pas <strong>vibraient mais ne tournaient pas</strong>. Cause : courant mal calibré sur le potentiomètre du driver A4988 et/ou câblage incorrect des bobines.
</div>

### Moteurs pas-à-pas

| Caractéristique | Valeur |
|-----------------|--------|
| Angle de pas | 1,8° (200 pas/tour) |
| Tension nominale | 12V |
| Courant par phase | ~1,5A |
| Couple de maintien | ~40 N·cm |

### Servo-moteur FeeTech FS5103B

Pilote la levée et l'abaissement du stylo via un signal PWM :

- `M3 S0` → Stylo **levé** (déplacement sans tracé)
- `M3 S255` → Stylo **abaissé** (tracé actif)

| Caractéristique | Valeur |
|-----------------|--------|
| Plage PWM | 600 à 2400 µs |
| Angle | 180° |
| Tension | 4,8 à 6V |

### Alimentation

| Caractéristique | Valeur |
|-----------------|--------|
| Tension | 12V |
| Courant recommandé | 5A minimum |
| Connexion | Bornier CNC Shield |

{: .warning }
> L'Arduino est alimenté séparément via USB. Les moteurs via le 12V externe. Ne jamais alimenter les moteurs directement depuis l'Arduino.

---

## Firmware — GRBL-Servo

GRBL-Servo est un firmware open-source transformant un Arduino Uno en contrôleur CNC. Il réaffecte la commande broche (`M3/M5`) au contrôle d'un servo-moteur pour lever/abaisser le stylo.

### Installation prévue

1. Télécharger [grbl-servo](https://github.com/vankesteren/grbl-servo)
2. Copier le dossier `grbl` dans `Documents/Arduino/libraries/`
3. Modifier `spindle_control.c` : `#define RC_SERVO_SHORT 9` et `#define RC_SERVO_LONG 34`
4. Arduino IDE → `File > Examples > grbl > grblUpload`
5. Uploader sur l'Arduino Uno

{: .warning }
> **Non réalisé :** Nous n'avons pas eu le temps d'uploader le firmware. Les problèmes PCB et moteurs ont consommé le budget-temps avant cette étape.

### Paramètres de calibration

| Paramètre | Commande | Valeur typique |
|-----------|----------|----------------|
| Steps/mm axe X | `$100=` | À calibrer |
| Steps/mm axe Y | `$101=` | À calibrer |
| Vitesse max X (mm/min) | `$110=` | 800 |
| Vitesse max Y (mm/min) | `$111=` | 800 |
| Accélération X (mm/s²) | `$120=` | 10 |

---

## Logigramme du fonctionnement global

Ce logigramme décrit le fonctionnement complet de la machine, de l'image source jusqu'au tracé physique sur la feuille.

<div style="border:1px solid #e1e4e8;border-radius:6px;padding:20px;margin:20px 0;background:#f5f6fa;overflow-x:auto;">
<svg width="100%" viewBox="0 0 680 900" role="img" style="display:block;">
  <defs><marker id="ar2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="#888" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker></defs>
  <rect x="240" y="20" width="200" height="44" rx="22" fill="#ecedf3" stroke="#888" stroke-width="1"/>
  <text x="340" y="42" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#27262b">Image source</text>
  <line x1="340" y1="64" x2="340" y2="100" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <rect x="190" y="100" width="300" height="56" rx="8" fill="#E1F5EE" stroke="#0F6E56" stroke-width="1"/>
  <text x="340" y="120" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#085041">Inkscape</text>
  <text x="340" y="142" text-anchor="middle" dominant-baseline="central" font-size="12" fill="#0F6E56">Vectorisation en SVG</text>
  <line x1="340" y1="156" x2="340" y2="196" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <rect x="190" y="196" width="300" height="56" rx="8" fill="#E1F5EE" stroke="#0F6E56" stroke-width="1"/>
  <text x="340" y="216" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#085041">Processing + Geomerative</text>
  <text x="340" y="238" text-anchor="middle" dominant-baseline="central" font-size="12" fill="#0F6E56">Génération du fichier G-code</text>
  <line x1="340" y1="252" x2="340" y2="292" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <rect x="190" y="292" width="300" height="56" rx="8" fill="#EEEDFE" stroke="#534AB7" stroke-width="1"/>
  <text x="340" y="312" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#3C3489">Universal G-Code Sender</text>
  <text x="340" y="334" text-anchor="middle" dominant-baseline="central" font-size="12" fill="#534AB7">Envoi du G-code à l'Arduino via USB</text>
  <line x1="340" y1="348" x2="340" y2="388" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <rect x="190" y="388" width="300" height="56" rx="8" fill="#EEEDFE" stroke="#534AB7" stroke-width="1"/>
  <text x="340" y="408" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#3C3489">GRBL-Servo (Arduino)</text>
  <text x="340" y="430" text-anchor="middle" dominant-baseline="central" font-size="12" fill="#534AB7">Interprétation des commandes G-code</text>
  <line x1="340" y1="444" x2="340" y2="484" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <polygon points="340,484 500,524 340,564 180,524" fill="#fff3cd" stroke="#ffc107" stroke-width="1"/>
  <text x="340" y="524" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#27262b">Type de commande ?</text>
  <line x1="180" y1="524" x2="80" y2="524" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <text x="128" y="512" text-anchor="middle" font-size="11" fill="#666">G0 / G1</text>
  <line x1="80" y1="524" x2="80" y2="580" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <rect x="16" y="580" width="160" height="56" rx="8" fill="#FAECE7" stroke="#993C1D" stroke-width="1"/>
  <text x="96" y="600" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#712B13">Déplacement XY</text>
  <text x="96" y="620" text-anchor="middle" dominant-baseline="central" font-size="12" fill="#993C1D">Moteurs pas-à-pas</text>
  <line x1="500" y1="524" x2="600" y2="524" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <text x="552" y="512" text-anchor="middle" font-size="11" fill="#666">M3 / M5</text>
  <line x1="600" y1="524" x2="600" y2="580" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <rect x="504" y="580" width="160" height="56" rx="8" fill="#FAECE7" stroke="#993C1D" stroke-width="1"/>
  <text x="584" y="600" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#712B13">Levée / abaissement</text>
  <text x="584" y="620" text-anchor="middle" dominant-baseline="central" font-size="12" fill="#993C1D">Servo-moteur stylo</text>
  <line x1="96" y1="636" x2="96" y2="680" stroke="#ccc" stroke-width="1" fill="none"/>
  <line x1="96" y1="680" x2="340" y2="680" stroke="#ccc" stroke-width="1" fill="none"/>
  <line x1="600" y1="636" x2="600" y2="680" stroke="#ccc" stroke-width="1" fill="none"/>
  <line x1="600" y1="680" x2="340" y2="680" stroke="#ccc" stroke-width="1" fill="none"/>
  <line x1="340" y1="680" x2="340" y2="716" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <polygon points="340,716 480,746 340,776 200,746" fill="#fff3cd" stroke="#ffc107" stroke-width="1"/>
  <text x="340" y="746" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#27262b">Dessin terminé ?</text>
  <line x1="200" y1="746" x2="96" y2="746" stroke="#ccc" stroke-width="1" fill="none"/>
  <line x1="96" y1="746" x2="96" y2="434" stroke="#ccc" stroke-width="1" fill="none"/>
  <line x1="96" y1="434" x2="190" y2="434" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <text x="52" y="590" text-anchor="middle" font-size="11" fill="#666">Non</text>
  <line x1="340" y1="776" x2="340" y2="816" stroke="#888" stroke-width="1.5" marker-end="url(#ar2)" fill="none"/>
  <text x="358" y="798" text-anchor="start" font-size="11" fill="#666">Oui</text>
  <rect x="240" y="816" width="200" height="44" rx="22" fill="#ecedf3" stroke="#888" stroke-width="1"/>
  <text x="340" y="838" text-anchor="middle" dominant-baseline="central" font-size="13" font-weight="600" fill="#27262b">Dessin sur feuille ✓</text>
</svg>
</div>

---

## Récapitulatif des composants

| Composant | Modèle | Qté | Rôle |
|-----------|--------|-----|------|
| Microcontrôleur | Arduino Uno | 1 | Pilotage GRBL |
| Carte d'extension | CNC Shield v3 | 1 | Interface moteurs |
| Driver moteur | A4988 | 2 | Contrôle moteurs pas-à-pas |
| Moteur pas-à-pas | — | 2 | Déplacement X et Y |
| Servo-moteur | FeeTech FS5103B | 1 | Levée/abaissement stylo |
| Alimentation | 12V / 5A | 1 | Alimentation moteurs |
| Firmware | GRBL-Servo | — | Interprétation G-code |
| Logiciel PC | UGS + Processing | — | Envoi G-code |
