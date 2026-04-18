---
layout: post
title: L'IA à plusieurs têtes — multiplicité des modèles et défis de gouvernance
permalink: blog/ia-multimodele-gouvernance
date: '2026-04-17 00:00:00 -0500'
categories: cyber risques IA gouvernance
comments_id: 11
draft: false
---

Au moment de l'arrivé sur le marché de ChatGPT, choisir un outil d'IA était relativement
simple. Les principaux éditeurs sortaient une version de leur modèle, toujours
plus puissant, Les outsiders se pressaient, et l'utilisateur pouvait choisir
entre Claude, ChatGPT, DeepSeek, Gemini, Mistral, etc.  Mais ce temps est
révolu. Les outils modernes combinent plusieurs modèles sous un seul toit — et
ça change tout en matière de risques.

## L'ère des frontends multi-modèles

Des outils comme Cursor, Adobe, Atlassian, Miro, Canva, etc. ne s'appuient plus
sur un seul modèle IA. Ils proposent un choix entre GPT-4, Claude, Gemini,
Mistral, ou des modèles open-source, parfois en changeant de fournisseur selon
la tâche ou le coût. Certains se spécialisent en simple front-end permettant de
lier les modèles les uns aux autres dans le flux d'exécution.

Dans le jargon juridique on parle de **sous-traitants IA** (*AI subprocessors*):
le vendeur de l'outil devient un intermédiaire qui délègue une partie du
traitement à des tiers qu'il choisit, souvent sans en informer l'utilisateur
final.

Des nouveaux modèles peuvent être ajoutés, mis à jours, sans que vous en soyez informé.
Les employés, pas forcément au courant de ce type de subtilité, peuvent commencer à 
utiliser ces modèles, simplement parce qu'ils sont disponible, sans se soucier des 
conséquences que cela peut avoir. 

C'est exactement le même problème que le *shadow IT*, mais cette fois poussé par
le fournisseur lui-même.

## Problème n°1 : quelle licence s'applique ?

Quand votre outil passe d'un modèle à l'autre selon les circonstances, les
conditions d'utilisation (*terms of use*) changent aussi. Or, ces licences ne
sont pas équivalentes.

- Certains modèles interdisent l'usage dans des secteurs réglementés (santé, finance, défense).
- D'autres autorisent le réentraînement sur vos données soumises, sauf opt-out explicite.
- Les licences open-source (LLaMA, Mistral) imposent leurs propres restrictions sur l'usage commercial.

Le problème : si votre outil bascule automatiquement sur un modèle dont vous ne
connaissez pas les conditions, votre organisation peut se retrouver en violation
sans le savoir. Vous pouvez aussi mettre à risque votre propriété intellectuelle si
vous n'êtes pas adéquatement protégé par les clauses juridiques appropriées. 

**À faire** : exiger du fournisseur une liste exhaustive et à jour de ses
sous-traitants IA (*subprocessor list*), avec les conditions d'utilisation
associées à chacun (**SPOILER**: c'est un travail de recherche ardu qui complexifie
encore davantage l'évaluation du risque de Tiers).

## Problème n°2 : traçabilité et audit des données

Quand plusieurs modèles sont impliqués, comment savoir quelle donnée a transité
par quel fournisseur ?  Impossible de savoir aussi ou sont traité ces données.
Dans notre contexte actuel ou on parle de plus en plus de cloud souverain, la
zone géographique de traitement des données est tout aussi importante

Ce n'est pas qu'une question théorique. Dans un contexte de conformité (RGPD,
Loi 25 au Québec, HIPAA), vous devez être capable de répondre à : *"Quelles
données personnelles ont été traitées, où, et par qui ?"*

Demander les journaux d'audit (*audit logs*) détaillés avec le modèle utilisé
pour chaque requête est illusoire avec des solutions tournant exclusivement dans
le cloud, et des hyperscalers pas très motivés pour fournir cette information.
En pratique, ces informations sont souvent impossible à identifier et obtenir. 

Avec un frontend multi-modèles, vous vous retrouvez dans la situation suivante: 

- Le modèle utilisé peut changer d'une session à l'autre, voire d'une requête à l'autre.
- Les logs (si fournis par le frontend) n'identifient pas toujours le modèle sous-jacent.
- Les données peuvent transiter par des régions géographiques différentes selon le modèle sélectionné.


**À faire** :  Vérifier que les accords de traitement de données (*DPA — Data Processing Agreement*) couvrent tous les sous-traitants listés.
## Problème n°3 : gouvernance et usage non validé

Le risque le plus insidieux est peut-être celui-ci : le fournisseur peut ajouter
un nouveau modèle IA à son frontend sans en informer ses clients. Votre
organisation se retrouve alors à utiliser un service qu'elle n'a jamais évalué,
ni validé.

Ce schéma crée un angle mort dans votre programme de gestion des risques
fournisseurs (*third-party risk management*) :

- L'outil a été évalué et approuvé. Mais les sous-traitants IA derrière lui ne l'ont pas été.
- Les équipes métier utilisent l'outil sans se douter que le moteur a changé.
- La sécurité et la conformité ne sont jamais consultées pour ces changements silencieux.
- Votre département juridique doit approuver les clauses d'utilisation de ces modèles.

**À faire** : inclure dans les contrats avec les fournisseurs d'outils IA une clause de notification obligatoire avant tout changement ou ajout de sous-traitant IA, avec un délai d'évaluation pour l'organisation cliente.

## En résumé

| Risque | Question clé | Action recommandée |
|---|---|---|
| Légal / Licences | Quelle licence s'applique à chaque modèle ? | Obtenir la liste des subprocessors avec leurs conditions |
| Données / Traçabilité | Où vont mes données et quel modèle les traite ? | Essayer d'obtenir des  audit logs par modèle + DPA complets |
| Gouvernance | Suis-je informé quand le fournisseur change de modèle ? | Clause contractuelle de notification préalable |

La multiplication des modèles IA n'est pas une mauvaise chose en soi. Mais elle
déplace la complexité vers des zones que les organisations ne surveillent pas
encore. Comme souvent en cybersécurité, le risque ne vient pas de ce qu'on voit
— il vient de ce qu'on suppose déjà géré.
