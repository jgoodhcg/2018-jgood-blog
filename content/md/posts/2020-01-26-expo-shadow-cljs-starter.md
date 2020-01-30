{:title "A template for Expo apps with Shadow CLJS" :layout :post :tags []}

Do you want to make a native app? Are you familiar with a React stack? Do you also want to write in Clojure? Then the solution is -- [Expo](https://expo.io) with [Shadow CLJS](https://shadow-cljs.github.io/docs/UsersGuide.html). It's an easy technology transition and a well maintained platform to build on. I've used Expo with plain Javascript at my day job for a couple of years. My most significant personal project is an Expo app written in CLJS

## Why

To be honest, I would prefer if the world ran on progressive web apps (PWAs).  If your project can fit within [what web can do](https://whatwebcando.today/) then go for it.

However, sometimes a PWA won't cut it. Device capabilities like push notifications [^push-notifications] and geofencing are only available to native apps. The stores can also be a marketing boon. 

My day job needed a native app and so did one of my personal projects. Geofence ability was an important feature for my personal project. My day job wanted a presence in the app stores and the ability to do push notifications to all users on iOS and Android.

For my personal projects I also _had_ to use [Clojure](https://clojure.org/). Clojure is a great general language. [There](https://clojure.org/about/rationale) are a [bunch](https://youtu.be/BThkk5zv0DE) of [reasons](https://news.ycombinator.com/item?id=20773921) to use it. There are a few flavors but I've been using ClojureScript (CLJS) the most. It compiles to Javascript. If you want to take your first steps with clojure I recommend [these practice problems](http://www.4clojure.com/), and a [few](https://www.braveclojure.com/) general [resources](https://lambdaisland.com/) for [learning](https://sekao.net/lightmod/).

## How
The goal of this post is to get you setup with a native app project that uses Expo and CLJS. To make it easy for you (and me) I made [a template repository](https://github.com/jgoodhcg/shadow-cljs-expo-starter). It has a selection of opinionated library choices that are _necessary_ to make a real app.

### Tools
Let's start with my system setup. I'm running [PopOS](https://system76.com/pop), which is almost the same as Ubuntu. [Spacemacs](https://www.spacemacs.org/) is my editor of choice. I use [Cider](https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Blang/clojure) as my editor/CLJS integration.

To develop an Expo/CLJS project I have these things open the whole time:
- Browser (for [debugging](https://docs.expo.io/versions/latest/workflow/debugging))
- Terminal for using the [Expo command line interface](https://docs.expo.io/versions/latest/workflow/expo-cli/)
- Spacemacs for editing
- [GenyMotion](https://www.genymotion.com/) for device emulation
- [GitKraken](https://www.gitkraken.com/) for a GIT GUI [^git-gui]

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

Disable hot reloading and live reloading through the [dev menu](https://docs.expo.io/versions/latest/workflow/debugging/#developer-menu) because Shadow CLJS already does this.

![img](./../../img/ "Demo app running")

### Opinions

Now that you have a running app let's explore some of the libraries that are included.

[Re-frame](https://github.com/day8/re-frame) is the standard state management for CLJS. If you are coming from Javascript it's like Redux but without the annoying boilerplate.

There is _app state_, in this project the default value is in `src/main/new_project_name/db.cljs`. It looks like this:
```clojure
(def default-app-db
  {:settings {:theme :dark}
   :version  "version-not-set"})
```
When you are in the REPL the runtime value is accessable from `re-frame.db/app-db`. More on that later.

Then there are _handlers_, in `src/main/new_project_name/handlers.cljs`, that allow for putting things into app state. Dispatch action on user interactions triggers them.

```clojure
;; handler definition and registration
(defn set-theme [db [_ theme]]
  (->> db
       (setval [:settings :theme] theme)))
(reg-event-db :set-theme [spec-validation] set-theme)

;; dispatching an event, from a component, that is picked up by the handler
          [:> paper/Switch {:value           (= theme-selection :dark)
                            :on-value-change #(>evt [:set-theme (if (= theme-selection :dark)
                                                                  :light
                                                                  :dark)])}]]
```
The `:>` macro is a [reagent react interop thing](http://reagent-project.github.io/docs/master/InteropWithReact.html). It takes the react class imported and wraps it around a reagent component.

There are _subscriptions_, in `src/main/new_project_name/subscriptions` that allow components to pull things out of state.

```clojure
(defn theme [db _]
  (->> db
       (select-one! [:settings :theme])))

(reg-sub :theme theme)
```

In the components these can be accessed with `(subscribe)`. There is a [helper function](https://lambdaisland.com/blog/2017-02-11-re-frame-form-1-subscriptions) for these called `<sub`. In `src/main/new_project_name` you can see it's usage.

```clojure
(defn home-scene [props]
  (r/as-element
   (let [version         (<sub [:version]) 
         theme-selection (<sub [:theme]) ;; <---------------------- use of the helper function to get a value out of app state
         theme           (.-theme props)]
     [:> paper/Surface {:style (.-surface styles)}
      [:> rn/View
       [:> paper/Card
        [:> paper/Card.Title {:title    "A nice Expo/Shadow-cljs template"
                              :subtitle "For quick project startup"}]
        [:> paper/Card.Content
         [:> rn/View {:style (.-themeSwitch styles)}
          [:> paper/Text {:style {:color (->> theme .-colors .-accent)}}
           "Dark mode"]
          [:> paper/Switch {:value           (= theme-selection :dark) ;; <------------------------- use of that app state value
                            :on-value-change #(>evt [:set-theme (if (= theme-selection :dark)
                                                                  :light
                                                                  :dark)])}]]
         [:> paper/Paragraph (str "Version: " version)]]]]])))
```

For any app that gets past the _toy_ stage the app-db (state) is going to get _complicated_. For that you will want [Specter](https://github.com/redplanetlabs/specter). It makes dealing with state objects easier. Deeply nested values in state are messy to deal with in just plain Clojure. It's still better than Object Oriented Programming but Specter brings it to the next level. The examples in this repo are super simple so it might not be apparent. But check out this from a more _involved_ project I've been working on.

```clojure
(defn apply-pattern-to-displayed-day [db [_ {:keys [pattern-id new-periods]}]]
  (let [displayed-day     (get-in db [:time-navigators :day])
        pattern           (->> db
                               (select-one [:patterns sp/ALL
                                            #(= (:id %) pattern-id)]))

        all-bucket-ids (->> new-periods
                            (select [sp/ALL :bucket-id]))]

    ;; put the periods in the buckets
    (->> db
         (transform
          [:buckets sp/MAP-VALS
           #(some? (some #{(:id %)} all-bucket-ids))
           (sp/collect-one (sp/submap [:id]))
           :periods]

          (fn [bucket old-period-map]
            (let [periods-to-add
                  (->> new-periods
                       ;; select periods for this bucket
                       (filter #(= (:bucket-id %) (:id bucket)))
                       ;; make the periods valid
                       (map #(pseudo-template->perfect-period
                              displayed-day %))
                       ;; turn list into a map indexed by id
                       ((fn [period-list]
                          (apply merge (->> period-list
                                            (map #(hash-map (:id %) %)))))))]
              ;; put the periods in the bucket
              (merge old-period-map periods-to-add)))))))
```

*Don't look too deply into this*. Basically there are _buckets_ in a map, indexed by id, that need time interval objects (_periods_) put into them. Those objects are coming from a template that represents a plan for a day. Because of Specter this is _only_ 30 lines of code. Before specter it was a lot more. Specter works really well with re-frame.

[Camel-snake-kebab](https://clj-commons.org/camel-snake-kebab/) is a nice little convenience. Reagent does a great job of converting component parameters from camel case to snake case for wrapped React components. There are some spots where you will want to use this library to do the same thing. In this example it is for the style sheets. In `src/main/new_project_name/app.cljs` you can see it's use to allow style properties to be referenced in camel case.

```clojure
(def styles
  ^js (-> {:surface
           {:flex           1
            :justify-content "center"}

           :theme-switch
           {:flex-direction  "row"
            :justify-content "space-between"}}
          (#(cske/transform-keys csk/->camelCase %))
          (clj->js)
          (rn/StyleSheet.create)))
```

[React Native paper](https://callstack.github.io/react-native-paper/getting-started.html) is an excellent component library. 

Another example this repo provides, although not a library, is unit tests. This is tricky in an Expo project because you will likely want to run unit tests in a ci/cd pipeline. Which means you will probably want a runtime in Node, not a native app running React Native. There are examples of handler and subscription unit tests. They utilize shadow-cljs [node tests configuration](https://shadow-cljs.github.io/docs/UsersGuide.html#target-node-test). Check out `src/test/new_project_name/` for the test files. They use a different build configuration in `shadow-cljs.edn`. 

### The REPL

[^push-notifications]: Push notifications are available for Android browsers.
[^git-gui]: I know, you don't need a gui, you know the command line. Previous git clients suck, yes. But this one is good. Everyone I've gotten to try it, even the most steadfast cli advocates, stick with it. The visual information density is high. That cannot be matched by the cli. The buttons pretty clearly map to cli commands and the client even has a log to show you what every gui action does on the command line. Use this client.
