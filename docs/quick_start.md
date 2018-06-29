# Quick Start

This document is intended to get you aquainted with the features and
workflow of Figwheel.

This quick start is based on the Clojure CLI tools. If you are on
Windows Opersting System you will want to use the [Leiningen version of
this document](/quick_start_lein.html)

## Install the Clojure CLI tools

You will want to install the Clojure tools, they are very helpful in
working with Clojure in general.

Install the [Clojure CLI tools](CLI tools).

If you are on Mac OSX and you can quickly install the Clojure tools
via [homebrew](brew).

In the terminal at the shell prompt enter:

```shell
$ brew install clojure
```

If you've already installed Clojure now would be a good time to ensure
that you have updated to the latest version with:

```shell
$ brew upgrade clojure
```

You can check that everything installed OK by running a Clojure REPL

```shell
$ clj
```

You should a `user=>` prompt where you can enter Clojure code. 

Try entering `(+ 1 2 3)` you should get a response of `6` along with
the next prompt asking you for more code. Type `Control-C` to get out
of the Clojure REPL.

If everything worked well you have successfully installed Clojure!

## Make a new directory to work in

Make a directory for an example ClojureScript project.

For example in `/Users/[your username]/workspace` you could create a
directory called `hello-cljs`:

```
workspace$ mkdir hello-cljs
```

## Specifying that you want to use Figwheel

Figwheel is a library, or rather it is a Jar of Clojure code that you
will use. If you are familiar Ruby's `bundler` and `Gemfile`, Python's
`pip` and `requirements.txt`, or Javascript's `npm/yarn` and
`package.json` then the concept of specifying dependencies in a file
should be familiar to you.

Figwheel is just a dependency and with the Clojure Tools the way you
will specify that you want to have certain libraries available to you
is in the `deps.edn` file.

So in your `hello-cljs` directory create a file `deps.edn` with the
following contents.

```clojure
{:deps {com.bhauman/figwheel-main {:mvn/version "0.1.4"}}}
```

## Starting a ClojureScript REPL

Now that you have done that you can get a ClojureScript REPL running
straight away. Make sure you are still in the `hello-cljs` directory
and enter:

```clojure
clojure -m figwheel.main
```

This should boot up a ClojureScript REPL and pop open a Browser
window with a page like this:

![Repl host page in browser](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/fm-repl-host-page.png)

If you see the green "Connected" animation it means that this page is
connected to the REPL that you just launched. This webpage is where all
the ClojureScript expressions that you type into the REPL will be
evaluated.

Speaking of the REPL, if you head back to the terminal window where you
launched Figwheel from, you should now see something like:

![figwheel repl prompt in terminal](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/figwheel-main-repl.png)

If this is the case, you have successfully started a ClojureScript REPL
and you can now type ClojureScript at the `cljs.user=>` prompt.

Let's try some ClojureScript. Type the following expressions at the
prompt as demonstrated in the example REPL session below:

```clojure
cljs.user=> (println "Hello World!")
Hello World!
nil
cljs.user=> (range 5)
(0 1 2 3 4)
cljs.user=> (map (fn [x] (+ 1 x)) (range 5))
(1 2 3 4 5)
cljs.user=> (filter odd? (map (fn [x] (+ 1 x)) (range 5)))
(1 3 5)
cljs.user=> (js/alert "ClojureScript!")
nil
```

That last expression should cause a JavaScript Alert to pop up in the
browser on our REPL host page.

## Amping up the REPL

The REPL we just launched will do fine for trying simple expressions
but if you are learning a new language it's nice to have a REPL that
can 

* syntax highlight the code as you type it
* allows for easy multi-line editing of expressions
* autocomplete the current function name that you are typing
* display the documentation for the function where your cursor is
* display the source code of the function where your cursor is
* allows you to query for functions that are similar to the word under your cursor

My library [Rebel Readline](rebel) provides these features for Clojure REPLs and it
will be very helpful for further explorations of using Figwheel and ClojureScript.

Add `com.bhauman/rebel-readline-cljs 0.1.4` as another dependency in your `deps.edn` file:

```clojure
{:deps {com.bhauman/figwheel-main {:mvn/version "0.1.4"}
        com.bhauman/rebel-readline-cljs {:mvn/version "0.1.4"}}}
```

When you launch `figwheel.main` now it will detect the presence of
`com.bhauman/rebel-readline-cljs` and it will launch a REPL using this
new and improved terminal line reader.

