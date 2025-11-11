---
title: TD 5 – Dômes célestes
draft: false
---

# Lab 3 – TD 5 : Dômes célestes

## Skydome

### Théorique:

Un skydome est une sphère placé autour de la scène qui permet d’avoir un ciel. Cette sphère est toujours autour du joueur. On peut simuler plein de chose dedans telles que des nuages et le soleil.

Sa permet de créer une ambiance plus réaliste avec un ajout de cycle jour/nuit, une ambiance lumineuse et des éléments atmosphérique tels que la brume et des couches de soleil.

### Pratique :

Étapes de création d’un Skydome dans Unity

- Création du modèle de base
  - Créez une sphère 3D.
  - Inversez les normales de la sphère pour que la texture soit visible de l’intérieur.
  - Agrandissez la sphère pour qu’elle englobe toute la scène.
- Application d’un matériau
  - Créez un nouveau matériau Unity.
  - Assignez-lui un shader adapté, par exemple :
    - Skybox/Panoramic ou Unlit/Texture,
    - ou un shader personnalisé pour gérer les transitions jour/nuit, les nuages ou la lumière dynamique.
  - Ajoutez une texture du ciel.
- Configuration dans la scène
  - Placez le Skydome centré sur le joueur (ou liez-le au Player pour qu’il se déplace avec lui).
  - Désactivez la collision et assurez-vous que le Skydome est toujours derrière les autres éléments

### Considérations importantes pour une bonne intégration

- Échelle et position : Le Skydome doit être suffisamment grand pour ne jamais être atteint par le joueur ni visible à ses limites. Il doit suivre la position du joueur pour éviter tout effet de “bord du monde”.
- Performance : Utiliser des shaders Unlit ou optimisés, car le Skydome n’a pas besoin d’éclairage ni d’ombres. Éviter des textures trop lourdes, surtout sur mobile.
- Cohérence visuelle : Adapter la couleur du ciel et de la lumière directionnelle pour qu’elles correspondent. Ajuster la teinte et l’intensité selon la scène (par exemple, un ciel plus rouge au coucher du soleil).
- Compatibilité multi-cartes : Créer un système paramétrable (ou un prefab) où le même Skydome peut changer selon :
  - la météo (ensoleillé, orage, nuit),
  - le thème de la carte (désert, neige, forêt),
  - ou la progression du joueur.

Ainsi, toutes les cartes peuvent partager le même modèle de Skydome, mais avec des paramètres personnalisés.

## Turbidité

### Théorique:

Sa représente la quantité  de particule en suspension dans l’air. Sa contrôle la diffusion de la lumière et la teinte du ciel. Une faible turbidité donne un ciel clair et bleu avec une lumière intense, tandis qu’une forte turbidité produit un ciel jaunâtre, brumeux ou voilé. 

La simulation de la turbidité est importante pour reproduire des conditions atmosphériques réalistes et renforcer l’immersion du joueur.

### Pratique :

Implémentez un script dans Unity qui simule différents niveaux de turbidité atmosphérique (sans l’utilisation du skydome). Quels paramètres exposeriez-vous au concepteur pour ajuster l'effet, et comment ces changements seraient-ils représentés visuellement dans le jeu ?

## Diffusion de Rayleigh

### Théorique:

La diffusion de Rayleigh est le phénomène physique qui explique pourquoi le ciel est bleu. C’est le passage de la lumière dans des micro particule d’eau. Comme les longueur d’onde plus courte sont celle qui sont plus diffusé la couleur vu est le bleu/violet. Grace a la compréhension de cette diffusion, on peut créer des ciels plus réalistes.

### Pratique:

Concevez un système dans Unity qui ajuste dynamiquement l'effet de la diffusion de Rayleigh en fonction de l'altitude du joueur dans le jeu (sans l’utilisation du skydome). Comment cette altitude influencerait-elle la couleur du ciel et la visibilité à différentes heures de la journée? Quelles formules ou données utiliseriez-vous pour modéliser cet effet?

## Théorie de Mie

### Théorie:

