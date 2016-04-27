.. _installation-physical:
 
*************
Physical Mode
*************

To run GIS.lab in Physical mode, there are some hardware and software requirements. 
As well as using virtual mode, :ref:`GIS.lab source code <GL-clone>` is needed. 
Information about requirements can be found below together with graphical 
figuration, see :num:`#requirements-physical`. 

*Hardware*

- GIS.lab Unit machine 

  - Intel NUC, CPU Intel Core i5 Ivy Bridge DC53427HYE
  - 16 GB RAM: KINGSTON KVR16S11/8 8GB 2Rx8 1G x 64-Bit PC3-12800 CL11 204-Pin SODIMM DDR3
  -  mSata SSD 525 Series, 60GB

- 4 GB RAM on host machine, Intel NUC (optional)
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
