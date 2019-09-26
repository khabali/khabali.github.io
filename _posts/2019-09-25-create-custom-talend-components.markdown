---
layout: post
title:  "Create a custom Talend components using Talend Component Kit"
date:   2019-09-25 11:45:00 +0200
author: Anas KHABALI
tags: Talend data-integration
comments: true
---

# 1. Introduction
In this article, we're going to take a look at **TCK** - Talend Component Kit. The new Talend framework for developing components for Talend Studio.

The main purpose of TCK is providing an easy way for building high-performance components for Talend Studio based on Java with separation
and loose coupling of the targeted execution environment and the business logic components. The components may run in a standard java process
or in a distributed execution runner for big data like Spark, Flink or others.

# 2. Core Concepts
TCK is a Java annotation based framework. This leads to shorthand form of development and avoid providing additional configuration (xml) files
for parameters, layout and dependencies definition as it was the case in the old javajet based framework.

## 2.1. User Interface
TCK comes with a set of annotations and classes providing an easy way to define the component configuration user interface, handle the configuration validation,
provide dynamic values and setting up the configuration layout.

## 2.2. Runtime
In TCK, a component can be an **Input** component, an **Output** component or a **Processor**.

### 2.2.1. Input
Input are responsible of reading data from any data provider like databases, files system, HTTP API... Input components are able of distributing the work load (reading)
when executed in in a distributed system  - this offers a huge performance improvement for big data environments.

Input components also handle batch and streaming mode for reading.

### 2.2.2. Output
Output are responsible of writing data to a target system like a database, files system... Output components can handle the write operations in a batch mode by processing
records by groups or in an element by element manner.

### 2.2.3. Processor
Processor are responsible of transforming or applying a business logic on the incoming records and producing new transformed records. This kind of components is mostly
for implementing filtering, grouping, data mapping, sorting _(when possible)_ or any other transformation logic.

## 2.3. Development tools
TCK comes with a suite of development tools for testing and validation :

- A [starter tool kit](https://starter-toolkit.talend.io/) is provided for setting up and generating a components project
- A maven plugin comes with the framework and provide goals for validating meta-data, generating documentation, deploying component into the studio, an embedded web server for browsing the configuration UI and it's layout.
- A Job API for creating test pipelines and run them on multi execution environments _(distributed or simple java process)_

# 3. Example Input component
Let's create a project representing a simple input component for mysql which will be able of executing a SQL query.

# 3.1. Project Setup
Open the [starter tool kit](https://starter-toolkit.talend.io/) and fill in the project information as bellow.

Then click finish and download the project archive.

# 3.2. Data Store and Data Set.

# 3.3. Runtime

# 4. Example Output component

# 5. Example Processor component
