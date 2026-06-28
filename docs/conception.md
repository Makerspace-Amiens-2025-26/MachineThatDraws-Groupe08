---
layout: default
nav_order: 5
title: Conception et prototypage
---

# Conception et prototypage
{: .no_toc }

<details open markdown="block">
  <summary>Sommaire</summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Logiciel de modélisation

Toutes les pièces ont été modélisées sur **Onshape**, un logiciel de CAO en ligne. Il nous a permis de concevoir les pièces, les assembler virtuellement et exporter les fichiers STL pour l'impression 3D.

[Voir le modèle complet sur Onshape](https://cad.onshape.com/documents/e895d617fa85e3b715768f5c/v/efa0b0bc12fe522d979fde8b/e/5e27dc6e7103b2e81bcf4c4e){: .btn .btn-primary .mb-4 }

<iframe height="500" width="100%" src="https://modelembedder.net/embed?did=e895d617fa85e3b715768f5c&wvm=v&wvmid=efa0b0bc12fe522d979fde8b&eid=5e27dc6e7103b2e81bcf4c4e&elementType=ASSEMBLY" frameborder="0" allowfullscreen style="display:block;margin:16px 0;border:1px solid #e1e4e8;"></iframe>

---

## Vue d'ensemble de la machine

- Châssis en **contreplaqué perforé** fourni par le makerspace
- **4 pieds de coin** surélevant la planche pour le passage des courroies
- **Tiges lisses en acier** pour guidage linéaire sur chaque axe
- **2 moteurs pas-à-pas** pour le déplacement X et Y
- **1 servo-moteur FS5103B** pour la levée/abaissement du stylo
- Toutes les pièces de support **imprimées en PLA**

---

## Description des pièces

### Support moteur

[Voir le STL sur GitHub](https://github.com/Makerspace-Amiens-2025-26/MachineThatDraws-Groupe08/blob/main/docs/images/support%20moteur.stl){: .btn .mb-3 }

**Rôle :** Logement et fixation d'un moteur pas-à-pas sur le châssis. Boîte ouverte sur une face avec trous ovales latéraux, ouverture frontale pour l'axe et la poulie, semelle percée pour vissage.

**Matériau :** PLA · **Remplissage :** 30 %

<div style="background:#fff3cd;border-left:4px solid #ffc107;padding:12px 16px;margin:12px 0;font-size:13px;">
  <strong>⚠ Problème rencontré :</strong> La première version présentait des trous de fixation trop petits pour la visserie M3. Corrigé avec un jeu de +0,3 mm dans Onshape et réimprimé.
</div>

---

### Support moteur simple

[Voir le STL sur GitHub](https://github.com/Makerspace-Amiens-2025-26/MachineThatDraws-Groupe08/blob/main/docs/images/support%20moteur%20simple.stl){: .btn .mb-3 }

**Rôle :** Fixation compacte avec rainure centrale pour l'axe moteur et épaulement de butée de positionnement.

**Matériau :** PLA · **Remplissage :** 30 %

---

### Support moteur double

[Voir le STL sur GitHub](https://github.com/Makerspace-Amiens-2025-26/MachineThatDraws-Groupe08/blob/main/docs/images/support%20moteur%20double.stl){: .btn .mb-3 }

**Rôle :** Double appui latéral pour les extrémités soumises à plus d'efforts. Grande platine verticale avec deux bras horizontaux.

**Matériau :** PLA · **Remplissage :** 35 %

---

### Support double poulie

[Voir le STL sur GitHub](https://github.com/Makerspace-Amiens-2025-26/MachineThatDraws-Groupe08/blob/main/docs/images/support%20double%20poulie.stl){: .btn .mb-3 }

**Rôle :** Maintien des poulies de renvoi à l'extrémité opposée aux moteurs. Accueille deux poulies folles sous tension de courroie GT2.

**Matériau :** PLA · **Remplissage :** 25 %

---

### Support barre métal

[Voir le STL sur GitHub](https://github.com/Makerspace-Amiens-2025-26/MachineThatDraws-Groupe08/blob/main/docs/images/support%20barre%20me%CC%81tal.stl){: .btn .mb-3 }

**Rôle :** Maintien des tiges lisses en acier servant de rails. Logement cylindrique, platine de fixation et ergot de positionnement pour alignement précis.

**Matériau :** PLA · **Remplissage :** 25 %

<div style="background:#fff3cd;border-left:4px solid #ffc107;padding:12px 16px;margin:12px 0;font-size:13px;">
  <strong>⚠ Problème rencontré :</strong> Le logement cylindrique était trop serré pour les tiges acier. Corrigé avec +0,2 mm de jeu dans Onshape et réimprimé.
</div>

---

### Adaptateur

[Voir le STL sur GitHub](https://github.com/Makerspace-Amiens-2025-26/MachineThatDraws-Groupe08/blob/main/docs/images/adaptateur.stl){: .btn .mb-3 }

**Rôle :** Liaison entre l'axe du moteur et la poulie GT2. Solidarise la poulie sans jeu, transmet le couple sans glissement.

**Matériau :** PLA · **Remplissage :** 100 % (pièce soumise à torsion)

---

## Récapitulatif des pièces

| Pièce | Quantité | Remplissage | Fonction |
|-------|----------|-------------|---------|
| Support moteur | 1 | 30 % | Logement moteur axe X |
| Support moteur simple | 1 | 30 % | Fixation moteur axe Y |
| Support moteur double | 1 | 35 % | Fixation renforcée |
| Support double poulie | 2 | 25 % | Renvoi de courroie |
| Support barre métal | 4 | 25 % | Maintien tiges guidage |
| Adaptateur | 2 | 100 % | Liaison axe/poulie |
| Pieds de coin | 4 | 20 % | Rigidité châssis |

---

## Démarche de conception

1. **Analyse du cahier des charges** — Contraintes fonctionnelles, dimensionnelles et budgétaires
2. **Benchmark** — Étude de machines cartésiennes open-source
3. **Modélisation Onshape** — Conception pièce par pièce, vérification des interférences
4. **Itérations** — Plusieurs pièces réimprimées suite aux erreurs dimensionnelles
