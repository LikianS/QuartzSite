---
title: TD 1 – Rendu d'une sphère avec la marche des rayons (Ray Marching)
draft: false
---

## Objectif du TD
L'objectif de ce TD est d'implémenter les **fondamentaux de la marche des rayons (Ray Marching)** pour effectuer le rendu procédural d'une sphère unique, incluant un ombrage de base de type **Lambert**, à l'aide d'un **Compute Shader** (Shader de Calcul) dans Unity.

---

## I. Configuration Initiale et Définition de la Scène

### **1. Initialisation du Shader et du Wrapper C#**
- Création du `Compute Shader` : **RaymarchingSphere.compute** avec la fonction de base `CSMain` pour remplir l'écran en noir (rendu initial).
- Création de la classe C# : **ComputeRaymarchingSphere.cs** (dérivée de `URPComputeAsset`) pour servir de *wrapper* et pour lier le shader au **pipeline URP** (Universal Render Pipeline).


---

### **2. Définition du Champ de Distance Signée (SDF)**

Le **SDF** (Signed Distance Field) est le cœur du Ray Marching. Il définit la surface des formes.

- **Fonction `sdfSphere`** : Définit une sphère de manière mathématique.
    $$\text{float sdf} = \text{length}(P - C) - r$$
    *(Où $P$ est le point échantillonné, $C$ la position du centre et $r$ le rayon).*

- **Fonction `mapSceneSdf`** : Permet d'encapsuler la scène. Dans ce TD, elle appelle simplement `sdfSphere` avec des valeurs arbitraires :
    ```c
    float mapSceneSdf(float3 p) {
        float sphere = sdfSphere(p, float3(0, 0, 1), 5); // Sphère à (0, 0, 1) de rayon 5
        return sphere;
    }
    ```

---

## II. Génération des Rayons et Ray Marching

### **1. Génération des Rayons dans `CSMain`**

Contrairement aux shaders classiques, les coordonnées doivent être calculées à partir de l'ID du thread (`SV_DispatchThreadID`).

- **Calcul des coordonnées UV** : Convertit l'ID du thread en coordonnées normalisées `[-1, 1]`, avec le centre à `(0, 0)`.
- **Origine du rayon (`rayOrigin`)** : Déterminée par la position de la caméra dans l'espace monde, transmise via la matrice **`CameraToWorld`** :
    ```c
    float3 rayOrigin = mul(CameraToWorld, float4(0, 0, 0, 1)).xyz;
    ```
- **Direction du rayon (`rayDirection`)** : Calculée en convertissant les coordonnées de l'espace écran à l'espace caméra, puis à l'espace monde, en utilisant les matrices **`CameraInverseProjection`** et **`CameraToWorld`**.

### **2. Logique de la Marche des Rayons (`raymarch`)**

La fonction `raymarch` est le cœur de l'algorithme, utilisant des itérations pour trouver la surface.

- **Boucle de recherche** : Le rayon avance par étapes, la taille de chaque pas étant déterminée par la distance signée retournée par `mapSceneSdf` (le **plus court chemin jusqu'à la surface**).
    ```c
    static const float MINIMUM_HIT_DISTANCE = 0.001f;
    static const float MAXIMUM_TRACE_DISTANCE = 1000.0f;
    // ...
    float dist = mapSceneSdf(currentPosition);
    if (dist <= MINIMUM_HIT_DISTANCE) {
        return SurfaceColor; // Touché
    }
    totalDistanceTraveled += dist; // Avance du rayon
    ```

- **Condition d'arrêt** :
    - **Impact (Hit)** : Si `dist <= MINIMUM_HIT_DISTANCE`, la surface est trouvée. La couleur de la surface (`SurfaceColor`) est renvoyée.
    - **Échec (Miss)** : Si `totalDistanceTraveled >= MAXIMUM_TRACE_DISTANCE`, le rayon n'a rien trouvé. La couleur de l'arrière-plan (noir) est renvoyée.

---

## III. Ajout de l'Ombrage de Lambert

Pour créer un rendu réaliste, l'ombrage est ajouté en utilisant le modèle de **Lambert**.

### **1. Estimation de la Normale de Surface**
La normale est estimée en calculant le **gradient du FDS** autour du point d'impact (`p`) en utilisant un petit décalage (`EPSILON`).

- **Fonction `estimateNormal`** :
    ```c
    // Calcule le gradient du FDS pour X, Y et Z
    float gradX = mapSceneSdf(float3(p.x + EPSILON, ...)) - mapSceneSdf(float3(p.x - EPSILON, ...));
    // ...
    return normalize(float3(gradX, gradY, gradZ));
    ```

### **2. Définition et Intégration de la Lumière**
- Ajout de la variable **`DirectionalLight`** (float4) au shader pour contenir la direction et l'intensité de la lumière.
- Mise à jour de la classe C# `ComputeRaymarchingSphere` pour calculer la direction de la lumière à partir de la rotation et transmettre ces données au shader.

### **3. Calcul de la Couleur Finale dans `raymarch`**
L'intensité diffuse est calculée en faisant le **produit scalaire** (dot product) entre la **normale de la surface** et la **direction de la lumière inversée** (pour aller de la surface vers la lumière).

```c
// Calcul de l'intensité diffuse de Lambert
float3 normal = estimateNormal(currentPosition);
float diffuseIntensity = saturate(dot(normal, -DirectionalLight.xyz)) * DirectionalLight.w;

// Retour de la couleur finale éclairée
return SurfaceColor * diffuseIntensity;


<video controls src="tds-chap4-scratch - GeometricShapes - Windows, Mac, Linux - Unity 2023.2.20f1 _DX11_ 2025-12-06 20-01-20.mp4" title="Title"></video>