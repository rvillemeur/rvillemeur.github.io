--- 
layout: post
title: Note sur le langage C, assembleur de haut niveau
permalink: blog/clanguage
date: '2026-01-06 21:15:55 -0500'
categories: cyber programmation langage c assembleur
comments_id: 9
draft: true
---

# Le langage de programmation C

## introduction

Incontournable de l'histoire de l'informatique, inventé dans les années 70, étroitement lié au système Unix/Linux, le langage de programmation C est toujours incontournable plus de 60 ans après son invention.

Il a inspiré de mutiples langages avec ses éléments de syntaxe, des structures de controles ( *for, while, if-then-else, switch-case*, etc.), et sa fameuse utilisation des accolades pour définir le périmètre (scope) d'exécution. 

Cet article vise à donner un aperçu du C, et pourquoi certains l'ont appelé un assembleur de haut niveau, alors que, de part sa proximité avec le matériel, il est essentiellement considéré comme un langage de bas-niveau dans l'informatique d'aujourd'hui. Je ne cherche pas à documenter de façon exhaustive la syntaxe du langage. Il y a suffisament de livre et de documentation pour quiconque cherche à maitriser les arcanes de ce langage. 

Mon objectif est essentiellement de couvrir comment les développeurs de l'époque ont dépassé les limites du langage assembleur pour introduire, à l'aide de compilateur, un langage de plus haut niveau, permettant de raisonner sur des abstractions plus avancées, tout en gardant la puissance et la vitesse d'exécution de l'assembleur.

## Les instructions de base d'un processeur et représentation de la mémoire.

