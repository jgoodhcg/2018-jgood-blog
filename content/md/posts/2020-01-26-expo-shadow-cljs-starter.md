{:title "A template for Expo apps with Shadow CLJS" :layout :post :tags []}

The best path I've seen for web developers, familiar with the React stack, to make a native app is -- Expo. It's an easy technology transition and a well maintained project. 

## Why

To be honest, I would prefer if the world ran on webapps. Progressive web apps should be your first consideration though. If your project can fit within [what web can do]() then go for it. Web apps should be a great open standard for mobile solutions. I would love it if  Google and Apple opened their platforms up to web apps. They might if enough popular apps are web apps. 

Sometimes a PWA won't cut it. Device capabilities like push notifications [^] and geofencing are only available to native apps. The stores can also be a marketing boon. When those things are important then the solution is a native app.

An impediment for me are the Android and iOS stacks. [Like a lot of people](), coming from a web background, I'm most familiar with React apps. I don't want to learn two new stacks and write two identical projects in them. Immediately I narrow down to React Native as a solution. From there I add another filter -- Clojure.

Clojure is a great general language. [There]() are a [bunch]() of [reasons]() to use it. There are a few flavors but I've been using ClojureScript (CLJS) the most. It compiles to Javascript.

## How
This post is about how to setup  a native app project that uses Expo and CLJS. To get started I made [a template repository](). It has a selection of opinionated library choices. It's also forked from [the best solution]() I've seen for compiling CLJS for Expo.

### Tools
Let's start with my system setup. I'm running [PopOS](), which is almost the same as Ubuntu. [Spacemacs]() is my editor of choice. I use [Cider]() as my editor/CLJS integration. 

To develop an Expo/CLJS project I have these things open the whole time:
- Browser (for debugging)
- Terminal for using the Expo command line interface
- Spacemacs for editing
- [GenyMotion]() for device emulation
- [GitKraken]() for a GIT GUI [^] <--- you don't think you need one but you do footnote.

### Get a running project
Clone the repo first.
```
```

Connect the Spacemacs REPL and start Shadow-CLJS compilation. The [REPL is one of the most important parts]() of CLJS. 
```
```

![img](./../../img/ "Editor starting up a repl")


Start the Expo bundler.
```
```
![img](./../../img/ "Expo CLI")

Open up Genymotion. 
Install [the Expo client app]() if you don't have it. This client app  is only for development. Expo provides a way to [build binaries]() for iOS and Android platforms.

Open the Expo client and select your app. You may want to create an Expo account and log in to be able to see it as an option.

![img](./../../img/ "Expo client app project selection")

The demo app will open up.

![img](./../../img/ "Demo app running")

Now go build a CLJS native app! :)











