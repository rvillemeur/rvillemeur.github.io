--- 
layout: post
title: Étude de haut niveau sur le langage de programmation Rust
permalink: blog/rustlanguage
date: '2026-01-05 21:15:55 -0500'
categories: cyber programmation langage rust
comments_id: 10
draft: true
---

# Le langage de programmation Rust

## introduction

Rust est annoncé depuis plusieurs année comme une alternative de langage bas niveau et sécurisé à C/C++. Clairement inspiré de ces langages pour la syntaxe par accolage (le couple *{, }* ), on trouve aussi des éléments de programmation fonctionnelle comme les fonctions anonymes (*closure*).

Par bas niveau, c'est un langage qui s'exécute directement sur le processeur, sans passer par une machine virtuelle comme Java/C#/Python, etc.
Par sécurisé, ce sont les concepts intégrés au langage qui évite les principaux problèmes rencontrés avec le C/C++, à savoir les fuites mémoires (memory leak) ou les dépassements de buffer (stack/buffer overflow).
Curieux de mieux comprendre les innovations apportés par ce langagage, voici mes principales notes dessus.

## Type de donnée de base

## première innovation - la notion de propriété (ownership)

En C/C++, l'opérateur le plus critique pour gérer la mémoire est *}*. Il définit la fin d'une portée (scope), et quand on le rencontre, le développeur consciencieux doit être capable de dire à qui appartient les ressources utilisées. En pratique, il est compliqué de le savoir.

Rust, par sa notion d'ownership, place cette problématique au coeur de son modèle de programmation. Si le développeur n'est pas explicite sur la portée d'allocation d'une variable, le programme ne compilera tout simplement pas. Cela limite énormement les problèmes de fuite mémoire trop fréquente en C/C++.

Enfin, Rust introduit la notion de *lifetime*, qui permet de savoir dans quel portée une variable doit continuer d'exister, en particulier quand il y a passage de référence entre fonctions.

## Type de donnée utilisateur - struct, traits et générique

Pour définir un nouveau type de donnée, on utilise la construction *struct*. 
Ce type de donnée existe déja en C. Cependant, ayant appris des bonnes pratique de développement C, ou il est d'usage d'avoir une série de fonction permettant d'appliquer un traitement spécifique aux données de la structure, Rust apporte une notation similaire à la programmation orienté object, en associant des méthodes directement à la structure. On ne parle pas d'héritage ni ce polymorphisme, mais simplement d'encapsulation.

On passe alors de 

```
myType = struct {...}
function do_something_with_myType(myType, ...)
do_something_with_myType(myTypeInstance, ...)
```

à un syntaxe plus proche de la syntaxe popularisé par le C++

```
myType = struct { ... };
impl myType = do_something(&self, ...) {...}
myTypeInstance.do_something
```
L'héritage et le polymorhisme sont rendu possible par les *traits*, permettant de partager la définition de méthode entre différentes structure, et la programmation générique (issue directement des *templates* c++), pour éviter la duplication de code pour différents type de données.


## type de donnée pour le flux d'exécution - enum

Comme le mentionne Kent Beck, un programme écrit dans le style procédural va choisir les différents possibilité d'exécution avec une logique de condition, soit par la construction *if-then-else*, ou par de multiple instruction batis autour de *case*. 

De part ce type de construction, Rust se place clairement à ce niveau dans une logique de programmation procédurale. Cependant, la ou le langage C n'autorisait que les entiers pour définir les *enums*, Rust autorise n'importe quelle type, que ce soit un type de base, ou un type *struct* utilisateur.

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

## Gestion des erreurs.

Aucun programme n'est immunisé contre les erreurs. Erreur logique ou erreur dans l'allocation des ressources. Chaque langage essaye d'y répondre à sa manière.

Le langage C utilise un code retour pour déterminer si une fonction retourne une erreur ou pas. Ce code est la plupart du temps un entier, caché parfois sous forme de constante. Le langage C++ a introduit la notion d'exception, visant à séparer le traitement voulu par l'utilsateur aux traitement à exécuter en cas d'erreur.

Rust fait un usage des enums pour répondre au même besoin. La bibliothèque standard inclus:

- *Option<T>*: Une énumération avec les variantes *Some(T)* (valeur présente) et *None* (aucune valeur) pour gérer l'absence potentielle d'une valeur, éliminant ainsi les erreurs de pointeur nul qui affectent le langage C et de nombreux autres langages.

- *Result<T, E>*: Une énumération avec les variantes *Ok(T)* (succès avec la valeur T) et *Err(E)* (échec avec l'erreur E) pour une gestion robuste des erreurs, forçant la reconnaissance explicite et la gestion des erreurs potentielles.

## références

[Rust book](https://doc.rust-lang.org/book/)
[Rust Cheat sheet](https://cheats.rs/)
[Rust ugly syntax](https://matklad.github.io/2023/01/26/rusts-ugly-syntax.html)
