--- 
layout: post
title: Étude de haut niveau sur le langage de programmation Rust
permalink: blog/rustlanguage
date: '2026-01-10 21:15:55 -0500'
categories: cyber programmation langage rust
comments_id: 10
draft: true
---

# Le langage de programmation Rust

## introduction

Rust est annoncé depuis plusieurs années comme une alternative de langage bas niveau et sécurisé à C/C++. Clairement inspiré de ces langages pour la syntaxe par accolage (le couple *{, }* ), on trouve aussi des éléments de programmation fonctionnelle comme les fonctions anonymes (*closure*).

Par bas niveau, c'est un langage qui s'exécute directement sur le processeur, sans passer par une machine virtuelle comme Java/C#/Python, etc. Par sécurisé, ce sont les concepts intégrés au langage qui évite les principaux problèmes rencontrés avec le C/C++, à savoir les fuites mémoires (memory leak) ou les dépassements de buffer (stack/buffer overflow).

Curieux de mieux comprendre les innovations apportées par ce langage, voici mes principales notes dessus. C'est article présuppose une connaissance basique du C - chaine de compilation, inclusion de module tiers, définition de *struct* et *enum*, de la gestion de mémoire et de la gestion des pointeurs.

## Type de donnée de base

Rust, comme le langage C, fournis un certains nombre de type primaire. Ces types sont strictement définis en Rust, contrairement au C qui permettait des types de taille varié dans ses premières versions. C'est cependant dans l'usage des données et valeur que Rust commence à se démarquer du C.

### première innovation - la notion de propriété (ownership)

En C/C++, l'opérateur le plus critique pour gérer la mémoire est ***}***. Il définit la fin d'une portée (scope), et quand on le rencontre, le développeur consciencieux doit être capable de dire à qui appartient les ressources utilisées. En pratique, il est souvent compliqué de le savoir, surtout dans des bases de code complexes et maintenues sur plusieurs années.

Rust, par sa notion d'ownership, place cette problématique au cœur de son modèle de programmation. Si le développeur n'est pas explicite sur la portée d'allocation d'une variable, le programme ne compilera tout simplement pas. Cela limite énormement les problèmes de fuite mémoire trop fréquente en C/C++.

Enfin, Rust introduit la notion de *lifetime*, qui permet de savoir dans quelle portée une variable doit continuer d'exister, en particulier quand il y a passage de référence entre fonctions.

### Type de donnée utilisateur - struct, traits et générique

Pour définir un nouveau type de donnée, on utilise la construction *struct*. 
Ce type de donnée existe déjà en C. Cependant, ayant appris des bonnes pratiques de développement C, ou il est d'usage d'avoir une série de fonctions permettant d'appliquer un traitement spécifique aux données de la structure, Rust apporte une notation similaire à la programmation orientée objet, en associant des méthodes directement à la structure. On ne parle pas d'héritage ni ce polymorphisme, mais uniquement d'encapsulation.

Un appel typique en C ressemble à:

```
myType = struct {...}
function do_something_with_myType(myType, ...)
do_something_with_myType(myTypeInstance, ...)
```

Rust utilise une syntaxe plus proche de la syntaxe popularisé par le C++

```
myType = struct { ... };
impl myType = do_something(&self, ...) {...}
myTypeInstance.do_something
```
L'héritage et le polymorphisme sont rendu possible par les *traits*, permettant de partager la définition de méthode entre différentes structures, et la programmation générique (issue directement des *templates* c++), pour éviter la duplication de code pour différents type de données.


### type de donnée pour le flux d'exécution - enum

Comme le mentionne Kent Beck, un programme écrit dans le style procédural va choisir les différents possibilitée d'exécution avec une logique de condition, soit par la construction *if-then-else*, ou par de multiple instruction batis autour de *case*. 

De part ce type de construction, Rust se place clairement à ce niveau dans une logique de programmation procédurale. Cependant, la ou le langage C n'autorisait que les entiers pour définir les *enums*, Rust autorise n'importe quel type, que ce soit un type de base, ou un type *struct* utilisateur.

Les *enums* en **Rust** peuvent être considéré comme des *tagged union*, ou un type de donnée algébrique (algebraic data type). 

Un enum définit en Rust
```rust
enum Foo {
  Bar(i32),
  Baz(f32),
}
```

