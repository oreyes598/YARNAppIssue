# YARNAppIssue

Yet Another React Native App (YARNApp) with Issues

# Overview
This project follows the steps in React Native documentation "[Integration with Existing Apps]( https://reactnative.dev/docs/integration-with-existing-apps)" and outlines my experience, the issues I encountered, and how I got around them. The solution may not be entirely the right one so hoping to share this and get feedback.

# Quick Summary
I am able to make this simple project work but one big gap I'm not able to find a solution is connecting to the Metro server. Connecting to Metro server is not needed for release but needed to speed up development. React Native fetches JavaScript code from Metro server to reflect it to the emulator immediately (simplified explanation).

I've also tried the steps for iOS Objective-C and that went without issues.

# Environment
```
$ react-native info
info Fetching system and libraries information...
System:
    OS: macOS Mojave 10.14.6
    CPU: (8) x64 Intel(R) Core(TM) i7-7920HQ CPU @ 3.10GHz
    Memory: 30.73 MB / 16.00 GB
    Shell: 3.2.57 - /bin/bash
  Binaries:
    Node: 13.12.0 - /usr/local/bin/node
    Yarn: 1.22.4 - /usr/local/bin/yarn
    npm: 6.14.4 - /usr/local/bin/npm
    Watchman: 4.9.0 - /usr/local/bin/watchman
  Managers:
    CocoaPods: 1.8.4 - /usr/local/bin/pod
  SDKs:
    iOS SDK:
      Platforms: iOS 12.4, macOS 10.14, tvOS 12.4, watchOS 5.3
    Android SDK:
      API Levels: 28, 29
      Build Tools: 28.0.3, 29.0.2
      System Images: android-29 | Google Play Intel x86 Atom
      Android NDK: 21.0.6113669
  IDEs:
    Android Studio: 3.6 AI-192.7142.36.36.6308749
    Xcode: 10.3/10G8 - /usr/bin/xcodebuild
  Languages:
    Python: 2.7.16 - /usr/bin/python
  npmPackages:
    @react-native-community/cli: Not Found
    react: 16.11.0 => 16.11.0 
    react-native: ^0.62.1 => 0.62.1 
  npmGlobalPackages:
    *react-native*: Not Found
```


# Details
Now on to the details and braise yourself for this long sequence of steps. I tried my best to document every step of the way.

- Created Android project with Empty Activity

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/40e003bee46e265ac347bcb1f2e3fcee554b579c

- Added package.json

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/89d6d9f46e1c5e9fb2f1ce93b4caa621671f4380

- Add react-native by running the command in the terminal
> yarn add react-native

- See all these warning
```
[1/4] 🔍  Resolving packages...
warning react-native > fbjs > core-js@2.6.11: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
warning react-native > create-react-class > fbjs > core-js@1.2.7: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
warning react-native > fbjs-scripts > core-js@2.6.11: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
warning react-native > metro-babel-register > core-js@2.6.11: core-js@<3 is no longer maintained and not recommended for usage due to the number of issues. Please, upgrade your dependencies to the actual version of core-js@3.
warning react-native > @react-native-community/cli > metro-core > jest-haste-map > micromatch > snapdragon > source-map-resolve > urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
warning react-native > @react-native-community/cli > metro-core > jest-haste-map > micromatch > snapdragon > source-map-resolve > resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
warning "react-native > metro-react-native-babel-transformer@0.58.0" has unmet peer dependency "@babel/core@*".
warning "react-native > use-subscription@1.4.1" has unmet peer dependency "react@^16.8.0".
warning " > react-native@0.62.1" has unmet peer dependency "react@16.11.0".
```

- Resolved all warnings by running these commands
    > $ yarn add core-js@3
    
    > $ yarn add @babel/core@*
    
    > $ yarn add react@16.11.0


  Commit: https://github.com/oreyes598/YARNAppIssue/commit/e8a487ed5e82c4cf5936b670ded63974ba6ab5b0

- Added dependencies to Android app build.gradle and repository in root build.gradle project

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/b8bed2b8d263da3992d8c426db7247ba490847ba

- Updated AndroidManifest file to add permissions and Actvity

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/1eed5ba318a6ff1c14bd7f6b10d92790ccb49c43

- Created ReactNative Activity in the Android project

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/2280efa1cde06ac5445beea04f2bc5c8d701f51c

- Added a button in the MainActivity to start the MyReactActivity

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/9d41ac099a2b5d50bd5080c703cf23013c7d844c

- Now ready to run the application. Start Metro server
> yarn start

- **ISSUE #1**: In Android Studio, ran the app and clicked the button to start the MyReactActivity but encountered this crash. Also attempted to start the app just using 'npx react-native run-android' and the same issue.

