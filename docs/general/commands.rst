.. _commands:
 
***************
Useful commands
***************

:command:`gislab-adduser`
   creates GIS.lab user account

:command:`gislab-backupall`
   backups all GIS.lab user accounts

:command:`gislab-backupuser`
   backups GIS.lab user account

.. _gislab-client-image:

:command:`gislab-client-image`
   builds GIS.lab client image/upgrades GIS.lab system

.. _gislab-client-shell:

:command:`gislab-client-shell`
   runs command or launches interactive shell in GIS.lab client's root

:command:`gislab-cluster event <event>`
   sends a custom event through the Serf cluster

:command:`gislab-cluster event reboot`
   should reboot all members of cluster

:command:`gislab-cluster event reboot "<hostname>,<hostname>,..."`
   should reboot particular client machines

:command:`gislab-cluster event shutdown`
   should shutdown all members of cluster

:command:`gislab-cluster leave`
   gracefully leaves the Serf cluster and shuts down 

:command:`gislab-cluster members`
   lists the members of a serve cluster

:command:`gislab-cluster members -tag sesion-active=*`
   lists client machines which are currently running user session

:command:`gislab-deluser`
   removes GIS.lab user account

:command:`gislab-listusers`
   lists GIS.lab users

:command:`gislab-machines`
   adds or removes GIS.lab client machine's MAC address

:command:`gislab-password`
   changes GIS.lab user's password

:command:`gislab-restoreuser`
   restores GIS.lab user account from backup

:command:`git config --get remote.origin.url`
   retrieves the git remote origin URL of the current repo 

:command:`nslookup`
   displays information that can be used to diagnose DNS infrastructure, e.g. 
   domain name or IP address, it is available only if 
   TCP/IP protocol is installed

:command:`ping <hostname-or-ip-address-of-the-target-computer>`
   sends a test packet of data to a designated IP address to test connection 
   using the TCP/IP protocol, it finds out whether the peer host/gateway is 
   reachable

:command:`shutdown -h now`
   brings the system down; instructs the hardware to stop all CPU functions
   immediately 

:command:`tmux kill-session`
   destroyes the given session, closing any windows linked to it and no other 
   sessions, and detaching all clients attached to it; if ``-a`` is given, 
   all sessions but the specified one is killed


:command:`vagrant destroy` 
   stops the running Vagrant machine and destroys all resources that were 
   created during the machine creation process

.. _vagrant-halt:
:command:`vagrant halt` 
   shuts down the running machine Vagrant is managing

.. _vagrant-provision:
:command:`vagrant provision` 
   runs any configured provisioners that allow user to automatically install 
   software, alter configurations, and more on the machine as part of the 
   :ref:`vagrant up <vagrant-up>` process against the running Vagrant managed 
   machine

:command:`vagrant provision --provision-with test`
   runs tests with Vagrant

.. important:: |imp| Variable ``GISLAB_TESTS_ENABLE`` must be set as ``yes`` 
   in ``system/host_vars/gislab_vagrant`` file.

:command:`vagrant reload` 
   the equivalent of running :ref:`vagrant halt <vagrant-halt>` followed by 
   :ref:`vagrant up <vagrant-up>`

.. _vagrant-status:
:command:`vagrant status`
   tells the state of the machines Vagrant is managing 

.. _vagrant-up:
:command:`vagrant up`
   creates and configures guest machines according to *Vagrantfile*

.. _vagrant-version:
:command:`vagrant version`
   tells the version of the installed Vagrant as well as the latest version of 
   Vagrant that is currently available

:command:`VBoxManage list runningvms`
   gets a list of all running VirtualBox virtual machines
