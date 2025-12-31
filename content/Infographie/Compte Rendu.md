---
title: Projet Omega Renouveau - Compte Rendu Technique
draft: false
author: Killian Diboues
---

# Projet Omega Renouveau - Compte Rendu Technique

## Vue d'ensemble
Projet Omega Renouveau est un jeu d'aventure en monde ouvert développé sous Unity, mettant en avant un système émotionnel et élémentaire complexe qui influence visuellement l'environnement et le gameplay.

---

## Système de Particules et Auras Élémentaires

### Auras du Joueur
Le joueur possède un système d'aura dynamique basé sur quatre émotions/éléments principaux :

<video controls src="vid 9.mp4" title="Title"></video>

- **Colère (Fire)** : Particules de feu enflammées autour du joueur
- **Tristesse (Water)** : Particules aquatiques et effet de pluie
- **Joie (Light)** : Particules lumineuses avec effet de glow sur le modèle du joueur
- **Stress (Glitch)** : Particules chaotiques et effet de distorsion

Le système utilise **EmotionAuraController.cs** qui gère :
- Le changement d'émotion via la manette (D-Pad)
- L'activation/désactivation des systèmes de particules correspondants
- La modification des matériaux du joueur (mode normal vs. émissif pour la Joie)

### Auras des Ennemis
Les ennemis réagissent à l'émotion globale du donjon via **EnemyEmotion.cs** :
- Synchronisation automatique avec l'émotion du donjon
- Changement de particules en temps réel
- System d'abonnement aux événements pour optimiser les performances

### Manager Global
**GlobalEmotionManager.cs** centralise la gestion :
- Pattern Singleton pour accès global
- Système d'événements pour notifier tous les abonnés
- Support du changement d'émotion de zone (dungeons, triggers)

---

## Shaders d'Émotions et Post-Processing

<video controls src="vid 9.mp4" title="Title"></video>

### Post-Processing Émotionnel
Le système utilise **DungeonPostProcessing.cs** et **DungeonPostProcessingSimple.cs** pour modifier l'ambiance visuelle :

#### Effets par Émotion
- **Colère** : 
  - Filtre de couleur blanc éclatant
  - Bloom pulsant (1.2 + effet sinusoïdal)
  - Vignetting intense (0.4)
  - Aberration chromatique animée
  
- **Tristesse** :
  - Filtre bleuté (0.3, 0.5, 0.8)
  - Ambiance froide et mélancolique
  
- **Joie** :
  - Filtre chaud doré (1.0, 0.95, 0.6)
  - Luminosité accrue
  
- **Stress** :
  - Filtre gris (desaturation)
  - Grain de film
  - Effets de glitch

Le système intègre des animations procédurales avec `Mathf.Sin(Time.time)` pour créer des effets pulsants et vivants.

---

## Shaders de Plans : Lave et Eau

<video controls src="vid 8.mp4" title="Title"></video>

### Shader d'Eau Toon (Eau_shader.shader)
Shader complet avec support de transparence et profondeur :

**Caractéristiques** :
- **Three-tone coloring** : Shallow (bleu clair), Deep (bleu foncé), Foam (blanc)
- **Profondeur procédurale** : Utilise `_CameraDepthTexture` pour calculer la profondeur de l'eau
- **Vagues animées** : Déformation vertex avec sinus pour simuler les vagues
- **Écume dynamique** : Noise Simplex pour créer de l'écume sur les bords
- **Transparence alpha** : Mélange entre couleurs peu profonde et profonde

**Optimisations** :
- Shader target 4.5 pour performances
- Noise procédural (pas de texture additionnelle)
- ZWrite Off pour transparence correcte

### Shader de Lave Toon (Lave_shader.shader)
Shader opaque avec effets d'explosions :

<video controls src="vid 7.mp4" title="Title"></video>

**Caractéristiques** :
- **Système multi-couches** : Deep color (rouge), Surface (orange), Outline (blanc)
- **Explosions procédurales** : Grille d'explosions générées aléatoirement
- **Mouvement de flux** : Direction et vitesse personnalisables
- **Impact visuel** : Zones de contact plus claires simulant la chaleur
- **Vagues globales** : Ondulation de surface

**Technique** :
- Pattern de lave via noise Simplex
- Hash 2D pour randomisation des explosions
- Déformation vertex pour donner du relief

