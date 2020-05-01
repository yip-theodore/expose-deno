
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

Ce script implémente un serveur HTTP basique:
```javascript
import { serve } from "https://deno.land/std@v0.36.0/http/server.ts";
const s = serve({ port: 8000 });
console.log("http://localhost:8000/");
for await (const req of s) {
  req.respond({ body: "Hello World\n" });
}
```
Le script nécessite la permission explicite d'accéder au réseau:
```bash
$ deno example.ts
error: Uncaught PermissionDenied: network access to "0.0.0.0:8000", run again with the --allow-net flag
```
On peut le lui préciser en ligne de commande:
```bash
$ deno --allow-net example.ts
```

Exemple d'implémentation du programme Unix `cat` pour afficher le contenu d'un fichier:
```javascript
for (let i = 0; i < Deno.args.length; i++) {
  let filename = Deno.args[i];
  let file = await Deno.open(filename);
  await Deno.copy(file, Deno.stdout);
  file.close();
}
```
On peut également lui passer directement l'URL du script:
```bash
$ deno --allow-read https://deno.land/std/examples/cat.ts README.md
```
Deno va aller télécharger les fichiers distants, mettre en cache, compiler le code, pour ensuite l'éxécuter.



### Sources
https://deno.land/std/manual.md  
https://www.youtube.com/watch?v=M3BM9TB-8yA  
https://www.jesuisundev.com/deno-le-nouveau-nodejs  
https://dev.to/lsagetlethias/deno-first-approach-4d0  
https://en.wikipedia.org/wiki/Deno_(software)  
https://www.developpez.com/actu/208629/JSConf-Berlin-2018-moins-Ryan-Dahl-liste-10-erreurs-de-conception-sur-Node-js-et-devoile-son-prototype-deno/  


