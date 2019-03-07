# cordova-plugin-idscan
A Cordova plugin for the camera scanner and ID parser SDK's from [IDScan.net](http://IDScan.net). Use it to scan drivers licenses and other IDs with the device camera, and parse out the data. Android and iOS are supported.

A valid license key is required for both SDKs from IDScan.net for the plugin to work. You only need keys for the platform you are using.

To install the plugin directly from here, use the Cordova CLI command:

    cordova plugin add https://github.com/IDScanNet/cordova-plugin-idscan

Or you can clone the repository locally if you would like to modify the code and install from there. The plugin is exposed to your Cordova javascript code as the `IDScanner` object. Simply call the `scan()` function like so:
```javascript
    function successCallback(result) {
      console.log("Successfully scanned ID for " + result.fullName);
      console.log("Birth date: " + result.birthdate);
    }

    function errorCallback(errorMsg) {
      console.log(errorMsg);
    }

    IDScanner.scan(successCallback, errorCallback, cameraKey, parserKey);
```
For all fields available on the result object returned from the IDScanner, refer to DriverLicenseParser.h in the iOS sdk (e.g. firstName, lastName, address1, address2, city, postalCode, licenseNumber, expirationDate, issueDate, etc.)
    
If you need to update the SDK files from IDScan.net, you can just drop new ones in the sdk folders of the iOS and Android source.


## iOS Quirks

Since iOS 10 it's mandatory to provide an usage description in the `info.plist` if trying to access privacy-sensitive data. When the system prompts the user to allow access, this usage description string will displayed as part of the permission dialog box, but if you didn't provide the usage description, the app will crash before showing the dialog. Also, Apple will reject apps that access private data but don't provide an usage description.

This plugins requires the following usage descriptions:

- `NSCameraUsageDescription` specifies the reason for your app to access the device's camera.
- `NSPhotoLibraryUsageDescription` specifies the reason for your app to access the user's photo library.
- `NSLocationWhenInUseUsageDescription` specifies the reason for your app to access the user's location information while your app is in use. (Set it if you have `CameraUsesGeolocation` preference set to `true`)
- `NSPhotoLibraryAddUsageDescription` specifies the reason for your app to get write-only access to the user's photo library

To add these entries into the `info.plist`, you can use the `edit-config` tag in the `config.xml` like this:

```xml
<edit-config target="NSCameraUsageDescription" file="*-Info.plist" mode="merge">
    <string>need camera access to take pictures</string>
</edit-config>
```

```xml
<edit-config target="NSPhotoLibraryUsageDescription" file="*-Info.plist" mode="merge">
    <string>need photo library access to get pictures from there</string>
</edit-config>
```