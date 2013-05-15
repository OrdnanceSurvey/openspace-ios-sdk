Feed your apps with OS maps
-------

The openspace-ios-sdk enables access to [Ordnance Survey](http://www.ordnancesurvey.co.uk/) Web Map Tile Services (WMTS) for iOS based devices. It provides access to a number of mapping layers and gazetteer lookups and has a similar API to Apple's Mapkit, so that moving from Apple mapping to Ordnance Survey mapping is simple, see [Converting](#converting-apple-mapkit).

This SDK is available as a static framework, see the [Getting started](#getting-started) guide for instructions about downloading and importing into your own application or try a [demo app](#demo-projects) to get started quickly.

View available mapping layers [here] (http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/products.html)

![LocateMe-ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me/raw/master/screenshot.png "Screenshot of locate me demo app") ![TileSource-ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources/raw/master/screenshot.png "Screenshot of tilesource demo app") 
![VMD-ScreenShot](https://github.com/OrdnanceSurvey/ordnancesurvey-ios-sdk/raw/master/vmd-iphone-scr.png "Screenshot of VMD on iphone")


#### Features and benefits

Here are some of the features available

- Native iOS framework to incorporate Ordnance Survey mapping
- Select which products are displayed - see [products available](#product-codes)
- Zoom and pan controls - native touch gesture recogonisers provide tap, pinch zoom control, etc
- Annotations - create and customise annotations
- Overlays - create and style polylines and polygons
- Offline tile storage - read [about offline tile packages](#offline-databases)
- Geocoder - Search 1:50K Gazetteer, OS Locator and Codepoint Open datasets available either online or offline
- Uses [OSGB36 British National Grid](http://www.ordnancesurvey.co.uk/oswebsite/support/the-national-grid.html) map projection - ordnancesurvey-ios-sdk converts between WGS84 latitude/longitude and OSGB36 National Grid easting/northing. Most Classes handle geometry in either a CLLocationCoordinate2D or OSGridPoint
- User location - openspace-ios-sdk provides a wrapper around the standard Core Location API to easily display your app's user location on the map and use the data
- ARC (Automatic Reference Counting) support
- Street level mapping features detailed buildings property boundaries and accurate road network.
- World famous countryside and National park mapping featuring accurate tracks, paths and fields.

Here are some of the benefits

- As easy to use as the Apple SDK
- Most accurate and detailed mapping of GB, so you will have confidence when it comes to location.
- Fast rendering
- Online and offline capabilities
- Complements our service – another way to get our data.


Contents
-------

The openspace-ios-sdk itself comprises of a couple of items:

1. The openspace-ios-sdk framework is downloaded from [www.ordnancesurvey.co.uk](https://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/ios-sdk.html) 
2. [Documentation](http://ordnancesurvey.github.com/ordnancesurvey-ios-sdk/) - The documentation for the latest version of openspace-ios-sdk in appledoc format

There are several extensions available:

1.  OSTile offline [tile packages](#ostile-packages)
2.  Offline [POI database](#poi-database)



###### Demo projects

Check out a demo project to get started:

* [OS Locate Me](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me)
* [OS Tilesource demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources)
* [OS Mapkit conversion demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion)
* [OS Overlay Finder demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder)
 
Getting started
-------

### Registration and Access

In order to access our web services you must apply for an API key, by registering for a licence:-

- A free 90 day trial licence (for internal trial and testing)
- A free 90 day trial licence (for external trial and demonstrating)
- A commercial licence 

see http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/free-trial.html or contact Newbusinessenquiries@ordnancesurvey.co.uk or calling 08456 05 05 05

###Authentication parameters

When registering for an API key we'll need a couple of items:

#####App ID (The Bundle ID) of the application that will use the API key.

Example: uk.co.ordnancesurvey.demo.myDemoApp

Let us know the Bundle Identifier of the Xcode project in which you will be using the API key. This is available and configurable when creating an Xcode project or from project settings or `Info.plist`

######Apple ID to be associated with this application

Example: 577097874

Let us know this unique string generated when you have created an iOS application placeholder in iTunes connect and can be used to identify an application and view the iTunes page. For enterprise apps, this is not required.  



### Registration Process for PSMA customers

If you own a data licence, for example, you are a member of the PSMA, you can register for a licence in order to obtain an API key to access the service.(http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-ondemand/pricing.html).

### Download framework package

Download the latest openspace-ios-sdk static framework from [www.ordnancesurvey.co.uk](https://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/ios-sdk.html)

Unzip the downloaded file to reveal the OSMap framework and then import into your project.



### Import into Xcode project

1. Drag the OSMap.framework into your project
2. Select if you wish to copy the framework into your project or reference the external location
3. Ensure that the framework is added to relevant project targets

### Dependancies

Applications will need to link against OSMap.framework and its dependencies:

* libsqlite3.dylib
* CoreGraphics.framework
* CoreLocation.framework
* QuartzCore.framework
* UIKit.framework
* SystemConfiguration.framework

![Frameworks-ScreenShot](https://github.com/OrdnanceSurvey/ordnancesurvey-ios-sdk/raw/master/frameworks-scr.png "Screenshot of frameworks required in xcode")


### Import OSMap.h

In the ViewController you wish to display your map, import the OSMap/OSMap.h file

```objective-c
#import <OSMap/OSMap.h>
```


### Displaying a map

After completing the above steps to download and import the OSMap.framework, the minimum required to display a map using the openspace-ios-sdk is as follows:

- Initiate an `OSMapView` in a `UIView`
- Create at least one tile source and add to `OSMapView` instance

```objective-c
//In your ViewController.m

/*
 * Assuming there is an OSMapView instance, either created via a .xib or programmatically
 */

//create OSTileSource, in this case OS OpenSpace web map source with API key and associated URL
id<OSTileSource> webSource = [OSMapView webTileSourceWithAPIKey:@"API_KEY" openSpacePro:YES/NO];

//add to OSMapView
yourMapView.tileSources = [NSArray arrayWithObject:webSource];

```

Note: `OSMapView` cannot be completely initialised from a Interface Builder (`.xib`) or Storyboard, it requires at least one tile source

### Product Codes

A developer can select which Ordnance Survey mapping products to use by  selecting on of three pre-configured map stacks or customising their app by passing the product codes as and array of strings.

NOTE: Some OpenSpace Pro products require separate licenses, please check before use.

```objective-c

//select the 250K and 50K products
mapView.mapProductCodes = [NSArray arrayWithObjects:@"250KR", @"250K", @"50KR", @"50K", nil];

```

```objective-c

//Class methods to return pre-configured map product codes

//Default codes if no others set
[OSMapView defaultMapStackProductCodes] = @"SV",@"SVR",@"50K",@"50KR",@"250K",@"250KR",@"MS",@"MSR",@"OV2",@"OV1",@"OV0"

// As above but including VMD
[OSMapView completeFreeMapStackProductCodes] = @"SV",@"SVR",@"VMD",@"VMDR",@"50K",@"50KR",@"250K",@"250KR",@"MS",@"MSR",@"OV2",@"OV1",@"OV0"

//OpenSpace Pro only
[OSMapView zoomMapStackProductCodes] = @"CS10",@"CS09",@"CS08",@"CS07",@"CS06",@"CS05",@"CS04",@"CS03",@"CS02",@"CS01",@"CS00"

//Example usage
mapView.mapProductCodes = [OSMapView completeFreeMapStackProductCodes];

```

#### Full Product list

// OpenSpace products

- "SV"   // Street view
- "SVR"  // Street view resampled
- "VMD"  // Vector Map District
- "VMDR" // Vector Map District resampled
- "250K" // 1:250k
- "250KR"// 1:250k resampled
- "MS"   // 1:1M
- "MSR"  // 1:1M resampled
- "OV2"  // Overview 2
- "OV1"  // Overview 1
- "OV0"  // Overview 0

// Licenced products

- "VML"  // Vector Map Local
- "VMLR" // Vector Map Local resampled
- "25K"  // 1:25k
- "25KR" // 1:25k resampled
- "50K"  // 1:50k
- "50KR" // 1:50k resampled

// Zoom stack products - enables consistently styled layers

- "CS00" // "zoomed out"
- "CS01"
- "CS02"
- "CS03"
- "CS04"
- "CS05"
- "CS06"
- "CS07"
- "CS08"
- "CS09"
- "CS10" // "zoomed in"


### Offline databases

These offline databases are extensions to the openspace-ios-sdk and can replace online access or supplement it, they can help your application overcome network coverage issues and function wherever the user is located. 

##### OSTile packages

The OSTiles format is uses the small, ubiquitous and lightweight [sqlite](http://www.sqlite.org/) database and overcomes filesystem storage problems to handle millions of images. 

OSTiles packages are sets of map tiles, each package can store several layers, a layer being a rectangular bounding box for a given Ordnance Survey product and all the tile images to fill that bounding box. The packages are intended to transport presentational map tiles and can be used as a mechanism to allow mobile applications to display tiles offline, without the need to stream tiles from a web service.

Please refer to [OSTiles spec](ordnancesurvey-ios-sdk/ostiles_spec.md) for more details and how to create these packages.

#### POI database

The points of interest database is created from [OpenData](https://www.ordnancesurveyite.co.uk/oswebsite/products/os-opendata.html) products, it optimised for size and is packaged in the [sqlite](http://www.sqlite.org/) format as the OSTiles except with the custom file extension `.ospoi`.

This database can be downloaded from [www.ordnancesurvey.co.uk](https://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/ios-sdk.html).


### Converting Apple Mapkit

The openspace-ios-sdk is intended to provide a "drop-in" replacement for MapKit and has a similar API enabling existing Mapkit based project to be converted to use Ordnance Survey mapping instead.

Many applications can be converted by simply changing the "MK" prefix to "OS" or by renaming symbols with the preprocessor:

```objective-c
#import "OSMap/OSMap.h"

#define MKAnnotation OSAnnotation
#define MKPointAnnotation OSPointAnnotation
#define MKUserLocation OSUserLocation
#define MKMapViewDelegate OSMapViewDelegate
#define MKMapView OSMapView
#define MKAnnotationView OSAnnotationView
#define MKPinAnnotationView OSPinAnnotationView
#define MKOverlay OSOverlay
#define MKOverlayView OSOverlayView
#define MKCircle OSCircle
#define MKCircleView OSCircleView
#define MKPolygon OSPolygon
#define MKPolygonView OSPolygonView
#define MKPolyline OSPolyline
#define MKPolylineView OSPolylineView
#define MKOverlayPathView OSOverlayPathView
#define MKPinAnnotationColor OSPinAnnotationColor
#define MKPinAnnotationColorRed OSPinAnnotationColorRed
#define MKPinAnnotationColorPurple OSPinAnnotationColorPurple
#define MKAnnotationViewDragState OSAnnotationViewDragState
#define MKCoordinateRegion OSCoordinateRegion
#define MKCoordinateSpan OSCoordinateSpan
#define MKUserTrackingMode OSUserTrackingMode
#define MKUserTrackingModeNone OSUserTrackingModeNone
#define MKUserTrackingModeFollow OSUserTrackingModeFollow
#define MKUserTrackingModeFollowWithHeading OSUserTrackingModeFollowWithHeading

#import "OSMap/OSMap+MapPointCompat.h"

#define MKMapRect OSMapRect
#define MKMapPoint OSMapPoint
```

See the [OS Mapkit conversion demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion) project for more details.

### Versioning

Ordnance Survey will provide and offically support the latest version of the SDK. 

To get the version of SDK you are currently using;

```objective-c
NSLog(@"You are currently using SDK Version: %@", [OSMapView SDKVersion]);
```


API
-------

In this section we will run through some of the important components in the SDK. For more details please see the [reference documentation](http://ordnancesurvey.github.com/ordnancesurvey-ios-sdk/) or any [demo app](#demo-projects) for full application usage.

### OS Map View (`OSMapView` class)

The `OSMapView` class is the subclass of `UIView` that will provide the main interaction with the UI and the SDK, displaying maps and managing annotations and overlays.


The `OSMapView` class takes care of displaying map tiles on the screen, all UI gestures for panning and zooming. `OSMapView` implements a wrapper around iOS `CoreLocationManager` and `CLLocationManagerDelegate` to provide easy access to device location data, see User location below.

**NOTE:**

* The `OSMapView` class must be configured with a tile source - see Tilesources below.
* `OSMapView` does not support drawing maps outside Great Britain.
* `OSMapView` does not support being subclassed

**DEMO**

See any of the [demo projects](#demo-projects) for working examples


### Map View delegate (`OSMapViewDelegate` protocol)

The `OSMapView` class can have an optional delegate object to listen to events that occur on the map. For your class to respond to these events you must implement the `OSMapViewDelegate` protocol.

```objective-c
#import "MapViewController.h"

/*
 * Example class definition
 */
@interface MapViewController () <OSMapViewDelegate>

@end

/../

/*
 * In the method where you set up the mapView instance, set its delegate to your view controller
 */
 mapView.delegate = self;
 
 /../
 
 /*
  * Implement any of the delegate methods, such as:
  */
  #pragma mark OSMapViewDelegate methods
  - (void)mapView:(OSMapView *)map regionDidChangeAnimated:(BOOL)animated
  {
  	//Do something
  }
  

```

**NOTE:**

* All delegate methods are optional, listen for any singular event
* `OSMapViewDelegate` does not currently support the 'map loading' callbacks that MapKit does

**DEMO**

See any of the [demo projects](#demo-projects) for working examples


### Tilesources (`OSTileSource` protocol)

The `OSMapView` class requires atleast one online or offline tile source to render a map.

```objective-c
/*
 * In the method where you set up the mapView instance, create some tile sources
 */
 
 //For example if you had an .ostiles format file in the project bundle
 id<OSTileSource> packageSource = [OSMapView localTileSourceWithFileURL:[[NSBundle mainBundle] URLForResource:@"myTilePackage.ostiles" withExtension:nil]];

 
 //create web tile source with API details
id<OSTileSource> webSource = [OSMapView webTileSourceWithAPIKey:@"Your_API_Key"y openSpacePro:YES];


/*
 * Tile sources are consulted in the order they appear in the array, so here we render the local source first, falling back to the web if necessary
 */
mapView.tileSources = [NSArray arrayWithObjects: packageSource, webSource, nil];

```

Each `OSTileSource` added to `OSMapView` conforms to the `OSTileSource` protocol and implements the following methods:

```objective-c

- (OSGridRect)boundsForProductCode:(NSString *)productCode

```

**NOTE:**

* To configure product codes, see the [products available](#product-codes) section.

**DEMO**

* See the [OS Tilesource](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources) demo project for working examples

### Annotations (`OSAnnotation` protocol & `OSAnnotationView` class)

Annotations identify single point locations on the map and as with MapKit there are two object involved with displaying an annotation based on annotation data and the view object.

The `OSMapView` accepts an object that conforms to the `OSAnnotation` protocol and therefore must have atleast a `CLLocationCoordinate2D` coordinate property. The SDK includes the `OSBasicAnnotation` class that can be used with minimal configuration.

```objective-c
CLLocationCoordinate2D coord = {52.205298,0.118146};
    
OSBasicAnnotation * annotation = [[OSBasicAnnotation alloc] initWithCoordinate:coord];
annotation.title = @"Hello World!!";
    
[mapView addAnnotation:annotation];
```


To differentiate between the data and view objects and cope with potentially large numbers of annotations, the presentation of the annotation is handled by a view object. When an annotation comes into view `OSMapView` asks its delegate to provide an annotation view, simply implement the method below and provide your own class by subclassing `OSAnnotationView` or use the built in `OSPinAnnotationView` as below.

```objective-c
-(OSAnnotationView*)mapView:(OSMapView *)mapView viewForAnnotation:(id<OSAnnotation>)annotation
{
    // Use the default user location view.
    if ([annotation isKindOfClass: [OSUserLocation class]])
    {
        return nil;
    }
    
    /*
     * Create a OSPinAnnotationView and configure, for more details about options, see reference documentation
     */
    OSPinAnnotationView *view = [[OSPinAnnotationView alloc] initWithAnnotation: annotation reuseIdentifier: nil];
    view.animatesDrop = YES;
    view.canShowCallout = YES;
    view.draggable = NO;
    view.pinColor = OSPinAnnotationColorRed;
    return view;

}
```

View objects are designed to be reused and provide performance improvements during scrolling by avoiding the creation of new view objects. To do this, pass a `reuseIdentifier` and call `OSMapView` method `dequeueReusableAnnotationViewWithIdentifier:` to get a queued object.

**DEMO**

* See the [OS Mapkit conversion](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion) demo project for working example

### Overlays (`OSOverlay` protocol)

Overlays follow the same pattern as annotations, except that overlays are generally used to display more complex data and as such there are several implementations of shapes and the ability to implement your own.

The `OSMapView` accepts an object that conforms to the `OSOverlay` protocol and again the SDK provides several existing implementations and their respective views.

In this example we will add the respective components for a simple square, for more examples see the [OS Overlay Finder](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder) demo project.

```objective-c

OSGridPoint swCorner = {400000,400000};
    
//array of 4 corners
OSGridPoint points[] = {{swCorner.easting, swCorner.northing},
    {swCorner.easting + 1000, swCorner.northing},
    {swCorner.easting + 1000, swCorner.northing + 1000},
    {swCorner.easting, swCorner.northing + 1000}};

OSPolygon *square = [OSPolygon polygonWithGridPoints: points count: 4];
    
[mapView addOverlay: square];

```

There are existing SDK classes for the following shapes, please see reference documentation for more details:

* Polygon - with or without interior polygons
* Polyline
* Circle


**DEMO**

* See the [OS Overlay Finder](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder) demo project for working examples

### Geocoding (`OSGeocoder` class)

The `OSGeocoder` class provides offline search facility against the following datasets; 

* [1:50 000 Scale Gazetteer](http://www.ordnancesurvey.co.uk/oswebsite/products/50k-gazetteer/index.html) - Place names
* [Code-Point Open](http://www.ordnancesurvey.co.uk/oswebsite/products/code-point-open/index.html) - Post codes
* [OS Locator](http://www.ordnancesurvey.co.uk/oswebsite/products/os-locator/index.html) - Road names
* Grid Reference

Along with online search facility against the following datasets; 

* [1:50 000 Scale Gazetteer](http://www.ordnancesurvey.co.uk/oswebsite/products/50k-gazetteer/index.html) - Place names
* [Code-Point Open](http://www.ordnancesurvey.co.uk/oswebsite/products/code-point-open/index.html) - Post codes


The product to search is determined by passing a `OSGeocodeType` to the `OSGeocoder` instance. The type `OSGeocodeTypeCombined` is Gazetteer, Postcode and GridReferences whilst `OSGeocodeTypeCombined2` also includes OS Locator.

The online variety of search types are specified by containing online such as `OSGeocodeTypeOnlineGazetteer` and `OSGeocodeTypeOnlinePostcode`.

The search can be performed within an area by passing a `OSGridRect`, to search the entire country, specify either `OSGridRectNull` or `OSNationalGridBounds`.

The number of results can be limited by specifying the `NSRange`.

The geocoding request requries a completion handler block that is called when search complete, this will pass your block an `NSError *error` and an array of search results `NSArray *placemarks`, if any. This array contains the respective objects for the search; `OSRoad` or `OSPlacemark`. These can be presented via your own implementation from the completion handler.


**NOTE:**

* The `OSGeocoder` class can work with an offline database - See [about offline databases](#offline-databases) 
* The online access requires an API key configured
* There are currently no reverse geocoding facilities

**DEMO**

* See the [OS LocateMe](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me) demo project for working example



### Geometry

Conversion between `CLLocationCoordinate2D` and `OSGridPoint` are handled internally — applications should not perform unnecessary conversions. Coordinates should be stored in their source coordinate system in order to benefit from future accuracy improvements within the SDK. 

`OSGridPoint` - represents OSGB36 British National Grid easting/northing

`CLLocationCoordinate2D` - represents WGS84 latitude/longitude


Conversions can be performed via this SDK if required using the functions below;

```objective-c
OSGridPoint OSGridPointForCoordinate(CLLocationCoordinate2D coordinate);

CLLocationCoordinate2D OSCoordinateForGridPoint(OSGridPoint gridPoint);
```

### User location (`OSUserLocation` class)

The SDK provides a wrapper around standard Core Location API and so allows you to use the location data by conforming to the `OSMapViewDelegate` protocol and implementing the relevant delegate methods to receive updates.

When enabled, a distinct annotation will be added to the map complete with estimated GPS accuracy display in the form of a circle around the annotation, a large circle represents a low accuracy and large margin of error in determining the device location.

To start or stop receiving updates, change this propery:

```objective-c
mapView.showsUserLocation = YES/NO;
```

You will now receive location updates, implement the required delegate methods to perform actions on events as required.

```objective-c
- (void)mapView:(OSMapView *)mapView didUpdateUserLocation:(OSUserLocation *)userLocation
```


Sample UserLocationView:

![UserLocation-ScreenShot](https://github.com/OrdnanceSurvey/ordnancesurvey-ios-sdk/raw/master/userlocation-scr.png "Screenshot of user location annotation")


**NOTE:**

* It is still up to the developer to turn off GPS access in certain situations, eg running background modes, low memory

**DEMO**

* See the [OS LocateMe](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me) demo project for working example

### Scale view

The SDK allows a developer to subclass the `OSMapScaleView` class and represent the current scale being displayed at that zoom level however is appropriate for their application. This view can be configured to display a scale calculated using the current `metresPerPixel`.



Issues
--------

For any issues relating to developing with the SDK, obtaining API keys or service problems, please email osopenspace@ordnancesurvey.co.uk



Licence
-------

Ordnance Survey's OpenSpace iOS SDK is protected by © Crown copyright – Ordnance Survey 2013. It is subject to licensing terms granted by Ordnance Survey, the national mapping agency of Great Britain.

The OpenSpace iOS SDK includes the Route-Me library. The Route-Me
library is copyright (c) 2008-2013, Route-Me Contributors All rights reserved
(subject to the BSD licence terms as follows):

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer. * Redistributions in binary
  form must reproduce the above copyright notice, this list of conditions and
  the following disclaimer in the documentation and/or other materials provided
  with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Route-Me depends on the Proj4 Library. [ http://trac.osgeo.org/proj/wiki/WikiStart ]
Proj4 is copyright (c) 2000, Frank
Warmerdam / Gerald Evenden Proj4 is subject to the MIT licence as follows:

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