---

## Raymarching : Nuages, VFX et Sorts

### Nuages Volumétriques
Quatre variations de shaders de nuages ont été développées :

#### 1. **Nuage.shader** - Volumétric Cloud Safe
- Raymarching classique avec FBM (Fractal Brownian Motion)
- Éclairage directionnel avec absorption de lumière
- Fade au sol pour intégration naturelle
- Optimisé à 2 octaves pour performances

#### 2. **Nuage1.shader** - URP avec Shadow Fix
- Compatible Universal Render Pipeline
- **Casting d'ombres** : Deux passes (rendu + shadow caster)
- Scattering de phase pour interaction avec le soleil
- Container avec EdgeFade pour éviter le clipping

#### 3. **NuageF.shader** - Toon Sky Cloud
- Style cartoon avec éclairage simplifié
- VerticalStretch pour corriger l'aplatissement
- Three-tone shading (Top, Bottom, Light Boost)
- Optimisé pour ciel stylisé

#### 4. **Fumé.shader** - Volumetric Smoke Inside
- **Cull Off** crucial : visible de l'intérieur du volume
- Intersection de boîte améliorée pour caméra dans le volume
- Gradient vertical (couleur top/bottom)
- Émission de feu intégrée (FireColor, FireIntensity)

### VFX et Sorts (Raymarchingmaster.shader)
Shader universal avec 10 effets différents :

**Effets disponibles** :
- Plasma, Fractal, Singularity
- Whip (fouet), Storm, Wind
- Stalactites, Magma, Ice, Ferro

**Technique** :
- SDF (Signed Distance Fields) pour définir les formes
- Raymarching avec 64 steps
- Container-safe avec EdgeFade
- Calcul de normales pour éclairage procédural
- Support transparence avec Alpha blending

### Shader Élémentaire Toon (ToonElementalVFX.shader)
Shader multi-éléments :
- **Fire** : Effet de flamme avec taper (pointe)
- **Ice** : Cristaux froids
- **Earth** : Roche solide
- **Wind** : Turbulence aérienne

Three-tone shading : Core (mid), Outer (shadow), Hot (highlight)

<video controls src="/Infographie/vid-4.mp4" title="Title"></video>
<video controls src="/Infographie/vid-6.mp4" title="Title"></video>
<video controls src="/Infographie/vid-1.mp4" title="Title"></video>
<video controls src="/Infographie/vid-3.mp4" title="Title"></video>


---

## Cycle Jour/Nuit

<video controls src="/Infographie/vid-5.mp4" title="Title"></video>

### DayNightCycle.cs
Système complet avec gradients Unity :

**Fonctionnalités** :
- **Durée personnalisable** : Par défaut 120 secondes (2 minutes réelles)
- **Rotation solaire** : De -90° à 270° pour simuler l'arc du soleil
- **Gradients multiples** :
  - `sunColor` : Couleur du soleil (aube, jour, crépuscule, nuit)
  - `skyColor` : Couleur du ciel ambient
  - `equatorColor` : Couleur à l'horizon
  - `fogColor` : Couleur du brouillard

**Optimisations** :
- Intensité adaptative (maxIntensity/minIntensity)
- **Désactivation des ombres la nuit** : `LightShadows.None` quand timeOfDay < 0.2 ou > 0.8
- Transitions douces via `Mathf.Lerp` et `Gradient.Evaluate()`
- Fade-in/out du soleil au lever/coucher (0.23-0.25 et 0.73-0.75)

**Intégration** :
- Utilise `RenderSettings.ambientSkyColor` pour l'occlusion ambiante
- Support URP avec Volume Overrides possible

---

## Génération Procédurale du Monde

### Architecture Multicouche

La génération procédurale est le cœur technique du projet, permettant de créer un monde infini, varié et performant.

![alt text](/Infographie/image-5.png)

### Deux Approches Développées

#### 1. Génération Complète (CompleteWorldGenerator.cs)
Première approche : génération d'un mesh unique géant.

**Principe** :
- Boucle double sur `worldRadius` (ex: -60 à +60 = 120×120 = 14 400 quads)
- Construction d'un seul mesh avec tous les vertices
- Placement immédiat de tous les props

**Avantages** :
- Simple à implémenter et débugger
- Mesh unifié = moins de drawcalls
- Parfait pour prototypage rapide

