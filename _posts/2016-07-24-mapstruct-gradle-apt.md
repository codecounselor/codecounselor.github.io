---
layout: post
section-type: post
title: Configuring MapStruct compiler arguments in Gradle and IntelliJ
category: Technology
tags: [ 'software', 'coding']
---

[MapStruct](http://mapstruct.org/) is a great Java library that generates object mapping code using an annotation processor.  Some of the settings (i.e. the DI model) that you would otherwise apply via annotations can be applied globally using a compiler argument.  Here is how you can set it up if you're using Gradle and/or IntelliJ.

# Gradle Configuration

The MapStruct documentation shows an example of how to configure the Maven apt plugin, here is how you do it with Gradle.

```groovy
plugins {
    id 'net.ltgt.apt' version '0.6'
}
...
compileJava {
    options.compilerArgs = [
            '-Amapstruct.defaultComponentModel=spring'
    ]
}

dependencies {
    ...
    compile "org.mapstruct:mapstruct-jdk8:1.1.0.Beta2"
    compile "org.mapstruct:mapstruct-processor:1.1.0.Beta2"

    apt 'org.mapstruct:mapstruct-processor:1.0.0.Final'
}
```

The [compiler options](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.CompileOptions.html#org.gradle.api.tasks.compile.CompileOptions:compilerArgs) allow you to provide an array of compiler arguments.

You can find a list of all [MapStruct configuration options here](
http://mapstruct.org/documentation/1.1/reference/html/index.html#configuration-options).

# IntelliJ configuration

If you're already using MapStruct then you've probably enabled annotation processing, but if not that is the first thing you need to do.

After that apply the same compiler arguments you set in your Gradle file to your IntelliJ config, as shown here:

![IntelliJ Annotation Compiler Settings](/img/intellij/apt-intellij-compiler.png){: .center-image }

Thats all there is to it! Now you can apply global MapStruct configurations and keep your code clean.
