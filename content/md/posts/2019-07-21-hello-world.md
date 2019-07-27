{:title "Hello World" :layout :post :tags []}

# Workflow

```clojure
(defn export-md [org]
  (key-combo org ", e e g G"))

(defn write-to-md-content [export-buffer]
  (key-combo export-buffer ":w ../md/$TYPE/$DATE$FILE_NAME"))

(-> org-post
    export-md
    write-to-md-content)
```
