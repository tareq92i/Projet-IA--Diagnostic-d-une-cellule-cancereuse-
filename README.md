# Projet-IA--Diagnostic-d-une-cellule-cancereuse-

### **Informations Complémentaires**



Les caractéristiques sont calculées à partir d'une image numérisée d'un prélèvement par aspiration à l'aiguille fine (PAAF) d'une masse mammaire. Elles décrivent les caractéristiques des noyaux cellulaires présents dans l'image. Quelques images sont disponibles à l'adresse http://www.cs.wisc.edu/~street/images/. Le plan de séparation décrit précédemment a été obtenu par la méthode MSM-T (Multisurface Method-Tree) [KP Bennett, « Decision Tree Construction Via Linear Programming », Actes du 4e congrès de la Midwest Artificial Intelligence and Cognitive Science Society, p. 97-101, 1992], une méthode de classification qui utilise la programmation linéaire pour construire un arbre de décision. Les caractéristiques pertinentes ont été sélectionnées par une recherche exhaustive dans l'espace de 1 à 4 caractéristiques et 1 à 3 plans de séparation. Le programme linéaire utilisé pour obtenir le plan de séparation dans l'espace tridimensionnel est celui décrit dans : [KP Bennett et OL Mangasarian : « Robust Linear Programming Discrimination of Two Linearly Inseparable Sets », Optimization Methods and Software 1, 1992, 23-34]. Cette base de données est également accessible via le serveur FTP du département d'informatique de l'Université de Washington : ftp ftp.cs.wisc.edu cd math-prog/cpo-dataset/machine-learn/WDBC/

--------------------------------------------------------------------------------
En résumé
--------------------------------------------------------------------------------
Au lieu d'utiliser les modèles à base des CNN pour prédire notre cible, des algorithmes ont déjà été utilisé pour traduire les informations sur les images sous forme de données tabulaires. 

### **Description des données**


## Description des variables du dataset *Breast Cancer Wisconsin (Diagnostic)

### 1. Informations générales
- **ID number** : Identifiant unique de l’échantillon.  
- **Diagnosis** : Type de tumeur — **M** (*malignant*, maligne) ou **B** (*benign*, bénigne).



### 2. Caractéristiques cellulaires 

Chaque cellule est décrite à partir de **10 caractéristiques principales** calculées sur l’image numérique du noyau :


**radius** Moyenne des distances du centre au contour (taille globale) 

 **texture** ⁉ Écart-type des niveaux de gris (variation d’intensité)
 
**perimeter** : Longueur du contour cellulaire |

**area** : Surface de la cellule |

**smoothness** : Variation locale du rayon (régularité du contour) |

**compactness** : Degré de compacité *(perimeter² / area - 1)* |

**concavity** : Importance des creux dans le contour |

**concave points** : Nombre de creux (zones concaves) sur le contour |

**symmetry** : Symétrie du noyau |

**fractal dimension** : Complexité du contour (rugosité / forme irrégulière) 



### 3. Trois types de mesures pour chaque caractéristique

Chaque variable ci-dessus est mesurée selon **trois versions** :
- `*_mean` → **Valeur moyenne** sur l’image (ex. `radius_mean`)
- `*_se` → **Erreur standard** ou variation locale (ex. `radius_se`)
- `*_worst` → **Valeur maximale** observée (ex. `radius_worst`)

Ainsi, le dataset contient **10 caractéristiques × 3 mesures = 30 variables**,  
auxquelles s’ajoutent :
- `ID number`
- `Diagnosis`

 **Total : 32 colonnes.**



### Remarque
Les caractéristiques géométriques (*radius, perimeter, area, etc.*) permettent de distinguer les tumeurs bénignes (formes plus régulières et homogènes) des tumeurs malignes (formes irrégulières et plus complexes).


Historique à prendre en compte pour la présentation du projet

1. Comment les données ont été obtenues (imagerie médicale, PAAF, etc.)

    1. Prélèvement du tissu du sein (biopsie)

