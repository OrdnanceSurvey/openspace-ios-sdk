# OSTiles 1.0


Description
-------



The OSTiles format is uses the small, ubiquitous and lightweight [sqlite](http://www.sqlite.org/) database and overcomes filesystem storage problems to handle millions of images. 

OSTiles packages are sets of map tiles, each package can store several layers, a layer being a rectangular bounding box for a given Ordnance Survey product and all the tile images to fill that bounding box. The packages are intended to transport presentational map tiles and can be used as a mechanism to allow mobile applications to display tiles offline, without the need to stream tiles from a web service.

The OSTiles format is based on [MBTiles 1.1](http://www.mapbox.com/developers/mbtiles/) spec but for the following remarks:

* Does not use metadata table.
* Does not use power-of-2 zoom levels.
* Tile size and resolution are determined externally by the product code.




**NOTE:** 

* The file must have the extension `.ostiles` to be displayed by the SDK.
* A tile package must implement the specification below to ensure compatibility with the SDK.
* Tiles zoom_level, column and row `z,x,y` follow Tile Map Service in Transverse Mercator projection
* The tile images must be in [OSGB36 British National Grid](http://www.ordnancesurvey.co.uk/oswebsite/support/the-national-grid.html).



Database
---------


**NOTE:**

The schemas documented here for the OSTiles format are designed to be followed as interfaces. SQLite views that produce compatible results are equally valid.


#### Zoom Levels

The database is required to contain a table or view named `zoom_levels` implementing the schema below.

The available product codes are defined [here](https://github.com/OrdnanceSurvey/openspace-ios-sdk/blob/master/README.md#product-codes).

###### Schema

```sql
CREATE TABLE zoom_levels (
  zoom_level INTEGER PRIMARY KEY,
  product_code TEXT,
  bbox_x0 INTEGER,
  bbox_x1 INTEGER,
  bbox_y0 INTEGER,
  bbox_y1 INTEGER
);
```

###### Columns

* `zoom_level`: This is an arbitary number used as reference to `tiles` table.
* `product_code`: A string that references a SDK product code so that tile attributes can be assertained.
* `bbox_x0`, `bbox_x1`, `bbox_y0`, `bbox_y1`: Minimum/maximum column/row covered by the zoom level.




#### Tiles

The database is required to contain a table or view named `tiles` implementing the schema below.

The `tiles` table contains a row for each tile indexed by the zoom_level, column and row so that the SDK can find the tile.


###### Schema

```sql
CREATE TABLE tiles (
  zoom_level   INTEGER NOT NULL, -- REFERENCES zoom_levels(zoom_level)
  tile_column  INTEGER NOT NULL,
  tile_row     INTEGER NOT NULL,
  tile_data    BLOB,

  -- This also creates the corresponding index.
  PRIMARY KEY (zoom_level, tile_column, tile_row)
);
```


###### Columns

* `zoom_level`: Reference each tile to a `zoom_level`.
* `tile_column`: The column `x` for this tile.
* `tile_row`: The row `y` for this tile.
* `tile_data`: Raw image data as `blob`.


**NOTE:**

* The tile image must be `png` or `jpg` format.


Population
-------

This is a high level overview describing how to create OSTiles databases of map tile images. It requires a source raster map image sliced into smaller map tile images of predefined resolution and size and then inserted into the sqlite database using the schema described above.

#### Data requirements

Source data sould be raster map image data (possibly in TIFF format) for whichever product is required.  Ordnance Survey [Open datasets](https://www.ordnancesurveyite.co.uk/oswebsite/products/os-opendata.html) are available for free - otherwise it is possible to license other datasets.

The specification for the tiles expected in an OSTiles database is available [here](http://www.ordnancesurvey.co.uk/business-and-government/help-and-support/web-services/os-ondemand/configuring-wmts.html) - note the resolution and tile size.

#### Process

* Convert source data to `png` or `jpg` if required.
* Slice source data image into smaller 'tiles' at the appropriate resolution
* Optimise images to minimise file size.
* Create a row in the `zoom_levels` table referencing the correct `product_code` specify the bounding box covered by this package using this product.
* For each sub tile, create a row in the `tiles` table with the correct column and row and then insert the raw image data for this sub tile.


Change history
-------

This change history documents any changes to the OSTiles schema.

###### Version 1.0

* Initial specification release

