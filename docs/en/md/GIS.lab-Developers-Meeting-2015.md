**Date and time**:
* 23.9: 14:00 - until evening
* 24.9: 8:00 - 16:00

**Place**: FSv CVUT, Thakurova 7, Prague 6, Czech Republic

## Program
* very quick introduction of current state of GIS services implementation
* two days of discussion how to move forward

## Topics
### Overall goals of GIS.lab platform
* What is overall goal of GIS.lab ?
* What are client interfaces for ?
* What is GIS.lab cluster for ?

### WPS services integration
* choosing between QGIS Processing plugin or QGIS GRASS plugin
* if QGIS Processing plugin will be choosen, are there any possibilities for improvements for GRASS backend (detection of GRASS modules, input/output optimization when chaining processes)
* wps4server - http://www.3liz.com/blog/rldhont/index.php?post/2015/09/07/wps4server%3A-QGIS-Processing-to-Web-Processing-Service
* PyWPS4
* how list of services will be saved with QGIS project ?
* development of WPS clients for QGIS (or improving existing one), GIS.lab Web and Mobile

### Drawings
* keep current implementation of drawings service concept (Balls) ?
* separating of drawings service from GIS.lab Web client
* saving data in user's PostgreSQL schema
* drawings service improvements - sharing, integration to clients, user page, permissions, API
* drawings integration with WPS

### GIS.lab Web NG
* choosing new web development stack - OL 3, Angular ?
* integration of WPS
* search integration - keep current query builder or filter above attribute table or something else
* drawings integration
* new UI proposal

### QGIS GIS.lab Web improvements
* see similar plugin from Boundless - https://github.com/volaya/slides/tree/master/webappbuilder

### GeoData Science Tools Integration
* IPython notebook with cluster support - https://github.com/donnemartin/data-science-ipython-notebooks
* GeoPython
* R

### Web Admin interface
* features
* implementation

### Operating system upgrade
* Systemd
* packages versions - QGIS, GRASS ...

### Responsibilities and budget
* choosing responsible person for each task
* budget

## Tasks
* try to make some basic wps4server project to evaluate this solution