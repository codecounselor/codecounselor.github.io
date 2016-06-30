---
layout: post
section-type: post
title: Concurrent testing in ES6 with Ava and supertest-as-promised
category: Technology
tags: [ 'software', 'coding', 'testing']
---

[Ava](https://github.com/avajs/ava) is a JavaScript test runner that is new on the scene.  I recently selected Ava as my test runner for writing tests against a REST API.  Some of the benefits include:

1. Runs tests concurrently, which for longer running tests can significantly reduce total runtime.  It also forces tests to be coded such that they run in complete isolation from each other.
1. Native support for ES6 and ES7 (i.e. async/await) features by using Babel under the covers.
1. Simplicity

But, Ava by itself is not enough.  To interact with the REST API I chose to use supertest-as-promised.  This is a light wrapper around supertest/superagent that wraps the rest request and assertions in a promise object.

Using these together, a JavaScript test looks like this:

```javascript
import test from 'ava';
import request from 'supertest-as-promised';

/**
 * Get something from the API
 * @param urlSuffix
 * @returns {Promise}
 */
function getUrl(urlSuffix) {
    return request('http://localhost:8080/api/v1').get(urlSuffix);
}

test('GET inventory item', async t => {
    const res = await getUrl(`/inventory/741f11a0-87e2-11e5-8eda-00259000d29a`);
    const item = res.body;
    t.is(res.status, 200);
    t.is(item.name, 'Cocoa Krispies');
});
```

This is great, but it would be even better if the IDE (Webstorm in my case) could give me pertinent content assist for `t` so I know what assertion operations are available and what arguments they accept.  This is really hard in JavaScript! Thankfully TypeScript solves this exact problem.

## Converting Ava tests from JavaScript to TypeScript

All JavaScript code should be valid TypeScript code, but because of some of the tooling, I ran into a number of challenges.  These will be addressed one by one.

#### `async t =>` produces this error: Error:(31, 34) TS1005: ',' expected.

Resolved by simply putting parens around the argument t: `async (t) =>`

#### You may see TypeScript compilation errors:

- Error:(1, 33) TS2307: Cannot find module 'ava'.
- Error:(31, 28) TS1057: An async function or method must have a valid awaitable return type.
- Error:(5, 40) TS2304: Cannot find name 'Promise'.

First, TypeScript compiler options should be set as follows in a `tsconfig.json` file.  This goes in your project root.

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "ES6"
  }
}
```

Second, you need to make sure the project has type definitions installed.  My type definition file, `typings.json` looks like this:

```json
{
  "name": "api-integration-test",
  "dependencies": {
    "bluebird": "registry:npm/bluebird#3.3.4+20160622205516"
  },
  "globalDevDependencies": {
    "node": "registry:env/node#6.0.0+20160622202520",
    "superagent": "registry:dt/superagent#1.4.0+20160317120654",
    "supertest-as-promised": "registry:dt/supertest-as-promised#2.0.2+20160317120654"
  }
}
```

You can [read more about typings here](https://github.com/typings/typings), to set these up I had to do the following:

- `npm install -g typings`
- `typings install -DG dt~superagent dt~supertest-as-promised env~node`
- `typings install -D bluebird`

You may be wondering why a tsd file for Ava is not included.  Ava provides one in its npm module, and TypeScript finds it there.

The `typings.json` file should be included in version control so that others can just run `typings install` much like they would run `npm install` when setting up a project.

### Taking advantage of TypeScript in the test
With all of these changes in place, we are almost ready to achieve our goal of getting better content assist.  

First, make sure that you have a TypeScript compiler enabled in your IDE.  Alternatively, you can execute `npm install -g typescript` so that you are able to run `tsc` from the command line to transpile your test source into JavaScript so that Ava can run it.

My [previous post details how to configure Webstorm]({% post_url 2016-03-21-typescript-and-es6 %}).  Be sure to enable the TypeScript compiler and have it read the tsconfig.json file.

#### Update imports

The following changes are required to address this error: TS1192: Module '"supertest-as-promised"' has no default export.

```
import {test, TestContext} from 'ava';
import {agent as request} from 'supertest-as-promised';
```

We also want to import the `TestContext` type so that in our test function we can use TypeScript:

```javascript
test('GET inventory item', async(t:TestContext) => {
    const res:any = await getUrl(`/inventory/741f11a0-87e2-11e5-8eda-00259000d29a`);
    const item = res.body;
    t.is(res.status, 200);
    t.is(item.name, 'Cocoa Krispies');
});
```

Notice that a type was also added to `res` of type `any`.  Without this TypeScript gets confused and reports an error:
TS2339: Property 'body' does not exist on type 'Test'.

By adding the type for t (`async(t:TestContext)`) we now get only the relevant content assist options:

![TypeScript](/img/tses6/ts-content-assist.png){: .center-image }