```
2020-04-03 14:44:55.688 3337-3438/com.yarn.appissue D/SoLoader: About to load: libhermes.so
2020-04-03 14:44:55.691 3337-3438/com.yarn.appissue D/SoLoader: libhermes.so not found on /data/data/com.yarn.appissue/lib-main
2020-04-03 14:44:55.692 3337-3438/com.yarn.appissue D/SoLoader: libhermes.so not found on /data/app/com.yarn.appissue-kDz7svHCD1g1jexnsvDB3A==/lib/x86
2020-04-03 14:44:55.709 3337-3438/com.yarn.appissue D/SoLoader: libhermes.so not found on /vendor/lib
2020-04-03 14:44:55.709 3337-3438/com.yarn.appissue D/SoLoader: libhermes.so not found on /system/lib
2020-04-03 14:44:55.722 3337-3438/com.yarn.appissue E/SoLoader: couldn't find DSO to load: libhermes.so
    
    --------- beginning of crash
2020-04-03 14:44:55.723 3337-3438/com.yarn.appissue E/AndroidRuntime: FATAL EXCEPTION: create_react_context
    Process: com.yarn.appissue, PID: 3337
    java.lang.UnsatisfiedLinkError: couldn't find DSO to load: libhermes.so
        at com.facebook.soloader.SoLoader.doLoadLibraryBySoName(SoLoader.java:789)
        at com.facebook.soloader.SoLoader.loadLibraryBySoName(SoLoader.java:639)
        at com.facebook.soloader.SoLoader.loadLibrary(SoLoader.java:577)
        at com.facebook.soloader.SoLoader.loadLibrary(SoLoader.java:525)
        at com.facebook.hermes.reactexecutor.HermesExecutor.<clinit>(HermesExecutor.java:20)
        at com.facebook.hermes.reactexecutor.HermesExecutorFactory.create(HermesExecutorFactory.java:29)
        at com.facebook.react.ReactInstanceManager$5.run(ReactInstanceManager.java:997)
        at java.lang.Thread.run(Thread.java:919)
```

- **ISSUE #2**: Updated root project gradle.build and app gradle.build to add android-jsc support and that stopped the crash above from happening but getting new crash now.

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/511754990215d73745915897e111c124627cee5e

```
2020-04-03 14:57:34.887 3986-3986/com.yarn.appissue D/ReactNative: ReactInstanceManager.createReactContextInBackground()
2020-04-03 14:57:34.887 3986-3986/com.yarn.appissue D/ReactNative: ReactInstanceManager.recreateReactContextInBackgroundInner()
2020-04-03 14:57:34.935 3986-4052/com.yarn.appissue W/unknown:ReactNative: The packager does not seem to be running as we got an IOException requesting its status: socket failed: EPERM (Operation not permitted)
2020-04-03 14:57:34.940 3986-4051/com.yarn.appissue W/unknown:InspectorPackagerConnection: Couldn't connect to packager, will silently retry
2020-04-03 14:57:34.943 3986-4050/com.yarn.appissue W/unknown:ReconnectingWebSocket: Couldn't connect to "ws://10.0.2.2:8081/message?device=Android%20SDK%20built%20for%20x86%20-%2010%20-%20API%2029&app=com.yarn.appissue&clientid=DevSupportManagerImpl", will silently retry
2020-04-03 14:57:34.948 3986-3986/com.yarn.appissue W/unknown:ReactNative: Packager connection already open, nooping.
:
2020-04-03 14:57:35.061 3986-4054/com.yarn.appissue D/ReactNative: CatalystInstanceImpl.runJSBundle()
2020-04-03 14:57:35.065 3986-4054/com.yarn.appissue E/unknown:ReactNative: Exception in native call
    java.lang.RuntimeException: Unable to load script. Make sure you're either running a Metro server (run 'react-native start') or that your bundle 'index.android.bundle' is packaged correctly for release.
        at com.facebook.react.bridge.CatalystInstanceImpl.jniLoadScriptFromAssets(Native Method)
        at com.facebook.react.bridge.CatalystInstanceImpl.loadScriptFromAssets(CatalystInstanceImpl.java:235)
        at com.facebook.react.bridge.JSBundleLoader$1.loadScript(JSBundleLoader.java:29)
        at com.facebook.react.bridge.CatalystInstanceImpl.runJSBundle(CatalystInstanceImpl.java:259)
        at com.facebook.react.ReactInstanceManager.createReactContext(ReactInstanceManager.java:1243)
        at com.facebook.react.ReactInstanceManager.access$1000(ReactInstanceManager.java:132)
        at com.facebook.react.ReactInstanceManager$5.run(ReactInstanceManager.java:996)
        at java.lang.Thread.run(Thread.java:919)
```

