.. _configuration:
 
*************
Configuration
*************

GIS.lab is designed to install and run out-of-box with default
configuration. However, it is required to change default network
configuration variable ``GISLAB_NETWORK``, if GIS.lab's default network
range ``192.168.50.0/24`` already exists in LAN to prevent IP conflicts.

Default GIS.lab configuration file named ``all`` exists in ``system/group_vars``.
When user decides to adjust it, he/she should not modify this file
directly. 

.. rubric:: Virtual mode

For installation in VirtualBox it is recommended to create file
named ``gislab_vagrant`` in ``system/host_vars`` directory for host specific 
GIS.lab configuration and put various changes there. 

.. rubric:: Physical mode

When Physical mode is used, file in ``system/group_vars`` should
be named according to name of GIS.lab unit. This name is a part 
of Ansible inventory file content, script that Ansible uses
to determine what to provide. All file names must always match unique 
host name specified in inventory file.

File ``gislab_vagrant`` will be loaded automatically by Vagrant 
without need to manually create the Ansible inventory file. Example 
configuration in ``gislab_vagrant`` or ``<name-of-gislab-unit>``
file is shown below.

.. code:: sh

   GISLAB_ADMIN_FIRST_NAME: Ludmila
   GISLAB_ADMIN_SURNAME: Furtkevicova
   GISLAB_ADMIN_EMAIL: ludmilafurtkevicov@gmail.com

   GISLAB_NETWORK: 192.168.50
   GISLAB_TIMEZONE: Europe/Rome
   GISLAB_DNS_SERVERS:
    - 10.234.10.10
    - 8.8.8.8
   
   GISLAB_CLIENT_ARCHITECTURE: amd64
   GISLAB_CLIENT_LANGUAGES:
    - en
    - it
    - sk
   
   GISLAB_CLIENT_KEYBOARDS:
     - layout: en
     - layout: sk
     - layout: it
   
   GISLAB_CLIENT_OWS_WORKER_MIN_MEMORY: 4000

.. note:: Content of Ansible inventory file called ``<name-of-gislab-unit>.inventory`` 
   would be as follows
 
   .. code:: sh
      
      <name-of-gislab-unit> ansible_ssh_host=<host-url> ansible_ssh_user=<provisioning-user-account-name>

   Example content of ``gislab-unit-fem.inventory`` Ansible file is shown below.

   .. code:: sh
      
      gislab-unit-fem ansible_ssh_host=10.234.1.44 ansible_ssh_user=ubuntu
