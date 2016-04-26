.. _practice-desktop:
 
***************
GIS.lab Desktop
***************

It is assumed that GIS.lab server is running either using keyboard, monitor 
and GIS.lab server username and password, or using ``ssh key``  and 
``IP address`` together with laptop or computer which ``ssh key`` is 
registered in ``./ssh/authorized_keys`` file.
In virtual mode GIS.lab server is running after ``vagrant ssh`` command, see 
:ref:`login to GIS.lab <login-vagrant>` via ``SSH`` section.

.. _example-gdal:

=====================================
Latest GDAL version on GIS.lab client
=====================================

Let's see example custom installation of **latest GDAL version** from source code.

At first client's ``root`` and ``image`` backup is recommended. In a next step
interactive shell in GIS.lab client's ``root`` should be entered.

.. code:: sh

   $ sudo tar cjf /mnt/backup/root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root
   $ sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/image-`date -I`
   $ sudo gislab-client-shell -i

Then compilation and installation of GDAL can be executed.

.. code:: sh

   $ apt-get -y install g++ subversion
   $ cd /tmp
   $ svn checkout https://svn.osgeo.org/gdal/trunk/gdal gdal
   $ cd gdal
   $ ./configure
   $ make
   $ make install

After client's ``root`` is left by ``exit`` command, then ``image`` should 
be updated by ``sudo gislab-client-image``. 
Continue with :ref:`creation <user-creation>` of new user booting with 
latest GDAL version.

.. important:: |imp| Do not forget to set ``LD_LIBRARY_PATH`` variable and 
   configure dynamic linker run-time bindings on client before running GDAL 
   commands.
   
   .. code:: sh

      $ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
      $ sudo ldconfig
      $ /usr/local/bin/ogr2ogr --version
      GDAL 2.0.0dev, released 2014/04/16

.. _example-remove-geany:

===============================
Software uninstallation - Geany
===============================

Example with `Geany <https://www.geany.org/>`_ software is shown below.

.. code:: sh

   # root and image backup
   $ sudo tar cjf /mnt/backup/root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root
   $ sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/image-`date -I`
   # enter interactive schell in client's root
   $ sudo gislab-client-shell -i
   
   # display geany package status details
   $ dpkg -s geany
   Status: install ok installed
   Priority: optional
   Section: devel
   Installed-Size: 2422
   Maintainer: ... ... ...
   # check geany version
   $ geany --version
   geany 0.21 (built on Mar 19 2012 with GTK 2.24.10, GLib 2.31.20)
   # uninstall geany
   $ sudo apt-get remove geany
   # leave client's root
   $ exit

   # build updated image 
   $ sudo gislab-client-image
   # create new user that boots without geany software installed
   sudo gislab-adduser -g User -l GIS.lab -m x@mail.com -p <psw> <name>
     
==================================
Software installation - Vim editor 
==================================

See :ref:`software uninstallation <example-remove-geany>` section and in 
client's root enter following code. 

.. code:: sh
   
   $ dpkg -s vim
   $ sudo apt-get update
   $ sudo apt-get install vim
   $ vim test
   $ a
   $ Hello VIM!
   $ :wq
   $ cat test
   Hello VIM!
   $ exit

.. _customization-ansible:

============================================
Executing customization scripts from Ansible
============================================

.. todo:: |todo| prejsť!

Following example will execute the same script first on GIS.lab Server 
and than in GIS.lab client's ``root``. See Ansible playbook below.

.. code:: sh

   ---
   
   # Example GIS.lab customization playbook.
   
   - hosts: all
     sudo: yes
   
     vars:
       SERVER_SCRIPT: gislab-customize.sh
       CLIENT_SCRIPT: gislab-customize.sh
       GISLAB_INSTALL_CLIENTS_ROOT: /opt/gislab/system/clients
   
     tasks:
       # Customize GIS.lab Server
       - name: Run script on server
         script: "{{ SERVER_SCRIPT }}"
         tags:
           - customize-server
   
       # Customize GIS.lab Desktop client
       - name: Copy script to client's root
         copy: src={{ CLIENT_SCRIPT }}
               dest={{ GISLAB_INSTALL_CLIENTS_ROOT }}/desktop/root/tmp/customize.sh
               owner=root group=root mode=0755
         tags:
           - customize-client
   
       - name: Run script in client's root
         shell: gislab-client-shell /tmp/customize.sh
         tags:
           - customize-client
   
       - name: Remove script from client's root
         file: path={{ GISLAB_INSTALL_CLIENTS_ROOT }}/desktop/root/tmp/customize.sh
               state=absent
         tags:
           - customize-client
   
       - name: Rebuild client image
         shell: gislab-client-image
         tags:
           - customize-client
           - build-cient-image
   
   # vim:ft=ansible:

