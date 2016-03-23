---
layout: post
section-type: post
title: Rest Assured, Be Specific
category: Technology
tags: [ 'software', 'testing']
---

### What is Rest Assured?

If you are developing any kind of REST API, hopefully you are thinking about how you will test it.  If you're using Java you might even already be using [Rest Assured by Jayway](https://github.com/jayway/rest-assured).  Its been a little while since I've used it myself and I'm picking it up again because I'm starting to work on integrating it with my own framework ([SyrahTest](https://github.com/SyrahTest/syrahtest-jvm)) for writing data driven tests.

The project documentation is already great and there are many other articles written if you're just getting started.  I will be focusing on a very specific use case that will hopefully help you write more maintainable and expressive test suites.

### The Challenge

Rest Assured has several defaults that it uses, the two I will discuss here are the hostname, defined by `baseURI`, and the `port` defaulting to `http://localhost` and `8080`, respectively.

In the examples below, the application is written using StrongLoop's [LoopBack API Framework for Node.JS](http://loopback.io/) which runs on the default port of 3000.  The request is to retrieve a list of merchants having many attributes.  For the purposes of this post, the test is simply validating the name of the first merchant.

Every time a RequestSpecification is created (i.e. by invoking `given()`) these values are read from their global defaults, as [documented here](https://github.com/jayway/rest-assured/wiki/Usage#default-values).  It took a bit of searching and digging to get to this point, so hopefully this short post will benefit someone else facing the same question: 'How can I best parameterize the hostname and port for my tests?'

One way to use a different hostname or port is to be explicit when invoking the _method_ (i.e. `get()`) operation:

~~~java
@Test
public void getMerchantsExplicit(){
  given().get("http://localhost:3000/api/Merchants?access_token="+token)
    .then().assertThat().body("[0].name", equalTo("Dickies BBQ"));
}
~~~

Since this solution is anything but DRY you may extract the hostname and port into variables.  If your entire test suite is targeted at the same hostname and port then assigning these values before running the tests is an even better solution:

~~~java
@BeforeClass
public static void configureTarget(){
  RestAssured.baseURI = "http://qa-service-foo"; //Poor imagination, just not localhost
  RestAssured.port = 3000;
}
  
@Test
public void getMerchantsHardcoded(){
  given().get("/api/Merchants")
    .then().assertThat().body("[0].name", equalTo("Dickies BBQ"));
}
~~~

With this approach the hostname (baseURI) and the port will be used for any and all subsequent requests.

### The Solution

This can be further improved by [re-using a Specification](https://github.com/jayway/rest-assured/wiki/Usage#specification-re-use), which also allows other concerns to be addressed.  In my example, the token can be set universally as well.

~~~java
public class MerchantTest {

  private static RequestSpecification remoteQASpec, remoteQALoopBackSpec;
  private static String token = "QA3SiuivoPBpi5xNEBMvCI9JibhYMGlmqHx3ODsOrK4gd4RQl8TjRI0WFg";

  @BeforeClass
  public static void configureHost(){
    RestAssured.baseURI = "http://qa-service-foo";
    remoteQASpec = with(); //A spec using the default port first
    remoteQALoopBackSpec = with().port(3000).param("access_token", token);
  }

  @Test
  public void getMerchantsReusableSpec(){
    remoteQALoopBackSpec.get("/api/Merchants")
      .then().assertThat().body("[0].name", equalTo("Dickies BBQ"));
  }
}
~~~
This makes the test cases more expressive and all concerns about each endpoint can be centrally defined.

By assigning the `baseURI` before the invocation of `with()` we are capturing the value as they exist at that point in time.  Be aware that these are static values.  Consequently, if you are running a large number of tests concurrently be sure to treat the setup of any specifications as a protected resource that is thread-safe.

While this works just fine, and its concise, it limits the spec to the way it was configured as any changes would cause undesired side effects to other tests.  For this reason you may decide it best to follow the builder paradigm as illustrated in the documentation, which would look like this:

~~~java
  @Test
  public void getMerchantsReusableSpec(){
    given().
      spec(localhostLoopBackSpec).
    when().
      get("/api/Merchants").
    then().
      assertThat().body("[0].name", equalTo("Dickies BBQ"));
  }
~~~

Note that his approach also follows the definition of Behavior Driven Development (BDD) more closely by formalizing the given/when/then steps. 


Happy Testing! 
