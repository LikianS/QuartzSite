---
title: TD 3 – Générateur de grottes procédural
draft: false
---

## 1. Objectif du TD

L’objectif de ce TD est d’implémenter un **générateur de grottes procédural** dans Unity, basé sur les **automates cellulaires** pour la génération de la carte et un **système de rendu en maillage 3D**.  
Le système repose sur deux composants principaux :
- `CaveGenerator` : responsable de la **logique de génération** et du stockage de la carte abstraite.
- `MeshGenerator` : responsable du **rendu visuel 3D** de la carte générée.

---

## 2. Le composant CaveGenerator

### Rôle
Ce composant gère la création de la carte des grottes sous forme de grille de cellules (mur ou espace vide), et applique des règles d’automate cellulaire pour faire évoluer la carte au fil des itérations.

### Attributs exposés
- **Width / Height** : dimensions de la carte (nombre de cellules).
- **Seed / UseRandomSeed** : paramètres de génération aléatoire permettant de reproduire ou de varier la carte.
- **Clean (booléen)** : permet d’activer un second ensemble de règles pour générer des grottes plus lisses.

### Fonction `InitializeRandomGrid()`
Cette fonction initialise la carte avec des cellules aléatoires :
- Les bords sont toujours des murs (valeur `1`).
- Une ligne centrale est laissée vide pour assurer une connexion entre les zones.
- Le reste des cellules est aléatoirement défini comme mur (`1`) ou vide (`0`), selon la graine choisie.

---

### Fonctions utilitaires

#### `GetSurroundingWallCount()`
Compte le nombre de murs autour d’une cellule donnée dans un rayon spécifié.

#### `isWall()`
Retourne `true` si une coordonnée correspond à un mur ou si elle est hors des limites de la carte.

---

### Fonction `CellularAutomata(clean = false)`
C’est le cœur de la génération procédurale.  
Cette fonction applique les **règles de l’automate cellulaire (AC)** sur la carte, créant progressivement des formes cohérentes de grottes.

Deux ensembles de règles sont utilisés :
- **Mode normal** : crée des structures complexes et variées.
- **Mode clean** : applique des règles supplémentaires pour lisser les parois et supprimer les grandes cavités.

**Touches de contrôle :**
- `Espace` : applique les règles de l’AC (version “clean”).
- `G` : applique la version normale.
- `N` : régénère complètement la carte.

---

## 3. Fonction `DrawCaveMesh()`

Cette fonction appelle le `MeshGenerator` associé pour transformer la carte abstraite (tableau d’entiers) en un **maillage 3D visible dans la scène Unity**.

---

## 4. Le composant MeshGenerator

### Description
`MeshGenerator` est une **classe abstraite** définissant la structure minimale pour tout générateur de maillage.  
Elle contient une seule méthode à implémenter :
- `GenerateMesh(int[,] map, float squareSize)`

Cela permet de créer plusieurs versions du générateur selon la méthode de rendu souhaitée.

---

## 5. Rendu de base avec WallGenerator

### Fonctionnement
`WallGenerator` hérite de `MeshGenerator` et instancie un **cube 3D (prefab)** à chaque position de cellule représentant un mur.  
Ainsi, une carte de 200x200 pourrait générer jusqu’à **40 000 cubes**, ce qui peut ralentir Unity.

### Étapes principales
1. **Suppression des anciens murs** pour réinitialiser la scène.  
2. **Création d’un cube par cellule murale**.  
3. **Fusion des cubes** en un seul maillage avec `CombineMeshes` pour optimiser les performances.  
4. **Nettoyage mémoire** : suppression des objets cubes individuels après la fusion.

---

## 6. Optimisation avec Marching Squares

Pour résoudre le problème du grand nombre de sommets et d’objets, on introduit **l’algorithme Marching Squares**, inspiré d’un tutoriel officiel Unity.

### Avantages :
- **Rendu plus fluide** : les murs sont arrondis et continus.  
- **Moins de sommets** : le maillage est plus léger et rapide à afficher.  
- **Facile à intégrer** : il suffit de remplacer `WallGenerator` par `MarchingCubesGenerator`.

---

## 7. Configuration de la scène Unity

1. Créer un **GameObject “CaveGen”** avec un `MeshRenderer` et un `MeshFilter`.  
2. Ajouter les composants **CaveGenerator** et **WallGenerator (ou MarchingCubesGenerator)**.  
3. Lier le **prefab wallCube** au paramètre public de WallGenerator.  
4. Lancer la scène et observer la génération aléatoire et l’évolution des grottes.

---

## 8. Conclusion

Ce TD illustre le **pouvoir de la génération procédurale** dans les jeux vidéo.  
Grâce à un simple automate cellulaire et quelques itérations, il est possible de produire des **environnements complexes, variés et immersifs**.  
L’ajout du **rendu Marching Squares** améliore encore les performances et la qualité visuelle, préparant la base pour des systèmes plus avancés de **génération de mondes dynamiques**.
"""

<video controls src="Labs2 - chap2 - CaveScene-Simple - Windows, Mac, Linux - Unity 2023.2.20f1 _DX11_ 2025-12-04 23-15-37.mp4" title="Title"></video>
<video controls src="Labs2 - chap2 - CaveScene - Windows, Mac, Linux - Unity 2023.2.20f1 _DX11_ 2025-12-04 23-17-37.mp4" title="Title"></video>