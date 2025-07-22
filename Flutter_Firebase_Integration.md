# Guide Intégration Firebase à une application Flutter

## 1.Modifier le package name (optionnel)

Ajout du package `change_app_package_name` pour facilité la modification du nom de package.
```
flutter pub add -d change_app_package_name
```

Executer la commande suivante:
```
flutter pub run change_app_package_name:main com.example.mon_app
```

## 2.Télécharger et déposer le fichier google-service.json

Sur l'interface Firebase, récupérer le fichier `google-service.json` et le placer sous `android/app`.

## 3.Ajout du SDK

Dans `android/build.gradle`

```
plugins{
  // Add the dependency for the Google services Gradle plugin
  id 'com.google.gms.google-services' version '4.4.2' apply false
}
```


Dans `android/app/build.gradle`

```
dependencies {
    implementation platform('com.google.firebase:firebase-bom:33.12.0')
    implementation 'com.google.firebase:firebase-analytics'
}
```

## 4.Synchronisation gradle

```
cd android
./gradlew.bat --refresh-dependencies
```