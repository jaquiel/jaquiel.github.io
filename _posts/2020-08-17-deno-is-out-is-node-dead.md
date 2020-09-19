---
layout: post
title:  "Deno is out. Is Node dead?"
date:   2020-08-17 14:14:54 -0300
categories: deno node javascript typescript
---
In 13th May 2020, finally Deno is out, after two years of its first release.

For all the years of my career always I heard about the end of one or the other programing language or technology. So it was with Delphi, also PHP, within others. More recently it was the turn of the Node.Js.

One day, maybe every programming language will die (_I'm not sure ..._), but I really don't believe in the death of Node.js. At least not now.

#But, what is Deno?

As written in the Deno documentation, it is a secure runtime for JavaScript and TypeScript that uses V8 and is built in Rust. You can see more details in https://deno.land.

Basically, Deno is a program for running JavaScript and TypeScript code outside of a browser, just like the Node.

Deno was created by Ryan Dahl, the same creator of Node, but now with focus in security and productivity. It was announced by Dahl in 2018 during his talk "10 Things I Regret About Node.js" at [JSConf EU](https://2018.jsconf.eu/) that year.

{% youtube M3BM9TB-8yA %}

#Introduction to Deno Features

First, to start, we need to install Deno and this task is very easy for any operating system. See more at https://deno.land/#installation.

Deno is a command-line program. After its installation, you can get using the following commands to help you start works with it.

```
$ deno help
```

```
$ deno --h
```

```
$ deno --help
```


To launch a Deno app you need simply use at the command line: 

```
$ deno run <entry-point>
```

This entry point can be a JavaScript (_.js_) file or TypeScript (_.ts_) file. But the great news is the possibility to use a URL that points to an app entry point.

Deno’s website provides some examples, like these.

Let’s run it to see what happens.

```
$ deno run https://deno.land/std/examples/welcome.ts
Download https://deno.land/std/examples/welcome.ts
Compile https://deno.land/std/examples/welcome.ts
Welcome to Deno 🦕
```

Deno downloaded the _`welcome.ts`_ file, and compiled it, and ran it. If we run the app again, Deno will just run it, because it’s cached by Deno.

```
$ deno run https://deno.land/std/examples/welcome.ts
Welcome to Deno 🦕
```

Deno downloads all the modules and caches them. It will not download them again until you specifically request them with the _`reload flag`_.

```
$ deno run --reload https://deno.land/std/examples/welcome.ts
Download https://deno.land/std/examples/welcome.ts
Compile https://deno.land/std/examples/welcome.ts
Welcome to Deno 🦕
```

And the best so far is that when Deno runs the app, it only compiled the TypeScript file, that is, we don’t need to use any __transpiler__ for that.    

It happens because Deno __supports Typescript out of the box__.

##EcmaScript modules vs. CommonJS

Deno uses the last ECMAScript features in its API and libraries and because of the native support of ES Modules in Deno you don’t have to use other build tools to make your application ready to use in a browser.

Deno supports ES Modules, instead of the CommonJS syntax used by Node. As a result, dependency management is very simple and flexible and it just uses a local or remote URL, but it provides compatibility with Node too.

For example, require functions (_`require()`_) is not supported. The ES Modules standard syntax uses the import statement for that (_`import defaultExport from "module-name"`_).

To load CommonJS modules you can use _`createRequire(...)`_, like the example below.

```javascript
import { createRequire } from "https://deno.land/std/node/module.ts";
 
const require = createRequire(import.meta.url);
// Loads native module polyfill.
const path = require("path");
// Loads extensionless module.
const cjsModule = require("./my_mod");
// Visits node_modules.
const leftPad = require("left-pad");
```

More details about that and examples like this can be seen at the link https://deno.land/std/node.

As we saw earlier, we can use a URL as an entry point to run a Deno application. And as we can also use a local or remote URL to manage the dependencies I created a repository on [GitHub](https://github.com/jaquiel/deno-features/tree/master/std) to send some examples from Deno and I can simply import dependencies from there.

I simply created a file called _`hello.ts`_ that imports a _welcome.ts function_ that prints the same standard greeting message that we used in the previous examples.

Let's see below the content of _`hello.ts`_.

```javascript
import { welcome } from 'https://github.com/jaquiel/deno-features/raw/master/std/welcome.ts'

welcome()
```

Now, the result of running the app.

```
$ deno run hello.ts
Compile file:///C:/Users/jaquiel/Documents/vscode/deno/deno-features/std/hello.ts
Download https://github.com/jaquiel/deno-features/raw/master/std/welcome.ts
Download https://raw.githubusercontent.com/jaquiel/deno-features/master/std/welcome.ts
Welcome to Deno 🦕
```

It's really very _easy_ and _simple_ to work with Deno.

#Security  

As previously seen, in Deno's own documentation it is defined as __a secure runtime for JavaScript and TypeScript__. To access to security-sensitive areas or functions, you must use permissions on the command line.

Let's see an example of the Deno website.

Running without using the `--allow-net` flag we will get the following result.

```
$ deno run https://deno.land/std/examples/echo_server.ts
error: Uncaught PermissionDenied: network access to "0.0.0.0:8080", run again with the --allow-net flag
    at unwrapResponse ($deno$/ops/dispatch_json.ts:43:11)
    at Object.sendSync ($deno$/ops/dispatch_json.ts:72:10)
    at Object.listen ($deno$/ops/net.ts:51:10)
    at Object.listen ($deno$/net.ts:152:22)
    at https://deno.land/std/examples/echo_server.ts:4:23

```

We can see that we had a permission denied error. But if we use the _flag_ the app will run without any problem.

```
$ deno run --allow-net https://deno.land/std/examples/echo_server.ts
Listening on 0.0.0.0:8080
```

In the link https://deno.land/manual/getting_started/permissions#permissions-list , we can see the detailed list of all available permissions.

#Conclusion

Well, my goal wasn’t to explain Deno's features to you, because the documentation can help you understand it easily. Rather, I just wanted to try these advantages in practice. 
 
I reiterate my opinion that Node.js will not die and will not be replaced by Deno, because it is a well-established technology that will certainly remain for many years and, in addition,  its own creator developed Deno only as an alternative to Node, and not as a replacement.

Finally, although I believe in a long life for Node I can't help saying that the new Deno’s features are truly exciting and it is, likely, the new Hype.
