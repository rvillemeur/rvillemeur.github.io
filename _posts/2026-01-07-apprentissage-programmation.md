--- 
layout: post
title: Comment expliquer les fondamentaux en programmation
permalink: blog/fondamentauxprogrammation
date: '2026-01-07 21:15:55 -0500'
categories: programmation langage 
comments_id: 8
draft: false
---

Dans le cadre de la scolarité dans le programme français, on essaye de donner des notions de programmation aux élèves. Le langage utilisé, *Python* 🐍, est facile d'accès. Permettant de faire des scripts rapides pour illustrer certaines notions, il évite d'aborder trop vite les notions plus complexe de gestion de la mémoire comme en C/C++, ou des concepts comme la programmation procédurale, orienté objet ou fonctionnelle. 

Cependant, en discutant avec ma fille, j'ai pu voir qu'elle avait du mal à bien appréhender des notions de base en programmation. Au lycée, avec l'utilisation de fonction affine, de type `y = a.x + b`, largement étudiée en classe, la notion de fonction mathématique est déjà abordée. Il est alors relativement simple d'expliquer la notion de fonction ou de variable en Python.

En revanche, plusieurs exemples donnés introduisaient directement la notion de boucle. Boucle *for*, bouche *while*. Et sa question: À quoi ça peut bien servir ??? Ce n'est pas tant le concept de répétition que le manque d'exemple parlant qui posait problème. Les exemples présentés initialement montraient surtout Python et la programmation comme une calculatrice sur stéroïde. Comment expliquer après la transition du codage d'une fonction affine aux belles interfaces et applications actuelles, sans introduire la capacité de modélisation apporté par la programmation.

J'ai finalement trouvé une explication concrète et pratique en revenant aux fondamentaux. Plutôt que de chercher à expliquer une abstraction par une image par forcément parlante, j'ai pris comme base le premier programme qu'on cite comme exemple en programmation: le "hello world". Il faut se rappeler qu'un ordinateur, au plus bas niveau, ne fait que manipuler des entiers. C'est une fantastique machine à calculer, capable aujourd'hui de faire plusieurs millions de calculs par seconde. 

Cependant, pour pouvoir être utilisé autrement, il faut modéliser l'information, c'est-à-dire se mettre d'accord sur comment une suite de chiffre permet de représenter certaines choses. On a tellement pris l'habitude de voir du texte représenté sur un écran, qu'on en a oublié que même le texte nécessite d'être modélisé pour pouvoir être manipulé par un ordinateur. Si aujourd'hui le standard *Unicode* permet de représenter tout caractère existant, initialement, c'est le standard *Ascii* (American Standard Code for Information Interchange) qui s'est répandu.

Ce code associe à chaque lettre et plusieurs symboles (espace, saut à la ligne, etc.) un chiffre. L'ordinateur est capable de manipuler ces chiffres, et d'afficher ensuite le résultat sous forme de texte, donnant l'illusion de manipuler le texte directement.

Voici un extrait pour les premières lettres de l'alphabet. La référence complète est disponible [ici](https://en.wikipedia.org/wiki/ASCII)

| A  | B  | C  | D  | E  |
|----|----|----|----|----|
| 41 | 42 | 43 | 44 | 45 |
| a  | b  | c  | d  | e  |
| 61 | 62 | 63 | 64 | 65 |

Notre texte peut alors être vu comme:

```txt
+-+-+-+-+-+-+-+-+-+-+-+
|h|e|l|l|o| |w|o|r|l|d|
+-+-+-+-+-+-+-+-+-+-+-+
```

Transcrit en ascii, avec la valeur exprimée en [hexadécimale](https://fr.wikipedia.org/wiki/Syst%C3%A8me_hexad%C3%A9cimal).

```txt
+--+--+--+--+--+--+--+--+--+--+--+
|68|65|6C|6C|6F|20|78|6F|73|6C|64|
+--+--+--+--+--+--+--+--+--+--+--+
```

Pour le convertir en majuscule, on peut soustraire le nombre 32 (ou 20 en hexadécimal) à chaque lettre pour obtenir son équivalent minuscule/majuscule. **Attention**, le caractère *espace* ne doit pas être converti. On parcourt le tableau de caractère à l'aide d'une boucle utilisant chaque élément un par un, jusqu'à l'élément final. 

Voici un exemple de code: 

```python
tableau = ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd']

# Parcours par indice
for i in range(len(tableau)):
    ascii_val = ord(tableau[i]) #convertit un caractère en valeur ASCII

    # Conversion minuscule → majuscule via ASCII
    if 97 <= ascii_val <= 122:
        tableau[i] = chr(ascii_val - 32) #convertit une valeur ASCII en caractère
```

Le résultat serait alors

```txt
+--+--+--+--+--+--+--+--+--+--+--+
|48|45|4C|4C|4F|20|58|4F|53|4C|44|
+--+--+--+--+--+--+--+--+--+--+--+
```

L'ordinateur peut alors nous afficher à l'écran

```txt
+-+-+-+-+-+-+-+-+-+-+-+
|H|E|L|L|O| |W|O|R|L|D|
+-+-+-+-+-+-+-+-+-+-+-+
```

Un simple exemple comme celui-là permet d'illustrer un certain nombre de concept fondamentaux en programmation:
- la modélisation de donnée
- la notation hexadécimale
- le concept de tableaux
- le choix *if/else* 
- la possibilité de parcourir la chaine pour la transformer, par exemple en la convertissant en majuscule. 

D'un seul coup, j'ai pu voir dans son regard 💡 que ce qui était totalement abstrait prenait une tournure plus réelle 🎉