La théorie de Mie explique comment la lumière est diffusée par des particules plus grosses, comme la poussière, la brume ou les gouttes d’eau. Contrairement à Rayleigh (qui rend le ciel bleu), Mie diffuse la lumière de façon plus uniforme, ce qui crée un halo lumineux autour du soleil ou un effet de brume atmosphérique. Dans les jeux, ça rend le ciel et l’atmosphère plus réalistes, surtout quand il y a de la brume ou des nuages.

### Pratique

Développez une fonctionnalité dans Unity qui utilise la théorie de Mie pour simuler l'effet de halo autour du soleil dû à la diffusion de particules plus grandes, comme la brume ou les nuages (sans l’utilisation du skydome). Quels paramètres ajustables fourniriez-vous pour contrôler l'intensité et la taille du halo sous différentes conditions atmosphériques?

## Luminosité de la lumière du soleil

### Théorique:

La luminosité du soleil peut changer en fonction de différent paramètres. Elle peut changer  en fonction de l’heure, de l’angle du soleil et du temps.

Sur unity, on peut ajuster plusieurs paramètres sur une lumière directionnel comme la couleur et l’intensité. Avec de bon paramètres, on peut simuler une transition du matin au soir réaliste et immersive.

### Pratique

Créez un script Unity qui ajuste dynamiquement la luminosité et la couleur de la lumière du soleil dans un environnement de jeu en fonction de l'heure de la journée (sans l’utilisation du skydome). Quelles techniques utiliseriez-vous pour assurer une transition fluide entre ces états ?

## Objectifs d'un Skydome

### Théorique:

Sa sert à créer l’illusion d’un ciel pour le jeu, cela ajoute une atmosphère et des lumières réalistes. Cela est possible aussi grâce a la météo et l’ambiance selon l’heure. 

### Pratique

Concevez un système dans Unity où le skydome reflète la progression narrative du jeu ou les actions du joueur. Comment vous assurer que ce système s'intègre bien à plusieurs missions et cartes du jeu ?

## Avantages et inconvénients des skydomes

### Théorique:

C’est léger et simple a mettre en place. On peut facilement et rapidement avoir quelque chose de jolie. Peut paraitre peu réaliste si trop statique. Sur des grandes cartes, le skydome doit etre centré sur le joueur. On ne peut pas faire de chose trop complexe avec un skydome. On peut utiliser un ciel procédural pour les jeux plutôt réaliste avec des projets plutôt en URP et HDRP. Il y a aussi les ciel volumétrique quand on veut des visuels très réaliste comme du brouillard.

### Pratique

Proposez une solution dans Unity pour surmonter l'un des principaux inconvénients des skydômes que vous avez identifiés. Comment cette solution pourrait-elle être mise en œuvre dans un jeu comportant de vastes environnements extérieurs ?

## Intégration des skydomes dans les jeux à missions et cartes multiples

### Théorique:

Dans un jeu comportant plusieurs missions ou zones, le skydome peut être utilisé comme un outil narratif et immersif. Il permet d’adapter le ciel et l’ambiance à chaque environnement ou événement du scénario.

Par exemple :
- Un ciel rouge et orageux pendant une bataille.
- Une nuit étoilée pendant une mission d’infiltration.
- Un ciel doré au lever du soleil pour symboliser la victoire.

Pour une intégration fluide :
- On peut utiliser plusieurs profils de skydome et les changer dynamiquement selon la mission.
- Les transitions doivent être progressives (interpolation de la couleur, de l’intensité du soleil, de la turbidité…).
- Il faut veiller à optimiser le chargement des textures et shaders pour ne pas ralentir le jeu.

Ainsi, le skydome devient un véritable outil scénographique, soutenant la narration et le rythme du jeu tout en maintenant la performance.

### Pratique

Décrivez une approche pour changer dynamiquement l'apparence et les effets du skydome dans Unity en fonction de l'emplacement du joueur dans le monde du jeu ou de l'accomplissement de missions spécifiques. Quelles sont les considérations nécessaires pour maintenir les performances et la cohérence dans des environnements variés ?
