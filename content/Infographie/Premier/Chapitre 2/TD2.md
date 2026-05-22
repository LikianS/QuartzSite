---
title: TD 2 – Bruit de Perlin intégré à Unity
draft: false
author: Killian Diboues
---

## Objectif du TP

L’objectif de ce TD est de découvrir et d’expérimenter le **bruit de Perlin** dans Unity, afin de comprendre comment il peut être utilisé pour générer du contenu procédural cohérent, naturel et fluide.  
Le bruit de Perlin est une fonction mathématique souvent employée dans les jeux vidéo pour produire des effets d’irrégularité réaliste, comme les reliefs, les textures, ou les mouvements organiques.

---

## Le bruit de Perlin dans Unity

Unity propose une implémentation native du bruit de Perlin via la fonction :  
**`Mathf.PerlinNoise(xCoord, yCoord)`**  

Cette fonction retourne une valeur comprise entre **0 et 1**, correspondant à un échantillon du plan de bruit 2D.  
En modifiant les coordonnées `xCoord` et `yCoord`, on peut explorer ce plan de bruit comme une carte infinie.

**Caractéristiques principales :**
- Génère un **bruit 2D continu et lissé** (pas de transitions brutales).  
- Ne couvre **que la version 2D** du bruit de Perlin.  
- Pour un bruit 3D, il faut utiliser une implémentation externe ou la coder soi-même.

---

## Génération d’une texture procédurale

Pour produire une **texture aléatoire** et naturelle, on échantillonne le plan du bruit de Perlin à différentes coordonnées.  
Chaque pixel reçoit une intensité correspondant à la valeur du bruit, ce qui permet d’obtenir un rendu visuel cohérent.

Le principe de génération est le suivant :
1. Définir une origine (`xOrg`, `yOrg`) et un facteur d’échelle (`scale`).
2. Parcourir chaque pixel de la texture.
3. Calculer les coordonnées à échantillonner dans le plan du bruit.
4. Affecter la couleur du pixel selon la valeur du bruit (du noir au blanc).

Cette méthode est idéale pour simuler :
- des **terrains montagneux**,  
- des **textures organiques**,  
- ou encore des **motifs naturels** tels que les nuages ou la pierre.

---

## Animation avec le bruit de Perlin (Bobbling)

Le bruit de Perlin peut aussi être utilisé pour **générer des mouvements doux et naturels**.  
Un exemple courant consiste à simuler une **sphère flottante** dont la position varie lentement selon le temps.

Le principe :
- On fixe une coordonnée du bruit (par exemple `y = 0`),  
- On fait varier l’autre coordonnée (`x = Time.time * facteur`),  
- La valeur obtenue contrôle la hauteur de l’objet.

Cela produit un **mouvement sinueux et fluide**, contrairement à un déplacement purement aléatoire, qui serait saccadé.

---

## Applications du bruit de Perlin

Le bruit de Perlin est une base essentielle dans la **génération procédurale** :  
il permet de créer des environnements et comportements réalistes de manière automatisée.  
Quelques exemples d’utilisation :
- Génération de **terrains et de reliefs**.  
- Animation d’éléments naturels : **flottement**, **ondulation**, **feuilles**, **eau**, **fumée**.  
- Création de **textures procédurales**.  
- Génération de **cartes de niveaux**.

Son principal atout réside dans sa **cohérence spatiale** : les valeurs voisines dans le plan du bruit sont similaires, produisant des transitions douces.

---

## Conclusion

Ce TD a permis de comprendre comment le **bruit de Perlin** peut servir de fondation à la création de contenus procéduraux :  
- Génération de textures cohérentes,  
- Animation fluide d’objets,  
- Simulation de phénomènes naturels.  

Grâce à sa continuité et à sa flexibilité, le bruit de Perlin constitue une **pierre angulaire du réalisme procédural** dans le développement de jeux vidéo.

<video controls src="Labs2 - chap2 - Perlin - Windows, Mac, Linux - Unity 2023.2.20f1 _DX11_ 2025-12-04 23-09-13.mp4" title="Title"></video>