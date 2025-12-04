---
title: TD 3 – Création des Nuages dans Unity
draft: false
---

-----

## Objectif du TD

L'objectif est de créer un **shader de matériau personnalisé pour la skybox** (`RaymarchedSkybox.shader`) afin d'afficher des **nuages volumétriques** générés par la technique de **Ray Marching** et basés sur une texture de bruit de Perlin. Cela implique de calculer la position dans l'espace monde et d'accumuler la couleur et l'opacité le long d'un rayon de vue.

-----

## I. Shader de Matériau pour la Skybox (`RaymarchedSkybox.shader`)

Voici le code HLSL complet du shader `Custom/RaymarchedSkybox`, intégrant la logique de séparation ciel/sol, le Ray Marching pour les nuages, et la simulation d'absorption de lumière via un gradient.

### 1\. Propriétés et Configuration du Shader

Nous définissons les couleurs, les paramètres de brouillard, et les nouvelles propriétés spécifiques aux nuages (textures, échelle, hauteur).

```hlsl
Shader "Custom/RaymarchedSkybox" {
    Properties {
        // Couleurs et Brouillard de base
        _SkyColor("Sky Color", Color) = (0.2, 0.4, 0.8, 1)
        _GroundColor("Ground Color", Color) = (0.3, 0.2, 0.1, 1)
        _FogColor("Fog Color", Color) = (0.6, 0.7, 0.8, 1)
        _FogClouds("Fog Amount Clouds", Float) = 0.001
        _FogSky("Fog Amount Sky", Float) = 0.1
        _FogGround("Fog Amount Ground", Float) = 0.1
        
        // Paramètres des Nuages
        _CloudTex("Cloud Noise Texture (R)", 2D) = "white" {}
        _GradientTex("Cloud Gradient (Ramp)", 2D) = "white" {}
        _CloudHeight("Cloud Height (Y)", Float) = 20.0
        _CloudThickness("Cloud Thickness (Y)", Float) = 15.0
        _CloudOpacity("Cloud Opacity", Float) = 1.0
        _CloudScale("Cloud Scale (XZ)", Vector) = (0.001, 0.001, 0, 0)
        _TopSurfaceScale("Top Surface Scale", Float) = 1.0
        _BottomSurfaceScale("Bottom Surface Scale", Float) = 1.0
        _CloudSoftness("Cloud Softness", Float) = 2.0
    }

    SubShader {
        Tags {
            "Queue" = "Background"
            "RenderType" = "Background" 
            "PreviewType" = "Skybox" 
        }
        Cull Off ZWrite Off
        
        Pass {
            HLSLPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #define SAMPLES 32 // Nombre d'échantillons pour le Ray Marching
            
            #include "Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl"

            // Variables de base
            float4 _SkyColor;
            float4 _GroundColor;
            float4 _FogColor;
            float _FogClouds;
            float _FogSky;
            float _FogGround;
            
            // Variables des Nuages
            TEXTURE2D(_CloudTex); SAMPLER(sampler_CloudTex);
            TEXTURE2D(_GradientTex); SAMPLER(sampler_GradientTex);
            float _CloudHeight;
            float _CloudThickness;
            float _CloudOpacity;
            float4 _CloudScale;
            float _TopSurfaceScale;
            float _BottomSurfaceScale;
            float _CloudSoftness;

            struct appdata {
                float4 vertex : POSITION;
            };

            struct v2f {
                float4 vertex : SV_POSITION;
                float3 viewVector : TEXCOORD1;
            };

            // Fonction Vertex (vert)
            v2f vert (appdata v) {
                v2f o;
                o.vertex = TransformObjectToHClip(v.vertex);
                o.viewVector = v.vertex.xyz; // Le vecteur de vue est la position du vertex dans l'espace objet
                return o;
            }

            // Fonction Fragment (frag)
            float4 frag (v2f i) : SV_Target {
                float3 viewVector = i.viewVector;
                
                // ----------------------------------------------------
                // 1. Logique Sol/Horizon
                // ----------------------------------------------------
                
                if (viewVector.y < 0) { // GROUND
                    viewVector = viewVector / viewVector.y;
                    float groundFog = 1 - (1 / (_FogGround * length(viewVector) + 1));
                    return lerp(_GroundColor, _FogColor, groundFog);
                }
                else if (viewVector.y == 0) { // HORIZON
                    return _FogColor;
                }

                // ----------------------------------------------------
                // 2. Logique Ciel & Nuages (viewVector.y > 0)
                // ----------------------------------------------------
                
                // Normalisation du vecteur de vue
                viewVector = viewVector / viewVector.y;

                // 1. Calcul de la position de départ du Ray Marching (au niveau de la base du nuage)
                float3 viewerPosition = _WorldSpaceCameraPos;
                // La position de départ est l'intersection entre le rayon et le plan Y = _CloudHeight
                float3 position = viewerPosition + viewVector * (_CloudHeight - viewerPosition.y);
                
                // 2. Préparation du pas et de l'opacité
                float3 stepSize = viewVector * _CloudThickness / SAMPLES;
                float stepOpacity = 1 - (1 / (_CloudOpacity * length(stepSize) + 1));
                
                // Brouillard de fond pour les nuages
                float cloudFog = 1 - (1 / (_FogClouds * length(viewVector) + 1));
                float4 col = float4(_FogColor.rgb * cloudFog, cloudFog);
                
                // 3., 4., 5., 7. Boucle de Ray Marching
                for (int i = 0; i < SAMPLES; i++) {
                    position += stepSize;
                    
                    // 3. Échantillonnage de la texture de bruit pour la hauteur
                    float2 uv = position.xz * _CloudScale.xy; // Utilisation de l'échelle
                    float h = SAMPLE_TEXTURE2D(_CloudTex, sampler_CloudTex, uv).r;
                    float cloudTopHeight = 1 - (h * _TopSurfaceScale);
                    float cloudBottomHeight = h * _BottomSurfaceScale;
                    
                    // 4. Vérification si le point est dans le nuage
                    // f est la position relative du point dans la "boîte" du nuage [0..1]
                    float f = (position.y - _CloudHeight) / _CloudThickness;
                    
                    if (f > cloudBottomHeight && f < cloudTopHeight) {
                        
                        // Calcul de la distance à la surface la plus proche (pour la douceur)
                        float dist = min(cloudTopHeight - f, f - cloudBottomHeight);
                        
                        // 4. Calcul de l'opacité locale
                        float localOpacity = saturate(dist / _CloudSoftness);
                        
                        // 7. Échantillonnage du dégradé (simulation d'absorption lumineuse)
                        // L'UV est 1 - position relative dans la couche supérieure pour aller du blanc au gris
                        float4 cloudColor = SAMPLE_TEXTURE2D(_GradientTex, sampler_GradientTex, float2(1 – saturate(cloudTopHeight - f), 0));

                        // 4. Accumulation de couleur et d'opacité
                        col.rgb += (1 - col.a) * stepOpacity * localOpacity * cloudColor.rgb;
                        col.a += (1 - col.a) * stepOpacity * localOpacity;
                        
                        // 5. Arrêt rapide si opaque
                        if (col.a > 0.99) { 
                            col.rgb *= 1 / col.a; 
                            col.a = 1;
                            break;
                        }
                    }
                }
                
                // 4. Mélange final des nuages avec le ciel de fond
                float skyFog = 1 – (1 / (_FogSky * length(viewVector) + 1));
                float4 totalSkyColor = lerp(_SkyColor, _FogColor, skyFog);
                col += (1 - col.a) * totalSkyColor;
                
                return col;
            }
            ENDHLSL
        }
    }
}
```