Lorsqu’une masse suspecte est détectée, un médecin réalise une biopsie :
 un petit échantillon de tissu du sein est prélevé.

Cet échantillon contient des cellules, certaines normales (bénignes) et parfois cancéreuses (malignes).

2. Observation des cellules au microscope

Les cellules prélevées sont ensuite :
-colorées pour bien voir leur forme
- placées sous un microscope numérique
- photographiées avec une caméra haute résolution

Ces images sont la base du dataset.

3. Extraction automatique de caractéristiques (feature extraction)

Un logiciel spécialisé analyse l’image pour mesurer la forme, la structure et la texture des noyaux cellulaires.
Ce sont les fameux attributs du dataset.

Les principales catégories de mesures sont :

*Forme

radius (rayon)

perimeter

area

compactness

concavity

* Texture

texture (variation d’intensité sur l’image)

smoothness (régularité du contour)

* Symétrie et régularité

symmetry

fractal_dimension

concave_points

4. Calcul des 3 versions : mean, se, worst

Pour chaque image, le logiciel analyse plusieurs cellules, pas juste une.
Et il en extrait trois valeurs pour chaque type de mesure :

A mean — moyenne

→ “À quoi ressemblent les cellules en général ?”

B se — erreur standard (standard error) ou variabilité locale (std/sqrt(n) avec n = nombre de cellules)

→ “À quel point les cellules sont-elles différentes les unes des autres ?”

C worst — valeurs les plus extrêmes

→ “Quelles sont les cellules les plus irrégulières ou suspectes ?”

Les tumeurs malignes ont souvent :

des cellules plus grandes

des contours irréguliers

une variabilité élevée

D'où l’importance de ces 3 niveaux.

5. Annotation de la tumeur : B (benign) ou M (malignant)

Finalement, un médecin spécialiste (pathologiste) vérifie la biopsie et attribue la classe cible :

M (Malignant) → tumeur cancéreuse

B (Benign) → tumeur non cancéreuse

Ensuite, cette variable est encodée (ex : B=0, M=1).

 6. Création du dataset final

On rassemble toutes ces informations dans un tableau :

radius_mean	radius_se	radius_worst	…	diagnosis
14.2	0.5	16.3	…	M
8.5	0.2	10.1	…	B

 Chaque ligne = une patiente
Chaque colonne = une caractéristique de l’image




# objectives de la prédiction 


1. Ce que détermine le dataset :
Ce n’est pas pour savoir s’il y a un cancer dans le corps.
 C’est pour déterminer si une tumeur déjà détectée est :
 Bénigne (non cancéreuse)

ou

Maligne (cancéreuse)

Donc :
Oui, c’est de la détection de cancer du sein, mais sur une tumeur déjà observée en imagerie ou palpée par un médecin.

 2. Pourquoi analyser les cellules ?

Quand une tumeur est trouvée, une biopsie est faite pour analyser les cellules au microscope.

Ce dataset utilise l’image du noyau cellulaire pour prédire si la tumeur est :

bénigne → cellules régulières, rondes, stables

maligne → cellules agressives, irrégulières, déformées

Le but du modèle est donc :
“Dire automatiquement si une tumeur du sein est cancéreuse ou non, à partir de l’image de ses cellules.”

3. Si le diagnostic est BENIN (B)

→ Pas de cancer
→ Pas de propagation
→ Souvent on surveille seulement, aucun traitement lourd
→ La tumeur peut rester ou être retirée si elle gêne

4. Si le diagnostic est MALIN (M)
là oui, c'est officiellement un cancer du sein

Une tumeur maligne peut se multiplier, envahir les tissus autour, et se propager dans l’organisme (métastases).

C’est pour ça que la suite est très importante.

5. Prochaine étape quand une tumeur est maligne

Une fois que la biopsie confirme que la tumeur est cancéreuse, le processus médical commence :

Étape 1 – Déterminer le stade du cancer (staging)

Le médecin mesure :

la taille de la tumeur

si elle touche les ganglions

si elle s’est propagée (métastases)

