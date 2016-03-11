## Server
GIS.lab Server can be customized by running standard Linux/Ubuntu commands, but it is recommended to use some isolated environment like LXC or Docker containers when deploying custom service.


## User accounts
Process of creation and removal of GIS.lab user accounts can be customized by placing scripts in following directories. **Important note**: Scripts must have executable permissions assigned and can't contain file extension (see _man run-parts_).
* /opt/gislab/custom/accounts/before-add - executed before account is created 
* /opt/gislab/custom/accounts/after-add - executed after account is created
* /opt/gislab/custom/accounts/before-delete - executed before account is deleted
* /opt/gislab/custom/accounts/after-delete - executed after account is deleted
* /opt/gislab/custom/accounts/files - content of this directory is copied to user's home directory before _after-add_ hooks are executed

It is possible to use following variables in customization scripts:
* gislab-adduser
 * GISLAB_USER - user name
 * GISLAB_USER_GIVEN_NAME - first name
 * GISLAB_USER_SURNAME - last name
 * GISLAB_USER_EMAIL - email
 * GISLAB_USER_DESCRIPTION - description
 * GISLAB_USER_SUPERUSER - superuser status
 * GISLAB_USER_GROUPS - groups membership

* gislab-deluser
 * GISLAB_USER - user name

For content stored in _files_ directory, it is possible to use template variables in following format:
* gislab-adduser
 * {+ GISLAB_USER +} - user name
 * {+ GISLAB_USER_GIVEN_NAME +} - first name
 * {+ GISLAB_USER_SURNAME +} - last name
 * {+ GISLAB_USER_EMAIL +} - email
 * {+ GISLAB_USER_DESCRIPTION +} - description
 * {+ GISLAB_USER_SUPERUSER +} - superuser status
 * {+ GISLAB_USER_GROUPS +} - groups membership

## Desktop client
GIS.lab Desktop client can also be customized by running standard Linux/Ubuntu commands, but they must be executed in isolated environment called _chroot_. Two administrator scripts are used to perform this action - _gislab-client-shell_ to enter client's chroot and _gislab-client-image_ to rebuild updated image.  

_Note, that client's chroot and resulting image are always restored to original state after every GIS.lab upgrade, so customization must be applied again (this behaviour is planed to be changed in future)._

### Backup

It is very good idea to backup client's chroot and image in case if something will go wrong in process of customization or rollback is required. Backup operation can be done by simple backup of chroot directory and client image file. Approximate total backup size is 2 GB.

* create backup of client's chroot directory
```
$ sudo tar cjf /mnt/backup/client-desktop-root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root
```

* create backup of client's image
```
$ sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/client-desktop-image-`date -I`
```
#### Recover backup

First of all we remove current client's root and image (assuming that we have already a backup):

```
$ sudo rm -r /opt/gislab/system/clients/desktop/root
$ sudo rm -r /opt/gislab/system/clients/desktop/image
```

Afterwards we recover selected backup of client's root and image:

```
$ sudo tar xjf /mnt/backup/client-desktop-root-2015-11-29.tar.bz2 -C /
$ sudo cp -a /mnt/backup/client-desktop-image-2015-11-29/ /opt/gislab/system/clients/desktop/image
```

### Example custom installation of latest GDAL version from source code
* enter client's chroot
```
$ sudo gislab-client-shell -i
```

* compile and install GDAL
```
$ apt-get -y install g++ subversion
$ cd /tmp
$ svn checkout https://svn.osgeo.org/gdal/trunk/gdal gdal
$ cd gdal
$ ./configure
$ make
$ make install
```

* leave client's chroot
```
$ exit
```

* build updated image
```
$ sudo gislab-client-image
```

Do not forget to set LD_LIBRARY_PATH variable on client before running GDAL commands
```
$ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
$ /usr/local/bin/ogr2ogr --version
GDAL 2.0.0dev, released 2014/04/16
```

## Executing customization scripts from Ansible
It is recommended to use Ansible to execute customization scripts directly from local machine. Following example will execute the same script first on GIS.lab Server and than in GIS.lab Client chroot.

* example Ansible playbook
```
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
    - name: Copy script to client chroot
      copy: src={{ CLIENT_SCRIPT }}
            dest={{ GISLAB_INSTALL_CLIENTS_ROOT }}/desktop/root/tmp/customize.sh
            owner=root group=root mode=0755
      tags:
        - customize-client

    - name: Run script in client chroot
      shell: gislab-client-shell /tmp/customize.sh
      tags:
        - customize-client

    - name: Remove script from client chroot
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
```

* example customization script
```
#!/bin/bash
# Example GIS.lab customization script.
# Author: Ivan Mincik, ivan.mincik@gmail.com


# detect if we are running on GIS.lab Server or inside GIS.lab Desktop
# Client chroot
if [ "$(ls -di /)" == "2 /" ]; then
	echo "Hello from GIS.lab Server."
else
	echo "Hello from GIS.lab Client chroot."
fi


# vim: set ts=4 sts=4 sw=4 noet:
```

* running Ansible playbook in Vagrant environment
```
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

```

### Boot loader
To customize GIS.lab Desktop client boot loader, create copy of boot loader source file (http-boot/gislab-bootloader.ipxe) and modify it as required. For more information about iPXE syntax see [documentation](http://ipxe.org/cmd). Than follow build process below.

* download iPXE source code
```
$ git clone git://git.ipxe.org/ipxe.git && cd ipxe
```

* checkout to version used by GIS.lab (optional)
```
$ git checkout d644ad41f5a17315ab72f6ebeeecf895f7d41679
```

* build customized ISO image (bin/ipxe.iso)
```
$ cd src
$ make EMBED=CUSTOM-BOOT-LOADER-SOURCE-FILE.ipxe
```