Let's launch `figwheel.main` again, from the `hello-cljs` directory:

```shell
$ clojure -m figwheel.main
```

After launching a Browser will open and a REPL will start just as when
we launched it above. However, you will see the following line right
before the `cljs.user=>` prompt. 

```shell
[Rebel readline] Type :repl/help for online help info
```

This notifies you that you are using Rebel Readline. 

If you type `:repl/help` at the prompt you will first notice that
`:repl/help` itself is now has color syntax highlighting. Upon hitting
enter it will output a useful reference for the REPL's capabilities.

![rebel readline help](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/rebel-readline-help.png)

#### Rebel Readline feature walkthrough

Let's quickly walk through how to use some of the **Clojure Key
Bindings** listed in the help.

Type `(ra` at the prompt and don't hit enter but hit the TAB key.

You will see a list of choices that you can TAB through or you can
keep typing to narrow the selection down.

Select `rand-int`, you will now have `cljs.user=> (rand-int` on the
line. 

Let's find out how to use `rand-int`. Hit `Control-X Control-D` to
bring up the documentation for `rand-int`, upon doing so you will see 

![rebel redline displaying rand-int doc](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/demo-rebel-documentation.png)

This documentation is showing you the function argument signature
`([n])` which indicates that it takes one argument `n` and from the
rest of the documentation and the name of the function we can infer
that it takes a single integer argument.

If we want to know how `rand-int` was implemented we can see it's
source code by hitting `Control-X Control-S`.

Now that we know that `rand-int` takes a single integer argument let's
call it. Complete the REPL line by typing `(rand-int 5)` and hit enter
and you should indeed get a random number from 0 to 4.

If you hit the up arrow you can get `(rand-int 5)` back at the REPL
prompt and you can hit enter again to get a different result.

Now let's look at a function with a more complex argument signature.

Enter `(range` at the prompt (use TAB completion if you like). Now
look at it's documentation with `Control-X Control-D` you will notice
that the argument signature is described differently. 

```clojure
([] [end] [start end] [start end step])
```

It's actually just a list of signatures. So for `range` you can call
it with no arguments `[]` which will return an infinite sequence (not
recommended at the REPL), you can call it with one argument `[end]`
specifying where a range starting at `0` should end, you can call it
with the other argument arities `[start end]` and `[start end step]`.

Let's try this:

```clojure
cljs.user=> (range 4)
(0 1 2 3)
cljs.user=> (range 4 10)
(4 5 6 7 8 9)
cljs.user=> (range 4 10 2)
(4 6 8)
```

We intentionally didn't try to use `(range)` you can try this now.  It
should freeze the REPL and the browser because the browser is now
stuck in a tight loop trying to iterate through all the integers up
the maximum integer possible.

> **TIP**: Sometimes its easy to forget that the REPL is backed by a
> browser window, which is a very simple thing to reset. If your REPL
> gets stuck in a tight loop, you can return to the REPL host page,
> take note of the URL (most likely `http://localhost:9500/` in our
> case) and then explicitly close it (a page reload often doesn't work
> in this case) to kill the tight loop and then open it again. At this
> point the REPL should have timed out and returned to functioning
> normally. If you are not in a tight loop and need to reset the state
> of the REPL at any point you can simply reload the REPL host page
> and that will give you a fresh slate to start from.

Okay, back from the brink?

Be sure that you have started a REPL again and that you are back at
the `cljs.user=>` prompt.

There is one more Rebel Readline feature that I'd like to go over
before moving on.

At the prompt enter the expression `(+ 1 2 3 4)`, and after that when
your cursor is just after the last `)`, hit `Control-X Control-E`.

You should see that the expression was evaluated and the result `#_=>
10` displayed just under the line where your cursor is. Rebel readline
allows you to evaluate any expression or sub expression before hitting
enter.

Let's try that again, now get back to an empty prompt and enter the
expression `(+ (+ 1 2 3) (+ 4 5 6))` and now place the cursor after
the last paren of the sub-expression `(+ 1 2 3)`. If you hit
`Control-X Control-E` at this point you will see that you get the
value `6`. Experiment with evaluating in other parts of the expression
on this line.

What's the value of this feature? It allows you to work on larger
multiline expressions while verifying that the sub expressions are
doing what you expect them to do.

> Having easy to parse expressions is part of the magic of LISP. It
> allows tools to understand delimited expressions without having to
> implement a complete parser for the language.