**Inconvénients** :
- Tout le monde chargé en mémoire (~500k vertices)
- Performances limitées pour grands mondes (>100m de rayon)
- Pas de streaming (chargement initial long)
- Limite Unity : 65 535 vertices par mesh (nécessite subdivision)

#### 2. Système de Chunks (Approche Retenue)
Implémentation via **ChunkManager.cs** et **WorldChunk.cs** :

**Architecture en 3 couches** :

##### Couche 1 : ChunkManager (Orchestrateur)
```csharp
Dictionary<Vector2Int, WorldChunk> activeChunks
```
- Calcule la position du joueur en coordonnées chunk
- Détermine quels chunks doivent être actifs (grille 5×5 autour du joueur)
- Génère les nouveaux chunks nécessaires
- Détruit les chunks trop éloignés (>loadRadius + 1)

**Algorithme de mise à jour** :
```csharp
void UpdateChunks() {
    Vector2Int playerChunk = GetChunkCoord(player.position);
    
    // 1. Générer les nouveaux chunks
    for (int x = -loadRadius; x <= loadRadius; x++) {
        for (int z = -loadRadius; z <= loadRadius; z++) {
            Vector2Int chunkCoord = playerChunk + new Vector2Int(x, z);
            if (!activeChunks.ContainsKey(chunkCoord)) {
                CreateChunk(chunkCoord);
            }
        }
    }
    
    // 2. Supprimer les chunks éloignés
    foreach (var chunk in activeChunks.ToList()) {
        if (Distance(chunk.Key, playerChunk) > loadRadius + 1) {
            Destroy(chunk.Value.gameObject);
            activeChunks.Remove(chunk.Key);
        }
    }
}
```

<video controls src="/Infographie/vid-2.mp4" title="Title"></video>

##### Couche 2 : WorldChunk (Générateur Individuel)
Chaque chunk est autonome et génère son propre terrain.

**Pipeline de génération** :

1. **Calcul de hauteur** (par vertex) :
```csharp
float worldX = offset.x + x;
float worldZ = offset.y + z;
float distFromCenter = Vector2.Distance(new Vector2(worldX, worldZ), Vector2.zero);

// Zone village (plat)
if (distFromCenter < villageRadius) {
    height = 0;
}
// Zone biome
else {
    BiomeProfile biome = GetBiomeForPosition(worldX, worldZ);
    float noise = Mathf.PerlinNoise(
        worldX * biome.noiseFrequency + seed, 
        worldZ * biome.noiseFrequency + seed
    );
    height = noise * biome.heightMultiplier;
}
```

2. **Assignation de couleur** (Vertex Colors) :
```csharp
Color vertexColor = biome.baseGroundColor;

// Zone de donjon (cercle coloré)
```

Le monde est conçu comme une roue élémentaire avec le village au centre et 4 biomes distincts rayonnant vers l'extérieur.

### Système de Biomes Avancé

Chaque biome est défini par un **BiomeProfile.cs** ScriptableObject, permettant une configuration granulaire sans toucher au code.

#### Structure Complète des Biomes

**Identité** :
- **Type** : Fire, Water, Earth, Air (enum)
- **Assets spéciaux** : dungeonPrefab, liquidSurfacePrefab
- **Palettes de couleurs** :
  - `baseGroundColor` : Couleur de base du terrain
  - `dungeonZoneColor` : Couleur autour du donjon
  - `eventZoneColor` : Couleur des zones de gameplay

**Topologie** :
- `heightMultiplier` (0-10) : Amplitude du relief
  - Fire : 8 (montagnes volcaniques)
  - Water : 2 (plat, marécages)
  - Earth : 6 (collines rocheuses)
  - Air : 4 (plateaux venteux)
  
- `noiseFrequency` (0.05-0.2) : Détail du terrain
  - Basse fréquence : Grandes formations douces
  - Haute fréquence : Terrain chaotique et détaillé

**Écosystème** :
- `trees[]` : 3-5 variations d'arbres
- `bushes[]` : Buissons et végétation basse
- `twigs[]` : Petits éléments décoratifs
- `rocks[]` : Rochers et formations rocheuses
- `vegetationDensity` (0-1) : Pourcentage de spawn

**Gameplay** :
- `enemies[]` : Mobs spécifiques au biome
- `puzzles[]` : Mécaniques d'énigmes
- `collectibles[]` : Objets à ramasser

