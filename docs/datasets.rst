.. _data:

*********
Data sets
*********

Some basic information about our data aiming to be a part of examples indroduced
in this documentation.

================
1. Natural earth
================

Database ``natural-earth.sqlite``:

.. rubric:: Data source

The data is sourced from `Natural Earth dataset <http://www.naturalearthdata.com/downloads/>`_. It is a public domain map dataset available at ``1:10 million``, 
``1:50 million``, and ``1:110 million`` map scales. It is 
free for use in any type of project. 
Dataset was built through a collaboration of many volunteers, it is supported 
by *NACIS* - North American Cartographic Information Society and contains a series 
of vector and raster data. With Natural Earth one can make a variety of maps 
with all commonly used cartography and GIS software. 

.. rubric:: Coordinate systems

All Natural Earth data use the Geographic coordinate system WGS84, 
datum ``+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs``.

.. rubric:: Format and other key features

Natural Earth Vector comes in ESRI shapefile format. Character encoding is 
``Windows-1252``. Vector features include name attributes and bounding box extent.
Natural Earth Raster comes in TIFF format with a TFW world file. 

Natural Earth is very useful collection of data. Most of their attributes are 
equally important for mapmaking. They contain embedded feature names, 
which are ranked by relative importance. Other attributes 
facilitate faster map production, such as width attributes assigned to river 
segments for creating tapers, etc.

---------------
Thematic layers
---------------

1. Administrative areas

**Area** - matched boundary polygon for area of interest

* *Layer name* : ``area``
* *Storage type* : SQLite database
* *Geometry type of the features in layer* : Polygon
* *Primary key attributes* : PK_UID 
* *The number of features* : 1
* *The number of attributes* : 0
* *In layer spatial reference system units* : ``xMin = 5.75267, yMin = 45.3242, xMax = 24.2430, yMax = 55.1653``

**Countries** - matched boundary lines and polygons with various attributes for 
countries

* *Layer name* : ``countries``
* *Storage type* : SQLite database
* *Geometry type of the features in layer* : Polygon
* *Primary key attributes* : PK_UID 
* *The number of features* : 9
* *The number of attributes* : 5
* *In layer spatial reference system units* : ``xMin = 5.85249, yMin = 45.4236, xMax = 24.1432, yMax = 55.0653``
* *Attributes* :

.. csv-table:: Attributes of country layer.
   :header: "Name", "Description"
   :widths: 10, 10

   "*adm0_a3*", "country code"
   "*name*", "estimated total population"
   "*gdp_md_est*", "estimated total GDP in millions of dollars"
   "*subregion*", "part of a larger region or continent"

2. Urban landscape

**Places** - point symbols with name attributes. Includes DEM data, population 
data and other information 

* *Layer name* : ``places``
* *Storage type* : SQLite database
* *Geometry type of the features in layer* : Point
* *Primary key attributes* : PK_UID 
* *The number of features* : 161
* *The number of attributes* : 10
* *In layer spatial reference system units* : ``xMin = 6.14003, yMin = 46.0004, xMax = 23.17, yMax = 54.7837``
* *Attributes* :

.. csv-table:: Attributes of places layer.
   :header: "Name", "Description"
   :widths: 10, 10

   "*name*", "name of entity"
   "*adm0name*", "country name"
   "*adm0_a3*", "country code"
   "*adm1name*", "sub-country name"
   "*lattitude*", "latitude of interior point (degrees)"
   "*longitude*", "longitude of interior point (degrees)"
   "*pop_max*", "population for the metropolitan area	"
   "*pop_min*", "population for the incorporated city"
   "*gtopo30*", "DEM with 30-arc second resolution"
   "*timezone*", "timezone"

**Roads** - road lines with attributes 

* *Layer name* : ``roads``
* *Storage type* : SQLite database
* *Geometry type of the features in layer* : Line
* *Primary key attributes* : PK_UID 
* *The number of features* : 1536
* *The number of attributes* : 5
* *In layer spatial reference system units* : ``xMin = 6.071, yMin = 45.5358, xMax = 23.8521, yMax = 54.8022``
* *Attributes* :

.. csv-table:: Attributes of places layer.
   :header: "Name", "Description"
   :widths: 10, 10

   "*type*", "type of road"
   "*length_km*", "road length (km)"
   "*label*", "label"
   "*local*", "local label"
   "*expressway*", "1 for expressway, 0 for other"

=========
2. Prague
=========

Database ``prague.sqlite``:

.. rubric:: Data source

The data is sourced from 
open data `IPR <http://www.geoportalpraha.cz/en/opendata>`_ provided by 
*Prague Institute of Planning and Development*, open data 
`RÚIAN <http://vdp.cuzk.cz/vdp/ruian/stat/>`_ supplied by the 
*Registry of Territorial Identification, Addresses and Real Estate* and data 
`DIBAVOD <http://www.dibavod.cz/index.php?id=27&PHPSESSID=vcbxqccbl>`_ provided 
by *T. G. Masaryk water research institute, public research institution*. 

.. rubric:: Coordinate systems

.. rubric:: Format and other key features

---------------
Thematic layers
---------------

1. x

**x** - x

* *Layer name* : ``x``

==============
Issues (draft)
==============

* 

