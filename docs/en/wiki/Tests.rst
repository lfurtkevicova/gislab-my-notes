Tests
-----

Run tests with Vagrant \* set *GISLAB*\ TESTS\_ENABLE: yes\_ in
*system/host*\ vars/gislab\_vagrant\_

::

    $ vagrant provision --provision-with test

Run tests with Ansible

::

    $ ansible-playbook --inventory=gislab-unit.inventory --private-key=<private-SSH-key-file> system/test.yml