-----

## VII. Questions Théoriques et Pratiques

### 1\. Shader de Matériau pour la Skybox

#### Théorique :

Le **shader de matériau pour la skybox** a pour rôle de définir l'environnement englobant la scène (le ciel, le sol et l'atmosphère lointaine). Il ne s'agit pas d'un simple arrière-plan, mais de la principale source d'**illumination et d'ambiance** pour la scène, influençant :

1.  **L'éclairage global :** La couleur du ciel (particulièrement près du soleil) est utilisée par l'illumination globale (GI) pour éclairer les objets de la scène (lumière indirecte).
2.  **La perception de l'espace :** Le dégradé du ciel et le niveau de brouillard atmosphérique renforcent la perspective et la profondeur (distance).
3.  **L'atmosphère/Tonalité :** La couleur, la densité et le mouvement des nuages déterminent l'ambiance émotionnelle (jour ensoleillé, crépuscule inquiétant, orage imminent).

#### Pratique : Cycle Jour-Nuit Dynamique

Pour simuler un cycle jour-nuit fluide, on utilise un script C\# qui manipule les propriétés du shader de skybox en fonction du temps (`Time.time` ou une variable de temps customisée).

**Techniques utilisées :**

1.  **Interpolation (`Lerp`) :** Utiliser `Mathf.Lerp` ou `Color.Lerp` pour effectuer des transitions fluides entre des valeurs prédéfinies (profils) pour le jour et la nuit.
2.  **Manipulation des paramètres :**
      * **Couleurs :** Faire varier `_SkyColor`, `_GroundColor` et `_FogColor` (Bleu clair/Orange pour le jour -\> Bleu foncé/Violet pour la nuit).
      * **Position de la lumière :** Lier la rotation d'une lumière directionnelle (le soleil) à la variable de temps. La couleur et l'intensité du soleil doivent également s'assombrir la nuit.
      * **Nuages :** Ajuster l'`_CloudOpacity` et `_CloudThickness`. La nuit, on peut éclaircir le ciel pour simuler une lune et assombrir la base des nuages.

