.. _installation-physical:
 
*************
Physical Mode
*************

In this section, attention shall be given to installation of GIS.lab using 
physical mode.

.. _requirements-physical:

To run GIS.lab in physical mode, there are some hardware and software requirements. 
As well as using virtual mode, :ref:`GIS.lab source code <GL-clone>` is needed. 
Information about requirements can be found below together with graphical 
figuration, see :num:`#requirementsphysical`. 

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

.. _requirementsphysical:

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

The process of installation consists of five main steps:

1. :ref:`Preparation of bootable USB stick <usb-preparation>`
2. :ref:`Adjusted operating system installation <basic-os>`
3. :ref:`GIS.lab unit initialization <initialization>`
4. :ref:`Configuration <configuration>`
5. :ref:`GIS.lab unit installation <unit-installation>`

.. _usb-preparation:

Actual GIS.lab version runs on top of **Ubuntu 12.04 Precise** release. 
GIS.lab developers are currently working on upgrade to new Ubuntu version 
**Ubuntu 16.04 Xenial**. Meanwhile, these materials are dedicated to old 
Ubuntu version. 

Following steps will guide user to install basic Ubuntu operating system with
default ``ubuntu`` super user account and password ``ubuntu``. Network is
configured to automatically obtain :ref:`IP address <ip-address>` from 
:ref:`DHCP server <dhcp-server>`.

In the first step download latest 
`64-bit PC (AMD64) server install CD <http://releases.ubuntu.com/precise>`_ 
type of **image**.

.. _generate-ssh:

Furthermore, it is important to create **private key**. Generated public part 
of **keypair** will be used as a way to identify trusted computers 
without involving passwords. It can be generated with ``ssh-keygen`` command
from ``home/.ssh`` directory. It is recommended to rename new key suitably, 
for example ``id_rsa_gislab_unit``.

Then use script ``providers/gislab-unit/gislab-unit-iso.sh`` to create 
custom **GIS.lab unit** installation **ISO image file** from original Ubuntu 
server ISO image file downloaded in above step. 
This command has some parametres `-s`, ``-t``, ``-k``, ``-w`` and ``-i``. 

.. tip:: |tip| From cloned ``gislab`` directory included in 
   :ref:`GIS.lab source code <GL-clone>` 
   run ``sudo ./providers/gislab-unit/gislab-unit-iso.sh -h`` command to see 
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
``.ssh`` directory and new adjusted image should be saved in ``tmp`` directory, 
then the script can be run as follows.

.. code:: sh

   sudo ./providers/gislab-unit/gislab-unit-iso.sh -s IT -t Europe/Rome -k ~/.ssh/id_rsa_gislab_unit.pub -w /tmp/ -i ~/Downloads/ubuntu-12.04.5-server-amd64.iso

Continue with preparation of bootable installation USB stick from custom 
GIS.lab Unit ISO image file created in previous step. On Ubuntu 
`Startup Disk Creator <https://en.wikipedia.org/wiki/Startup_Disk_Creator>`_ 
or `UNetbootin <https://en.wikipedia.org/wiki/UNetbootin>`_ application can 
be used and they exists also for other Linux distributions. 
Probably the most recommended option is usage of ``dd`` command. 
See example bellow.

.. code:: sh

   # Format USB flash disk 
   # In is assumed that USB flash disk is connected as /dev/sdf
   # Please check 'dmesg' for your configuration
   sudo mkdosfs -n 'GIS.lab Base System' -I /dev/sdf -F 32
   isohybrid /path/to/your/gislab.iso
   sudo dd if=/path/to/your/gislab.iso of=/dev/sdf bs=4k
   sudo eject /dev/sdf

.. _basic-os:

When above process is done, together with ready USB stick attach also power 
supply, HDMI display, keyboard and Ethernet cable into GIS.lab unit machine,
see :num:`#installation-unit`. 
Power it on, press ``F10`` key to run boot manager and select ``boot from USB`` 
option. Then fully automatic installation should start. When finished, 
machine will be turned of. USB stick should then be removed. 

.. _installation-unit:

.. figure:: ../img/installation/installation-unit.svg
   :align: center
   :width: 450

   Necessary hardware components in adjusted operating system installation 
   process.

.. note:: |note| After booting there is only one notification related to 
   **cash packages** that allows to choose them in case they are existing.
   Otherwise just ``Continue`` option should be selected.

As a next step, power on GIS.lab unit and log in to machine using already 
noted username ``ubuntu`` and password ``ubuntu``. 

This is one of the ways how to log in to unit and it is possible only with 
LCD monitor and keyboard connected to unit. Another way means using SSH
key and log in to unit from another computer or laptop. That is why SSH
key :ref:`was generated <generate-ssh>`.

.. important:: |imp| GIS.lab unit has to be registered in the network. In other
   words ``IP address`` has to be assigned to unit. IP address can be assigned
   only to machine which ``MAC address`` is registered. Run ``ip a`` command to 
   detect this address. 

After registration run command again and detect ``IP address`` assigned by 
DHCP server.

.. code:: sh 

   ip a | grep eth0

In case unit is not registered automatically, run DHCP client that apply for
``IP address``. Then verify working internet connection, 
e.g. with ``ping`` command. 

.. code:: sh

   sudo dhclient eth0 -v
   ping www.google.com

.. tip:: |tip| To restart network use ``sudo /etc/init.d/networking restart``
   command.

If one wants to know if unit is already in network, ``ifconfog`` command 
should be used to see ``inet addr`` and then ``ssh ubuntu@<inet addr>`` 
should be run to connect to unit from another computer.

.. note:: |note| Instead of ``IP address`` also assigned ``name`` of registered 
   unit should work, for example ``gislab.intra.ismaa.it``. This name can be 
   found in output of ``nslookup <ip address>`` command.





.. _initialization:

.. _configuration:

It is recommended to set at least some basic configuration before
GIS.lab installation is performed. See 
:ref:`Configuration section <configuration-section>` of this documentation for
detailed instructions.

.. _unit-installation: 

Once GIS.lab is configured, installation can be performed. Run following 
command to execute **Ansible playbook**.

.. code:: sh

   $ ansible-playbook --inventory=gislab-unit.inventory --private-key= system/gislab.yml 

Now, GIS.lab unit machine is installed with GIS.lab system. Do not forget 
to :ref:`create user accounts <user-creation>` by ``gislab-adduser`` command 
and :ref:`allow their client machines <client-enabling>` to connect by running 
``gislab-machines`` 
command.
