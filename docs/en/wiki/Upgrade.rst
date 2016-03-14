GIS.lab upgrade
---------------

GIS.lab upgrade procedure consists from three steps - *server software
upgrade*, *client images upgrade* and *GIS.lab system itself upgrade*.
Although, it is possible to run each step separately by hand, GIS.lab
provisioner is designed as idempotent task, which is capable of both,
GIS.lab installation and also upgrade. This means, that GIS.lab upgrade
is performed by the same provisioner command as used for GIS.lab
installation. Using GIS.lab provisioner for upgrade is recommended to
keep all parts of GIS.lab in consistent state.

GIS.lab source code update (development version)

::

    $ git pull

Upgrade with Vagrant

::

    $ vagrant provision

Upgrade with Ansible

::

    $ ansible-playbook --inventory=gislab-unit.inventory --private-key=<private-SSH-key-file> system/gislab.yml