Also you may have noticed that you can create multi-line expressions
at the REPL prompt in a fairly straight forward way.

Try enterinng the following expression at the prompt and make sure you
format it so that it spans multiple lines.

```clojure
cljs.user=> (+ 1
       #_=>    2
       #_=>    3)
```

You should notice that you were able to hit enter to create newlines
in your expression while the cursor was inside of an **open
expression** and that once you closed the expression (by adding the
last paren), when you hit enter it was submitted for evaluation.

You can exit the Rebel Readline REPL by hitting `Control-C Control-D`.

## Break Time

Once you have made it this far you have learned how to add
dependencies to `deps.edn` and how to start a `figwheel.main` REPL
with Clojure's CLI tools. You have then learned how to include
[Rebel Readline](rebel) and how to use it to intraspect your
environment and evalute code.



This is more than enough to justify a break, may a suggest a nice walk
or perhaps a chat with a co-worker nearby?

## Working at the REPL

Now let's create a file that contains some ClojureScript source code
and load it into the REPL.

We'll have to create a file where it can be found, which means it will
have to be on the classpath. It just so happens the the Clojure tool
adds the local `src` directory to the classpath.

Let's create a file there. 

Leave the REPL running and create a file `src/hello/cruel_world.cljs`
with the following contents:

```clojure
(ns hello.cruel-world)

(defn what-kind? []
  "Cruel")
```

The file layout should be of your project should now be:

```shell
hello-cljs
├── deps.edn
└── src
    └── hello
        └── cruel_world.cljs
```

Take note that we are declaring a namespace `hello.cruel-world` and
that the path to our file mirrors this namespace and it is rooted in
the `src` directory which is on the classpath. This is what will allow
the REPL to find and compile our code.

> **TIP**: an extremely common mistake is to forget to use replace
> hyphens `-` with underscores `_` when creating a namespace path for
> a source file.

Now that we have created the file with the `hello.cruel-world`
namespace let's require it into our running REPL.

```clojure
cljs.user=> (require 'hello.cruel-world)
nil
```

That `nil` response is exactly what we want, the REPL will let you now
if it couldn't find the file or if there was a problem compiling it.

Now we can call functions defined in `hello.cruel-world`

```clojure
cljs.user=> (hello.cruel-world/what-kind?)
"Cruel"
```

So above, we call our function and it returns the string `"Cruel"` as
expected.

But it seems like a bit much to type so let's require
`hello.cruel-world` again and create an alias for it. Remeber that you
can hit the up arrow to get back to the original require statement and
edit it.

```clojure
cljs.user=> (require '[hello.cruel-world :as world])
cljs.user=> (world/what-kind?)
"Cruel"
```

As you can see we created an alias `world` for our namespace and we
can now call functions in that namespace with that alias.

Now let's change the and reload it.

Alter the `what-kind?` function int the `src/hello/cruel_world.cljs`
file so that it looks like this:

```clojure
(ns hello.cruel-world)

(defn what-kind? []
  "Brilliantly Cruel")
```

Now we can head back to the REPL prompt and reload the
`hello.cruel-world` namespace like so:

```clojure
cljs.user=> (require '[hello.cruel-world :as world] :reload)
nil
```

And then verify that our changes were loaded:

```clojure
cljs.user=> (world/what-kind?)
"Brilliantly Cruel"
```

There is one more quick trick that you will find helpful while working
at the REPL. You can change your current namespace (`cljs.user`) to
another loaded namespace.

So for instance we can change into the `hello.cruel-world` namespace
like so:

```clojure
cljs.user=> (in-ns 'hello.cruel-world)
nil
hello.cruel-world=>
```

You will see the prompt change and now that your current namespace is
`hello.cruel-world` you can call functions in that namespace without
needing to provide a namespace.

For example:

```clojure
hello.cruel-world=> (what-kind?)
"Brilliantly Cruel"
```

Remember that you are working in a Browser environment so you can
interact with it. 

```clojure
hello.cruel-world=> (js/document.getElementById "app")
#object[HTMLDivElement [object HTMLDivElement]]
```

There is an "app" element available on the REPL host page. Let's
override the content on it with content from our `what-kind?`
function.

```clojure
hello.cruel-world=> (def app-element (js/document.getElementById "app"))
#'hello.cruel-world/app-element
hello.cruel-world=> (set! (.-innerHTML app-element) (what-kind?))
"Brilliantly Cruel"
```

