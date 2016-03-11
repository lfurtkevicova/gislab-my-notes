_Following procedure is assuming that GIS.lab Unit machine is going to be installed in network with automatic IP address assigning from DHCP server._

# Hardware requirements
## GIS.lab Unit (required)
* GIS.lab Unit machine: Intel NUC, CPU Intel Core i5 Ivy Bridge DC53427HYE
* 16 GB RAM: KINGSTON KVR16S11/8 8GB 2Rx8 1G x 64-Bit PC3-12800 CL11 204-Pin SODIMM DDR3 ([Intel doc](http://www.intel.com/support/motherboards/desktop/sb/CS-034249.htm))
* mSata SSD 525 Series, 60GB

See also official Intel's documentation for NUC hardware [Intel's documentation](http://www.intel.com/support/motherboards/desktop/sb/CS-034031.htm)

## GIS.lab client machines (optional)
* Intel NUC (*)
* 8 GB RAM (*)
* HDMI to DVI adapter
* 0,5m HDMI cable
* Logitech Wireless Keyboard
* Logitech Wireless Mouse
* LCD display 

(*) - same hardware specification as used for GIS.lab Unit

## Networking accessories (optional)
* 1 Gb Ethernet switch (16 ports): Netgear GS116GE
* CAT 5e Ethernet cables
* Wireless AP: Zyxel WAP3205 v2


# Software requirements
* latest GIS.lab source code from Git repository
* Ansible (in our case 1.7.2)

# Installation
## Basic operating system installation
Current GIS.lab version runs on top of Ubuntu 12.04 Precise. Following steps will guide you to install basic Ubuntu operating system, with default 'ubuntu' super user account (password 'ubuntu'). Network is configured to automatically obtain IP address from DHCP server.

* download latest Ubuntu 12.04 Precise AMD64 _Server_ installation ISO image from http://releases.ubuntu.com/precise

* use script _providers/gislab-unit/gislab-unit-iso.sh_ to create custom GIS.lab Unit installation ISO image file from original Ubuntu Server ISO image file downloaded in previous step. Run _gislab-unit-iso.sh -h_ to see required options.
This image will be used for automatic installation of basic operating system on GIS.lab Unit machine.

* prepare bootable installation USB stick from custom GIS.lab Unit ISO image file created in previous step. On Ubuntu, you can use _Startup Disk Creator_ application or UNetbootin which exists also for other Linux distributions

* attach power supply, HDMI display, keyboard and Ethernet cable in to GIS.lab Unit machine

* insert USB stick prepared in previous step in to GIS.lab Unit machine, power it on, press F10 key to run boot manager and select boot from USB. Then, fully automatic installation should start. When finished, machine will be turned of.

* remove USB stick used for installation

## GIS.lab Unit initialization
GIS.lab Unit is using SSD disk as primary storage. To perform well and preserve longer lifetime, some special configuration must be performed. Special Ansible playbook exists for this task.

* power on GIS.lab Unit machine

* log in to GIS.lab Unit machine using user name 'ubuntu' and password 'ubuntu' and run following command to detect IP assigned by DHCP server
```
$ ip addr | grep eth0
```

* create Ansible inventory file _gislab-unit.inventory_ with following content (replace <gislab-unit-ip-address> with IP address detected in previous step)
```
gislab-unit ansible_ssh_host=<gislab-unit-ip-address> ansible_ssh_user=ubuntu
```

* run following command to execute Ansible playbook, GIS.lab Unit will reboot when finished
```
 $ ansible-playbook --inventory=gislab-unit.inventory --private-key=<private-SSH-key-file> providers/gislab-unit/gislab-unit.yml
```

## GIS.lab installation
It is recommended to set at least some basic configuration before GIS.lab installation is performed. GIS.lab configuration is done in _system/host_vars_ directory. Configuration files placed in to this directory are automatically overriding default configuration set in _system/group_vars/all_. The most important task is to set _GISLAB_NETWORK_,  to value which is not in conflict with your LAN configuration.

* create configuration file _system/host_vars/gislab-unit_ and set _GISLAB_NETWORK_ and other configuration there
```
# GIS.lab Unit example configuration file

GISLAB_NETWORK: 192.168.19
GISLAB_APT_HTTP_PROXY: http://192.168.1.218:3142
```


Once GIS.lab is configured, installation can be performed.

* run following command to execute Ansible playbook
```
$ ansible-playbook --inventory=gislab-unit.inventory --private-key=<private-SSH-key-file> system/gislab.yml
```

Now, GIS.lab Unit machine is installed with GIS.lab system. Do not forget to create user accounts by _gislab-adduser_ command and allow their client machines to connect by running _gislab-machines_ command.
