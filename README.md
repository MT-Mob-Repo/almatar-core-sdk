# Almatar Core SDK

## Overview
Easy way to integrate with the Almatar system to launch the flights and hotels reservation flow.

## Installation Instructions

1. Open your `AndroidManifest.xml` file and make sure you add a valid google API_KEY:

    ```xml
    <meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="<replace with your google API KEY>" />
    ```

2. Open your `gradle.properties` file and add the authToken provided by Almatar:

    ```properties
    authToken=replace_with_your_auth_token
    ```

3. Open your `settings.gradle` file and add the following:

    ```groovy
    dependencyResolutionManagement {
        repositories {
            maven {
                url 'https://jitpack.io'
                credentials { username authToken }
            }
        }
    }
    ```

4. Open your `build.gradle` (Module: app) file and add the following dependency, then sync your project:

    ```groovy
    implementation 'com.github.almatar-core:almatar-sdk:replace_with_version'
    ```

5. Open your application class and initialize the Almatar SDK:

    ```kotlin
    override fun onCreate() {
        super.onCreate()
        AlmatarAppInitializer.init(this)
    }
    ```

Now you can launch Almatar flight flow by calling this function:
```kotlin
AlmatarAppInitializer.launchFlights()
```

Or launch Almatar hotel flow by calling this function:
```kotlin
AlmatarAppInitializer.launchHotels()
```
