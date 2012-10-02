Description
-------

The ordnancesurvey-ios-sdk enables access to [Ordnance Survey](http://www.ordnancesurvey.co.uk/) services for iOS based devices. It provides access to a number of mapping layers and gazetteer lookups from the OS OpenSpace service and has a largely similar API to Apple's Mapkit; see [Converting](#converting-apple-mapkit).

Developers who wish to use the OpenSpace on-line services will need to register and obtain an API Key for either the Ordnance Survey [OpenSpace](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/api/) or [OpenSpace Pro](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/) services.

This SDK is available as a static framework, please see the [Getting started](#getting-started) guide for instructions about downloading and importing into your own application or try a demo app.

#### Features

Here are some of the features available

- Native iOS framework incorporating OS mapping
- OS OpenSpace Pro and free services - [layers available](#product-code-list)
- Zoom and pan controls - incorporated gesture recogonisers to provide tap and pinch zoom control
- Annotations - create and customise annotations
- Overlays - create and style polylines and polygons
- Offline tile storage - SEE TODO
- Geocoder - Search 1:50k Gazetteer & TODO: postcode product? is it codepoint?
- Uses OSGB36 National Grid map projection - ordnancesurvey-ios-sdk converts between WGS84 latitude/longitude and OSGB36 National Grid easting/northing


Contents
-------

Within the ordnancesurvey-ios-sdk project we have:

- /downloads - contains the ordnancesurvey-ios-sdk framework and documentation packages
- There are several demo projects to get started with such as TODO


Getting started
-------

#### API Key

Developers who wish to use online services will need to register and obtain an API Key for one of the [OS OpenSpace services](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/) in order to access the mapping service.

#### Download framework package

Notes about downloading zip

#### Import into Xcode project

1. Drag the OSMap.framework into your project
2. TODO

#### Dependancies

Applications will need to link against OSMap.framework and its dependencies:

* libsqlite3.dylib
* CoreGraphics.framework
* CoreLocation.framework
* QuartzCore.framework
* UIKit.framework

#### Licence and Attribution

need to include? TODO

#### Converting Apple Mapkit

The ordnancesurvey-ios-sdk is intended to provide a "drop-in" replacement for MapKit and has a largely similar API enabling existing Mapkit based project to use OS mapping instead.

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


#### Displaying a map

After completing the above steps to download and import the OSMap.framework, the minimum required to display a map using the ordnancesurvey-ios-sdk is as follows:

- Instatiate an OSMapView in a UIView
- Create atleast one OSTileSource and add to OSMapView

<pre>
In your ViewController.m

//create OSTileSource, in this case OS OpenSpace web map source with API key and associated URL
id<OSTileSource> webSource = [OSMapView webTileSourceWithAPIKey:@"API_KEY" refererUrl:@"YOUR_WEBPAGE_URL" openSpacePro:true/false];

//add to OSMapView
mapView.tileSources = [NSArray arrayWithObject:webSource];

</pre>

#### Product Code list

A developer can select which Ordnance Survey products to use by passing the product codes as and array of strings. There are three pre-configured map stacks available.

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

</pre>

##### Full list

TODO: is this available on the OS website? If not, why not

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

- "10K"    // 1:10k
- "10KR"   // 1:10k resampled
- "10KBW"  // 1:10k Black and White
- "10KBWR" // 1:10k Black and White resampled
- "CSG06"  // TODO ??
- "CSG07"  // TODO
- "CSG08"  // TODO
- "CSG09"  // TODO

License
-------

Some notes about license & use TODO