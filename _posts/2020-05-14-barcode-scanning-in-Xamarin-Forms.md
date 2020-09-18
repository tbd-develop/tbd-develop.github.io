---
layout: post
title:  "Barcode Scanning in Xamarin Forms"
date:   2020-05-14 23:00:00 -0400
categories: development update
---

Introduction
---

I decided to create an application that I could use to catalog my game collection. This application should allow me
to gather a name and a platform (eg. PS4, Nintendo etc.) and a barcode. The barcode is useful if I want to gather other 
information about the games such as purchase price, rarity or number of copies. 

The two main options I found for Barcode scanning were Scandit and ZXing (Zebra Crossing). I haven't done exhaustive testing 
comparing the two libraries presented here, but I've done enough to get up and running, hit some pitfalls and get them 
doing what I needed to do. 

All my testing has been done on a Samsung Galaxy S10, I do not have any iOS device available to me. 

Both libraries will need the Camera permission (and possibly Flashlight), the packages should insert the requests to do so, 
but it's worth making sure. Xamarin Essentials has made this simpler. 

Scandit    
----

Available at [Scandit.com](https://scandit.com)

Not a totally free option. There is a community edition available if you're a student or pre-commercial startup, but 
(and you should read for yourself) the terms do indicate that if you should go on to monetize your application, 
Scandit gets a percentage of each app sold. 

One major caveat of the "free" option here, is that you can only have one app per platform. You tie the license to an 
application identifer, and once you've done that you can't create another and you can't delete or rename the app associated. 

What this also prevents is being able to distribute the application via an open source product, because anyone using the scanner 
needs the license key and the app has to be the same identifier as the one registered. 

*Disclaimer*: I originally signed up for Scandit many years ago, when there was an easily available community edition available. Things 
have changed in the years in between. 

To get started;

- Sign up for an account on their site, this will give you a 30 day access to the SDK
- Contact them for access to the community edition license if you meet their requirements
- On your shared Xamarin Forms app, open your nuget packages and search for `Scandit.BarcodePicker.Unified`. According to the [Scandit Docs](https://docs.scandit.com/stable/xamarin/index.html) Unified is the version to target cross platform, but is simpler and reduced in features to if you use the platform specific versions.
- All the following instructions can be found in the [Unified SDK Docs](docs.scandit.com/stable/xamarin/xamarin-unified-integrate.html)
- At this point, make sure you have the license key available from the dashboard, you're going to need it. 
- Go to the page from which you wish to trigger scanning
- In the constructor, add the following
  
```C#
ScanditService.ScanditLicense.AppKey = "-- ENTER YOUR SCANDIT LICENSE KEY HERE --";
```
- Create a button, add OnClick handler, and in the handler add

```C#
// Configure the barcode picker through a scan settings instance by defining which
// symbologies should be enabled.
var settings = ScanditService.BarcodePicker.GetDefaultScanSettings();
// prefer backward facing camera over front-facing cameras.
settings.CameraPositionPreference = CameraPosition.Back;
// Enable symbologies that you want to scan.
settings.EnableSymbology(Symbology.UPCA, true);

ScanditService.BarcodePicker.DidScan += OnDidScan;
await ScanditService.BarcodePicker.ApplySettingsAsync(settings);
// Start the scanning process.
await ScanditService.BarcodePicker.StartScanningAsync();
```

- You can enable other symbologies if you require, but I'm scanning game barcodes. 
- You then need to create the OnDidScan method
```C#
void OnDidScan(ScanSession session)
{
    // Guaranteed to always have at least one element, so we don't have to 
    // check the size of the NewlyRecognizedCodes array.
    var firstCode = session.NewlyRecognizedCodes.First();
    var message = string.Format("Code Scanned:\n {0}\n({1})", firstCode.Data,
                                firstCode.SymbologyString.ToUpper());
    // Because this event handler is called from an scanner-internal thread, 
    // you must make sure to dispatch to the main thread for any UI work.
    Device.BeginInvokeOnMainThread(() => {
        this.ResultLabel.Text = message;
        ScanditService.BarcodePicker.DidScan -= OnDidScan;
    });
    session.StopScanning();
}
```
- If you need to show another page, you're going to do that inside the Device.BeginInvokeOnMainThread as Navigation isn't 
available in the background thread.

ZXing (Zebra Crossing)
----

Available on [Github](https://github.com/Redth/ZXing.Net.Mobile)

This is a .NET implementation of the ZXing library. ZXing itself is in maintenance mode, no longer being actively maintained, 
but this library is in ongoing development and is being updated for the latest version of Xamarin Forms at the time of writing. 

I made an attempt to use the ZXing library originally, and fell foul of some permissions issues. Mainly, when I tried to scan 
I was told the camera wasn't available. This was actually because I was trying to reference components in the XAML, rather than 
the full code based approach. I am sure there's more to learn here, but getting the barcode scan was my first priority. 

So, after realizing that Scandit wasn't really my answer, I took it back up and worked it in the same way as I had Scandit. And off 
to the races! 

- At time of writing, the latest version (3.0.0-beta) is still in pre-release, so make sure Pre-Release is checked. 
- To your shared project, open your nuget packages and add a reference to `ZXing.Net.Mobile.Forms`
- On the page you wish to initiate scanning from, add a private variable
```C#
private ZXScanningPage _scanner;
```
- In the constructor for the page
```C#
    _scanner = new ZXingScannerPage();

    _scanner.OnScanResult += BarcodePickerOnDidScan;
```
- Then, add a Button, and on the OnClick handler
```C#
    await Navigation.PushModalAsync(_scanner);
```
- Then create the method BarcodePickerOnDidScan (or whatever you want to call it)
- In the method
```C#
Device.BeginInvokeOnMainThread(async () =>
    {
        _scanner.IsScanning = false;

        await Navigation.PopModalAsync();

        if (string.IsNullOrEmpty(result.Text)) 
            return;

        string scannedCode = result.Text;
    });
```
- From here, as before, I navigated to another page from within the BeginInvoke. 
- One thing I did miss here was there is no audible beep when the scan is made as with Scandit. There are ways to add that sound, but that's not my priority right now. 