-----

### 2\. Nuages Volumétriques et Ray Marching

#### Théorique :

La technique de **Ray Marching** est appliquée aux nuages en simulant le passage d'un rayon de vue à travers un **volume 3D** (défini par le bruit de Perlin et l'épaisseur du nuage). Au lieu de chercher un point d'impact unique, le rayon prend de multiples échantillons le long de sa trajectoire.

**Avantages du Ray Marching :**

  * **Détail Volumétrique :** Permet une simulation réaliste de la lumière traversant le nuage (ombrage interne, absorption, diffusion) qui est difficile à obtenir avec des textures 2D.
  * **Génération Procédurale :** Un code simple permet de générer une infinité de variations de nuages en changeant la graine du bruit ou les paramètres.
  * **Moins de Mémoire :** Évite de stocker d'énormes textures de volume 3D.

#### Pratique : Équilibrer Qualité et Performance / Autre système

**Considérations clés pour l'équilibre :**
Le principal goulot d'étranglement est le nombre d'itérations (`SAMPLES`).

  * **Haute Qualité :** Augmenter `SAMPLES` (ex: 64 ou 128), ce qui coûte cher.
  * **Optimisation :** Utiliser des techniques de **LOD (Level of Detail)** : réduire le nombre de `SAMPLES` pour les nuages lointains.

**Autre système de nuages volumétriques :**
On pourrait implémenter un système basé sur l'**échantillonnage de textures de bruit 3D** (Volume Textures) pour définir la densité interne du nuage, au lieu d'utiliser une texture 2D pour les surfaces supérieure et inférieure.

  * **Avantages :** Permet des formes de nuages plus complexes et irrégulières, comme les cumulus ou cirrus.
  * **Inconvénient :** La texture de volume 3D est très coûteuse en mémoire et en génération.

-----

### 3\. Impact du Bruit de Perlin sur Ray Marching et Nuages Volumétriques

#### Théorique :

Le **bruit de Perlin** est essentiel car il génère des motifs **cohérents** et **organiques** dans la nature. Dans le contexte des nuages :

  * **Réalisme :** Il permet des transitions douces et des structures hiérarchiques (petites ondulations dans de plus grandes masses), imitant la turbulence atmosphérique réelle.
  * **Complexité :** Il est souvent utilisé en combinaison d'octaves (différentes fréquences de bruit) pour superposer des détails fins sur des formes globales (fractalité).

#### Pratique : Ajustements pour des variations naturelles et alternatives

**Ajustements pour variation naturelle :**

1.  **Combinaison d'Octaves :** Utiliser plusieurs échantillons du bruit (à différentes échelles/fréquences) et les additionner pour enrichir le détail (comme dans l'exemple du terrain procédural).
2.  **Distorsion du Domaine (Domain Warping) :** Utiliser un bruit pour *déformer* les coordonnées d'échantillonnage d'un autre bruit, simulant un effet de vent ou de turbulence.

