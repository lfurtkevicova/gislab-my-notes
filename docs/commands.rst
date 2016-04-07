.. _commands:
 
***************
Useful commands
***************

:command:`gislab-adduser`
   creates GIS.lab user account

:command:`gislab-listusers`
   lists GIS.lab users

:command:`git config --get remote.origin.url`
   retrieves the git remote origin URL of the current repo 

:command:`vagrant destroy` 
   stops the running Vagrant machine and destroys all resources that were 
   created during the machine creation process

.. _vagrant-halt:
:command:`vagrant halt` 
   shuts down the running machine Vagrant is managing

.. _vagrant-provision:
:command:`vagrant povision` 
   runs any configured provisioners that allow user to automatically install 
   software, alter configurations, and more on the machine as part of the 
   :ref:`vagrant up <vagrant-up>` process against the running Vagrant managed 
   machine

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
