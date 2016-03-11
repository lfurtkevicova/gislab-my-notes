GIS.lab GOALS
=============

-  rapid deployment of complete geospatial solution for collaborative
   data capturing, processing, analysis and publication on web

   -  fully automatic provisioning
   -  out-of-box working deployment with GIS.lab Unit

-  self containing system

-  very quick results (from plain hardware to web application in
   minutes)
-  high added values

   -  integration of precisely chosen set of geospatial FOSS (one best
      tool for one thing) to one system with consistent behaviour

      -  opposite to OSGeo Live

   -  collaboration tools
   -  user support, software support

-  full client computer performance utilisation (opposite to thin
   client)
-  thinking about client environment more as about some kind of
   specialized client interface providing tools from desktop world
   rather than a traditional desktop

-  computer resources sharing

-  same deployment in LAN and cloud
-  mobile clients (TODO)
-  web administration similar to router or NAS (TODO)
-  utilization: education and real work

HOW IS IT DONE ?
================

-  automatic provisioning - Ansible
-  virtual machine deployment - Vagrant and VirtualBox
-  client boot service - LTSP Fat client/ own solution
-  OWS services load balancing
-  QGIS Desktop and Server as GIS.lab Desktop + own GIS.lab Web app
-  GRASS as processing backend under QGIS Processing plugin and WPS
-  own GIS software packaging packaging

GOALS OF THIS WORKSHOP
======================

-  to provide technical knowledge required to build GIS.lab from source
   code and start using and hacking
-  get serious feedback to know what to change and improve
-  discuss GRASS 7 integration
-  discuss improvements needed for using GIS.lab in education
-  propose future development
-  get new contributors

WORKSHOP TOPICS
===============

-  introduction to GIS.lab
-  introduction to Vagrant and Vagrant commands (status, up, destroy,
   provision, ssh)
-  GIS.lab installation in VirtualBox using Vagrant provisioner,
   configuration, update
-  introduction to GIS.lab management commands (adduser, deluser,
   password, machine)
-  extensibility with plugins and Docker containers
-  creation of GIS.lab network from scratch
-  booting physical client
-  booting virtual clients (HTTP boot, file sharing with host machine)
-  introduction to GIS.lab client environment (shared folders, Projects,
   Publish, Booster, software, services, PostgreSQL schemas, chat)
-  introduction to GIS.lab OWS services load balancing
-  creation of GIS projects in GIS.lab, publishing in GIS.lab Web
-  testing GRASS in GIS.lab

-  introduction to Ansible
-  introduction to GIS.lab source code

-  discussion about integration of education support tools
-  discussion about integration of GRASS 7
-  general discussion


