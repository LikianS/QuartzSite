---
title: TD 1 – Implémentation d'un simple générateur de noms de gobelins
draft: false
---

# Lab 2 – TD1 Implémentation d'un simple générateur de noms de gobelins


## Objectif du TP

L’objectif de ce TP est de concevoir un générateur procédural de noms de gobelins pour un jeu de rôle (RPG).  
L’idée est de produire des noms uniques et cohérents sans avoir à les écrire manuellement, en suivant un modèle inspiré des jeux comme *World of Warcraft*.

---

## Principe de génération

Les noms de gobelins suivent généralement une structure reconnaissable :

- **Prénom** : constitué de 2 ou 3 syllabes choisies parmi des sons caractéristiques (Ba, Fiz, Griz, etc.).
- **Nom de famille** : formé d’un préfixe (objet/adjectif : Bolt, Gear, Fiz) et d’un suffixe (verbe ou action : blast, button, bomb).

Ces règles permettent de produire une grande variété de noms comme :
> Baxdibles Gearblast  
> Fizdaez Cogbutton  
> Danbieka Boltboot

---

## Implémentation du générateur de noms

Le générateur repose sur des bases de syllabes et d’éléments de noms stockés dans des tableaux.  
À chaque génération, l’algorithme sélectionne aléatoirement une syllabe de chaque base pour le prénom et deux parties pour le nom de famille.  
Le résultat est un nom unique conforme au style gobelin.

---

## Génération et affichage des gobelins

Un second script, appelé **GoblinWriter**, est associé à un objet vide dans la scène Unity.  
Ce script permet d’afficher à l’écran une description complète d’un gobelin comprenant :

- Son nom (généré aléatoirement),
- Son âge (entre 20 et une limite fixée),
- Son métier (choisi parmi une liste).

Le texte s’actualise automatiquement au lancement du jeu et à chaque pression sur la **barre d’espace**.

---

## Configuration dans Unity

1. Créer un **GameObject vide** et le renommer en *GoblinWriter*.
2. Ajouter le script *GoblinWriter* à cet objet.
3. Créer un **TextMeshPro** (GameObject → UI → Text - TextMeshPro) et le relier au champ `textMesh`.
4. Ajouter les métiers dans la liste `goblinJobs` (par exemple : warrior, archer, blacksmith, shaman).

👉 **IMAGE 1 ICI** : capture de l’Inspecteur Unity avec les propriétés `textMesh`, `goblinJobs`, et `goblinMaxAge` renseignées.

---

## Résultat obtenu

Au lancement du jeu, un texte s’affiche à l’écran, par exemple :  
> “Fizdiebles Gearbomb is a 87 years old goblin blacksmith.”

Chaque pression sur la barre d’espace génère un nouveau gobelin avec un nom, un âge et un métier aléatoires.

👉 **IMAGE 2 ICI** : capture du résultat à l’écran (texte généré).

---

## Interaction et variété

- Barre d’espace : génère un nouveau gobelin.  
- Âge : compris entre 20 et `goblinMaxAge`.  
- Métier : choisi dans la liste des métiers définis.  
- Nom : produit par assemblage procédural.

Cette approche garantit une grande diversité de personnages sans saisie manuelle.

---

## Lien avec la génération procédurale

Ce TP illustre les bases de la génération procédurale :  
- Combinaison d’éléments préexistants pour créer du contenu nouveau,  
- Utilisation du hasard contrôlé,  
- Similitude avec les tables aléatoires du *Donjon & Dragons Dungeon Master’s Guide*.

👉 **IMAGE 3 (optionnelle)** : plusieurs exemples de gobelins générés (collage de 3–4 résultats).

---

## Conclusion

Ce premier TP montre comment une logique simple de combinaison aléatoire peut donner de la richesse et de la personnalité à un jeu.  
Le générateur de noms de gobelins :  
- Automatise la création de personnages,  
- Évite la redondance,  
- Et prépare le terrain pour des systèmes procéduraux plus complexes.

