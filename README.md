# ARCHIVED REPOSITORY FOR IOS 3.0 -> 3.2.3;

# IF YOU NEED THE NEWEST PLUGIN FOR IOS PLEASE CHECK THIS LINK: https://github.com/Poing-Studios/godot-admob-ios

[![Build%20iOS%203.0%20-%3E%203.2.3](https://github.com/Poing-Studios/Godot-AdMob-Android-iOS/workflows/Build%20iOS/badge.svg)](https://github.com/Poing-Studios/Godot-AdMob-Android-iOS/actions)
![test](https://img.shields.io/badge/Google%20Mobile%20ADS%20SDK-v7.69-informational)

> *For Android 3.2+ and iOS 3.3+ check [here](https://github.com/Poing-Studios/Godot-AdMob-Android-iOS) the `master` branch*

# Godot AdMob for Android and iOS
This repository uses [GitHub Actions](https://github.com/features/actions), this means that whenever a new update is sent to the repository, the action will automatically test the code of the module, compile, compress the binary files and export to the ["Releases tab"](https://github.com/Poing-Studios/Godot-AdMob-Android-iOS/releases) of the repository for the respective operational system and versions supported by the module, like v3.2.3.


<p align="center">
	<img align="center" src="https://i.imgur.com/u5y2GEx.png">
</p>

### Ad Formats
- Banner 
- Interstitial
- Rewarded

Is high recommended that when you use AdMob, please include it as AutoLoad and Singleton

Download example project to see how the Plugin works!

# iOS (v3.0.0 -> 3.2.3):
- ~~We don't have pre compiled files for 3.1 and 3.1.1 [due bugs with x86_64 and arm64](https://github.com/godotengine/godot/issues/27658)~~
- Download the ```ios-template-v{{ your_godot_version }}.zip``` in the releases tab. [STANDARD VERSION](https://github.com/Poing-Studios/Godot-AdMob-Android-iOS/releases/tag/iOS_v3.0%2B) 
- Export your game to iOS
- Copy the library ```libgodot.iphone.release.fat.a``` you have downloaded from releases tab inside the exported Xcode project. **You must delete the 'your_project_name.a' (example: AdMob.a) and rename the 'libgodot.iphone.release.fat.a' with "your_project_name.a", should be like: 'AdMob.a'.** 
- ![](https://media2.giphy.com/media/miNlL020ZQYjK4r8e7/giphy.gif)
- Import the [Mobile Ads SDK](https://developers.google.com/admob/ios/quick-start#import_the_mobile_ads_sdk), we recommend you using the Cocoapods since the version build on this Branch is `7.69`.
- Add the following frameworks to the project linking it using the "Build Phases" -> "Link Binary with Libraries" option:
	- These frameworks are already in your computer
		- AppTrackingTransparency | ```Status: (Optional) ``` (if not appear: need to update XCode or SDK version to iOS 14.0)
		- AdSupport | ```Status: (Optional) ```
		- JavaScriptCore
- Add the -ObjC linker flag to Other Linker Flags in your project's build settings:
![-ObjC](https://developers.google.com/admob/images/ios/objc_linker_flag.png)
- [Update your GAMENAME-Info.plist file](https://developers.google.com/admob/ios/quick-start#update_your_infoplist), add a GADApplicationIdentifier key with a string value of your [AdMob app ID](https://support.google.com/admob/answer/7356431):
![plist](https://i.imgur.com/1tcKXx5.png)
- [Enable SKAdNetwork to track conversions](https://developers.google.com/admob/ios/ios14#skadnetwork):
![SKAdNetwork](https://developers.google.com/admob/images/idfa/skadnetwork.png)
- [Request App Tracking Transparency authorization](https://developers.google.com/admob/ios/ios14#request)
![RequestAuthorization](https://developers.google.com/admob/images/idfa/editor.png)
- (Optional) If you are using UMP, you can add too the [Delay app measurement](https://developers.google.com/admob/ump/ios/quick-start#delay_app_measurement_optional)
![DelayAppMeasurement](https://developers.google.com/admob/images/delay_app_measurement_plist.png)


# User Messaging Platform (UMP):
- To use UMP due of EUROPE ePrivacy Directive and the General Data Protection Regulation (GDPR), you first need to do configure your [Funding Choices](https://support.google.com/fundingchoices/answer/9180084).
- If your app is "ForChildDirectedTreatment" then the UMP [won't appear and signals won't work for consent](https://stackoverflow.com/a/63232045), this is normal so don't worry.
- To show personalized or non-personalized ads, then you need to change inside your [AdMob Account](https://apps.admob.com/?utm_source=internal&utm_medium=et&utm_campaign=helpcentrecontextualopt&utm_term=http://goo.gl/6Xkfcf&subid=ww-ww-et-amhelpv4)
![npa-image](https://i.stack.imgur.com/0v1eL.png)

# API References
Signals:
```GDScript
initialization_complete(status, adapter_name) #when AdMob initializes, can be enum NOT_READY(0) or READY(1)

banner_loaded() #ad finishes loading
banner_destroyed() #banner view is destroyed
banner_failed_to_load(error_code : int) #ad request fails
banner_opened() #ad opens an overlay that
banner_left_application() #user has left the app
banner_closed() # user is about to return to the app after tapping on an ad

interstitial_loaded() #ad finishes loading
interstitial_failed_to_load(error_code : int) #ad request fails
interstitial_opened() #ad is displayed
interstitial_left_application() #user has left the app
interstitial_closed() #interstitial ad is closed

rewarded_ad_loaded() #ad successfully loaded
rewarded_ad_failed_to_load() #ad failed to load
rewarded_ad_opened() #ad is displayed
rewarded_ad_closed() #ad is closed
rewarded_user_earned_rewarded(currency : String, amount : int) #user earner rewarded
rewarded_ad_failed_to_show(error_code) #ad request fails

unified_native_loaded() #unified native loaded and shows the ad
unified_native_destroyed() #unified native view destroyed
unified_native_failed_to_load(error_code : int) #ad request fails

unified_native_opened() #ad opens an overlay that
unified_native_closed() #user is about to return to the app after tapping on an ad

consent_form_dismissed() #then the consent is REQUIRED(user in EEA or UK) and user dismissed the form
consent_status_changed(consent_status_message) #get the ConsentStatus
consent_form_load_failure(error_code, error_message) #get the code and message of error to see why form is not loading
consent_info_update_success() #consent information state was updated
consent_info_update_failure(error_code, error_message) #get the code and message of error to see why info update consent fail
```

Methods
```GDScript
#Private
#-----------------
_initialize() #init the AdMob
_on_AdMob_*() #just to emit signals

#Public
#-----------------
load_banner() #load the banner will make him appear instantly
load_interstitial() #loads the interstitial and make ready for show
load_rewarded() #loads the rewarded and make ready for show
load_unified_native(control_node_to_be_replaced : Control = Control.new()) #load the unified native will make him appear instantly (unified native and banner are View in Android and iOS, it is recommended to only use one of them at a time, if you try to use both, the module will not allow it, it will remove the older view

show_interstitial() #shows interstitial
show_rewarded() #shows rewarded

destroy_banner() #completely destroys the Banner View
destroy_unified_native() #completely destroys the Unified Native View

```