- NOTE: Metro server is running and even accessible in browser http://localhost:8081/. Getting "React Native packager is running." message when loaded in the browser.

- After several Google search for this issue and not able to find solution I ended up not relying on Metro server to get JavaScript code so just added it to the Android project. Ran the command below to bundle the JavaScript code to Android project.

>npx react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/9dab535e63756ddbb1e1c236c0a4542ab71a4adb

- **ISSUE #3**: Ran the app and clicking the button to start the MyReactActivity crashed

```
2020-04-03 15:13:47.016 4850-4914/com.yarn.appissue W/unknown:ViewManagerPropertyUpdater: Could not find generated setter for class com.facebook.react.views.swiperefresh.SwipeRefreshLayoutManager
2020-04-03 15:13:47.021 4850-4914/com.yarn.appissue E/unknown:NativeModuleInitError: Failed to create NativeModule "UIManager"
    java.lang.NoClassDefFoundError: Failed resolution of: Landroidx/swiperefreshlayout/widget/SwipeRefreshLayout;
        at com.facebook.react.shell.MainReactPackage.createViewManagers(MainReactPackage.java:190)
        at com.facebook.react.ReactInstanceManager.getOrCreateViewManagers(ReactInstanceManager.java:830)
        at com.facebook.react.CoreModulesPackage.createUIManager(CoreModulesPackage.java:189)
        at com.facebook.react.CoreModulesPackage.getModule(CoreModulesPackage.java:157)
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:159)
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:147)
        at com.facebook.react.bridge.ModuleHolder.create(ModuleHolder.java:191)
        at com.facebook.react.bridge.ModuleHolder.getModule(ModuleHolder.java:156)
        at com.facebook.react.bridge.NativeModuleRegistry.getModule(NativeModuleRegistry.java:149)
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:590)
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:567)
        at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:42)
        at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1111)
        at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1057)
        at com.facebook.react.ReactInstanceManager.access$1400(ReactInstanceManager.java:132)
        at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1017)
        at android.os.Handler.handleCallback(Handler.java:883)
        at android.os.Handler.dispatchMessage(Handler.java:100)
        at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27)
        at android.os.Looper.loop(Looper.java:214)
        at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:226)
        at java.lang.Thread.run(Thread.java:919)
     Caused by: java.lang.ClassNotFoundException: Didn't find class "androidx.swiperefreshlayout.widget.SwipeRefreshLayout" on path: DexPathList[[zip file "/data/app/com.yarn.appissue-YeTA4f34eyuQqCA-XrZD7w==/base.apk"],nativeLibraryDirectories=[/data/app/com.yarn.appissue-YeTA4f34eyuQqCA-XrZD7w==/lib/x86, /data/app/com.yarn.appissue-YeTA4f34eyuQqCA-XrZD7w==/base.apk!/lib/x86, /system/lib, /system/product/lib]]
        at dalvik.system.BaseDexClassLoader.findClass(BaseDexClassLoader.java:196)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:379)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:312)
        at com.facebook.react.shell.MainReactPackage.createViewManagers(MainReactPackage.java:190) 
        at com.facebook.react.ReactInstanceManager.getOrCreateViewManagers(ReactInstanceManager.java:830) 
        at com.facebook.react.CoreModulesPackage.createUIManager(CoreModulesPackage.java:189) 
        at com.facebook.react.CoreModulesPackage.getModule(CoreModulesPackage.java:157) 
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:159) 
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:147) 
        at com.facebook.react.bridge.ModuleHolder.create(ModuleHolder.java:191) 
        at com.facebook.react.bridge.ModuleHolder.getModule(ModuleHolder.java:156) 
        at com.facebook.react.bridge.NativeModuleRegistry.getModule(NativeModuleRegistry.java:149) 
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:590) 
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:567) 
        at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:42) 
        at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1111) 
        at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1057) 
        at com.facebook.react.ReactInstanceManager.access$1400(ReactInstanceManager.java:132) 
        at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1017) 
        at android.os.Handler.handleCallback(Handler.java:883) 
        at android.os.Handler.dispatchMessage(Handler.java:100) 
        at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27) 
        at android.os.Looper.loop(Looper.java:214) 
        at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:226) 
        at java.lang.Thread.run(Thread.java:919) 
2020-04-03 15:13:47.021 4850-4850/com.yarn.appissue D/ReactNative: ReactInstanceManager.attachRootViewToInstance()
    
    --------- beginning of crash
2020-04-03 15:13:47.023 4850-4914/com.yarn.appissue E/AndroidRuntime: FATAL EXCEPTION: mqt_native_modules
    Process: com.yarn.appissue, PID: 4850
    java.lang.NoClassDefFoundError: Failed resolution of: Landroidx/swiperefreshlayout/widget/SwipeRefreshLayout;
        at com.facebook.react.shell.MainReactPackage.createViewManagers(MainReactPackage.java:190)
        at com.facebook.react.ReactInstanceManager.getOrCreateViewManagers(ReactInstanceManager.java:830)
        at com.facebook.react.CoreModulesPackage.createUIManager(CoreModulesPackage.java:189)
        at com.facebook.react.CoreModulesPackage.getModule(CoreModulesPackage.java:157)
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:159)
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:147)
        at com.facebook.react.bridge.ModuleHolder.create(ModuleHolder.java:191)
        at com.facebook.react.bridge.ModuleHolder.getModule(ModuleHolder.java:156)
        at com.facebook.react.bridge.NativeModuleRegistry.getModule(NativeModuleRegistry.java:149)
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:590)
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:567)
        at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:42)
        at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1111)
        at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1057)
        at com.facebook.react.ReactInstanceManager.access$1400(ReactInstanceManager.java:132)
        at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1017)
        at android.os.Handler.handleCallback(Handler.java:883)
        at android.os.Handler.dispatchMessage(Handler.java:100)
        at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27)
        at android.os.Looper.loop(Looper.java:214)
        at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:226)
        at java.lang.Thread.run(Thread.java:919)
     Caused by: java.lang.ClassNotFoundException: Didn't find class "androidx.swiperefreshlayout.widget.SwipeRefreshLayout" on path: DexPathList[[zip file "/data/app/com.yarn.appissue-YeTA4f34eyuQqCA-XrZD7w==/base.apk"],nativeLibraryDirectories=[/data/app/com.yarn.appissue-YeTA4f34eyuQqCA-XrZD7w==/lib/x86, /data/app/com.yarn.appissue-YeTA4f34eyuQqCA-XrZD7w==/base.apk!/lib/x86, /system/lib, /system/product/lib]]
        at dalvik.system.BaseDexClassLoader.findClass(BaseDexClassLoader.java:196)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:379)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:312)
        at com.facebook.react.shell.MainReactPackage.createViewManagers(MainReactPackage.java:190) 
        at com.facebook.react.ReactInstanceManager.getOrCreateViewManagers(ReactInstanceManager.java:830) 
        at com.facebook.react.CoreModulesPackage.createUIManager(CoreModulesPackage.java:189) 
        at com.facebook.react.CoreModulesPackage.getModule(CoreModulesPackage.java:157) 
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:159) 
        at com.facebook.react.TurboReactPackage$ModuleHolderProvider.get(TurboReactPackage.java:147) 
        at com.facebook.react.bridge.ModuleHolder.create(ModuleHolder.java:191) 
        at com.facebook.react.bridge.ModuleHolder.getModule(ModuleHolder.java:156) 
        at com.facebook.react.bridge.NativeModuleRegistry.getModule(NativeModuleRegistry.java:149) 
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:590) 
        at com.facebook.react.bridge.CatalystInstanceImpl.getNativeModule(CatalystInstanceImpl.java:567) 
        at com.facebook.react.uimanager.UIManagerHelper.getUIManager(UIManagerHelper.java:42) 
        at com.facebook.react.ReactInstanceManager.attachRootViewToInstance(ReactInstanceManager.java:1111) 
        at com.facebook.react.ReactInstanceManager.setupReactContext(ReactInstanceManager.java:1057) 
        at com.facebook.react.ReactInstanceManager.access$1400(ReactInstanceManager.java:132) 
        at com.facebook.react.ReactInstanceManager$5$2.run(ReactInstanceManager.java:1017) 
        at android.os.Handler.handleCallback(Handler.java:883) 
        at android.os.Handler.dispatchMessage(Handler.java:100) 
        at com.facebook.react.bridge.queue.MessageQueueThreadHandler.dispatchMessage(MessageQueueThreadHandler.java:27) 
        at android.os.Looper.loop(Looper.java:214) 
        at com.facebook.react.bridge.queue.MessageQueueThreadImpl$4.run(MessageQueueThreadImpl.java:226) 
        at java.lang.Thread.run(Thread.java:919) 
```

- Added swiperefreshlayout as a dependency to the app in the app build.gradle file.

  Commit: https://github.com/oreyes598/YARNAppIssue/commit/b76221bb2054cee8b03c4b7f64668b39164693ea

- Ran the app, click on the button to start the MyReactActivity and got it working.


