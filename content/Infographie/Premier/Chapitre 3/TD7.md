---
title: TD 7 – Hisser le drapeau
draft: false
author: Killian Diboues
---

##  Objectif
L’objectif de ce TD est d’apprendre à **simuler un drapeau flottant au vent** à l’aide du **système de physique de tissu (Cloth Physics)** d’Unity.  
L’exercice permet de comprendre comment appliquer la gravité, les contraintes de mouvement et les forces de vent sur un maillage flexible.

---

##  Étapes principales

### **1. Création de la base du drapeau**
- Créez un **nouveau projet Unity**.
- Ajoutez un **Cube** dans la scène, puis redimensionnez-le pour qu’il ait la forme d’un **mât de drapeau**.
- Ajoutez un **Plan** et positionnez-le **au sommet du mât** : ce sera le drapeau.
- Supprimez sur ce plan :
  - Le **Mesh Renderer**
  - Le **Mesh Collider**  
   Le drapeau devient invisible temporairement.

---

### **2. Ajout du composant Cloth**
- Sélectionnez le plan (le drapeau).
- Dans l’inspecteur, cliquez sur **Ajouter un composant → Physics → Cloth**.
- Le plan devient alors un **tissu simulé dynamiquement**, soumis à la gravité et au vent.

---

### **3. Création du matériau du drapeau**
- Créez un **nouveau matériau** dans le projet.
- Assignez-lui une **texture de drapeau** (par exemple un drapeau national).
- Ajoutez ce matériau au **Skinned Mesh Renderer** du drapeau.
- Dans ce composant, définissez la propriété **Mesh** sur celle du **plan** afin que la texture s’applique correctement.

---

### **4. Définition des contraintes du tissu**
- Cliquez sur le bouton **Modifier les contraintes du tissu** dans le composant Cloth.
- Tous les **sommets du drapeau** apparaissent en **noir**.
- Avec l’outil **Paint Tool**, sélectionnez les sommets **près du mât** :
  - Ils deviendront **verts**, indiquant qu’ils sont **fixés** et ne bougeront pas.  
   Cela simule les points d’attache du drapeau au mât.

---

### **5. Test de la simulation physique**
- Lancez la scène en **Play Mode**.
- Le drapeau :
  - Tombe naturellement sous l’effet de la **gravité** ;
  - Reste accroché au mât grâce aux sommets fixés.  
   Vous pouvez ajuster la rigidité du tissu dans les paramètres du composant Cloth pour modifier sa souplesse.

---

### **6. Ajout du vent**
- Sélectionnez le plan avec le composant Cloth.
- Localisez les champs suivants :
  - **External Acceleration**
  - **Random Acceleration**

 **Définissez les valeurs suivantes :**
External Acceleration = (80, 5, 0)
Random Acceleration = (100, 5, 20)
Ces paramètres appliquent une **force constante dans la direction X** (le vent principal) tout en ajoutant des **variations aléatoires**.

---

##  Explications physiques

| Paramètre | Rôle | Exemple / Interprétation |
|------------|------|---------------------------|
| **External Acceleration** | Force constante exercée sur le tissu selon les axes X, Y, Z. | `(80, 5, 0)` crée un vent dominant vers la droite avec une légère poussée verticale. |
| **Random Acceleration** | Force variable qui perturbe le vent constant. | `(100, 5, 20)` ajoute des fluctuations pour un effet turbulent. |
| **Contraintes du tissu** | Sommets fixés au mât du drapeau. | Empêchent le drapeau de s’envoler. |

 Le vent constant (80 en X) est modifié aléatoirement par ±100, donnant des valeurs entre **−20 et +180**.  
 Cela crée un mouvement de drapeau **fluide mais irrégulier**, reproduisant le comportement naturel du tissu dans le vent.

---

## Conclusion
Ce TD illustre comment utiliser le **système de Cloth Physics** d’Unity pour animer un drapeau de manière réaliste.  
En combinant **gravité**, **contraintes de sommets**, et **forces externes aléatoires**, il est possible de créer des effets de vent dynamiques et crédibles.  
Ces techniques peuvent également être appliquées à d’autres objets souples : **rideaux, voiles, capes, draperies, etc.**

