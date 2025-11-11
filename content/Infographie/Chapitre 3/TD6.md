---
title: TD 6 – Nuages volumétriques
draft: false
---

# TD 6 : Nuages volumétriques

## Principes de base des nuages volumétriques

### Théorique :

Cette technique va faire en sorte de généré un nuage par sa densité et son volume au lieu de juste affiché une image plate.

On prend un volume comme un carré et on lui applique un bruit et un shader pour avoir une forme et un effet de lumière qui passe à travers le nuage.

Traditionnellement on utilise plus un plan 2d alors que pour le volumétrique le nuage est en 3d. En fonction de l’angle le nuage volumétrique va etre différent. La lumière va etre dynamique et va se rapprocher de la vraie alors que pour le traditionnel ça va plutôt etre statique. Le cout d’un tel nuage va etre plus élevé qu’une simple image pour Unity. Et grâce a cette technologie, on peut avoir une météo dynamique ce qui n’est pas possible dans l’autre cas.

### Pratique :

Pour créer un nuage volumétrique dans Unity, je m’y prendrais comme ça :

- Créer un cube
- Générer un bruit de perlin pour la forme du nuage
- Implémenter un ray marching pour l’interaction de la lumière dans ce nuage
- Et enfin ajouter de l’animation sur ce nuage pour donner l’impression qu’il bouge et se déforme pendant son cycle de vie.

## Réalisme et optimisation

### Théorique :

Quels sont les principaux défis techniques et artistiques à surmonter pour créer des nuages
volumétriques réalistes et performants dans un jeu vidéo développé avec Unity ?

Je pense que le cout peut etre un défi assez conséquent car on peut vite avoir une génération très lourde si peu contrôlé. La diffusion de la lumière est un défi assez compliqué pour les non avertis. Et tous les déplacement et transformation de nuage au cours de son cycle de vie sont challengeais a mettre en place.

## Interaction avec l'éclairage et l'environnement

### Théorique :

Comment les nuages volumétriques interagissent-ils avec les systèmes d'éclairage
dynamique dans Unity ? Discutez de l'importance de cette interaction pour l'immersion et le
réalisme de l'environnement de jeu.

La lumière est effectivement un gros acteur dans la création d’un ciel et de nuage réussi. Comme dans la vraie vie la lumière doit etre partiellement absorbé par les nuages créant des zones d’ombre et de reflet. C’est possible grâce a la diffusion et l’absorption.

Après nous avons la couleur de la lumière qui va etre modifié par ce passage dans ces nuages, en fonction de la météo ou de l’heure de la journée. Les nuages vont pouvoir prendre différentes teintes.

Tous ceci dans un objectif de rajouter au joueur une immersion qui est des plus concrète. Non pas pour lui rappeler le monde réel mais pour ne pas le dépayser. Un joueur pour etre immerger à besoin d’etre bouleverser tous en gardant pied a terre. Donc l’envoyer dans un nouveau monde mais en lui laissant quelques repères connus. 

### Pratique :

Implémentez un système dans Unity où les nuages volumétriques changent d'apparence en
fonction de l'heure du jour, en réagissant à la position et à la couleur de la lumière solaire.
Quelles techniques utiliseriez-vous pour réaliser cela ?

## Effets météorologiques dynamiques

### Théorique :

Quel rôle les nuages volumétriques jouent-ils dans la simulation d'effets météorologiques
dynamiques dans les jeux vidéo ? Comment peuvent-ils améliorer l'expérience du joueur ?

Les nuages volumétriques sont aux cœurs de la simulation météorologiques dynamiques dans les jeux. On ne peut pas faire de dynamiques avec du statique. Grace au différente taille, couleur, dispersion…

A sa s’ajoute différents éléments comme le vent, la pluie, les éclairs et/ou de la brume qui rendent les mondes plus crédibles. Ces derniers éléments modifient les nuages.

### Pratique :

Concevez un système météorologique dynamique basique dans Unity où les nuages
volumétriques évoluent en fonction des conditions météorologiques changeantes. Comment
assureriez-vous la transition fluide entre différents types de temps ?

## Art et direction visuelle

### Théorique :

En termes d'art et de direction visuelle, comment les nuages volumétriques peuvent-ils être
utilisés pour soutenir la narration ou l'atmosphère d'un jeu vidéo ?

Un nuage n’est pas fait uniquement pour simuler le temps mais aussi pour appuyer la direction artistique et narrative du jeu. Grace a eu, on va pouvoir créer différentes ambiances comme une ambiance pleine de tension avec des nuages dense et sombre ou plus de sérénité avec des nuages légers et lumineux.

La narration avec un ciel représentant l’état émotionnelle de la situation. Guider ce joueur dans le monde en orientant le point de fuite de nuages vers une direction.
