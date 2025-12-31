---
title: TD 1 – Création d’un terrain dans Unity
draft: false
author: Killian Diboues
---

## Objectif du TD

L’objectif de ce TD est de découvrir les **fonctionnalités de base de l’éditeur de terrains d’Unity**.  
À travers la création d’un paysage inspiré du **Mont Fuji**, on apprend à sculpter, texturer et enrichir un terrain à l’aide des outils intégrés de Unity.

---

## Étapes principales de la création du terrain

### **1. Création du projet et ajout d’un terrain**
- Démarrer Unity et créer un nouveau projet 3D.
- Ajouter un terrain via : **GameObject → 3D Object → Terrain**.  
  → Un large plan plat apparaît dans la scène.

### **2. Utilisation des outils de l’éditeur de terrain**
Le panneau **Terrain** contient plusieurs outils principaux :

| N° | Outil | Description |
|----|--------|-------------|
| 1 | **Outil multifonction** | Permet de **peindre des textures**, **sculpter le relief**, **ajuster la hauteur** et **lisser** le terrain. |
| 2 | **Outil d’arbres** | Sert à placer des **arbres** à la surface du terrain. |
| 3 | **Outil de détails** | Permet d’ajouter des **plantes, fleurs et roches**. |
| 4 | **Paramètres du terrain** | Contrôle les **niveaux de détail (LOD)** et le **rendu des arbres** (billboards ou 3D). |

---

### **3. Sculpture du relief**
- Sélectionner l’outil 1 → « Élever ou abaisser le terrain ».
- Ajuster la **taille du pinceau**, l’**opacité** et la **force** pour contrôler la sculpture.
- **Clic gauche** : soulève le terrain.  
- **MAJ + clic gauche** : abaisse le terrain.


---

### **4. Texturage du terrain**
Une fois le relief terminé, on applique des **textures sans couture** pour donner du réalisme au terrain.

1. Sélectionner l’outil 1 → « Texture de peinture ».
2. Cliquer sur **Modifier les textures → Ajouter une texture**.
3. Importer une image depuis l’Asset Store ou depuis [holistic3d.com/resources](http://www.holistic3d.com/resources/).
4. Ajuster la **taille de tuile** et l’**opacité** du pinceau.

La première texture ajoutée recouvre tout le terrain : il est donc conseillé de choisir une **texture dominante** (ex. herbe ou terre).

---

### **5. Ajout de végétation (arbres, herbe, fleurs)**
- Utiliser l’**outil 2** pour peindre des arbres.  
- Télécharger le pack **Extra Terrain Assets** ou un pack gratuit depuis [holistic3d.com/resources](https://holistic3d.com/resources/).  
- Ajouter les arbres au pinceau, puis les peindre selon la répartition naturelle.

Observation :  
Les arbres sont plus denses près du lac et disparaissent en altitude, simulant un **écosystème réaliste**.
  
Ensuite :
- Utiliser l’**outil 3** pour ajouter **herbe et fleurs**.
- Peindre autour du lac pour créer une **zone de transition** entre l’eau et la terre.

---

### **6. Ajout de l’eau**
Deux options possibles :
1. **Créer un shader d’eau personnalisé** (via un tutoriel).  
2. **Importer un prefab d’eau** depuis l’Asset Store, comme **Stylized Water Texture**.

Une fois importé :
- Glisser le prefab dans la scène.
- Le **redimensionner** et le **placer** dans la zone du lac.

---

### **7. Finalisation du terrain**
- Supprimer la photo de référence.
- Ajouter un **Prefab FirstPersonController** pour explorer la scène.  
- Lancer le mode **Play** pour découvrir le rendu final.


---

## Résumé des apprentissages

Ce TD permet de maîtriser les bases de la **création d’environnements naturels dans Unity** :  
- Manipulation du **terrain editor**,  
- Sculpter des **reliefs réalistes**,  
- Appliquer des **textures procédurales**,  
- Ajouter des **arbres, fleurs et herbes**,  
- Intégrer de l’**eau** pour compléter le paysage.

Grâce à ces outils, il est possible de **créer rapidement des environnements immersifs** adaptés à n’importe quel type de jeu.


## Conclusion

Ce premier TD a posé les bases de la **création de mondes naturels dans Unity**.  
En combinant sculpture, texturage et placement d’éléments naturels, on obtient un environnement immersif et réaliste.  
Ce travail servira de fondement aux TD suivants, axés sur la **génération procédurale** et les **systèmes automatisés**.
