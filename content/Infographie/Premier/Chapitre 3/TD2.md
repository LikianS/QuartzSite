---
title: TD 2 – Génération procédurale d’un terrain
draft: false
author: Killian Diboues
---


## Objectif du TD
Ce TD a pour but de découvrir la **génération procédurale de terrains** dans Unity.  
On apprend à créer un terrain réaliste à l’aide de fonctions mathématiques et du **bruit de Perlin**, permettant d’obtenir des reliefs naturels et variés.

---

## Étapes principales de la génération procédurale

### **1. Création du projet**
- Créer un nouveau projet Unity.
- Importer le package **Character Controller**.
- Ajouter une capsule et un contrôleur **First Person Controller (FPC)**.
- Placer le FPC juste au-dessus du plan et ajouter une lumière directionnelle.

---

### **2. Préparation du terrain**
- Sélectionner le plan dans la hiérarchie.
- Supprimer le **Mesh Collider** existant pour le recalculer après modification des sommets.

---

### **3. Terrain avec hauteurs aléatoires**
- Les sommets du maillage sont modifiés avec une **fonction aléatoire** pour définir la hauteur.
- Un **Mesh Collider** est ajouté après modification des sommets.
- Les normales et le volume englobant sont recalculés pour le rendu et les collisions.

Observation :  
Cette méthode crée un terrain **dentelé** si l’amplitude des valeurs aléatoires est trop élevée.

---

### **4. Terrain sinusoïdal**
- Une fonction **sinusoïdale** est appliquée pour obtenir un relief plus lisse et régulier.
- Le résultat est uniforme mais manque de réalisme.

---

### **5. Terrain avec bruit de Perlin**
- Le **bruit de Perlin** génère des variations douces et naturelles de hauteur.
- Chaque sommet du terrain est modifié selon une valeur de Perlin, ce qui produit un **paysage réaliste** similaire à celui utilisé dans Minecraft.

Observation :  
Le bruit de Perlin permet d’obtenir des **reliefs plus naturels** par rapport à une simple valeur aléatoire.

---

### **6. Comparaison des méthodes**
- **Valeurs aléatoires** : terrain très irrégulier, peu naturel.  
- **Fonction sinusoïdale** : relief lisse mais trop uniforme.  
- **Bruit de Perlin** : relief varié et naturel, idéal pour la génération procédurale de terrains.

![alt text](/Infographie/Chapitre-3/image.png)
---

## Résumé des apprentissages
Ce TD a permis de comprendre :  
- La manipulation des **sommets d’un maillage** dans Unity.  
- L’importance des **normales et des colliders** après modification du terrain.  
- L’application du **bruit de Perlin** pour créer des terrains réalistes.  
- La différence entre génération **aléatoire**, **sinusoïdale** et **Perlin**.


---

## Conclusion
Ce TD illustre l’importance de la **génération procédurale** pour créer des terrains réalistes et variés dans Unity.  
L’utilisation du **bruit de Perlin** permet de simuler des paysages naturels avec peu de code et offre une base solide pour des environnements de jeux immersifs.



## Génération procédurale d’un terrain

### Théorique : 

La génération procédurale d’un terrain dans un environnement 3D est le fait de créer un terrain de façon automatique. Selon un programme le terrain sera généré sans avoir a dessiné quoi que ce soit.

Grace a la génération procédural, on peut faire en sorte d’avoir des mondes différents à chaque partie car ils sont par principe aléatoire ou suivant une règle mathématique comme le bruit de Perlin.

## Application du bruit de Perlin dans la génération procédurale d'un terrain

### Théorique :

Le bruit de Perlin va créer des variation plus douce et moi brute que celle des valeurs aléatoires pour créer des terrain, vallée et trou exploitable pour un jeu.

## Intérêt de la génération procédurale

### Théorique :

Permet de créer rapidement un monde diversifié. Un seul algo peut créer des milliers de terrain.

## Gain de temps

### Théorique :

Permet de générer des terrains en quelques seconde ce qui contraste avec les heures de création pour un monde similaire à la main. Obtenir plusieurs proposition rapidement ainsi que faire des changements.
