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








































