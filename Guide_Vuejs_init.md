# Initialisation d'un nouveau projet VueJS

## 🔗 Création du projet

```bash
npm create vue@latest
```

## 🔗 Lancer le projet vue
### Initialisation
```sh
npm install
```

### En développement, compilation + Hot Reload 
```sh
npm run dev
```

### En Production, compilation + Minification
```sh
npm run build
```

## 📦 Architecture

```bash
PROJET/FRONT/
├── src/
│  	|	├──  assets/
│  	|	├──  infrastructure/
|   | 	| 	├──  api/
|   | 	| 	├──  dto/
| 	|   | 	└──  utils/
│  	|	├──  locales/
│  	|	├──  presentation/
|   | 	| 	├──  components/
|   | 	| 	├──  router/
|   | 	| 	├──  store/
| 	|   | 	└──  views/
| 	├──  App.vue
| 	└── [...]

```

## Librairies

### 🔗 vue-i18n

#### Introduction à i18n

Vue I18n est la solution d'internationalisation pour Vue.js. Il s'intègre parfaitement à votre application Vue et fournit une façon simple d'ajouter le support multilingue.

#### Installation

```bash
npm install vue-i18n@next

# OU

yarn add vue-i18n@next
```

#### Intégration au projet

Dans le fichier `main.js`
```js
import { createI18n } from 'vue-i18n'
import fr_locale from '@/locales/fr.json'

const messages = {
  fr: fr_locale, 
}

const i18n = createI18n({
  legacy: false,
  locale: APP_LOCALE || 'fr', // Langue par défaut depuis .env
  fallbackLocale: 'fr',
  messages,
})

app.use(i18n)
```

#### Fichier JSON

Création du dossier `src/locales` qui regroupe les fichiers JSON pour chaque traductions.

Exemple fichier `fr.json`
```json
{
    "welcome": "Bienvenue",
	"hello": "Bonjour {name}!",
    "info": "Bonjour {0}, vous avez {1} messages",
	"car": "Aucune voiture | Une voiture | {n} voitures"
}
```

#### Utilisation dans un template

```vue
<template>
  <!-- Avec des paramètres nommés -->
  <p>{{ $t('welcome') }}</p>
  <!-- Avec des paramètres nommés -->
  <p>{{ $t('hello', { name: 'Jean' }) }}</p>
  <!-- Avec des paramètres par index -->
  <p>{{ $t('info', ['Jean', 5]) }}</p>
  
  <p>{{ $tc('car', 0) }}</p> <!-- Affiche: Aucune voiture -->
  <p>{{ $tc('car', 1) }}</p> <!-- Affiche: Une voiture -->
  <p>{{ $tc('car', 2) }}</p> <!-- Affiche: 2 voitures -->
  <p>{{ $tc('car', 5) }}</p> <!-- Affiche: 5 voitures -->
</template>
```

### 🔗 Pinia

#### Introduction de Pinia

**Pinia** est la **bibliothèque officielle de gestion d'état** recommandée pour les applications **Vue.js**. Elle a été développée par l'équipe Vue et est considérée comme le successeur de Vuex. Pinia permet de centraliser toutes les données partagées entre les composants dans des **stores**, ce qui facilite la gestion, la traçabilité et la cohérence des données dans les applications complexes.

Pinia adopte une **architecture simplifiée** par rapport à Vuex avec :

-   **State** : l'état centralisé (les données globales),
    
-   **Getters** : des fonctions pour lire/calculer des valeurs dérivées de l'état,
    
-   **Actions** : des fonctions pour modifier l'état et effectuer des opérations (synchrones ou asynchrones),
    
-   **Stores modulaires** : chaque store est défini indépendamment sans nécessiter de namespace explicite.

Pinia est particulièrement utile pour le partage de données entre composants, et offre une meilleure intégration avec TypeScript et les outils de développement Vue.

---
#### Installation de Pinia

##### Installation via npm/yarn
```bash
npm install pinia

# yarn
yarn add pinia

# pnpm
pnpm add pinia
```

##### Configuration avec Vue 3
```js
// main.js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)
const pinia = createPinia()

app.use(pinia)
app.mount('#app')
```

