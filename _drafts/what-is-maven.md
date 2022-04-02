---
title: What is maven
date: 2021-10-27T00:00:00+00:00
layout: post
permalink: /compiling-your-first-assembly-program/
categories:
  - java
tags:
  - maven
  - java
  - build tools
---

Maven is a tool for building and managing java-based projects. The word "Maven" means accumulator of knowledge.

## The POM file
The Project Object Model(POM) file `pom.xml` is the core of a projects configuration. It describes things like metadata, dependencies, plugins, relationship to other pom files and build settings.

A minimal `pom.xml` needs to have a groupId, artifactId and a version. The fields are called `Maven Coordinates`.
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>org.my.group</groupId>
  <artifactId>my-project</artifactId>
  <version>1.0</version>
</project>
```

## Build lifecycles
Mavens build on the concept of lifecycles. There are three default lifecycles.

* clean
* default 
* site

### Phases
A lifecycle is defined by a list build phases which can be seen as a stage in a lifecycle.

These are the phases of the default lifecycle
* validate - validates project configuration
* compile - compiles the source code
* test - run the tests
* package - packages the compiled code to a distributable format. Ex a JAR file.
* verify - verifies the ingration tests results
* install - installs the package to a local repository
* deploy - installs the package in a remote repository

When you run `mvn package` every phase including `package` is executed sequentially in the order above.

### Plugin and goals
All of the work done by maven is done by plugins. Plugins defines goals. A goal is executed in phases which determines the order goals get executed in. For example the `compile` goal comes from the `compiler` plugin and is attached to the `compile` phase.

## Standard directory layout
Maven defines a standard directory layout so that it's easy to work with any maven project.

For example 
* `src/main/java` - Source code
* `src/main/resources` - Resources
* `src/test/java` - Test source code
* `README.txt`

## Dependency management
Maven helps with defining, creating and maintaining reproducible builds. Dependecies are defined in the projects `pom.xml`.

### Scop

### Management



* dependencies
* SNAPSHOT version

