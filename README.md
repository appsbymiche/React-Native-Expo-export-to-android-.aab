# React-Native-Expo-export-to-android-.aab Windows
Explains how to export an expo project to a .aab file

Step 1. Generate a keystore

1. Open cmd as administrator and enter "keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000"
2. Enter a password, firstname, lastname and org names.
3. enter yes
4. Your key should be generated in "C:\Windows\System32\" as "my-upload-key.keystore". Copy it to a folder of your choice.

Step 2. Edit android/gradle.properties file

1. In your expo project, go to the android folder and edit the gradle.properties file.
2. Add at the bottom of the file (remove the comments): 

MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore     # Path to the "keystore" file
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias                # Replace with value of the `keystore.keyAlias` field in the credentials.json file
MYAPP_UPLOAD_STORE_PASSWORD=*****                  # Replace with value of the `keystore.password` field in the credentials.json file
MYAPP_UPLOAD_KEY_PASSWORD=*****                    # Replace with value of the `keystore.keyPassword` field in the credentials.json file

3. Open android/app/build.gradle and update signingConfigs and buildTypes block: signingConfigs {
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
            signingConfig signingConfigs.release
        }
    }

Step 3.

1. Open your expo project in Visual Studio abd in the Visual Studio terminal cd to your project's android folder.
2. Enter "./gradlew bundleRelease" in the Visual Studio terminal.
3. Your .aab file will be located here "android/app/build/outputs/bundle/release/app-release.aab"