##### Configuration avec Vue 2
```js
// main.js
import Vue from 'vue'
import { createPinia, PiniaVuePlugin } from 'pinia'
import App from './App.vue'

Vue.use(PiniaVuePlugin)
const pinia = createPinia()

new Vue({
  pinia,
  render: (h) => h(App)
}).$mount('#app')
```

---
#### Interaction avec un Store Pinia

##### Définir un store
```js
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  // state
  state: () => ({
    count: 0
  }),
  // getters
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  // actions
  actions: {
    increment() {
      this.count++
    },
    async fetchData() {
      // opérations asynchrones
      const data = await api.get('/data')
      this.count = data.value
    }
  }
})
```

##### Utiliser un store avec Composition API
```js
import { useCounterStore } from '@/stores/counter'

export default {
  setup() {
    const counterStore = useCounterStore()
    
    // Accéder au state
    console.log(counterStore.count)
    
    // Appeler une action
    counterStore.increment()
    counterStore.fetchData()
    
    // Accéder à un getter
    console.log(counterStore.doubleCount)
    
    return { counterStore }
  }
}
```

##### Utiliser un store avec Options API
```js
import { mapState, mapActions, mapStores } from 'pinia'
import { useCounterStore } from '@/stores/counter'

export default {
  computed: {
    // Option 1: Accéder au store complet
    ...mapStores(useCounterStore),
    
    // Option 2: Mapper state et getters spécifiques
    ...mapState(useCounterStore, ['count', 'doubleCount'])
  },
  methods: {
    // Mapper des actions
    ...mapActions(useCounterStore, ['increment', 'fetchData'])
  }
}
```

##### Modifier le state directement
```js
// Avec Pinia, on peut modifier l'état directement (contrairement à Vuex)
const store = useCounterStore()
store.count = 10

// Ou modifier plusieurs propriétés à la fois
store.$patch({
  count: 20,
  otherProp: 'value'
})

// Ou utiliser une fonction pour des modifications complexes
store.$patch((state) => {
  state.count += 10
  if (state.count > 30) {
    state.otherProp = 'new value'
  }
})
```

##### Observer les changements d'état
```js
import { storeToRefs } from 'pinia'

// Extraction réactive (ne pas destructurer directement le store)
const { count, doubleCount } = storeToRefs(useCounterStore())

// Surveiller les changements d'état
watch(count, (newValue) => {
  console.log(`Count est maintenant: ${newValue}`)
})
```

##### Reset d'un store
```js
// Réinitialiser un store à son état initial
const store = useCounterStore()
store.$reset()
```

### 🔗 Axios

#### Installation d'Axios

##### Installation via npm/yarn
```bash
# npm
npm install axios

# yarn
yarn add axios

# pnpm
pnpm add axios
```

##### Import dans un composant ou un fichier
```js
import axios from 'axios'
```

#### Introduction d'Axios

**Axios** est une **bibliothèque HTTP client** basée sur les promesses pour le navigateur et Node.js. Elle permet d'effectuer des requêtes HTTP de manière simple et élégante vers des APIs ou des serveurs. Axios est devenu un standard dans l'écosystème JavaScript pour gérer les communications réseau.

Axios offre de nombreuses fonctionnalités :

- Support des **promesses** et de **async/await**
- **Intercepteurs** pour les requêtes et les réponses
- **Transformation** automatique des données JSON
- **Annulation** des requêtes
- Protection contre les attaques **XSRF**
- Gestion des **timeouts**
- **Retries** automatiques sur erreur (avec modules complémentaires)
- Support de **téléchargement** de fichiers avec suivi de progression

Axios est particulièrement utile pour communiquer avec des APIs REST dans les applications Vue.js, React, Angular ou Node.js.

---
#### Utilisation d'Axios

##### Requêtes HTTP basiques
```js
// GET request
axios.get('/api/users')
  .then(response => {
    console.log(response.data)
  })
  .catch(error => {
    console.error('Erreur:', error)
  })

// POST request
axios.post('/api/users', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
  .then(response => {
    console.log(response.data)
  })
  .catch(error => {
    console.error('Erreur:', error)
  })

// Autres méthodes HTTP
axios.put('/api/users/1', userData)
axios.delete('/api/users/1')
axios.patch('/api/users/1', userData)
```