La réprésentation ci-dessous est volontairement simpliste, et proche de ce qui ce faisait avec le premier processeur type 6800, que des processeurs plus moderne (comme le [mode protégé](https://en.wikipedia.org/wiki/Protected_mode)). Je ne traite pas non plus de la réprésentation petit ou grand boutisme (little ou big endians) des informations, même si cela dépend de la famille du processeur. Pour comprendre comment fonctionne un processeur, à partir du transistor, vous pouvez lire le livre "code" de Charles Petzold (Microsoft press - 978-0-13-790910-0) ou celui disponible sur le site [from nand to tetris](https://www.nand2tetris.org/)

Le modèle retenu correspond à cette représentation:

| addresse | valeur                                          |
|----------|-------------------------------------------------|
| 00000000 | 23 20 68 65 61 64 69 6E 67 20 31 2C 20 6F 6E 6C |
| 00000010 | 79 20 6F 6E 65 20 69 6E 20 61 20 64 6F 63 75 6D |
| 00000020 | 65 6E 74 0D 0A 23 23 20 68 65 61 64 69 6E 67 20 |
| ........ | 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 |

A chaque adresse mémoire correspond un octet (byte ou 8 bit, dont la valeur va de `00` à `FF`), et qui peut signifier aussi bien un champ de donnée qu'une instruction (opcode) du processeur. Un CPU de base ne peut faire la distinction entre donnée binaire de code ou de donnée, un mauvais référencement de pointeur peut entrainer la défaillance du programme en cours d'exécution en essayant d'exécuter du code qui n'en est pas ou n'est pas le bon (stack overflow). 

Un programme s'exécute de façon séquentielle, une instruction après l'autres.Un processeur, de base, fait finalement peu de chose:
- charger une valeur en mémoire ou dans un registre, par valeur directe ou indirecte.
- Additioner ou soustraires 2 nombres.
- Opérer un décalage de bit, vers la gauche ou la droite.
- comparer une valeur avec une autre.
- Sauter à un autre adresse mémoire

Les processeurs plus récents couvre la conversion de 8 à 32 ou 64 bits, offrent un mode protégé, et d'autres fonctionnalités avancé. Néanmoins, pour exécuter un programme, ses responsabilités se limitent très grossièrement à ces quelques familles d'opérations. Pour se faire une idée, la liste complète des instruction pour l'architecture [x86 et x86-64](https://en.wikipedia.org/wiki/List_of_x86_instructions). On y retrouve bien ses opérations de bases, bien qu'adaptés à la complexité des processeurs et des architectures actuelles.

Un développeur utilisant l'assembleur doit spécifier chacune de ces actions, ce qui nécessite un code extrèmenent verbeux, difficile à modifier et à faire évoluer. La syntaxe de langage assembleur est très proche des instructions du processeurs, ultra-concise, et très peu orienté vers la (re)lecture. 

## Évolution vers le C.

Le C est un langage compilé. Cette phase de compilation permet de traduire des instruction en langage assembleur qui seront à leur tour traduite en langage machine. La chaine de compilation ressemble à:

```txt
+-----------+					  +-----------+			    +----------+			   +-----------+		   +----------+ 
|fichier    |- C preprocessor ->  |fichier    |- compiler ->|fichier   |-- assembler ->|fichier    |- linker ->|executable|
|	code    |		gcc -E		  | processé  |	  gcc -S	|assembleur|    gcc -c	   | object    |  gcc -o   | final    |
|    source |                     |           |             |  (*.s)   |               |  (*.o)    |           |  (a.out) |
+-----------+					  +-----------+			    +----------+			   +-----------+		   +----------+
```

### La structure de données.

Le langage C fournis des type de base permettant de formaliser les types de base des assembleurs:
- void.
- int, float, double. 
- char.

Le type des variables est vérifié au moment de la compilation. Cependant, le langage autorise la conversion de type ( *casting* ) sans que le compilateur ne l'interdise, offrant au développeur la même liberté qu'en assembleur au détriment d'une sécurité plus stricte sur les types.

Les valeurs sont affectés par `variable = valeur`.
Le C fournis aussi les opérations de base `+, -, *, /, %, &, |, ^, ~, <<, >>`

Dans un programme un peu conséquent, on ne traite plus une valeur à la fois. Hormis les types de bases (nombres, caractères), on créé des représentation plus avancée en traitant différents type de données simultanément.

(e langage C à introduit la notion de structure ( *struct* ), pour représenter cette association de donnée permettant de représenter des informations plus riche.

```c
typedef struct {
    char name[50];
    int id;
    float salary;
} Person;
```

### constante et nombre magiques.

Un ordinateur ne peut que manipuler des nombres entiers. Il faut créer un modèle pour chaque abstraction que l'on souhaite utiliser. Cela peut se faire à l'aide de struct. Mais pour beaucoup d'élément, on utilise un nombre en décrétant que cela représente une constante et un certain type de données.

Pour des valeurs qui ne peuvent prendre que des variant prédéfinis, C introduit la notion d' *enum* . 
`enum jours_de_la_semaine { lundi, mardi, mercredi, jeudi, vendredi, samedi, dimanche };`

### Les structures de controles.

Un processeur peut faire des *jump* à certaines adresses mémoires après avoir comparés des valeurs. Les operateurs de comparaison se retrouvent en langage assembleur, (`==, !=, >, <, >=, <=, &&, ||, !`).

On peut l'utiliser pour modifier le flux d'exécution du programme basé sur le résultat de comparaison logiques:
- `if (test) opération1; else opération2;`
- `switch (valeur) { case cas1: [instruction; [break;] ] [default: [instruction; [break;] ] ] }`
- `return`, `continue`, et `break` permettent de modifier le flux après un test en quittant ou non le contexte d'exécution en cours. 

On a aussi introduit la répétition de code, par les structures de contrôles:
- `for (initialisation ; test ; itération) opération;`
- `while (test) opération;`
- `do opération; while (test);`

On peut enfin spéficier un point spécifique d'exécution avec l'instruction  avec le couple `etiquette: ` et *goto* `étiquette`. 

Chaque instruction contenu dans ses structures de controles peut être considéré comme une mini-fonction, dont le contexte d'état des variables est apporté par l'environnement d'exécution en cours.

### La découpe en sous-routine, et sous-programme

La encore, pour un programme un peu conséquent, on découpe la logique d'exécution. En assembleur, on spécifie directement l'adresse de la zone et du contexte d'exécution du programme. Le C introduit la notion de fonction, qui permet de délimniter le périmètre de certaines instructions, de passer des paramètres, sans avoir à spécifier tout manuellement.

```c
type identificateur(paramètres)
{
    ...   /* Instructions de la fonction. */
}
```

Le point d'entrée du programme est d'ailleurs une fonction, dont le nom est `main`. On trouve de ce fait dans tout programme sa déclaration `int main(argc, char* argv)`. Cette fonction retourne un entier qui pourra être collecté par le systèmes d'exploitation hôte du programme, et collecte les informations passé sur la ligne de commande dans les paramètres *argc* et *argv*.

Un autre élément du langage C, Le préprocesseur, assure une phase préliminaire de la compilation des programmes. Il permet principalement l'inclusion d'un segment de code source disponible dans un autre fichier (fichiers d'en-tête ou header), la substitution de chaînes de caractères (macro définition), ainsi que la compilation conditionnelle.

Enfin, comme en assembleur, il est possible de laisser ses commentaires (`/* commentaire */`) dans le code pour en améliorer la compréhension.

### La manipulation des données et des variables.


L'assembleur, de base, permet 2 accès aux données

1. Accès direct.
On déclare les variables directement `type identificateur;`, et leur contenu est détruit une fois le périmètre d'exécution (scope) terminé. Ce périmètre peut être une structure de controle, ou une fonction.

2. Accès indirect et l'utilisations des pointeurs.
* pointeur vers valeurs
* pointeur vers fonctions (zone mémoire de code exécutable).

Un pointeur référence simplement une adresse mémoire. Cette adresse permet soit d'idenfier une valeur (pointeur simple), ou d'obtenir l'adresse d'une valeur cible (pointeur de pointeur). Le pointeur de fonction permet d'adresser de façon dynamique quelle fonction appeler, et de palier la rigidité d'un appel aux fonctions par leur nom (et donc leur adresse fixe), permettant une forme de polymorphisme. 

```c
int i=0; /* Déclare une variable entière. */
int *pi; /* Déclare un pointeur sur un entier. */
pi=&i;   /* Initialise le pointeur avec l'adresse de cette variable. */
*pi = *pi+1; /* Effectue un calcul sur la variable pointée par pi*/
```

Les pointeurs permettent une allocation dynamique de la mémoire qu'il est possible de faire en assembleur, mais au prix d'un code fastidieux, sujet à erreur et répétitifs. Le C, cependant, force toujours le développeur à s'assurer que la mémoire alloué est libéré au bon moment, au prix sinon de fuite de mémoire ( *memory leak* )
```

A noter, l'existence du type *array*, permettant de définir un tableau, en pratique un ensemble de donnée contigüe en mémoire. Les données étant proche, il est possible d'y accéder soit par une syntaxe spécifique aux tableaux (`array[position]`) soit par les pointeurs (`adresse + position`).

## conclusions

Le langage C permet de transcrire dans un langage plus haut niveau des instructions finalement très proche du langage assembleur. Sa syntaxe universelle permet de traduire un même code source pour des architectures différentes. Lors de la compilation, il faudra tenir compte des différences d'architecture, en particulier sur le petit ou grand boutisme, la taille des champs de données, etc. Cependant, cette traduction se fait dans un langage plus avancé que l'assembleur, autorisant une portabilité de fait d'une architecture matérielle vers une autre. 
