# Guide signature d'une application Flutter


## 1.Fichier key.properties

Création du fichier `key.properties` dans `android/`

```
storePassword=<mot de passe du keystore>
keyPassword=<mot de passe de la clé>
keyAlias=key
storeFile=<chemin absolu vers votre fichier keystore, ex: /home/utilisateur/key.jks>
```

## 2.Modification build.gradle app

Dans `android/app/build.gradle`

```
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android{
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
            storePassword keystoreProperties['storePassword']
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

## 3.Génératio de l'APK

flutter build apk --release