##### Avec async/await
```js
async function getUsers() {
  try {
    const response = await axios.get('/api/users')
    console.log(response.data)
    return response.data
  } catch (error) {
    console.error('Erreur:', error)
    throw error
  }
}
```

##### Paramètres de requête
```js
// URL avec paramètres de requête
axios.get('/api/users', {
  params: {
    page: 1,
    limit: 10,
    sortBy: 'name'
  }
})

// Équivalent à: /api/users?page=1&limit=10&sortBy=name
```

##### Configuration d'instance
```js
// Créer une instance avec une configuration personnalisée
const api = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000,
  headers: {
    'Authorization': 'Bearer token',
    'Content-Type': 'application/json'
  }
})

// Utiliser l'instance
api.get('/users')
```

##### Intercepteurs
```js
// Intercepteur de requête
axios.interceptors.request.use(config => {
  // Modifier la config avant envoi de la requête
  config.headers.Authorization = `Bearer ${localStorage.getItem('token')}`
  return config
}, error => {
  return Promise.reject(error)
})

// Intercepteur de réponse
axios.interceptors.response.use(response => {
  // Traitement de la réponse
  return response
}, error => {
  // Traitement des erreurs (ex: redirection sur 401)
  if (error.response && error.response.status === 401) {
    // Rediriger vers la page de connexion
  }
  return Promise.reject(error)
})
```

##### Annulation de requête
```js
// Avec AbortController (moderne)
const controller = new AbortController()

axios.get('/api/users', {
  signal: controller.signal
})

// Annuler la requête
controller.abort()

// Avec CancelToken (méthode classique)
const CancelToken = axios.CancelToken
const source = CancelToken.source()

axios.get('/api/users', {
  cancelToken: source.token
})

// Annuler la requête
source.cancel('Requête annulée par l'utilisateur')
```

##### Traitement des erreurs
```js
axios.get('/api/users')
  .then(response => {
    console.log(response.data)
  })
  .catch(error => {
    if (error.response) {
      // Le serveur a répondu avec un code d'erreur
      console.error('Erreur serveur:', error.response.status)
      console.error('Données:', error.response.data)
    } else if (error.request) {
      // La requête a été faite mais pas de réponse
      console.error('Pas de réponse:', error.request)
    } else {
      // Erreur de configuration
      console.error('Erreur:', error.message)
    }
  })
```

##### Requêtes simultanées
```js
// Exécuter plusieurs requêtes en parallèle
Promise.all([
  axios.get('/api/users'),
  axios.get('/api/products')
])
  .then(([usersResponse, productsResponse]) => {
    const users = usersResponse.data
    const products = productsResponse.data
    // Traiter les données
  })
  .catch(error => {
    console.error('Erreur:', error)
  })
```

##### Téléchargement avec progression
```js
axios.get('/api/download', {
  responseType: 'blob',
  onDownloadProgress: progressEvent => {
    const percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total)
    console.log(`Téléchargement: ${percentCompleted}%`)
  }
})
  .then(response => {
    // Créer un lien pour le téléchargement
    const url = window.URL.createObjectURL(new Blob([response.data]))
    const link = document.createElement('a')
    link.href = url
    link.setAttribute('download', 'fichier.pdf')
    document.body.appendChild(link)
    link.click()
  })
```

##### Intégration avec Vue.js
```js
// Dans un plugin Vue (plugins/axios.js)
import axios from 'axios'

export default {
  install(app) {
    // Créer une instance Axios
    const api = axios.create({
      baseURL: process.env.VUE_APP_API_URL || 'https://api.example.com'
    })
    
    // Ajouter des intercepteurs
    api.interceptors.request.use(config => {
      // Logique d'ajout de token
      return config
    })
    
    // Rendre l'instance disponible globalement
    app.config.globalProperties.$axios = api
    app.provide('axios', api)
  }
}

// Dans main.js
import axiosPlugin from './plugins/axios'
app.use(axiosPlugin)

// Utilisation dans un composant Options API
this.$axios.get('/users')

// Utilisation dans un composant Composition API
import { inject } from 'vue'
const axios = inject('axios')
axios.get('/users')
```