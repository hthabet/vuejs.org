---
title: Installation
type: guide
order: 0
vue_version: 1.0.20
dev_size: "258.56"
min_size: "72.92"
gz_size: "25.10"
---

### Compatibilité

Vue.js **ne supporte pas** IE8 et les versions antérieures, parce que Vue.js utilise les fonctionnalités ECMAScript 5 qui ne sont pas "shimmables" dans IE8. Cela dit, Vue.js supporte tous les [navigateurs compatibles ECMAScript 5](http://caniuse.com/#feat=es5).

### Note de version

Les notes de version détaillées pour chaque version sont disponibles sur [GitHub](https://github.com/vuejs/vue/releases).

## Autonome (Standalone)

Il suffit de télécharger et d'inclure une balise `<script>`. `Vue` sera déclaré comme une variable globale. **Conseil pro: n'utilisez pas la version minifiée pendant le développement car vous ne bénéficierez pas des avertissements pour les erreurs courantes.**

<div id="downloads">
<a class="button" href="/js/vue.js" download>Version de dév</a><span class="light info">Avec tous les avertissements et le mode de débogage</span>

<a class="button" href="/js/vue.min.js" download>Version de production</a><span class="light info">Avertissements retirés, {{gz_size}}ko min+gzip</span>
</div>

### CDN

Disponible sur [jsdelivr](//cdn.jsdelivr.net/vue/{{vue_version}}/vue.min.js) or [cdnjs](//cdnjs.cloudflare.com/ajax/libs/vue/{{vue_version}}/vue.min.js) (La synchronisation de la dernière version peut prendre du temps et cette dernière peut ne pas être encore disponible).

### Build conforme à CSP 

Certains environnements, tels que les Applications de Google Chrome, font respecter la politique de sécurité du contenu (Content Security Policy - CSP) et ne permettent pas l'utilisation de `new function()` pour évaluer les expressions. Dans ce cas, vous pouvez utiliser le [build conforme à CSP] (https://github.com/vuejs/vue/tree/csp/dist).

## NPM

NPM est la méthode d'installation recommandée lors du développement de larges applications à grande échelle avec Vue.js. Il s'associe bien avec les modules d'empaquetage CommonJS tels que [Webpack](http://webpack.github.io/) ou [Browserify](http://browserify.org/). Vue.js fournit également des outils d'accompagnement pour la rédaction [Composants à fichier unique](application.html#Single-File-Components).

``` bash
# dernière version stable
$ npm install vue
# dernière version stable + CSP-conforme
$ npm install vue@csp
```

## CLI

Vue.js fournit une [Interface en Ligne de Commande officielle] (https://github.com/vuejs/vue-cli) pour mettre en place rapidement la base d'une application SPA (Singe Page Application). Vue.js fournit également une série de configurations pour un workflow de frontend moderne. Ça ne prend que quelques minutes démarrer le développement incluant: rechargement et lint du code lors de la sauvegarde et la génération d'un build prêt à la production:

``` bash
# installer vue-cli
$ npm install -g vue-cli
# créer un nouveau projet avec le kit de démarrage "webpack"
$ vue init webpack my-project
# installer des dependances et c'est parti!
$ cd my-project
$ npm install
$ npm run dev
```

## Build de développement 

**Important**: le paquet CommonJS distribué sur NPM (`vue.common.js`) est uniquement vérifié par rapport aux versions sur la branche `master`, de sorte que le fichier dans la branche `dev` soit le même que celui de la version stable. Pour utiliser la dernière version du code source de Vue.js sur GitHub, vous devrez le construire vous-même!

``` bash
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```

## Bower

``` bash
# dernière version stable
$ bower install vue
```

## Chargeurs de module AMD

Les téléchargements autonomes (standalones) ou les versions installées via Bower sont enveloppées avec UMD afin qu'ils puissent être utilisés directement comme un module AMD.
