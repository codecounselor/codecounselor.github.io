

### The old way
So you have a web application and you need to provide your customers with exports, what do you do?  For many years people turned to a mryiad of SDK providers in hopes that they could generate PDF and Office documents that rivaled the richness of the application itself.  What you ended up with was a sizeable codebase to duplicate the content in your app.  It doesn't have to be that way anymore.

### The new way
Browser and HTML advances have led to even richer online experiences, including the use of features like the Canvas and SVG.  Recently, I was tasked with a project to export a rich HTML5 web app into pixel perfect PDF and PowerPoint exports.  So, the research effort began, and I deserately hoped to find a better solution than those I had worked on in years gone by.

My research led me to a lot of libraries that could do bits and pieces of what I was envisioning.  Namely, to have the same quality that can be achieved by printing to PDF in modern browsers.  This post is not about all the solutions which did not work, but I will reference the article I found that addresses the shortcomings of a few popular approaches.

### Electron to the rescue
[Froser Xu](https://fraserxu.me/) published a great article on the research his team did and the solution they build around Electron and the Chromium browser, [you can find it here](https://fraserxu.me/2015/08/20/pdf-generation-on-the-web/) 

If you havn't heard of [Electron](http://electron.atom.io/) it is likely that you use it on a daily basis. A number of applications including the Atom Editor, Slack, and Visual Studio Code are build using Electron.  It just so happens that it serves as a great PDF rendering engine as well!  At the time of this article, Electron supports the Node 6.5.0 runtime and Chromium 53 which means you can write nearly all ES6 code.

I have been contributing to [Electron-PDF](https://github.com/fraserxu/electron-pdf) and am proud to say that in addition to the excellent support for CLI operations in the past, it is now a fully capable export engine for any Node Server Process.  The rest of this post will demonstrate how you can put it to use for your own exporting needs.   Because Chromium produces such high quality PDFs you can convert the PDFs into other document formats as well.  There are few options that may be worth mentioning:

1. [Pandoc](http://pandoc.org/)
1. [ImageMagick](http://www.imagemagick.org/script/index.php)
1. []()


### Give it a try (htmlexport.com)

### PDF
### PNG
### Powerpoint

### Challenges
Authentication

### Deployment

OSX
Linux
Docker