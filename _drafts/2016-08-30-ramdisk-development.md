
## Optimize your development environment and save your SSD at the same time

Note: this is specific to OSX but the same can be done on any linux environment

### Master of the obvious: this is an ephemeral disk because its in memory and any contents will not survive a machine restart

1. Create a ram disk by using this script. https://gist.github.com/rxin/5085564 Make sure you provision enough space for the current and future content you with to host on this drive.
2. Move the contents of your IDE output directory onto the new RAM Disk (i.e. `/volumes/ramdisk`)
3. Create a symlink to the ramdisk directory
4. Make sure Tomcat is configured to follow symlinks inside your application context file:
```xml
  <Context docBase="/Users/nategood/projects/myapp/web" debug="0" reloadable="false" useHttpOnly="false" >
    <Resources allowLinking="true" cachingAllowed="true" cacheMaxSize="100000" />
  </Context>
```

This resulted in a reduction in application startup time by approximately 25% in my case.  And more importantly, my IDE is a lot happier too.