Example customization script would be as follows.

.. code:: sh

   #!/bin/bash
   # Example GIS.lab customization script.
   # Author: Ivan Mincik, ivan.mincik@gmail.com
   
   
   # detect if we are running on GIS.lab Server or inside GIS.lab Desktop
   # Client root
   if [ "$(ls -di /)" == "2 /" ]; then
       echo "Hello from GIS.lab Server."
   else
       echo "Hello from GIS.lab Client's root."
   fi
   
   
   # vim: set ts=4 sts=4 sw=4 noet:

And for running Ansible playbook in Vagrant environment see next example.

.. code:: sh

   PYTHONUNBUFFERED=1 \
   ANSIBLE_FORCE_COLOR=true \
   ANSIBLE_HOST_KEY_CHECKING=false \
   ANSIBLE_SSH_ARGS='-o UserKnownHostsFile=/dev/null -o ForwardAgent=yes -o ControlMaster=auto -o ControlPersist=60s' \
   ansible-playbook -v \
   --private-key=$(pwd)/.vagrant/machines/gislab_vagrant/virtualbox/private_key \
   --user=vagrant \
   --connection=ssh \
   --limit='gislab_vagrant' \
   --inventory-file=$(pwd)/.vagrant/provisioners/ansible/inventory \
   --tags customize-server,customize-client,build-cient-image \
   gislab-customize.yml 

.. _cluster-parallel-ssh:

===================================================
Running commands on whole cluster with parallel-ssh
===================================================

Deploy public ``ssh`` key to GIS.lab user to allow passwordless login.

.. code:: sh

   $ ssh-copy-id -i ~/.ssh/id_rsa.pub  gislab@<GIS.lab server IP>

Get list of currently running client machines

.. code:: sh

   $ MACHINES="$(gislab-cluster members -tag role=client -status=alive | awk -F " " '{printf "%s ", $1}')"

Install gedit on all client machines

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" sudo DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends gedit

   [1] 23:02:57 [SUCCESS] c51
   Reading package lists...
   Building dependency tree...
   Reading state information...
   The following NEW packages will be installed:
     gedit
   0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
   Need to get 0 B/827 kB of archives.
   After this operation, 2,781 kB of additional disk space will be used.
   Selecting previously unselected package gedit.
   (Reading database ... 134642 files and directories currently installed.)
   Unpacking gedit (from .../gedit_3.4.1-0ubuntu1_amd64.deb) ...
   Processing triggers for desktop-file-utils ...
   Setting up gedit (3.4.1-0ubuntu1) ...
   update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode.
   Processing triggers for libc-bin ...
   ldconfig deferred processing now taking place
   [2] 23:02:57 [SUCCESS] c50
   Reading package lists...
   Building dependency tree...
   Reading state information...
   The following NEW packages will be installed:
     gedit
   0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
   Need to get 0 B/827 kB of archives.
   After this operation, 2,781 kB of additional disk space will be used.
   Selecting previously unselected package gedit.
   (Reading database ... 134642 files and directories currently installed.)
   Unpacking gedit (from .../gedit_3.4.1-0ubuntu1_amd64.deb) ...
   Processing triggers for desktop-file-utils ...
   Setting up gedit (3.4.1-0ubuntu1) ...
   update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode.
   Processing triggers for libc-bin ...
   ldconfig deferred processing now taking place

