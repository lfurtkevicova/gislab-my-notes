.. _virtual:
 
************
Virtual Mode
************

This part describes installation of GIS.lab in virtual machine using Vagrant 
and VirtualBox. Only **Linux** and **MAC OS X** operating systems are supported. 
Installation process will NOT modify anything on your host machine. Every 
operation is done inside of virtual machine.

GIS.lab server contains its own DHCP server. DHCP server is used in almost every 
local network for automatic network configuration. By default, access to GIS.lab's 
DHCP server is restricted only for GIS.lab client machines (by list of their 
MAC addresses) and it is managed by ``gislab-machines`` administrator command. 
It is possible to switch access policy to unrestricted, but it is required to 
double check that no DHCP servers conflict will occur. 
Otherwise, serious network breakage may be done. 

.. attention:: |att| Please, never change policy to unrestricted when connecting 
   to your corporate LAN!

GIS.lab creates it's own network. By default it is in range ``192.168.50.0/24``. 
If this range already exists in LAN where GIS.lab is going to be deployed, 
it is required to change it using ``GISLAB_NETWORK`` configuration variable.

See :ref:`Configuration section <configuration>` for information about changing 
GIS.lab configuration. 

.. important:: |imp| Without changing network configuration variable IP 
   conflicts may occur. 
   
To run GIS.lab in Virtual mode, there are some hardware and software requirements. 
Another important point is GIS.lab source code, see :ref:`GIS.lab source code download <GL-clone>`. 

*Hardware*

- 4 GB RAM on host machine

*Software*

-  host machine running Linux or MAC OSX
-  Git, see :ref:`Git installation <git-installation>`
-  Ansible 2.0 or higher, see :ref:`Ansible installation <ansible-installation>`
-  VirtualBox 4.3 or higher, see :ref:`VirtualBox installation <vb-installation>`
-  Vagrant 1.7 or higher , see :ref:`Vagrant installation <vagrant-installation>`

.. tip:: |tip| Check the version of software that are installed by typing

   .. code:: sh

      $ ansible --version
      $ vboxmanage --version
      $ vagrant --version

------
Server
------

GIS.lab installation takes from 30 minutes to few hours depending on
your machine performance and Internet connection speed.

Run following command in source code directory. 

.. code:: sh

   $ vagrant up

The output should be as follows.

.. code:: sh

   Bringing machine 'gislab_vagrant' up with 'virtualbox' provider...
   ==> gislab_vagrant: Clearing any previously set forwarded ports...
   ==> gislab_vagrant: Clearing any previously set network interfaces...
   ==> gislab_vagrant: Available bridged network interfaces:
   1) eth0
   2) wlan0
   ==> gislab_vagrant: When choosing an interface, it is usually the one that is
   ==> gislab_vagrant: being used to connect to the internet.
       gislab_vagrant: Which interface should the network bridge to? 

If machine contains multiple network adapters, user is asked to choose one 
corresponding adapter. For example, in case of ``eth0`` connection, selection 
``2`` should be choosen. Then the installation goes ahead.

.. code:: sh

   ==> gislab_vagrant: Preparing network interfaces based on configuration...
       gislab_vagrant: Adapter 1: nat
       gislab_vagrant: Adapter 2: bridged
   ==> gislab_vagrant: Forwarding ports...
       gislab_vagrant: 22 => 2222 (adapter 1)
   ==> gislab_vagrant: Running 'pre-boot' VM customizations...
   ==> gislab_vagrant: Booting VM...
   ==> gislab_vagrant: Waiting for machine to boot. This may take a few minutes...
       gislab_vagrant: SSH address: 127.0.0.1:2222
       gislab_vagrant: SSH username: vagrant
       gislab_vagrant: SSH auth method: private key
       gislab_vagrant: Warning: Connection timeout. Retrying...
   ==> gislab_vagrant: Machine booted and ready!
   ==> gislab_vagrant: Checking for guest additions in VM...
       gislab_vagrant: The guest additions on this VM do not match the installed version of
       gislab_vagrant: VirtualBox! In most cases this is fine, but in rare cases it can
       gislab_vagrant: prevent things such as shared folders from working properly. If you see
       gislab_vagrant: shared folder errors, please make sure the guest additions within the
       gislab_vagrant: virtual machine match the version of VirtualBox you have installed on
       gislab_vagrant: your host and reload your VM.
       gislab_vagrant: 
       gislab_vagrant: Guest Additions Version: 4.1.44
       gislab_vagrant: VirtualBox Version: 4.3
   ==> gislab_vagrant: Configuring and enabling network interfaces...
   ==> gislab_vagrant: Machine already provisioned. Run `vagrant provision` or use the `--provision`
   ==> gislab_vagrant: flag to force provisioning. Provisioners marked to run always will still run.

