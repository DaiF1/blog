---
title: Le grand nettoyage
showDate: false
---

{{< katex >}}

Aujourd'hui, c'est journée nettoyage dans le village des Schtroumpfs !
Mais le Grand Schtroumpf a du mal à organiser et motiver tout le monde.
Il a besoin de toi pour l'aider !

## Des listes, plein de listes

### Répartition des taches

Tout d'abord, il a besoin de lister les gens intéressés pour chaque tâche.
Écris une fonction qui renvoie une liste avec les 4 noms donnés en paramètre.

Si l'un des noms de contient pas le mot "schtroumpf", ne l'ajoute pas à la liste.

```py
def repartition(schtroumpf1: str, schtroumpf2: str, schtroumpf3: str, schtroumpf4: str) -> []:
    pass # Remplace ça par ton code
```

Exemples : 

```py
liste = repartiontion("grand schtroumpf", "schtroumpf à lunettes", "schtroumpf grognon", "schtroumpf costaud")
print(liste)

liste = repartiontion("poisson", "schtroumpf bricoleur", "table", "schtroumpf maladroit")
print(liste)
```
```
[ "grand schtroumpf", "schtroumpf à lunettes", "schtroumpf grognon", "schtroumpf costaud" ]
[ "schtroumpf bricoleur", "schtroumpf maladroit" ]
```

### Échange de places

Certains schtroumpfs ne sont pas content de la répartion et souhaitent échanger
avec un autre.

Écris une fonction qui échange les 2 schtroumpfs aux positions données.
Si l'une des positions n'est pas dans la liste, ne fait rien.

```py
def echange(liste: list, position1: int, position2: int):
    pass # Remplace ça par ton code
```

```py
liste = ["schtroumpf bricoleur", "grand schtroumpf", "schtroumpf costaud"]
print(liste)

echange(0, 2)
print(liste)

echange(1, 12)
print(liste)
```
```
[ "schtroumpf bricoleur", "grand schtroumpf", "schtroumpf costaud" ]
[ "schtroumpf costaud", "grand schtroumpf", "schtroumpf bricoleur" ]
[ "schtroumpf costaud", "grand schtroumpf", "schtroumpf bricoleur" ]
```

### Étiquettes à l'envers

Schtroumpf à lunettes est chargé de réordonner le stockage de la bibliothèque.
Malheureusement, Schtroumpf maladroit est passé par là et a écrit à l'envers
le nom le tous les cartons. Aide le en écrivant une fonction permettant de
remettre tout ça à l'endroit.

```py
def renverse(etiquette: list) -> list:
    pass # Remplace ça par ton code
```

```py
etiquette = ['n', 'o', 'i', 't', 'c', 'i', 'f', '-', 'e', 'c', 'n', 'e', 'i', 'c', 's']
print(renverse(etiquette))
```
```
[ 's', 'c', 'i', 'e', 'n', 'c', 'e', '-', 'f', 'i', 'c', 't', 'i', 'o', 'n' ]
```

### Des bougies partout

Pendant que Schtroumpf à lunettes s'occupait des cartons, Schtroumpf farceur a
décidé de remplacer toutes les ampoules par des bougies. Écris une fonction qui
supprime toutes les "bougie" d'une bibliothèque.

```py
def suppression_bougies(bibliotheque: list):
    pass # Remplace ça par ton code
```

```py
bibliotheque = ["Le Schtroumpfissime", "bougie", "Les Schtroumpfs et le Cracoucass", "Histoires de Schtroumpfs", "bougie"]
suppression_bougies(bibliotheque)
print(bibliotheque)
```
```
[ "Le Schroumpfissime", "Les Schtroumpfs et le Cracoucass", "Histoires de Schtroumpfs" ]
```

### Ordre alphabétique

Maintenant que les bougies sont retirées, il est temps de réorganiser les livres.
Écris une méthode qui trie les éléments d'une liste en ordre alphabétique.

Bien entendu, il est interdit d'utiliser la fonction `sort`.

{{< alert "circle-info" >}}

