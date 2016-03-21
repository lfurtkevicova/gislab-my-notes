.. _virtual:
 
************
Virtual Mode
************

This part describes installation of GIS.lab in virtual machine using Vagrant 
and VirtualBox. Only **Linux** and **MAC OS X** operating systems are supported. 
Installation process will NOT modify anything on your host machine. Every 
operation is done inside of virtual machine.

.. attention:: |att| GIS.lab server contains its own DHCP server. DHCP 
   server is used in almost every local network for automatic network 
   configuration. By default, access to GIS.lab's DHCP server is restricted only 
   for GIS.lab client machines (by list of their MAC addresses) and it is 
   managed by gislab-machines administrator command. It is possible to switch 
   access policy to unrestricted, but it is required to double check that no DHCP 
   servers conflict will occur. Otherwise, serious network breakage may be done. 
   Please, never change policy to unrestricted when connecting to your corporate 
   LAN!

.. important:: |imp| GIS.lab creates it's own network. By default it is in range 
   ``192.168.50.0/24``. If this range already exists in LAN where GIS.lab is 
   going to be deployed, it is required to change it using ``GISLAB_NETWORK`` 
   configuration variable, otherwise IP conflicts may occur. See Configuration 
   section for information about changing GIS.lab configuration. 
   
=====================
Installation on Linux
=====================



========================
Installation on MAC OS X
========================
