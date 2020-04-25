version 1.0 - 13 mai 2020 (2 ans)
« en phase de développement très précoce »

Typescript: [x]

Modules standards (deno/std)
> portage de la bibliothèque standard de Go

### Exécutable unique
deno (15 Mo) + URL
both runtime and package manager (?)

### Sécurité
pas accès aux [système de fichier ou disque, environnement, réseau] sauf autorisation explicite
`--allow-read=/tmp` ou `--allow-net=google.com`

### Compatibilité avec le navigateur


## Différences avec Node.js
- n'utilise pas npm
- ni le package.json
- utilisation systématique des _promesses_
- gestion des erreurs (stoppe toujours l’exécution sur des erreurs non capturées)
- syntaxe import au lieu de require (ES Modules) + url HTTP
- le code distant est mis en cache (indéfiniment sauf --reload)

## Utilitaires / commandes
- Inspecteur de dépendances (deno info)
- Formateur de code (deno fmt)
- bundling (deno bundle)
- runtime type info (deno types)
- Test (deno test)
- Débogueur en ligne de commande (--debug)
- linter (deno lint) coming soon


## Ryan Dahl
2009: Node.js


## 10 Things I Regret About Node.js - Ryan Dahl - JSConf EU (2 juin 2018)
The goal of Node was event driven HTTP servers.(?)

5:04
> 1 Regret: Not sticking with Promises.
 * I added promises to Node in June 2009 but foolishly removed them in February 2010.
 * Promises are the necessary abstraction for async/await.
 * It's possible unified usage of promises in Node would have sped the delivery of the eventual standartization and async/await.
 * Today Node's many async APIs are aging baldly due to this.