Pour trier ta liste, tu peux utiliser un [Bubble Sort](https://fr.wikipedia.org/wiki/Tri_%C3%A0_bulles).

{{< /alert >}}

```py
def tri(bibliotheque: list):
    pass # Remplace ça par ton code
```

```py
bibliotheque = ["Histoires de Schtroumpfs", "Docteur Schtroumpf", "Salade de Schtroumpfs", "Schtroumpf le héros"]
tri(bibliotheque)
print(bibliotheque)
```
```
["Docteur Schtroumpf", "Histoires de Schtroumpfs", "Salade de Schtroumpfs", "Schtroumpf le héros"]
```

## Une dimension de plus

### La cuisine de Schtroumpf bricoleur

Schtroumpf maladroit s'est trompé en rangeant des ustensiles dans la cuisine 
du Schtroumpf cuisinier. Il a ajouté les clous qu'il devait rendre au Schtroumpf
bricoleur.

La cuisine est composée de plusieurs étagères, toutes représentées par une liste.
Voici un exemple de cuisine :

```py
[
    [ "ustensile 1", "ustensile 2" ],               # Étagère 1
    [ "ustensile 1", "ustensile 2", "ustensile 3" ] # Étagère 2
]
```

Aide le à compter le nombre de clous qu'il a égaré sur les étagères.

```py
def compte_clous(cuisine: list) -> list:
    pass # Remplace ça par ton code
```

```py
cuisine = [
    [ "fouet", "clou" ],
    [ "spatule", "clou", "clou" ],
    [ "clou" ]
]

print(compte_clous(cuisine))
```
```
4
```

### À la recherche des maisons de peinture

Schtroumpf farceur a encore fait des siennes, et recouvert certaines maisons
de schtroumpf de peinture. Liste les coordonnées de chaque maison à nettoyer.

On représente le village à l'aide d'une matrice :

```
[
    [ 0, 0, 1],
    [ 0, 1, 0],
    [ 0, 1, 1]
]
```

Chaque `1` correspond à une maison recouverte de peinture.

{{< alert "circle-info" >}}

Dans une matrice, on obtient l'élément à la position \\((x, y)\\) en faisant
`matrice[y][x]`.

{{< /alert >}}

```py
def trouve_maison_peinture(village) -> list:
    pass # Remplace ça par ton code
```

```py
village = [
    [ 0, 0, 1],
    [ 0, 1, 0],
    [ 0, 1, 1]
]
print(trouve_maison_peinture(village))
```
```
[ (0, 2), (1, 1), (2, 1), (2, 2) ]
```

### Rotation d'étagères

Schtroumpf bricoleur a construit une étagère pour aider au rangement, mais
celle-ci n'a pas été placée correctement. Encore un coup de Schtroumpf
maladroit !

L'étagère est placée sur le côté, ce qui change sa disposition.

Voici par exemple, une étagère à 2 colonnes et 3 étages :

```
[
    [ "emplacement 1", "emplacement 2"],
    [ "emplacement 3", "emplacement 4"],
    [ "emplacement 5", "emplacement 6"]
]
```

Celle-ci ayant été placée sur le côté, voici donc à quoi elle ressemble :

```
[
    [ "emplacement 1", "emplacement 2", "emplacement 3"],
    [ "emplacement 4", "emplacement 5", "emplacement 6"]
]
```

Écris une fonction pour remettre les étagères correctement.

```py
def rotation_etageres(etagere: list) -> list:
    pass # Remplace ça par ton code
```

```py
etagere = [
    [ "1, "2", "3" ],
    [ "4, "5", "6" ]
]

print(rotation_etageres(etagere))
```
```
[
    [ "1", "2" ],
    [ "3", "4" ],
    [ "5", "6" ]
]
```

## Les défis de Schtroumpf à lunettes

### Rangement d'étagères

Schtroumpf à lunettes a bientôt fini son rangement. Il lui reste qu'une liste
de livre à ranger.

Il souhaiterait la ranger sur une étagère aux dimensions spécifiques (nombre
colonnes/lignes)et que tu lui donne la disposition des livres sur l'étagère à
l'aide d'une matrice.

Si ce n'est pas possible, donne lui simplement une liste vide.

```py
def rangement_etageres(livres: list, nb_colonnes: int, nb_lignes: int) -> list:
    pass # Remplace ça par ton code
```

```py
livres = [ "livre 1", "livre 2", "livre 3", "livre 4", "livre 5", "livre 6" ]
print(livres, 2, 3)
print(livres, 3, 2)
print(livres, 3, 3)
```
```
[
  [ "livre 1", "livre 2" ],
  [ "livre 3", "livre 4" ],
  [ "livre 5", "livre 6" ]
]

[
  [ "livre 1", "livre 2", "livre 3" ],
  [ "livre 4", "livre 5", "livre 6" ]
]

[]
```

### Le nombre parfait

Schtroumpf à lunette vient de remarquer qu'un livre est manquant sur l'une des
étagères. Il va donc regarder le registre pour voir qui l'a emprunté.

Dans le registre, chaque livre est représenté par un nombre : son identifiant.
Schtroumpf à lunettes connait l'identifiant du livre manquant, mais a besoin
d'une méthode pour le trouver le plus rapidement possible.
Heureusement pour lui, les identifiant sont triés en ordre croissant.

Écris une méthode qui donne la position d'un nombre dans une liste à l'aide d'une
recherche dichotomique. Si le nombre n'existe pas dans la liste, renvoie `-1`.

Pour rappel, voici l'algorithme de la recherche dichotomique :
```
Prends l'élément au milieu de la liste.
S'il est égal, renvoyer la position
S'il est plus petit, recommencer sur la moitié gauche de la liste
S'il est plus grand, recommencer sur la moitié droite de la liste
S'arrêter quand on ne peut plus découper de moitié
```

```py
def recherche_dichotomique(registre: list) -> list:
    pass # Remplace ça par ton code
```

```py
registre = [ 1, 3, 4, 6, 11, 16, 20, 29, 31, 42, 50, 55, 57, 58, 63, 67 ]
print(recherche_dichotomique(42))
print(recherche_dichotomique(104))
```

```
9
-1
```

### Paiement optimal

Avant de partir, Schtroumpf à lunettes décide d'acheter un livre. Le soucis :
il n'y a pas de monnaie dans la caisse, il va donc devoir donner le montant
exact.

Écris un algorithme qui en fonction des pièces dans son porte-monnaie, liste
celles nécessaires pour payer.

Pour trouver la solution, il faut utiliser un *algorithme glouton*.
Voici la description de l'algorithme :

```
Tant qu'il y a de la monnaie à rendre
Prendre la plus grande pièce à disposition qui ne dépasse pas le montant à rendre
La retirer du porte-monnaie
```

```py
def paiement_optimal(porte_monnaie: list, montant: int) -> list:
    pass # Remplace ça par ton code
```

```py
porte_monnaie = [ 50, 50, 20, 10, 10, 10, 5, 1, 1 ]
print(paiement_optimal(porte_monnaie), 82)
```

```
[ 50, 20, 10, 1, 1 ]
```
