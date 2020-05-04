# Deno

<img align="left" width="120" src="https://github.com/denoland/deno_website2/blob/master/public/images/deno_logo_3.svg">

Deno est un environnement d'exécution JavaScript et TypeScript construit sur le moteur JavaScript V8 et le language de programmation Rust.  

Deno a été crée par Ryan Dahl, l'auteur de Node.js, et a été dévoilé pendant la JSConf EU 2018 lors de sa présentation 
["10 choses que je regrette à propos de Node.js"](https://www.youtube.com/watch?v=M3BM9TB-8yA).  

La sortie de la version 1.0 est prévu pour le 13 mai 2020.


## Erreurs de conception de Node
- L'abandon des promesses vers le début du projet
- Un manque de sécurité
- La dépendance au système du compilation GYP
- L'intégration du `package.json`, et du gestionnaire de paquets centralisé NPM
- La redondance des dossiers `node_modules`
- La complexité de l'algorithme de résolution de modules


## Différences avec Node
- Deno a été entièrement réecrit en Rust
- Un exécutable minimal, avec une bibliothèque de modules standards
- Un support natif de TypeScript
- L'importation des dépendances via URL, et un cache partagé
- La main sur les permissions
- L'intégration d'utilitaires de développement


## Exemples

[Guide d'installation officiel](https://deno.land)

Voici un exemple d'implémentation d'un serveur HTTP basique:
```javascript
/* exemple.ts */
import { serve } from "https://deno.land/std@v0.36.0/http/server.ts";
const s = serve({ port: 8000 });
console.log("http://localhost:8000/");
for await (const req of s) {
  req.respond({ body: "Hello World\n" });
}
```
Sans une autorisation explicite, le script n'a aucun accès au réseau:
```bash
$ deno example.ts
error: Uncaught PermissionDenied: network access to "0.0.0.0:8000", run again with the --allow-net flag
```
On peut le lui donner à travers la ligne de commande:
```bash
$ deno --allow-net example.ts
```

Un autre exemple d'implémentation du programme Unix `cat`, pour afficher le contenu d'un fichier:
```javascript
for (let i = 0; i < Deno.args.length; i++) {
  let filename = Deno.args[i];
  let file = await Deno.open(filename);
  await Deno.copy(file, Deno.stdout);
  file.close();
}
```
On peut également lui passer une URL comme chemin du script à éxécuter:
```bash
$ deno --allow-read https://deno.land/std/examples/cat.ts README.md
```


## Sources
https://deno.land/std/manual.md  
https://www.jesuisundev.com/deno-le-nouveau-nodejs  
https://www.lemondeinformatique.fr/actualites/lire-deno-le-runtime-javascript-et-typescript-palliant-aux-lacunes-de-nodejs-78304.html  
https://www.developpez.com/actu/208629/JSConf-Berlin-2018-moins-Ryan-Dahl-liste-10-erreurs-de-conception-sur-Node-js-et-devoile-son-prototype-deno/  
https://en.wikipedia.org/wiki/Deno_(software)  
https://dev.to/lsagetlethias/deno-first-approach-4d0  

