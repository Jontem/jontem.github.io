---
title: Maven basics
date: 2021-10-27T00:00:00+00:00
layout: post
permalink: /maven-basics/
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
* test - runs the tests
* package - packages the compiled code to a distributable format. Ex a JAR file.
* verify - verifies the ingration tests results
* install - installs the package to a local repository
* deploy - installs the package in a remote repository

When you run for example `mvn package` every phase including `package` is executed sequentially in the order above.

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
Maven helps with defining, creating and maintaining reproducible builds. Dependecies are a core feature of maven and are defined in the projects `pom.xml`.

### Transitive dependencies
Maven resolves all dependencies of your project including and all the dependencies of their `pom.xml`. 


### Dependency Scope
A dependency can declare a scope which means when to include the dependency in the `classpath`. Examples of scopes are
* - compile - The default scope this means that the dependency will be included in all classpaths of a project
* test - The dependency is needed only for running tests and is needed for normal execution.
* import - Dependencies with type of `pom` and scope `import` can be used to import all dependencies from the specified dependency.

### Dependency Management
The `<dependencyManagement>` can be used in a parent `pom` file to centralize configuration for shared dependencies. In a child `pom` you only need to specify `groupId` and `artifactId`.


## Example maven commands

```sh
# Create a project from the quickstart template you can run
$ mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false

# Compile
$ mvn compile

# Create package
$ mvn package

# Clean the build artifacts
$ mvn clean
```