Perform performance test of parallel write to network share

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/dev/zero of=/mnt/barrel/file-$(hostname).io bs=1M count=1024'

   [1] 09:42:11 [SUCCESS] c54
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 37.7824 s, 28.4 MB/s
   [2] 09:42:11 [SUCCESS] c52
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 38.1136 s, 28.2 MB/s
   [3] 09:42:11 [SUCCESS] c51
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 38.4403 s, 27.9 MB/s
   [4] 09:42:12 [SUCCESS] c53
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 38.6802 s, 27.8 MB/s

Perform performance test of parallel read from network share

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/mnt/barrel/file-$(hostname).io of=/dev/zero bs=1M'

   [1] 09:42:45 [SUCCESS] c51
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.207453 s, 5.2 GB/s
   [2] 09:42:45 [SUCCESS] c53
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.210259 s, 5.1 GB/s
   [3] 09:42:45 [SUCCESS] c52
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.227793 s, 4.7 GB/s
   [4] 09:42:45 [SUCCESS] c54
   Stderr: 1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 0.207774 s, 5.2 GB/s

Perform CPU performance test

.. code:: sh

   $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/dev/zero bs=1M count=1024 | md5sum'

   [1] 09:39:05 [SUCCESS] c52
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c52,192.168.19.52' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.51008 s, 428 MB/s
   [2] 09:39:05 [SUCCESS] c53
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c53,192.168.19.53' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.50255 s, 429 MB/s
   [3] 09:39:06 [SUCCESS] c54
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c54,192.168.19.54' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.52551 s, 425 MB/s
   [4] 09:39:06 [SUCCESS] c51
   cd573cfaace07e7949bc0c46028904ff  -
   Stderr: Warning: Permanently added 'c51,192.168.19.51' (ECDSA) to the list of known hosts.
   1024+0 records in
   1024+0 records out
   1073741824 bytes (1.1 GB) copied, 2.56706 s, 418 MB/s

===============
GIS.lab project
===============

GIS.lab projects are created and managed by **QGIS** application, which 
is a main tool for all geospatial tasks. GIS.lab is containing its
own version of QGIS, which is improved with bug fixes and features and
it is accessible under **GIS.lab Desktop** item in GIS.lab applications menu.

**GIS.lab Web** client is automatically publishing all GIS projects
created in desktop to web environment.

Following steps will create simplest possible GIS project and will
publish it on web.

1. Log in to GIS.lab session 

Use login and password for user account that has been 
:ref:`created <user-creation>` by ``sudo gislab-adduser`` command from GIS.lab
server. 

2. Prepare data

Create working directory called ``my-first-project`` in ``~/Projects`` directory.
Copy example SpatiaLite database file 
``~/Repository/gislab-project/prague/prague.sqlite`` to 
``~/Projects/my-first-project`` directory.

3. Create project

Launch **GIS.lab Desktop** as ``GIS.lab > GIS.lab Desktop`` (QGIS) and add 
SpatiaLite database file as ``Layer > Add SpatiaLite layer > New``. 
Coose the database in ``Projects`` directory and connect to database by 
pressing ``Connect`` button.

Load some layer by mouse selection and press ``Add`` button. Set project 
title in ``Project > Project Properties > Project title``, save the project 
as ``~/Projects/my-first-project/my-first-project.qgs`` with ``Project > Save``
option. Now first GIS project is ready.

4. Publish project on Web

Install **GIS.lab Web plugin** in ``Plugins > Manage and Install Plugins`` 
section and launch it. 

.. note:: |note| It is safe to ignore on-the-fly transformation warning.

Publish project by pressing ``Next``, ``Publish`` and ``Finish`` buttons 
in plugin's wizards presented in dialogs. In next step copy whole directory 
``~/Projects/my-first-project`` to ``~/Publish/lab1`` directory.

Launch **GIS.lab Web** as ``GIS.lab > GIS.lab Web`` applications menu from main
GIS.lab panel. Ignore security warnings produced by self-signed certificate, 
i.e. *I Understand the Risks > Add Exception > Confirm Security Exception*
and log in with user's credentials. Then inspect published project which 
should be listed as second, right below 
default ``Empty`` project. Click on project's link in URL column to launch
project in web environment.

.. tip:: |tip| To get more familiar with possible project configurations, 
   copy some of whole GIS.lab example projects directories located in 
   ``~/Repository/gislab-project`` to ``~/Projects`` directory and start 
   exploring.
