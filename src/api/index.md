---
type: api
---

## Configuration globale

`Vue.config` est un objet contenant les configurations globales de Vue. Vous pouvez modifier ses propriétés citées ci-dessous avant le lancement de votre application:

### debug

- **Type:** `Booléen`

- **Par défaut:** `false`

- **Usage:**

  ``` js
  Vue.config.debug = true
  ```

  En mode débogage, Vue:

  1. affichera les traces d'appels pour tous les avertissements.

  2. Fera que tous les nœuds d'ancrage seront visibles dans le DOM sous forme de commentaire. Cela rend plus facile l'inspection de la structure du résultat rendu.

  <p class="tip">Le mode débogage est disponible uniquement dans le build de développement.</p>

### delimiters

- **Type:** `Tableau<Chaine>`

- **Par défaut:** `{% raw %}["{{", "}}"]{% endraw %}`

- **Usage:**

  ``` js
  // modèle ES6 de chaine
  Vue.config.delimiters = ['${', '}']
  ```

  Changer les délimiteurs d'interpolation du texte.

### unsafeDelimiters

- **Type:** `Tableau<Chaine>`

- **Par défaut:** `{% raw %}["{{{", "}}}"]{% endraw %}`

- **Usage:**

  ``` js
  // le faire paraître plus dangereux
  Vue.config.unsafeDelimiters = ['{!!', '!!}']
  ```

  Changer les délimiteurs d'interpolation du HTML brut.

### silent

- **Type:** `Booléen`

- **Par défaut:** `false`

- **Usage:**

  ``` js
  Vue.config.silent = true
  ```

  Supprimer tous les journaux et les avertissements de Vue.js.

### async

- **Type:** `Booléen`

- **Par défaut:** `true`

- **Usage:**

  ``` js
  Vue.config.async = false
  ```

  Lorsque le mode async n'est pas actif, Vue effectuera toutes les mises à jour du DOM de manière synchrone dès la détection de changement de données. Cela peut faciliter le débogage dans certains scénarios, mais pourrait également provoquer une dégradation des performances et affecter l'ordre dans lequel les fonctions de retour d'observation sont appelées. **`async: false` n'est pas recommandé en production.**

### devtools

- **Type:** `Booléen`

- **Par défaut:** `true` (`false` dans le build de production)

