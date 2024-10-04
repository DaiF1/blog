---
title: Monter un site web simple
summary: |
    Comment monter un site web tout simple à l'aide de ViteJS et GitHub Pages
date: 2024-09-27
tags: ["web", "tutoriel"]
authors:
    - "daif.fr"
---

Un ami m'a récemment demandé comment je créais mes différents sites web.
Donc voici un tutoriel sur le sujet.

## Créer un site web à l'aide de ViteJS

### Préparation

Pour réaliser ce site, on va utiliser *ViteJS*. C'est un framework utilisé pour
faire des sites webs rapidement.

On peut créer un nouveau projet *Vite* à l'aide de la commande suivante :

```bash
npm create vite@latest
```

Après exécution, le programme demande le nom du projet, et crée un dossier pour.
Il demande également le langage et le framework utilisé pour le projet.
Pour ce tutoriel, j'utilise **Javascript Vanilla**.

Une fois terminé, on peut entrer dans le dossier et lancer la commande
`npm install`.

Pour deployer le site en *localhost*, il suffit d'écrire `npm run dev`.
Cela devrait afficher une page avec un simple compteur de
clics.

### Analyse du dossier

Maintenant qu'on a un site fonctionnel, c'est le moment de regarder le contenu
de notre tout nouveau dossier.

Le plus petit site que l'on peut faire suit cette architecture :

```
projet/
├── index.html
├── main.js
├── package.json
├── public/
│   └── Ici se trouvent toutes les images du site
└── style.css
```

Le fichier `index.html` contient le contenu principal de la page et `style.css`
tout son style.
Le point d'entrée du site, c'est le fichier `main.js`. C'est le premier js
appelé au chargement de la page.

### Bonjour tout le monde !

Essayons de modifier notre site un petit peu. Ajoutons le code suivant dans la
balise `div id="app"`.

```html
<h1>Bonjour tout le monde !</h1>
<p>Ceci est mon premier site web</p>

<button id="clic-btn">Clique ici !</button>
```

On va essayer d'afficher "Bien joué !" sur la page lorsqu'on clique sur le
bouton "Clique ici !".

Voici le code Javascript de `main.js` qui permet de faire ça :

```js
import './style.css'

let bouton = document.getElementById("clic-btn");
let app = document.getElementById("app");

bouton.addEventListener("click", () => {
    app.innerHTML += "<p>Bien joué !</p>"
})
```

{{< alert "circle-info" >}}
La première ligne dans `main.js` sert à importer le CSS associé au script.
Il est importé de cette façon car *ViteJS* est souvent utilisé pour des sites
fonctionnant avec des composants (comme le permet *React* ou *Vue*), où chaque
script correspond à un objet de la page.
{{< /alert >}}

Maintenant, en lançant `npm run dev`, le bouton devrait ajouter le texte
"Bien joué !" en bas de page après un clic.

## Le déployer sur GitHub Pages

### Créer son repo GitHub

La première étape pour déployer notre site est de créer un repo GitHub.

Il faut donc se rendre sur son compte GitHub et en créer un nouveau. La
visibilité de ce repo doit être "Publique".

Une fois le repo créé, on peut récupérer son URL à l'aide du bouton `Code`
en vert.

Une fois l'URL copié, voici les commandes à taper pour pousser notre site sur
le repo :

```
git remote add origin <url-du-repo>
git branch -M main
git pull origin main
git push -u origin main
```

{{< alert "circle-info" >}}
Lors de la création d'un repo *vraiment* vide (pas de `README`,
pas de `.gitignore`, rien), ces commandes sont listées en bas de la page.
{{< /alert >}}

### Installer `gh-pages`

Pour déployer notre site, on va utiliser un paquet très utile appelé `gh-pages`.
C'est un outil qui déploie automatiquement notre site web sur une branche pour
GitHub Pages.

On peut l'installer sur le projet avec la commande suivante :

```bash
npm install gh-pages --save-dev
```

Une fois l'installation terminée, il faut ajouter les 2 règles suivantes dans
la partie `script` du fichier `package.json` :

```js
"predeploy": "npm run build",
"deploy": "gh-pages -d dist",
```

La ligne décrivant la "homepage" doit aussi être ajoutée au dessus de la section
`"script"` (en remplaçant `<username>` avec le pseudo GitHub).
```js
"homepage": "https://<username>.github.io/"
```

### Il est live !

Maintenant, il est possible de taper `npm run deploy` et le script va lui même
créer une branche appelée `gh-pages`, avec toutes les pages du site.

Pour enfin tout terminer, il suffit d'aller dans les paramètres du repo, et dans
la section "Pages"
Voici à quoi doit ressembler la partie "Build and deployment" :

{{< figure src="./pages-settings.png" >}}

Une fois que la pipeline est terminée, le site devrait être live sur l'url
`https://<username>.github.io/`.

{{< alert "circle-info" >}}

À chaque fois qu'il est nécessaire de déployer une nouvelle version du site,
il faut simplement relancer `npm run deploy` dans un terminal.

{{< /alert >}}
