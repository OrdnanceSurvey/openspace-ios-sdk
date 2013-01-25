# OSTiles 1.0


Description
-------



The OSTiles format is uses the small, ubiquitous and lightweight [sqlite](http://www.sqlite.org/) database and overcomes filesystem storage problems to handle millions of images. 

OSTiles packages are sets of map tiles, each package can store several layers, a layer being a rectangular bounding box for a given Ordnance Survey product and all the tile images to fill that bounding box. The packages are intended to transport presentational map tiles and can be used as a mechanism to allow mobile applications to display tiles offline, without the need to stream tiles from a webservice.

The OSTiles format is based on [MBTiles 1.1](http://www.sqlite.org/) spec but for the following remarks:

* Does not use metadata table.
* Does not use power-of-2 zoom levels.* Tile size and resolution are determined by the product code.

**NOTE:** 
* The file must have the extension `.ostiles` to be displayed by the SDK.* A tile package must implement the specification below to ensure compatibility with the SDK.* Tiles zoom_level, column and row `z,x,y` follow Tile Map Service in Transverse Mercator projection
* The tile images must be in [OSGB36 British National Grid](http://www.ordnancesurvey.co.uk/oswebsite/support/the-national-grid.html)



Database
---------


**NOTE:**

The schemas documented here for the OSTiles format are designed to be followed as interfaces. SQLite views that produce compatible results are equally valid.


#### Zoom Levels

The database is required to contain a table or view named `zoom_levels` implementing the schema below.

The available product codes are defined [here](ordnancesurvey-ios-sdk/tree/master/README.md#product-code-list).

###### Schema

<pre>
CREATE TABLE zoom_levels (
  zoom_level INTEGER PRIMARY KEY,
  product_code TEXT,
  bbox_x0 INTEGER,
  bbox_x1 INTEGER,
  bbox_y0 INTEGER,
  bbox_y1 INTEGER
);
</pre>

###### Columns

* `zoom_level`: This is an arbitary number used as reference to tiles.
* `product_code`: A string that references a SDK product code so that tile attributes can be assertained.
* `bbox_x0`, `bbox_x1`, `bbox_y0`, `bbox_y1`: Minimum/maximum column/row covered by the zoom level.




#### Tiles

The database is required to contain a table or view named `tiles` implementing the schema below.

The `tiles` table contains a row for each tile indexed by the zoom_level, column and row so that the SDK can find the tile.


###### Schema

<pre>
CREATE TABLE tiles (
  zoom_level   INTEGER NOT NULL, -- REFERENCES zoom_levels(zoom_level)
  tile_column  INTEGER NOT NULL,
  tile_row     INTEGER NOT NULL,
  tile_data    BLOB,

  -- This also creates the corresponding index.
  PRIMARY KEY (zoom_level, tile_column, tile_row)
);
</pre>


###### Columns

* `zoom_level`: Reference each tile to a `zoom_level`.
* `tile_column`: The column `x` for this tile.
* `tile_row`: The row `y` for this tile.
* `tile_data`: Raw image data as `blob`.


**NOTE:**

* The tile image must be `png` or `jpg` format


Population
-------

How to populate a DB

This is TBC - do we release a script to 'scrape' Openspace for tiles? Or point people in the direction of Opendata raster products and get them to cut and georefernce themselves?


Change history
-------

###### Version 1.0

* Initial specification release

License
-------
What license is this released under??