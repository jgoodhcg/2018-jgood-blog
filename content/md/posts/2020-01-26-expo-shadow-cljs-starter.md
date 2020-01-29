{:title "A template for Expo apps with Shadow CLJS" :layout :post :tags []}

The best path I've seen for web developers, familiar with the React stack, to make a native app is -- [Expo](https://expo.io). It's an easy technology transition and a well maintained project. 

## Why

To be honest, I would prefer if the world ran on progressive web apps (PWAs).  If your project can fit within [what web can do](https://whatwebcando.today/) then go for it.

Sometimes a PWA won't cut it. Device capabilities like push notifications [^] and geofencing are only available to native apps. The stores can also be a marketing boon. 

My day job needed a native app and so did one of my personal projects.  Geofence ability was an important feature for my personal project. My day job wanted a presence in the app stores and the ability to do push notifications to all users.

For my personal projects I also want to use [Clojure](). Clojure is a great general language. [There]() are a [bunch]() of [reasons]() to use it. There are a few flavors but I've been using ClojureScript (CLJS) the most. It compiles to Javascript.

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











