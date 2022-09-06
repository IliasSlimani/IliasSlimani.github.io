---
layout: post
comments: true
author: islimani
title: Business Intelligence with Pentaho
date: 2021-01-18 20:50:20 +0300
words: 7800
summary: In this tutorial, we will use Pentaho in a real case scenario where we have a bunch of Csv files that are updated constantly. We need to process these files continuously, analyse them and extract useful informations that will decide the future decisions that we will made.
thumbnail: /assets/img/pentaho.png
category:
        - Pentaho
---


### This tutorial includes how to

1- Use *ETL* **Extract, Transform and Load** processing to pull data from a bunch of *Csv* and *Excel* files and place them into a **datawarehouse** (*Mysql* database).

2- Creating *OLAP* **On-Line Analytical Processing** cubes with *Schema Workbench*

3- Analyse and Explore the resulted data.

### Tools used in this tutorial

1- Mysql Server

2- Mysql Client ( Mysql Workbench )

3- Pentaho Data Integration (Kettle)

4- Schema Workbench

5- Bi server with Saiku plugin

6- MySql Java Connector

### Scenario

Pour chaque accident corporel (soit un accident survenu sur une voie ouverte à la circulation publique, impliquant au moins un véhicule et ayant fait au moins une victime ayant nécessité des soins), des saisies d’information décrivant l’accident sont effectuées par l’unité des forces de l’ordre (police, gendarmerie, etc.) qui est intervenue sur le lieu de l’accident. Ces saisies sont rassemblées dans une fiche intitulée bulletin d’analyse des accidents corporels. L’ensemble de ces fiches constitue le fichier national des accidents corporels de la circulation dit " Fichier BAAC " administré par l’Observatoire national interministériel de la sécurité routière "ONISR".

Les bases de données, extraites du fichier BAAC, répertorient l'intégralité des accidents corporels de la circulation intervenus durant une année précise en France métropolitaine ainsi que les départements d’Outre-mer (Guadeloupe, Guyane, Martinique, La Réunion et Mayotte depuis 2012) avec une description simplifiée. Cela comprend des informations de localisation de l’accident, telles que renseignées ainsi que des informations concernant les caractéristiques de l’accident et son lieu, les véhicules impliqués et leurs victimes.

[source](https://www.data.gouv.fr/fr/datasets/base-de-donnees-accidents-corporels-de-la-circulation/)

### Tutorial

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/watch?v=nigFz3Lzgtk&t' frameborder='0' allowfullscreen></iframe></div>

In case video doesn't show up

[link](https://www.youtube.com/watch?v=nigFz3Lzgtk&t)
