---
comments: true
date: "2016-05-06 16:50:08 +0800"
layout: post
slug: "customize-crosswalk-for-cordova-project"
title: "Customize Crosswalk for Cordova project"
categories:
- Tips
tags:
- xwalk
- crosswalk
- ionic
- cordova
---

I'm working on a [Ionic](http://ionicframework.com) project which is deployed to mobile devices via [Cordova](https://cordova.apache.org). For performance and compatibility, I introduced [Crosswalk](https://crosswalk-project.org), which is a library embedding a Chrome into the app.

It's working great and as expected. There is only one problem: the size is too big. It comes about 20MB for each architecture, so ARM and x86 need 40MB, while
my app itself is only 10MB.

Fortunately, with [Crosswalk-lite](https://crosswalk-project.org/documentation/crosswalk_lite.html), the library size can be reduced by ~50%. The price is [disabled features](https://crosswalk-project.org/documentation/crosswalk_lite/lite_disabled_feature_list.html), and decompression at the first time of boot after installed. On my old Galaxy Nexus, the decompression takes ~20 seconds.

[Crosswalk-lite](https://crosswalk-project.org/documentation/crosswalk_lite.html) seems in immature state, lacking customizations, including the ability to set the title and content of the prompt during the decompression, which is important to me that I want to comfort the users, using Chinese.

I followed the [instruction](https://crosswalk-project.org/contribute/building_crosswalk.html), and successfully compiled a library with customized decompression prompt. I'm writing down some outdated steps, and missing caveats.

#### Compilation Steps ####

- Get a nice machine. I used a VPS with 8 cores and 16GB memory. It gonna use a lot of resources to compile chromium.
- Follow steps in [Linux Build Instructions — Prerequisites](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions_prerequisites.md), mainly download depot tools and install dependencies.
- Follow steps in [Build Instructions (Android)](https://www.chromium.org/developers/how-tos/android-build-instructions), checking out latest chromium code. I used `--no-history` parameter to get a shallow clone, so it's much smaller.
- Run `build/install-build-deps-android.sh` for more dependencies.
- Clone Crosswalk source as described [here](https://crosswalk-project.org/contribute/building_crosswalk.html), using gclient. Use crosswalk-lite branch in the url (appending `@origin/crosswalk-lite`).
- Set XWALK_OS_ANDROID, configure chromium.gyp_env file (setting `target_arch=ia32` generates a library with _both_ ARM and x86 support, setting `target_arch=arm` generates a library with _only_ ARM support).
- Change anything you need in src/xwalk. In my case, I changed the string of `IDS_CROSSWALK_INSTALL_TITLE` and `IDS_DECOMPRESSION_PROGRESS_MESSAGE` in `runtime/android/core/strings/xwalk_app_strings.grd`.
- Let the compilation begins! We'll need `ninja -C out/Release xwalk_core_library`, and `ninja -C out/Release xwalk_core_library_aar xwalk_shared_library_aar`. It took 0.5~1 hour in my VPS. (The `aar` file seems just a zip file, so to combine different architectures, unzip, copy, and zip again should do, according to [XWALK-1930](https://crosswalk-project.org/jira/browse/XWALK-1930)).
- All we need is the generated `xwalk_core_library.aar` in `src/out/Release`.


#### Usage Steps ####

- According to [Developing with Crosswalk AAR](https://crosswalk-project.org/documentation/android/embedding_crosswalk/crosswalk_aar.html), use `mvn install:install-file` to install `xwalk_core_library.aar` into local mvn repository. The version is specified in parameters and will be used later.
- In `config.xml` of the Ionic project, update `xwalkVersion` to the version we just made up and used.
- In the generated `platform/android`, edit `cordova-plugin-crosswalk-webview/xxx-xwalk.gradle`, find `repositories` block, and _add_ `mavenLocal()`. (The cordova doc is wrong, the url should not be deleted.) It's like

```
repositories {
  maven {
    url xwalkMavenRepo
  }
  mavenLocal()
}
```

and the `dependencies` block, with the full version info.

```
dependencies {
  compile 'org.xwalk:xwalk_core_library_canary:(version without parenthese)'
}
```

- Build and install the apk.

- `<preference name="loadUrlTimeoutValue" value="60000" />` seems necessary in `config.xml`, otherwise, the decompression will use up the time and the page won't load, though it'll work in next boot.

**TODO**:

- Cordova plugin `cordova-plugin-crosswalk-webview` needs work to support customized libraries.
- There may be code setting timeout for device ready. The first try to load contents may be error-prone. It may be a good idea to refresh somehow after decompression.
