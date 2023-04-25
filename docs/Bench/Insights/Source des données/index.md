---
tags: 
    - Insights
---


Title: Aperçu
Summary: Insights vous permet de connecter et d’analyser diverses sources de données. Vous pouvez ajouter plusieurs sources de données pour traiter et combiner des données provenant de différentes bases de données.
Authors:
    - Jérémy JOVINAC
Date: 2025-04-25
some_url: https://docs.frappeinsights.com/

# Source des données
Insights vous permet de connecter et d’analyser diverses sources de données. Vous pouvez ajouter plusieurs sources de données pour traiter et combiner des données provenant de différentes bases de données.

## Ajout d’une source de données#
Pour ajouter une nouvelle source de données, accédez à l’onglet Sources de données et cliquez sur Nouveau. Vous pouvez ajouter les types de sources de données suivants :

- Base de données MySQL distante
- Base de données SQLite locale
- Fichier CSV
- Base de données de l’application

!!! question "Base de données de l’application"
    Par défaut, Insights crée une source de données pour se connecter à la base de données de l’application. Vous pouvez utiliser cette source de données pour interroger la base de données de l’application.

## Ajout d’une base de données MySQL distante#
Vous pouvez ajouter une base de données MySQL distante en entrant les informations d’identification de la base de données.

Voici ce dont vous aurez besoin pour vous connecter à votre base de données :

- Le nom d’hôte du serveur sur lequel se trouve votre base de données (laissez vide pour localhost)
- Le port du serveur de base de données (laissez vide pour le port par défaut)
- Nom de la base de données à laquelle vous souhaitez vous connecter
- Le nom d’utilisateur que vous utilisez pour la base de données (de préférence un utilisateur avec des privilèges en lecture seule)
- Le mot de passe que vous utilisez pour la base de données

![Ajout d'une base de données MySQL distante](https://docs.frappeinsights.com/assets/new-data-source.848cd741.png){ loading=lazy }

## Schéma de synchronisation#
Une fois que vous avez ajouté une source de données, Insights synchronise automatiquement le schéma de la base de données. Vous pouvez cliquer sur la source de données pour afficher la liste des tables.

Vous pouvez également synchroniser manuellement le schéma en cliquant sur le bouton Synchroniser les tables dans le menu à 3 points.
![Synchroniser les tables](https://docs.frappeinsights.com/assets/sync-tables.226adf34.png){ loading=lazy }

