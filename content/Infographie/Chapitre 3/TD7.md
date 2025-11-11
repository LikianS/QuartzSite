---
title: TD 7 – Hisser le drapeau
draft: false
---

# TD 7 : Hisser le drapeau

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
