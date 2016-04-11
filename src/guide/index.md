---
title: Commencer
type: guide
order: 1
---

Commençons par un tour rapide concernant les caractéristiques de data binding dans Vue.js. Si vous êtes intéressé par des exemples plus avancés, Lisez cet [article](http://blog.evanyou.me/2015/10/25/vuejs-re-introduction/).

La façon la plus facile d'essayer Vue.js est d'utiliser l'[Exemple Hello World sur JSFiddle](https://jsfiddle.net/yyx990803/okv0rgrk/). Vous êtes libre de l'ouvrir dans un autre onglet et de suivre pendant que nous montrions quelques exemples de base. Si vous préférez le téléchargement et installation à partir d'un gestionnaire de paquets, consultez la rubrique [Installation](/guide/installation.html).

### Hello World

``` html
<div id="app">
  {{ message }}
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    message: 'Bonjour Vue.js!'
  }
})
```
{% raw %}
<div id="app" class="demo">
  {{ message }}
</div>
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Bonjour Vue.js!'
  }
})
</script>
{% endraw %}

### Two-way Binding (Liaison de modèle de donnée à deux sens)

``` html
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    message: 'Bonjour Vue.js!'
  }
})
```
{% raw %}
<div id="app2" class="demo">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
<script>
new Vue({
  el: '#app2',
  data: {
    message: 'Bonjour Vue.js!'
  }
})
</script>
{% endraw %}

### Générer une liste

``` html
<div id="app">
  <ul>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ul>
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    todos: [
      { text: 'Apprendre JavaScript' },
      { text: 'Apprendre Vue.js' },
      { text: 'Construire quelque chose de génial!' }
    ]
  }
})
```
{% raw %}
<div id="app3" class="demo">
  <ul>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ul>
</div>
<script>
new Vue({
  el: '#app3',
  data: {
    todos: [
      { text: 'Apprendre JavaScript' },
      { text: 'Apprendre Vue.js' },
      { text: 'Construire quelque chose de génial!' }
    ]
  }
})
</script>
{% endraw %}

### Gérer les données saisies par l'utilisateur

``` html
<div id="app">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Inverser le message</button>
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    message: 'Bonjour Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
{% raw %}
<div id="app4" class="demo">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Inverser le message</button>
</div>
<script>
new Vue({
  el: '#app4',
  data: {
    message: 'Bonjour Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
{% endraw %}

### Maintenant, le tout ensemble:

``` html
<div id="app">
  <input v-model="newTodo" v-on:keyup.enter="addTodo">
  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.text }}</span>
      <button v-on:click="removeTodo($index)">X</button>
    </li>
  </ul>
</div>
```
``` js
new Vue({
  el: '#app',
  data: {
    newTodo: '',
    todos: [
      { text: 'Ajouter des tâches' }
    ]
  },
  methods: {
    addTodo: function () {
      var text = this.newTodo.trim()
      if (text) {
        this.todos.push({ text: text })
        this.newTodo = ''
      }
    },
    removeTodo: function (index) {
      this.todos.splice(index, 1)
    }
  }
})
```
{% raw %}
<div id="app5" class="demo">
  <input v-model="newTodo" v-on:keyup.enter="addTodo">
  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.text }}</span>
      <button v-on:click="removeTodo($index)">X</button>
    </li>
  </ul>
</div>
<script>
new Vue({
  el: '#app5',
  data: {
    newTodo: '',
    todos: [
      { text: 'Ajouter des tâches' }
    ]
  },
  methods: {
    addTodo: function () {
      var text = this.newTodo.trim()
      if (text) {
        this.todos.push({ text: text })
        this.newTodo = ''
      }
    },
    removeTodo: function (index) {
      this.todos.splice(index, 1)
    }
  }
})
</script>
{% endraw %}

J'espère que tout ceci vous donne une idée de base de la façon dont Vue.js fonctionne. Maintenant, je suis sûr que vous avez aussi beaucoup de questions - continuer à lire, nous allons les couvrir dans le reste du guide.
