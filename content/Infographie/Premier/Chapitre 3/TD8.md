---
title: TD 8 – Une retraite enneigée
draft: false
author: Killian Diboues
---

## Objectif du TD
L’objectif de ce TD est d'apprendre à **créer et configurer des systèmes de particules** dans Unity pour simuler des effets environnementaux réalistes, tels que la fumée et la neige, dans une scène existante.

---

## Étapes principales : Création de la fumée de cheminée

### **1. Préparation de la scène**
- Récupérez le fichier `Particles.zip` et ouvrez la scène **SnowyRetreat** dans Unity.
- Vous y trouverez un paysage enneigé, des arbres et une cabane en rondins.

### **2. Ajout et positionnement du système de particules "Fumée"**
- Dans le menu principal, sélectionnez **GameObject > Effets > Système de particules**.
- Un système de particules par défaut sera ajouté à la scène.
- Positionnez le point de départ du système de particules (l'émetteur) dans la cheminée de la cabine.
- **Position de la transformation** : Réglez la position sur `(1190, 30, 1180)` pour le placer au bon endroit.

---

### **3. Application du matériau de fumée**
- Créez un nouveau **matériau**.
- Appliquez-lui la texture de bouffée de fumée que vous trouverez dans le projet.
- Définissez l'ombrage du matériau sur **Particules/Additif**.
- Faites glisser et déposez le nouveau matériau sur le système de particules. Les petits points blancs se transformeront en bouffées de fumée.

---

### **4. Ajustement de la taille (Start Size & Size over Lifetime)**
- Dans l’inspecteur, modifiez la **taille de particule de départ** (**Start Size**) à `5`.
- Activez l'option **Size over Lifetime** (Taille sur la durée de vie) pour simuler la croissance de la fumée.
- Ouvrez l'éditeur de particules (**Open Editor...**).
- Modifiez la courbe de taille : déplacez le point de contrôle initial jusqu'à environ `0.3` (pour une fumée qui grossit).

### **5. Configuration de la durée de vie et de l'émetteur**
- Modifiez la **durée de vie initiale** (**Start Lifetime**) à `15` pour que la fumée monte plus haut.
- Pour condenser la fumée en un jet plus étroit, allez dans les paramètres de **Shape** (Forme) et définissez la valeur de **Angle** à `10` (l'émetteur est un cône).

---

### **6. Masquer l'effet de disparition (Color over Lifetime)**
- Activez l'option **Couleur sur la durée de vie** (**Color over Lifetime**).
- Ouvrez l'éditeur de dégradé.
- Sélectionnez le pointeur en haut à droite du nuancier de couleur et définissez son **alpha sur 0** pour que la fumée s'estompe en douceur.

### **7. Finalisation et densité de la fumée**
- Pour augmenter l'épaisseur de la fumée, augmentez la **taille de départ** (**Start Size**) à `20`.
- Ajustez la courbe **Taille sur la durée de vie** pour que la fumée commence petite et grossisse plus rapidement à l'extérieur.

<video controls src="Labs - chap8 - SnowyRetreat - Windows, Mac, Linux - Unity 2023.2.20f1_ _DX11_ 2025-12-04 23-04-04.mp4" title="Title"></video>
<video controls src="Labs - chap8 - SnowyRetreat - Windows, Mac, Linux - Unity 2023.2.20f1_ _DX11_ 2025-12-04 23-05-56.mp4" title="Title"></video>
---

## Questions Théoriques et Pratiques

### Fondamentaux des Systèmes de Particules
### Théorique :
Un système de particules est crucial pour simuler des phénomènes naturels et des effets visuels complexes qui seraient difficiles à modéliser avec des géométries traditionnelles. Ils fonctionnent en émettant de nombreuses petites entités (particules) qui peuvent être contrôlées individuellement ou en groupe pour créer des effets dynamiques tels que la fumée, le feu, la pluie, etc. Dans Unity, les systèmes de particules utilisent des émetteurs pour générer des particules avec des propriétés définies (taille, couleur, vitesse, durée de vie) et permettent une grande flexibilité grâce à divers modules de configuration.

### Pratique :
Créez un système de particules basique dans Unity qui simule une averse légère, avec des gouttes de pluie tombant verticalement ou légèrement inclinées en fonction du vent. Quels paramètres ajustez-vous pour obtenir un effet réaliste ?
Exemple des paramètres à ajuster :
- Vitesse des particules : Augmentez légèrement pour simuler la chute rapide des gouttes de pluie.
- Taille des particules : Réduisez pour refléter la finesse des gouttes d'eau.
- Transparence : Ajustez pour un effet plus réaliste de l'eau, avec une légère opacité.
- Direction et Variation : Modifiez pour simuler l'effet du vent sur la pluie.

---

### Optimisation des Systèmes de Particules
### Théorique :
Les systèmes de particules peuvent être gourmands en ressources, surtout lorsqu'ils impliquent un grand nombre de particules ou des effets complexes. Sur les plateformes mobiles, les contraintes de performance sont plus strictes, ce qui nécessite une optimisation rigoureuse. Les défis incluent la gestion du nombre de particules émises, l'utilisation efficace des shaders, et la réduction des appels de rendu. Il est crucial d'équilibrer la qualité visuelle avec les performances pour assurer une expérience utilisateur fluide.

---

### Interaction des Particules avec l'Environnement
### Théorique :
Les particules peuvent interagir avec l'environnement de plusieurs façons pour augmenter le réalisme et l'immersion. Par exemple, les particules de fumée peuvent être affectées par le vent, changeant de direction et de vitesse en fonction des conditions atmosphériques simulées. De plus, les particules peuvent réagir aux collisions avec d'autres objets, comme des gouttes de pluie éclaboussant lorsqu'elles touchent le sol ou des surfaces. L'utilisation de systèmes de particules basés sur la physique permet également de simuler des interactions plus complexes, telles que la dispersion des cendres dans l'air ou la formation de nuages de poussière lorsqu'un personnage court sur une surface sèche.

### Pratique :
Développez un système de particules dans Unity où les particules réagissent à un objet
mobile (par exemple, l'eau éclaboussant lorsqu'un personnage marche à travers une flaque).
Comment implémentez-vous cette interaction ?

---

### Personnalisation et Créativité avec les Systèmes de Particules
### Théorique :
La personnalisation des systèmes de particules est essentielle pour permettre aux développeurs et aux artistes de créer des effets visuels uniques qui renforcent l'identité artistique d'un jeu vidéo. En ajustant les paramètres tels que la taille, la couleur, la vitesse, la durée de vie et le comportement des particules, les créateurs peuvent concevoir des effets qui correspondent parfaitement à l'ambiance et au style visuel du jeu. Par exemple, un jeu fantastique pourrait utiliser des particules lumineuses et colorées pour représenter de la magie ou des sorts, tandis qu'un jeu post-apocalyptique pourrait utiliser des particules sombres et poussiéreuses pour simuler un environnement dévasté. La capacité à personnaliser ces effets permet également de raconter une histoire visuelle plus riche et immersive.

---

### Avancées Technologiques et Tendances dans les Systèmes de Particules
### Théorique :
Quelles sont les dernières avancées et tendances en matière de systèmes de particules dans le développement de jeux vidéo, et comment Unity les accommode-t-il ?

Ces dernières années, les systèmes de particules ont vu des avancées significatives grâce à l'intégration de technologies telles que le GPU computing, qui permet de gérer un plus grand nombre de particules avec une meilleure performance. Unity a intégré des fonctionnalités avancées dans son système de particules, comme le support des shaders personnalisés, les simulations basées sur la physique, et l'utilisation de la technologie VFX Graph pour créer des effets visuels complexes et interactifs. De plus, Unity facilite l'intégration de systèmes de particules avec d'autres aspects du moteur, tels que l'éclairage dynamique et les effets post-traitement, permettant ainsi aux développeurs de créer des environnements visuellement riches et immersifs.

### Pratique :
Intégrez une fonctionnalité récente des systèmes de particules de Unity dans un projet pour améliorer un effet existant ou en créer un nouveau. Quelle est cette fonctionnalité et comment l'avez-vous appliquée ?