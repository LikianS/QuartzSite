---
title: TD 8 – Une retraite enneigée
draft: false
---

## Objectif du TD
L’objectif de ce TD est d'apprendre à **créer et configurer des systèmes de particules** dans Unity pour simuler des effets environnementaux réalistes, tels que la fumée et la neige, dans une scène existante.

---

## Étapes principales : Création de la fumée de cheminée

### **1. Préparation de la scène**
- [cite_start]Récupérez le fichier `Particles.zip` et ouvrez la scène `SnowyRetreat` dans Unity[cite: 475].
- [cite_start]Vous y trouverez un paysage enneigé, des arbres et une cabane en rondins[cite: 476].

### **2. Ajout du système de particules "Fumée"**
- [cite_start]Dans le menu principal, sélectionnez **GameObject > Effets > Système de particules**[cite: 477].
- [cite_start]Un système de particules par défaut sera ajouté à la scène[cite: 478].
- [cite_start]Positionnez le point de départ du système de particules (l'émetteur) dans la cheminée de la cabine[cite: 499].
- [cite_start]**Position de la transformation** : Réglez la position sur `(1190, 30, 1180)` pour le placer au bon endroit[cite: 500].

### **3. Application du matériau de fumée**
- [cite_start]Créez un nouveau matériau[cite: 503].
- [cite_start]Appliquez-lui la texture de bouffée de fumée que vous trouverez dans le projet[cite: 503].
- [cite_start]Définissez l'ombrage du matériau sur **Particules/Additif**[cite: 504, 505].
- [cite_start]Faites glisser et déposez le nouveau matériau sur le système de particules[cite: 505]. [cite_start]Les petits points blancs se transformeront en bouffées de fumée[cite: 506].

### **4. Ajustement de la taille de la fumée (Start Size & Size over Lifetime)**
- [cite_start]Dans l’inspecteur, modifiez la **taille de particule de départ** (**Start Size**) à `5`[cite: 507].
- [cite_start]Activez l'option **Size over Lifetime** pour simuler la croissance de la fumée en s'éloignant de l'émetteur[cite: 508, 509].
- [cite_start]Ouvrez l'éditeur de particules (**Open Editor...**)[cite: 510].
- [cite_start]Activez l'option **Taille sur la durée de vie** (**Size over Lifetime**)[cite: 417].
- [cite_start]Modifiez la courbe de taille : déplacez le point de contrôle initial jusqu'à environ `0.3`[cite: 418].

### **5. Configuration de la durée de vie et de l'émetteur**
- [cite_start]Modifiez la **durée de vie initiale** (**Start Lifetime**) à `15` pour que la fumée monte plus haut avant de disparaître[cite: 421, 422].
- [cite_start]Pour condenser la fumée en un jet plus étroit, allez dans les paramètres de **Shape** (Forme) et définissez la valeur de **Angle** à `10` (l'émetteur est un cône)[cite: 423, 424].

### **6. Masquer l'effet de disparition (Color over Lifetime)**
- [cite_start]Activez l'option **Couleur sur la durée de vie** (**Color over Lifetime**)[cite: 428].
- [cite_start]Ouvrez l'éditeur de dégradé[cite: 429].
- [cite_start]Sélectionnez le pointeur en haut à droite du nuancier de couleur et définissez son **alpha sur 0** pour que la fumée s'estompe en douceur au lieu de s'arrêter brusquement[cite: 430, 431, 432].

### **7. Finalisation et densité de la fumée**
- [cite_start]Pour augmenter l'épaisseur de la fumée, augmentez la **taille de départ** (**Start Size**) à `20`[cite: 434].
- [cite_start]Ajustez la courbe **Taille sur la durée de vie** pour que la fumée commence petite à la sortie de la cheminée et grossisse plus rapidement à l'extérieur[cite: 434, 435].
- [cite_start]*Astuce : Cliquez avec le bouton droit de la souris sur la courbe pour ajouter une nouvelle touche et mieux manipuler la croissance de la taille*[cite: 435].

---

## Les Systèmes de Particules dans les Environnements Immersifs

### Création d’effets de pluie et de neige

#### Théorique :
[cite_start]Définissez les paramètres de base des systèmes de particules dans Unity pour créer des effets réalistes de neige et de pluie[cite: 457, 458].

#### Pratique :
[cite_start]Décrivez la méthodologie pour la création d'un système de particules de neige en utilisant les paramètres de base (taille, vitesse, durée de vie...)[cite: 461].

### Optimisation des Systèmes de Particules

#### Théorique :
[cite_start]Quels sont les principaux défis liés à l'optimisation des systèmes de particules dans Unity, en particulier pour les jeux destinés aux plateformes mobiles[cite: 466, 467]?

### Interaction des Particules avec l'Environnement

#### Théorique :
[cite_start]Comment les particules peuvent-elles interagir avec les éléments de l'environnement dans Unity pour créer des effets plus dynamiques et immersifs[cite: 470, 471]?

#### Pratique :
Développez un système de particules dans Unity où les particules réagissent à un objet mobile (par exemple, l'eau éclaboussant lorsqu'un personnage marche à travers une flaque). [cite_start]Comment implémentez-vous cette interaction[cite: 472, 473]?

### Personnalisation et Créativité

#### Théorique :
En quoi la personnalisation des systèmes de particules est-elle déterminante pour l'expression artistique dans le développement des jeux vidéo? [cite_start]Donnez des exemples d'effets qui peuvent être réalisés[cite: 476, 477, 478].

### Avancées Technologiques et Tendances

#### Théorique :
[cite_start]Quelles sont les dernières avancées et tendances en matière de systèmes de particules dans le développement de jeux vidéo, et comment Unity les accommode-t-il[cite: 483, 484]?