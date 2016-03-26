---
layout: post
section-type: post
title: Developer setup for Typescript and ES6
category: Technology
tags: [ 'software', 'coding']
---

For the past couple years I have been dabbling in JavaScript and the plethora of tools in its ecosystem, including various Node.js frameworks and AngularJS.

## What is all the hype?

In case you missed it, in the past few years the term 'isomorphic javascript' has taken hold of the industry.  With the advent of Node and tools like browserify and webpack, not developers can write code and share it between their server side APIs and client side applications.  The entire full stack application can be written in JavaScript, and given the large populous of JavaScript developers in the industry this can come as a huge advantage.

But this article is intented to cover a broader subject.  Now that I am working on a React.JS and Node project professionally, I wanted to write in detail about something I've noticed as I've observed many developers work with this stack.  

Here is what a typical chain of events looks like:

1. Install Node
2. Update npm
3. Create a new Node.js or client app manually or by cloning a starter project
4. Build and run the app using npm script, or your favorite task runner (i.e. grunt, gulp, etc)
5. Start hacking away your favorite hip new text editor (i.e. sublime, atom, etc) pulling dependencies down from NPM as your heart desires

## Sounds great, what is the problem?
It seems that we may have forgotten some of the great lessons of the past decade as developer tooling for mature, statically typed languages evoloved.  Developers are throwing away their IDEs in favor of the new Monkai themed text editors.  We always joke about the guy that still insites on using emacs or vim, but have we noticed that we are becoming _that guy_?

### Your Options
I'm not knocking these text editors, I use several of them on a regular basis.  But I am going to explain how far they will take you and where you may want to consider adopting an IDE, even for a nominal cost.  The following list are applications I've personally investigated at one point or another.  In this series I will only be focusing on Webstorm, Visual Studio Code, and Sublime.

#### Text Editors
 - Sublime Text
 - Brackets
 - Atom
 
#### IDEs
 - WebStorm
 - Visual Studio
 - Visual Studio Code
 - Cloud 9 (Online)

### You said TypeScript and Babel? 

![TypeScript](/img/tses6/tslogo.jpeg){: .center-image }

#### What is TypeScript?
TypeScript is an open source project sponsored by Microsoft.  It is a superset of JavaScript and ES6 features.  This means that you can rename any valid `.js` file to `.ts` and it will be pass the TypeScript compiler.  There are features available in TypeScript that you may find in other object orieneted languages but not in JavaScript, some of which include enums, generics, and interfaces.

#### Why should you care?
Modern frameworks including Angular 2 and Ionic 2 require you to write your application in TypeScript.  More importantly, it marries the benefits of JavaScript's dynamic nature to those of a statically typed language:

- compile time checking (i.e. types)
- readability and intellisense
- additional possibilities for tooling (i.e. refactorings)

