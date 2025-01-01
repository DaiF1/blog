---
title: Fusion de maisons
summary: |
    Ou « peut-on créer un nombre infini d'objet 3D pour un *jeu de fusion* ? »
date: 2024-12-22
tags: ["image", "génération procédurale"]
authors:
    - "daif.fr"
---

Ma mère aime beaucoup jouer aux jeux mobiles que qu'on appelle « merge game ».

Pour ceux qui ne savent pas de quoi il s'agit, ce sont des jeux où il faut
fusionner 2 objets identiques pour en créer un nouveau.
Dans la plupart des cas, il y a des tâches à compléter qui vous demande de
réunir certains objets.

Il y a souvent une limite à la quantité d'objet que l'on peut fusionner.
Principalement car cela demande une grande quantité de personne pour designer
et dessiner autant d'objets.

La question est donc : peut-on automatiser ce processus ? Et peut-on créer un
jeu avec un nombre illimité d'objets ?

## Les outils

Pour ce projet, nous allons utiliser les bibliothèques python *gmsh*, ainsi que
*pygmsh* pour une syntaxe simplifiée.

Voici le lien des 2 repo:

{{< github repo="sasobadovinac/gmsh" >}}

<br/>

{{< github repo="nschloe/pygmsh" >}}

## Construire une maison

En guise d'objet infini, j'ai décidé de partir sur des maisons. Il nous faut
donc une maison !

### Le premier cube

Une maison, ça commence par un cube. Donc il nous faut commencer par créer un
cube à l'aide de *gmsh*. Voici le script python qui s'en charge :

```py
import gmsh
import pygmsh

# Démarre la description de l'objet
with pygmsh.occ.Geometry() as geom:
    # Ajoute un cube
    mesh = geom.add_box(
        (0, 0, 0), # position
        (1, 1, 1)  # scale
    )

    # Convertie la géométrie décrite en un vrai objet 3D
    geom.generate_mesh()

    # Sauvegarde l'objet dans le fichier 'out.msh'
    gmsh.write("out.msh")
    gmsh.clear()
```

En ouvrant le fichier `out.msh` avec *gmsh*, on obtient ce magnifique
petit cube :

{{< figure src="./images/cube.png" alt="a yellow cube" >}}

{{< alert "circle-info" >}}

Par défaut, *gmsh* affiche uniquement les triangles qui composent le cube.
Pour sélectionner les informations à afficher, il faut se rendre dans 
`Tools > Options > Mesh`.

{{< /alert >}}

### C'est mieux avec un toit

Notre maison a désormais besoin d'un toit. Malheureusement, aucune des formes
3D de bases ne correspond à ce que l'on cherche.

C'est pourquoi il nous faut utiliser l'*extrusion*.

#### Extrusion

Le but de l'extrusion est de transformer une forme 2D en forme 3D.
Par exemple, en transformant un carré en cube.

Voici les différentes étapes qui composent une extrusion :
1. faire une copie de l'objet que l'on veut extruder, et le bouger à la position désirée;
2. lier chaque sommet des 2 objets pour créer les faces manquantes.

{{< figure src="./images/extrude.gif" alt="extrusion d'un triangle" caption="Extrusion d'un triangle" >}}

#### Créer le toit

Avant de faire notre extrusion, il nous faut d'abord notre objet 2D.
On va donc commencer par dessiner un triangle au dessus de notre cube.

```py
triangle = geom.add_polygon([
    [-0.1, -0.1,   1], # On retire -0.1 de chaque côté du toit pour
    [-0.1,  1.1,   1], # avoir un petit décalage avec le cube de base
    [-0.1,  0.5, 1.5],
], mesh_size=1)
```

{{< figure src="./images/roof_triangle.png" alt="un cube avec un triangle au dessus" >}}

Maintenant, il faut faire cette fameuse extrusion. Heureusement, *pygmsh*
fournit une méthode pour faire cela :

```py
# La copie est bougée de 1.2 sur l'axe x
geom.extrude(triangle, (1.2, 0, 0))
```

