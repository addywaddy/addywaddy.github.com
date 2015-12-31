---
layout: post
title: Clojure Web Development Reboot
date: 2015-12-31 14:12
categories:
---

After installing El Capitan last week, I decided it was time for a rethink of my setup for clojure web development. Up until now, I've been using the following:

* Leiningen
* Ring
* Compojure
* Component

But there are some new kids on the block that I wanted to take for a spin in a new project I'm working on. And as 2016 is but a few hours away, it's out with the old and in with the new! ;)

## Leiningen -> Boot
Leiningen is THE build and automation tool in the clojurespere, but boot seems to be gaining traction. Check out this article for a great overview of boot and what it can do for you [http://www.flyingmachinestudios.com/programming/boot-clj/](http://www.flyingmachinestudios.com/programming/boot-clj/).

## Ring.Core -> Aleph
Aleph has been around for a while now, but was rewritten just over a year ago, so it's newish :) Aleph is a Ring-spec compliant networking library running on Netty and using Manifold for representing streams.

## Compojure -> Bidi
I think most web developers have had a stab at writing a routing library at one time or another. I know [I have](https://github.com/addywaddy/autobahn/). Bidi is written for both clojure and clojurescript and avoids the use of macros, instead using data structures for defining your routes.

## System -> Mount
Mount is a clojure(script) library for managing application state similar to component but [with some differences](https://github.com/tolitius/mount#differences-from-component).



## Emacs -> Spacemacs!
One tool that I still use for clojure development is the Emacs/CIDER/nREPL toolchain. If you are already happy with your clojure development environment then skip the next section. If not - here's my setup (this is a Mac-only guide - sorry!):

1. Install [homebrew](http://brew.sh/)
2. Install Emacs with `brew install emacs-mac --with-spacemacs-icon`
3. Install spacemacs following [these steps](https://github.com/syl20bnr/spacemacs#install)
4. Edit your spacemacs file
  * add `clojure` to the `dotspacemacs-configuration-layers` section. This layer provides clojure-mode and Cider, among other things.
  * add `(add-to-list 'auto-mode-alist '("\\.boot$" . clojure-mode))` to the `dotspacemacs/user-config` function. This tells emacs to treat `.boot` files as clojure files.
  
Time to create our project :)

Create the following file structure:

    $ tree testapp
    .
    ├── build.boot
    ├── resources
    │   └── public
    │       └── hello.txt
    └── src
        └── clj
            └── testapp
                └── core.clj

Put anything you want into hello.txt. The contents of the other 2 files are as follows:

`$ cat build.boot`

{% highlight clj %}
    (set-env!
      :source-paths #{"src/cljs" "src/clj"}
      :resource-paths #{"resources"}
      :dependencies '[
                       [mount "0.1.7"]
                       [org.clojure/clojure "1.7.0"]
                       [aleph "0.4.1-beta2"]
                       [bidi "1.24.0"]
                       [org.clojure/tools.nrepl "0.2.11"]])

{% endhighlight %}

`$ cat src/clj/testapp/core.clj`

{% highlight clj %}

    (ns testapp.core
      (:require
        [aleph.http :as http]
        [mount.core :refer [defstate] :as mount]
        [bidi.ring :refer [make-handler] :as br]))

    (defn index []
      {:status 200
       :headers {"content-type" "text/plain"}
       :body "hello world!"})

    (def routes
      ["" [
            ["/" index]
            ["" (br/resources {:prefix "public"})]]])

    (def handler
      (make-handler routes))

    (defstate http-server
      :start (http/start-server handler {:port 3001})
      :stop (.close http-server))

{% endhighlight %}

Let's jump into an nREPL now to make sure everything is working:

1. Open spacemacs
2. Switch to our testapp directory with `C-x d` and entering the correct path.
3. Connect to an nrepl using cider with `M-x cider-jack-in`
4. Once it has connected, you may need to switch to the correct buffer using `C-x b`
5. Open up our `core.clj` file in a split next to the repl: `C-c 3` and then `C-x C-f` to find the file.
6. Switch to our testapp.core namespace by pressing `C-c M-n` while the cursor is in the file.
7. Compile the file using `C-c C-k`
8. In the cider repl, enter `(mount/start)` and
  * navigate to localhost:3001
  * navigate to localhost:3001/hello.txt
  
9. enter `(mount/stop)`. The server should have shut down.

Have a great start to 2016!
