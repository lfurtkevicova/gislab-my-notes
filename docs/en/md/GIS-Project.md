__GIS.lab__ projects are created and managed by __QGIS__ application, which is a main tool for all geospatial tasks. GIS.lab is containing its own version of QGIS, which is improved with bug fixes and features and it is accessible under _GIS.lab Desktop_ item in _GIS.lab_ applications menu.

__GIS.lab Web__ client is automatically publishing all GIS projects created in desktop to web environment.

Following steps will create simplest possible GIS project and will publish it on web.

## Log in
* log in to GIS.lab session using 'lab1' user credentials

![client-login](images/quick-start/gis-project/client-login.png)  


## Data preparation
* create working directory called _my-first-project_ in _~/Projects_ directory
* copy example SpatiaLite database file _~/Repository/gislab-project/natural-earth/natural-earth.sqlite_ to _~/Projects/my-first-project_ directory

![first-project-copy-data](images/quick-start/gis-project/first-project-copy-data.png)


## Project creation
* launch _GIS.lab Desktop_ (_GIS.lab > GIS.lab Desktop_ applications menu). New project will be automatically created

![gislab-desktop-applications-menu](images/quick-start/gis-project/gislab-desktop-applications-menu.png)


* add SpatiaLite database file to _GIS.lab Desktop_ project (_Layer > Add SpatiaLite layer > New_)
* connect to database by pressing _Connect_ button

![first-project-connect-data](images/quick-start/gis-project/first-project-connect-data.png)


* load _counties_ layer by mouse selection and pressing _Add_ button

![first-project-countries-layer](images/quick-start/gis-project/first-project-countries-layer.png)


* set project title to _Countries of Central Europe_ (_Project > Project Properties > Project title_)

![first-project-title](images/quick-start/gis-project/first-project-title.png)


* save project as _~/Projects/my-first-project/europe.qgs_ (_Project > Save_). Our first GIS project is ready

![first-project-save-europe](images/quick-start/gis-project/first-project-save-europe.png)


## Project publishing
* install _GIS.lab Web_ plugin (_Plugins > Manage and Install Plugins_)

![gislab-web-plugin](images/quick-start/gis-project/gislab-web-plugin.png)


* launch _GIS.lab Web_ plugin (_Web > GIS.lab Web > Publish in GIS.lab Web_). It is safe to ignore on-the-fly transformation warning

![first-project-plugin-publish1](images/quick-start/gis-project/first-project-plugin-publish1.png)


* publish project by pressing _Next_ button in wizard. Press _Publish_ and _Finish_ buttons on the last two pages

![first-project-plugin-publish2](images/quick-start/gis-project/first-project-plugin-publish2.png)
![first-project-plugin-publish3](images/quick-start/gis-project/first-project-plugin-publish3.png)


* copy whole directory _~/Projects/my-first-project_ to _~/Publish/lab1_ directory to finish project publishing

![first-project-copy-to-publish](images/quick-start/gis-project/first-project-copy-to-publish.png)


## Using project on web
* launch _GIS.lab Web_ User page (_GIS.lab > GIS.lab Web_ applications menu)

![gislab-web-applications-menu](images/quick-start/gis-project/gislab-web-applications-menu.png)


* ignore security warnings produced by self-signed certificate (_I Understand the Risks > Add Exception > Confirm Security Exception_)

![gislab-web-user-page-warnings](images/quick-start/gis-project/gislab-web-user-page-warnings.png)


* log in to GIS.lab Web User page using 'lab1' user credentials

![gislab-web-user-page-login](images/quick-start/gis-project/gislab-web-user-page-login.png)


* inspect published project. Our project should be listed as second, right below default _Empty_ project

![gislab-web-user-page-projects](images/quick-start/gis-project/gislab-web-user-page-projects.png)


* click on project's link in _URL_ column to launch it

![gislab-web-first-project](images/quick-start/gis-project/gislab-web-first-project.png)


## What's next
To get more familiar with possible project configurations, copy whole GIS.lab example project directory _~/Repository/gislab-project/natural-earth_ to _~/Projects_ directory and start exploring.
