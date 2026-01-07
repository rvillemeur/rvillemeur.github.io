---
layout: post
title: êtes vous un dentiste de l'IA ?
permalink: blog/dentisteIA
date: '2025-11-13 21:15:55 -0500'
categories: cyber risques IA
comments_id: 8
draft: true
---

L'IA se retrouve partout.
On parle surtout les LLMs, mais il faut aussi prendre en compte les modèles open-sources qu'on peut faire tourner sur des instances locales.

Quand l'IA générative est apparu sur le marché, j'avais l'impression de voir un gigantesque paquet de bonbon pour les créatifs que je cotoyais dans l'univers du jeu vidéo. C'était Halloween tous les jours. Or, comme avec les bonbons, un manque d'hygiène peut entrainer des effets secondaires indésirables, et la visite régulière chez le dentiste pour corriger ou réparer ces conséquences.

En gestion des risques:

## IA locale
* IA  locale, mais quid des modèles de données utiliser (origine, droit d'auteur, license d'utilisation).
On parle de bulle IA, mais les IAs locale ne vont pas disparaitre tout de suite. Certains se positionne comme des fournisseurs logiciel traditionnel (ex: TabNine, Comfy UI). Ces solutions continueront de perdurer même si la bulle apparente sur les data-centers IA explose par manque de rentabilité.

## IA Cloud
* IA cloud, on perd totalement le controle, et on doit se fier aux conditions d'utilisations, et à la bonne volonté du fournisseur de ne pas utiliser 
d'avantage les données fournies

Service SAAS, le plus connu, on se branche directement sur le fournisseur. Les plus connu, Gemini, Anthropic Claude, ChatGPT.
- Problème de shadow AI après le shadow IT, problème de sécurité des données avec les informations confidentielles.
- Problème de la qualité des réponses, et des "halucinations" des IA.
- Problème sur les licences "terms of use" qui peuvent changer sans préavis.
- Problème de conformité, on ne sait pas ou les données sont traitées. Ex avec Copilot, qui ne traite pas les données au Canada, mais aux US avec un transit de donnée "live".

Service inclus dans les SAAS. Ex. Miro, Slack, Confluence. Se base sur des modèles plus connu, mais intégré dans leur espace, et parfois tourne aussi en local.
Nécessite de bien regarder les clauses d'utilisation, et le périmètre de sécurité accordé. Le SOC2 Type2 ou Iso27001 permet d'avoir une première idée de la sécurité.
Si la sécurité est sérieuse, cela sera mis en avant dans la page *Trust* ou *compliance*. 

Service multi-IA. Ex: Cursor, Photoshop firefly.
But: amélior le produit en proposant plusieurs IA pour choisir la meilleur.
Problème: Intégration avec ses fournisseurs flous. Il faut analyser chaque service et trouver les termes et conditions pour chaque intégration avant de déterminer ce est acceptable ou non.



