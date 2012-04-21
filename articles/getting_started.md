---
title: "Getting Started with Clojure & MongoDB using Monger"
layout: article
---

## About this guide

This guide combines an overview of Monger with a quick tutorial that helps you to get started with it.
It should take about 19 minutes to read and study the provided code examples. This guide covers:

 * Feature of Monger, why Monger was created
 * How to add Monger dependency to your project
 * Basic operations (created, read, update, delete)
 * Overview of Monger Query DSL
 * Overview of how Monger integrates with libraries like clojure.data.json and JodaTime.
 * GridFS support
 * Clojure and MongoDB version requirements

This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/3.0/">Creative Commons Attribution 3.0 Unported License</a> (including images & stylesheets). The source is available [on Github](https://github.com/clojurewerkz/monger.docs).

## What version of Monger does this guide cover?

This guide covers Monger 1.0.0-SNAPSHOT, an evolving pre-release version. Monger is a young project but most of the public API
is fleshed out and will not change before 1.0 release.


## Monger overview

Monger is an idiomatic Clojure wrapper around MongoDB Java driver. It offers powerful expressive query DSL, strives to support
every MongoDB 2.0+ feature and is well maintained.

### Project Goals

Monger authors wanted a MongoDB client that will

 * Support most of MongoDB 2.0+ features but only those that really matter.
 * Target Clojure 1.3.0 and later from the ground up.
 * Be well documented.
 * Be well tested.
 * Be maintained, not have technical debt accumulated over several years.
 * Integrate with libraries like clojure.data.json and Joda Time.
 * Integrate usage of JavaScript files and ClojureScript (as soon as the compiler gets artifact it is possible to depend on for easy embedding).
 * Learn from other clients like the Java and Ruby ones.

### Integration with popular libraries

Monger heavily relies on relatively recent Clojure features like protocols to integrate with libraries like
[clojure.data.json](http://github.com/clojure/data.json) or [Joda Time](http://joda-time.sourceforge.net/). As the result you can focus on your application instead of figuring out how to glue
two libraries together.

### What Monger is not

Monger is not a replacement for the MongoDB Java driver, instead, Monger is symbiotic with it. Monger does not try to offer
object/document mapping functionality. With Monger, you work with native Clojure and Java data structures like maps,
vectors, strings, dates and so on. This approach has pros and cons but (we believe) closely follows Clojure's philosophy of
reducing incidental complexity. It also fits MongoDB data model very well.


## Adding Monger Dependency To Your Project

### With Leiningen

    [com.novemberain/monger "1.0.0-beta2"]

### With Maven

    <dependency>
      <groupId>com.novemberain</groupId>
      <artifactId>monger</artifactId>
      <version>1.0.0-beta2</version>
    </dependency>

## Connecting to MongoDB

Monger supports working with multiple connections and/or databases but is optimized for applications that only use one connection
and one database.

To connect, you use `monger.core/connect!` and `monger.core/connect` functions.

{% example 01_connecting.clj, 2af1bcee22e0f14a8741 %}

To set default database Monger will use, use `monger.core/get-db` and `monger.core/set-db!` functions in combination:

{% example 02_setting_db.clj, 86340be2cabe43a4ec18 %}



## How to Insert Documents with Monger

To insert documents, use `monger.collection/insert` and `monger.collection/insert-batch` functions.

{% example 04_inserting.clj, 901c3c3c4aee166efc0f %}


### Default WriteConcern

To set default write concern, use `monger.core/set-default-write-concern!` function:

{% example 03_setting_default_write_concern.clj, 3b43b7f91341a393eb81 %}


### Safe By Default

By default Monger will use `WriteConcern/SAFE` as write concern. We believe that MongoDB Java driver (as well as other
official drivers) have **very unsafe defaults** when no exceptions are raised, even for network issues. This does not sound
like a good default for most applications: many applications use MongoDB because of the flexibility, not extreme write throughput
requirements. Monger's default is to be on the safe side.



## How to Find Documents with Monger

Monger provides two ways of finding documents:

* Using finder functions in the `monger.collection` namespace
* Using query DSL in the `monger.query` namespace

The former is designed to cover simple cases better while the latter gives you access to full power of MongoDB querying
capabilities and extra features like pagination.

### Using finder functions

Finder functions in Monger return either Clojure maps (commonly used) or Java driver's objects like `DBObject` and `DBCursor`.

For example, `monger.collection/find` returns a `DBCursor`:

{% example 05_find_multiple_documents_using_find.clj, 0 %}


`monger.collection/find-map-by-id` finds one document by `ObjectId` and automatically converts it to a Clojure map
before returning:

{% example 10_find_one_document_using_find_map_by_id.clj, 0 %}



### Using query DSL

For cases when it is necessary to combine sorting, limiting or offseting results, pagination and even more advanced features
like cursor snapshotting or manual index hinting, Monger provides a very powerful query DSL. Here is what it looks like:

{% example 11_query_dsl_example1.clj, 0 %}

## How to Update Documents with Monger

`monger.collection/update` is the most commonly used way of updating documents. `monger.collection/update-by-id` is useful
when document id is known:

{% example 06_updating.clj, 8313a67bac %}


TBD: upsert examples


## How to Remove Documents with Monger

Documents are removed using `monger.collection/remove` function.  `monger.collection/update-by-id` is useful
when document id is known.

https://gist.github.com/44e3cdae129deeea7512

{% example 07_removing.clj, 44e3cdae129deeea7512 %}


## Monger Query DSL

TBD


## Integration With Other Libraries

TBD


## GridFS Support

TBD


## Clojure and MongoDB Version Requirements

Monger is built from the ground up for Clojure 1.3 and later. It currently uses MongoDB Java driver 2.7.x under
the hood and thus supports MongoDB 2.0 and later versions. Please note that some features may be specific to MongoDB 2.2 and
later versions.