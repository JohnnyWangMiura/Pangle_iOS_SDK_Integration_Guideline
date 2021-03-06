
# 5. Banner Ads

## Introduction
Banner ads occupy a spot within an app's layout, either at the top or bottom of the device screen.

**Note: Pangle only supports 300x250 and 320x50 for the traffic outside of Chinese Mainland at this time.**

## Precondition
Create an app and banner ad placement on Pangle platform
 - Create an application: [Application] -> [App] -> [Add App]
    - Reference：[How do I create a new App?](https://www.pangleglobal.com/jp/help/doc/5dd362e23d7897001168e334)

  - Create an ad placement：[Application] -> [Ad Placements] -> [Add Ad Placement] -> [Banner Ads]
    - Reference：[How do I create an ad placement?](https://www.pangleglobal.com/jp/help/doc/5e62079cfe8738000fd184cf)
    
## Banner Implementation

### Create Banner Object and Request Ads

#### BUNativeExpressBannerView
Banner ads are requested by `BUNativeExpressBannerView` object. The first step in using one is to instantiate it and set its ad placement Id. Then call `loadAdData` on `BUNativeExpressBannerView` to load a banner ad and call `addSubview` to add banner view into your app layout. Size should be passed in 'point' for iOS

Requied：

| Field Definition | Field Name | Field Type | Remarks                                                            |
|------------------|------------|------------|--------------------------------------------------------------------|
| slotID           | slot  ID   | NSString   | ad placement ID                                                        |
| adSize           | ad size    | CGSize     | Ad size must be the same size as the pangle platform configuration, it should be passed in 'point' on iOS |

Create a `BUNativeExpressBannerView` object, and call `loadAdData` on `BUNativeExpressBannerView` to load a banner ad.

```objective-c
self.bannerView = [[BUNativeExpressBannerView alloc] initWithSlotID:@"Your_Ad_Placement_Id" rootViewController:self adSize:CGSizeMake(screenWidth, bannerHeigh) IsSupportDeepLink:YES];
self.bannerView.frame = CGRectMake(0, self.view.height-bannerHeigh, screenWidth, bannerHeigh);
self.bannerView.delegate = self;
[self.bannerView loadAdData];
```

Optional：

| Field Definition | Field Name        | Field Type | Remarks                                                              |
|------------------|-------------------|------------|----------------------------------------------------------------------|
| interval         | Rotation interval | NSInteger  | The interval of rotation was 30s to 120s. |

### BUNativeExpressBannerViewDelegate Callback

|      BUNativeExpressBannerViewDelegate Callback      |      Description         |
|----------------------------------------------------------------------|--------------------------------------------------------|
| nativeExpressBannerAdViewDidLoad:                                   | This method is called when bannerAdView ad slot loaded successfully.          |
| nativeExpressBannerAdView: didLoadFailWithError:                    | This method is called when bannerAdView ad slot failed to load.               |             
| nativeExpressBannerAdViewRenderSuccess:                             | This method is called when rendering a nativeExpressAdView successed.          |
| nativeExpressBannerAdViewRenderFail:error:                          | This method is called when a nativeExpressAdView failed to render.If the rendering fails due to network or hardware reasons, you can change the phone or the network environment. It is recommended to upgrade to the latest version of the Pangle platform.  |
| nativeExpressBannerAdViewWillBecomVisible:                          | This method is called when bannerAdView ad slot shows new ad.               |
| nativeExpressBannerAdViewDidClick:                                  | This method is called when bannerAdView is clicked.                          |
| nativeExpressBannerAdView:dislikeWithReason:                        | This method is called when the user clicked dislike button and chose dislike reasons.  |
| nativeExpressBannerAdViewDidCloseOtherController: interactionType:  | This method is called when another controller has been closed.  interactionType : open appstore in app or open the webpage or view video ad details page.    |

### Display Banner
To show a banner ad, check the `nativeExpressBannerAdViewDidLoad` callback to verify that if the ad is returned. Then check the `nativeExpressBannerAdViewRenderSuccess` to verify if the banner view is rendered successfully. Then a`ddSubview` to display the banner ad.

Instance:
```objective-c
- (void)showBanner {
    [self.view addSubview:self.bannerView];
}
```

#### Banner showtime
Banner ad could only be displayed after receiving `nativeExpressBannerAdViewRenderSuccess`

```objective-c
- (void)nativeExpressBannerAdViewRenderSuccess:(BUNativeExpressBannerView *)bannerAdView {
 //After the callback method, the advertisement is displayed, which can ensure the smooth playing and display, and the user experience is better.
}
```

## Test with test ads

Now you have finished the integration. If you wanna test your apps, make sure you use test ads rather than live, production ads. The easiest way to load test ads is to use test mode. It's been specially configured to return test ads for every request, and you're free to use it in your own apps while coding, testing, and debugging. 

Refer to the [How to add a test device?](https://www.pangleglobal.com/help/doc/5fba365f7b550100157bfc06) to add your device to the test devices on Pangle platform.



### Note
1. At present, banner ads support the center display of the bottom or top of the content, and developers can adjust the left and right margins by themselves.

### Resource
Demo: [GitHub](https://github.com/bytedance/Bytedance-UnionAD/blob/master/Example/BUDemo/App/Example/controller/BUDExpressBannerViewController.m)

