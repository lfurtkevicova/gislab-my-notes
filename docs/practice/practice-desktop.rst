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

===============
GIS.lab project
===============
