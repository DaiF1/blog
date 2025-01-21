---
title: Notions de C
showDate: false
summary: |
    Cours et exercices sur quelques notions de base du C pour des programmeurs
    venant du Python.
authors: 
    - "daif.fr"
---

## Structures de base

### Déclarations de variables

En C, les variables sont typées. Pour déclarer une variable, on indique donc
son type comme ceci :

```c
int ma_variable = 1;
```

{{< alert "circle-info" >}}

Toute instruction en C doit se finir par un `;`. D'où sa présence dans l'exemple
ci-dessus.

{{< /alert >}}

Voici une liste des types usuels :

- `void` signifie "n'a pas de type", est utilisé uniquement dans le cas des
    fonctions qui n'ont pas de type de retour;
- `char` décrit les caractères;
- `int` décrit les entiers;
- `float` décrit les flottants simple précision (sur 32 bits);
- `double` décrit les flottants double précision (sur 64 bits).

Il existe en réalité plusieurs types pour les entiers : `short`, `int`, `long`
et `long long`.

Le nombre d'octet pour chaque type n'est pas fixe et dépend de l'architecture
du PC.

Voici une table indiquant leurs différences :

| Nom         | Nombre de bits |
|-------------|----------------|
| `short`     | 16             |
| `int`       | 16 ou 32       |
| `long`      | 32 ou 64       |
| `long long` | 64             |

{{< alert "circle-info" >}}

Les cas où la taille importe sont rares en dehors de la lecture
et du parsing de fichiers. On utilise donc `int` dans la majorité des cas.

{{< /alert >}}

Par défaut, les types sont signés (on peut leur donner une valeur négative).
Si l'on veut retirer le signe, il faut mettre `unsigned` devant le type :

```c
int un_nombre_signé = -1;
unsigned int un_nombre_non_signé = 1;
```

### Structures de contrôle

Comme la majorité des langages de programmation, le C fournit les structures
suivantes : les `if`, `for` et `while`.

Voici la syntaxe pour chacune de ces structures :

```c
int i = 0;
if (i == 0)
{
    // Ceci est un commentaire
}
else if (i == 1)
{
    // Ceci est un commentaire
}
else
{
    // Ceci est un commentaire
}
```

{{< alert "circle-info" >}}

Dans le cas des conditions, les mots clés Python `and`, `or` et `not` se
traduisent par `&&`, `||` et `!`.

{{< /alert >}}

```c
for (int i = 0; i < 4; i++) // L'équivalent de `for i in range(0, 4, 1)`
{
    // Ceci est un commentaire
}
```

{{< alert "circle-info" >}}

L'instruction `variable++` revient à faire `variable += 1`.

{{< /alert >}}

```c
int a = 0;
while (a < 4)
{
    a += 1;
}
```

À noter que les accolades sont optionelles si le bloc ne contient qu'une seule ligne.

Aussi, contrairement au Python, il n'y a aucune règle sur les tabulation
(même si c'est mieux de s'en imposer pour la clarté du code)

### Compilation

Un programme C doit toujours contenir une fonction `main`. Voici à quoi elle
ressemble :

```c
int main(int argc, char **argv)
{
    return 0;
}
```

Voici à quoi correspondent les différents arguments :
- `argc` correspond au nombre d'arguments passé au programme;
- `argv` correspond à la liste des arguments;

{{< alert "circle-info" >}}

Le retour de la fonction correspond au code de statut du programme. Par
convention, il s'agit de `1` en cas d'erreur, `0` dans les autres cas.

{{< /alert >}}

Pour transformer notre programme en executable, on utilise `gcc`. Voici la
commande à utiliser pour compiler notre programme :

```sh
gcc main.c -o main
```

Cette commande génère un fichier `main` qui peut ensuite être lancé comme ceci :

```sh
./main
```

Les arguments sont placés à la suite, comme ce serait le cas d'une commande bash.

```sh
./main arg1 arg2 arg3
```

La commande ci-dessus donne donc :

| argv      | Valeur   |
|-----------|----------|
| `argv[0]` | "./main" |
| `argv[1]` | "arg1"   |
| `argv[2]` | "arg2"   |
| `argv[3]` | "arg3"   |

### Inclure une bibliothèque

La plupart des méthodes de la bibliothèque standard doivent être incluses en
haut du programme. Le détail de ces fonctions peut être obtenu à l'aide de la
commande :

```sh
man la_fonction
```

