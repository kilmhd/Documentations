
# Guide Complet d'Installation et d'Utilisation de Vue.js avec Visual Studio Code

## Table des matières

1.  [Introduction](#introduction)
2.  [Prérequis](#pr%C3%A9requis)
3.  [Installation de Node.js et npm](#installation-de-nodejs-et-npm)
    -   [Windows](#installation-sur-windows)
    -   [macOS](#installation-sur-macos)
    -   [Linux](#installation-sur-linux)
4.  [Installation de Visual Studio Code](#installation-de-visual-studio-code)
5.  [Configuration de VS Code pour Vue.js](#configuration-de-vs-code-pour-vuejs)
6.  [Installation de Vue.js](#installation-de-vuejs)
    -   [Vue CLI](#installation-de-vue-cli)
    -   [Vite](#installation-avec-vite)
    -   [Nuxt.js](#installation-avec-nuxtjs)
7.  [Création d'un nouveau projet Vue.js](#cr%C3%A9ation-dun-nouveau-projet-vuejs)
    -   [Avec Vue CLI](#cr%C3%A9ation-avec-vue-cli)
    -   [Avec Vite](#cr%C3%A9ation-avec-vite)
    -   [Avec Nuxt.js](#cr%C3%A9ation-avec-nuxtjs)
8.  [Structure d'un projet Vue.js](#structure-dun-projet-vuejs)
    -   [Structure Vue CLI](#structure-vue-cli)
    -   [Structure Vite](#structure-vite)
    -   [Structure Nuxt.js](#structure-nuxtjs)
9.  [Fonctionnalités essentielles de Vue.js](#fonctionnalit%C3%A9s-essentielles-de-vuejs)
10.  [Exécution de votre application](#ex%C3%A9cution-de-votre-application)
11.  [Débogage avec VS Code](#d%C3%A9bogage-avec-vs-code)
12.  [Extensions VS Code recommandées](#extensions-vs-code-recommand%C3%A9es)
13.  [Astuces et raccourcis](#astuces-et-raccourcis)
14.  [Génération de composants](#g%C3%A9n%C3%A9ration-de-composants)
15.  [Tests unitaires et e2e](#tests-unitaires-et-e2e)
16.  [Déploiement](#d%C3%A9ploiement)
17.  [Résolution des problèmes courants](#r%C3%A9solution-des-probl%C3%A8mes-courants)
18.  [Ressources complémentaires](#ressources-compl%C3%A9mentaires)

## Introduction

Vue.js est un framework JavaScript progressif pour la construction d'interfaces utilisateur. Contrairement à d'autres frameworks monolithiques, Vue est conçu pour être adopté de manière incrémentielle. La bibliothèque principale est focalisée uniquement sur la couche vue, ce qui facilite l'intégration avec d'autres bibliothèques ou projets existants. Visual Studio Code (VS Code) est un éditeur de code léger mais puissant qui offre un excellent support pour le développement Vue.js grâce à ses extensions.

Ce guide vous aidera à installer Vue.js, configurer VS Code et commencer à développer des applications Vue.js modernes et efficaces.

## Prérequis

Avant de commencer, assurez-vous que votre système répond aux exigences minimales :

-   **Système d'exploitation** : Windows, macOS ou Linux
-   **Espace disque** : Au moins 1 Go d'espace libre
-   **Connaissances** : Bases en HTML, CSS et JavaScript

## Installation de Node.js et npm

Vue.js nécessite Node.js et npm (Node Package Manager) pour fonctionner. Voici comment les installer sur différents systèmes d'exploitation :

### Installation sur Windows

1.  Téléchargez le [programme d'installation de Node.js](https://nodejs.org/en/download/) pour Windows
2.  Exécutez le programme d'installation et suivez les instructions à l'écran
3.  Assurez-vous de cocher la case pour installer npm (généralement inclus par défaut)
4.  Vérifiez l'installation en ouvrant une invite de commande et en tapant :
    
    ```
    node --versionnpm --version
    
    ```
    

### Installation sur macOS

#### Option 1 : Téléchargement direct

1.  Téléchargez le [programme d'installation de Node.js](https://nodejs.org/en/download/) pour macOS
2.  Exécutez le programme d'installation et suivez les instructions à l'écran

#### Option 2 : Homebrew (recommandé)

1.  Si vous n'avez pas Homebrew, installez-le en exécutant :
    
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    
    ```
    
2.  Installez Node.js via Homebrew :
    
    ```bash
    brew install node
    
    ```
    
3.  Vérifiez l'installation :
    
    ```bash
    node --versionnpm --version
    
    ```
    

### Installation sur Linux

#### Debian et Ubuntu

```bash
sudo apt update
sudo apt install nodejs npm

```

#### Fedora, CentOS et RHEL

```bash
sudo dnf install nodejs

```

#### Arch Linux

```bash
sudo pacman -S nodejs npm

```

Vérifiez l'installation :

```bash
node --version
npm --version

```

## Installation de Visual Studio Code

1.  Téléchargez [Visual Studio Code](https://code.visualstudio.com/) pour votre système d'exploitation
2.  Exécutez le programme d'installation et suivez les instructions à l'écran
3.  Sur Linux, selon votre distribution, vous pouvez également installer VS Code via le gestionnaire de paquets (apt, snap, etc.)

## Configuration de VS Code pour Vue.js

1.  Ouvrez VS Code
    
2.  Accédez à l'onglet Extensions (Ctrl+Shift+X ou Cmd+Shift+X sur macOS)
    
3.  Recherchez et installez les extensions suivantes :
    
    -   **Volar** (Vue Language Features) - Support officiel pour Vue 3
    -   **Vue VSCode Snippets**
    -   **ESLint**
    -   **Prettier - Code formatter**
4.  Configuration recommandée pour VS Code :
    
    -   Ouvrez les paramètres (Ctrl+, ou Cmd+, sur macOS)
    -   Ajoutez les paramètres suivants (en JSON) :
    
    ```json
    {
      "editor.formatOnSave": true,
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "volar.autoCompleteRefs": true,
      "eslint.validate": ["javascript", "javascriptreact", "vue"],
      "[vue]": {
        "editor.defaultFormatter": "Vue.volar"
      }
    }
    
    ```

## Installation de Vue.js

### Installation de Vue CLI

Vue CLI est un outil standard pour le développement Vue.js qui fournit une interface en ligne de commande pour créer et gérer des projets Vue.

```bash
# Installation globale avec npm
npm install -g @vue/cli

# Vérifier l'installation
vue --version

```

### Installation avec Vite

Vite est un outil de développement moderne qui offre une expérience de développement plus rapide que Vue CLI. Recommandé pour Vue 3.

```bash
# Installation avec npm
npm create vite@latest

```

### Installation avec Nuxt.js

Nuxt.js est un framework basé sur Vue.js pour créer des applications universelles ou statiques.

```bash
# Installation avec npm
npx nuxi init <nom-du-projet>

```

## Création d'un nouveau projet Vue.js

### Création avec Vue CLI

```bash
# Créer un projet interactif
vue create mon-projet-vue

# Dans l'assistant interactif, vous pouvez choisir :
# - Vue 2 ou Vue 3
# - Support TypeScript
# - Router, Vuex, CSS pré-processeurs, linting, testing, etc.

```

### Création avec Vite

```bash
# Créer un projet avec Vite
npm create vite@latest mon-projet-vue -- --template vue

# Ou pour TypeScript :
npm create vite@latest mon-projet-vue -- --template vue-ts

```

### Création avec Nuxt.js

```bash
# Créer un projet Nuxt 3
npx nuxi init mon-projet-nuxt

# Puis installer les dépendances
cd mon-projet-nuxt
npm install

```

## Structure d'un projet Vue.js

### Structure Vue CLI

```
mon-projet-vue/
├── node_modules/         # Dépendances du projet
├── public/               # Fichiers statiques non traités
│   ├── favicon.ico       # Icône du site
│   └── index.html        # Template HTML
├── src/                  # Code source principal
│   ├── assets/           # Ressources statiques (images, styles, etc.)
│   ├── components/       # Composants Vue réutilisables
│   ├── router/           # Configuration du routeur (si activé)
│   ├── store/            # Configuration de Vuex (si activé)
│   ├── views/            # Composants de page
│   ├── App.vue           # Composant racine
│   └── main.js           # Point d'entrée JavaScript
├── tests/                # Tests unitaires et e2e (si activés)
├── .eslintrc.js          # Configuration ESLint
├── .gitignore            # Fichiers ignorés par Git
├── babel.config.js       # Configuration Babel
├── jest.config.js        # Configuration Jest (si tests unitaires activés)
├── package.json          # Dépendances et scripts npm
├── README.md             # Documentation du projet
└── vue.config.js         # Configuration Vue CLI (optionnel)

```

### Structure Vite

```
mon-projet-vue/
├── node_modules/         # Dépendances du projet
├── public/               # Fichiers statiques non traités
│   └── favicon.ico       # Icône du site
├── src/                  # Code source principal
│   ├── assets/           # Ressources statiques (images, styles, etc.)
│   ├── components/       # Composants Vue réutilisables
│   ├── App.vue           # Composant racine
│   └── main.js           # Point d'entrée JavaScript
├── index.html            # Template HTML principal (différent de Vue CLI)
├── .eslintrc.js          # Configuration ESLint (si configuré)
├── .gitignore            # Fichiers ignorés par Git
├── package.json          # Dépendances et scripts npm
├── README.md             # Documentation du projet
└── vite.config.js        # Configuration Vite

```

### Structure Nuxt.js

```
mon-projet-nuxt/
├── .nuxt/                # Fichiers générés (ignorés par Git)
├── assets/               # Ressources statiques (CSS, images)
├── components/           # Composants Vue réutilisables
├── layouts/              # Layouts de l'application
├── middleware/           # Middleware de routes
├── node_modules/         # Dépendances du projet
├── pages/                # Pages de l'application (génère automatiquement les routes)
├── plugins/              # Plugins Vue
├── public/               # Fichiers statiques non traités
├── server/               # Middleware et routes API côté serveur
├── stores/               # Stores Pinia
├── app.vue               # Composant racine
├── nuxt.config.js        # Configuration Nuxt.js
├── package.json          # Dépendances et scripts npm
├── tsconfig.json         # Configuration TypeScript (si utilisé)
└── README.md             # Documentation du projet

```

## Fonctionnalités essentielles de Vue.js

### Composants

Les composants sont la base de Vue.js. Ils sont des instances Vue réutilisables avec un nom.

**Exemple de composant (Vue 3 avec Composition API) :**

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="increment">Compteur: {{ count }}</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Props
defineProps({
  msg: String
});

// État réactif
const count = ref(0);

// Méthodes
const increment = () => {
  count.value++;
};
</script>

<style scoped>
.hello {
  padding: 20px;
  background-color: #f5f5f5;
}
</style>

```

### Directives

Les directives sont des attributs spéciaux avec le préfixe `v-` qui appliquent un comportement réactif au DOM.

-   `v-bind` ou `:` : Lie un attribut HTML à une expression JavaScript
-   `v-on` ou `@` : Attache un écouteur d'événement à un élément
-   `v-if`, `v-else-if`, `v-else` : Affiche conditionnellement un élément
-   `v-for` : Rend une liste d'éléments basée sur un tableau
-   `v-model` : Crée une liaison bidirectionnelle sur les éléments de formulaire

### Routage

Vue Router est la bibliothèque de routage officielle pour Vue.js.

**Configuration de base (Vue 3) :**

```js
import { createRouter, createWebHistory } from 'vue-router';
import Home from './views/Home.vue';
import About from './views/About.vue';

const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About }
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;

```

### Gestion d'état

#### Avec Pinia (recommandé pour Vue 3)

```js
// stores/counter.js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});

// Dans un composant
import { useCounterStore } from '@/stores/counter';

const counterStore = useCounterStore();
counterStore.increment();

```

#### Avec Vuex (Vue 2 et Vue 3)

```js
// store/index.js
import { createStore } from 'vuex';

export default createStore({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++;
    }
  },
  actions: {
    increment(context) {
      context.commit('increment');
    }
  }
});

// Dans un composant
this.$store.dispatch('increment');

```

## Exécution de votre application

### Serveur de développement

```bash
# Avec Vue CLI
npm run serve

# Avec Vite
npm run dev

# Avec Nuxt.js
npm run dev

```

Par défaut, l'application sera accessible à l'adresse http://localhost:8080 (Vue CLI), http://localhost:5173 (Vite) ou http://localhost:3000 (Nuxt.js).

### Construction pour la production

```bash
# Avec Vue CLI ou Vite
npm run build

# Avec Nuxt.js
npm run build

```

Le résultat de la construction sera placé dans le dossier `dist` (Vue CLI/Vite) ou `.output` (Nuxt.js).

## Débogage avec VS Code

### Configuration du débogage

1.  Créez un fichier `.vscode/launch.json` à la racine de votre projet :

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Démarrer Chrome avec Vue DevTools",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}/src",
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*"
      }
    }
  ]
}

```

2.  Ajustez l'URL selon votre configuration (8080 pour Vue CLI, 5173 pour Vite, 3000 pour Nuxt.js)

### Utilisation du débogage

1.  Démarrez votre serveur de développement (`npm run serve` ou équivalent)
2.  Définissez des points d'arrêt dans VS Code
3.  Appuyez sur F5 pour lancer le débogueur
4.  Chrome s'ouvrira et s'arrêtera aux points d'arrêt définis

### Vue DevTools

1.  Installez l'extension [Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) pour Chrome ou Firefox
2.  Ouvrez les DevTools dans votre navigateur et accédez à l'onglet "Vue"
3.  Explorez la hiérarchie des composants, les propriétés, les événements et l'état Vuex/Pinia

## Extensions VS Code recommandées

1.  **Volar** : Support officiel de Vue 3 (recommandé)
2.  **Vue VSCode Snippets** : Snippets pour Vue.js
3.  **ESLint** : Linting du code
4.  **Prettier** : Formatage du code
5.  **Vue Peek** : Navigation rapide des composants Vue
6.  **Auto Import** : Import automatique des modules
7.  **Path Intellisense** : Autocomplétion des chemins de fichiers
8.  **Tailwind CSS IntelliSense** : Si vous utilisez Tailwind CSS
9.  **i18n Ally** : Si vous utilisez l'internationalisation
10.  **Vetur** : Pour Vue 2 (utilisez Volar pour Vue 3)

## Astuces et raccourcis

### Raccourcis VS Code utiles

-   `Ctrl+Space` (ou `Cmd+Space` sur macOS) : Suggestions de code
-   `Alt+Enter` (ou `Option+Enter` sur macOS) : Actions rapides
-   `Ctrl+.` (ou `Cmd+.` sur macOS) : Refactoring rapide
-   `Ctrl+P` (ou `Cmd+P` sur macOS) : Navigation rapide entre fichiers
-   `Ctrl+Shift+P` (ou `Cmd+Shift+P` sur macOS) : Palette de commandes

### Snippets Vue utiles

-   `vbase` : Squelette de composant Vue
-   `vscript` : Bloc script
-   `vfor` : Directive v-for
-   `vif` : Directive v-if
-   `vmodel` : Directive v-model
-   `vemit` : Déclaration d'événements avec defineEmits()

## Génération de composants

### Avec Vue CLI

```bash
vue generate component MonComposant

```

### Outils tiers

Plusieurs CLI peuvent vous aider à générer des composants et du code :

```bash
# Plop.js (personnalisable)
npm install -g plop

# Générateur de Nuxt (pour Nuxt.js)
npx nuxi add component MonComposant

```

## Tests unitaires et e2e

### Tests unitaires avec Jest et Vue Test Utils

**Installation (si non inclus dans la configuration du projet) :**

```bash
npm install --save-dev jest @vue/test-utils vue-jest babel-jest

```

**Exemple de test (composant Counter.vue) :**

```js
import { mount } from '@vue/test-utils';
import Counter from '@/components/Counter.vue';

describe('Counter.vue', () => {
  it('incrémente la valeur lorsque le bouton est cliqué', async () => {
    const wrapper = mount(Counter);
    await wrapper.find('button').trigger('click');
    expect(wrapper.find('.count').text()).toContain('1');
  });
});

```

**Exécution des tests :**

```bash
npm run test:unit

```

### Tests e2e avec Cypress

**Installation (si non inclus dans la configuration du projet) :**

```bash
npm install --save-dev cypress

```

**Exemple de test e2e :**

```js
describe('Exemple de test e2e', () => {
  it('visite la page d\'accueil et vérifie le contenu', () => {
    cy.visit('/');
    cy.contains('h1', 'Bienvenue');
    cy.get('button').click();
    cy.get('.count').should('contain', '1');
  });
});

```

**Exécution des tests :**

```bash
npm run test:e2e

```

## Déploiement

### Déploiement statique (SPA)

Pour déployer une application Vue.js comme site statique (Single Page Application) :

1.  **Construire l'application :**
    
    ```bash
    npm run build
    
    ```
    
2.  **Options de déploiement :**
    
    -   **Netlify :**
        
        -   Connectez votre dépôt Git à Netlify
        -   Configurez la commande de build : `npm run build`
        -   Configurez le répertoire de publication : `dist`
    -   **Vercel :**
        
        -   Installez Vercel CLI : `npm i -g vercel`
        -   Déployez : `vercel`
    -   **GitHub Pages :**
        
        -   Configurez `vue.config.js` (Vue CLI) :
            
            ```js
            module.exports = {  publicPath: process.env.NODE_ENV === 'production' ? '/nom-repo/' : '/'}
            
            ```
            
        -   Ou `vite.config.js` (Vite) :
            
            ```js
            export default {  base: process.env.NODE_ENV === 'production' ? '/nom-repo/' : '/'}
            
            ```
            

### Déploiement SSR/SSG (Nuxt.js)

Pour Nuxt.js, vous avez plusieurs options de rendu :

1.  **SSR (Server-Side Rendering) :**
    
    ```bash
    npm run build
    node .output/server/index.mjs
    
    ```
    
2.  **SSG (Static Site Generation) :**
    
    ```bash
    # Dans nuxt.config.js, configurez :
    export default {
      ssr: true,
      target: 'static'
    }
    
    # Puis générez le site statique :
    npm run generate
    
    ```
    
3.  **Options de déploiement :**
    
    -   Vercel, Netlify : Détection automatique de Nuxt.js
    -   Plateforme Node.js (Heroku, DigitalOcean, etc.) pour SSR

## Résolution des problèmes courants

### Erreur "Module not found"

**Problème :** Impossible de trouver un module lors de l'importation.

**Solutions :**

-   Vérifiez que le module est installé : `npm install <nom-module>`
-   Vérifiez les chemins d'importation (sensibles à la casse)
-   Pour les alias dans Vue CLI, vérifiez `vue.config.js`
-   Pour Vite, vérifiez `vite.config.js` et les alias configurés

### Hot-Reload ne fonctionne pas

**Solutions :**

-   Redémarrez le serveur de développement
-   Vérifiez que vous n'avez pas d'erreurs de syntaxe
-   Vérifiez les conflits avec des extensions de navigateur
-   Pour Vue 3 + Vite, essayez `vite --force`

### Erreurs de compilation TypeScript

**Solutions :**

-   Vérifiez les versions de TypeScript et Vue
-   Assurez-vous que `tsconfig.json` est correctement configuré
-   Pour Vue 3, utilisez Volar au lieu de Vetur
-   Utilisez les types corrects pour les props et les refs

### Performances lentes en développement

**Solutions :**

-   Utilisez Vite au lieu de Vue CLI pour un développement plus rapide
-   Limitez l'utilisation des watchers complexes
-   Utilisez la mise en cache des composants avec `v-once` si approprié
-   Évitez trop de calculs dans les computed properties

### Conflit entre Vetur et Volar

**Solution :**

-   Désactivez Vetur si vous utilisez Vue 3 avec Volar
-   Dans VS Code, allez dans Extensions, cliquez sur Vetur, et sélectionnez "Disable" ou "Disable (Workspace)"

## Ressources complémentaires

### Documentation officielle

-   [Vue.js](https://vuejs.org/guide/introduction.html)
-   [Vue Router](https://router.vuejs.org/)
-   [Pinia](https://pinia.vuejs.org/)
-   [Vuex](https://vuex.vuejs.org/)
-   [Nuxt.js](https://nuxt.com/docs)
-   [Vite](https://vitejs.dev/guide/)
-   [Vue Test Utils](https://test-utils.vuejs.org/)
-   [Volta](https://docs.volta.sh/guide/)

### Tutoriels et cours

-   [Vue Mastery](https://www.vuemastery.com/)
-   [Vue School](https://vueschool.io/)
-   [Academind Vue Course](https://academind.com/learn/vue-js/)
-   [Udemy Vue JS Course](https://www.udemy.com/topic/vue-js/)

### Bibliothèques et outils

-   [Awesome Vue](https://github.com/vuejs/awesome-vue) - Liste de ressources Vue
-   [Vue Use](https://vueuse.org/) - Collection de composants utilitaires pour Vue
-   [Prime Vue](https://primevue.org/) - Bibliothèque de composants UI
-   [Vuetify](https://vuetifyjs.com/) - Framework Material Design pour Vue
-   [Quasar](https://quasar.dev/) - Framework Vue pour applications web, mobiles et de bureau