![alt text](/Infographie/image-6.png)![alt text](/Infographie/image-7.png)![alt text](/Infographie/image-8.png)![alt text](/Infographie/image-9.png)

### Positionnement Géométrique des Zones

#### Calcul de Position
```csharp
Vector3 position = new Vector3(worldX, height, worldZ);
Vector2 posFlat = new Vector2(worldX, worldZ);
float angle = Mathf.Atan2(worldZ, worldX); // Angle polaire [-π, π]
float distFromCenter = posFlat.magnitude;
```

#### Disposition Radiale
```
           Nord
        Air (NW)
     Quadrant 2
   (-60, 60, 90°)
             |
Water (SW) --+-- Earth (NE)
  Quadrant 3 |  Quadrant 1
(-60,-60,180°)|  (60,60,0°)
             |
       Fire (SE)
     Quadrant 4
   (60, -60, -90°)
```

**Fonction de détermination** :
```csharp
BiomeProfile GetBiome(float angle) {
    if (angle >= -PI && angle < -PI/2)      return biomeWater;  // SW
    if (angle >= -PI/2 && angle < 0)        return biomeFire;   // SE
    if (angle >= 0 && angle < PI/2)         return biomeEarth;  // NE
    if (angle >= PI/2 && angle <= PI)       return biomeAir;    // NW
}
```

**Calcul de distance aux donjons** :
```csharp
Dictionary<BiomeType, Vector3> dungeonPositions;
float dungeonDist = Vector3.Distance(position, dungeonPos);
```

### Génération par Zone - Système Multi-Anneaux

#### 1. Village Central (Zone 0)
**Critère** : `distFromCenter < villageRadius` (15m)

**Caractéristiques** :
- Terrain parfaitement plat (`height = 0`)
- Aucune végétation (spawn désactivé)
- Couleur uniforme (grass vert clair)
- Zone de spawn sécurisée
- Accès aux 4 biomes équidistant

**Code** :
```csharp
if (distFromCenter < villageRadius) {
    height = 0;
    color = Color.green;
    skipPropPlacement = true;
}
```


#### 2. Zone de Transition (15m - 30m)
**Caractéristiques** :
- Blend entre couleur village et biome
- Relief progressif (multiplication par facteur 0-1)
- Végétation éparse (densité × 0.5)

```csharp
float transitionBlend = (distFromCenter - villageRadius) / transitionWidth;
height = Mathf.Lerp(0, biomeHeight, transitionBlend);
color = Color.Lerp(villageColor, biomeColor, transitionBlend);
```

#### 3. Zone de Biome Pure (30m - donjon)
**Caractéristiques** :
- Terrain pleinement influencé par le biome
- Paramètres maximums appliqués
- Végétation à densité normale

**Génération multicouche** :
```csharp
// Layer 1 : Forme générale (octave 1)
float noise1 = Mathf.PerlinNoise(x * 0.05f + seed, z * 0.05f + seed);

// Layer 2 : Détails moyens (octave 2)
float noise2 = Mathf.PerlinNoise(x * 0.1f + seed, z * 0.1f + seed) * 0.5f;

// Layer 3 : Micro-détails (octave 3)
float noise3 = Mathf.PerlinNoise(x * 0.2f + seed, z * 0.2f + seed) * 0.25f;

// Combinaison FBM
float finalNoise = (noise1 + noise2 + noise3) / 1.75f;
height = finalNoise * biome.heightMultiplier;
```

#### 4. Zone de Donjon (Cercle autour du donjon)
**Critère** : `distFromDungeon < dungeonRadius` (8m)

**Caractéristiques** :
- **Couleur distinctive** : Signal visuel clair
  - Fire : Rouge/orange brillant
  - Water : Bleu profond
  - Earth : Marron/vert foncé
  - Air : Blanc/cyan
  
- **Aplatissement du terrain** : Plateforme pour le donjon
- **Clearing végétal** : Pas d'obstruction
- **Particules ambiantes** : Système de particules élémentaire

```csharp
if (distFromDungeon < dungeonRadius) {
    float blend = 1 - (distFromDungeon / dungeonRadius);
    color = Color.Lerp(biomeColor, biome.dungeonZoneColor, blend);
    height *= 0.3f; // Aplatissement
    
    if (distFromDungeon < dungeonRadius * 0.8f) {
        skipPropPlacement = true; // Zone dégagée
    }
}
```

