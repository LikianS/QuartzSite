---
title: TD 3 – Génération procédurale de villes
draft: false
author: Killian Diboues
---

## Objectif du TD
Ce TD a pour objectif de découvrir la **génération procédurale de villes** dans Unity, en utilisant le **bruit de Perlin** pour déterminer la position et la densité des bâtiments.  
La méthode est inspirée du travail de Müller et Parish et du logiciel **CityEngine**.

---

## Étapes principales de la génération de ville

### **1. Création du projet**
- Créer un nouveau projet Unity.
- Importer le package **Character Controller**.
- Ajouter une capsule et un **First Person Controller (FPC)**.
- Positionner le FPC juste au-dessus du plan et ajouter une lumière directionnelle.

---

### **2. Préparation du terrain**
- Importer le fichier `Perlin_noise.cs` dans un dossier **Plugins**.
- Créer un fichier `MakeCity.cs` basé sur le TD2 (`MakeTerrain.cs`) mais pour générer une ville.
- Tester la scène : le terrain surélevé est affiché, prêt à accueillir des bâtiments.

---

### **3. Attribution des valeurs de Perlin**
- Les valeurs du bruit de Perlin sont comprises entre **-4 et 4**.
- Ces valeurs seront utilisées pour déterminer quel type de bâtiment placer à chaque point.
- Télécharger ou importer huit modèles de bâtiments (ex. TurboSquid ou dossier partagé `buildings.zip`).

---

### **4. Création de préfabriqués**
- Ajouter chaque bâtiment à la scène.
- Ajuster l’échelle et la rotation pour chaque bâtiment.
- Créer **huit préfabriqués**, un par bâtiment, en les faisant glisser de la hiérarchie vers le projet.

---

### **5. Placement procédural des bâtiments**
- Modifier `MakeCity.cs` pour utiliser les préfabriqués et les valeurs du bruit de Perlin.  
- Les bâtiments sont instanciés sur le plan selon les valeurs calculées.  
- La valeur du Perlin détermine la densité et la taille relative des bâtiments :
  - **Valeurs élevées (blanc)** → gratte-ciel et bâtiments denses.
  - **Valeurs faibles (noir)** → petites maisons et zones moins denses.

---

### **6. Configuration dans l’inspecteur**
- Sélectionner l’objet **plan** dans la hiérarchie.
- Définir la taille du tableau **Buildings** sur 8.
- Glisser-déposer chaque préfabriqué de bâtiment dans les éléments du tableau.
- Le script s’adapte automatiquement au nombre de préfabriqués fournis.

---

### **7. Résultat final**
- Tester le gameplay.
- Une ville générée de manière procédurale apparaît sur le terrain.

---

### Remarques importantes
- Le bruit de Perlin crée des zones lisses et continues, ce qui permet de moduler la **densité urbaine**.
- Les zones blanches du Perlin correspondent aux quartiers denses avec des gratte-ciels.  
- Les zones noires correspondent aux quartiers peu peuplés avec de petites maisons.  
- Cette méthode permet de générer rapidement et automatiquement une ville relativement réaliste à partir d’un terrain.

---

## Conclusion
Ce TD illustre comment le **bruit de Perlin** peut être utilisé au-delà des terrains, pour générer **des villes procédurales** avec une densité et une variété de bâtiments réalistes.  
Cette approche offre une base solide pour créer des environnements urbains dans des jeux ou des simulations 3D.

<video controls src="Labs - chap3 - TD3 - Windows, Mac, Linux - Unity 2023.2.20f1 _DX11_ 2025-12-04 23-21-03.mp4" title="Title"></video>
![alt text](/Infographie/Chapitre-3/image-2.png)
![alt text](/Infographie/Chapitre-3/image-3.png)
![alt text](/Infographie/Chapitre-3/image-4.png)
![alt text](/Infographie/Chapitre-3/image-5.png)

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

Il faudra placer les maisons au bord des routes avec un espace pour le trottoir. De plus les bâtiments doivent etre espacés assez pour laissez une zone de jardin autour. Ajouté des props dans la rue et les jardin permettrais d’ajouté de la vie et du réalisme. Le fait de d’ajouter des angles différent a chaque virage de route permet de rendre moins monotone la ville.

## Génération procédurale de villes et optimisation de la latence

### Théorique :

Des jeux comme Daggerfall, Cities: Skylines ou Minecraft utilisent la gestion procédurales pour des villes ou des villages. Tout n’est pas chargé ou affiché en même temps. Uniquement ce que vois le joueur est chargé. De plus, plus le joueur est loin moins les objets ou bâtiment sont de bonnes qualité.

### Pratique :

On pourrait utiliser le pooling, les LOD, gérer la distance d’affichage des objets…