^^^^^^^^^^^^^
User accounts
^^^^^^^^^^^^^

By default, GIS.lab installation creates only a superuser account ``gislab``. 
Ordinary user account can be created by logging in to GIS.lab server, i.e. 
running Vagrant machine in source code directory via SSH.

.. code:: sh

   $ vagrant ssh

For example ``lab1`` user account with password ``lab`` can then be created by 
using ``gislab-adduser`` command.

.. code:: sh 

   $ sudo gislab-adduser -g User -l GIS.lab -m lab1@gis.lab -p lab lab1

With ``gislab-listusers`` list of all GIS.lab users is displayed, see example below.

.. code:: sh

.. code::
	
   $ sudo gislab-listusers | grep uid:
   uid: uid=gislab
   uid: uid=lab1

   $ sudo gislab-adduser -g User -l GIS.lab -m furtkevicova@gis.lab -p ludmila furtkevicova
   
   $ sudo gislab-listusers | grep dn:
   uid: uid=gislab
   uid: uid=lab1
   uid: uid=furtkevicova

------
Client
------

Running GIS.lab client in virtual mode is very useful when one wants to
keep working in his favourite operating system, e.g. Windows 7 OS but also wants 
to use GIS.lab environment.
GIS.lab virtual client is running in VirtualBox virtual machine, which
is capable to run on **Windows**, **Linux** or **Mac OS X** operating systems.
The process consists of four main steps: 

1. :ref:`Virtual machine creation <vm-creation>`
2. :ref:`Booting <booting>`
3. :ref:`Enabling GIS.lab client on GIS.lab server <client-enabling>`
4. :ref:`Running virtual GIS.lab client <client-running>`

.. _vm-creation:

.. rubric:: Virtual machine creation

Machines are created in VirtualBox environment and their creation depends on 
type of booting, see :num:`#pxe-vb-settings` and :num:`#http-vb-settings`. 

.. _booting:

.. rubric:: Booting

It is possible to boot using :ref:`PXE <pxe-boot>` or :ref:`HTTP <http-boot>` boot. 

.. _pxe-boot:

^^^^^^^^
PXE boot
^^^^^^^^

PXE boot is a default boot mode for GIS.lab clients. It is a simplest
method to get client up and running, but it may not work if
multiple DHCP boot servers or GIS.lab servers exists in network.

It is necessary to configure boot order to boot only **from network**, enable IO APIC, 
configure network adapter in bridged mode, make sure that ``PCnet-FAST III (Am79C973)`` 
is selected as the adapter type and allow promiscuous mode for all. 

.. _pxe-vb-settings:

.. figure:: ../img/installation/pxe-vb-settings.png
   :align: center
   :width: 750

   Settings in VirtalBox using PXE boot.

.. todo:: |todo| new screenshots + instructions

.. _http-boot:

^^^^^^^^^
HTTP boot
^^^^^^^^^

HTTP boot is an alternative boot method for launching GIS.lab Desktop
clients, which offers some advanced features and allows to boot if
multiple DHCP boot servers or GIS.lab servers exists in LAN. HTTP boot is 
performed by loading 
system from special GIS.lab bootloader **ISO image file**, which exists 
in *http-boot/gislab-bootloader.iso*. Here is a
list of notable advantages of HTTP boot over PXE:

-  it is the only way to boot if multiple DHCP boot servers or GIS.lab
   servers exists in network
-  it allows to manually choose target GIS.lab server which is very
   handy if multiple GIS.lab servers are running in one network
-  it is easier to boot from HTTP (which is actually done by booting
   from USB stick) than to setup PXE boot on some new machines
-  boot process is faster
-  it allows to use para-virtualized network adapter for Virtual clients
   (VirtualBox), which is many times faster than network adapter used
   for PXE

