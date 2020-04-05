{:title "💎 Stealing with deps.edn" :layout :post :tags ["expo" "cljs" "shadow-cljs" "create-kit" "npm"]}

This builds on [a previous post](/posts/2020-01-26-expo-shadow-cljs-starter) about a template repo I made. The goal was to make it easier for me and others to start a project on this stack. The ideal state wasn't a repo that needs manual setup. The ideal state is one command that generates a ready to run project. This post will describe how I made an npm module called [create-expo-cljs-app](https://www.npmjs.com/package/create-expo-cljs-app).  

## The Target
First I found an example to work from. [Create CLJS App](https://github.com/filipesilva/create-cljs-app) is exactly what I wanted to make for expo cljs projects. Once I grokked the codebase I was set. Clojure open source projects are the only projects I've ever been able to dive into an understand without someone guiding me through the codebase. I think that's a merit of clojure itself -- it's easy to read.  

I'm going to refer to the concept this project relies on as an __npm create kit__. An npm create kit seems to be a convention for creating npm modules that generate projects.  

To start the project must be named `create-<something>`. Then it needs a couple of specific things in its `package.json`. Most important is a `bin` key with a script that is keyed to the same name as the project. Like so:
```
  "bin": {
    "create-cljs-app": "./bin/create-cljs-app.js"
  },
```
A `files` key scopes any other necessary files in the create kit. Like so:
```
  "files": [
    "bin",
    "dist",
    "template"
   ],
```
This is all the create kit convention needs to run your project given any of the commands for `npx` `yarn` or `npm`. [Here are some examples of those commands](https://github.com/filipesilva/create-cljs-app#npx).  

## The Heist
It's up to the create kit to actually generate the project in some way. The `create-cljs-app` project has a simple entry bin script.
```
const { create } = require('../dist/lib');
const [, , projectPath = ''] = process.argv;
return create(process.cwd(), projectPath);
```
It calls one `create` function in a cljs namespace that does everything. Some things include checking for dependencies and writing messages to the console.
It took awhile but I realized that I could include this repo with [deps.edn](https://clojure.org/guides/deps_and_cli#_using_git_libraries) and reuse most of the code.

The first change was to add `deps.edn` to the original create kit. That [was easy with shadow-cljs](https://shadow-cljs.github.io/docs/UsersGuide.html#deps-edn).  

Then I extended that [create function](https://github.com/jgoodhcg/create-cljs-app/blob/extendable-ish/src/create_cljs_app/lib.cljs#L20) to take a map of optional `options`.You can see those changes on [this branch](https://github.com/jgoodhcg/create-cljs-app/commits/extendable-ish) of my fork.  

My create kit project [includes the create-cljs-app](https://github.com/jgoodhcg/create-expo-cljs-app/blob/master/deps.edn#L3) as a dependency. It has basically the same bin entrypoint:
```
const { create } = require('../dist/lib');
const [, , projectPath = ''] = process.argv;
return create(process.cwd(), projectPath);
```
The only cljs I needed to make a new project was some functions to check for dependencies and a custom message to display at the end. 
```
(defn create [cwd path]
  (cca-lib/create cwd path {:done-msg       done-msg
                            :get-commands   get-commands
                            :has-extras?    (fn [] (has-binary-on-PATH? "expo"))
                            :extras-warning expo-cli-warning}))

(def exports #js {:create create})
```
The [whole file](https://github.com/jgoodhcg/create-expo-cljs-app/blob/master/src/create_expo_cljs_app/lib.cljs) is only about 50 lines.  

## Loose Ends
One _gotcha_ I ran into was needing to [install the Javacsript dependencies](https://github.com/jgoodhcg/create-expo-cljs-app/blob/master/package.json#L24-L28) of the [project I was including](https://github.com/jgoodhcg/create-cljs-app/blob/5efc7915dcb0d88a744e5d68b0f178cfc7142b09/package.json#L47-L51). It doesn't seem like `deps.edn` can manage javascript dependencies.  

Locally I tested with something like:
```
node ~/create-expo-cljs-app/bin/create-expo-cljs-app.js my-app
```
Once it was working all I had to do was setup an npm account [and publish](https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages).  

Now creating a project is as simple as `yarn create expo-cljs-app my-app`. I wish this had existed when I started trying to make my first expo cljs app.  