![alt text](/Infographie/image-10.png)

#### 5. Anneaux d'Événements (Zones de Gameplay)
**Système concentrique** :
```csharp
float distBand1 = Mathf.Abs(distFromCenter - 40f); // Anneau à 40m
float distBand2 = Mathf.Abs(distFromCenter - 60f); // Anneau à 60m
float distBand3 = Mathf.Abs(distFromCenter - 80f); // Anneau à 80m

if (distBand1 < 3f || distBand2 < 3f || distBand3 < 3f) {
    isEventZone = true;
    color = Color.Lerp(biomeColor, biome.eventZoneColor, 0.5f);
}
```

**Distribution du contenu** :
- **Anneau 1 (40m)** : Collectibles faciles + puzzles simples
- **Anneau 2 (60m)** : Ennemis de niveau moyen
- **Anneau 3 (80m)** : Puzzles complexes + boss mineurs

**Spawn contrôlé** :
```csharp
if (isEventZone && Random.value < spawnChance) {
    GameObject prefab = biome.GetRandomGameplay(GameplayType.Puzzle);
    Instantiate(prefab, position, Quaternion.identity);
}
```

#### 6. Barrière Limite (worldLimitRadius)
**Critère** : `distFromCenter > worldLimitRadius - barrierWidth`

**Mur d'obsidienne** :
```csharp
if (distFromCenter > worldLimit - 0.35f) {
    height = 10f; // Mur haut
    color = new Color(0.15f, 0.15f, 0.2f); // Noir-bleuté
    isBarrier = true;
}
```

**Fonction** :
- Limite physique du monde
- Évite le vide visuel
- Colliders pour bloquer le joueur
- Esthétique : Muraille naturelle


### Placement Intelligent de la Végétation

#### Algorithme de Densité Adaptative
```csharp
void PlaceProp(Vector3 position, BiomeProfile biome) {
    // 1. Vérification de zone
    if (IsInVillage(position)) return;
    if (IsInDungeonZone(position)) return;
    
    // 2. Test de densité
    float roll = Random.value;
    if (roll > biome.vegetationDensity) return;
    
    // 3. Test de pente
    float slope = CalculateSlope(position);
    if (slope > 30f) return; // Trop pentu
    
    // 4. Test de proximité (éviter le clustering)
    if (IsTooCloseToOtherProp(position, 2f)) return;
    
    // 5. Sélection du prop
    int propType = Random.Range(0, 4); // Tree, Bush, Twig, Rock
    GameObject prefab = biome.GetRandomProp(propType);
    
    // 6. Placement avec variation
    Quaternion rotation = Quaternion.Euler(0, Random.Range(0f, 360f), 0);
    float scale = Random.Range(0.8f, 1.2f);
    
    GameObject prop = Instantiate(prefab, position, rotation);
    prop.transform.localScale = Vector3.one * scale;
    
    // 7. Ajout à la liste de combine
    AddToCombineList(prop);
}
```

**Distribution par type** :
- **Arbres** : 40% (landmarks, structure verticale)
- **Buissons** : 30% (remplissage, couverture)
- **Twigs** : 20% (détails au sol)
- **Rochers** : 10% (accents, obstacles)

### Spécificités par Biome

#### Fire Biome
- **Relief** : Montagnes escarpées (heightMultiplier = 8)
- **Végétation** : Arbres morts, rochers volcaniques
- **Sol** : Rouge-orange foncé
- **Liquide** : Lave (liquidSurfacePrefab avec Lave_shader)
- **Particules** : Cendres montantes, fumée
- **Gameplay** : Ennemis agressifs, puzzles de timing

**Génération spécifique** :
- **Petits lacs de lave** : Placés aléatoirement dans le biome selon un seed procédural
- **Petits monticules** : Formations rocheuses de 3-5m de hauteur dispersées
- **Terrain fragmenté** : Haute variabilité avec crêtes et vallées abruptes
- **Zones dangereuses** : Dégâts périodiques proches des lacs de lave

#### Water Biome
- **Relief** : Plat avec dépressions (heightMultiplier = 2)
- **Végétation** : Palm trees, roseaux
- **Sol** : Sable beige, zones bleues
- **Liquide** : Eau (Eau_shader avec transparence)
- **Particules** : Brouillard, pluie légère
- **Gameplay** : Puzzles de navigation, collectibles sous-marins