Pour inclure une bibliothèque, on écrit :

```c
#include <ma_bibliothèque>
```

Par exemple, la commande `printf` est dans la bibliothèque `stdio.h`. Voici
donc un programme qui affiche *"Hello World!"* sur la console :

```c
#include <stdio.h>

int main(int argc, char **argv)
{
    printf("Hello World!\n");
    return 0;
}
```

{{< alert "circle-info" >}}

Par défaut, la commande `printf` ne fait pas de retour à la ligne.
Il faut donc l'ajouter manuellement à l'aide d'un `\n`.

{{< /alert >}}

Dans le cas où l'inclusion d'une bibliothèque a été oublié, `gcc` renvoie une
erreur lors qu'on tente de compiler le programme. Dans l'exemple ci-dessus,
voici ce que donne `gcc` si l'on oublie d'inclure `stdio.h` :

```sh
main.c: In function ‘main’:
main.c:3:5: error: implicit declaration of function ‘printf’ [-Wimplicit-function-declaration]
    3 |     printf("Hello World!\n");
      |     ^~~~~~
main.c:1:1: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’
  +++ |+#include <stdio.h>
    1 | int main(int argc, char **argv)
main.c:3:5: warning: incompatible implicit declaration of built-in function ‘printf’ [-Wbuiltin-declaration-mismatch]
    3 |     printf("Hello World!\n");
      |     ^~~~~~
main.c:3:5: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’
```

### Exercice : factorial

Écris une fonction `factorial` qui affiche `i!` pour `i` allant de 1 à `n`.
Chaque nombre doit être séparé par un espace et l'affichage se termine par un
`\n`.

```c
void factorial(int n) { }
```

```c
#include <stdlib.h>

int main(int argc, char **argv)
{
    if (argc != 2)
        return 1;

    // récupère le premier argument et le convertit en int
    int n = strtol(argv[1], NULL, 10);
    factorial(n);
    return 0;
}
```

```sh
sh$ gcc factorial.c -o factorial
sh$ ./factorial 5 | cat -e
1 2 6 24 120$
```

{{< alert "circle-info" >}}

Pour afficher un nombre avec `printf`, tu peux écrire `printf("%d", mon_nombre)`.

{{< /alert >}}

## Listes

En C, les listes ont une taille fixé à l'avance, ainsi qu'un type pour ses
éléments. Voici comment déclarer des listes :

```c
int[10] une_liste_de_10_entiers = {};
char[5] une_liste_de_5_caracteres = {};
```

{{< alert "circle-info" >}}

Il est très important d'ajouter le `= {}` pour initialiser la liste.
Un liste non initialisée peut générer des bugs et/ou des erreurs d'exécution.

{{< /alert >}}

Il est possible de donner une valeur par défaut aux éléments de la liste. Toutes
les valeurs non indiquées sont mises à 0 :

```c
int[5] liste = { 1, 2, 3, 4 };
printf("%d\n", liste[4]); // Affiche 0
```

{{< alert "circle-info" >}}

Dans le cas d'une liste pré-remplie, le nombre d'éléments peut être omis.

{{< /alert >}}

La taille d'une liste ne peut pas être calculée une fois celle-ci créée. C'est
pourquoi elle est souvent accompagnée d'un `size_t` (`unsigned int`, défini dans
la bibliothèque `stdlib.h`) stockant la taille de la liste :

```c
int[] liste = { 1, 2, 3, 4, 5 };
size_t longueur = 5;
```

### Exercice : my_print

Écris une fonction `my_print` qui affiche les caractères d'une liste sur la
console. Un `\n` doit être affiché à la fin de la liste. 

```c
void my_print(char[] liste, size_t lon) { }
```

```c
int main(int argc, char **argv)
{
    char[] liste = { 'H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd' , '!' };
    my_print(liste, 12);
    return 0;
}
```

```sh
sh$ gcc my_print.c -o my_print
sh$ ./my_print | cat -e
Hello World!$
```

## Chaines de caractère

Une chaine de caractère, c'est simplement une liste de caractères :

```c
char[] ma_chaine = "Bonjour !";
```

Comme il s'agit d'une liste, on ne connait donc pas la taille de la chaine.

Pour pallier ce problème, une chaine finit toujours par le caractère ayant la
valeur `0` (ou `'\000'`).

Les 2 listes suivantes sont donc équivalentes :

