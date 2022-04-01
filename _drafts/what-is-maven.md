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


* Goals
* Phases
* pom files
* dependencies
* SNAPSHOT version

