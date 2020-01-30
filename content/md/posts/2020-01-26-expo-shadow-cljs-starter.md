{:title "A template for Expo apps with Shadow CLJS" :layout :post :tags []}

Do you want to make a native app? Are you familiar with a React stack? Do you also want to write in Clojure? Then the solution is -- [Expo](https://expo.io) with [Shadow CLJS](https://shadow-cljs.github.io/docs/UsersGuide.html). It's an easy technology transition and a well maintained platform to build on. I've used Expo with plain Javascript at my day job for a couple of years. My most significant personal project is an Expo app written in CLJS

## Why

To be honest, I would prefer if the world ran on progressive web apps (PWAs).  If your project can fit within [what web can do](https://whatwebcando.today/) then go for it.

However, sometimes a PWA won't cut it. Device capabilities like push notifications [^] and geofencing are only available to native apps. The stores can also be a marketing boon. 

My day job needed a native app and so did one of my personal projects.  Geofence ability was an important feature for my personal project. My day job wanted a presence in the app stores and the ability to do push notifications to all users on iOS and Android.

For my personal projects I also _had_ to use [Clojure](https://clojure.org/). Clojure is a great general language. [There](https://clojure.org/about/rationale) are a [bunch](https://youtu.be/BThkk5zv0DE) of [reasons](https://news.ycombinator.com/item?id=20773921) to use it. There are a few flavors but I've been using ClojureScript (CLJS) the most. It compiles to Javascript. If you want to take your first steps with clojure I recommend [these practice problems](http://www.4clojure.com/), and a [few](https://www.braveclojure.com/) general [resources](https://lambdaisland.com/) for [learning](https://sekao.net/lightmod/).

## How
The goal of this post is to get you setup with a native app project that uses Expo and CLJS. To make it easy for you (and me) I made [a template repository](https://github.com/jgoodhcg/shadow-cljs-expo-starter). It has a selection of opinionated library choices that are _necessary_ to make a real app.

### Tools
Let's start with my system setup. I'm running [PopOS](https://system76.com/pop), which is almost the same as Ubuntu. [Spacemacs](https://www.spacemacs.org/) is my editor of choice. I use [Cider](https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Blang/clojure) as my editor/CLJS integration.

To develop an Expo/CLJS project I have these things open the whole time:
- Browser (for debugging)[^]
- Terminal for using the [Expo command line interface](https://docs.expo.io/versions/latest/workflow/expo-cli/)
- Spacemacs for editing
- [GenyMotion](https://www.genymotion.com/) for device emulation
- [GitKraken](https://www.gitkraken.com/) for a GIT GUI [^] <--- you don't think you need one but you do footnote.

### Get a running project
Clone the repo first.
```
```

Connect the Spacemacs REPL and start Shadow-CLJS compilation. The [REPL is one of the most important parts](https://dev.to/jr0cket/repl-driven-development-ano) of Clojure. 
```
```

![img](./../../img/ "Editor starting up a repl")


Start the Expo bundler.
```
```
![img](./../../img/ "Expo CLI")

Open up Genymotion. 
Install [the Expo client app](https://expo.io/tools#client) if you don't have it. This client app is only for development. Expo provides a way to [build binaries](https://docs.expo.io/versions/latest/distribution/building-standalone-apps/) for iOS and Android platforms.

Open the Expo client and select your app. You may want to create an Expo account and log in to be able to see it as an option.

![img](./../../img/ "Expo client app project selection")

The demo app will open up.

![img](./../../img/ "Demo app running")

Now go build a CLJS native app! :)
