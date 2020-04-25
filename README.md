version 1.0 - 13 mai 2020 (2 ans)


Typescript: [x]

Modules standards (deno/std)

### Exécutable unique
deno (15 Mo) + URL
both runtime and package manager (?)

### Sécurité
pas accès aux [système de fichier, environnement, réseau] sauf autorisation explicite

### Compatibilité avec le navigateur


## Différences avec Node.js
- n'utilise pas npm
- ni le package.json
- utilisation systématique des _promesses_
- gestion des erreurs (die)
- syntaxe import au lieu de require (ES modules) + url HTTP
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

## Deno c'est quoi?
[V8, Rust, et Tokio]
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








































