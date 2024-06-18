# Expo Starter Kit (JavaScript)

In order to use it, you must 

## Installation

To use it with EAS, you **MUST** create your own development tool. To do this, follow the steps described in this guide:

1. Execute `npm install` or `npx expo install`.
2. Follow the integration instructions at [Viro Community Documentation on Integrating with Expo](https://viro-community.readme.io/docs/integrating-with-expo).

3. After completing the prebuild process described below, you must check that files exist and include all the changes described in the [Viro Community Documentation on Installation Instructions](https://viro-community.readme.io/docs/installation-instructions).

4. Then, navigate to your `MainApplication.kt` file located at `android/src/main/java/com/${your_expo_account}/expostarterkit/MainApplication.kt` and replace its content with the provided code.

```kotlin
package com.lmatamoros.expostarterkit

import android.app.Application
import android.content.res.Configuration
import androidx.annotation.NonNull
import com.facebook.react.PackageList
import com.facebook.react.ReactApplication
import com.facebook.react.ReactNativeHost
import com.facebook.react.ReactPackage
import com.facebook.react.ReactHost
import com.facebook.react.config.ReactFeatureFlags
import com.facebook.react.defaults.DefaultNewArchitectureEntryPoint.load
import com.facebook.react.defaults.DefaultReactHost.getDefaultReactHost
import com.facebook.react.defaults.DefaultReactNativeHost
import com.facebook.react.flipper.ReactNativeFlipper
import com.facebook.soloader.SoLoader
import expo.modules.ApplicationLifecycleDispatcher
import expo.modules.ReactNativeHostWrapper
import com.viromedia.bridge.ReactViroPackage // Correct import for ReactViroPackage

class MainApplication : Application(), ReactApplication {

  override val reactNativeHost: ReactNativeHost = ReactNativeHostWrapper(
        this,
        object : DefaultReactNativeHost(this) {
          override fun getPackages(): List<ReactPackage> {
            val packages = PackageList(this).packages // Gets the list of auto-linked packages

            // Adds ReactViroPackage to the list
            packages.add(ReactViroPackage(ReactViroPackage.ViroPlatform.valueOf("AR")))

            // Returns the modified list
            return packages
          }

          override fun getJSMainModuleName(): String = "index" // Make sure this is the correct main module for your project

          override fun getUseDeveloperSupport(): Boolean = BuildConfig.DEBUG

          override val isNewArchEnabled: Boolean = BuildConfig.IS_NEW_ARCHITECTURE_ENABLED
          override val isHermesEnabled: Boolean = BuildConfig.IS_HERMES_ENABLED
      }
  )

  override val reactHost: ReactHost
    get() = getDefaultReactHost(this.applicationContext, reactNativeHost)

  override fun onCreate() {
    super.onCreate()
    SoLoader.init(this, false)
    if (!BuildConfig.REACT_NATIVE_UNSTABLE_USE_RUNTIME_SCHEDULER_ALWAYS) {
      ReactFeatureFlags.unstable_useRuntimeSchedulerAlways = false
    }
    if (BuildConfig.IS_NEW_ARCHITECTURE_ENABLED) {
      // If you opted-in for the New Architecture, we load the native entry point for this app.
      load()
    }
    if (BuildConfig.DEBUG) {
      ReactNativeFlipper.initializeFlipper(this, reactNativeHost.reactInstanceManager)
    }
    ApplicationLifecycleDispatcher.onApplicationCreate(this)
  }

  override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
    ApplicationLifecycleDispatcher.onConfigurationChanged(this, newConfig)
  }
}

```

5. Run `npx eas build --profile development`.

6. Execute `npx expo start -c`.

## Running

```shell
npm run start`
```

## Expo Docs

[Expo Docs](https://docs.expo.dev/)

## Need help?

<a href="https://discord.gg/H3ksm5NhzT">
   <img src="https://discordapp.com/api/guilds/774471080713781259/widget.png?style=banner2" alt="Discord Banner 2"/>
</a>