[Pour l'asynchrone: promesse > callback]

6:02
> 2 Regret: Security
 * V8 by itself is a very good security sandbox
 * Had I put more thought into how that could be maintained for certain applications, Node colud have had some nice security guarantees not available in any other language.
 * Example: Your linter shouldn't get complete access to your computer and network.
[La sécurité]

7:01
> 3 Regret: The Build System (GYP)
 * Build systems are very difficult and very important.
 * V8 (via Chrome) started using GYP and I switched Node over in tow.
 * Later Chrome dropped GYP for GN. Leaving Node the sole GYP user.
 * GYP is not an ugly internal interface either - it is exposed to anyone who's trying to bind to V8.
 * It's an awful experience for users. It's this non-JSON, Python adaptation of JSON.
 * The continued usage of GYP is the probably largest failure of Node core.
 * Instead of guiding users to write C++ bindings to V8, I should have provided a core foreign function interface (FFI)
 * Many people, early on, suggested moving to an FFI (namely Cantrill) and regrettably I ignored them.
 * (And I am extremely displeased that libuv adopted autotools.)
[GYP pas bien]

9:52
> 4 Regret: package.json
 * Isaac, in NPM, invented package.json (for the most part)
 * But I sanctioned it by allowing Nod's require() to inspect package.json files for "main"
 * Ultimately I included NPM in the Node distribution, which much made it the defacto standard.
 * It's unfortunate that there is centralized (privately controlled even) repository for modules.
 * Allowing package.json gave rise to the concept of a "module" as a directory of files.
 * This is no a strictly necessary abstraction - and one that doesn't exist on the web.
 * package.json now includes all sorts of unnecessary information. License? Repository? Description? It's boilerplate noise.
 * If only relative files and URLs were used when importing, the path defines the version. There is no need to list dependencies.
[Pas besoin de tout ça]

12:35
> 5 Regret: node_modules
 * It massively complicates the module resolution algorithm.
 * vendored-by-default has good intentions, but in practice just using $NODE_PATH wouldn't have precluded that.
 * Deviates greatly from browser semantics
 * It's my fault and I'm very sorry.
 * Unfortunately it's impossible to undo now.
[Compliqué]

14:00
> 6 Regret: require("module") without the extension ".js"
 * Needlessly less explicit.
 * Not how browser javascript works. You cannot omit the ".js" in a script tag src attribute.
 * The module loader has to query the file system at multiple locations trying to guess what the user intended.
[Pourquoi]

14:40
> 7 Regret: index.js
 * I thought it was cute, because there was index.html
 * It needlessly complicated the module loading system.
 * It became especially unnecessary after require supported package.json
[Hum]

15:28 Talks about Deno.


### Problèmes de Node.js
- les promesses
- sécurité faible (?)
- GYP

- un système de modules mal conçu, avec une distribution centralisée ;
- un grand nombre d'API héritées qui doivent être prises en charge ;
- un manque de sécurité.

## Deno c'est quoi?
[V8, ~~Go~~>Rust, et Tokio]
> Deno est un __runtime en ligne de commande pour exécuter du code Javascript__ et Typescript. Comme NodeJS, Deno est __construit sur le moteur Javascript de Chrome: V8__. __Il est écrit entièrement en Rust__ (Nodejs est écrit en C et C++) ce qui promet pas mal de performances. Enfin pour garantir de l’I/O asynchrone __l’event loop utilisée est Tokio__. C’est celle de Rust, l’équivalent de libuv en NodeJS. Si je te parle chinois, c’est que t’as pas lu mon [article sur le fonctionnement de Javascript](https://www.jesuisundev.com/comprendre-javascript-en-5-minutes) et je t’invite à le faire.
> Il va télécharger puis compiler le serveur HTTP et ses dépendances au moment du runtime (?)



```javascript
import { serve } from "https://deno.land/std@v0.36.0/http/server.ts";
const s = serve({ port: 8000 });
console.log("http://localhost:8000/");
for await (const req of s) {
  req.respond({ body: "Hello World\n" });
}
```


# Deno (software)
https://en.wikipedia.org/wiki/Deno_(software)

Deno is a runtime for JavaScript and TypeScript that is based on the V8 JavaScript engine and the Rust programming language. It was created by Ryan Dahl, original creator of Node.js, and is focused on security and productivity.[5] It was announced by Dahl in 2018 during his talk "10 Things I Regret About Node.js".[6] Deno explicitly takes on the role of both runtime and package manager within a single executable, rather than requiring a separate package-management program.[7][8]

## History
Deno was announced on JSConf EU 2018 by Ryan Dahl in his talk "10 Things I Regret About Node.js".[6] In his talk, Ryan mentioned his regrets about the initial design decisions with Node.js, focusing on his choices of not using Promises in API design, usage of the legacy GYP building system, node_modules and package.json, leaving out file extensions, magical module resolution with index.js and breaking the sandboxed environment of V8.[9] He eventually presented the prototype of Deno, aiming to achieve system call bindings through message passing with serialization tools such as Protocol Buffers, and to provide command line flags for access control.
Deno was initially written in Go and used Protocol Buffers for serialization between privileged (Go, with system call access) and unprivileged (V8) sides.[10] However, Go was soon replaced with Rust due to concerns of double runtime and garbage collection pressure.[11] Tokio is introduced in place of libuv as the asynchronous event-driven platform,[12] and Flatbuffers is adopted for faster, "zero-copy" serialization and deserialization[13] but later in August 2019, FlatBuffers were finally removed[14] after publishing benchmarks that measured a significant overhead of serialization in April 2019.[15]
A standard library, modeled after Go's standard library, was created in November 2018 to provide extensive tools and utilities, partially solving Node.js' dependency tree explosion problem.[16]

## Overview
Deno aims to be a productive and secure scripting environment for the modern programmer.[7] Similar to Node.js, Deno emphasize on event-driven architecture, providing a set of non-blocking core IO utilities, along with their blocking versions. Deno could be used to create web servers, perform scientific computations, etc.
Comparison with Node.js[edit]
Deno and Node.js are both runtimes built on Google's V8 JavaScript engine, the same engine used in Google Chrome. They both have internal event loops and provide command-line interfaces for running scripts and a wide range of system utilities.
Deno mainly deviates from Node.js in the following aspects[7]:
- Uses ES Module as the default module system, instead of CommonJS.
- Uses URLs for loading local or remote dependencies, similar to browsers.
- Includes a built-in package manager for resource fetching, thus no need for NPM.
- Supports TypeScript out of the box, using a snapshotted TypeScript compiler with caching mechanisms.
- Aims better compatibility with browsers with a wide range of Web API.
- Allows control to file system and network access in order to run sandboxed code.
- Redesigns API to utilize Promises, ES6 and TypeScript features.
- Minimizes core API size, while providing a large standard library with no external dependencies.
- Using message passing channels for invoking privileged system APIs and using bindings.

## Example

The following runs a basic Deno script without any read/write/network permissions (sandbox mode):
```
deno run main.ts
```

Explicit flags are required to expose corresponding permission:
```
deno run --allow-read --allow-net main.ts
```

To inspect the dependency tree of the script, use the `info` subcommand:
```
deno info main.ts
```

A basic hello-world program in Deno looks like the following (as in Node.js):
```
console.log("Hello world");
```

Deno provides the global namespace for most Deno specific APIs that are not available in the browser. A Unix cat program could be implemented as follows:
```
/* cat.ts */

/* Deno APIs are exposed through the `Deno` namespace. */
const { stdout, open, copy, args } = Deno;

// Top-level await is supported
for (let i = 0; i < args.length; i++) {
    const filename = args[i]; // Obtains command-line arguments.
    const file = await open(filename); // Opens the corresponding file for reading.
    await copy(stdout, file); // Performs a zero-copy asynchronous copy from `file` to `stdout`.
}
```
The above Deno.copy function works in the similar way as Go's io.Copy, where stdout (standard output) is the destination Writer and file is the source Reader. To run this program, we need to provide the read permission to the filesystem:
```
deno run --allow-read cat.ts myfile
```

The following Deno script implements a basic HTTP server:
```
// Imports `serve` from the remote Deno standard library, using URL.
import { serve } from "https://deno.land/std@v0.21.0/http/server.ts";

// `serve` function returns an asynchronous iterator, yielding a stream of requests
for await (const req of serve({ port: 8000 })) {
    req.respond({ body: "Hello World\n" });
}
```

When running this program, Deno would automatically download and cache the remote standard library files and compile the code. Similarly, we can run a standard library script (such as a file server) directly without explicitly downloading, by providing the URL as input filename (-A turns on all permissions):
```
$ deno run -A https://deno.land/std/http/file_server.ts
Download https://deno.land/std/http/file_server.ts
Compile https://deno.land/std/http/file_server.ts
...
HTTP server listening on http://0.0.0.0:4500/
```



































