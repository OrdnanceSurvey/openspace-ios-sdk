Description
-------

The ordnancesurvey-ios-sdk enables access to [Ordnance Survey](http://www.ordnancesurvey.co.uk/) services for iOS based devices. It provides access to a number of mapping layers and gazetteer lookups from the OS OpenSpace service and has a similar API to Apple's Mapkit, so that moving from Apple mapping to OS Openspace is simple, see [Converting](#converting-apple-mapkit).

Developers who wish to use the OpenSpace on-line services will need to register and obtain an API Key for either the Ordnance Survey [OpenSpace](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/api/) or [OpenSpace Pro](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/) services.

This SDK is available as a static framework, see the [Getting started](#getting-started) guide for instructions about downloading and importing into your own application or try a [demo app](#demo-projects) to get started quickly.

![LocateMe-ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me/raw/master/screenshot.png "Screenshot of locate me demo app") ![TileSource-ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources/raw/master/screenshot.png "Screenshot of tilesource demo app") ![OverlayFinder-ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder/raw/master/screenshot.png "Screenshot of overlay-finder demo app")




#### Features

Here are some of the features available

- Native iOS framework to incorporate Ordnance Survey mapping
- OS OpenSpace Pro and free services
- Select which products are displayed - see [products available](#product-code-list)
- Zoom and pan controls - native touch gesture recogonisers provide tap, pinch zoom control, etc
- Annotations - create and customise annotations
- Overlays - create and style polylines and polygons
- Offline tile storage - read [about offline tile packages](#offline-databases)
- Geocoder - Search 1:50K Gazetteer, OS Locator and Codepoint Open datasets
- Uses [OSGB36 British National Grid](http://www.ordnancesurvey.co.uk/oswebsite/support/the-national-grid.html) map projection - ordnancesurvey-ios-sdk converts between WGS84 latitude/longitude and OSGB36 National Grid easting/northing. Most Classes handle geometry in either a CLLocationCoordinate2D or OSGridPoint
- User location - ordnancesurvey-ios-sdk provides an wrapper around standard Core Location API to easily display your app's user location on the map and use the data
- ARC support


Contents
-------

Within the ordnancesurvey-ios-sdk project we have:

1. /TBC - contains the ordnancesurvey-ios-sdk framework and Appledoc documentation packages
2. [Documentation](http://ordnancesurvey.github.com/ordnancesurvey-ios-sdk/) - The documentation for the latest version of ordnancesurvey-ios-sdk in appledoc format



###### Demo projects

There are several demo projects as other github repos to get started

* [OS Locate Me](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me)
* [OS Tilesource demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources)
* [OS Mapkit conversion demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion)
* [OS Overlay Finder demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder)
 





Getting started
-------

#### API Key

Developers who wish to use online services will need to register and obtain an API Key for one of the [OS OpenSpace services](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/) in order to access the mapping and geocoding services.

#### Download framework package

Head to the TBC to download the latest OSMap zip package. Unzip the downloaded file to reveal the OSMap framework and then import into your project.



#### Import into Xcode project

1. Drag the OSMap.framework into your project
2. Select if you wish to copy the framework into your project or reference the external location
3. Ensure that the framework is added to relevant project targets

#### Dependancies

Applications will need to link against OSMap.framework and its dependencies:

* libsqlite3.dylib
* CoreGraphics.framework
* CoreLocation.framework
* QuartzCore.framework
* UIKit.framework

![Frameworks-ScreenShot](https://github.com/OrdnanceSurvey/ordnancesurvey-ios-sdk/raw/master/frameworks-scr.png "Screenshot of frameworks required in xcode")


#### Import OSMap.h

In the ViewController you wish to display, import the OSMap/OSMap.h file

<pre>
#import &lt;OSMap/OSMap.h>
</pre>


#### Displaying a map

After completing the above steps to download and import the OSMap.framework, the minimum required to display a map using the ordnancesurvey-ios-sdk is as follows:

- Instatiate an `OSMapView` in a `UIView`
- Create atleast one tile source and add to `OSMapView` instance

<pre>
//In your ViewController.m

/*
 * Assuming there is an OSMapView instance, either created via a .xib or programmatically
 */

//create OSTileSource, in this case OS OpenSpace web map source with API key and associated URL
id&lt;OSTileSource&gt; webSource = [OSMapView webTileSourceWithAPIKey:@"API_KEY" refererUrl:@"YOUR_WEBPAGE_URL" openSpacePro:true/false];

//add to OSMapView
yourMapView.tileSources = [NSArray arrayWithObject:webSource];

</pre>

Note: `OSMapView` cannot be completely initialised from a Interface Builder (`.xib`) or Storyboard, it requires at least one tile source

#### Product Code list

A developer can select which Ordnance Survey mapping products to use by  selecting on of three pre-configured map stacks or customising their app by passing the product codes as and array of strings.

NOTE: Some OpenSpace Pro products require separate licenses, please check before use.

<pre>

//select the 250K and 50K products
mapView.mapProductCodes = [NSArray arrayWithObjects:@"250KR", @"250K", @"50KR", @"50K", nil];

</pre>

<pre>

//Class methods to return pre-configured map product codes

//Default codes if not set
[OSMapView defaultMapStackProductCodes] = @"SV",@"SVR",@"50K",@"50KR",@"250K",@"250KR",@"MS",@"MSR",@"OV2",@"OV1",@"OV0"

// As above but including VMD
[OSMapView completeFreeMapStackProductCodes] = @"SV",@"SVR",@"VMD",@"VMDR",@"50K",@"50KR",@"250K",@"250KR",@"MS",@"MSR",@"OV2",@"OV1",@"OV0"

//OpenSpace Pro only
[OSMapView zoomMapStackProductCodes] = @"CS10",@"CS09",@"CS08",@"CS07",@"CS06",@"CS05",@"CS04",@"CS03",@"CS02",@"CS01",@"CS00"

//Example usage
mapView.mapProductCodes = [OSMapView completeFreeMapStackProductCodes];

</pre>

##### Full Product list

TODO: is this available on the OS website? 

// Opendata products [OS OpenSpace Free and Pro]

- "SV"   // Street view
- "SVR"  // Street view resampled
- "VMD"  // Vector Map District
- "VMDR" // Vector Map District resampled
- "50K"  // 1:50k
- "50KR" // 1:50k resampled
- "250K" // 1:250k
- "250KR"// 1:250k resampled
- "MS"   // 1:1M
- "MSR"  // 1:1M resampled
- "OV2"  // Overview 2
- "OV1"  // Overview 1
- "OV0"  // Overview 0

// Pro products [OS OpenSpace Pro]

- "VML"  // Vector Map Local
- "VMLR" // Vector Map Local resampled
- "25K"  // 1:25k
- "25KR" // 1:25k resampled

// Zoom stack products - enables consistently styled layers [OS OpenSpace Pro]

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

// Other products [OS OpenSpace Pro]

TODO: MJG to clarify if we can remove any of these

- "10K"    // 1:10k
- "10KR"   // 1:10k resampled
- "10KBW"  // 1:10k Black and White
- "10KBWR" // 1:10k Black and White resampled

//POSSIBLY REMOVE THESE

- "CSG06"  // Consistently styled 25K,50K
- "CSG07"  // Consistently styled 25K,50K
- "CSG08"  // Consistently styled 25K,50K
- "CSG09"  // Consistently styled 25K,50K


#### Offline databases


##### OSTile packages

The OSTiles format is uses the small, ubiquitous and lightweight [sqlite](http://www.sqlite.org/) database and overcomes filesystem storage problems to handle millions of images. 

OSTiles packages are sets of map tiles, each package can store several layers, a layer being a rectangular bounding box for a given Ordnance Survey product and all the tile images to fill that bounding box. The packages are intended to transport presentational map tiles and can be used as a mechanism to allow mobile applications to display tiles offline, without the need to stream tiles from a webservice.

Please refer to [OSTiles spec](ordnancesurvey-ios-sdk/tree/master/ostiles_spec.md) for more details.

##### Offline search database

TBC how we dist this


#### Converting Apple Mapkit

The ordnancesurvey-ios-sdk is intended to provide a "drop-in" replacement for MapKit and has a similar API enabling existing Mapkit based project to be converted to use Ordnance Survey mapping instead.

Many applications can be converted by simply changing the "MK" prefix to "OS" or by renaming symbols with the preprocessor:

<pre>
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
</pre>

See the [OS Mapkit conversion demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion) project for more details.

#### Versioning

Ordnance Survey will only provide and offically support the latest version of the SDK. 
TODO: will we only support latest? best efforts on older?

To get the version of SDK you are currently using;

<pre>
NSLog(@"Currently using SDK Version: %@",[OSMapView SDKVersion]);
</pre>


API
-------

In this section we will run through some of the important components in the SDK. For more details please see the [reference documentation](http://ordnancesurvey.github.com/ordnancesurvey-ios-sdk/) or any [demo app](#demo-projects) for full application usage.

#### OS Map View (`OSMapView` class)

The `OSMapView` class is the subclass of `UIView` that will provide the main interaction with the UI and the SDK, displaying maps and managing annotations and overlays.


The `OSMapView` class takes care of displaying map tiles on the screen, all UI gestures for panning and zooming. `OSMapView` implements a wrapper around iOS `CoreLocationManager` and `CLLocationManagerDelegate` to provide easy access to device location data, see User location below.

**NOTE:**

* The `OSMapView` class must be configured with a tile source - see Tilesources below.
* `OSMapView` does not support drawing maps outside Great Britain.
* `OSMapView` does not support being subclassed


#### Map View delegate (`OSMapViewDelegate` protocol)

The `OSMapView` class can have an optional delegate object to listen to events that occur on the map. For your class to respond to these events you must implement the `OSMapViewDelegate` protocol.

<pre>
#import "MapViewController.h"

/*
 * Example class definition
 */
@interface MapViewController () &lt;OSMapViewDelegate&gt;

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
  

</pre>

**NOTE:**

* All delegate methods are optional, listen to any particular event
* `OSMapViewDelegate` does not currently support the 'map loading' callbacks that MapKit does
* See any of the [demo projects](#demo-projects) for working examples


#### Tilesources (`OSTileSource` protocol)

The `OSMapView` class requires atleast one online or offline tile source to render a map.

<pre>
/*
 * In the method where you set up the mapView instance, create some tile sources
 */
 
 //For example if you had an .ostiles format file in the project bundle
 id&lt;OSTileSource&gt; packageSource = [OSMapView localTileSourceWithFileURL:[[NSBundle mainBundle] URLForResource:@"myTilePackage.ostiles" withExtension:nil]];

 
 //create web tile source with API details
id&lt;OSTileSource&gt; webSource = [OSMapView webTileSourceWithAPIKey:kOSApiKey refererUrl:kOSApiKeyUrl openSpacePro:kOSIsPro];


/*
 * Tile sources are consulted in the order they appear in the array, so here we render the local source first, falling back to the web if necessary
 */
mapView.tileSources = [NSArray arrayWithObjects: packageSource, webSource, nil];

</pre>

Each `OSTileSource` added to `OSMapView` implements the `OSTileSource` protocol and implements the following methods.

<pre>
- (OSGridRect)boundsForProductCode:(NSString *)productCode

- (bool)isLocal
</pre>

**NOTE:**

* To configure product codes, see the [products available](#product-code-list) section.
* See the [OS Tilesource](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources) demo project for working examples

#### Annotations (`OSAnnotation` protocol & `OSAnnotationView` class)

Annotations identify single point locations on the map and as with MapKit there are two object involved with displaying an annotation based on annotation data and the view object.

The `OSMapView` accepts an object that conforms to the `OSAnnotation` protocol and therefore must have atleast a `CLLocationCoordinate2D` coordinate property. The SDK includes the `OSBasicAnnotation` class that can be used with minimal configuration.

<pre>
CLLocationCoordinate2D coord = {52.205298,0.118146};
    
OSBasicAnnotation * annotation = [[OSBasicAnnotation alloc] initWithCoordinate:coord];
annotation.title = @"Hello World!!";
    
[mapView addAnnotation:annotation];
</pre>


To differentiate between the data and view objects and cope with potentially large numbers of annotations, the presentation of the annotation is handled by a view object. When an annotation comes into view `OSMapView` asks its delegate to provide an annotation view, simply implement the method below and provide your own class by subclassing `OSAnnotationView` or use the built in `OSPinAnnotationView` as below.

<pre>
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
</pre>

View objects are designed to be reused and provide performance improvements during scrolling by avoiding the creation of new view objects. To do this, pass a `reuseIdentifier` and call `OSMapView` method `dequeueReusableAnnotationViewWithIdentifier:` to get a queued object.

**NOTE:**

* See the [OS Mapkit conversion](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion) demo project for working example

#### Overlays (`OSOverlay` protocol)

Overlays follow the same pattern as annotations, except that overlays are generally used to display more complex data and as such there are several implementations of shapes and the ability to implement your own.

The `OSMapView` accepts an object that conforms to the `OSOverlay` protocol and again the SDK provides several existing implementations and their respective views.

In this example we will add the respective components for a simple square, for more examples see the [OS Overlay Finder](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder) demo project.

<pre>

OSGridPoint swCorner = {400000,400000};
    
//array of 4 corners
OSGridPoint points[] = {{swCorner.easting, swCorner.northing},
    {swCorner.easting + 1000, swCorner.northing},
    {swCorner.easting + 1000, swCorner.northing + 1000},
    {swCorner.easting, swCorner.northing + 1000}};

OSPolygon *square = [OSPolygon polygonWithGridPoints: points count: 4];
    
[mapView addOverlay: square];

</pre>

There are existing SDK classes for the following shapes, please see reference documentation for more details:

* Polygon - with or without interior polygons
* Polyline
* Circle


**NOTE:**

* See the [OS Overlay Finder](https://github.com/OrdnanceSurvey/ios-sdk-demo-overlay-finder) demo project for working examples

#### Geocoding (`OSGeocoder` class)

The `OSGeocoder` class provides offline search facility against the following datasets; 

* [1:50 000 Scale Gazetteer](http://www.ordnancesurvey.co.uk/oswebsite/products/50k-gazetteer/index.html) - Place names
* [Code-Point Open](http://www.ordnancesurvey.co.uk/oswebsite/products/code-point-open/index.html) - Post codes
* [OS Locator](http://www.ordnancesurvey.co.uk/oswebsite/products/os-locator/index.html) - Road names
* Grid Reference

The product to search is determined by passing a `OSGeocodeType` to the `OSGeocoder` instance. The type `OSGeocodeTypeCombined` is Gazetteer, Postcode and GridReferences whilst `OSGeocodeTypeCombined2` also includes OS Locator.

The search can be performed within an area by passing a `OSGridRect`, to search the entire country, specify either `OSGridRectNull` or `OSNationalGridBounds`.

The number of results can be limited by specifying the `NSRange`.

The geocoding request requries a completion handler block that is called when search complete, this will pass your block an `NSError *error` and an array of search results `NSArray *placemarks`, if any. This array contains the respective objects for the search; `OSRoad` or `OSPlacemark`. These can be presented via your own implementation from the completion handler.


**NOTE:**

* The `OSGeocoder` class requires an offline database - See [about offline databases](#offline-databases) 
* See the [OS LocateMe](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me) demo project for working example
* There are currently no reverse geocoding facilities



#### Geometry

Conversion between `CLLocationCoordinate2D` and `OSGridPoint` is handled internally — applications should not perform unnecessary conversions. Coordinates should be stored in their source coordinate system in order to benefit from future accuracy improvements within the SDK. 

`OSGridPoint` - represents OSGB36 British National Grid easting/northing

`CLLocationCoordinate2D` - represents WGS84 latitude/longitude


Conversions can be performed via this SDK if required using the functions below;

<pre>
OSGridPoint OSGridPointForCoordinate(CLLocationCoordinate2D coordinate);

CLLocationCoordinate2D OSCoordinateForGridPoint(OSGridPoint gridPoint);
</pre>

#### User location (`OSUserLocation` class)

The SDK provides a wrapper around standard Core Location API and so allows you to use the location data by conforming to the `OSMapViewDelegate` protocol and implementing the relevant delegate methods to receive updates.

When enabled, a distinct annotation will be added to the map complete with estimated GPS accuracy display in the form of a circle around the annotation, a large circle represents a low accuracy and large margin of error in determining the device location.

To start receiving updates, turn on user location:

<pre>
mapView.showsUserLocation = YES/NO;
</pre>

You will now receive location updates, implement the required delegate methods to perform actions on events as required.

<pre>
- (void)mapView:(OSMapView *)mapView didUpdateUserLocation:(OSUserLocation *)userLocation
</pre>


Sample UserLocationView:
![UserLocation-ScreenShot](https://github.com/OrdnanceSurvey/ordnancesurvey-ios-sdk/raw/master/userlocation-scr.png "Screenshot of user location annotation")


**NOTE:**

* See the [OS LocateMe](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me) demo project for working example

#### Scale virew

Scale virew


Issues
--------

For any issues relating to developing with the SDK, obtaining API keys or service problems, please head over to the [OS Openspace forums](https://openspaceforums.ordnancesurvey.co.uk/openspaceforum//index.jspa) and we will investigate and get back to you.



License
-------

The Ordnance Survey iOS SDK is protected by © Crown copyright – Ordnance
Survey 2012. It is subject to licensing terms granted by Ordnance Survey, the
national mapping agency of Great Britain.

The Ordnance Survey iOS SDK includes the Route-Me library. The Route-Me
library is copyright (c) 2008-2012, Route-Me Contributors All rights reserved
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