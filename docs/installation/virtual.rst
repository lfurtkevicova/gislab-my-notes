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
   
============
Requirements
============

--------
Hardware
--------

- 4 GB RAM on host machine

--------
Software
--------

-  host machine running Linux or MAC OSX
-  Git, see :ref:`Git installation <git-installation>`
-  Ansible 2.0 or higher, see :ref:`Ansible installation <ansible-installation>`
-  VirtualBox 4.3 or higher, see :ref:`VirtualBox installation <vb-installation>`
-  Vagrant 1.7 or higher , see :ref:`Vagrant installation <vagrant-installation>`

-----------
Source code
-----------

- GIS.lab source code, see :ref:`GIS.lab source code download <GL-clone>` to get it

====================
Installation process
====================

GIS.lab installation takes from 30 minutes to few hours depending on
your machine performance and Internet connection speed.

Run following command in source code directory. 

.. code:: sh

   $ vagrant up

The output should be as follows.

.. code:: sh

   ...

If machine contains multiple network adapters, user will be asked to choose one. 
corresponding to wired network adapter. In case of ``eth0`` connection, ``2`` 
should be selected.

.. rubric:: User accounts

By default, GIS.lab installation creates only a superuser account ``gislab``. 
Let's create ordinary user account. Log in to GIS.lab server and run following 
command in source code directory.

.. code:: sh

   $ vagrant ssh

Then create ``lab1`` user account with password ``lab`` on GIS.lab server.

.. code:: sh 

   $ sudo gislab-adduser -g User -l GIS.lab -m lab1@gis.lab -p lab lab1

----------------------------
Installation of requirements
----------------------------

.. _git-installation:

.. rubric:: Git installation

By far the easiest way of getting Git installed and ready to use is by using 
default repositories. This is the fastest method, but the version may 
be older than the newest version. For GIS.lab version from official repositories 
should be normally sufficient. At firt, ``apt`` package management tools can be 
used to update local package index. Afterwards, Git can be downloaded and installed.

.. code:: sh

   $ sudo apt-get update
   $ sudo apt-get install git

.. _ansible-installation:

.. rubric:: Ansible installation

Ansible is an automation engine and its installation includes adding Ansible 
repository and installing it by typing ordinary commands.

.. code-block:: sh

   $ sudo apt-get install software-properties-common
   $ sudo apt-add-repository ppa:ansible/ansible
   $ sudo apt-get update
   $ sudo apt-get install ansible

.. _vb-installation:

.. rubric::  VirtualBox installation

Firstly, add VirtualBox repository signing key, then add repository to system, 
install Dynamic Kernel Module Support Framework and finally install VirtualBox.

.. code-block:: sh
   
   $ wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
   $ sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian trusty contrib" > /etc/apt/sources.list.d/virtualbox.list'
   $ sudo apt-get update && sudo apt-get install dkms
   $ sudo apt-get install virtualbox-4.3

.. _vagrant-installation:

.. rubric:: Vagrant installation

It should be first removed previously downloaded Vagrant packages, then 
downloaded from `www.vagrantup.com <http://www.vagrantup.com/downloads.html>`_ 
and eventually package should be installed. See instructions below.

.. code-block:: sh

   $ rm -vf ~/Downloads/vagrant_*.deb
   $ sudo dpkg -i ~/Downloads/vagrant_*.deb
   $ sudo apt-get -f install

.. attention:: |att| If running 32-bit host operating system, run following command 
   to download 32-bit Vagrant box from whatever directory.
   
   .. code:: sh
   
      $ vagrant box add precise-canonical http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-i386-vagrant-disk1.box

.. _GL-clone:

.. rubric:: GIS.lab source code download

Following command will grab GIS.lab source code to user system.

.. code:: sh

   $ git clone https://github.com/imincik/gis-lab.git

.. tip:: |tip| Lastly, check the version of software that are installed by typing

   .. code:: sh

      # Git
      $ git --version
      # Ansible
      $ ansible --version
      # VirtualBox
      $ vboxmanage --version
      # Vagrant
      $ vagrant --version
