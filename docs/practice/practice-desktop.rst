.. _practice-desktop:
 
***************
GIS.lab Desktop
***************

.. _example-gdal:

=====================================
Latest GDAL version on GIS.lab client
=====================================

Let's see example custom installation of **latest GDAL version** from source code.

At first client's root should be entered.

.. code:: sh

   $ sudo gislab-client-shell -i

Then compilation and installation of GDAL should be executed.

.. code:: sh

   $ apt-get -y install g++ subversion
   $ cd /tmp
   $ svn checkout https://svn.osgeo.org/gdal/trunk/gdal gdal
   $ cd gdal
   $ ./configure
   $ make
   $ make install

After client's ``root`` is left by ``exit`` command, then ``image`` should 
be updated by ``sudo gislab-client-image``.

.. important:: |imp| Do not forget to set ``LD_LIBRARY_PATH`` variable on 
   client before running GDAL commands.
   
   .. code:: sh

      $ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
      $ /usr/local/bin/ogr2ogr --version
      GDAL 2.0.0dev, released 2014/04/16

===============
GIS.lab project
===============