- **Usage:**

  ``` js
  // assurez-vous de définir ceci de manière synchrone immédiatement après le chargement de Vue
  Vue.config.devtools = true
  ```

  Permettre d'autoriser ou non l'inspection avec [vue-devtools](https://github.com/vuejs/vue-devtools). La valeur par défaut de cette option est `true` dans le build de développement et `false` dans le build de production. Cela dit, vous avez la possibilité de le mettre à `true` pour permettre l'inspection en production.

## API globale

<h3 id="Vue-extend">Vue.extend( options )</h3>

- **Arguments:**
  - `{Objet} options`

- **Usage:**

  Créer une "sous-classe" du constructeur de base de Vue. L'argument doit être un objet contenant des options du composant.

  Les cas particuliers à noter ici sont les propriétés `el` et `data` - Elles doivent être des fonctions lorsqu'elles sont utilisées avec `Vue.extend()`.

  ``` html
  <div id="mount-point"></div>
  ```

  ``` js
  // créer un contructeur réutilisable
  var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>'
  })
  // créer une instance de profil
  var profile = new Profile({
    data: {
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  })
  // attacher l'instance à un élement
  profile.$mount('#mount-point')
  ```

  Le résultat sera:

  ``` html
  <p>Walter White aka Heisenberg</p>
  ```

- **Voir aussi:** [Composants](/guide/components.html)

<h3 id="Vue-nextTick">Vue.nextTick( rappel )</h3>

- **Arguments:**
  - `{Fonction} rappel`

- **Usage:**

  Différer le rappel à exécuter après le prochain cycle de mise à jour du DOM. Utilisez-le immédiatement après que vous ayez changé certaines données pour attendre la mise à jour du DOM.

  ``` js
  // modifier la donnée
  vm.msg = 'Hello'
  // DOM pas encore mis à jour
  Vue.nextTick(function () {
    // DOM mis à jour
  })
  ```

- **Voir aussi:** [File d'attente de mise à jour asynchrone](/guide/reactivity.html#Async-Update-Queue)

<h3 id="Vue-set">Vue.set( objet, clé, valeur )</h3>

- **Arguments:**
  - `{Objet} objet`
  - `{Chaine} clé`
  - `{*} valeur`

- **Retourne:** La valeur définie.

- **Usage:**

  Définir une propriété sur un objet. Si l'objet est réactif, assurer que la propriété soit créée comme une propriété réactive et déclenche des mises à jour de vue/interface. Ceci est principalement utilisé pour contourner la limitation de Vue, ce dernier ne détectant pas les ajouts de propriété.

- **Voir aussi:** [Le concept de réactivité](/guide/reactivity.html)

<h3 id="Vue-delete">Vue.delete( objet, clé )</h3>

- **Arguments:**
  - `{Objet} objet`
  - `{Chaine} clé`

- **Usage:**

  Supprimer une propriété sur un objet. Si l'objet est réactif, assurer que la suppression déclenche des mises à jour de vue/interface. Ceci est principalement utilisé pour contourner la limitation de Vue, ce dernier ne détectant pas les suppressions de propriété, mais vous devriez rarement avoir besoin de l'utiliser.

- **Voir aussi:** [Le concept de réactivité](/guide/reactivity.html)

<h3 id="Vue-directive">Vue.directive( id, [definition] )</h3>

- **Arguments:**
  - `{Chaine} id`
  - `{Fonction | Objet} [definition]`

- **Usage:**

  Déclarer ou récupérer une directive globable

  ``` js
  // déclaration
  Vue.directive('my-directive', {
    bind: function () {},
    update: function () {},
    unbind: function () {}
  })

  // déclaration (une fonction simple de directive)
  Vue.directive('my-directive', function () {
    // this will be called as `update`
  })

  // récupération, retourne la définition de la directive si celle ci est préalablement déclarée
  var myDirective = Vue.directive('my-directive')
  ```

- **Voir aussi:** [Directives personnalisées](/guide/custom-directive.html)

<h3 id="Vue-elementDirective">Vue.elementDirective( id, [definition] )</h3>

- **Arguments:**
  - `{Chaine} id`
  - `{Objet} [definition]`

- **Usage:**

  Déclarer or récupérer une directive d'élément global.

  ``` js
  // déclaration
  Vue.elementDirective('my-element', {
    bind: function () {},
    // les directives d'élément ne possède pas de `update`
    unbind: function () {}
  })

  // récupération, retourne la définition de la directive si celle ci est préalablement déclarée
  var myDirective = Vue.elementDirective('my-element')
  ```

- **Voir aussi:** [Directives d'Élément](/guide/custom-directive.html#Element-Directives)

<h3 id="Vue-filter">Vue.filter( id, [definition] )</h3>

- **Arguments:**
  - `{Chaine} id`
  - `{Fonction | Objet} [definition]`

- **Usage:**

  Déclarer or récupérer un filtre global.

  ``` js
  // déclaration
  Vue.filter('my-filter', function (value) {
    // retour la valeur traitée
  })

  // filtre en lecture/écriture (two-way)
  Vue.filter('my-filter', {
    read: function () {},
    write: function () {}
  })

  // récupération, retourne le filtre si celui ci est préalablement déclaré
  var myFilter = Vue.filter('my-filter')
  ```

- **Voir aussi:** [Filtres personnalisés](/guide/custom-filter.html)

<h3 id="Vue-component">Vue.component( id, [definition] )</h3>

- **Arguments:**
  - `{Chaine} id`
  - `{Fonction | Objet} [definition]`

- **Usage:**

  Déclarer or récupérer un composant global.

  ``` js
  // déclarer un constructeur étendu
  Vue.component('my-component', Vue.extend({ /* ... */ }))

  // déclarer un objet d'options (appelle automatiquement Vue.extend)
  Vue.component('my-component', { /* ... */ })

  // récupérer un composant déclaré (retourne toujours le constructeur)
  var MyComponent = Vue.component('my-component')
  ```

- **Voir aussi:** [Composants](/guide/components.html).

<h3 id="Vue-transition">Vue.transition( id, [hooks] )</h3>

- **Arguments:**
  - `{Chaine} id`
  - `{Objet} [hooks]`

- **Usage:**

  Déclarer or récupérer un objet de hooks de transition globale.

  ``` js
  // déclaration
  Vue.transition('fade', {
    enter: function () {},
    leave: function () {}
  })

  // récupération de hooks déclarés
  var fadeTransition = Vue.transition('fade')
  ```

- **Voir aussi:** [Transitions](/guide/transitions.html).

<h3 id="Vue-partial">Vue.partial( id, [partial] )</h3>

- **Arguments:**
  - `{Chaine} id`
  - `{Chaine} [partial]`

- **Usage:**

  Déclarer or récupérer un modèle global de chaine partielle.

  ``` js
  // déclaration
  Vue.partial('my-partial', '<div>Hi</div>')

  // récupération du fragment de chaine déclaré
  var myPartial = Vue.partial('my-partial')
  ```

- **Voir aussi:** [Elements spéciaux - &lt;partial&gt;](#partial).

<h3 id="Vue-use">Vue.use( plugin, [options] )</h3>

- **Arguments:**
  - `{Objet | Fonction} plugin`
  - `{Objet} [options]`

- **Usage:**

  Installer un plugin Vue.js. Si le plugin est un objet, il doit avoir une méthode `install`. Si il est lui-même une fonction, il sera traité comme la méthode `install. La méthode `install` sera appelée avec Vue comme argument.

- **Voir aussi:** [Plugins](/guide/plugins.html).

<h3 id="Vue-mixin">Vue.mixin( mixin )</h3>

- **Arguments:**
  - `{Objet} mixin`

- **Usage:**

  Appliquer une mixin globalement. Cela affecte toutes les instances Vue précédemment créées. Cela peut être utilisé par les auteurs du plugin pour injecter un comportement personnalisé dans les composants. **Pas recommandé dans le code d'application**.

- **Voir aussi:** [Mixins Globales](/guide/mixins.html#Global-Mixin)

## Options / Data

### data

- **Type:** `Objet | Fonction`

- **Restriction:** accepte seulement `Fonction` lorsqu'il est utilisé dans une définition de composant.

- **Détails:**

  C'est l'objet de données pour l'instance de Vue. Vue.js va récursivement convertir ses propriétés en "getter" / "setters" pour le rendre «réactif». **L'objet doit être simple**: objets natifs, getter / setters existants et les propriétés de prototype sont ignorés. Il est recommandé de ne pas observer des objets complexes.

  Une fois que l'instance est créée, l'objet de données d'origine est accessible avec `vm.$data`. L'instance de Vue permet également l'accès direct à toutes les propriétés figurant sur l'objet de données.

  Les propriétés prefixées par `_` ou `$` **ne seront pas** accessibles directement depuis l'instance de Vue parce qu'ils peuvent entrer en conflit avec les propriétés internes de Vue et les méthodes de l'API. Vous devez y accéder avec `vm.$data._property`.

  Lors de la définition d'un **composant**, `data` doit être déclarée comme une fonction qui renvoie l'objet initial de données, car il y aura de nombreuses instances créées en utilisant la même définition. Si nous utilisons encore un objet clair pour `data`, ce même objet sera **partagé par référence** avec toutes les instances créées! En fournissant `data` comme une fonction, chaque fois qu'une nouvelle instance est créée, nous pouvons simplement appeler cette fonction pour retourner une nouvelle copie des données initiales.

  Si nécessaire, un clonage en profondeur de l'objet original peut être obtenu en appliquant `JSON.parse(JSON.stringify(...))` sur `vm.$data`.

- **Exemple:**

  ``` js
  var data = { a: 1 }

  // création directe d'instance
  var vm = new Vue({
    data: data
  })
  vm.a // -> 1
  vm.$data === data // -> true

  // on doit utiliser function à l'intérieur de Vue.extend()
  var Component = Vue.extend({
    data: function () {
      return { a: 1 }
    }
  })
  ```

- **Voir aussi:** [Le concept de réactivité](/guide/reactivity.html).

### props

- **Type:** `Tableau | Objet`

- **Détails:**

  C'est une liste d'attributs qui recevoivent des données provenant du composant parent. Cette propriété a une syntaxe simple basée sur un tableau et une syntaxe alternative basée sur un objet qui permet des configurations avancées telles que la vérification de type, validation personnalisée et les valeurs par défaut.

- **Exemple:**

  ``` js
  // syntaxe simple en tableau
  Vue.component('props-demo-simple', {
    props: ['size', 'myMessage']
  })

  // syntaxe en objet avec validation
  Vue.component('props-demo-advanced', {
    props: {
      // vérification du type
      size: Number,
      // vérification du type + d'autres validations
      name: {
        type: String,
        required: true
      }
    }
  })
  ```

- **Voir aussi:** [Propriétés](/guide/components.html#Props)

### computed

- **Type:** `Objet`

- **Détails:**

  C'est les propriétés calculées disponibles dans l'instance de Vue. Elles peuvent être lues (getters) ou définies (setters) en utilisant `this`, étant le contexte lié automatiquement à l'instance de Vue.

- **Exemple:**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    computed: {
      // lecture seulement, on a besoin uniquement d'un fonction
      aDouble: function () {
        return this.a * 2
      },
      // dans le cas de lecture et ecriture (getter et setter)
      aPlus: {
        get: function () {
          return this.a + 1
        },
        set: function (v) {
          this.a = v - 1
        }
      }
    }
  })
  vm.aPlus   // -> 2
  vm.aPlus = 3
  vm.a       // -> 2
  vm.aDouble // -> 4
  ```

- **Voir aussi:**
  - [Propriétés calculées](/guide/computed.html)
  - [Le concept de réactivité: à l'intérieur des propriétés calculées](/guide/reactivity.html#Inside-Computed-Properties)

### methods

- **Type:** `Objet`

- **Détails:**

  C'est les méthodes disponibles dans l'instance de Vue. Vous pouvez accéder à ces méthodes directement depuis l'instance de Vue, ou les utiliser dans les expressions des directives. Toutes les méthodes ont leur context `this` automatiquement lié à l'instance de Vue.

- **Exemple:**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    methods: {
      plus: function () {
        this.a++
      }
    }
  })
  vm.plus()
  vm.a // 2
  ```

- **Voir aussi:** [Méthodes et gestion des événements](/guide/events.html)

### watch

- **Type:** `Objet`

- **Détails:**

  C'est un objet où les clés sont des expressions à surveiller et les valeurs sont les fonctions de rappel correspondantes. La valeur peut aussi être une chaîne d'un nom de méthode, ou un objet qui contient des options supplémentaires. À l'instanciation, l'instance de Vue appellera `$watch()` pour chaque entrée dans l'objet.

- **Exemple:**

  ``` js
  var vm = new Vue({
    data: {
      a: 1
    },
    watch: {
      'a': function (val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // une chaîne d'un nom de méthode
      'b': 'someMethod',
      // deep watcher
      'c': {
        handler: function (val, oldVal) { /* ... */ },
        deep: true
      }
    }
  })
  vm.a = 2 // -> new: 2, old: 1
  ```

- **Voir aussi:** [Méthodes d'instance - vm.$watch](#vm-watch)

## Options / DOM

### el

- **Type:** `Chaine | ÉlémentHTML | Fonction`

- **Restriction:** accepte uniquement le type `Fonction` lorsque c'est utilisé dans une définition de composant.

- **Détails:**

  Fournit à l'instance de Vue un élément DOM existant auquel elle sera rattachée. Il peut être une chaîne de sélecteur CSS, un ÉlémentHTML, ou une fonction qui retourne un ÉlémentHTML. Notez que l'élément fourni ne sert que de point de rattachement; il sera remplacé si un `template` est également fourni, à moins que `replace` est définie sur `false`. L'élément résolu sera accessible comme `vm.$el`.

  Lorsque `el` est utilisé dans `Vue.extend`, une fonction doit être fournie de sorte que chaque instance obtienne un élément créé séparément.

  Si cette option est disponible à l'instanciation, l'instance va immédiatement entrer dans la compilation; sinon, l'utilisateur devra appeler explicitement `vm.$mount()` pour déclencher la compilation.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### template

- **Type:** `Chaine`

- **Détails:**

  C'est un modèle HTML a utilisé comme la vue de l'instance de Vue. Par défaut, le modèle **remplacera** l'élément rattaché. Lorsque l'option `replace` est définie à `false`, le modèle sera inséré dans l'élément rattaché. Dans les deux cas, aucun balisage existant à l'intérieur de l'élément rattaché sera ignoré, à moins que des fentes de distribution de contenu soient présentes dans le modèle.

  Si la chaîne commence par `#`, ça sera utilisé comme un querySelector et utilisera innerHTML de l'élément sélectionné comme chaîne de modèle. Cela permet l'utilisation de `<script type="x-template">` pour inclure des modèles.

  Notez que, dans certaines situations, comme lorsque le modèle contient plus d'un élément de niveau supérieur, ou ne contient que du texte brut, l'instance deviendra un fragment d'instance (gère une liste de nœuds plutôt que d'un seul nœud). Dans ce cas, les directives sans contrôle de flux sur le point de rattachement pour les fragments d'instance sont ignorées.

- **Voir aussi:**
  - [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)
  - [Distribution du contenu](/guide/components.html#Content-Distribution-with-Slots)
  - [Fragement d'instance](/guide/components.html#Fragment-Instance)

### replace

- **Type:** `Booléen`

- **Par défaut:** `true`

- **Restriction:** appliqué seulement si l'option **template** est également présente.

- **Détails:**

  Détermine si oui ou non, pour remplacer l'élément rattaché. Si la valeur est `false`, le modèle va écraser le contenu à l'intérieur de l'élément sans remplacer l'élément lui-même.

- **Exemple**:

  ``` html
  <div id="replace"></div>
  ```

  ``` js
  new Vue({
    el: '#replace',
    template: '<p>remplacé</p>'
  })
  ```

  Will result in:

  ``` html
  <p>remplacé</p>
  ```

  Par contre, lorsque `replace` est définie à `false`:

  ``` html
  <div id="insert"></div>
  ```

  ``` js
  new Vue({
    el: '#insert',
    replace: false,
    template: '<p>inséré</p>'
  })
  ```

  Le résultat est:

  ``` html
  <div id="insert">
    <p>inséré</p>
  </div>
  ```

## Options / Cycle de vie des Hooks

### init

- **Type:** `Fonction`

- **Détails:**

  Appelée de manière synchrone après l'initialisation de l'instance mais avant l'observation des données et configuration des événements/observateurs.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### created

- **Type:** `Fonction`

- **Détails:**

  Appelée de manière synchrone après la création de l'instance. A ce stade, l'instance a fini de traiter les options, ce qui signifie ce qui suit a été mis en place: l'observation des données, les propriétés calculées, les méthodes, les fonctions de rappel d'observation et d'événement. Cependant, la compilation du DOM ne serait pas encore démarrée et la propriété `$el` ne sera pas disponible pour l'instant.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### beforeCompile

- **Type:** `Fonction`

- **Détails:**

  Appelée juste avant que la compilation ne démarre.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### compiled

- **Type:** `Fonction`

- **Détails:**

  Appelée après que la compilation ne soit terminée. A ce stade, toutes les directives ont été liées afin que les modifications de données puissent déclencher des mises à jour du DOM. Cependant, Il n'est pas garanti que `$el`ne soit inséré dans le document.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### ready

- **Type:** `Fonction`

- **Détails:**

  Appelée après la compilation **et** après que `$el` est **inséré dans le document pour la première fois**, à savoir juste après le premier hook `attached`. Notez que cette insertion doit être exécutée via Vue (avec des méthodes comme `vm.$AppendTo()` ou à la suite d'une mise à jour de la directive) pour déclencher le hook `ready`.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### attached

- **Type:** `Fonction`

- **Détails:**

  Appelée lorsque `vm.$el` est attaché au DOM par une directive ou par une méthode d'instance VM comme `$appendTo()`. La manipulation directe de `vm.$el`  **ne déclenchera pas** ce hook.

### detached

- **Type:** `Fonction`

- **Détails:**

  Appelée lorsque `vm.$el` est retiré du DOM par une directive ou d'une méthode d'instance VM. La manipulation directe de `vm.$el` **ne déclenchera pas** ce hook.

### beforeDestroy

- **Type:** `Fonction`

- **Détails:**

  Appelée juste avant qu'une instance de Vue est détruite. A ce stade, l'instance est encore pleinement fonctionnelle.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

### destroyed

- **Type:** `Fonction`

- **Détails:**

  Appelée après qu'une instance de Vue a été détruite. Lorsque ce hook est exécuté, toutes les liaisons et les directives de l'instance de Vue sont dissociées et toutes les instances de Vue de l'enfant sont également détruites.

  Notez que s'il y a une transition en cours (transition de sortie), le hook `destroyed` est exécuté **après** que la transition ne soit terminée.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

## Options / Assets

### directives

- **Type:** `Objet`

- **Détails:**

  Une liste de directives disponibles pour l'instance de Vue.

- **Voir aussi:**
  - [Directives personnalisées](/guide/custom-directive.html)
  - [Convention de nommage des assets](/guide/components.html#Assets-Naming-Convention)

### elementDirectives

- **Type:** `Objet`

- **Détails:**

  Une liste de directives d'élement disponibles pour l'instance de Vue.

- **Voir aussi:**
  - [Directives d'Élément](/guide/custom-directive.html#Element-Directives)
  - [Convention de nommage des assets](/guide/components.html#Assets-Naming-Convention)

### filters

- **Type:** `Objet`

- **Détails:**

  Une liste de filtres disponibles pour l'instance de Vue.

- **Voir aussi:**
  - [Filtres personnalisés](/guide/custom-filter.html)
  - [Convention de nommage des assets](/guide/components.html#Assets-Naming-Convention)

### components

- **Type:** `Objet`

- **Détails:**

  Une liste de composants disponibles pour l'instance de Vue.

- **Voir aussi:**
  - [Composants](/guide/components.html)

### transitions

- **Type:** `Objet`

- **Détails:**

  Une liste de transitions disponibles pour l'instance de Vue.

- **Voir aussi:**
  - [Transitions](/guide/transitions.html)

### partials

- **Type:** `Objet`

- **Détails:**

  Une liste de chaines partielles (HTML/texte) pour l'instance de Vue.

- **Voir aussi:**
  - [Éléments Spéciaux - partial](#partial)

## Options / Misc

### parent

- **Type:** `Instance de Vue`

- **Détails:**

  Spécifie l'instance de parent pour l'instance à créer. Établit une relation parent-enfant entre les deux. Le parent sera accessible avec `this.$parent` pour l'enfant, et l'enfant sera ajouté au tableau `$children` du parent.

- **Voir aussi:** [Communication parent-enfant](/guide/components.html#Parent-Child-Communication)

### events

- **Type:** `Objet`

- **Détails:**

  Un objet où les clés sont des événements à écouter et les valeurs sont les fonctions de rappel correspondantes. Notez que ce sont des événements Vue plutôt que des événements DOM. La valeur peut aussi être le nom d'une méthode. À l'instanciation, l'instance de Vue appellera `$on()` pour chaque entrée dans l'objet.

- **Exemple:**

  ``` js
  var vm = new Vue({
    events: {
      'hook:created': function () {
        console.log('created!')
      },
      greeting: function (msg) {
        console.log(msg)
      },
      // La valeur peut aussi être le nom d'une méthode
      bye: 'sayGoodbye'
    },
    methods: {
      sayGoodbye: function () {
        console.log('goodbye!')
      }
    }
  }) // -> created!
  vm.$emit('greeting', 'hi!') // -> hi!
  vm.$emit('bye')             // -> goodbye!
  ```

- **Voir aussi:**
  - [Méthodes d'instance - Évènements](#Instance-Methods-Events)
  - [Communication parent-enfant](/guide/components.html#Parent-Child-Communication)

### mixins

- **Type:** `Tableau`

- **Détails:**

  L'option `mixins` accepte un tableau d'objets mixins. Ces objets peuvent contenir des options d'instance, tout comme chaque objet d'instance de Vue, et ils seront fusionnés avec ces options en utilisant la même logique dans `Vue.extend()`. par exemple. Si votre mixin contient un hook `created` et le composant lui-même en a aussi un, les deux fonctions seront appelées.

  Les hooks de Mixin sont appelés dans l'ordre dans lequel ils sont fournis, et sont appelés avant les hooks du composant.

- **Exemple:**

  ``` js
  var mixin = {
    created: function () { console.log(1) }
  }
  var vm = new Vue({
    created: function () { console.log(2) },
    mixins: [mixin]
  })
  // -> 1
  // -> 2
  ```

- **Voir aussi:** [Mixins](/guide/mixins.html)

### name

- **Type:** `Chaine`

- **Restriction:** appliqué seulement si c'est utilisé à l'intérieur de `Vue.extend()`.

- **Détails:**

  Permet au composant de s'invoquer récusirvement dans son modèle. Notez que lorsqu'un composant est déclaré au niveau global avec `Vue.component()`, l'identifiant global est automatiquement défini comme son nom.

  Un autre avantage de spécifier une option `name` est l'inspection de la console. Lors de l'inspection d'un composant étendu de Vue dans la console, le nom du constructeur par défaut est `VueComponent`, ce qui n'est pas très significatif. En définissant l'option `name` (option facultative) dans `Vue.extend()`, vous obtiendrez un meilleur retour d'inspection afin que vous sachiez quel composant vous observez. La chaîne sera en "camelCase" et sera utilisé comme le nom du constructeur du composant.

- **Exemple:**

  ``` js
  var Ctor = Vue.extend({
    name: 'stack-overflow',
    template:
      '<div>' +
        // s'invoque de manière récursive
        '<stack-overflow></stack-overflow>' +
      '</div>'
  })

  // ceci entrainera une erreur - dépassement de la taille maximale de la pile
  // mais supposons que ça fonctionne ...
  var vm = new Ctor()

  console.log(vm) // -> StackOverflow {$el: null, ...}
  ```

## Propriété d'instance

### vm.$data

- **Type:** `Objet`

- **Détails:**

  C'est l'objet de données observé par l'instance de Vue. Vous pouvez l'échanger avec un nouvel objet. L'instance Vue redirecte l'accès aux propriétés sur son objet de données.

### vm.$el

- **Type:** `ÉlémentHTML`

- **Lecture seule**

- **Détails:**

  C'est l'élément DOM géré par l'instance de Vue. Notez que pour [Fragment d'instance](/guide/components.html#Fragment-Instance), `vm.$el` retournera un noeud d'ancrage qui indique la position de départ du fragment.

### vm.$options

- **Type:** `Objet`

- **Lecture seule**

- **Détails:**

  C'est les options d'instanciation utilisées pour l'instance de Vue actuelle. Ceci est utile lorsque vous souhaitez inclure des propriétés personnalisées dans les options:

  ``` js
  new Vue({
    customOption: 'foo',
    created: function () {
      console.log(this.$options.customOption) // -> 'foo'
    }
  })
  ```

### vm.$parent

- **Type:** `Instance de Vue`

- **Lecture seule**

- **Détails:**

  C'est l'instance parente. si l'instance actuelle en a une.

### vm.$root

- **Type:** `Instance de Vue`

- **Lecture seule**

- **Détails:**

  C'est l'instance de Vue racine de l'arborescence du composant actuel. Si l'instance actuelle n'a pas de parents, cette valeur sera l'instance elle-même.

### vm.$children

- **Type:** `Array<Vue instance>`

- **Lecture seule**

- **Détails:**

  C'est le composant enfant direct de l'instance actuelle.

### vm.$refs

- **Type:** `Objet`

- **Lecture seule**

- **Détails:**

  Un objet qui contient les composants enfants qui ont `v-ref` déclaré.

- **Voir aussi:**
  - [Références du composant enfant](/guide/components.html#Child-Component-Refs)
  - [v-ref](#v-ref).

### vm.$els

- **Type:** `Objet`

- **Lecture seule**

- **Détails:**

  Un objet qui contient les éléments DOM qui ont `v-el` déclaré.

- **Voir aussi:** [v-el](#v-el).

## Méthodes d'instance / Données

<h3 id="vm-watch">vm.$watch( expOrFn, callback, [options] )</h3>

- **Arguments:**
  - `{Chaine | Fonction} expOrFn`
  - `{Fonction} callback`
  - `{Objet} [options]`
    - `{Booléen} deep`
    - `{Booléen} immediate`

- **Retourne:** `{Fonction} unwatch`

- **Usage:**

  Observe les changements sur une expression ou une fonction calculée dans l'instance de Vue. La fonction de rappel est appelée avec la nouvelle valeur et l'ancienne valeur. L'expression peut être un seul chemin de clé (keypath) (`clé.clé.clé`) ou d'expressions de liaison valides.

<p class="tip">Remarque: lors de la mutation d'un objet ou d'un tableau, l'ancienne valeur sera la même que la nouvelle valeur parce qu'elles font référence au même objet/tableau. Vue ne conserve pas de copie de la valeur pré-muter.</p>

- **Exemple:**

  ``` js
  // keypath
  vm.$watch('a.b.c', function (newVal, oldVal) {
    // traitement
  })

  // expression
  vm.$watch('a + b', function (newVal, oldVal) {
    // traitement
  })

  // fonction
  vm.$watch(
    function () {
      return this.a + this.b
    },
    function (newVal, oldVal) {
      // traitement
    }
  )
  ```

  `vm.$watch` retourne une fonction `unwatch` qui arrête le déclenchement de la fonction du rappel:

  ``` js
  var unwatch = vm.$watch('a', cb)
  // later, teardown the watcher
  unwatch()
  ```

- **Option: deep**

  Pour détecter également des changements des valeurs imbriquées à l'intérieur des objets, vous devez déclarer `deep: true` dans l'argument `options`. Notez que vous n'avez pas besoin de le faire pour écouter les mutations de tableau.

  ``` js
  vm.$watch('someObject', callback, {
    deep: true
  })
  vm.someObject.nestedValue = 123
  // la fonction de rappel est déclenchée
  ```

- **Option: immediate**

  En déclarant `immediate: true` dans l'option déclenchera la fonction de rappel immédiatement avec la valeur actuelle de l'expression:

  ``` js
  vm.$watch('a', callback, {
    immediate: true
  })
  // la fonction de rappel est déclenché immédiatement avec la valeur actuelle `a`
  ```

<h3 id="vm-get">vm.$get( expression )</h3>

- **Arguments:**
  - `{Chaine} expression`

- **Usage:**

  Récupére une valeur de l'instance de Vue via une expression. Les expressions qui génèrent des erreurs seront supprimées et la valeur retournée sera `undefined`.

- **Exemple:**

  ``` js
  var vm = new Vue({
    data: {
      a: {
        b: 1
      }
    }
  })
  vm.$get('a.b') // -> 1
  vm.$get('a.b + 1') // -> 2
  ```

<h3 id="vm-set">vm.$set( keypath, value )</h3>

- **Arguments:**
  - `{Chaine} keypath`
  - `{*} value`

- **Usage:**

  Définit une valeur de données sur l'instance de Vue suivant un chemin de clés valide. Dans la plupart des cas, Il est préférable de définir des propriétés en utilisant la syntaxe de l'objet brut, par exemple `vm.a.b = 123`. Cette méthode est nécessaire uniquement dans deux scénarios:

  1. Lorsque vous avez un chemin de clés et que vous voulez définir dynamiquement la valeur à l'aide de ce chemin.

  2. Lorsque vous souhaitez définir une propriété qui n'existe pas.

  Si le chemin n'existe pas, il sera créer de manière récursive et sera réactif. Si une nouvelle propriété réactive est créée au niveau racine par la méthode `$set`, l'instance de Vue sera forcée dans un «cycle de digestion», au cours duquel tous ses observateurs sont réévalués.

- **Exemple:**

  ``` js
  var vm = new Vue({
    data: {
      a: {
        b: 1
      }
    }
  })

  // definir un chemin de clé/propriété existante
  vm.$set('a.b', 2)
  vm.a.b // -> 2

  // définir un chemin inexistant, forcera l'exécution du cycle de digestion
  vm.$set('c', 3)
  vm.c // ->
  ```

- **Voir aussi:** [Le concept de réactivité](/guide/reactivity.html)

<h3 id="vm-delete">vm.$delete( key )</h3>

- **Arguments:**
  - `{Chaine} key`

- **Usage:**

  Supprime une propriété situé au niveau racine sur l'instance de Vue (et aussi sa propriété `$data`). Force un cycle de digestion. Non recommandé.

<h3 id="vm-eval">vm.$eval( expression )</h3>

- **Arguments:**
  - `{Chaine} expression`

- **Usage:**

  Évalue une expression de liaison (binding expression) valide sur l'instance en cours. L'expression peut également contenir des filtres.

- **Exemple:**

  ``` js
  // supposant que vm.msg = 'hello'
  vm.$eval('msg | uppercase') // -> 'HELLO'
  ```

<h3 id="vm-interpolate">vm.$interpolate( templateString )</h3>

- **Arguments:**
  - `{Chaine} templateString`

- **Usage:**

  Évalue un partie de modèle contenant des interpolations `{{...}})`. Notez que cette méthode effectue simplement l'interpolation de chaîne; les directives d'attribut sont ignorées.

- **Exemple:**

  ``` js
  // supposant que vm.msg = 'hello'
  vm.$interpolate('{{msg}} world!') // -> 'hello world!'
  ```

<h3 id="vm-log">vm.$log( [keypath] )</h3>

- **Arguments:**
  - `{Chaine} [keypath]`

- **Usage:**

Enregistre les données de l'instance actuelle comme un objet simple, cela est plus efficace à l'inspection qu'un ensemble de getter/setters. Cette fonction accepte également un paramètre `keypath` facultatif.

  ``` js
  vm.$log() // logs entire ViewModel data
  vm.$log('item') // logs vm.item
  ```

## Méthodes d'instance / Évènements

<h3 id="vm-on">vm.$on( event, callback )</h3>

- **Arguments:**
  - `{Chaine} event`
  - `{Fonction} callback`

- **Usage:**

  Écoute un événement personnalisé sur l'actuelle vm. Les événements peuvent être déclenchés par `vm.$emit`, `vm.$dispatch` ou `vm.$broadcast`. La fonction de rappel recevra tous les arguments supplémentaires passés dans ces méthodes de déclenchement d'événement.

- **Exemple:**

  ``` js
  vm.$on('test', function (msg) {
    console.log(msg)
  })
  vm.$emit('test', 'hi')
  // -> "hi"
  ```

<h3 id="vm-once">vm.$once( event, callback )</h3>

- **Arguments:**
  - `{Chaine} event`
  - `{Fonction} callback`

- **Usage:**

  Écoute un événement personnalisé, mais seulement pour une fois. L'écouteur d'évènement sera supprimé une fois qu'il est déclenché pour la première fois.

<h3 id="vm-off">vm.$off( [event, callback] )</h3>

- **Arguments:**
  - `{Chaine} [event]`
  - `{Fonction} [callback]`

- **Usage:**

  Supprime un ou plusieurs écouteurs d'événement.

   - Si aucun argument n'est fourni, tous les écouteurs d'événement seront supprimés;

   - Si seulement l'événement est fourni, cela supprime tous les écouteurs pour cet événement;

   - Si l'événement et la fonction de rappel sont tous les deux fournis, cela supprimer l'écouteur uniquement pour cette fonction de rappel spécifique.

<h3 id="vm-emit">vm.$emit( event, [...args] )</h3>

- **Arguments:**
  - `{Chaine} event`
  - `[...args]`

  Déclenche un événement sur l'instance actuelle. Tous les autres arguments seront passés dans la fonction de rappel de l'écouteur.

<h3 id="vm-dispatch">vm.$dispatch( event, [...args] )</h3>

- **Arguments:**
  - `{Chaine} event`
  - `[...args]`

- **Usage:**

  Déclenche un événement, d'abord sur l'instance actuelle, puis se propage vers le haut le long de la chaîne hiérarchique. La propagation s'arrête quand il déclenche un écouteur d'événement parent, à moins que l'écouteur retourne `true`. Tous les arguments additonels seront passés dans la fonction de rappel de l'écouteur.

- **Exemple:**

  ``` js
  // créer une arborescence d'instances de Vue
  var parent = new Vue()
  var child1 = new Vue({ parent: parent })
  var child2 = new Vue({ parent: child1 })

  parent.$on('test', function () {
    console.log('parent notified')
  })
  child1.$on('test', function () {
    console.log('child1 notified')
  })
  child2.$on('test', function () {
    console.log('child2 notified')
  })

  child2.$dispatch('test')
  // -> "child2 notified"
  // -> "child1 notified"
  // le parent n'est pas notifié, parce que child1 n'a pas retourné 
  // 'true' dans sa fonction de rappel
  ```

- **Voir aussi:** [Communication parent-enfant](/guide/components.html#Parent-Child-Communication)

<h3 id="vm-broadcast">vm.$broadcast( event, [...args] )</h3>

- **Arguments:**
  - `{Chaine} event`
  - `[...args]`

- **Usage:**

  Broadcast an event that propagates downward to all descendants of the current instance. Since the descendants expand into multiple sub-trees, the event propagation will follow many different "paths". The propagation for each path will stop when a listener callback is fired along that path, unless the callback returns `true`.
  Diffuse un événement qui se propage vers le bas pour tous les descendants de l'instance actuelle. Étant donné que les descendants s'étendent en plusieurs sous-arbres, la propagation de l'événement suivra plusieurs "chemins" différents. La propagation pour chaque chemin s'arrêtera quand une fonction de rappel de l'écouteur est déclenchée le long de ce chemin, à moins qu'elle retourne `true`.

- **Exemple:**

  ``` js
  var parent = new Vue()
  // child1 et child2 sont au même niveau hiérarchique
  var child1 = new Vue({ parent: parent })
  var child2 = new Vue({ parent: parent })
  // child3 est l'enfant de child2
  var child3 = new Vue({ parent: child2 })

  child1.$on('test', function () {
    console.log('child1 notified')
  })
  child2.$on('test', function () {
    console.log('child2 notified')
  })
  child3.$on('test', function () {
    console.log('child3 notified')
  })

  parent.$broadcast('test')
  // -> "child1 notified"
  // -> "child2 notified"
  // child3 n'est pas notifié, parce que child2 n'a pas retourné
  // 'true' dans sa fonction de rappel
  ```

## Méthodes d'instance / DOM

<h3 id="vm-appendTo">vm.$appendTo( elementOrSelector, [callback] )</h3>

- **Arguments:**
  - `{Élément | Chaine} elementOrSelector`
  - `{Fonction} [callback]`

- **Retourne:** `vm` - l'instance elle-même

- **Usage:**

  Rajoute l'élément ou fragment DOM de l'instance Vue à l'élément cible. La cible peut être soit un élément ou une chaîne de querySelector. Cette méthode va déclencher des transitions si elle est présente. La fonction de rappel est déclenchée après que la transition ne soit terminée (ou immédiatement si aucune transition a été déclenchée).

<h3 id="vm-before">vm.$before( elementOrSelector, [callback] )</h3>

- **Arguments:**
  - `{Élément | Chaine} elementOrSelector`
  - `{Fonction} [callback]`

- **Retourne:** `vm` - l'instance elle-même

- **Usage:**

  Insére l'élément ou fragment DOM de l'instance Vue avant l'élément cible. La cible peut être soit un élément ou une chaîne de querySelector. Cette méthode va déclencher des transitions si elle est présente. La fonction de rappel est déclenchée après que la transition ne soit terminée (ou immédiatement si aucune transition a été déclenchée).

<h3 id="vm-after">vm.$after( elementOrSelector, [callback] )</h3>

- **Arguments:**
  - `{Élément | Chaine} elementOrSelector`
  - `{Fonction} [callback]`

- **Retourne:** `vm` - l'instance elle-même

- **Usage:**

  Insère l'élément ou fragment DOM de l'instance de Vue après l'élément cible. La cible peut être soit un élément ou une chaîne de querySelector. Cette méthode va déclencher des transitions si elle est présente. La fonction de rappel est déclenchée après que la transition ne soit terminée (ou immédiatement si aucune transition a été déclenchée).

<h3 id="vm-remove">vm.$remove( [callback] )</h3>

- **Arguments:**
  - `{Fonction} [callback]`

- **Retourne:** `vm` - l'instance elle-même

- **Usage:**

  Supprime l'élément ou fragment DOM de l'instance de Vue du DOM. Cette méthode va déclencher des transitions si elle est présente. La fonction de rappel est déclenchée après que la transition ne soit terminée (ou immédiatement si aucune transition a été déclenchée).

<h3 id="vm-nextTick">vm.$nextTick( callback )</h3>

- **Arguments:**
  - `{Fonction} [callback]`

- **Usage:**

  Diffère l'exécution de la fonction de rappel après le prochain cycle de mise à jour DOM. À utiliser immédiatement après changement de certaines données pour attendre la mise à jour du DOM. Ceci est simliaire à la fonction globale `Vue.nextTick`, sauf que le contexte `this` de la fonction de rappel est automatiquement lié à l'instance qui fait appel à cette méthode.

- **Exemple:**

  ``` js
  new Vue({
    // ...
    methods: {
      // ...
      example: function () {
        // changement de données
        this.message = 'changed'
        // DOM n'est pas encore mis à jour
        this.$nextTick(function () {
          // DOM est maintenant mis à jour
          // `this` est lié à l'instance actuelle
          this.doSomethingElse()
        })
      }
    }
  })
  ```

- **Voir aussi:**
  - [Vue.nextTick](#Vue-nextTick)
  - [File d'attente de mise à jour asynchrone](/guide/reactivity.html#Async-Update-Queue)

## Méthodes d'instance / Cycle de vie

<h3 id="vm-mount">vm.$mount( [elementOrSelector] )</h3>

- **Arguments:**
  - `{Élément | Chaine} [elementOrSelector]`

- **Retourne:** `vm` - l'instance elle-même

- **Usage:**

  Si une instance de Vue n'a pas reçu l'option `el` à l'instanciation, elle sera dans l'état "détachée/démontée", sans un élément DOM ou un fragment associé. `vm.$mount()` peut être utilisé pour démarrer manuellement le montage/compilation d'une instance de Vue démontée.

  Si aucun argument est fourni, le modèle sera créé comme un fragment hors-du- document, et vous aurez à utiliser vous-même d'autres méthodes d'instance de DOM pour l'insérer dans le document. Si l'option `replace` est définie sur `false`, alors un `<div>` sera automatiquement créé comme élément d'enveloppement.

  L'appel à `$mount()` sur une instance déjà montée n'a aucun effet. La méthode renvoie l'instance elle-même de sorte que vous pouvez enchaîner d'autres méthodes d'instance après elle.

- **Exemple:**

  ``` js
  var MyComponent = Vue.extend({
    template: '<div>Hello!</div>'
  })

  // créer et attacher à #app (remplacera <div id="#app"></div>)
  new MyComponent().$mount('#app')

  // ci-dessus est le même que:
  new MyComponent({ el: '#app' })

  // Ou, compiler hors-du-document et ajouter ensuite:
  new MyComponent().$mount().$appendTo('#container')
  ```

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

<h3 id="vm-destroy">vm.$destroy( [remove] )</h3>

- **Arguments:**
  - `{Booléen} [remove] - default: false`

- **Usage:**

  Détruit complètement une `vm`. Supprime ses connexions avec d'autres vms existantes, détache toutes ses directives, annule tous les écouteurs d'événements et, si l'argument `remove` est défini à `true`, supprime alors son élément DOM ou son fragment du DOM associé.

  Déclenche les hooks `beforeDestroy` et `destroyed`.

- **Voir aussi:** [Diagramme du cycle de vie](/guide/instance.html#Lifecycle-Diagram)

## Directives

### v-text

- **S'attend à:** `Chaine`

- **Détails:**

  Met à jour le `textContent` de l'élément. 

  En interne, les interpolations `{% raw %}{{ Mustache }}{% endraw %}` sont aussi compilées sur un noeud texte comme une directive `v-text`. La forme de la directive exige un élément d'enveloppement, mais offre des performances légèrement meilleures et évite FOUC (Flash of Uncompiled Content - Flashage du contenu non compilé).

- **Exemple:**

  ``` html
  <span v-text="msg"></span>
  <!-- est la même chose que: -->
  <span>{{msg}}</span>
  ```

### v-html

- **S'attend à:** `Chaine`

- **Détails:**

  Met à jour le `innerHTML` de l'élément. Le contenu est inséré en tant que HTML brut - les liaisons de données sont ignorées. Si vous avez besoin de réutiliser des parties de modèle, vous devez utiliser [partials](#partial).

  En interne, les interpolations `{% raw %}{{ Mustache }}{% endraw %}` sont aussi compilées comme une directive `v-html` utilisant des noeuds d'ancrage. La forme de la directive exige un élément d'enveloppement, mais offre des performances légèrement meilleures et évite FOUC (Flash of Uncompiled Content - Flashage du contenu non compilé).

  <p class="tip">Le rendu dynamique de contenu HTML sur votre site Web peut être très dangereux, car il peut facilement conduire à des [Attaques XSS] (https://fr.wikipedia.org/wiki/Cross-site_scripting). Utilisez uniquement `v-html` sur un contenu de confiance et **jamais** sur un contenu fourni par l'utilisateur.</p>

- **Exemple:**

  ``` html
  <div v-html="html"></div>
  <!-- est la même chose que: -->
  <div>{{{html}}}</div>
  ```

### v-if

- **S'attend à:** `*`

- **Usage:**

  Rendu conditonnel de l'élément sur la base de la valeur de l'expression `true` ou `false`. L'élément, ses liaisons de données et ses composants sont détruits et re-générés lors du changement de condition. Si l'élément est un `<template>` élément, son contenu sera extrait comme étant le bloc conditionnel.

- **Voir aussi:** [Rendu conditionel](/guide/conditional.html)

### v-show

- **S'attend à:** `*`

- **Usage:**

  Toggle's the element's `display` CSS property based on the truthy-ness of the expression value. Triggers transitions if present.

  Change la propriété CSS `display` de l'élément sur la base de la valeur d'expression `true` ou `false`. Déclenche les transitions si elles sont présentes.

- **Voir aussi:** [Rendu condionnel - v-show](/guide/conditional.html#v-show)

### v-else

- **Ne s'attend pas à avoir une expression**

- **Restriction:** précédent élément du même niveau dom doit avoir `v-if` ou ` v-show`.

- **Usage:**

  Désigne le "bloc else" pour `v-if` et `v-show`.

  ``` html
  <div v-if="Math.random() > 0.5">
    Sorry
  </div>
  <div v-else>
    Not sorry
  </div>
  ```

- **Voir aussi:** [Rendu conditionel - v-else](/guide/conditional.html#v-else)

### v-for

- **S'attend à:** `Tableau | Objet | Nombre | Chaine`

- **Paramètres d'attribut:**
  - [`track-by`](/guide/list.html#track-by)
  - [`stagger`](/guide/transitions.html#Staggering-Transitions)
  - [`enter-stagger`](/guide/transitions.html#Staggering-Transitions)
  - [`leave-stagger`](/guide/transitions.html#Staggering-Transitions)

- **Usage:**

  Génère l'élément ou bloc de modèle à plusieurs reprises sur la base des données source. La valeur de la directive doit utiliser la syntaxe spéciale `alias (in|of) expression` pour fournir un alias pour l'élément courant de l'itération:

  ``` html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Notez que l'utilisation de `of` comme délimiteur est uniquement diponible à partir de la version 1.0.17+.

  Alternativement, vous pouvez également spécifier un alias pour l'index (ou la clé si l'itération est sur un objet):

  ``` html
  <div v-for="(index, item) in items"></div>
  <div v-for="(cle, valeur) in objet"></div>
  ```

  L'utilisation détaillée de `v-for` est expliquée dans la section [Rendu de liste](/guide/list.html)

- **Voir aussi:** [Rendu de liste](/guide/list.html).

### v-on

- **Raccourci:** `@`

- **S'attend à:** `Fonction | déclaration en ligne (inline statement)`

- **Argument:** `event (requis)`

- **Modificateurs:**
  - `.stop` - appelle `event.stopPropagation()`.
  - `.prevent` - appelle `event.preventDefault()`.
  - `.capture` - ajoute un écouteur d'évènement en mode capture.
  - `.self` - déclenche uniquement le gestionnaire si l'évènement est envoyé de cet élément.
  - `.{keyCode | keyAlias}` - déclenche uniquement le gestionnaire sur certaines touches.

- **Usage:**

  Attache un écouteur d'événement à l'élément. Le type d'événement est désigné par l'argument. L'expression peut être soit un nom de méthode ou une déclaration en ligne (inline statement), ou tout simplement omis quand il y a des modificateurs présents.

  Lorsqu'il est utilisé sur un élément normal, il écoute les **événements DOM natifs** seulement. Lorsqu'il est utilisé sur un composant d'élément personnalisé, il écoute aussi les **événements personnalisés** émis sur ce composant enfant.

  Lors de l'écoute des événements DOM natifs, la méthode reçoit l'événement natif comme seul argument. Si vous utilisez la déclaration en ligne, la déclaration a accès à la propriété particulière `$event`: `v-on:click="handle('ok', $event)"`.

  **1.0.11+** Lorsque de l'écoute ses événements personnalisés, des déclaration en ligne (inline statement) ont accès à la propriété spéciale `$arguments`, qui est un tableau des arguments supplémentaires passés à l'appel `$emit` des composants de l'enfant.

- **Exemple:**

  ``` html
  <!-- method handler -->
  <button v-on:click="doThis"></button>

  <!-- inline statement -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- shorthand -->
  <button @click="doThis"></button>

  <!-- stop propagation -->
  <button @click.stop="doThis"></button>

  <!-- prevent default -->
  <button @click.prevent="doThis"></button>

  <!-- prevent default without expression -->
  <form @submit.prevent></form>

  <!-- chain modifiers -->
  <button @click.stop.prevent="doThis"></button>

  <!-- key modifier using keyAlias -->
  <input @keyup.enter="onEnter">

  <!-- key modifier using keyCode -->
  <input @keyup.13="onEnter">
  ```

  En écoutant des événements personnalisés sur un composant enfant (le gestionnaire est appelé lorsque «my-evenement" est émis sur l'enfant):

  ``` html
  <my-component @my-event="handleThis"></my-component>

  <!-- inline statement -->
  <my-component @my-event="handleThis(123, $arguments)"></my-component>
  ```

- **Voir aussi:** [Méthodes et gestion des événements](/guide/events.html)

### v-bind

- **Raccourci:** `:`

- **S'attend à:** `* (avec argument) | Object (sans argument)`

- **Argument:** `attrOrProp (facultatif)`

- **Modificateurs:**
  - `.sync` - Fait que la liaison soit dans les deux sens (two-way). Appliqué seulement sur la liaison de propriétés.
  - `.once` - Fait que la liaison soit faite une fois uniquement. Appliqué seulement sur la liaison de propriétés.
  - `.camel` - Convertit le nom d'attribut en camelCase lors de son initialisation. Seulement respecté pour les attributs normaux. Utilisé pour la liaison des attributs SVG camelCase.

- **Usage:**

  Lie dynamiquement un ou plusieurs attributs, ou une propriété de composant à une expression.

  Lorsqu'il est utilisé pour lier l'attribut `class` ou `style`, il prend en charge les types de valeurs supplémentaires, tels que les tableau ou les objets. Voir la section de guidage lié ci-dessous pour plus de détails.

  Lorsqu'il est utilisé pour la liaison de propriété, la propriété doit être correctement déclarée dans le composant enfant. les liaisons de propriété peuvent spécifier un type de liaison différent en utilisant l'un des modificateurs.

  Lorsqu'il est utilisé sans argument, peut être utilisé pour lier un objet contenant des paires nom-valeur attribut. Remarque: dans ce mode, `class` et `style` ne supporte pas les tableaux ou les objets.


- **Exemple:**

  ``` html
  <!-- bind an attribute -->
  <img v-bind:src="imageSrc">

  <!-- shorthand -->
  <img :src="imageSrc">

  <!-- class binding -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">

  <!-- style binding -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- prop binding. "prop" must be declared in my-component. -->
  <my-component :prop="someThing"></my-component>

  <!-- two-way prop binding -->
  <my-component :prop.sync="someThing"></my-component>

  <!-- one-time prop binding -->
  <my-component :prop.once="someThing"></my-component>
  ```

- **Voir aussi:**
  - [Classes et liaisons de style](/guide/class-and-style.html)
  - [Propriétés de composant](/guide/components.html#Props)

### v-model

- **S'attend à:** varie en fonction du type d'input

- **Limité à:**
  - `<input>`
  - `<select>`
  - `<textarea>`

- **Paramètres d'attribut:**
  - [`lazy`](/guide/forms.html#lazy)
  - [`number`](/guide/forms.html#number)
  - [`debounce`](/guide/forms.html#debounce)

- **Usage:**

  Crée une liaison dans les deux sens sur un élément input.

- **Voir aussi:** [Liaisons d'input](/guide/forms.html)

### v-ref

- **Ne s'attend pas à avoir une expression**

- **Limité à:** child components

- **Argument:** `id (required)`

- **Usage:**

  Register a reference to a child component on its parent for direct access. Does not expect an expression. Must provide an argument as the id to register with. The component instance will be accessible on its parent's `$refs` object.

  When used on a component together with `v-for`, the registered value will be an Array containing all the child component instances corresponding to the Array they are bound to. If the data source for `v-for` is an Object, the registered value will be an Object containing key-instance pairs mirroring the source Object.

- **Note:**

  Because HTML is case-insensitive, camelCase usage like `v-ref:someRef` will be converted to all lowercase. You can use `v-ref:some-ref` which properly sets `this.$refs.someRef`.

- **Exemple:**

  ``` html
  <comp v-ref:child></comp>
  <comp v-ref:some-child></comp>
  ```

  ``` js
  // access from parent:
  this.$refs.child
  this.$refs.someChild
  ```

  With `v-for`:

  ``` html
  <comp v-ref:list v-for="item in list"></comp>
  ```

  ``` js
  // this will be an array in parent
  this.$refs.list
  ```

- **Voir aussi:** [Références du composant enfant](/guide/components.html#Child-Component-Refs)

### v-el

- **Ne s'attend pas à avoir une expression**

- **Argument:** `id (requis)`

- **Usage:**

  Enregistre une référence à un élément DOM sur l'objet `$els` de sa propre instance Vue pour en faciliter l'accès.

- **Note:**

  Because HTML is case-insensitive, camelCase usage like `v-el:someEl` will be converted to all lowercase. You can use `v-el:some-el` which properly sets `this.$els.someEl`.
  HTML étant insensible à la casse, l'utilisation de camelCase comme `v-el: someEl` sera converti en minuscules. Vous pouvez utiliser `v-el: some-el` qui définit correctement `this.$els.someEl`.

- **Exemple:**

  ``` html
  <span v-el:msg>hello</span>
  <span v-el:other-msg>world</span>
  ```
  ``` js
  this.$els.msg.textContent // -> "hello"
  this.$els.otherMsg.textContent // -> "world"
  ```

### v-pre

- **Ne s'attend pas à avoir une expression**

- **Usage**

  Ignore la compilation pour cet élément et tous ses enfants. Vous pouvez l'utiliser pour afficher les balises d'interpolation mustache. Ignorer un grand nombre de nœuds sans directives peut aussi accélérer la compilation.

- **Exemple:**

  ``` html
  <span v-pre>{{ ceci ne sera pas compilé }}</span>
  ```

### v-cloak

- **Ne s'attend pas à avoir une expression**

- **Usage:**

  This directive will remain on the element until the associated Vue instance finishes compilation. Combined with CSS rules such as `[v-cloak] { display: none }`, this directive can be used to hide un-compiled mustache bindings until the Vue instance is ready.
  Cette directive restera sur l'élément jusqu'à ce que l'instance de Vue associée termine la compilation. Combiné avec les règles CSS comme `[v-cloak] {display: none}`, cette directive peut être utilisé pour cacher les interpolations non-compilées jusqu'à ce que l'instance de Vue soit prête.

- **Exemple:**

  ``` css
  [v-cloak] {
    display: none;
  }
  ```

  ``` html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  `<div>` ne sera visible que lorsque la compilation ne soit terminée.

## Éléments Spéciaux

### component

- **Attributs:**
  - `is`

- **Paramètres d'attribut:**
  - [`keep-alive`](/guide/components.html#keep-alive)
  - [`transition-mode`](/guide/components.html#transition-mode)

- **Usage:**

  Syntaxe alternative pour appeler les composants. Principalement utilisé pour les composants dynamiques avec l'attribut `is`:

  ``` html
  <!-- un compsant dynamique controllé par -->
  <!-- la propriété `componentId` sur l'instance vm -->
  <component :is="componentId"></component>
  ```

- **Voir aussi:** [Composants dynamiques](/guide/components.html#Dynamic-Components)

### slot

- **Attributs:**
  - `name`

- **Usage:**

  `<slot>` éléments servent de points de distribution de contenu dans les modèles de composants. L'élément slot lui-même sera remplacé.

  Un slot avec l'attribut `name` est appelé un slot nommé. Un slot nommé va distribuer le contenu avec un attribut `slot` qui correspond à son nom.

  Pour une utilisation détaillée, voir la section du guide ci-dessous.

- **Voir aussi:** [Distribution de contenu avec des slots](/guide/components.html#Content-Distribution-with-Slots)

### partial

- **Attributs:**
  - `name`

- **Usage:**

  `<partial>` éléments servent de noeuds pour des modèles partiels  enregistrés. les contenus partiels sont également compilées par l'instance de Vue lorsqu'ils sont insérés. L'élément `<partial>` sera remplacé. cet élément exige un attribut `name` qui sera utilisé pour résoudre le contenu partiel.

- **Exemple:**

  ``` js
  // déclaration d'un contenu partiel
  Vue.partial('my-partial', '<p>This is a partial! {{msg}}</p>')
  ```

  ``` html
  <!-- contenu partiel statique -->
  <partial name="my-partial"></partial>

  <!-- contenu partiel dynamique -->
  <!-- compile le contenu partiel avec id === vm.partialId -->
  <partial v-bind:name="partialId"></partial>

  <!-- contenu partiel dynamique utilisant le raccourci de v-bind -->
  <partial :name="partialId"></partial>
  ```

## Filtres

### capitalize

- **Exemple:**

  ``` html
  {{ msg | capitalize }}
  ```

  *'abc' => 'Abc'*

### uppercase

- **Exemple:**

  ``` html
  {{ msg | uppercase }}
  ```

  *'abc' => 'ABC'*

### lowercase

- **Exemple:**

  ``` html
  {{ msg | lowercase }}
  ```

  *'ABC' => 'abc'*

### currency

- **Arguments:**
  - `{Chaine} [symbol] - default: '$'`

- **Exemple:**

  ``` html
  {{ amount | currency }}
  ```

  *12345 => $12,345.00*

  Utilisant un autre symbole:

  ``` html
  {{ amount | currency '£' }}
  ```

  *12345 => £12,345.00*

### pluralize

- **Arguments:**
  - `{Chaine} single, [double, triple, ...]`

- **Usage:**

  Pluralise l'argument en se basant sur la valeur filtrée. Quand il y a exactement un argument, cela ajoutera simplement un «s» à la fin. Quand il y a plus d'un argument, les arguments seront utilisés comme tableau de chaînes correspondant aux formes: (simples, doubles, triples...) de mot à pluraliser. Lorsque le numéro à pluraliser dépasse la longueur des arguments, il utilisera la dernière entrée dans le tableau.

- **Exemple:**

  ``` html
  {{count}} {{count | pluralize 'item'}}
  ```

  *1 => '1 item'*
  *2 => '2 items'*

  ``` html
  {{date}}{{date | pluralize 'st' 'nd' 'rd' 'th'}}
  ```

  Le résultat sera:

  *1 => '1st'*
  *2 => '2nd'*
  *3 => '3rd'*
  *4 => '4th'*
  *5 => '5th'*

### json

- **Arguments:**
  - `{Nombre} [indent] - default: 2`

- **Usage:**

  Retourne le résultat de l'appel `JSON.stringify()` sur la valeur au lieu de retourner le résultat de `toString ()` (par exemple `[object Object]`).

- **Exemple:**

  Affiche un objet avec une indentation de 4-espaces:

  ``` html
  <pre>{{ nestedObject | json 4 }}</pre>
  ```

### debounce

- **Limité à:** Les directives qui s'attendent à `Fonction` comme valeur, e.g. `v-on`

- **Arguments:**
  - `{Nombre} [wait] - default: 300`

- **Usage:**

  Retarde l'exécution de la `Fonction` de `x` millisecondes, où `x` est l'argument. Le temps de délai par défaut est de 300ms. Si le gestionnaire est appelé à nouveau avant le délai imparti, le délai sera remis à `x` millisecondes.

- **Exemple:**

  ``` html
  <input @keyup="onKeyup | debounce 500">
  ```

### limitBy

- **Limité à:** Les directives qui s'attendent à `Array` comme valeur, e.g. `v-for`

- **Arguments:**
  - `{Nombre} limit`
  - `{Nombre} [offset]`

- **Usage:**

  Limite le tableau pour les N premiers éléments, spécifié par l'argument. Un second argument facultatif peut être fourni pour définir un offset de départ.

  ``` html
  <!-- affiche uniquement les 10 premiers éléments -->
  <div v-for="item in items | limitBy 10"></div>

  <!-- affiche 10 éléments à partir du 5ème élément, soit l'intervalle 5 à 15 -->
  <div v-for="item in items | limitBy 10 5"></div>
  ```

### filterBy

- **Limité à:** Les directives qui s'attendent à `Array` comme valeur, e.g. `v-for`

- **Arguments:**
  - `{Chaine | Fonction} targetStringOrFunction`
  - `"in" (optional delimiter)`
  - `{Chaine} [...searchKeys]`

- **Usage:**

  Retourne une version filtrée du tableau source. Le premier argument peut être une chaîne ou une fonction.

  Lorsque le premier argument est une chaîne, elle sera utilisée comme la chaîne cible à rechercher dans chaque élément du tableau:

  ``` html
  <div v-for="item in items | filterBy 'bonjour'">
  ```

  Dans l'exemple ci-dessus, seuls les éléments contenant la chaîne cible `" bonjour"` seront affichés.

  Si l'élément est un objet, le filtre recherchera la chaîne cible récursivement dans chaque propriété imbriquée de l'objet. Pour restreindre la portée de la recherche, des clés supplémentaires de recherche peuvent être spécifiées:

  ``` html
  <div v-for="user in users | filterBy 'Jack' in 'name'">
  ```

  Dans l'exemple ci-dessus, le filtre ne recherchera `" Jack "` que dans le champ `name` de chaque objet. **C'est une bonne idée de toujours limiter la portée de la recherche pour une meilleure performance.**

  Les exemples ci-dessus utilisent des arguments statiques - nous pouvons, bien sûr, utiliser des arguments dynamiques comme chaîne cible ou les touches de recherche. Combiné avec `v-model` nous pouvons facilement mettre en œuvre un filtrage "typeahead":

  ``` html
  <div id="filter-by-example">
    <input v-model="name">
    <ul>
      <li v-for="user in users | filterBy name in 'name'">
        {{ user.name }}
      </li>
    </ul>
  </div>
  ```

  ``` js
  new Vue({
    el: '#filter-by-example',
    data: {
      name: '',
      users: [
        { name: 'Bruce' },
        { name: 'Chuck' },
        { name: 'Jackie' }
      ]
    }
  })
  ```

  {% raw %}
  <div id="filter-by-example" class="demo">
    <input v-model="name">
    <ul>
      <li v-for="user in users | filterBy name in 'name'">
        {{ user.name }}
      </li>
    </ul>
  </div>
  <script>
  new Vue({
    el: '#filter-by-example',
    data: {
      name: '',
      users: [{ name: 'Bruce' }, { name: 'Chuck' }, { name: 'Jackie' }]
    }
  })
  </script>
  {% endraw %}

- **Exemples supplémentaires:**

  Recherche à multiples clés:

  ``` html
  <li v-for="user in users | filterBy searchText in 'name' 'phone'"></li>
  ```

  Recherche à multiples clés avec comme argument un tableau dynamique:

  ``` html
  <!-- fields = ['fieldA', 'fieldB'] -->
  <div v-for="user in users | filterBy searchText in fields">
  ```

  Utilisation d'une fonction de filtre personnalisé:

  ``` html
  <div v-for="user in users | filterBy myCustomFilterFunction">
  ```

### orderBy

- **Limité à:** Les directives qui s'attendent à `Array` comme valeur, e.g. `v-for`

- **Arguments:**
  - `{Chaine} sortKey`
  - `{Chaine} [order] - default: 1`

- **Usage:**

  Retourne une version triée du tableau source. Le `sortKey` est la clé à utiliser pour le tri. L'argument facultatif `order` spécifie si le résultat doit être en ordre croissant (`ordre> = 0`) ou décroissant (`ordre <0`).

  Pour les tableaux de valeurs primitives, toute vraie valeur de `sortKey` fonctionnera.

- **Exemple:**

  Tri des utilisateurs par la propriété 'name':

  ``` html
  <ul>
    <li v-for="user in users | orderBy 'name'">
      {{ user.name }}
    </li>
  </ul>
  ```

  Par ordre descendant:

  ``` html
  <ul>
    <li v-for="user in users | orderBy 'name' -1">
      {{ user.name }}
    </li>
  </ul>
  ```

  Trier les valeurs primitives:

  ``` html
  <ul>
    <li v-for="n in numbers | orderBy true">
      {{ n }}
    </li>
  </ul>
  ```

  Ordre de tri dynamique:

  ``` html
  <div id="orderby-example">
    <button @click="order = order * -1">Reverse Sort Order</button>
    <ul>
      <li v-for="user in users | orderBy 'name' order">
        {{ user.name }}
      </li>
    </ul>
  </div>
  ```

  ``` js
  new Vue({
    el: '#orderby-example',
    data: {
      order: 1,
      users: [{ name: 'Bruce' }, { name: 'Chuck' }, { name: 'Jackie' }]
    }
  })
  ```

  {% raw %}
  <div id="orderby-example" class="demo">
    <button @click="order = order * -1">Reverse Sort Order</button>
    <ul>
      <li v-for="user in users | orderBy 'name' order">
        {{ user.name }}
      </li>
    </ul>
  </div>
  <script>
  new Vue({
    el: '#orderby-example',
    data: {
      order: 1,
      users: [{ name: 'Bruce' }, { name: 'Chuck' }, { name: 'Jackie' }]
    }
  })
  </script>
  {% endraw %}

## Méthodes d'extension de tableau

Vue.js étend `Array.prototype` avec deux autres méthodes rendant plus facile la réalisation de certaines opérations communes de tableau, tout en assurant que les mises à jour réactives sont correctement déclenchées.

### array.$set(index, value)

- **Arguments**
  - `{Nombre} index`
  - `{*} value`

- **Usage**

  Définit une valeur à un élément du tableau par l'index et déclenche les mises à jour de la vue.

  ``` js
  vm.animals.$set(0, { name: 'Aardvark' })
  ```

- **Voir aussi:** [Mises en garde de détection de tableau](/guide/list.html#Caveats)

### array.$remove(reference)

- **Arguments**
  - `{Référence} reference`

- **Usage**

  Supprime un élément d'un tableau par référence et déclenche les mises à jour de la vue. Ceci est une "pré-méthode" qui, dans un premier temps, recherche l'élément dans le tableau, et ensuite, si trouvé, fait appel à `Array.splice (index, 1)`.

  ``` js
  var aardvark = vm.animals[0]
  vm.animals.$remove(aardvark)
  ```

- **Voir aussi:** [Méthodes de mutation](/guide/list.html#Mutation-Methods)