<video controls src="/Infographie/Chapitre-3/Labs---chap3---TD7---Windows,-Mac,-Linux---Unity-2023.2.20f1-_DX11_-2025-12-04-22-58-32.mp4" title="Title"></video>
<video controls src="/Infographie/Chapitre-3/Labs---chap3---TD7---Windows,-Mac,-Linux---Unity-2023.2.20f1-_DX11_-2025-12-04-22-59-04.mp4" title="Title"></video>

## Fondamentaux du Vent dans les Systèmes Physiques

### Théorique :

Le vent dans unity est une force directionnelle simulé qui agit sur les objets qui ont une physique. Cela peut se faire selon plusieurs critères. Les rigidbody et leur force ou des système spécialisé comme des wind zone, des shaders et des particules.

### Pratique :

Créez un script dans Unity qui simule l'effet du vent sur des objets légers (comme des feuilles
ou des papiers) en utilisant le système de physique. Quels composants et paramètres utilisez-
vous pour rendre cet effet réaliste ?

## Interaction entre le Vent et les Objets

### Théorique :

Les différentes considérations clés sont la masse, la densité, la surface qui va prendre le vent, la forme de l’objet, les contraintes et le coefficient de trainé.

Les objets légers vont plus facilement se faire influencer par l’effet de vent que les lourds, l’aérodynamisme comme dans la vraie vie joue un rôle important dans la prise au vent et la taille de l’objet montre la zone touchée par le vent. 

### Pratique :

Implémentez une démonstration dans Unity où différents types d'objets réagissent de
manière variée à un même effet de vent, basé sur leurs propriétés physiques. Comment
gérez-vous ces différences dans votre script ?

## Optimisation des Effets de Vent

### Théorique :

Tous rigidbody présent dans une scène sera influencé par le vent donc une mauvaise optimisation peut facilement amener des problèmes de framerate. Si l’on veut un vent réaliste, la possibilité d’utiliser des bruits procéduraux est possibles mais peut peser sur les performances. Et bien sûr avoir une bonne cohérence entre le vent et le résultat.

Avoir une zone de vent général plutôt que plusieurs effets de vent sur une zone.

Appliquer le vent que sur ce qui est proche du joueur ou désactiver les zones de vent éloigné du joueur.

## Simulation de Vent Dynamique et Changeant

### Théorique :

Comment peut-on simuler des variations dynamiques du vent (changements de direction et
de force) dans Unity pour améliorer le réalisme et l'immersion d'un environnement de jeu ?

On peut passer par un bruit procédural qui va générer un bruit réaliste et naturel, prendre des fonctions tels que les sinusoïdale, changer la direction, faire des modifications en fonction d’évènement dans le jeu, de la météo ou de l’environnement.

### Pratique :

Concevez un système dans Unity qui permet au vent de changer dynamiquement (exemple
similaire du lab) de direction et de force au fil du temps ou en réponse à des événements
spécifiques dans le jeu. Quelles sont les clés pour réussir cette mise en œuvre ?

## Vent et Direction Artistique

### Théorique :

De quelle manière les effets de vent peuvent-ils être utilisés pour soutenir la direction
artistique et la narration d'un jeu vidéo développé avec Unity ? Donnez des exemples de
comment ces effets peuvent influencer l'atmosphère ou l'émotion d'une scène.

Un vent peut faire beaucoup de chose comme dans les films. Un vent for va pouvoir montrer du chaos, une tempête ou de la tension. A l’inverse une douce brise montre un moment réconfortant et calmant.

Un changement d’intensité dans le vent peut aussi montre un changement d’intensité dans le jeu comme le passage de la zone normal a une zone de boss.

Dans l’immersion en général, le vent active beaucoup de chose autant visuellement qu’au niveau du son. On va etre beaucoup plus immerger dans une foret où les arbres et feuilles bouge que dans un foret inerte.

### Pratique :

Créez une scène simple dans Unity où l'effet de vent joue un rôle clé dans l'établissement de
l'ambiance ou de la tonalité de l'histoire. Comment coordonnez-vous l'effet de vent avec
d'autres éléments visuels et sonores pour renforcer cette ambiance ?
