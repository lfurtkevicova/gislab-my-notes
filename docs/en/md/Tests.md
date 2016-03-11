## Tests
Run tests with Vagrant
* set _GISLAB_TESTS_ENABLE: yes_ in _system/host_vars/gislab_vagrant_
```
$ vagrant provision --provision-with test
```

Run tests with Ansible
```
$ ansible-playbook --inventory=gislab-unit.inventory --private-key=<private-SSH-key-file> system/test.yml
```