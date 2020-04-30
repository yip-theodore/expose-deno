# Deno
Ryan Dahl - Node.js (2009)
10 Things I Regret About Node.js - Ryan Dahl - JSConf EU (2 juin 2018)
version 1.0 - 13 mai 2020


## Exécutable unique
deno (15 Mo)
Bibliothèque standard (deno/std)

## Sécurité
~~Node: pas~~ besoin d'autorisation explicite pour accéder au système de fichier ou au réseau

## Expérience développeur
### Typescript
supporté par défaut
### Sous-commandes utilitaires
info, fmt, bundle, types, test, --debug, lint
### Gestion des erreurs
~~Node: erreurs non capturées ignorées~~ stoppe toujours l’exécution

## Fonctionnement interne
### Asynchrone
~~Node: callback~~ Basé sur les promesses
### Écrit en
~~Node: GYP~~
[V8, ~~Go~~>Rust, et Tokio]
~~Node: C C++~~ Rust
~~Node: libuv~~ Tokio (Rust)

## Gestion des dépendances
~~Node: package.json~~
~~Node: npm (distribution centralisée)~~
syntaxe import au lieu de require (ES Modules) + url HTTP
le code distant est mis en cache (indéfiniment sauf --reload)


---

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

> Deno est un environnement d'exécution JavaScript et TypeScript construit sur le moteur JavaScript V8 et le language de programmation Rust.
Ryan Dahl, créateur de Node.js - la sécurité et la productivité
"10 choses que je regrette à propos de Node.js" 2018
environnement d'exécution et gestionnaire de paquets à la fois, en un seul exécutable ~~npm~~

## History
Deno was announced on JSConf EU 2018 by Ryan Dahl in his talk "10 Things I Regret About Node.js".[6] In his talk, Ryan mentioned his regrets about the initial design decisions with Node.js, focusing on his choices of not using Promises in API design, usage of the legacy GYP building system, node_modules and package.json, leaving out file extensions, magical module resolution with index.js and breaking the sandboxed environment of V8.[9] He eventually presented the prototype of Deno, aiming to achieve system call bindings through message passing with serialization tools such as Protocol Buffers, and to provide command line flags for access control.
Deno was initially written in Go and used Protocol Buffers for serialization between privileged (Go, with system call access) and unprivileged (V8) sides.[10] However, Go was soon replaced with Rust due to concerns of double runtime and garbage collection pressure.[11] Tokio is introduced in place of libuv as the asynchronous event-driven platform,[12] and Flatbuffers is adopted for faster, "zero-copy" serialization and deserialization[13] but later in August 2019, FlatBuffers were finally removed[14] after publishing benchmarks that measured a significant overhead of serialization in April 2019.[15]
A standard library, modeled after Go's standard library, was created in November 2018 to provide extensive tools and utilities, partially solving Node.js' dependency tree explosion problem.[16]

> avoir abandonné le concept des promesses lors de la conception
> l'utilisation de GYP, système de compilation abandonné
> node_modules et le package.json
> les extensions de fichier
> résolution des modules magique et l'index.js
> casse l'environnement sécurisé de V8

> Go + Protocol Buffers
> ~~Go~~ > Rust
> ~~libuv~~ > Tokio (platforme orientée événements asynchrone?)
> ~~Protocol Buffers~~ > ~~Flatbuffers~~
> Bibliothèque standard

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

> Architecture événementielle, série d'utilitaires entrées-sorties non bloquants
> boucle d’événement interne, interface en ligne de commande, utilitaires système

> utilisation de ECMAScript Module pour système de module ~~CommonJS~~
> utilisation des URLs pour charger les dépendances externes, similairement aux navigateurs
> Supporte le TypeScript par défaut (snapshotted compiler?)
> Meilleur compatibilité avec les navigateurs avec un large panel d'interfaces de programmation d'application Web
> Permet de contrôler l'accès au système de fichiers et au réseau
> Réécriture des API en utilisant les Promesses, ES6 et Typescript
> Minimisation du poids du noyeau, en mettant à disposition une large bibliothèque de modules standard
> Passage de messages pour les APIs du système privilégié et lien?

## Example

The following runs a basic Deno script without any read/write/network permissions (sandbox mode):
> Exécute un simple script Deno sans aucune permission de lecture, d'écriture, ou d'accès réseau, (en mode bac à sable)
```
deno run main.ts
```
Explicit flags are required to expose corresponding permission:
> option explicite pour accorder des permissions
> --allow-read=/tmp, --allow-net=google.com, --allow-write, --allow-run, --allow-env
```
deno run --allow-read --allow-net main.ts
```
To inspect the dependency tree of the script, use the `info` subcommand:
> Inspecter l'arbre de dépendances d'un script avec la sous-commande info
```
deno info main.ts
```
A basic hello-world program in Deno looks like the following (as in Node.js):
```
console.log("Hello world");
```
Deno provides the global namespace for most Deno specific APIs that are not available in the browser. A Unix cat program could be implemented as follows:
> Exemple d'implémentation du programme Unix cat
> Les APIs de Deno sont exposées dans l'objet Deno
> top-level await est supporté
> Récupère les arguments de la ligne de commande
> zero-copy
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
> Ce script Deno implémente un serveur HTTP basique
> Importe serve de la bibliothèque standard Deno en utilisant l'URL
> Un itérateur qui retourne un flux de requêtes
```
// Imports `serve` from the remote Deno standard library, using URL.
import { serve } from "https://deno.land/std@v0.21.0/http/server.ts";

// `serve` function returns an asynchronous iterator, yielding a stream of requests
for await (const req of serve({ port: 8000 })) {
    req.respond({ body: "Hello World\n" });
}
```
When running this program, Deno would automatically download and cache the remote standard library files and compile the code. Similarly, we can run a standard library script (such as a file server) directly without explicitly downloading, by providing the URL as input filename (-A turns on all permissions):
> Deno va automatiquement télécharger et mettre en cache les fichiers distants, puis compiler le code
> En passant une URL en tant que chemin du script à exécuter, -A donne toutes les permissions
```
$ deno run -A https://deno.land/std/http/file_server.ts
Download https://deno.land/std/http/file_server.ts
Compile https://deno.land/std/http/file_server.ts
...
HTTP server listening on http://0.0.0.0:4500/
```




### Do I have to import it by the URL all the time?
```
export { test, assertEquals } from "https://deno.land/std/testing/mod.ts";
```
```
{
   "imports": {
      "http/": "https://deno.land/std/http/"
   }
}
```


---
[![img](https://www.developpez.net/forums/attachments/p387662d1/a/a/a)](https://www.developpez.com/actu/208629/JSConf-Berlin-2018-moins-Ryan-Dahl-liste-10-erreurs-de-conception-sur-Node-js-et-devoile-son-prototype-deno/)

---


https://dev.to/lsagetlethias/deno-first-approach-4d0

https://dev.to/search?q=deno
























