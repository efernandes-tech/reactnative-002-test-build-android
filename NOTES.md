# Notes

- Run:

```bash
cd TestBuildAndroid

npm install

npm start
```

- Build APK:

```bash
# CMD (admin)
cd C:\Program Files\Java\jdkx.x.x_x\bin

keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

```
copy my-upload-key.keystore
paste android/app/my-upload-key.keystore
```

```
# file: android/gradle.properties
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=*****
MYAPP_UPLOAD_KEY_PASSWORD=*****
```

```
# file: android/app/build.gradle
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
                storeFile file(MYAPP_UPLOAD_STORE_FILE)
                storePassword MYAPP_UPLOAD_STORE_PASSWORD
                keyAlias MYAPP_UPLOAD_KEY_ALIAS
                keyPassword MYAPP_UPLOAD_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

```bash
# CMD (admin)
npx react-native build-android --mode=release

npm run android -- --mode="release"
```

```
# APK:
...\reactnative-002-test-build-android\TestBuildAndroid\android\app\build\outputs\apk\release
```

# Links

- Init

    https://reactnative.dev/docs/getting-started-without-a-framework

- APK

    https://reactnative.dev/docs/signed-apk-android

# Bugs Fixes

- React Native Error : java.io.UncheckedIOException: Could not move temporary workspace

    https://stackoverflow.com/questions/78384724/react-native-error-java-io-uncheckedioexception-could-not-move-temporary-work

    You need to change the gradle version. your price is 8.6 or 8.7. Lower it to 8.5

    YourApp/android/gradle/wrapper/gradle-wrapper.properties