```c
char[] ma_chaine = "toto";
char[] ma_liste = { 't', 'o', 't', 'o', 0 };
```

### Exercice : my_strlen

Écris une fonction `my_strlen` qui renvoie la taille de la chaine de caractère
passée en paramètre.

```c
size_t my_strlen(char[] chaine) { }
```

```c
char[] chaine1 = "Hello World!";
printf("%d\n", my_strlen(chaine1)); // Affiche 12
```

## Pointeurs

### Organisation d'une mémoire

La mémoire d'un ordinateur est simplement une immense liste. Lors de l'execution
d'un programme, toute variable est associée à une adresse, l'indice de la
variable dans la mémoire.

{{< figure src="./example_pointer.png" caption="Représentation en mémoire d'une variable `a`" >}}

### Récupérer une adresse

Un pointeur, c'est l'adresse d'un élément en mémoire. En C, on peut récupérer
l'adresse de nos variables comme ceci :

```c
int a = 3; // Une variable a
int *addr = &a; // addr stocke l'adresse de a dans la mémoire
```

On remarque assez vite qu'un pointeur s'écrit comme ceci :
```
type_stocké_en_mémoire *
```

Dans l'exemple ci-dessus, `addr` est une variable qui doit être stockée en
mémoire. On peut donc récupérer son adresse comme ceci :

```c
int **addr2 = &addr;
```

La double étoile d'`addr2` indique que ce qui est stocké en mémoire correspond
à l'adresse d'un entier.

### Accéder à la mémoire

Une fois un pointeur en notre possesion, on peut lire la valeur stockée à cet
endroit de la mémoire :

```c
int *p; // Un pointeur vers un nombre
int valeur = *p; // La valeur stockée à l'adresse `p`
```

### Listes à taille variable

Les pointeurs sont souvent utilisés pour représenter des listes à taille
variable. La méthode consiste à garder l'adresse du premier élément de la liste,
puis de stocker les autres éléments juste après dans la mémoire.

Comme les éléments se suivent, l'adresse du suivant correspond
simplement à l'adresse de l'élément + 1.

Pour créer notre liste, on commence par réserver (ou allouer) un espace dans la
mémoire afin de stocker nos éléments. Pour cela, on utilise la fonction `malloc`
(dans la bibliothèque `stdlib.h`).

```c
int *mon_adresse = malloc(/* nombre d'octets que l'on veut réserver */);
```

Pour calculer le nombre d'octets de la liste, on utilise la fonction `sizeof`.
Celle-ci renvoie la taille des différents types de variables.

```c
// Réserve de la place pour stocker 4 int
int *mon_adresse = malloc(sizeof(int) * 4);
```

Une fois notre mémoire réservée, on peut accéder aux éléments comme ceci :

```c
int premier_elt = mon_adresse[0];
int deuxième_elt = mon_adresse[1];
// etc
```

{{< alert "circle-info" >}}

Lorsque l'on écrit `adresse[1]`, le C fait automatiquement la conversion à
`*(adresse + 1)` pour accéder au bon élément.

{{< /alert >}}

### Libérer la mémoire

Une fois qu'on en a terminé avec notre liste, il est important de libérer notre
mémoire afin qu'un autre programme puisse utiliser cet espace.

Pour cela on utilise la fonction `free` :

```c
int *addr;
free(addr); // Libère la mémoire réservée à l'adresse `addr`
```

{{< alert "circle-info" >}}

Bien que toute la mémoire allouée soit automatiquement libérée à la fin du
programme, un exécutable qui consomme trop de mémoire sans la libérer sera
automatiquement arrêté par le système d'exploitation. Sous Linux, c'est un
programme appelé *OOM Killer* qui en est chargé.

{{< /alert >}}

### Exercice : create_list

Écris une fonction `create_list` qui crée une liste de `n` éléments, et qui
stocke les nombres de `1` à `n`. Renvoie l'adresse de cette nouvelle liste.

```c
int *create_list(size_t n) { }
```

```c
#include <stdlib.h>

int main(int argc, char **argv)
{
    if (argc != 2)
        return 1;

    // récupère le premier argument et le convertit en int
    int n = strtol(argv[1], NULL, 10);
    int *liste = create_list(n);

    for (int i = 0; i < n; i++)
        printf("%d\n", liste[i]);

    free(liste);
    return 0;
}
```

```sh
sh$ gcc create_list.c -o create_list
sh$ ./create_list 4 | cat -e
1$
2$
3$
4$
```