correspond à la définition suivante en langage C:

```cpp
enum Tag { TAG_BAR, TAG_BAZ };
typedef struct  {
  enum Tag tag;
  union {
    int bar;
    float baz;
  } Foo;
};
```

## Modularisation du code - Packages, crates et modules

Le langage C est souvent associé aux fichiers *MakeFile* pour la construction de logiciel. Néanmoins, la gestion des dépendances et des librairies tierce reste, malgré toute ces années, encore relativement anarchique. D'autres outils sont apparu sur le marché pour pallier les faiblesses initiales de la chaine de compilation de projet C/C++, mais aucun ne s'est vraiment imposé comme référence incontournable. 

D'autre part, en C, il faut répéter le même code redodant `ifndef...define...endif` dans l'en-tête des fichiers à inclure pour éviter que le compilateur ne sélectionne plusieurs fois le même fichier. Autant d'erreur potentielle pour un débutant. 

Rust vient directement avec ses propres outils pour gérer les projets. *Cargo* permet de lister les dépendances et de concentrer les cibles de builds (debug, test, final) en un seul fichier. La définition de module tiers est également standardisé, en permettant de sélectionner qu'est ce qui est public et ce qui reste privé, permettant une meilleure ségregation du code. S'il est possible d'obtenir un résultat similaire en C/C++, c'est au prix de contorsion du code qui rend l'ensemble plus difficile à comprendre. 

Un ajout intéressant dans le système de build cargo, est l'inclusion de la version minilale de Rust nécessaire pour compiler le projet, ainsi que la définition claire des dépendances et de leur numéro de version. Certains outils de l'univers C/C++ permettent ce genre de chose, mais Cargo vient par défaut à l'installation du compilateur et de la suite d'outils, et est de fait un standard dans l'écosystème Rust.

## Gestion des erreurs.

Aucun programme n'est immunisé contre les erreurs. Erreur logique ou erreur dans l'allocation des ressources. Chaque langage essaye d'y répondre à sa manière.

Le langage C utilise un code retour pour déterminer si une fonction retourne une erreur ou pas. Ce code est la plupart du temps un entier, caché parfois sous forme de constante. Le langage C++ a introduit la notion d'exception, visant à séparer le traitement voulu par l'utilisateur aux traitements à exécuter en cas d'erreur.

Rust fait un usage des enums pour répondre au même besoin. La bibliothèque standard inclus:

- *Option<T>*: Une énumération avec les variantes *Some(T)* (valeur présente) et *None* (aucune valeur) pour gérer l'absence potentielle d'une valeur, éliminant ainsi les erreurs de pointeur nul qui affectent le langage C et de nombreux autres langages.

- *Result<T, E>*: Une énumération avec les variantes *Ok(T)* (succès avec la valeur T) et *Err(E)* (échec avec l'erreur E) pour une gestion robuste des erreurs, forçant la reconnaissance explicite et la gestion des erreurs potentielles.

De nouveau, Rust inclut, par défaut, un mécanisme et des outils qui font soit partie des bonnes pratiques ou des conventions apportées par les années. En incluant directement dans la distribution du compilateur et de la librairie standard des mécanismes pour gérer ce type de situation, Rust simplifie la vie du développeur qui n'a pas besoin de s'assurer que son intention est bien comprise par les utilisateurs de son code. 

## conclusion

Rust concentre dans sa conception le résultat de décennie d'apprentissage et d'observation sur les faiblesses du C et du C++. Avec la notion d'"ownership", il limite drastiquement les problèmes de fuite mémoire qu'on peut observer en C/C++. Outre les autes innovations décrites dans ce documents, Rust vient également avec une librairie standard assez fournie, un écosystème permettant de publier ses librairies facilement.

Son plus gros ajout, à mon sens, vient du fait d'inclure, soit dans le langage, soit dans l'ecosystème inclus par défaut, des constructions, des façons de faire, ou des outils qui sont considérés comme des meilleures pratiques en C/C++, mais dont l'implémentaiton repose sur la bonne volonté du développeur dans ces langages, alors que Rust l'impose par défaut. 

## références

[Rust book](https://doc.rust-lang.org/book/)
[Rust Cheat sheet](https://cheats.rs/)
[Rust ugly syntax](https://matklad.github.io/2023/01/26/rusts-ugly-syntax.html)