**Génération spécifique** :
- **Grands lacs** : Vastes étendues d'eau navigables avec îles intérieures
- **Petits monticules** : Îles et presqu'îles dispersées au sein des lacs
- **Système de canaux** : Connexions aquatiques entre les zones de gameplay
- **Zones submersibles** : Plateformes basculantes entre niveaux d'eau

#### Earth Biome
- **Relief** : Collines ondulées (heightMultiplier = 6)
- **Végétation** : Forêts denses, rochers massifs
- **Sol** : Vert-brun, terre
- **Liquide** : Aucun (biome sec)
- **Particules** : Poussière, feuilles
- **Gameplay** : Énigmes d'armures, exploration verticale

**Génération spécifique** :
- **Labyrinthe au sol** : Structure procédurale de chemins rocailleux et murailles de terre
- **Monticules intégrés** : Formations collinaires servant de points de repère et d'obstacles
- **Passages cachés** : Tunnels et grottes reliant les sections du labyrinthe
- **Énigmes spatiales** : Perspective et topologie comme mécaniques de puzzle

#### Air Biome
- **Relief** : Plateaux (heightMultiplier = 4)
- **Végétation** : Arbres épars, cristaux
- **Sol** : Gris-blanc, nuancé
- **Liquide** : Aucun (biome aérien)
- **Particules** : Vent, nuages bas (Fumé.shader)
- **Gameplay** : Puzzles de miroirs, ennemis volants

**Génération spécifique** :
- **Labyrinthe aérien** : Structures flottantes (plateformes cristallines) entrelacées en 3D
- **Pas de terrain solide** : Chutes mortelles encadrant le labyrinthe
- **Chemins suspendus** : Ponts et rebords étroits nécessitant de la précision
- **Navigation verticale** : Montée/descente à travers les niveaux du labyrinthe céleste

### Optimisation de la Génération Procédurale

#### Combine Meshes (Batching Statique)
```csharp
Dictionary<GameObject, List<CombineInstance>> propsToCombine;

void AddToCombineList(GameObject prop) {
    GameObject prefabSource = PrefabUtility.GetPrefabParent(prop);
    if (!propsToCombine.ContainsKey(prefabSource)) {
        propsToCombine[prefabSource] = new List<CombineInstance>();
    }
    
    CombineInstance ci = new CombineInstance();
    ci.mesh = prop.GetComponent<MeshFilter>().sharedMesh;
    ci.transform = prop.transform.localToWorldMatrix;
    propsToCombine[prefabSource].Add(ci);
    
    Destroy(prop); // Original détruit
}

void FinalizeCombining() {
    foreach (var kvp in propsToCombine) {
        GameObject combined = new GameObject(kvp.Key.name + "_Combined");
        MeshFilter mf = combined.AddComponent<MeshFilter>();
        MeshRenderer mr = combined.AddComponent<MeshRenderer>();
        
        mf.mesh = new Mesh();
        mf.mesh.CombineMeshes(kvp.Value.ToArray(), true, true);
        mr.sharedMaterial = kvp.Key.GetComponent<MeshRenderer>().sharedMaterial;
        
        // Static batching
        combined.isStatic = true;
    }
}
```

**Impact** : 
- Avant : 150 GameObjects = 150 drawcalls
- Après : 5 GameObjects combinés = 5 drawcalls
- **Gain : 97% de drawcalls en moins**

#### Culling Automatique
- **Frustum Culling** : Intégré Unity (chunks hors écran)
- **Distance Culling** : Déchargement des chunks éloignés
- **Occlusion Culling** : Possible avec bake (non implémenté)

**Statistiques** :
- Distance de chargement : 2 chunks (100m)
- Chunks actifs simultanés : 25 (5×5 grille)
- Mémoire par chunk : ~2-3 MB
- Total en mémoire : 50-75 MB (vs 500+ MB en génération complète)

```csharp
if (distFromDungeon < dungeonRadius) {
    vertexColor = Color.Lerp(baseColor, biome.dungeonZoneColor, blend);
}

// Zone d'événement (anneaux concentriques)
if (isInEventRing) {
    vertexColor = Color.Lerp(baseColor, biome.eventZoneColor, 0.5f);
}

colors.Add(vertexColor);
```

