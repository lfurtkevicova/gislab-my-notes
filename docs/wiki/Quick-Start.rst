.. _quick-start:

***********
Quick Start
***********

*This page describes installation of GIS.lab development version in
virtual machine using Vagrant and VirtualBox. Only **Linux** and **MAC
OS X** operating systems **are supported**. Installation process will
NOT modify anything on your host machine. Every operation is done inside
of virtual machine.*

**BIG FAT WARNINGS:**

1. GIS.lab server contains its own DHCP server. DHCP server is used in
   almost every local network for automatic network configuration. By
   default, access to GIS.lab's DHCP server is restricted only for
   GIS.lab client machines (by list of their MAC addresses) and it is
   managed by *gislab-machines* administrator command. It is possible to
   switch access policy to unrestricted, but it is required to double
   check that no DHCP servers conflict will occur. Otherwise, serious
   network breakage may be done. Please, never change policy to
   unrestricted when connecting to your corporate LAN !

2. GIS.lab creates it's own network. By default it is in range
   192.168.50.0/24. If this range already exists in LAN where GIS.lab is
   going to be deployed, it is required to change it using
   *GISLAB*\ NETWORK\_ configuration variable, otherwise IP conflicts
   may occur. See `Configuration <#configuration>`__ section for
   information about changing GIS.lab configuration.

Command line interface
----------------------

All lines beginning with '$' (dollar) character are shell commands which
must be run in command window. In most cases you can copy and paste
commands from this page, but sometimes you will need to modify them
little bit to suite to your conditions.

| **On Linux**, open terminal window in *Accessories > Terminal
Emulator* menu.
| |Terminal window on Linux|

Installation
------------

Hardware requirements
~~~~~~~~~~~~~~~~~~~~~

-  4 GB RAM on host machine

Software requirements
~~~~~~~~~~~~~~~~~~~~~

-  host machine running Linux or MAC OSX (in our case Kubuntu 14.04 LTS)
-  Git
-  Ansible 1 (in our case 1.9.4, Ansible 2 is currently not supported !)
-  VirtualBox (in our case ver. 4.3)
-  Vagrant 1.7 or higher (in our case ver. 1.7.2)

Ansible installation
~~~~~~~~~~~~~~~~~~~~

-  add Ansible repository

   ::

       $ sudo apt-get install software-properties-common
       $ sudo apt-add-repository ppa:ansible/ansible
       $ sudo apt-get update

-  | install Ansible
   | ``$ sudo apt-get install ansible``

VirtualBox installation
~~~~~~~~~~~~~~~~~~~~~~~

-  add VirtualBox repository signing key
   ``$ wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -``
-  add VirtualBox repository to system
   ``$ sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian trusty contrib" > /etc/apt/sources.list.d/virtualbox.list'``
-  install Dynamic Kernel Module Support Framework
   ``$ sudo apt-get update && sudo apt-get install dkms``
-  install VirtualBox
   ``$ sudo apt-get install virtualbox-4.3``

Vagrant installation
~~~~~~~~~~~~~~~~~~~~

-  remove previously downloaded Vagrant packages
   ``$ rm -vf ~/Downloads/vagrant_*.deb``
-  download Vagrant from http://www.vagrantup.com/downloads.html
-  install package
   ``$ sudo dpkg -i ~/Downloads/vagrant_*.deb``
   ``$ sudo apt-get -f install``

32-bit Vagrant Box installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  if running 32-bit host operating system, run following command to
   download 32-bit Vagrant box. If running 64-bit host system, skip this
   step (you can run this command from whatever directory)

::

    $ vagrant box add precise-canonical \
    http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-i386-vagrant-disk1.box

GIS.lab source code download
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  run following command to grab GIS.lab source code

::

    $ git clone https://github.com/imincik/gis-lab.git

Configuration
~~~~~~~~~~~~~

GIS.lab is designed to install and run out-of-box with default
configuration. However, it is required to change default network
configuration variable *GISLAB*\ NETWORK\_, if GIS.lab's default network
range (192.168.50.0/24) already exists in LAN to prevent IP conflicts.

We kindly invite you to briefly look at default GIS.lab configuration
file, which exists in
`system/group\_vars/all <https://github.com/imincik/gis-lab/blob/master/system/group_vars/all>`__.
If you have decided to adjust it to your needs, do not modify this file
directly. For installation in VirtualBox, create file
*system/host*\ vars/gislab\_vagrant\_ and put your changes there.

GIS.lab installation
~~~~~~~~~~~~~~~~~~~~

GIS.lab installation takes from 30 minutes to few hours depending on
your machine performance and Internet connection speed.

