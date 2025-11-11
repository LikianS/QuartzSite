---
title: TD 3 – Génération procédurale de villes
draft: false
---

# Lab 3 – TD3 Génération procédurale de villes

## Génération procédurale de villes

### Théorique : 

La génération procédurale d’une ville est le principe de créer une ville selon une formule mathématique. On va placer nos bâtiments en fonction des paramètres entré a cette fonction et des résultat obtenue. On peut utiliser des grilles ou des fonctions mathématique tels que le bruit de Perlin Pour décider où va chaque chose. Grace a ça on peut même déterminer des quartier et des zones spécifiques.

### Pratique : 

On peut modifier la densité, la variété des bâtiments et l’offset de celle-ci.

## Intérêt de la génération procédurale de villes et l'application du bruit de Perlin

### Théorique :

Le bruit de Perlin va nous servir à créer une ville plus réaliste avec des règles. Donc on va obtenir une ville plutôt naturel avec des bâtiments espacé et répartie de façon non monotone. On peut ajuster facilement le design de notre ville a nos envies et nos besoins.

### Pratique : 

Varié l’altitude du terrain dans une ville permet d’ajouter du réalisme au monde car la terre n’est pas 100% plate. Cela apporte aussi de la diversité au gameplay avec des différents relief.

## Le réalisme dans la génération procédurale de villes

### Théorique : 

Pour qu’une ville soit réaliste, il faut essayer de ne pas avoir de patern qui se répète, ne pas avoir des quartier qui change du tout au tout d’un coup, il faut que les maisons puissent avoir des parcelles différentes, pouvoir placé des routes et des axes logique.

### Pratique :

Il faudra placer les maisons a le bord des routes avec un espace pour le trottoir. De plus les bâtiments doivent etre espacés assez pour laissez une zone de jardin autour. Ajouté des props dans la rue et les jardin permettrais d’ajouté de la vie et du réalisme. Le fait de d’ajouter des angles différent a chaque virage de route permet de rendre moins monotone la ville.

## Génération procédurale de villes et optimisation de la latence

### Théorique :

Des jeux comme Daggerfall, Cities: Skylines ou Minecraft utilisent la gestion procédurales pour des villes ou des villages. Tout n’est pas chargé ou affiché en même temps. Uniquement ce que vois le joueur est chargé. De plus, plus le joueur est loin moins les objets ou bâtiment sont de bonnes qualité.

### Pratique :

On pourrait utiliser le pooling, les LOD, gérer la distance d’affichage des objets…
