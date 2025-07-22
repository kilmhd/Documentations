
# Guide Complet d'Installation et d'Utilisation de Flutter avec Visual Studio Code

## Table des matières

1.  [Introduction](#introduction)
2.  [Prérequis](#pr%C3%A9requis)
3.  [Installation de Flutter](#installation-de-flutter)
    -   [Windows](#installation-sur-windows)
    -   [macOS](#installation-sur-macos)
    -   [Linux](#installation-sur-linux)
4.  [Installation de Visual Studio Code](#installation-de-visual-studio-code)
5.  [Configuration de VS Code pour Flutter](#configuration-de-vs-code-pour-flutter)
6.  [Utilisation de Flutter Version Management (FVM)](#utilisation-de-flutter-version-management-fvm)
    -   [Installation de FVM](#installation-de-fvm)
    -   [Configuration de FVM avec VS Code](#configuration-de-fvm-avec-vs-code)
    -   [Commandes FVM essentielles](#commandes-fvm-essentielles)
    -   [Utilisation de FVM dans un projet](#utilisation-de-fvm-dans-un-projet)
    -   [Bonnes pratiques avec FVM](#bonnes-pratiques-avec-fvm)
7.  [Création d'un nouveau projet Flutter](#cr%C3%A9ation-dun-nouveau-projet-flutter)
8.  [Structure d'un projet Flutter](#structure-dun-projet-flutter)
9.  [Exécution de votre application](#ex%C3%A9cution-de-votre-application)
10.  [Débogage avec VS Code](#d%C3%A9bogage-avec-vs-code)
11.  [Extensions VS Code recommandées](#extensions-vs-code-recommand%C3%A9es)
12.  [Astuces et raccourcis](#astuces-et-raccourcis)
13.  [Résolution des problèmes courants](#r%C3%A9solution-des-probl%C3%A8mes-courants)
14.  [Ressources complémentaires](#ressources-compl%C3%A9mentaires)

## Introduction

Flutter est un framework d'interface utilisateur open-source développé par Google pour créer des applications nativement compilées pour mobile, web et desktop à partir d'une base de code unique. Visual Studio Code (VS Code) est l'un des éditeurs de code les plus populaires pour le développement Flutter grâce à son excellent support via des extensions dédiées.

Ce guide vous aidera à installer Flutter, configurer VS Code et commencer à développer des applications Flutter.

## Prérequis

Avant de commencer, assurez-vous que votre système répond aux exigences minimales :

-   **Système d'exploitation** : Windows 10 ou version ultérieure (64 bits), macOS 10.14 (Mojave) ou version ultérieure, ou Linux
-   **Espace disque** : Au moins 2,5 Go (sans compter l'IDE/outils)
-   **Outils** : Git doit être disponible dans votre environnement

## Installation de Flutter

### Installation sur Windows

1.  Téléchargez le [dernier SDK Flutter](https://docs.flutter.dev/get-started/install/windows) (fichier zip)
2.  Extrayez le zip dans un emplacement de votre choix (évitez les chemins nécessitant des privilèges élevés comme `C:\Program Files\`)
3.  Ajoutez Flutter à votre PATH :
    -   Dans la recherche Windows, tapez "environnement" et sélectionnez "Modifier les variables d'environnement système"
    -   Dans l'onglet "Avancé", cliquez sur "Variables d'environnement"
    -   Dans "Variables système", sélectionnez "Path" et cliquez sur "Modifier"
    -   Cliquez sur "Nouveau" et ajoutez le chemin vers `flutter\bin` (par exemple : `C:\flutter\bin`)
    -   Cliquez sur "OK" pour fermer toutes les fenêtres

### Installation sur macOS

1.  Téléchargez le [dernier SDK Flutter](https://docs.flutter.dev/get-started/install/macos)
2.  Extrayez l'archive dans l'emplacement désiré :
    
    ```bash
    cd ~/developmentunzip ~/Downloads/flutter_macos_3.x.x-stable.zip
    
    ```
    
3.  Ajoutez Flutter à votre PATH :
    
    -   Pour Bash, modifiez `~/.bash_profile` ou `~/.bashrc`
    -   Pour Zsh, modifiez `~/.zshrc`
    
    ```bash
    export PATH="$PATH:$HOME/development/flutter/bin"
    
    ```
    
4.  Exécutez `source ~/.zshrc` (ou le fichier correspondant) pour appliquer les changements

### Installation sur Linux

1.  Téléchargez le [dernier SDK Flutter](https://docs.flutter.dev/get-started/install/linux)
2.  Extrayez l'archive tar :
    
    ```bash
    cd ~/developmenttar xf ~/Downloads/flutter_linux_3.x.x-stable.tar.xz
    
    ```
    
3.  Ajoutez Flutter à votre PATH :
    
    ```bash
    export PATH="$PATH:$HOME/development/flutter/bin"
    
    ```
    
4.  Ajoutez cette ligne à votre fichier `.bashrc` ou `.zshrc` pour rendre le changement permanent

### Vérification de l'installation

Après l'installation, vérifiez que tout est correctement installé en exécutant :

```bash
flutter doctor

```

Cette commande vérifie votre environnement et affiche un rapport sur l'état de votre installation Flutter. Suivez les instructions pour résoudre les problèmes identifiés.

## Installation de Visual Studio Code

1.  Téléchargez et installez [Visual Studio Code](https://code.visualstudio.com/)
2.  Suivez les instructions d'installation standard pour votre système d'exploitation

## Configuration de VS Code pour Flutter

1.  Ouvrez VS Code
2.  Accédez à l'onglet Extensions (Ctrl+Shift+X ou Cmd+Shift+X sur macOS)
3.  Recherchez "Flutter" et installez l'extension officielle "Flutter" de Dart DevTools
4.  Cette extension installera également automatiquement l'extension Dart

Une fois les extensions installées, redémarrez VS Code pour vous assurer que toutes les fonctionnalités sont correctement chargées.

## Utilisation de Flutter Version Management (FVM)

Flutter Version Management (FVM) est un outil qui permet de gérer facilement plusieurs versions de Flutter SDK sur un même système. Cela est particulièrement utile lorsque vous travaillez sur différents projets nécessitant des versions spécifiques de Flutter.

### Installation de FVM

#### Via Dart pub (recommandé)

1.  Assurez-vous que Dart SDK est installé et disponible dans votre PATH
2.  Exécutez la commande suivante :
    
    ```bash
    dart pub global activate fvm
    
    ```
    
3.  Assurez-vous que le répertoire des binaires pub est dans votre PATH :
    -   Windows : `%LOCALAPPDATA%\Pub\Cache\bin`
    -   macOS/Linux : `$HOME/.pub-cache/bin`

#### Via Homebrew (macOS uniquement)

```bash
brew tap leoafarias/fvm
brew install fvm

```

#### Via Chocolatey (Windows uniquement)

```bash
choco install fvm

```

#### Via Scoop (Windows uniquement)

```bash
scoop bucket add fvm https://github.com/leoafarias/fvm.git
scoop install fvm

```

### Configuration de FVM avec VS Code

1.  Installez l'extension "FVM" dans VS Code :
    
    -   Ouvrez VS Code
    -   Accédez à l'onglet Extensions (Ctrl+Shift+X ou Cmd+Shift+X sur macOS)
    -   Recherchez "FVM" et installez l'extension officielle
2.  Configuration des paramètres VS Code :
    
    -   Ouvrez les paramètres de VS Code (Ctrl+, ou Cmd+, sur macOS)
    -   Recherchez "flutter sdk path"
    -   Assurez-vous que le champ est vide ou configuré pour utiliser le chemin FVM
3.  Pour configurer VS Code pour utiliser automatiquement la version Flutter spécifiée par FVM dans chaque projet, ajoutez ces paramètres dans votre `settings.json` :
    

```json
{
  "dart.flutterSdkPath": ".fvm/flutter_sdk",
  "search.exclude": {
    "**/.fvm": true
  },
  "files.watcherExclude": {
    "**/.fvm": true
  }
}

```

### Commandes FVM essentielles

Voici les commandes les plus courantes de FVM :

-   **Installer une version spécifique** :
    
    ```bash
    fvm install 3.10.0
    
    ```
    
-   **Lister les versions installées** :
    
    ```bash
    fvm list
    
    ```
    
-   **Lister toutes les versions disponibles** :
    
    ```bash
    fvm releases
    
    ```
    
-   **Définir une version globale par défaut** :
    
    ```bash
    fvm global 3.10.0
    
    ```
    
-   **Utiliser une version spécifique (temporairement)** :
    
    ```bash
    fvm use 3.10.0
    
    ```
    
-   **Supprimer une version installée** :
    
    ```bash
    fvm remove 3.10.0
    
    ```
    

### Utilisation de FVM dans un projet

1.  **Initialiser FVM dans un projet existant** :
    
    ```bash
    cd mon_projet_flutter
    fvm use 3.10.0 --force
    
    ```
    
    L'option `--force` crée le projet s'il n'existe pas encore.
    
2.  **Configurer Git pour ignorer les fichiers FVM** : Ajoutez les lignes suivantes à votre fichier `.gitignore` :
    
    ```
    # FVM
    .fvm/flutter_sdk
    # Vous pouvez choisir de conserver le fichier de configuration dans le référentiel
    !.fvm/fvm_config.json
    
    ```
    
3.  **Exécuter des commandes Flutter avec FVM** : Au lieu d'utiliser `flutter`, préfixez vos commandes avec `fvm` :
    
    ```bash
    # Au lieu de
    flutter pub get
    
    # Utilisez
    fvm flutter pub get
    
    ```
    
4.  **Utilisation dans un CI/CD** : Pour les environnements CI/CD, vous pouvez configurer le workflow pour utiliser FVM :
    
    Exemple pour GitHub Actions :
    
    ```yaml
    jobs:
      build:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - uses: kuhnroyal/flutter-fvm-config-action@v1
          - uses: subosito/flutter-action@v2
            with:
              flutter-version: ${{ env.FLUTTER_VERSION }}
              channel: ${{ env.FLUTTER_CHANNEL }}
          - run: flutter pub get
    
    ```
    

### Bonnes pratiques avec FVM

1.  **Spécifier la version Flutter dans votre fichier README** : Indiquez clairement la version de Flutter nécessaire pour le projet.
    
2.  **Verrouiller la version dans le contrôle de source** : Assurez-vous que le fichier `.fvm/fvm_config.json` est inclus dans votre dépôt Git.
    
3.  **Migrer progressivement vers les nouvelles versions** : Utilisez FVM pour tester votre application avec des versions plus récentes avant de migrer officiellement.
    
4.  **Gérer les versions par projet** : Utilisez des versions spécifiques pour chaque projet plutôt qu'une version globale.
    
5.  **Nettoyage régulier** : Supprimez périodiquement les anciennes versions inutilisées pour libérer de l'espace :
    
    ```bash
    fvm remove <version>
    
    ```
    
6.  **Utiliser les alias pour les versions** : FVM prend en charge des alias comme `stable`, `beta`, `dev`, `master` :
    
    ```bash
    fvm use stable
    
    ```
    

## Création d'un nouveau projet Flutter

Il existe plusieurs façons de créer un nouveau projet Flutter dans VS Code :

### Méthode 1 : Via la Palette de Commandes

1.  Ouvrez VS Code
2.  Appuyez sur `Ctrl+Shift+P` (ou `Cmd+Shift+P` sur macOS) pour ouvrir la palette de commandes
3.  Tapez "Flutter: New Application Project" et sélectionnez cette option
4.  Choisissez un dossier où créer le projet
5.  Entrez un nom pour votre projet (utilisez des lettres minuscules et des underscores, ex: `mon_application_flutter`)

### Méthode 2 : Via le Terminal

1.  Ouvrez VS Code
    
2.  Ouvrez un terminal intégré avec ``Ctrl+` `` (ou ``Cmd+` `` sur macOS)
    
3.  Naviguez vers le dossier parent où vous souhaitez créer votre projet
    
4.  Exécutez la commande :
    
    ```bash
    flutter create mon_application_flutter
    
    ```
    
    Ou avec FVM :
    
    ```bash
    fvm flutter create mon_application_flutter
    
    ```
    
5.  Ouvrez le projet avec File > Open Folder (ou File > Open... sur macOS)
    

## Structure d'un projet Flutter

Voici la structure typique d'un projet Flutter nouvellement créé :

```
mon_application_flutter/
├── .dart_tool/           # Outils et configurations Dart (généré)
├── .fvm/                 # Configuration FVM (si utilisé)
│   ├── flutter_sdk/      # Lien symbolique vers le SDK Flutter (ignoré par Git)
│   └── fvm_config.json   # Configuration de la version Flutter
├── .idea/                # Configurations IntelliJ/Android Studio
├── android/              # Code spécifique à Android
├── build/                # Fichiers de construction (généré)
├── ios/                  # Code spécifique à iOS
├── lib/                  # Code source Dart principal
│   └── main.dart         # Point d'entrée de l'application
├── linux/                # Code spécifique à Linux (si activé)
├── macos/                # Code spécifique à macOS (si activé)
├── test/                 # Tests unitaires et d'interface
├── web/                  # Code spécifique au Web (si activé)
├── windows/              # Code spécifique à Windows (si activé)
├── .gitignore            # Fichiers à ignorer par Git
├── .metadata             # Métadonnées du projet Flutter
├── analysis_options.yaml # Configuration des règles d'analyse Dart
├── pubspec.lock          # Versions verrouillées des dépendances
├── pubspec.yaml          # Dépendances et configuration du projet
└── README.md             # Documentation du projet

```

Les fichiers et dossiers les plus importants sont :

-   **lib/** : Contient votre code source Dart. C'est ici que vous passerez la plupart de votre temps.
-   **pubspec.yaml** : Définit les dépendances de votre projet, les assets et autres métadonnées.
-   **test/** : Contient les tests unitaires et d'interface de votre application.
-   **android/**, **ios/**, etc. : Contiennent le code spécifique à chaque plateforme.
-   **.fvm/** : Contient la configuration FVM et le lien vers le SDK Flutter spécifique au projet.

## Exécution de votre application

### Configuration d'un émulateur/simulateur

Avant d'exécuter votre application, vous devez configurer un appareil cible :

#### Android

1.  Installez [Android Studio](https://developer.android.com/studio)
2.  Ouvrez Android Studio et accédez à Tools > AVD Manager
3.  Cliquez sur "Create Virtual Device" et suivez les instructions

#### iOS (macOS uniquement)

1.  Installez Xcode depuis l'App Store
2.  Ouvrez Xcode et acceptez les conditions d'utilisation
3.  Pour ouvrir le simulateur : Xcode > Open Developer Tool > Simulator

#### Navigateur (pour le web)

-   Aucune configuration supplémentaire n'est nécessaire

### Exécution depuis VS Code

1.  Ouvrez votre projet Flutter dans VS Code
2.  Dans la barre d'état en bas de VS Code, vous verrez "No Device"
3.  Cliquez sur "No Device" pour sélectionner l'appareil cible (émulateur Android, simulateur iOS, Chrome, etc.)
4.  Plusieurs méthodes pour lancer l'application :
    -   Cliquez sur le bouton "Run" dans la barre de débogage
    -   Appuyez sur F5
    -   Ouvrez la palette de commandes (Ctrl+Shift+P) et tapez "Flutter: Run"
    -   Dans le terminal, naviguez vers le dossier du projet et exécutez :
        
        ```bash
        # Sans FVMflutter run# Avec FVMfvm flutter run
        
        ```
        

## Débogage avec VS Code

VS Code offre d'excellentes fonctionnalités de débogage pour Flutter :

### Démarrage du débogage

1.  Définissez des points d'arrêt en cliquant sur la marge à gauche du numéro de ligne
2.  Appuyez sur F5 ou cliquez sur le bouton "Run and Debug" dans l'onglet Run
3.  L'application se lancera en mode débogage

### Fonctionnalités de débogage

-   **Console de débogage** : Affiche les logs et les sorties de l'application
-   **Variables** : Examine les valeurs des variables pendant l'exécution
-   **Points d'arrêt** : Pause l'exécution à des lignes spécifiques
-   **Hot Reload** : Appuyez sur `Ctrl+F5` (ou `Cmd+F5` sur macOS) pour appliquer rapidement vos modifications
-   **Hot Restart** : Réinitialise l'état de l'application mais conserve la session de débogage (`Shift+Ctrl+F5` ou `Shift+Cmd+F5` sur macOS)

### Outils DevTools de Flutter

VS Code intègre Flutter DevTools, un ensemble puissant d'outils de performance et de débogage :

1.  Pendant que l'application est en cours d'exécution, cliquez sur "Open DevTools" dans la console de débogage
2.  Explorer les différents outils :
    -   **Widget Inspector** : Examinez la hiérarchie des widgets
    -   **Performance** : Analysez les performances de rendu
    -   **Memory** : Surveillez l'utilisation de la mémoire
    -   **Debugger** : Débogage avancé

## Extensions VS Code recommandées

Outre les extensions Flutter et Dart, voici quelques extensions utiles pour le développement Flutter :

1.  **Awesome Flutter Snippets** : Snippets de code Flutter courants
2.  **Flutter Widget Snippets** : Snippets pour créer rapidement des widgets
3.  **Pubspec Assist** : Aide à gérer les dépendances dans pubspec.yaml
4.  **Better Comments** : Améliore la lisibilité des commentaires
5.  **bloc** : Support pour le pattern BLoC (si vous utilisez cette architecture)
6.  **Error Lens** : Affiche les erreurs et avertissements en ligne
7.  **Git Graph** : Visualisez l'historique Git
8.  **Material Icon Theme** : Icônes de fichiers adaptées au développement Flutter
9.  **FVM** : Intégration de Flutter Version Management dans VS Code

## Astuces et raccourcis

### Raccourcis VS Code utiles pour Flutter

-   `Ctrl+Space` (ou `Cmd+Space` sur macOS) : Suggestions de code
-   `Alt+Enter` (ou `Option+Enter` sur macOS) : Actions rapides pour les corrections
-   `F1` ou `Ctrl+Shift+P` (ou `Cmd+Shift+P` sur macOS) : Palette de commandes
-   `Ctrl+P` (ou `Cmd+P` sur macOS) : Recherche rapide de fichiers
-   `Ctrl+Shift+F` (ou `Cmd+Shift+F` sur macOS) : Recherche globale
-   `Ctrl+.` (ou `Cmd+.` sur macOS) : Actions rapides pour le refactoring

### Commandes Flutter utiles

-   `fvm flutter pub get` : Récupère les dépendances du projet
-   `fvm flutter pub add [package]` : Ajoute une nouvelle dépendance
-   `fvm flutter clean` : Nettoie le dossier build
-   `fvm flutter build [platform]` : Construit l'application pour la distribution
-   `fvm flutter analyze` : Analyse le code pour détecter les problèmes
-   `fvm flutter test` : Exécute les tests

### Astuces pour le développement Flutter

1.  **Utilisez les snippets** : Tapez `stless` ou `stful` puis appuyez sur Tab pour créer rapidement des classes de widgets
2.  **Exploitez le refactoring** : Utilisez "Extract Widget" pour décomposer les widgets complexes
3.  **Utilisez les fonctionnalités d'organisation** : Region folding avec `// #region` et `// #endregion` pour organiser votre code
4.  **Utilisez des constantes pour les couleurs et les dimensions** : Créez un fichier de thème pour une cohérence visuelle
5.  **Activez le formatage automatique** : Dans VS Code, activez "Format On Save" pour un code toujours propre

## Résolution des problèmes courants

### Problème : Flutter Doctor signale des problèmes

**Solution** : Suivez les instructions spécifiques fournies par `flutter doctor`. Généralement, vous devrez exécuter des commandes supplémentaires comme `flutter doctor --android-licenses`.

### Problème : Hot Reload ne fonctionne pas

**Solutions** :

-   Vérifiez que vous avez sauvegardé le fichier (`Ctrl+S` ou `Cmd+S`)
-   Certains changements (comme l'ajout de nouvelles dépendances) nécessitent un Hot Restart (`Shift+Ctrl+F5`)
-   Redémarrez l'application si les modifications sont trop importantes

### Problème : Erreurs de dépendances

**Solutions** :

-   Exécutez `fvm flutter pub get` dans le terminal
-   Vérifiez que votre `pubspec.yaml` est correctement formaté
-   Vérifiez la compatibilité des versions des packages

### Problème : VS Code ne reconnaît pas Flutter

**Solutions** :

-   Vérifiez que l'extension Flutter est installée
-   Redémarrez VS Code
-   Vérifiez que Flutter est correctement installé avec `flutter doctor` ou `fvm flutter doctor`

### Problème : Erreurs avec FVM

**Solutions** :

-   **FVM n'est pas reconnu** : Vérifiez que FVM est correctement installé et dans votre PATH
-   **Erreur lors de l'installation d'une version** : Essayez avec `--force` ou vérifiez votre connexion internet
-   **VS Code n'utilise pas la bonne version** : Vérifiez les paramètres VS Code et assurez-vous que `dart.flutterSdkPath` est correctement configuré
-   **Permissions insuffisantes** : Exécutez la commande avec des privilèges élevés (sudo sur macOS/Linux, admin sur Windows)

### Problème : Erreurs de compilation pour une plateforme spécifique

**Solutions** :

-   Pour Android : Vérifiez la configuration dans `android/app/build.gradle`
-   Pour iOS : Vérifiez les paramètres dans Xcode
-   Assurez-vous que les SDK appropriés sont installés

## Ressources complémentaires

### Documentation officielle

-   [Site officiel de Flutter](https://flutter.dev/)
-   [Documentation Flutter](https://docs.flutter.dev/)
-   [Cookbook Flutter](https://docs.flutter.dev/cookbook)
-   [Documentation de Dart](https://dart.dev/guides)
-   [FVM - GitHub](https://github.com/leoafarias/fvm)
-   [FVM - Documentation](https://fvm.app/docs/getting_started/overview)

### Tutoriels et cours

-   [Flutter Codelabs](https://flutter.dev/docs/codelabs)
-   [Widget of the Week](https://www.youtube.com/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG) (YouTube)
-   [Flutter App Development Course](https://www.appbrewery.co/p/flutter-development-bootcamp-with-dart)

### Packages et ressources

-   [pub.dev](https://pub.dev/) : Dépôt officiel de packages Dart et Flutter
-   [Flutter Gems](https://fluttergems.dev/) : Collection de packages Flutter organisés par catégorie
-   [Flutter Icon](https://fluttericon.com/) : Générateur d'icônes personnalisées