Tout cela nous donne finalement notre toit :

{{< figure src="./images/cube_with_roof.png" alt="a cube with a roof" >}}

### Comment rentrer

Pour donner l'illusion de l'existence d'une porte, il nous faut creuser un trou
rectangulaire dans le cube pour insinuer sa présence.

Pour cela, il nous faut utiliser les *opérations booléennes*.

#### Opérations booléennes

Les opérations booléennes nous permettent 2 choses : de combiner, ou de creuser
des objets 3D.
Il existe 2 types d'opérations booléennes dont nous allons avoir besoin pour ce
projet :
- l'union, qui combine 2 objets 3D;
- la difference, qui sculpte un objet à l'intérieur de l'autre.

C'est identique à l'union et la différence entre 2 ensembles mathématiques :

{{< figure src="./images/boolean.gif" alt="schema de l'union et différence booléenne" caption="Union et différence booléenne" >}}

#### Créer la porte

Pour créer notre porte, nous allons utiliser la différence booléenne.
Il nous faut donc souligner la zone à retirer :

```py
door = geom.add_box(
    (0.4,   0,   0),
    (0.4, 0.1, 0.6)
)
```

Voici le résultat, avec uniquement les arêtes pour plus de visibilité :

{{< figure src="./images/house_wireframe.png" alt="A house" >}}

Une fois que l'espace à retirer est correctement délimité, il nous suffit de le
retirer avec la différence.

```py
geom.boolean_difference(mesh, door)
```

{{< figure src="./images/house.png" alt="Une maison" >}}

### Une maison complète

Voici le script complet qui génère la maison :

```py
import gmsh
import pygmsh

# Démarre la description de l'objet
with pygmsh.occ.Geometry() as geom:
    # Ajoute un cube
    mesh = geom.add_box(
        (0, 0, 0), # position
        (1, 1, 1)  # scale
    )

    # Crée la zone de la porte
    door = geom.add_box(
        (0.4,   0,   0),
        (0.4, 0.1, 0.6)
    )

    # Retire la du corps de la maison
    geom.boolean_difference(mesh, door)

    # Crée le triangle pour le toit
    triangle = geom.add_polygon([
        [-0.1, -0.1,   1],
        [-0.1,  1.1,   1],
        [-0.1,  0.5, 1.5],
    ], mesh_size=1)

    # Fait l'extrusion
    geom.extrude(triangle, (1.2, 0, 0))

    # Convertit la géométrie en un objet 3D
    geom.generate_mesh()

    # Sauvegarde l'objet dans le fichier 'out.msh'
    gmsh.write("out.msh")
    gmsh.clear()
```

## Définir un modèle de fusion

Maintenant que nous avons une maison complète, définissons un modèle
permettant de décrire n'importe quelle maison.

On considère notre petite maison comme étant une maison de *niveau 0*.

Voici donc les spécifications de la maison :
- une maison niveau 1 a une base plus large;
- une maison niveau 2 a un pot de fleur sur la facade;
- une maison niveau 3 a un petit balcon.

Lorsqu'elle atteint le niveau 4, une maison se retrouve avec un nouvel étage
de niveau 0.  
Le cycle contine ainsi : une maison de niveau 5 est composée d'un rez-de-chaussée
de niveau 3 et d'un étage de niveau 1, une maison de niveau 6 est composée d'un
rez-de-chaussée de niveau 3 et d'un éþage de niveau 2, etc.

### Une plus grande maison

Pour faire une maison plus large, il nous suffit de transformer notre cube de
base en un pavé.

Il faut aussi penser à considérer le changement de position de la porte.

{{< figure src="./images/house_lvl_1.png" alt="A level 1 house" >}}

### Un peu de verdure

Pour les plantes, on ajoute un pavé en guise de pot à l'avant de la maison,
puis 3 ellipsoïdes pour le remplir.

Les 3 variations d'ellipsoïdes sont visibles dans l'image ci-dessous.

{{< figure src="./images/house_lvl_2.png" alt="A level 2 house" >}}

### Un petit balcon

