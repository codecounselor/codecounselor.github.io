---
layout: post
section-type: post
title: Progressive Web Applications
category: Technology
tags: [ 'software', 'newtech']
---

# Google Progressive Web App Roadshow
Notes from NYC roadshow on August 15th, 2016

This page will be updated once Google publishes the slide presentations.

## Keynote

### Some stats

- 80% of mobile user's time is spent in 3 apps
- The average mobile user installs zero new apps per month
- 60% of connections world-wide are 2G
- Why does no-one open their browser when they know they are offline?

### User Experience needs to be
  1. Reliable
    - When we had dial-up modems everyone in the house knew if the house was online or offline.  
    - In the age of broadband we just expect connectivity; but what if its not there?
    - Native apps don't just break when there is no connectivity, why do webapps?
  1. Performant
  1. Engaging
    - Measure usage and provide option to add to home screen
    - Should be immersive
    - Provide relevant, timely, and useful notifications

## Pete LePage - HTTPS

- Stop using http; Secrity aside, you'll forfeit APIs that require Https:
  - service workers (because they have the power to manipulate the network)
  - getUserMedia (cam/mic)
  - push notifications
  - app cache
  - encrypted media extensions
  - geolocation
  - https/2

- Performance Concerns: http redirect to https, then TLS handshake (2 round trips)
  - Optimization: can set `strict-transport-security` headers on the server, can include sub-domains
  - TLS False start - server-side optimization to eliminate the last response
  - TLS session resumption - another server-side feature

- Buying a cert: sslmate, let's encrypt (Free) AND cli tools for managing certs

- Wnen moving sites:
  1. setup a 301 perm redirect
  1. Use [Canonical URLs](https://support.google.com/webmasters/answer/139066?hl=en); adding a `<link rel=canonical href="site url" >` to your pages.  A good idea in general actually.

## Engaging Clients
Goal should be to **add to home screen**.  Be smart how this is presented to users. Good example is Washington Post [http://wapo.com/pwa](http://wapo.com/pwa)) in which the home screen add option is presented after an engagement heuristic is met.

- `manifest.json` - short name, icons, start url, background_color, screen orientation, etc.
  - display: "standalone" - nothing but the splashscreen during launch
- Always starts from where the developer wants them to go (not some old page)
- Safari and Edge don't work today (Edge will) - Safari uses meta tags?

### Web Push notifications

Pete Recommends: https://www.npmjs.com/package/web-push

- The idea of web push notifications is new - but supported widely in a standard way
- Make sure the user knows what the subscription is for
- Make sure if a user clicks a notification they get to a page that loads offline (pre-cache the data)
- Need [VAPID](https://tools.ietf.org/html/draft-thomson-webpush-vapid-02) keys, JWT and headers for encryption details.  But, use libraries to do the heavy lifting.
- You can provide action buttons in a notifications, by adding an `actions` property during `registration.showNotification`.  Also need a `notificationclick` event listener
- **Service Workers** enable disconnected apps and push notifications, but carry some responsibilities for the developer.
  - Once you start controlling caching **you are in control** and the browser expiry is ignored.  Make sure you manage the cache appropriately.

## A Demo..

Use [lighthouse](https://github.com/GoogleChrome/lighthouse) to test the application

[Google sw-precache app-shell-demo](https://github.com/GoogleChrome/sw-precache/tree/master/app-shell-demo)

- Using the [iFixIt API](https://www.ifixit.com/api/2.0/doc/)
- [swprecache](https://github.com/GoogleChrome/sw-precache) - to have service worker automatically cache matching resources, including dynamic ones

## Code Labs

1. [Your First Progressive Web App](https://codelabs.developers.google.com/codelabs/your-first-pwapp/index.html#0)
1. [Debugging Service Workers](https://docs.google.com/document/d/160EgIx-G68pfcKu-jeafAmXViInJin55Dx8IWPTXXXw/view#heading=h.ihg8fgrwmhz9)
1. [Push Notifications](https://docs.google.com/document/d/1JHGBI39CEH9IbcR0aNyIP6zOd-W57oRzXXf23_WZ76A/view)

#### Tiny URLs

```
goo.gl/9DJRJq
goo.gl/HzwyOP
goo.gl/i2m7Fk
```