-  run following command in source code directory. If your machine
   contains multiple network adapters, you will be asked to choose one.
   Select one corresponding to your wired network adapter (on Linux it
   is usually 'eth0')
   \`\`\` $ vagrant up

Bringing machine 'gislab\_vagrant' up with 'virtualbox' provider... ==>
gislab\_vagrant: Importing base box 'precise-canonical'... ==>
gislab\_vagrant: Matching MAC address for NAT networking... ==>
gislab\_vagrant: Setting the name of the VM:
gis-lab\_gislab\_vagrant\_1436974375613\_27055 ==> gislab\_vagrant:
Clearing any previously set forwarded ports... ==> gislab\_vagrant:
Fixed port collision for 22 => 2222. Now on port 2200. ==>
gislab\_vagrant: Clearing any previously set network interfaces... ==>
gislab\_vagrant: Available bridged network interfaces: 1) wlan0 2) eth0
3) docker0 ==> gislab\_vagrant: When choosing an interface, it is
usually the one that is ==> gislab\_vagrant: being used to connect to
the internet. gislab\_vagrant: Which interface should the network bridge
to? 2 ==> gislab\_vagrant: Preparing network interfaces based on
configuration... gislab\_vagrant: Adapter 1: nat gislab\_vagrant:
Adapter 2: bridged ==> gislab\_vagrant: Forwarding ports...
gislab\_vagrant: 22 => 2200 (adapter 1) ==> gislab\_vagrant: Running
'pre-boot' VM customizations... ==> gislab\_vagrant: Booting VM... ==>
gislab\_vagrant: Waiting for machine to boot. This may take a few
minutes... gislab\_vagrant: SSH address: 127.0.0.1:2200 gislab\_vagrant:
SSH username: vagrant gislab\_vagrant: SSH auth method: private key
gislab\_vagrant: Warning: Connection timeout. Retrying...
gislab\_vagrant: gislab\_vagrant: Vagrant insecure key detected. Vagrant
will automatically replace gislab\_vagrant: this with a newly generated
keypair for better security. gislab\_vagrant: gislab\_vagrant: Inserting
generated public key within guest... gislab\_vagrant: Removing insecure
key from the guest if its present... gislab\_vagrant: Key inserted!
Disconnecting and reconnecting using new SSH key... ==> gislab\_vagrant:
Machine booted and ready! ==> gislab\_vagrant: Checking for guest
additions in VM... gislab\_vagrant: The guest additions on this VM do
not match the installed version of gislab\_vagrant: VirtualBox! In most
cases this is fine, but in rare cases it can gislab\_vagrant: prevent
things such as shared folders from working properly. If you see
gislab\_vagrant: shared folder errors, please make sure the guest
additions within the gislab\_vagrant: virtual machine match the version
of VirtualBox you have installed on gislab\_vagrant: your host and
reload your VM. gislab\_vagrant: gislab\_vagrant: Guest Additions
Version: 4.1.12 gislab\_vagrant: VirtualBox Version: 4.3 ==>
gislab\_vagrant: Configuring and enabling network interfaces... ==>
gislab\_vagrant: Running provisioner: install (ansible)...
PYTHONUNBUFFERED=1 ANSIBLE\_FORCE\_COLOR=true
ANSIBLE\_HOST\_KEY\_CHECKING=false ANSIBLE\_SSH\_ARGS='-o
UserKnownHostsFile=/dev/null -o ForwardAgent=yes -o ControlMaster=auto
-o ControlPersist=60s' ansible-playbook
--private-key=/home/imincik/Projects-dev/gis-lab/.vagrant/machines/gislab\_vagrant/virtualbox/private\_key
--user=vagrant --connection=ssh --limit='gislab\_vagrant'
--inventory-file=/home/imincik/Projects-dev/gis-lab/.vagrant/provisioners/ansible/inventory
--extra-vars={"GISLAB\_ADMIN\_PASSWORD":"gislab"} -vv system/gislab.yml

PLAY [all]
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

GATHERING FACTS
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
<127.0.0.1> REMOTE\_MODULE setup ok: [gislab\_vagrant]

...

::



    # User accounts
    By default, GIS.lab installation creates only a superuser account _gislab_. Let's create ordinary user account:

    * log in to GIS.lab server (run following command in source code directory)

$ vagrant ssh

::


    * run following command on GIS.lab server to create _lab1_ user account with password _lab_ 

$ sudo gislab-adduser -g User -l GIS.lab -m lab1@gis.lab -p lab lab1
\`\`\`

Vagrant commands
================

GIS.lab server launched by *Vagrant* can be managed only by *Vagrant*
commands. Here is a list of the most important *vagrant* commands:

-  **vagrant up** - start server
-  **vagrant halt** - halt server
-  **vagrant reload** - restart server
-  **vagrant destroy** - delete (destroy) server
-  **vagrant ssh** - connect to server via SSH
-  **vagrant status** - print status information
-  **vagrant provision** - update GIS.lab
-  **vagrant help** - print help on Vagrant commands

Continue with `Virtual client <Virtual-Client>`__.

.. |Terminal window on Linux| image:: images/quick-start/installation/terminal-window-linux.png