Pour le balcon, même principe. On combine des pavés à l'aide d'une union
booléenne :

{{< figure src="./images/house_lvl_3.png" alt="A level 3 house" >}}

### Nouvel étage

C'est là que les choses deviennent intéressantes.

Les nouveaux étages suivent les même règles décrites précédemment, mais avec
un petit détail en plus : on ne peut pas avoir de porte au 1er étage.

Pour garder un niveau de détail consistent, nous allons ajouter une fenêtre
avec la même méthode que pour la porte.

On commence par créer une forme 3D ressemblant à une fenêtre :

{{< figure src="./images/window.png" >}}

Et on la retire à l'aide d'une différence booléenne :

{{< figure src="./images/carved_window.png" >}}

Voici le code python associé :

```py
# Ajoute un cube pour l'étage
mesh = geom.add_box(
    (0, 0, 0), # position
    (1, 1, 1)  # scale
)

# Créer le contour de la fenêtre
window_frame = geom.add_box((0.2, -0.1, 0.25), (0.6, 0.15, 0.5))

# Verres de la fenêtre
glasslb = geom.add_box((0.225, -0.1, 0.275), (0.25, 0.2, 0.2))
glasslt = geom.add_box((0.525, -0.1, 0.275), (0.25, 0.2, 0.2))
glassrb = geom.add_box((0.225, -0.1, 0.525), (0.25, 0.2, 0.2))
glassrt = geom.add_box((0.525, -0.1, 0.525), (0.25, 0.2, 0.2))

# Combine le contour et les verres à l'aide d'une union booléenne
window = geom.boolean_union([window_frame, glasslb, glasslt, glassrb, glassrt])

# Retire la du cube à l'aide d'une différence
geom.boolean_difference(mesh, window)
```

Il nous manque un seul petit détail afin de rendre la maison complète.

À l'heure actuelle, il y a un grand espace vide à côté du dernier étage :

{{< figure src="./images/missing_roof.png" >}}

Pour pallier ce problème, il nous suffit de rajouter une contination du toit.

{{< figure src="./images/house_lvl_4.png" alt="Une maison niveau 4" caption="La maison niveau 4 finale" >}}

### Quelques variations

Pour la fin, nous allons ajouter quelques variations aux détails apportés par
les niveaux.

Nous allons alterner le côté où la fenêtre et le balcon sont créés en fonction
de l'étage.

Voici une maison de niveau 8 pour illustrer ces changements :

{{< figure src="./images/house_lvl_8.png" alt="Une maison niveau 8" >}}

## Notes de fin

Le script fonctionne plutôt bien, mais il n'est pas parfait.
Vous l'avez peut-être remarqué en ouvrant un fichier avec *gmsh*, mais la maison
générée est composée de bien trop de triangles pour la complexité du modèle.

Le script n'a aussi pas toujours de très bonnes performances en raison des
nombreuses opérations booléennes effectuées :

| Niveau de la maison | Temps (en sec) |
|---------------------|----------------|
| 0                   | 0.77           |
| 1                   | 1.06           |
| 2                   | 1.36           |
| 3                   | 1.53           |
| 4                   | 2.39           |
| 16                  | 8.98           |
| 100                 | 111.55         |

Si vous voulez vous attaquer à ces problèmes, vous pouvez trouver le script
final ici :

<style>
/* https://github.com/lonekorean/gist-syntax-themes */
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/d49b91b3/stylesheets/idle-fingers.css');

@import url('https://fonts.googleapis.com/css?family=Open+Sans');
body {
  font: 16px 'Open Sans', sans-serif;
}
body .gist .gist-file {
  border-color: #555 #555 #444
}
body .gist .gist-data {
  border-color: #555
}
body .gist .gist-meta {
  color: #ffffff;
  background: #373737; 
}
body .gist .gist-meta a {
  color: #ffffff
}
body .gist .gist-data .pl-s .pl-s1 {
  color: #a5c261
}
</style>
<script src="your-gist-embed"></script>

{{< gist DaiF1 e2d84b194dc2c893ada48777c9187b5b >}}
