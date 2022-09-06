---
layout: post
comments: true
author: islimani
title: Spring Batch
date: 2021-01-02 22:00:20 +0300
words: 0
summary: Spring Batch permet le traitement d'un entrepot de donnée depuis plusieurs sources et les transformer à une autre forme. # Add post description (optional)
thumbnail: /assets/img/Spring-Batch.png
category:
        - spring_batch
---

## Introduction

Spring Batch permet le traitement d'un entrepot de donnée depuis plusieurs sources et les transformer à une autre forme

## Définitions

Disclaimer: cet article n'est pas un tutorial. Il est seulement un résumé que j'ai fait pour un devoir surveillé.

### Job

> Un job correspond à un traitement qui est effectué lors de l’exécution d’un batch 

1. **Dans Spring Batch, un job est représenté par un Bean implémentant l’interface Job**
2. **Un job est composé d’un ensemble d’étapes (step).**
3. **Les étapes d’un job sont séquentielles, c’est-à-dire qu’elles doivent être exécutées les unes à la suite des autres.**

### JobInstance

> JobInstance fait référence à l’exécution d’un batch. On parlerait de l’instance de job du 30 décembre 2014, de celle du 31 décembre, et ainsi de suite

*Une instance de job implique donc la définition de l’identité d’un job, qui passe dans Spring Batch par la classe **JobParameters**.*

### JobExecution

>  `JobExecution` est un objet qui contient ce qui s'est réellement passé pendant une exécution.

### JobParameters

> Ce sont normalement des meta-data qu'on ajoute à un job pour les récuperer plus tard. Par exemple, on veut savoir la date d'exécution de chaque Job. Dans ce cas, on ajoute un parameter date, qui va être générer à chaque exécution du Job.

```
JobParameters jobPara=new JobParametersBuilder().addLong("currentTime", new Long(System.currentTimeMillis())).toJobParameters();
```

Il est difficile, en outre, d'expliquer ces concepts sans les voir mettre en oeuvre. Voilà une image qui résume tout. 

![Figure 1 Job et instance]({{site.baseurl}}/assets/img/job.png)

### Step

#### Tasklet

Ce qui est fait dans un `Step` est representé par un `Tasklet`, càd, une tâche. Généralement, on utilise *Spring Batch* pour le *Chunk oriented processing*: lire d'une source, traiter les données et les persister à une autre source (csv, bd, ...), mais on peut définir nos propres tâches.

#### Chunk

Définit et configure les interface suivants: `ItemReade `,`ItemProcessor` et le `ItemWriter`.

1. **ItemReader**: chargé de récupérer des données depuis une source
2. **ItemProcessor**: chargé de transformer les données lues.
3. **ItemWriter**: chargé d’exporter les données vers un support de stockage

#### ItemReader

Généralement parlé, cet interface possède des dizaines d'implémentations, a titre d'exemple, `FlatFileItemReader` en cas d'un fichier texte. Tout dépend de nos besoins. Vous devez reférer toujours à la documentation pour savoir quelles sont les méthodes prises en charge.

Pour un `FlatFileItemReader`, savoir ces différentes properties et beans est nécessaire. 

* Resource: permet de spécifier le fichier source à lire.
* `DefaultLineMapper`: bean responsable à transformer (tokenizer) un ligne à un ensemble des tokens (fieldset).
	* `DelimitedLineTokenizer`: En effet, c'est ce bean qui est responsable de la transformation. on définit ici, le *delimiter* virgule par exemple et les *field names*.
* `fieldSetMapper`: définit le *TargetType*: l'objet à mapper. *fieldset* -> *targetType*

### JobRepository

> `JobRepository` comme son nom l'indique est un interface qui offre un mechanisme de persistence (écrire dans un BD) pour toutes les meta-data nécessaires à l'exécution d'un job.

`SimpleJobRepository` est la seule implementation pour cet interface.
```java
	<bean id="jobRepository"
		class="org.springframework.batch.core.repository.support.MapJobRepositoryFactoryBean">
		<property name="transactionManager" ref="transactionManager"/>
         <property name="table-prefix" value="BATCH_T"/>
	</bean>
```
On peut utiliser un `DataSource` au lieu d'un `TransactionManager`, cela revient au même chose.

`MapJobRepositoryFactoryBean` est utilisé lorsque on veut tester seulement le Batch processing sans persistance au BD.

### JobLauncher

> `JobLauncher` est l'interface qui permet l'exécution d'un Job. Son seule implémentation est `SimpleJobLauncher`.

### Exécuter un batch automatiquement 

```java
<task:scheduled-tasks>
  <task:scheduled ref="jobLauncher" method="run" cron=" 0 0-59/1 * * * * " />
</task:scheduled-tasks>

```

## Récapitulation

![Figure 2 Architecture Spring Batch]({{site.baseurl}}/assets/img/architecture_batch.png)




