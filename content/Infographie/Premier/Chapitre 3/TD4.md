---
title: TD 4 – Effets de post-traitement
draft: false
author: Killian Diboues
---

## Objectif du TD
Ce TD a pour objectif de découvrir l’application d’**effets de post-traitement** dans Unity afin d’améliorer l’apparence visuelle d’une scène et de rendre l’environnement de jeu plus réaliste et professionnel.

---

## Étapes principales

### **1. Installation de la stack de post-traitement**
- Télécharger la **stack de post-traitement** depuis l’Asset Store Unity (ou via le lien fourni dans l’annexe).
- Le terrain créé dans les TD précédents peut servir de base pour ajouter les effets.

---

### **2. Ajout du composant Post-Processing Behaviour**
- Sélectionner la **caméra principale** dans la hiérarchie.
- Dans l’inspecteur, ajouter le composant **Comportement de post-traitement**.

---

### **3. Création d’un profil de post-traitement**
- Dans le projet, créer un **nouveau profil** : clic droit > Créer > Profil de post-traitement.
- Renommer le profil selon vos préférences.
- Le sélectionner pour examiner ses propriétés dans l’inspecteur.

- Glisser-déposer le profil sur le composant **Post-Processing Behaviour** de la caméra pour appliquer les effets à la scène.

---

### **4. Résultat et comparaison**
- Comparer la scène **sans** et **avec** post-traitement.
- Les effets ajoutés peuvent inclure :
  - **Brouillard**
  - **Occlusion ambiante**
  - **Profondeur de champ**
- Ces effets améliorent fortement la qualité visuelle, donnant un rendu plus professionnel.

---

### Remarques importantes
- Les effets de post-traitement s’appliquent **après le rendu de la caméra**, ce qui peut demander beaucoup de ressources graphiques.
- Leur utilisation est déconseillée sur les plateformes à faible puissance graphique (ex. anciens appareils mobiles).
- Ces effets permettent de transformer une scène simple en un environnement plus immersif et réaliste.

---

## Conclusion
L’ajout de post-traitement est une étape clé pour **améliorer l’esthétique d’un jeu**. Même des effets simples comme le brouillard, l’occlusion ambiante et la profondeur de champ peuvent considérablement rehausser la qualité d’une scène Unity.

![alt text](/Infographie/Chapitre-3/image-6.png)
<video controls src="/Infographie/Chapitre-3/Labs---chap3---TD4---question4---Windows,-Mac,-Linux---Unity-2023.2.20f1-_DX11_-2025-12-04-22-51-55.mp4" title="Title"></video>

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