> **[IMAGE : Mesh avec vertex colors montrant les différentes zones]**

3. **Construction du mesh** :
```csharp
// Quad par quad (2 triangles)
for (int x = 0; x < size - 1; x++) {
    for (int z = 0; z < size - 1; z++) {
        int i = x + z * size;
        
        // Triangle 1
        triangles.Add(i);
        triangles.Add(i + size);
        triangles.Add(i + 1);
        
        // Triangle 2
        triangles.Add(i + 1);
        triangles.Add(i + size);
        triangles.Add(i + size + 1);
    }
}
```

4. **Placement des props** :
- Arbres, buissons, rochers selon `vegetationDensity`
- Règles d'exclusion (pas dans le village, pas sur pentes >30°)
- Randomisation contrôlée avec seed

5. **Optimisation finale** :
- **Mesh Combining** : Fusion des props identiques
- **MeshCollider** : Génération avec `cookingOptions` optimisé
- **Batching** : Material partagé entre chunks


##### Couche 3 : BiomeProfile (Configuration)
ScriptableObject définissant chaque biome.

**Données stockées** :
```csharp
[CreateAssetMenu]
public class BiomeProfile : ScriptableObject {
    public BiomeType type;              // Fire, Water, Earth, Air
    public Color baseGroundColor;       // Couleur du sol
    public float heightMultiplier;      // Amplitude du relief (0-10)
    public float noiseFrequency;        // Détail du terrain (0.05-0.2)
    public GameObject[] trees;          // Prefabs de végétation
    public float vegetationDensity;     // 0-1 (% de spawn)
    public GameObject[] enemies;        // Mobs du biome
    public GameObject dungeonPrefab;    // Structure unique
}
```

**Avantages du ScriptableObject** :
- Modification en temps réel dans l'éditeur
- Réutilisable entre scènes
- Pas de recompilation nécessaire
- Designer-friendly


![alt text](/Infographie/image-12.png)

### Optimisations Avancées

#### 1. Culling Automatique
- Unity culling frustum : objets hors écran non rendus
- Chunks entiers désactivés si éloignés
- Occlusion culling possible (bake requis)

---

## Minimap

![alt text](/Infographie/image-11.png)

### Implémentation
La minimap utilise une caméra orthographique secondaire :

**Configuration** :
- **Caméra dédiée** : Vue du dessus en mode Orthographic
- **Render Texture** : Texture de 512x512 mise à jour en temps réel
- **UI Canvas** : RawImage affichant la Render Texture
- **Culling Mask** : Layers spécifiques pour la minimap

**Suivi du Joueur** :
- Position de la caméra minimap = position joueur + offset Y
- Rotation lock ou rotation suivant le joueur (configurable)

**Optimisations** :
- Faible résolution de render (512x512)
- Layers simplifiés (terrain + landmarks)
- Update rate réduit si nécessaire
- Icons 2D pour points d'intérêt au lieu de 3D

**Intégration Monde Procédural** :
- La minimap se met à jour automatiquement avec le chunking
- Markers pour les donjons aux positions fixes
- Fog of war possible via masque

---

## Assets Créés

### Assets Terminés

#### Végétation
- **Arbres** 
  
- **Buissons**

![alt text](/Infographie/image.png)
![alt text](/Infographie/image-1.png)
![alt text](/Infographie/image-2.png)

#### Armes
- **Épée** 

### Travaux en Cours

#### Personnages
- **Mage** : Modèle en cours
  
- **Chevalier** : Modèle en cours

---

## Corrections et Améliorations

### Bugs Corrigés
- **Système d'émotions** : Abonnement/désabonnement aux événements (memory leaks)
- **Post-processing** : Rafraîchissement correct lors du changement de scène
- **Chunks** : Déchargement propre avec destruction de colliders
- **Raymarching** : Intersection de boîte quand caméra à l'intérieur
- **Shaders** : Support transparence avec depth buffer

### Énigmes Ajoutées

#### Système d'Énigmes à Miroirs
Un puzzle de réflexion optique complet avec physique de rayon laser.

![alt text](/Infographie/image-13.png)![alt text](/Infographie/image-14.png)

