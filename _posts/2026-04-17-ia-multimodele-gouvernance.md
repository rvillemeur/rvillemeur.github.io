---
layout: post
title: L'IA à plusieurs têtes — multiplicité des modèles et défis de gouvernance
permalink: blog/ia-multimodele-gouvernance
date: '2026-04-17 00:00:00 -0500'
categories: cyber risques IA gouvernance
comments_id: 8
draft: true
---

Pendant longtemps, choisir un outil d'IA revenait à choisir un fournisseur. ChatGPT, c'était OpenAI. Gemini, c'était Google. Mais ce temps est révolu. Les outils modernes combinent plusieurs modèles sous un seul toit — et ça change tout en matière de risques.

## L'ère des frontends multi-modèles

Des outils comme Cursor, GitHub Copilot, ou encore Perplexity ne s'appuient plus sur un seul modèle IA. Ils proposent un choix entre GPT-4, Claude, Gemini, Mistral, ou des modèles open-source, parfois en changeant de fournisseur selon la tâche ou le coût.

Dans le jargon juridique européen (notamment le cadre de l'AI Act et du RGPD), on parle de **sous-traitants IA** (*AI subprocessors*) : le vendeur de l'outil devient un intermédiaire qui délègue une partie du traitement à des tiers qu'il choisit, parfois sans en informer l'utilisateur final.

C'est exactement le même problème que le *shadow IT*, mais cette fois poussé par le fournisseur lui-même.

## Problème n°1 : quelle licence s'applique ?

Quand votre outil passe d'un modèle à l'autre selon les circonstances, les conditions d'utilisation (*terms of use*) changent aussi. Or, ces licences ne sont pas équivalentes.

- Certains modèles interdisent l'usage dans des secteurs réglementés (santé, finance, défense).
- D'autres autorisent le réentraînement sur vos données soumises, sauf opt-out explicite.
- Les licences open-source (LLaMA, Mistral) imposent leurs propres restrictions sur l'usage commercial.

Le problème : si votre outil bascule automatiquement sur un modèle dont vous ne connaissez pas les conditions, votre organisation peut se retrouver en violation sans le savoir.

**À faire** : exiger du fournisseur une liste exhaustive et à jour de ses sous-traitants IA (*subprocessor list*), avec les conditions d'utilisation associées à chacun.

## Problème n°2 : traçabilité et audit des données

Quand plusieurs modèles sont impliqués, comment savoir quelle donnée a transité par quel fournisseur ?

Ce n'est pas qu'une question théorique. Dans un contexte de conformité (RGPD, Loi 25 au Québec, HIPAA), vous devez être capable de répondre à : *"Quelles données personnelles ont été traitées, où, et par qui ?"*

Avec un frontend multi-modèles, cette réponse devient floue :

- Le modèle utilisé peut changer d'une session à l'autre, voire d'une requête à l'autre.
- Les logs fournis par le frontend n'identifient pas toujours le modèle sous-jacent.
- Les données peuvent transiter par des régions géographiques différentes selon le modèle sélectionné.

**À faire** : demander les journaux d'audit (*audit logs*) détaillés avec le modèle utilisé pour chaque requête, et vérifier que les accords de traitement de données (*DPA — Data Processing Agreement*) couvrent tous les sous-traitants listés.

## Problème n°3 : gouvernance et usage non validé

Le risque le plus insidieux est peut-être celui-ci : le fournisseur peut ajouter un nouveau modèle IA à son frontend sans en informer ses clients. Votre organisation se retrouve alors à utiliser un service qu'elle n'a jamais évalué, ni validé.

Ce schéma crée un angle mort dans votre programme de gestion des risques fournisseurs (*third-party risk management*) :

- L'outil a été évalué et approuvé. Mais les sous-traitants IA derrière lui ne l'ont pas été.
- Les équipes métier utilisent l'outil sans se douter que le moteur a changé.
- La sécurité et la conformité ne sont jamais consultées pour ces changements silencieux.

C'est la différence entre un contrat avec un architecte qui fait lui-même les travaux, et un architecte qui sous-traite chaque corps de métier à des entreprises que vous ne connaissez pas.

**À faire** : inclure dans les contrats avec les fournisseurs d'outils IA une clause de notification obligatoire avant tout changement ou ajout de sous-traitant IA, avec un délai d'évaluation pour l'organisation cliente.

## En résumé

| Risque | Question clé | Action recommandée |
|---|---|---|
| Légal / Licences | Quelle licence s'applique à chaque modèle ? | Obtenir la liste des subprocessors avec leurs conditions |
| Données / Traçabilité | Où vont mes données et quel modèle les traite ? | Exiger des audit logs par modèle + DPA complets |
| Gouvernance | Suis-je informé quand le fournisseur change de modèle ? | Clause contractuelle de notification préalable |

La multiplication des modèles IA n'est pas une mauvaise chose en soi. Mais elle déplace la complexité vers des zones que les organisations ne surveillent pas encore. Comme souvent en cybersécurité, le risque ne vient pas de ce qu'on voit — il vient de ce qu'on suppose déjà géré.
