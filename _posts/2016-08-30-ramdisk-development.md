---
layout: post
section-type: post
title: Speed up your development with a RAM disk
category: Technology
tags: [ 'coding']
---

#### Optimize your development environment and save your SSD at the same time
In a quest to tune IntelliJ one of the bits of advice I came across was to setup a ramdisk for build artifacts.
**Note:** this is specific to OSX but the same can be done on any linux environment

#### Master of the obvious: this is an ephemeral disk because its in memory and any contents will not survive a machine restart

The application I primarily work on runs on Tomcat, directly against the IDE build artifacts which are targeted under `WEB-INF/classes`.  For this reason I also had to enable the `allowLinking` feature in my Tomcat Context.

Here are the steps I took for setting up my ramdisk:

1. Create a ram disk by using [this script.](https://gist.github.com/rxin/5085564) Make sure you provision enough space for the current and future content you with to host on this drive.
2. Move the contents of your IDE output directory onto the new RAM Disk (i.e. `/volumes/ramdisk`)
3. Create a symlink to point to the new ramdisk directory
4. Make sure Tomcat is configured to follow symlinks inside your application context file:

``` xml
  <Context docBase="/Users/nategood/projects/myapp/web" debug="0" reloadable="false" useHttpOnly="false" >
    <Resources allowLinking="true" cachingAllowed="true" cacheMaxSize="100000" />
  </Context>
```

This resulted in a reduction in application startup time by approximately 25% in my case.  And more importantly, my IDE is a lot happier too.

#### What's Next?

1. create and mount the ramdisk on startup.  https://github.com/zafarella/OSX-RAMDisk is another option, but does not seem to be widely adopted.
2. I'll be moving [more into ramdisk](http://sheroz.com/pages/blog/improving-development-performance-moving-intellij-idea-cache-ramdisk.html), and setting up rsync to persist it to the SSD.
