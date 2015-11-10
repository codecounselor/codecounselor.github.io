---
layout: post
section-type: post
title: Agile Software Testing
category: Technology
tags: [ 'software', 'testing']
---

One significant change occurring as a result of an organization migrating their SDLC to an agile methodology is the shortened (sprint) cycle time.  This brings a number of immediate benefits, but also introduces a number of challenges.  One of those challenges is maintaining a consistent level of quality as part of every release without incurring exponential costs in testing.

This article serves as a starter guide for beginning automated agile software testing.

## Why is it important to build quality in?
Quality must be **built in** as part of every story; not tested in at the end.  A common but unfortunate practice is to introduce 'hardening sprints' prior to a release in which no new working software is delivered.  Instead, the team goes into a defect fixing phase.  A perceived need for hardening just means that you have a quality problem that needs addressed.  Instead, try adding multiple levels of test automation to your *definition of done*, and expect to see several positive side effects:

1. Supports and encourages ongoing refactoring by providing confidence to developers, testers, and management that the system still works after changes are introduced.
2. Guides the design of the system and ensures it is testable.
3. There are no surprises at the end of a sprint or release.  Risk is not completely removed, but it is addressed earlier and with higher frequency.  

## Who is responsible for quality?
**The entire team** is responsible for the quality of the product, at every increment.  In agile, testing is no longer the full responsibility of the quality analyst.  The diverse skillset of the entire cross functional agile team is required to ensure that the automated test strategy is robust and maintainable.  

Often times teams get off to a rapid start with automation only to discover that they are swallowed up trying to maintain their tests.  This can be avoided by applying engineering practices like incremental design and refactoring, as well as by addressing the concerns raised in the getting started section below.

This shared responsibility for quality is dependent upon a culture of continuous improvement.  It is better to slowly build up automation by *listening to the tests* and making necessary improvements than to attempt a blitz attack at automating all of your regression tests in a short period of time.  I have seen teams and entire organizations turn away from automation because of one bad experience where they failed to account for one or more key requirements (e.g test isolation, control over data, etc).
 
## What does testing look like on an agile team?
Testing should take on many forms.  Test Driven Development (TDD) is a popular Extreme Programming (XP) practice that can be applied at each of the levels below.  A highly collaborative team with good customer engagement may want to consider one of the Behavior Driven Development (BDD) frameworks for implementing what is referred to *Specification by Example* in which your requirements are captured as executable tests.

Most teams new to testing will rush straight to UI tests.  Teams with technical discipline may even already be practicing unit testing.  However, the majority of teams completely disregard everything in between.  Consider how you may benefit from adopting a more comprehensive testing strategy, and spreading the responsibility out across the team.

Whatever you decide, agree as an organization what terminology you will use and be consistent.  For example, it is common for the term 'unit test' to carry several different meanings depending who you ask.

#### Unit Tests 
These are sometimes referred to as 'micro tests'.  Unit tests should not be confused with manual testing performed by developers before handing features off to QA.  Unit tests are automated using an [xUnit](https://en.wikipedia.org/wiki/XUnit) framework which are executed as part of every build.  Two simple criteria to apply as you start are (1) should run without a network connection (2) should run for < 1ms.  There are numerous articles and books that you can reference if you are new to unit testing.

#### Component Tests
Validates the interaction of two or more components within a system.  These can still be run during a build because they do not interact with external systems.  Database connections may be made from these tests.  Favor in memory embedded databases since they support test isolation and parallel execution.

#### Integration Tests
Validates the interactions of the system under test with one or more of its external systems.  Integration tests must be run post build since they run against a deployed application running in container.

#### System Tests
Validate the functionality of a fully integrated, full stack system.  For example, these could be executed against a middleware service or MVC endpoint.  Batch jobs also fall into this category.  Using *[Service Virtualization](https://en.wikipedia.org/wiki/Service_virtualization)* is an acceptable practice (i.e. to setup a controlled environment, simulate failure scenarios, test SLA permutations, etc) so long as you test coverage all integrated systems running non-simulated.   

#### User Interface (UI) Test
A test against the browser or thick client application.  These tests may or may not exercise the full stack of services supporting the UI or they may leverage one or more layers of service virtualization to isolate the testing of the presentation logic.  With modern advances in front-end (JavaScript) tooling, be sure to consider all of the previous types of testing for your client side logic as well.

**Caution**: Remember that the time it takes to run, and the effort to maintain tests will increase exponentially as you move from Unit Tests to UI Tests.

## Where do you execute tests in the Agile SDLC?
In short, **everywhere**.  Well designed tests should be capable of running across all environments (development, test, production, etc.) and platforms (OS, browser, etc).  Data driven testing can be used to leverage a common test harness across a number of environments which do not share common data stores.

Tests should be maintained in a source control management (SCM) system (i.e. Git) that the entire team has access to.  They should run with little to no setup after they are fetched from the SCM repository. 

## When should you perform automated testing?
Because Agile places high value on automated tests, they should be run **at every opportunity**.  The faster and more stable the tests are, the higher their value will be.

1. After every code change: Unit
1. Before developer commit: Unit, Component, Integration, System & UI Smoke Tests
1. During Continuous Integration (CI) Builds: Unit, Component
1. Post Deployment (to ensure usable system):  Integration, System, UI Smoke Tests
1. Nightly: Integration, System, UI - Full suite of tests, validates all features
1. Weekly: Integration, System, UI - For large systems where all tests may take days to run; non-ideal
1. Release Milestone: Integration, System, UI - These may be tests that require manual intervention or carry a higher cost to execute.

* **Smoke tests** represent a subset of the longer running System and UI Tests that ensure a stable runtime.

The goal here is to **fail fast**.  Successful automation will catch regressions very shortly after they are introduced; while the cost to fix them is near zero, and before impacts spread to a larger audience.

## How can you get started?

Make sure your entire team, or organization, has **a common understanding of what the goal is**.  It will require a disciplined, full team focus for you operate efficiently.

There is no shortage of tools which make promises to ease the burden of automated testing.  The truth is that testing requires the same level of engineering rigor as that of your production system.  Be wary of vendors who make promises of trivial record and playback features.  As you begin to develop a test strategy, take the following concerns into consideration:

1. How will you **isolate your tests** from each other?  
	- Account for concerns like data conditioning, concurrent access to shared resources, and external dependencies.
	- Test Smell: a single problem results in multiple test failures.
1. How will the team evaluate, plan, and ensure an acceptable level of **test coverage** in each of the testing levels mentioned above?  
	- Be careful not to confuse line level code coverage metrics with coverage of all acceptance criteria, functional, and non-functional requirements.
	- Consider how your test status can be directly derived from your automated tests being implemented, passing, or failing.  Less time spent on reporting and tracking means more time delivering value in the form of working software (tests).   
1. Does the tooling you have (or plan to use) support **engineering practices** like object oriented design, refactoring, and code quality analysis?
	- Since automation requires coding make sure everyone has a working knowledge of house to use a modern Integrated Development Environment (IDE) and version control.


If you don't have individuals in your organization who have experience solving these kinds of problems consider leveraging a professional from outside your organization.

Please stay tuned for future articles which will address each of these questions in more detail.