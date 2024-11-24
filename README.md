# Almatar Core SDK

## Overview
Easy way to integrate with Almatar system to launch the flights and hotels reservation flow.

## Installation Instructions

1. Open your `AndroidManifest.xml` file and make sure you add a valid google API_KEY:

    ```xml
        <application>

              <meta-data
                android:name="com.google.android.geo.API_KEY"
                android:value="<replace with your google API KEY>" />

        />
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
    implementation 'com.github.MT-Mob-Repo:almatar-sdk:replace_with_version'
    ```

5. Open your application class and initialize Almatar SDK:

    ```kotlin
    override fun onCreate() {
        super.onCreate()
        AlmatarAppInitializer.init(application = this, almatarEnvironment = if (BuildConfig.DEBUG) AlmatarEnvironment.Development else AlmatarEnvironment.Production)
    }
    ```

Now you can launch Almatar flight flow by calling this function:
```kotlin
AlmatarAppInitializer.launchFlights(
                almatarFlowFinishedCallback = {
                    val intent = Intent(context, Splashscreen::class.java)
                    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_NEW_TASK)
                    context?.startActivity(intent)
                    (context as? ComponentActivity)?.finish()
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                startPaymentFlowImpl = { bookingId, amount, productType, callback ->
                    callback(
                        true, // payment status
                         "" // payment error message
                    )
                },
            )
```

Or launch Almatar hotel flow by calling this function:
```kotlin
AlmatarAppInitializer.launchHotels(
                almatarFlowFinishedCallback = {
                    val intent = Intent(context, Splashscreen::class.java)
                    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_NEW_TASK)
                    context?.startActivity(intent)
                    (context as? ComponentActivity)?.finish()
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                startPaymentFlowImpl = { bookingId, amount, productType, callback ->
                    callback(
                        true, // payment status
                         "" // payment error message
                    )
                },
            )
```

to launch Almatar trips history (completed and upcoming) flow call this function:
```kotlin
AlmatarAppInitializer.launchBookings(
                almatarFlowFinishedCallback = {
                    val intent = Intent(context, Splashscreen::class.java)
                    intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_NEW_TASK)
                    context?.startActivity(intent)
                    (context as? ComponentActivity)?.finish()
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                
            )
```

** Note: if you plan to use proguard rules, add the following to your app proguard file: 
```kotlin
# Keep all public classes in the com.almatar package (and subpackages)
-keep class com.almatar.** { *; }

# Keep all inner classes in the R$* (this is important for resources like drawables)
-keep class com.almatar.R$* { *; }

# Keep drawable resources (this includes images, shapes, etc.)
-keep class * implements android.graphics.drawable.Drawable { *; }
-keepclassmembers class * implements android.graphics.drawable.Drawable { *; }

# Keep all public methods
-keepclassmembers class com.almatar.** { public *; }

# Application classes that will be serialized/deserialized over Gson
-keep class com.almatar.model.** { <fields>; }

# Retain generic signatures of TypeToken and its subclasses with R8 version 3.0 and higher.
-keep,allowobfuscation,allowshrinking class com.google.gson.reflect.TypeToken
-keep,allowobfuscation,allowshrinking class * extends com.google.gson.reflect.TypeToken


# Prevent R8 from leaving Data object members always null
-keepclassmembers,allowobfuscation class * {
  @com.google.gson.annotations.SerializedName <fields>;
}
```
