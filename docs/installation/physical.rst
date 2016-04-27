.. _installation-physical:
 
*************
Physical Mode
*************

In this section, attention shall be given to installation of GIS.lab using 
physical mode.

To run GIS.lab in physical mode, there are some hardware and software requirements. 
As well as using virtual mode, :ref:`GIS.lab source code <GL-clone>` is needed. 
Information about requirements can be found below together with graphical 
figuration, see :num:`#requirements-physical`. 

*Hardware*

- GIS.lab Unit machine 

  - Intel NUC, CPU Intel Core i5 Ivy Bridge DC53427HYE
  - 16 GB RAM: KINGSTON KVR16S11/8 8GB 2Rx8 1G x 64-Bit PC3-12800 CL11 204-Pin SODIMM DDR3
  -  mSata SSD 525 Series, 60GB

- 8 GB RAM on host machine, Intel NUC (optional)
- keyboard
- mouse
- LCD display
- HDMI to DVI adapter 
- networking accessories (optional)

  -  1 Gb Ethernet switch (16 ports): Netgear GS116GE
  -  CAT 5e Ethernet cables
  -  Wireless AP: Zyxel WAP3205 v2

*Software*

-  host machine running Linux or MAC OSX
-  Git, see :ref:`Git installation <git-installation>`
-  Ansible 2.0 or higher, see :ref:`Ansible installation <ansible-installation>`

.. _requirements-physical:

.. figure:: ../img/installation/requirements-physical.svg
   :align: center
   :width: 750

   Requirements for installation in physical mode.

.. seealso:: |see| `Intel documentation <http://www.intel.com/support/motherboards/desktop/sb/CS-034249.htm>`_ and `Intel documentation for NUC hardware <http://www.intel.com/support/motherboards/desktop/sb/CS-034031.htm>`_.

.. important:: |imp| In following procedure it is assumed that GIS.lab Unit 
   machine is going to be installed in network with automatic IP address 
   assigning from :ref:`DHCP server <dhcp-server>`.

======
Server
======

The process of installation consists of three main steps:

1. :ref:`Basic operating system installation <basic-os>`
2. :ref:`GIS.lab Unit initialization <initialization>`
3. :ref:`GIS.lab Unit installation <unit-installation>`

.. _basic-os:

Actual GIS.lab version runs on top of **Ubuntu 12.04 Precise**. 
GIS.lab developers are currently working on upgrade to new Ubuntu version 
**Ubuntu 16.04 Xenial**. Meanwhile, these material are dedicated to old 
Ubuntu version. 

Following steps will guide user to install basic Ubuntu operating system with
default ``ubuntu`` super user account and password ``ubuntu``. Network is
configured to automatically obtain :ref:`IP address <ip-address>` from 
:ref:`DHCP server <dhcp-server>`.

In the first step download latest `64-bit PC (AMD64) server install 
CD <<http://releases.ubuntu.com/precise>>`_ type of **image**.

Then use script ``providers/gislab-unit/gislab-unit-iso.sh`` to create 
custom **GIS.lab unit** installation **ISO image file** from original Ubuntu 
server ISO image file downloaded in above step. 
This command has some parametres `-s`, ``-t``, ``-k``, ``-w`` and ``-i``. 

.. tip:: |tip| From cloned `gislab` directory (see :ref:`GIS.lab source code <GL-clone>` 
   run `sudo ./providers/gislab-unit/gislab-unit-iso.sh -h` command to see 
   details of required options. 

Options are written below. Adjusted image will be used for automatic 
installation of basic operating system on GIS.lab unit machine.

.. code:: sh

   USAGE: gislab-unit-iso.sh [OPTIONS]
   Create GIS.lab base system installation ISO image from Ubuntu Server ISO.
   Script must be executed with superuser privileges.
   
   OPTIONS
       -s country code used for choosing closest repository mirror (e.g. SK)
       -t timezone (e.g. Europe/Bratislava)
       -k SSH public key file, which will be used for GIS.lab installation or update
       -w working directory with enough disk space (2.5 x larger than ISO image size)
       -i Ubuntu Server installation ISO image file
       -h display this help

For example, assuming that downloaded original Ubuntu server installation 
``ISO image`` is located in ``Downloads`` directory, user wants to use 
``Italian`` official archive mirror, ``Rome`` timezone, ``SSH public key`` 
file particularly created for GIS.lab installation or update is located in 
``.ssh`` directory, then the script can be run as follows.

.. code:: sh

   sudo ./providers/gislab-unit/gislab-unit-iso.sh -s IT -t Europe/Rome -k ~/.ssh/id_rsa_gislab_unit.pub -w /tmp/ -i ~/Downloads/ubuntu-12.04.5-server-amd64.iso

Continue with preparation of bootable installation USB stick from custom 
GIS.lab Unit ISO image file created in previous step. On Ubuntu 
`Startup Disk Creator <>`_ or `UNetbootin <>`_ application can be used and 
they exists also for other Linux distributions. Recommended option is usage
of ``dd`` command. See example bellow.

.. code:: sh

   # Format USB flash disk 
   # In our example we assume that USB flash disk is connected as /dev/sdf
   # Please check 'dmesg' for your configuration
   sudo mkdosfs -n 'GIS.lab Base System' -I /dev/sdf -F 32
   isohybrid /path/to/your/gislab.iso
   sudo dd if=/path/to/your/gislab.iso of=/dev/sdf bs=4k
   sudo eject /dev/sdf

When above process is done, together with ready USB stick attach into GIS.lab 
unit machine also power supply, HDMI display, keyboard and Ethernet cable. 
Power it on, press F10 key to run boot manager and select ``boot from USB`` 
option. Then, fully automatic installation should start. When finished, 
machine will be turned of. USB stick should then be removed. 

.. _initialization:

.. _unit-installation: 

