Description
-------

The ordnancesurvey-ios-sdk enables access to [Ordnance Survey](http://www.ordnancesurvey.co.uk/) services for iOS based devices. It provides access to a number of mapping layers and gazetteer lookups from the OS OpenSpace service and has a largely similar API to Apple's Mapkit, so that moving from Apple mapping to OS Openspace is simple [Converting](#converting-apple-mapkit).

Developers who wish to use the OpenSpace on-line services will need to register and obtain an API Key for either the Ordnance Survey [OpenSpace](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/api/) or [OpenSpace Pro](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/pro/) services.

This SDK is available as a static framework, please see the [Getting started](#getting-started) guide for instructions about downloading and importing into your own application or try a [demo app](#demo-app).

![ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me/raw/master/screenshot.png "Screenshot of locate me demo app") ![ScreenShot](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources/raw/master/screenshot.png "Screenshot of tilesource demo app")




#### Features

Here are some of the features available

- Native iOS framework incorporating OS mapping
- OS OpenSpace Pro and free services
- Select which products are displayed - see [products available](#product-code-list)
- Zoom and pan controls - native touch gesture recogonisers provide tap, pinch zoom control and many more
- Annotations - create and customise annotations
- Overlays - create and style polylines and polygons
- Offline tile storage - read [about offline tile packages](#offline-tile-packages)
- Geocoder - Search 1:50K Gazetteer, OS Locator and Codepoint Open datasets
- Uses OSGB36 National Grid map projection - ordnancesurvey-ios-sdk converts between WGS84 latitude/longitude and OSGB36 National Grid easting/northing
- User location - provides an wrapper around standard Core Location API to easily display your user's location on the map and use the data


Contents
-------

Within the ordnancesurvey-ios-sdk project we have:

1. /downloads - contains the ordnancesurvey-ios-sdk framework and Appledoc documentation packages



###### Demo projects

There are several demo projects as other github repos to get started

* [OS Locate Me](https://github.com/OrdnanceSurvey/ios-sdk-demo-locate-me)
* [OS Tilesource Demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-tilesources)
* [OS Mapkit conversion Demo](https://github.com/OrdnanceSurvey/ios-sdk-demo-mapkit-conversion)
 





Getting started
-------

#### API Key

Developers who wish to use online services will need to register and obtain an API Key for one of the [OS OpenSpace services](http://www.ordnancesurvey.co.uk/oswebsite/web-services/os-openspace/) in order to access the mapping service.

#### Download framework package

Head to the [downloads section](https://github.com/OrdnanceSurvey/ordnancesurvey-ios-sdk/downloads) of this project and download the latest OSMap zip package. Unzip the downloaded file to reveal the OSMap framework and to use it.

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

#### Import OSMap.h

In the ViewController you wish to display, import the OSMap/OSMap.h file

<pre>
#import &lt;OSMap/OSMap.h>
</pre>


#### Displaying a map

After completing the above steps to download and import the OSMap.framework, the minimum required to display a map using the ordnancesurvey-ios-sdk is as follows:

- Instatiate an OSMapView in a UIView
- Create atleast one OSTileSource and add to OSMapView

<pre>
//In your ViewController.m

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
- "CSG06"  // Consistently styled 25K,50K
- "CSG07"  // Consistently styled 25K,50K
- "CSG08"  // Consistently styled 25K,50K
- "CSG09"  // Consistently styled 25K,50K


##### Offline tile packages

This feature is not documented at present, TODO


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