Well that was just a brief tour of what you can do working from the
REPL. This knowlegde should be enough to allow you to explore the
ClojureScript language a bit. 

> **TIP**: Much of what you learned above applies equally well to
> Clojure. So if you would like to try your hand at Clojure, as well,
> you can get a working Rebel Readline Clojure REPL by typing `clojure
> -m rebel-readline.main` in the `hello-cljs` directory. If you want
> to create and load Clojure files, everything is the same as above
> except Clojure files end with `.clj`

## Working with code more interactively

Sometimes you want to start the REPL with some code already compiled and loaded.

You can accomplish this by using the following command:

```clojure
$ clojure -m figwhee.main --compile hello.cruel-world --repl
```

You will notice some differences in the output this time:

![compile terminal output](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/fm-cruel-world-compile.png)

The output looks very similar to launching launching a REPL without
compiling. There are some important differences that I'd like to
address next.

The first thing of note is this warning:

![target classpath warning](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/fm-cruel-world-compile-warning.png)

This is absolutely expected and important. When started the
`figwheel.main` REPL without any arguments, it used a temporary
directory to host the compiled ClojureScript assets and added it to
the classpath automatically.

The path to the compiled assets must be on the classpath so that they
can be served by the built in figwheel server.

Running `figwheel.main` normally indicates that you are just
experimenting with some code and that you are not working on a project
that you have a commitment to yet. Once you start compiling things
this is an indication that we are commiting to a project of some kind
and thus we have to start maintaining the classpath correctly if we
want to be able to compile and run our code with `cljs.main` and other
tools that don't automatically add paths to the classpath for you.

We can fix the classpath warning by adding both `"src"` and `"target"`
to the `:paths` key in our `deps.edn` file:

```clojure
{:deps {com.bhauman/figwheel-main {:mvn/version "0.1.4"}
        com.bhauman/rebel-readline-cljs {:mvn/version "0.1.4"}}
 :paths ["src" "target"]}
```

We have to explicitly include `"src"` as well as `"target"` as once
you define a `:paths` key it becomes the definition and there are no
other implicit paths added.

Now we will return to run our REPL again from the `hello-cljs`
directory.

```clojure
$ clojure -m figwhee.main --compile hello.cruel-world --repl
```

You should see the familiar output when starting a `figwheel.main` REPL.

You shold see that your ClojureScript is being compiled to the local
"target" directory now and not to some temporary directory.

You should also notice this line:

![watching line in output](https://s3.amazonaws.com/bhauman-blog-images/figwheel-main/watching-line.png)

This line indicates that Figwheel is now watching the ClojureScript
files in the `src` directory for changes. When a change is detected
Figwheel will now hot reload it into the JavaScript environment.

This is a big difference from reloading the `hello.cruel-world`
namespace by hand at the REPL, now when you change and save the
`src/hello/cruel_world.cljs` file it will be automatically loaded for
you.

Let's try this out.

First let's verify that `hello.cruel-world` is loaded.

```clojure
cljs.user=> (hello.cruel-world/what-kind?)
"Brilliantly Cruel"
```

Looks good! We successfully started the repl with our namespace
already available so we don't have to explicitly require it.

Now let's change the namespace. Go to the `src/hello/cruel_world.cljs`
and change the `what-kind?` function so it returns `"Cruel No More"`
and then save the file.

```clojure
(ns hello.cruel-world)

(defn what-kind? []
  "Cruel No More")
```

Once you save the file you should see a green `Successfully compiled`
message appear in the REPL. You can now check the REPL to see that
your code has indeed been reloaded automatically. (Don't forget that
you can use TAB to help you enter the following)

```clojure
cljs.user=> (hello.cruel-world/what-kind?)
"Cruel No More"
```

Our code has been loaded. Now, we don't have to explicitly reload a
namespace as we work, we can simply save the file we are working on
and it will be reloaded automatically.

Now let's demonstrate how Figwheel provides feedback while you are
working. 

Normally when you are working on a Web Application your attention is
on the web page you are working on in the Browser. So let's bring our
focus to the "Figwheel Default Dev Page" that was launched.











[brew]: https://brew.sh/
[CLI Tools]: https://clojure.org/guides/getting_started#_installation_on_mac_via_code_brew_code 
[rebel]: https://github.com/bhauman/rebel-readline