#### What do you need?
You need a **TypeScript compiler** (or transpiler), see the [TypeScript homepage](http://www.typescriptlang.org/) for instructions based on your editor or IDE.  

To get the most benefit out of using typescript you will want to have the **TypeScript Definitions (TSDs)** installed for your npm modules.  Not all modules have tsds available at this time, for some options when this is the case you can [read this post](http://stackoverflow.com/questions/32173118/how-do-i-import-a-library-into-node-without-a-typescript-tsd-definition).

1. **Install tsd** by running `npm install -g tsd`
2. **Initialize a new _tsd.json_ file** for your project root: `tsd init`
3. Install the typescript definitions for each of your your modules.  
    - For example: `tsd install node --save`
    - The `--save` option is important as this will include the definintion in your `tsd.json` file and automatically keep a list of all tsds in `typings\tsd.d.ts` file.  This way you can check your tsd.json file into version control but exclude the `typings` folder, much like you do with `node_modules`.
4. In order for some IDEs and editors to be able to reference all the tsds, you may need to insert a line at the top of your source file that points to the primary tsd file.
    - `/// <reference path="typings/tsd.d.ts" />`    

You can refer to [Definately Typed](http://definitelytyped.org/) for more information.
 
The typescript compiler has [many options available](https://github.com/Microsoft/TypeScript/wiki/Compiler-Options) including the ability to output ES5 or ES6 code. 
 
Since I wanted to use Babel to compile from ES6 and also compile my JSX, I set a few custom compiler options in the `tsconfig.json` file:
 
~~~json
{
  "compilerOptions": {
    "target": "es6",
    "moduleResolution": "node",
    "jsx": "preserve"
  }
}
~~~

![Babel](/img/tses6/babel.jpeg){: .center-image }

#### What is Babel? 
Babel is another implementation of a transpiler; it processes ES6 (or 7) code and transforms it into valid ES5 code.  Currently, not all ES5 features are supported by browsers or Node.  Using Babel allows you to take advantage of the standard language features that will soon be available.

Babel 6 was recently released, and completely re-architected from the previous version.  Babel now also supports a variety of plugins to perform other tasks.  For example, it can turn JSX in a React.JS app into JavaScript, eliminating the need for a separate JSX transpiler.

With Babel you have some options regarding how your integrate it into your workflow, which I explain in more detail throughout the rest of this article.

### How does all of this work together?
If this is all new, you might be feeling overwhelmed and inclined to just stick with ES5.  I'm here to tell you it doesn't have to be difficult and its worth the extra steps.  

I did run into some challenges initially as I set all of this up for a new project, but hopefully you can benefit from my own experience. I'll try to illustrate what I'm about to document with a diagram.

This example depicts the **flow for a Node API server**, in which a tool called Nodemon watches for changes and restarts the node server automatically.

The flow is the same for the client app except I use a tool called **Webpack** which invokes Babel and packages all of the client side javascript into a bundle.  Webpack provides a dev server that will watch for changes, rebuild the changes into the bundle and hot deploy them inside the browser.  A subsequent post will go into more detail on that topic.

![Transpiler Workflow](/img/tses6/tranpile.jpg){: .center-image }

1. The IDE watches for changes to a TypeScript (`*.ts`) file
1. The file is transpiled by TypeScript and output as an ES6 Javascript file
1. The IDE watches for changes to javascript files and invokes Babel
1. The file is transpiled and ouput as an ES5 Javascript file in a separate folder I've configured named `js_es5`
1. Nodemon restarts the server anytime it notices a change to a file in the `js_es5` folder

Note: If you search the web, you will find instructions on how to import `babel-register` into your application entry point so that all code is transpiled on the fly.  This may be a convenient development tool but not something you want in your production code.  For that reason I have chosen to avoid it altogether and implement this toolchain.  It happens automatically every time I save a file and only takes a matter of seconds to reload.

## IDE Configuration

### Webstorm 

Webstorm comes with a typescript plugin that allows the entire project at compile at once.  This is nice since the Babel watcher only runs when a file is changed.  By configuring TypeScript to emit ES6 and configuring Babel to watch the .js files, The entire TS -> ES6 -> ES5 process can happen with a single click.  You may also consider writing a script to touch all of your JS files to trigger Babel to run against your entire project.

For example: `find server/**/*.js -exec touch {} \;`

#### TypeScript
Webstorm should automatically detect any files that end in `.ts` as TypeScript.  

#### Babel Watcher
Configure the Babel watcher to compile and emit an ES5 JavaScript file every time you make a change.

Webstorm may be configured out of the box to save files automatically.  I found that this happened as I was in the middle of an edit, which was too much churn for Bable and nodemon.  I made the following changes to only have Babel run when I physically save a file.

TODO: Insert image and instructions

#### Run Configurations
This is when we start to see the fruit of all our labor.  You don't necessarily have to setup run configurations inside of your IDe for simply running the application.  However, the next step will be debugging and for that its required.  I tend to like to have everything going on inside of one application

#### Debugging

#### How to debug?
There are a couple of items that you'll need to make sure are configured correctly first.

- .babelrc 
    - Set sourceMaps: "inline"

#### Testing
It may have been better to start with testing, but I saved it for last to emphasize the ability we now have to debug even our unit tests.  I prefer to execute my tests with `babel-node` which integrates nicely with Webstorms test runner which is capable of running single tests at a time.  As long as source maps are enabled this works well.

TODO: Screen shots