Étape 2 – Bilan complet

Exemples d’examens :

IRM du sein

mammographie

échographie

prise de sang

parfois scanner ou PET-scan

Étape 3 – Choix du traitement

Selon le stade, on peut avoir :

- Chirurgie

Enlever la tumeur (tumorectomie) ou parfois le sein (mastectomie)

- Radiothérapie

Détruire les cellules cancéreuses restantes

- Chimiothérapie

Traitement pour réduire ou éliminer la tumeur

- Hormonothérapie

Si la tumeur réagit aux hormones

- Immunothérapie

Stimuler le système immunitaire pour attaquer les cellules cancéreuses




# une tumeur c'est quoi ? 


1. Une tumeur n’est pas “une seule cellule”

Une tumeur est en réalité :

 un amas (une multiplication) de cellules anormales

qui se divisent trop rapidement et ne respectent plus le fonctionnement normal du tissu.

Donc :

- Une cellule devient anormale

- Elle perd son contrôle de croissance

- Elle commence à se multiplier

- Ces cellules s’accumulent

- L’ensemble forme une tumeur

2. Une cellule anormale → tumeur

Une tumeur commence toujours par une seule cellule qui a muté.
Cette cellule :

ignore les signaux pour arrêter de se diviser

se divise beaucoup plus vite que normal

transmet sa mutation aux cellules filles

Au bout d’un moment, on obtient une masse → la tumeur.

 3. Deux types de tumeurs
 Bénigne

→ cellules un peu anormales mais non agressives
→ elles restent localisées
→ elles ne détruisent pas les tissus autour
→ elles ne se propagent pas dans le corps

 Maligne (cancer)

→ cellules très anormales et agressives
→ elles envahissent les tissus voisins
→ elles peuvent migrer dans le sang ou la lymphe
→ métastases possibles

 4. Ce que le dataset mesure : les cellules qui composent la tumeur

Chaque variable (radius_mean, smoothness_se, compactness_worst, etc.) décrit :

la taille des cellules

leur forme

leur irrégularité

leur densité

leur symétrie

Le but est de voir si les cellules dans l’amas (la tumeur) ressemblent plus à :

des cellules normales / régulières → bénigne

des cellules déformées / agressives → maligne

Donc oui, la tumeur vient d’une cellule, mais ce que l’on analyse dans la réalité (et dans ton dataset) ce sont :

 les caractéristiques des cellules qui composent l’amas tumoral.


 Contexte du dataset

Le jeu de données Breast Cancer Wisconsin Diagnostic (ID=17 sur UCI) provient de photographies de cellules épithéliales du sein obtenues par aspiration à l’aiguille fine (FNA).

Chaque ligne du dataset correspond à un échantillon (un prélèvement d’une patiente).

Pour chaque échantillon, on prend une image microscopique.

Sur cette image, on identifie plusieurs cellules et on mesure différentes caractéristiques morphologiques (forme, texture, etc.).

D’où viennent les 30 variables ?

Sur chaque image, on mesure 10 caractéristiques de base des cellules :

radius

texture

perimeter

area

smoothness

compactness

concavity

concave points

symmetry

fractal dimension


pour chaque caractéristique, on calcule trois versions :

Suffixe	Signification	Interprétation
_mean	Moyenne des valeurs mesurées sur toutes les cellules de l’image	donne une idée générale de la forme des cellules
_se	Erreur standard (ou écart-type local)	mesure la variabilité des cellules dans l’échantillon
_worst	Valeur maximale observée parmi les cellules	mesure la cellule la plus anormale du groupe
Donc, pour répondre à ta question :

Est-ce qu’il y a plusieurs photos de cellule, ou bien plusieurs cellules sur une seule photo ?

C’est une seule photo par échantillon,
mais plusieurs cellules sont visibles dessus.

Chaque ligne du dataset représente :

→ une patiente
→ un échantillon de tissu (une photo microscopique)
→ et des statistiques calculées sur plusieurs cellules visibles dans cette photo.
