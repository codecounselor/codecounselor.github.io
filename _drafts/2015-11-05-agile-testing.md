---
layout: post
section-type: post
title: Agile Software Testing
category: Technology
tags: [ 'software', 'testing']
---

# Agile Software Testing

One significant change occuring as a result of an organization migrating their SDLC to an agile methodology is the shortened (sprint) cycle time.  This brings a number of immediate benefits, but also introduces a number of challenges.  One of those challenges is maintaining a consistent level of quality as part of every release without incurring exponential costs in testing.

This article serves as a guide to automated agile software testing.

## Why is it important to build quality in?
Quality must be **built in** as the software is being built; not tested in at the end.  This has several positive side effects:

1. supports and encourages ongoing refactoring by providing confidence to developers, testers, and management that the system still works after changes are introduced
2. guides the design of the system and ensures it is testable
3. there are no surprises at the end of a sprint or release

## Who is responsible for quality?
The entire team is responsible for the quality of the product, at every increment.  In agile, testing is no longer the full responsibility of the quality analyst.

## What does testing look like on an agile team?
Testing should take on many forms.  Test Driven Development (TDD) is a popular Extreme Programming (XP) practice that can be applied at each of the following levels:

1. Unit Tests - These are sometimes referred to as 'micro tests'.  Unit tests should not be confused with manual testing performed by developers before handing features off to QA.  Unit tests are automated using an [xUnit](https://en.wikipedia.org/wiki/XUnit) framework which are executed as part of every build.
2. Component Tests
3. Integration Tests
4. System Tests
5. User Interface (UI) Tests

Most teams new to testing will rush straight to number 5, and teams with technical discpline may also practice unit testing; completely disregarding everything inbetween. 

## Where do you execute tests in the Agile SDLC?
Well designed tests should be capable of running across all environments and platforms.  Data driven testing can be used to leverage a common test harness across a number of environments which do not share common data stores.

Tests should be maintained in a source control management (SCM) system (i.e. Git) that the entire team has access to.  They should run with little to no setup after they are fetched from the SCM repository. 

## When should you perform automated testing?
Because Agile places high value on automated tests, they should be run at every opportunity.  The faster and more stable the tests are, the higher their value will be.  

1. After every code change (Unit)
1. Before developer commit (Unit/Component/Integration)
1. During Continuous Integration (CI) Builds (Unit/Component)
1. Post Deployment (Integration/System/UI Smoke Tests) - Ensures a usable system
1. Nightly (Integration/System/UI) - Full suite of tests, validates all features
1. Weekly (Integration/System/UI) - For large systems where all tests may take days to run; non-ideal
1. Release Milestone (Integration/System/UI) - These may be tests that require manual intervention or carry some form of higher cost.

## How can you get started?
There is no shortage of tools which make promises to ease the burden of automated testing.  The truth is that testing requires the same level of engineering rigor as that of your production system.  Be wary of vendors who make promises of trival record and playback features.  As you begin to develop a test strategy, take the following into consideration:

1. How will you properly isolate your tests from each other?  Account for things like data conditioning, concurrent access to shared resources, and external dependencies.
1. How will the team evaluate, plan, and ensure an acceptable level of test coverage in each of the testing levels mentioned above?  
	* Be careful not to confuse line level code coverage metrics with coverage of your functional and non-functional requirements, or acceptance criteria.  

If you don't have test professionals in your organization who have experience solving these kinds of problems consider how you may get some outside help.