Using HTTP boot it is necessary to add virtual *gislab-bootloader.iso* file as 
virtual CD/DVD, configure boot order to boot only from virtual CD/DVD, enable *IO
APIC*, configure network adapter in bridged mode, make sure 
``Paravirtualized Network (virtio-net)`` is selected as the adapter type and allow
promiscuous mode for all.

.. _http-vb-settings:

.. figure:: ../img/installation/http-vb-settings.png
   :align: center
   :width: 750

   Settings in VirtalBox using HTTP boot.

.. todo:: |todo| new screenshots + instructions

.. important:: |imp| For next steps assigned ``MAC address`` is needed. 
   See *Network* section in VirtualBox environment and make a note of this 
   address.

Selection of the network adapter on the host system that traffic to and from 
which network card will go through should be different from current internet 
connection, e.g. in case of ``wlan0``, ``eth0`` should be set as ``Name`` 
of ``Bridged Adapter``.

.. _client-enabling:

.. rubric:: Enabling GIS.lab client on GIS.lab server

After virtual client is created, log in to GIS.lab server and with 
``gislab-machines -a`` allow client machine to connect.

.. code:: sh

   $ vagrant ssh
   $ sudo gislab-machines -a <MAC-address>

.. _client-running:

.. rubric:: Running virtual GIS.lab client

Start GIS.lab client virtual machine by pressing ``Start`` button in
VirtualBox Manager, log in and enjoy. In the figures :num:`#client-pxe-logging-in` 
and :num:`#client-pxe-running` one can see GIS.lab client logging in screen 
and Desktop of running virtual GIS.lab client using e.g. PXE boot.

.. _client-pxe-logging-in:

.. figure:: ../img/installation/client-pxe-logging-in.png
   :align: center
   :width: 450

   GIS.lab client logging in.

.. _client-pxe-running:

.. figure:: ../img/installation/client-pxe-running.png
   :align: center
   :width: 450

   GIS.lab client running environment.

Using HTTP boot there are two possible choices to choose from: 

A) :ref:`Automatic GIS.lab detection <automatic-detection>`
B) :ref:`Manual GIS.lab selection <manual-selection>`.

.. _automatic-detection:

.. rubric:: Automatic detection

This mode will run DHCP request to set initial network DNS server
configuration. It will use the first response from any DHCP server in
network. Then, it will try to boot from ``http://boot.gis.lab``. It means,
that if DHCP server response was from GIS.lab server, client machine
will successfully launch. If that response was from some third-party
DHCP server running in LAN, it will fail unless DNS server provided by
that DHCP response will be aware of ``boot.gis.lab``. It also means, that
if multiple GIS.lab server instances are running in one LAN, it is not
possible to predict which one will be used.

.. _http-boot-a:

.. figure:: ../img/installation/http-boot-menu.png
   :align: center
   :width: 450

   Automatic detection using HTTP boot.

.. _manual-selection:

.. rubric:: Manual selection

Manual GIS.lab server selection can be used to choose GIS.lab server by
entering its IP address. It means, that it is not vulnerable from
third-party DHCP responses and it is possible to choose particular
GIS.lab server, if multiple ones are running in LAN. GIS.lab server is
using multiple IP addresses, i.e. IP address from GIS.lab network range
``GISLAB_NETWORK.5`` or IP address assigned by LAN. Both of them can be
used for choosing GIS.lab server to boot.

.. _http-boot-m:

.. figure:: ../img/installation/http-boot-network-selection.png
   :align: center
   :width: 450

   Manual network selection using HTTP boot.

IP address can be found out after typing ``ip a | grep eth0`` on GIS.lab server 
after ``vagrant ssh`` command.

.. tip:: |tip| To set custom client display resolution run following command 
   on host machine.
   
   .. code:: sh
      
      $ VBoxManage controlvm "<GIS.lab client name>" setvideomodehint <xresolution> <yresolution> 32
      # example 
      $ VBoxManage controlvm "GIS.lab client PXE" setvideomodehint 1000 660 32

.. note:: |note| Getting a list of all running VirtualBox virtual machines by 
   name and UUID is possible with following command on host machine.

   .. code:: sh

      $ VBoxManage list runningvms

For logging out from GIS.lab server use ``logout`` and then use ``vagrant halt``
to shut down the running machine Vagrant is managing.

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

.. _GL-clone:

.. rubric:: GIS.lab source code download

Following command will grab GIS.lab source code to user system.

.. code:: sh

   $ git clone https://github.com/gislab-npo/gislab.git

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
