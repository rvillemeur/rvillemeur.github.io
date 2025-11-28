---
layout: post
title: la folie de l'IA
permalink: blog/folieIA
date: '2025-11-13 21:15:55 -0500'
categories: cyber risques IA tiers
comments_id: 7
draft: false
---


L'IA va nous rendre fou 🤪. Petit guide pour garder sa raison.

Oh, je n'ai pas l'intention de comparer tel modèle à la mode à tel autre ou de vous vanter une bibliothèque de prompt déjà obsolète 📖📖. Je ne vais pas non plus vous faire peur avec les derniers exploits des agents IA, ou du réalisme atteint dans les campagnes d'hameçonnage ou phishing 😱.

Dans la frénésie actuelle, c'est peine perdue, et d'autres s'en chargent bien mieux que moi. Je parle de l'IA, et en particulier de l'IA générative, d'un point de vue utilisation et gestion des risques en entreprises. L'IA étend le périmètre des risques de tiers de plusieurs façons:

➡️ Le premier niveau, les LLMs accessibles directement.
S'ils sont utilisés sans garde-fou, les LLMs grand public comme ChatGPT peuvent réutiliser nos prompts pour entrainer leur modèle. Outre le risque de fuite de données, vos informations peuvent se retrouver dans la réponse donnée à autre utilisateur en dehors de votre organisation. Si le risque est déjà bien identifié, il n'y a pas de remède universel, chaque organisation doit l'appréhender avec sa réalité d'affaire. 

➡️ Le second niveau, les LLMs intégré dans des offres SAAS ou des logiciels.
L'IA se retrouve maintenant ajouté ou intégré à des offres existantes. Cursor, Canva, Adobe Firefly, Slack, Atlassian/Confluence, Office 365, etc. intègrent des assistants IA. Outre les risques existants liés aux SAAS, il faut y intégrer les clauses spécifiques pour ces modules IA : 
* Les conditions d'utilisation
* Le risque tarifaire
* L'extraterritorialité des traitements. 
* etc.
Les garanties de sécurités sont rarement explicites sur le traitement IA et varient fortement d'un fournisseur à un autre. 
Ici, le risque est déjà plus subtil à appréhender et s'ajoute au risque initial des applications SAAS. 

➡️ Le troisième niveau, les LLMs installé en local
Pour éviter des outils SAAS, ou pour des raisons de conformité, il est souvent possible d'utiliser des modèles en local. Tabnine pour le code, ComfyUI pour la génération d'image et de vidéo, Mistral, etc.
On doit alors de soucier de :
* Obsolescence rapide des solutions déployées et pérennité de la solution.
* Un audit des sources utilisées pour limiter les attaques par chaine d'approvisionnement logiciel.
* Le support si le produit est open-source. 
* etc.
Un nouvel enjeu est également d'identifier la licence des modèles de données utilisés pour entrainer les moteurs IA intégrés si on veut s'assurer du respect des droits d'auteurs par exemple.

Et vous, quels sont vos enjeux en matière de risque de tiers apportés par les IA génératives ?