**Architecture** :
- **LaserBeam.cs** : Gère le rayon laser et ses réflexions
  - LineRenderer pour visualisation
  - Raycast avec rebonds multiples (jusqu'à 5 réflexions)
  - Détection via tags ("Mirror", "Receiver")
  
**Mécanique** :
```csharp
// Calcul des réflexions successives
direction = Vector3.Reflect(direction, hit.normal);
currentPos = hit.point;
```

- **MirrorPlacer.cs** : Placement et rotation des miroirs
  - **Grid snapping** : Alignement sur grille de 0.5m pour précision
  - Rotation par pas de 45° (8 orientations possibles)
  - Système de prise en main (holdPoint du joueur)
  
**Récepteur** :
- Tag "Receiver" active des mécanismes (portes, plateformes)
- Vérification continue via `Update()` du laser
- Activation uniquement si le laser touche directement

**Gameplay** :
1. Ramasser un miroir avec interaction
2. Le placer sur la grille (snap automatique)
3. Le faire pivoter pour orienter la réflexion
4. Créer un chemin de lumière jusqu'au récepteur

#### Système d'Énigmes d'Armures
Puzzle combinatoire complexe nécessitant observation et déduction.

![alt text](/Infographie/image-15.png)

**Concept** :
4 armures avec 2 éléments modifiables chacune :
- **Blasons** : Symboles sur le torse
- **Armes** : 5 types (Épée, Hache, Masse, Arc, Lance)

**Architecture modulaire** :

**ArmorSlot.cs** : Conteneur d'une armure
- Matériaux de blasons interchangeables
- Prefabs d'armes activables/désactivables
- Getters pour vérification de solution

**ClickablePart.cs** : Interface interactive
- Implémente `IInteractable`
- Distinction par `PartType` (Blason ou Arme)
- Différentes interactions selon `InteractionType`
- Notifie le PuzzleManager à chaque changement

**PuzzleManager.cs** : Vérificateur de solution
```csharp
for (int i = 0; i < armorSlots.Length; i++) {
    if (armorSlots[i].GetCurrentBlason() != correctBlasons[i] ||
        armorSlots[i].GetCurrentWeapon() != correctWeapons[i]) {
        allCorrect = false;
    }
}
```

**Déclencheurs** :
- **PuzzleZone.cs** : Active le mode puzzle au trigger
- **ArmorTrigger.cs** : Détecte la proximité avec une armure spécifique
- **PlayerInteractionArmor.cs** : Gère les inputs joueur (cycle blason/arme)

**Feedback** :
- Effets visuels à la résolution (particules)
- Ouverture de portes via `DoorController`
- Debug.Log pour le dev

**Variations possibles** :
- 4 armures × 3-5 blasons × 5 armes = 300-500 combinaisons
- Indices visuels dans l'environnement (peintures murales, parchemins)
- Timer optionnel pour difficulté accrue

#### Énigmes Émotionnelles
- Système de triggers par zone (**ZoneEmotionTrigger.cs**)
- Puzzles nécessitant la bonne émotion pour progresser
- Collectibles cachés dans chaque biome
- Ennemis spécifiques par zone

### Améliorations Sonores
**AmbianceManager.cs** gère l'ambiance par scène :
- Village (ambiance calme)
- Donjon (ambiance tendue)
- Maison (ambiance intérieure)
- Transition automatique au changement de scène

---

## Stack Technique

**Moteur** : Unity 2021+ (Universal Render Pipeline)
**Langage** : C# (.NET Standard 2.1)
**Rendering** : URP avec Volume Overrides
**Shaders** : HLSL/Cg (ShaderLab et Shader Graph)
**Génération** : Perlin/Simplex Noise
**Architecture** : Event-driven avec Singleton patterns

---

## Statistiques du Projet

- **Scripts** : 20+ classes C#
- **Shaders customs** : 12+ (dont 4 variations de nuages)
- **Biomes** : 4 complets avec props uniques
- **Système de particules** : 5+ par émotion
- **Optimisations** : Chunking, mesh combining, culling, shadow disabling
- **ScriptableObjects** : BiomeProfiles configurables

---

## Prochaines Étapes

- Finalisation des modèles Mage et Chevalier
- Système de quêtes lié aux émotions
- Boss par biome avec mécaniques uniques
- Sauvegarde du monde procédural
- Multithreading de la génération de chunks
- Amélioration du système de combat
- Intégration de l'épée avec combos

---

*Projet développé avec passion par l'équipe Omega*
