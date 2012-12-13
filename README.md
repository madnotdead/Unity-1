# Tap for Tap Unity Plugin (Coming Soon!)

Want to use Tap for Tap with Unity? We got you covered on Android and iOS.

## Preamble
This Unity plugin was built and tested against Unity 3.5.

### Minimum Requirements

#### Android
- None

#### iPhone
- An OS X machine running Lion (10.7)
- iOS 6.0 SDK

## Integration
If you don't have the plugin yet then head over to the [no link](need url).

Installing the Tap for Tap Unity Plugin is easy. This isn't a Unity tutorial so we 
assume that you have a Unity project already set up and working. All you need to 
do is import the TapForTap.unitypackage into your app. Then follow the steps below
for Android and iOS

### Configuring Tap for Tap for iOS
When Xcode loads you will need to add the AdSupport framework to your build target.
- Add the AdSupport framework.

### Configuring Tap for Tap for Android
A few additions need to be made to Unity's AndroidManifest.xml. If you do not
have a custom manifest Unity recommends to create your own under the Assets/Plugins/Android folder ([Unity docs](http://docs.unity3d.com/Documentation/Manual/PluginsForAndroid.html)).
Or you can edit the default manifest at
`/Applications/Unity/Unity.app/Contents/PlaybackEngines/AndroidPlayer` for Mac OSX
 and `C:\Program Files\Unity\Editor\Data\PlaybackEngines\AndroidPlayer\AndroidManifest.xml` for Windows.
 
1. Add the following permissions to the AndroidManifest.xml
  `<uses-permission android:name="android.permission.INTERNET" />`
  `<uses-permission android:name="android.permission.READ_PHONE_STATE" />`
  `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />`
  `<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />`
  `<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />`
2. Include the TapForTap activity in the AndroidManifest.xml  
  `<activity android:name="com.tapfortap.TapForTapActivity"/>`

Congratulations, you are done. You should now be able to call into the Tap for Tap library
and begin displaying ads.

## API Documentation
The C# API lets you create, place, and remove Tap for Tap ad views,
show interstitals and app walls, and set various optional information about your users
to help with targetting. Please make sure your privacy policy allows this
before providing any personal information. All the API calls are static methods
found in the TapForTap class.

### Tap for Tap API

#### public static void initialize(string apiKey)
This must be called once at the start of the app before any other TapForTap calls
can be made. It requires a Tap for Tap account API key which can be found by logging 
into Tap for Tap and clicking the account button.

Usage:

```cs
  // Initialize Tap for Tap
  TapForTap.initialize("MY API KEY");
```

#### public static void CreateAdView(TapForTapVerticalAlignment vertical, TapForTapHorizontalAlignment horizontal)
Create a banner AdView of width 350dp and height 50dp at the desired screen location. 
- **TapForTapVerticalAlignment** is an enum that has the following values : TOP, CENTER, BOTTOM. 
- **TapForTapHorizontalAlignment** is an enum that has the following values : LEFT, CENTER, RIGHT.
- 
By combining a vertical and horizontal alignment you can place an advertisement in 
one of 9 places. A `*` denotes a location where an ad can be placed on the screen.
<pre>
-----------  
|*   *   *|   
|         |   
|*   *   *|   
|         |   
|*   *   *|   
-----------   
</pre>

Usage:

```cs
  // Create an AdView at the bottom center of the screen
  TapForTap.CreateAdView(TapForTapVerticalAlignment.BOTTOM, TapForTapHorizontalAlignment.CENTER)
```

#### public static void RemoveAdView()
Remove any AdView that is currently being displayed.

Usage:

```cs
  // Remove the currently displayed AdView
  TapForTap.RemoveAdView();
```

#### public static void PrepareInterstitial()
Prepare an interstital ad by prefetching the ad. This method only needs to be called once.
After the interstitial is shown we automatically prepare another one.

Usage:

```cs
  TapForTap.PrepareInterstitial();
```

#### public static void ShowInterstitial()
Shows an interstitial ad.

Usage:

```cs
  TapForTap.ShowInterstitial();
```

#### public static void PrepareAppWall()
Prepare an app wall ad by prefetching the ad. This method only needs to be called once.
After the app wall is shown we automatically prepare another one.

Usage:

```cs
  TapForTap.PrepareAppWall();
```

#### public static void ShowAppWall()
Show an app wall ad.

Usage:

```cs
  TapForTap.ShowAppWall();
```

#### public static void SetYearOfBirth(int yearOfBirth)
#### public static void SetGender(TapForTapGender gender)
#### public static void SetLocation(double latitude, double longitude)
#### public static void SetUserAccountId(string userAccountId)

#### public static void setAdViewListener(ITapForTapAdView listener)
Sets the listener that will receive the AdView callbacks. See [IAdViewListener](#IAdViewListener)
for more details and available callback methods.

Usage:

```cs
  // MyAdViewListener.cs
  public class MyAdViewListener : ITapForTapAdView
  {
    public void OnTapAd()
	  {
		  Debug.Log("Called my OnTapAd);
	  }
    
	  public void OnReceiveAd()
	  {
		  Debug.Log("Called my OnReceiveAd");
	  }
    
	  public void OnFailToReceiveAd(string reason)
	  {
		  Debug.Log("Called my OnFailToReceiveAd because of " + reason);
	  }
  }

  // MyTapForTap.cs
  MyAdViewListener myAdViewListener = new MyAdViewListener();
  TapForTap.setAdViewListener(MyAdViewListener);
```

#### public static void setAppWallListener(ITapForTapAppWall listener)
Sets the listener that will receive the AppWall callbacks. See [IAppWallListener](#IAppWallListener)
for more details and available callback methods.

Usage:

```cs
  // MyAppWallListener.cs
  public class MyAppWallListener : ITapForTapAppWall
  {
    public void onDismiss()
    {
      Debug.Log("Called my app wall listener OnDismiss");
    }
  }

  // MyTapForTap.cs
  MyAppWallListener myAppWallListener = new MyAppWallListener();
  TapForTap.setAppWallListener(myAppWallListener);
```

#### public static void setInterstitiallListener(ITapForTapInterstitial listener) 
Sets the listener that will receive the Interstitial callbacks. See [IInterstitialListener](#IInterstitialListener)
for more details and available callback methods.

Usage:

```cs
  // MyInterstitialListener.cs
  public class MyInterstitialListener : ITapForTapInterstitial
  {
    public void onDismiss()
    {
      Debug.Log("Called my interstitial listener OnDismiss");
    }
  }

  // MyTapForTap.cs
  MyInterstitialListener myInterstitialListener = new MyInterstitialListener();
  TapForTap.setInterstitiallListener(myInterstitialListener);
```

### <a name="IAdViewListener"/>IAdViewListener

### <a name="IAppWallListener"/>IAppWallListener

### <a name="IInterstitialListener"/>IInterstitialListener

### <a name="TapForTapHorizontalAlignment"/>TapForTapHorizontalAlignment
### <a name="TapForTapVerticalAlignment"/>TapForTapVerticalAlignment

======

## [Back To Documentaion](http://tapfortap.github.com/Documentation/) ##

### *Support* ###
Things don't always go according to plan. If you hit a snag somewhere and need a hand don't hesitate to get in touch with us at [support@tapfortap.com](mailto:support@tapfortap.com) or on Zendesk [here](https://tapfortap.zendesk.com/anonymous_requests/new).

### *License* ###
Licensed under the terms of the MIT License.

Copyright &copy; 2012 Tap for Tap Promotions Inc.
