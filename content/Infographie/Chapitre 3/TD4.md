---
title: TD 4 – Effets de post-traitement
draft: false
---

# Lab 3 – TD 4 : Effets de post-traitement

## Compréhension des fondamentaux :

Les effets de post traitement sont des effets visuel qui vont s’appliquer a la caméra dans unity. Ces effets vont modifier visuellement certain aspect du jeu. On peut ajouter différent effets tels que le bloom, l’étiquette, le flou… Ces effets servent a guider le joueur, peuvent créer une ambiance et renforce l’immersion du joueur. A prendre en compte que ce n’est que des modification visuel et non sur le terrain ou les objets.

## Gestion des ressources :

Pour optimiser, il faut faire attention au nombre d’effet actif a la fois. Certain effets sont plus gourmand que d’autre. On peut faire en sorte de définir des global volume qui vont faire en sorte d’avoir un post processing qu’a certain endroit de la map.

## Application pratique spécifique :

Créer un Volume global.

Ajouter l’effet Motion Blur.

Régler l’intensité du flou et la qualité.

S’assurer que la caméra a le post-processing activé.

Cela va ajouter un flou directionnel, c’est-à-dire un flou qui va etre présent lors des mouvements du joueur. Il permet d’augmenter le réalisme, la vitesse et différent effet.

## Intégration avancée :

Scene dans Unity

## Nouveautés et tendances :

Un effet de post-traitement que je considère particulièrement prometteur est l’“Eye Adaptation”, aussi appelé Auto Exposure. Cet effet ajuste dynamiquement l’exposition de la caméra selon la luminosité de la scène, simulant la manière dont l’œil humain s’adapte à la lumière. Par exemple, lorsqu’un joueur sort d’un bâtiment sombre vers un environnement très lumineux, l’écran devient temporairement éblouissant avant de s’ajuster. Ce type d’effet renforce énormément le réalisme et l’immersion, surtout dans les jeux à environnement ouvert ou à changement climatique dynamique. Techniquement, il est déjà disponible dans Unity HDRP, mais pourrait être encore amélioré en combinant des systèmes de machine learning pour une adaptation plus naturelle, prenant en compte la direction du regard ou la zone d’intérêt du joueur. Ce genre d’évolution rapproche encore plus le rendu des jeux de la perception visuelle humaine, tout en restant performant sur les plateformes modernes.
