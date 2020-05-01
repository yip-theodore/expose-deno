
# Deno

<img align="left" width="120" src="https://github.com/denoland/deno_website2/blob/master/public/images/deno_logo_3.svg">

Deno est un environnement d'exécution JavaScript et TypeScript construit sur le moteur JavaScript V8 et le language de programmation Rust.  

Deno a été crée par Ryan Dahl, l'auteur de Node.js en 2009, et a été dévoilé lors de la JSConf EU de 2018 dans sa présentation 
["10 choses que je regrette à propos de Node.js"](https://www.youtube.com/watch?v=M3BM9TB-8yA).  

La sortie de la version 1.0 est prévu au 13 mai 2020.


## Problèmes de Node
- Avoir abandonné le concept des `Promises` lors de la conception pour les callbacks
- L'utilisation de GYP, un système de compilation abandonné
- Le dossier node_modules, et npm en tant que distribution centralisée intégrée dans les distributions de Node
- Les extensions `.js` pouvant être omises, plus la complexité de l'algorithme de résolution de modules
- N'exploite pas pleinement l'environnement sécurisé de V8 en offrant l'accès complet au disque et au réseau
- Les erreurs non capturées n'arrêtent pas l'éxécution


## Principales différences avec Node
- Un exécutable unique `deno` (15 Mo), qui dispose d'une large bibliothèque de modules standards `deno/std`
- Utilise le système de module ES Module là où Node utilise CommonJS
- Les dépendances peuvent être chargées aux travers d'URLs, l'utilisation d'un gestionnaire de paquets (npm) et du `package.json` ne sont donc plus nécessaires
- Les modules sont mis en cache dans un espace commun aux différents projets, et peuvent être rafraîchies
- Compatible avec le navigateur en supportant toutes les APIs Web
- Supporte TypeScript par défaut
- Permet un contrôl précis des permissions comme l'accès au système de fichiers et au réseau
- Écrit en Rust (après une première version en Go) alors que Node est en C et C++, plus le système de boucle d’évènements interne utilisé est celle de Rust: Tokio, au lieu de libuv pour Node
- Des sous-commandes utilitaires intégrées, comme un inspecteur de dépendances `deno info` ou un debugger `--debug`


## Installation
[Voir ici](https://deno.land)

## Exemples

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





### Sources
https://deno.land/std/manual.md  
https://www.youtube.com/watch?v=M3BM9TB-8yA  
https://www.jesuisundev.com/deno-le-nouveau-nodejs  
https://dev.to/lsagetlethias/deno-first-approach-4d0  
https://en.wikipedia.org/wiki/Deno_(software)  
https://www.developpez.com/actu/208629/JSConf-Berlin-2018-moins-Ryan-Dahl-liste-10-erreurs-de-conception-sur-Node-js-et-devoile-son-prototype-deno/  



















