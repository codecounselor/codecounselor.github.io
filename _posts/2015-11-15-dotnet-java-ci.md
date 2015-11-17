---
layout: post
section-type: post
title: Musings in .NET, Java, and Continuous Integration
category: Technology
tags: [ 'software', 'coding']
---

## .NET
I spent most of my Saturday sitting at a local [.NET Code Camp](http://www.meetup.com/Central-Penn-Dot-Net-User-Group/events/225230290/) and picked up some excellent insights in an area that I have not spent much time for many years.

* [Ken Dale](http://kendaleiv.com) gave an comprehensive overview on how to use [Choclatey](https://chocolatey.org) in order to "DevOps Yourself" and do some scripting to quickly setup a new VM with a suite of software through unattended installs.
* IBM was present and gave a live demo of how to provision VMs in BlueMix.  While BlueMix seemed to be on par with other cloud providers like AWS, Rackspace, OpenShift, etc. the key differentiator is their growing set of APIs that allow your applications to interface with the AI features of Watson.
 
## Java
To balance out all the .NET I had my macbook along and was working on some features for my open source project [SyrahTest](https://github.com/SyrahTest/syrahtest-jvm).  The first major feature I am working to release is one that will enable a more powerful API for data driven tests than what tools offer today.

First, a couple simple test cases (created using Test Driven Development): 

```java
@DataSource(dialect = DataSource.Dialect.JSON, data = "{ token: \"Class Value\"}")
public class DataSourceAnnotationTest {

  @Test
  public void replaceTokenInTestMethodAnnotatedWithDataSourceFromClass() {
    IDataSource dsc = DataStore.get().getDataSourceInContext();
    String result = new DefaultVariableResolver().resolveVariables("some {{token}}", dsc);
    assertEquals("some Class Value", result);
  }

  @DataSource(dialect = DataSource.Dialect.JSON, data = "{ token: \"Method Value\"}")
  @Test
  public void replaceTokenInTestMethodAnnotatedWithDataSource() {
    IDataSource dsc = DataStore.get().getDataSourceInContext();
    String result = new DefaultVariableResolver().resolveVariables("some {{token}}", dsc);
    assertEquals("some Method Value", result);
  }
}
```

And an explanation:

1. **@DataSource** is allowed on Types and Methods - This allows a test method and potentially multiple underlying methods to share a single datasource OR allow each method to define its own datasource.  DataSources may also be inherited from any type in the type hierarchy.
2. There will be support for several **dialects**, the first release will support JSON.
3. Inline **data**, like in this example, is supported but it will be more common for data to exist in a file or a database.
4. SyrahTest implements a **DataStore** which is a thread-safe factory for loading, managing, and serving DataSources of any type (Json, Excel, DB result sets, etc).
5. SyrahTest aims to provide a framework for automated testing, but the core functionality of the **DataStore** and **VariableResolver** is designed to be plugged into any tool or system that could benefit from it's feature set.

#### So what is the big deal? 

1. Since SyrahTest manages loading and setting the right data sources under the covers, it lets you the tester focus on writing tests that are concise by building a library of reusable components.
2. The above code shows the guts of the framework.  Lets suppose we were using the Page Object Model pattern to implement a test of a UI.  The test would look like this: 

```java
@Test
@DataSource(dialect = DataSource.Dialect.JSON, location="/testdata/stander-user-flow.json")
public void login(){
  LoginPage login = PageFactory.getLogin();
  HomePage home = loginPage.login("{{username}}", "{{password}}");
  home.clickLinkByText("{{linkText}}");
  ...
}
```

Future features and examples will illustrate how complex data structures can be composed to keep test data maintainable and test code readable at the same time.

## You Can't Beat Free

#### GitHub Pages
It is worth mentioning that the blog you are reading is being hosted for free on [GitHub Pages](https://pages.github.com) which has excellent support for building and instantaneously redeploying [Jekyll](https://jekyllrb.com) content.

#### Travis CI

[![Build Status](https://travis-ci.org/SyrahTest/syrahtest-jvm.svg?branch=master)](https://travis-ci.org/SyrahTest/syrahtest-jvm)

And I was delighted last night when in about 15 minutes I was able to sign up with [Travis CI](https://travis-ci.org/), enable my git project, configure my build, and successfully run my syrahtest-jvm build.  

Travis provides a cloud based build service free to any open source project, as well as a paid offering.  I'm still exploring its feature set and determining the best way to get my build artifacts into a central maven repository. 

Since it wasn't painfully obvious, for anyone getting started with TravisCI I recommend the following:

1. First, install the Travis Command Line Interface.  With ruby its as simple as `gem install travis`.
2. Run `travis init` in your project workspace.  Examine the generated file and make any additions or changes.  Mine only required two lines:

    ```yaml
    language: java
    jdk: oraclejdk8
    ```

3. Check in your .travis.yaml file and watch your build run!
4. Take a look at the other command options and do some experimenting.

#### Codecov

[![Test Coverage](https://img.shields.io/codecov/c/github/SyrahTest/syrahtest-jvm.svg)](https://codecov.io/github/SyrahTest)

Another free provider (for open source projects) that has direct integration with TravisCI is [Codecov](https://codecov.io/)
Their site provides simple setup instructions for Maven, but if you're using gradle I found this page to be accurate and easy to follow: <http://vorba.ch/2015/java-gradle-travis-jacoco-codecov.html>

