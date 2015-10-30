---
layout: post
section-type: post
title: A Conduct for Code
category: Technology
tags: [ 'software', 'coding']
---

A few colleagues and I agreed to discuss [this premise of this article]( http://derickbailey.com/2015/09/14/code-is-for-coworkers-not-compilers/), stating the code is for human readership, not compilers.  While I personally agree with the author, I believe there is a lot more to say on the topic.  The following questions come to mind as I read the article and some of its comments.

### Can the code be too expressive, or verbose?
I’ve seen a lot of naming standards over the years, some examples that come to mind:

```java
public class User{
  private int i_recordCount = 0; // 'i_' to indicate the type is an integer
  ...
  public void setFirstName(String pFirstName){
  	this.firstName = pFirstName; // 'p' to indicate the variable is a method parameter
  }
}
```

I use these trivial examples of [Hungarian Notation](https://en.wikipedia.org/wiki/Hungarian_notation) to emphasize one thing: modern IDEs provide the ability to visualize and report types, scopes, and all necessary context.  These naming conventions only serve to make the code harder to read and more costly to maintain.

I believe that the right level of expressiveness is the absolute minimum that makes the code easy to read and understand.  A few simple rules I follow are:

1. **Keep the length of variables and methods relative to their scope**: the smaller the scope the shorter the name, the larger the scope the longer the name.
	* **Exception:** test method names - Test methods are never referenced and can have very long names that express the full intent of the test case.
1. Relentlessly rename.  It is so easy to do, if something is misrepresented or unclear change it the moment you do the mental mapping.
1. Avoid redundant comments, and remove them when you find them.  For example: `i++; //increment i`
1. No comments that are used to explain names.
1. No unnecessary words
	*  Bad: `List<String> listOfAllToppingsAvailableForTheCake;`
	* Good: `List<String> availableToppings;`
1. Pay attention to packaging and class names.  The goal here is to make sure that the intent and purpose of a code module is immediately apparant.  Use package and class level documentation appropriately. 

### What about the law of diminishing returns? Or, how much effort should be expended in authoring code?
I believe the TDD cycle of Red -> Green -> Refactor informs this concern.  If you start with only the code necessary to pass a failing test, or to satisfy the first requirement, you end up with no extra code.  I have found that nearly all of my attempts to plan the final design upfront fail.  It is through the iterative development process that new ideas and problems present themselves.  

So, the right amount of effort is that which is required to satisfy all of the funtional AND non-functional requirements of the software, and the refactorings performed along the way to ensure the right level of readability and maintainability.

### Ok, the code is readable, but what about performance?
The aformentioned article addresses this question, and I'll add a few of my own beliefs:

1. Modern compilers are better at optimizing common algorithms than we are.  Simple code (brevity) more often than not performs better than code that is prematurely optimized.
1. If you understand the critical paths, or sections of code, optmize them only after you have working software with a comprehensive test suite which covers the concerned implementation.

### What is the right level of documentation?

I think this answer depends on what kind of code you're producing.  An library will need to have very good documentation of its public API.  Normal system components should follow a more standard set of commenting best practices:

* If the name of a method, field, or variable adequetely conveys purpose and intent leave it undocumented.
* If a method produces side effects, document them.
* If a method throws exceptions document them and what can cause them.
* Any overloaded method or constructor should be documented with a clear differentiator.  Alternatively, [implement the creation method pattern](https://www.industriallogic.com/xp/refactoring/constructorCreation.html).

### Human Concerns

My final list of thoughts serve the benefit of all current and future developers who work on your code.

1. Delete code without abandon.  If its commented it doesn't deserve to live.
    * Your best line of code is the one that you never write.
	* “If you love writing code-- really, truly love to write code-- you'll love it enough to write as little of it as possible.” – Jeff Atwood
1. Every team should define their own 'conduct for code' that is collectively agreed upon and followed.
1. Domain names and terms are important, document them and use them appropriately throughout the code.  Make sure terms are not confusing or contradictory.

### Helpful Resources

* [Sourcemaking.com](https://sourcemaking.com/antipatterns)
* [Naming](http://www.itiseezee.com/?p=83), a summary from Uncle Bob's Clean Code Chapter 2
* [Best line of code you'll ever write](http://elegantcode.com/2010/06/06/the-best-code-you-will-ever-write/)

