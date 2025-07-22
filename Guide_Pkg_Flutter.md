
# Tutoriel: Création et utilisation d'un package Flutter via GitLab

Ce guide vous explique comment créer un package Flutter personnalisé, l'héberger sur GitLab, et l'intégrer dans vos projets en utilisant des tags pour la gestion des versions.

## Table des matières

1.  [Création d'un package Flutter](#cr%C3%A9ation-dun-package-flutter)
2.  [Structure et développement du package](#structure-et-d%C3%A9veloppement-du-package)
3.  [Publication sur GitLab](#publication-sur-gitlab)
4.  [Gestion des versions avec tags](#gestion-des-versions-avec-tags)
5.  [Utilisation du package dans un projet Flutter](#utilisation-du-package-dans-un-projet-flutter)
6.  [Mise à jour du package](#mise-%C3%A0-jour-du-package)
7.  [Bonnes pratiques](#bonnes-pratiques)

## Création d'un package Flutter

Commencez par créer un nouveau package Flutter:

```bash
# Création d'un package Flutter
flutter create --template=package mon_package_flutter
cd mon_package_flutter

```

Cette commande génère la structure de base d'un package Flutter avec les dossiers et fichiers suivants:

-   `lib/`: Contient le code source de votre package
-   `test/`: Contient les tests unitaires
-   `pubspec.yaml`: Définit les métadonnées et dépendances du package
-   `README.md`: Documentation de votre package
-   `LICENSE`: Licence du package
-   `CHANGELOG.md`: Journal des modifications

## Structure et développement du package

### Modifier le fichier pubspec.yaml

Mettez à jour le fichier `pubspec.yaml` avec les informations de votre package:

```yaml
name: mon_package_flutter
description: Description de mon package Flutter
version: 0.0.1
homepage: https://gitlab.com/votre-compte/mon_package_flutter

environment:
  sdk: ">=2.17.0 <3.0.0"
  flutter: ">=3.0.0"

dependencies:
  flutter:
    sdk: flutter
  # Ajoutez ici vos autres dépendances

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^2.0.0

# Définit les plateformes supportées
platforms:
  android: null
  ios: null
  web: null

```

### Développer le code du package

Créez vos classes et fonctions dans le dossier `lib/`. Par exemple:

```dart
// lib/mon_package_flutter.dart
library mon_package_flutter;

export 'src/composant.dart';

```

```dart
// lib/src/composant.dart
import 'package:flutter/material.dart';

class MonComposant extends StatelessWidget {
  final String texte;
  
  const MonComposant({
    Key? key,
    required this.texte,
  }) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return Container(
      padding: const EdgeInsets.all(16.0),
      decoration: BoxDecoration(
        color: Colors.blue.shade100,
        borderRadius: BorderRadius.circular(8.0),
      ),
      child: Text(texte, style: const TextStyle(fontSize: 16.0)),
    );
  }
}

```

### Tester votre package

Créez des tests unitaires dans le dossier `test/`:

```dart
// test/mon_package_flutter_test.dart
import 'package:flutter_test/flutter_test.dart';
import 'package:mon_package_flutter/mon_package_flutter.dart';

void main() {
  testWidgets('Test de MonComposant', (WidgetTester tester) async {
    await tester.pumpWidget(
      const MaterialApp(
        home: Scaffold(
          body: MonComposant(texte: 'Test'),
        ),
      ),
    );
    
    expect(find.text('Test'), findsOneWidget);
  });
}

```

## Publication sur GitLab

### Initialiser Git

```bash
git init
git add .
git commit -m "Initial commit"

```

### Créer un nouveau projet sur GitLab

1.  Connectez-vous à votre compte GitLab
2.  Créez un nouveau projet (repository)
3.  Choisissez un nom pour votre projet (par exemple, "mon_package_flutter")
4.  Sélectionnez les options de confidentialité appropriées (public ou privé)
5.  Créez le projet

### Pousser votre code vers GitLab

```bash
git remote add origin https://gitlab.com/votre-compte/mon_package_flutter.git
git push -u origin master

```

## Gestion des versions avec tags

Les tags Git permettent de marquer des versions spécifiques de votre package.

### Créer un tag pour une nouvelle version

```bash
# Assurez-vous que la version dans pubspec.yaml correspond (par exemple, 0.1.0)
git tag -a v0.1.0 -m "Version 0.1.0"
git push origin v0.1.0

```

### Bonnes pratiques pour les versions

Suivez le versionnement sémantique (SemVer):

-   **MAJEUR.MINEUR.CORRECTIF** (par exemple, 1.2.3)
-   Incrémentez MAJEUR lorsque vous introduisez des changements incompatibles
-   Incrémentez MINEUR pour les nouvelles fonctionnalités compatibles
-   Incrémentez CORRECTIF pour les corrections de bugs compatibles

N'oubliez pas de mettre à jour le fichier `CHANGELOG.md` pour documenter les changements entre les versions.

### Gérer les tags automatiquement via vos commit
A la racine du projet, ajoutez le fichier `.gitlab-ci.yml`  suivant:
```yaml
stages:

- tag

  

create_tag:

stage: tag

image: alpine/git

script:

- git config --global user.email "<git-url>>"

- git config --global user.name "GitLab CI"

- TAG=$(echo "$CI_COMMIT_MESSAGE" | sed -n "s/.*\([0-9]\+\.[0-9]\+\.[0-9]\+\).*/\1/p" || echo "")

- DESCRIPTION=$(echo "$CI_COMMIT_MESSAGE" | sed -n 's/^[0-9]\+\.[0-9]\+\.[0-9]\+\s*-\s*//p' || echo "")

- |

if [[ ! -z "$TAG" ]]; then

TAG="v$TAG"

echo "Création du tag $TAG avec description : $DESCRIPTION"

# Créer le tag localement

git tag -a "$TAG" -m "$DESCRIPTION"

  

# Authentification GitLab via le Token

git push https://oauth2:${CI_JOB_TOKEN}@repo.git "$TAG"

else

echo "Aucun numéro de version trouvé dans le message de commit."

fi

only:

- main
```

Ce fichier permettra la création d'un nouveau tag à chaque commit, à partir du moment où le titre comprend un numéro de version valide. Egalement si le commit contient une description, cette dernière sera ajouté dans la description du tag.

## Utilisation du package dans un projet Flutter

Pour utiliser votre package hébergé sur GitLab dans un projet Flutter, ajoutez-le aux dépendances dans le fichier `pubspec.yaml` du projet:

```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # Utilisation du package via GitLab avec un tag spécifique
  mon_package_flutter:
    git:
      url: https://gitlab.com/votre-compte/mon_package_flutter.git
      ref: v0.1.0  # Tag de version

```

Pour un dépôt privé, vous pouvez utiliser un token d'accès:

```yaml
dependencies:
  mon_package_flutter:
    git:
      url: https://oauth2:YOUR_ACCESS_TOKEN@gitlab.com/votre-compte/mon_package_flutter.git
      ref: v0.1.0

```

Exécutez ensuite:

```bash
flutter pub get

```

### Utilisation dans le code

```dart
import 'package:mon_package_flutter/mon_package_flutter.dart';

class MonEcran extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Demo')),
      body: const Center(
        child: MonComposant(texte: 'Package personnalisé!'),
      ),
    );
  }
}

```

## Mise à jour du package

### Modification du package

1.  Faites vos modifications dans le code du package
2.  Mettez à jour la version dans `pubspec.yaml`
3.  Mettez à jour le `CHANGELOG.md`
4.  Poussez les modifications vers GitLab

```bash
git add .
git commit -m "Ajout de nouvelles fonctionnalités"
git push origin master

```

### Création d'un nouveau tag 
⚠️ Dans le cas où les tag ne sont pas générés automatiquement par les commit.

```bash
git tag -a v0.2.0 -m "Version 0.2.0"
git push origin v0.2.0

```

### Mise à jour du package dans votre projet

Modifiez le fichier `pubspec.yaml` de votre projet pour utiliser la nouvelle version:

```yaml
dependencies:
  mon_package_flutter:
    git:
      url: https://gitlab.com/votre-compte/mon_package_flutter.git
      ref: v0.2.0  # Nouveau tag

```

Puis exécutez:

```bash
flutter pub get

```

## Bonnes pratiques

### Documentation

Assurez-vous de bien documenter votre package:

-   Mettez à jour le `README.md` avec:
    -   Description claire du package
    -   Instructions d'installation
    -   Exemples d'utilisation
    -   API détaillée
-   Utilisez des commentaires dartdoc dans votre code

### Sécurité pour les dépôts privés

Pour les dépôts privés, utilisez des tokens d'accès ou des clés SSH plutôt que des mots de passe.