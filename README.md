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
        // or use AlmatarEnvironment.Production to use production env.
        AlmatarAppInitializer.init(application = this, almatarEnvironment = AlmatarEnvironment.Development)
    }
    ```

Now you can launch Almatar flight flow by calling this function:
```kotlin
AlmatarAppInitializer.launchFlights(
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
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
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
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
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                
            )
```

to launch Almatar Hajj flow call this function:
```kotlin
AlmatarAppInitializer.launchHajjFlow(
                HajjData(
                    jeddahOnwardDateFrom = 1743465600000, //Minimum travel date to Jeddah
                    jeddahOnwardDateTo = 1743692400000, //Maximum travel date time to Jeddah (Latest allowed time to catch the bus)
                    jeddahReturnDateFrom = 1744203600000, //Minimum return date time from Jeddah (Earliest allowed time to catch the bus)
                    jeddahReturnDateTo = 1744329600000, ///Maximum return date from Jeddah
                    taifOnwardDateFrom = 1743552000000, ///Minimum travel date to Taif
                    taifOnwardDateTo = 1743778800000, //Maximum travel date time to Taif (Latest allowed time to catch the bus)
                    taifReturnDateFrom = 1744290000000, //Minimum return date time from Taif (Earliest allowed time to catch the bus)
                    taifReturnDateTo = 1744416000000, //Maximum return date from Taif
                    passengers = listOf(
                        PassengerData(
                            title = PassengerTitle.Mr, //PassengerTitle.Mr or PassengerTitle.Mrs or PassengerTitle.Ms
                            firstName = "John",
                            lastName = "Doe",
                            birthDate = 856026800000,
                            nationality = "EG",
                            phoneNumberCountryCode = 966, // required for the first passenger only
                            phoneNumber = "555555555", // required for first passenger only
                            email = "johndoe@email.com", // required for first passenger only
                            documentType = PassengerDocumentType.NationalID, //Supports only PassengerDocumentType.NationalId and PassengerDocumentType.Iqama
                            documentNumber = "1234567890",
                            documentExpirationDate = 1773804800000
                        )
                    ),
                    supportedCity = HajjSupportedCities.Jeddah // HajjSupportedCities.Jeddah, HajjSupportedCities.Taif or HajjSupportedCities.Both
                ),
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC,
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                startPaymentFlowImpl = { bookingId /*Use for payment flow*/, amount, bookingKey /*Use for confirmation summary*/, legsData ->
                    //Start payment flow
                }
            )
```

to launch Almatar Hajj Confirmation screen call this function:
```kotlin
AlmatarAppInitializer.launchHajjConfirmationFlow(
                bookingKey = "",
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC,
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                }
            )
```

to launch Almatar Umrah Packages Flight flow call this function:
```kotlin
AlmatarAppInitializer.launchUmrahPackagesFlightsFlow(
                UmrahFlightData(
                    onwardDateFrom = 1743465600000, //Minimum travel date
                    onwardDateTo = 1743692400000, //Maximum travel date
                    returnDateFrom = 1744203600000, //Minimum return date
                    returnDateTo = 1744329600000, ///Maximum return date 
                    passenger = PassengerData(
                            title = PassengerTitle.Mr, //PassengerTitle.Mr or PassengerTitle.Mrs or PassengerTitle.Ms
                            firstName = "John",
                            lastName = "Doe",
                            birthDate = 856026800000,
                            nationality = "EG",
                            phoneNumberCountryCode = 966,
                            phoneNumber = "555555555",
                            email = "johndoe@email.com",
                            documentType = PassengerDocumentType.NationalID, //Supports PassengerDocumentType.Passport, PassengerDocumentType.NationalId and PassengerDocumentType.Iqama
                            documentNumber = "1234567890",
                            documentExpirationDate = 1773804800000
                    ),
                    destination = UmrahPackagesFlightsDestinations.Jeddah // Jeddah, Madinah, Taif, Yanbo or All
                ),
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC,
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                startPaymentFlowImpl = { bookingId /*Use for payment flow*/, amount, bookingKey /*Use for confirmation summary*/, legsData ->
                    //Start payment flow
                }
            )
```

to launch Almatar Umrah Packages Hotel flow call this function:
```kotlin
AlmatarAppInitializer.launchUmrahPackagesHotelsFlow(
                UmrahHotelData(
                            checkInDateFrom = 1745884800000, // Check In Date Range Start
                            checkInDateTo = 1745884800000, // Check In Date Range End
                            checkOutDateFrom = 1745971200000, // Check Out Date Range Start
                            checkOutDateTo = 1745971200000, // Check Out Date Range End
                            guest = GuestData(
                                title = GuestTitle.Mr, //GuestTitle.Mr or GuestTitle.Mrs or GuestTitle.Ms
                                firstName = "MOHAMMAD",
                                lastName = "AHMED",
                                birthDate = 788911200000,
                                nationality = "SA",
                                phoneNumberCountryCode = 966,
                                phoneNumber = "563142124",
                                email = "ASDFDFFFFGGH@FHJGHJJ.COM"
                            ),
                            destination = UmrahPackagesHotelsDestinations.Both // Makkah, Madinah or Both
                        ),
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC,
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                },
                startPaymentFlowImpl = { bookingId /*Use for payment flow*/, amount, publicKey /*Use for confirmation summary*/, hotelData ->
                    //Start payment flow
                }
            )
```

to launch Almatar Umrah Packages Flight Confirmation screen call this function:
```kotlin
AlmatarAppInitializer.launchUmrahFlightConfirmation(
                bookingKey = "",
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC,
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                }
            )
```

to launch Almatar Umrah Packages Hotel Confirmation screen call this function:
```kotlin
AlmatarAppInitializer.launchUmrahHotelConfirmation(
                publicKey = "",
                locale = Config.LOCALE.ARABIC, //Use your preferred locale: Config.LOCALE.ENGLISH or Config.LOCALE.ARABIC,
                almatarFlowFinishedCallback = {
                    // will be called when Almatar SDK flow is closed
                },
                generateTokenImpl = { callback ->
                    callback(
                        "<replace with your token>",
                        "" //error message incase of any failure in generate 
                    )
                }
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
