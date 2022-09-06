---
layout: post
author: islimani
comments: true
title: Spring Data and Hibernate
date: 2020-11-26 22:00:20 +0300
words: 0
summary: "In this article, we are gonna talk about the Data Tier"
thumbnail: /assets/img/Spring-Data.png
category:
        - spring_jpa
---


## Introduction

In this article, we are gonna talk about the *Data Tier*. For instance, the *Domain Model* and *Data Access* layer. When in the old days `Hibernate` made a revolution in the *ORM* world, `Spring Data` and its submodule `Spring Data JPA` says no less when it comes to *Data Access Layer*. And if it happens and you salt this stack with `Spring Boot`, believe me magic will become reality.

In the first section we will cover the **Model** layer. Then, the **DAO** layer with `Spring Data JPA` and later on we will see configuration with and without the magic of `Spring Boot`.

Before Starting off, this schema from the JBoss documentation is a good starting point.

![Figure 1 Data Access Layer and the Domain Model]({{site.baseurl}}/assets/img/data_access_layers.svg)

Let's dig in!

## The Model

So let's start by answering three basics questions:

### What's Hibernate?

> Hibernate is an **ORM** framework that helps storing and retrieving data from relational databases using **POJOs**.
* POJO: Plain Old Java Objects or simply classes with no extra stuff
* ORM: Object Relational Mapping 

### What's JPA?

> JPA or `Java Persistence API` is a **specification** for accessing, persisting, and managing data between Java objects / classes and a relational database[^1]

[^1]: https://en.wikibooks.org/wiki/Java_Persistence/What_is_JPA%3F

### How they are related to each other.

As you've noticed JPA is just a **specification** and Hibernate is one implementation of many to it. Theoretically you can change to another framework and your code will still intact. But, in practice, who does that? not me at least. Hibernate is more than sufficient for my needs.

However, it's a good practice to follow specifications for one good reason: **Readability of your code**.

Suppose you've developed a groundbreaking application that everyone starts to fork and read your source code. You wouldn't know how much you will make life easier for your fellow developers if you've used something everyone is familiar with.
That's why we need to abide by the specifications unless otherwise we've another reason not to do so.

Now let's move on to the basics.

### Entities and naming convention:

To define an *Entity* simply decorate your class with `@Entity`.
```java
@Entity
public class Engineer()
```
But suppose you have two entities that refer to the same table, in this case you could name your entity as follows:
```java
@Entity(name = "me")
public class Engineer()
```
But this will confuse Hibernate, since we are using *Implicit naming strategy*. So we need to name the table manually:
```java
@Entity(name = "me")
@Table(name = "You")
public class Engineer()
```

*You* is the name of the table inside the DB. *me* is what you will use to refer to your table while using *HQL*.

So what's with these naming strategies.
* Basically, hibernate will name your table or columns with the name that you specify inside `Entity` or `Column`, the so-called *ImplicitNamingStrategy*.
* The hard-way is when you have some requirements as to prefix all you tables with your organisation name. In this case, you need to implement `PhysicalNamingStrategy` and override the methods that you need. This is what's called *PhisicalNamingStrategy*. And don't worry, it's so simple, follow this example from [Jboss documentation,](https://docs.jboss.org/hibernate/orm/5.2/userguide/html_single/Hibernate_User_Guide.html#PhysicalNamingStrategy) and you are fine!

### Associations and relationships:

Understanding relationships (relations between tables) or associations (relations between objects) is crucial in working with hibernate.
Mainly, there are two concepts that we should be aware of: *Multiplicity* and *Directionality*.

#### Multiplicity

	Multiplicity refers to how entities relate to each other.

Basically, there are 3 types associations:

| Associations            | Definition                                                                                                                              | Example                                                      |
|:-----------------------:|:----------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------|
| `@OnetoOne`              | > Each object references only one object and vice versa                                                                                   | A country has only one capital city                          |
| `@OnetoMany` `@ManyToOne` | > These two associations go hand in hand in that  if we define the first in one entity automatically  the second is defined in the other. | A person can have many phone numbers.                        |
| `@ManytoMany`            | > Each object references one or more objects and vice versa                                                                               | A person with many addresses. An address with many persons. |

#### Directionality

> Directionality asks a simple question: By having two associated objects a and b, could a figure out b? if so we say this association is *unidirectional*. If b can also get a, we say it's *bidirectional*.

I know, I just made things worse explaining it this way. But let's take an example:

Suppose we are in a situation where an **author has written many books and a book can only be written by one author**, we can model this as follows:

```java
@Entity
public class Book {
	@Id
	@GeneratedValue
	private Long id;
	@ManyToOne
	@JoinColumn(name = "book_author")
	private Author author;
	...
}
```

```java
@Entity
public class Author {
	@Id
	@GeneratedValue
	private Long id;

	@OnetoMany(mappedBy = "author")
	private List<Book> books = new ArrayList<>();
	...
}
```

This is a *One-To-Many* or *Many-To-One* association.

Given an author, can I get all his Books? Indeed, Yes.

Given a Book, can I get the author? Without doubt.

So this is a *One-To-Many* **bidirectional** association.

We could, of course, delete `author` attribute from the Book class, and the association will be **unidirectional** (Author -> Book).

#### `@JoinColumn`, `mappedBy` and `@JoinTable`

> The entity that holds the `@JoinColumn` annotation is the owner of the relationship. That is, in the last example, the book table will contain a column named *book_author*.

As per the JPA specification, usually in **OneToMany** relationship, the entity with **ManyToOne** is the owner of the foreign key.
But, you can have two entities defining each a `@JoinColumn` if that makes sense to you.

> `@MappedBy = dude` instructs Hibernate that the foreign key is defined in a field with the name **dude** in the other entity. If it didn't find an already defined one, it will define it.

For instance, in the example above, we tell hibernate: Go look for a bean that contains **author** attribute, you will find the definition of the foreign key there!

> `@JoinTable` is used to define a whole new table that holds the relationships.
 
In the last example, we could omit the `mappedBy` and add the following annotation:

```java
@JoinTable(
  name="Author_Book",
  joinColumns=@JoinColumn(name="book_ID", referencedColumnName="ID"),
  inverseJoinColumns=@JoinColumn(name="author_ID", referencedColumnName="ID"))
```

In `@ManyToMany` relationship, we can use only a `@JoinTable` to define associations.

### `@Basic` and `@Transient`
### Fetching strategies


### Criteria


Work in progress!
## DAO
Work in progress!
### JPA Interface Hierarchy
Work in progress!

* `EntityManagerFactory` is used to create multiple `EntityManager`. It represents the configuration for a database in the application.
* `EntityManager`: is analogous to a database connection. Each one has:
	 * one single `EntityTransaction` which is required for persisting changes to the underlying database.
  * multiple classes that implements `Query` interface.

## Configuration: The old way and the new way
Work in progress!




