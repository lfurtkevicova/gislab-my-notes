.. _gislab-web:
 
***********
GIS.lab Web
***********

GIS.lab Web is a **web application** built on top of modern technologies with 
very modern user interface, optimized also for mobile devices. It stands on the 
shoulders of `QGIS <http://qgis.org/en/site/>`_ desktop and server software.

It is separated project from the core GIS.lab system with aim to produce 
generally usable QGIS web interface, usable with or without GIS.lab 
infrastructure.

.. raw:: html

   <center><iframe width="560" height="315" src="https://www.youtube.com/embed/7vBM1X5QuqE" frameborder="0" allowfullscreen></iframe></center>
   <p>

.. important:: |imp| No GIS.lab is needed!

It is possible to install environment in virtual machine using Vagrant.
Vagrant will automatically install whole development environment and build 
GIS.lab Web from source code.﻿

=====================
Software requirements
=====================

Only **Linux** and **Mac OS X** host systems are supported. 
   
============
Installation
============   


==========
Print tool
==========

The main idea is, that once print is activated in GIS.lab Web, it will 
download raw print output from QGIS Server using GetPrint request and will 
allow interactive visualization of map content directly in this template. 
One can zoom, pan and rotate map and see exactly how the result will look like. 
To get the better idea, see video below.

.. raw:: html

   <center><iframe width="560" height="315" src="https://www.youtube.com/embed/1g0YduhPwpk" frameborder="0" allowfullscreen></iframe></center>
   <p>
