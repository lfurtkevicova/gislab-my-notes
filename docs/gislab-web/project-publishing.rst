.. _project-publishing:
 
==================
Project publishing
==================

When ``Publish`` button is successfully pressed in GIS.lab QGIS plugin
:ref:`dialog <gislab-qgis-plugin-publish>`, 
unique project file name with timestamp together with it's metafile are created.

.. figure:: ../img/gislab-web/gislab-plugin-publish.png
   :align: center
   :width: 450

   Part of GIS.lab project publishing process.

Then it is necessary to **copy** published QGIS project with all associated data 
to ``vagrant`` directory that is located in ``gislab-web`` source code.

.. figure:: ../img/gislab-web/vagrant-directory.svg
   :align: center
   :width: 450

   Directory for QGIS projects going to be published.

As the final step, open web browser and launch published project in GIS.lab Web 
interface by entering URL below.
You will see welcome screen with possibility to enter credential but for now, 
you can just ``Continue as guest``. 

.. code:: 

   https://localhost:8000?PROJECT=vagrant/<project-directory-name>/<qgs-file-name>

.. _gislab-web-welcome:

.. figure:: ../img/gislab-web/gislab-web-welcome.png
   :align: center
   :width: 750

   GIS.lab Web welcome screen.

And now there are no obstacles to enjoy your published project.

.. _gislab-we-published:

.. figure:: ../img/gislab-web/gislab-web-published.png
   :align: center
   :width: 750

   QGIS project published with GIS.lab Web.