**Autre bruit pour améliorer l'aspect :**

  * **Bruit de Worley (Cellular Noise) :** Pour simuler des textures avec des "trous" ou des structures en cellules distinctes (nuages de types cirrus ou altocumulus). L'application d'un Bruit de Worley au SDF du nuage donne des bords plus nets et déchiquetés, augmentant le réalisme des formes distinctes.

-----

### 4\. Optimisation des Nuages Volumétriques

#### Théorique :

Le défi principal est la **performance** due au nombre élevé d'itérations du Ray Marching, surtout en haute résolution.

**Stratégies d'optimisation :**

1.  **Basse Résolution (Downsampling) :** Rendre les nuages à une résolution inférieure (ex: la moitié de la résolution de l'écran) et les ré-échantillonner (upsample) sur le résultat final.
2.  **Ray Marching Temporel :** Distribuer les calculs sur plusieurs images (time slicing). Par exemple, ne calculer qu'un quart des échantillons à chaque image, puis reprojeter le résultat de la frame précédente.
3.  **Masque de Nuages :** Ne pas calculer le Ray Marching pour les pixels où le ciel de fond est entièrement visible (en dehors de la zone occupée par les nuages).

#### Pratique : Solution d'optimisation (LOD Adaptatif)

**Solution : LOD Adaptatif basé sur la distance**

```c
// Pseudo-code d'optimisation dans le shader frag
// Ajout d'une propriété _MaxSamples et _DistanceFadeStart
float distToCamera = length(i.viewVector);

// Réduire les échantillons pour les nuages lointains
float fadeStart = _DistanceFadeStart;
float fadeRange = _DistanceFadeEnd - fadeStart;

// Facteur de réduction : 1.0 (proche) à 0.0 (loin)
float reductionFactor = saturate((distToCamera - fadeStart) / fadeRange);

// Le nombre d'échantillons est réduit pour les nuages lointains
int actualSamples = lerp(_MaxSamples, _MinSamples, reductionFactor); 
// Utiliser actualSamples dans la boucle for (int i = 0; i < actualSamples; i++)
```

Cette solution permet de **maintenir une haute fidélité visuelle** près du joueur (où le défaut est visible) tout en réduisant considérablement le coût pour les nuages à l'horizon.

-----

### 5\. Exploration Artistique des Nuages Volumétriques via Ray Marching

#### Théorique :

Le Ray Marching permet de dépasser le réalisme pour servir la **narration et l'émotion**. En manipulant les couleurs et les formes (via le bruit), l'artiste peut créer des ambiances uniques :

  * **Tension/Danger :** Nuages très denses, sombres, et rapides, utilisant des couleurs chaudes (rouge/orange) au lieu de gris.
  * **Sérénité/Calme :** Nuages légers, très doux, éclairés par une lumière très diffuse (peu de contraste).
  * **Monde Stylisé/Alien :** Utiliser des formes non naturelles (comme des nuages cubiques ou fractales) pour donner un ton unique à l'environnement.

#### Pratique : Scène narrative (Ambiance et Ton Émotionnel)

**Conception de Scène : Boss Fight Imminent**

  * **Objectif narratif :** Créer une sensation d'oppression et de pouvoir imminent.
  * **Techniques employées (via Ray Marching) :**
    1.  **Densité Extrême :** Augmenter fortement `_CloudThickness` et `_CloudOpacity` pour bloquer la lumière du soleil, rendant la scène au sol très sombre (Shadow Mask).
    2.  **Forme Agressive :** Utiliser un bruit de Perlin à haute fréquence ou du Bruit de Worley pour donner aux bords des nuages un aspect déchiqueté et menaçant.
    3.  **Couleur :** Définir `_GradientTex` (la rampe d'absorption) avec une couleur de base **rouge sombre/orange brûlé** pour simuler le rayonnement d'une force puissante cachée dans les nuages.
    4.  **Mouvement :** Augmenter la vitesse de défilement de la texture de bruit pour simuler des **vents de tempête** et donner une impression de mouvement et de chaos.

En coordonnées le sol avec cet environnement (par exemple, en ajoutant une faible lumière rouge émissive au sol), l'ambiance et le ton émotionnel sont fortement alignés sur l'idée d'une confrontation épique.