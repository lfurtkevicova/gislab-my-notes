Vagrant file for Apt Cacher service:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

GISLAB_NETWORK="192.168.50"

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise-canonical"

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--memory", "512"]
    v.customize ["modifyvm", :id, "--nictype1", "virtio"]
    v.customize ["modifyvm", :id, "--nictype2", "virtio"]

    config.vm.network "forwarded_port", guest: 3142, host: 3142, auto_correct: true
  end

  config.vm.hostname = "apt-cacher"
  config.vm.provision "shell", inline: "apt-get install apt-cacher-ng"
  config.vm.network "public_network", ip: "%s.%s" % [GISLAB_NETWORK, "6"]
end
```

Run Apt Cacher server:

```
$ vagrant up
```

Add to your GIS.lab configuration file:

```
GISLAB_APT_HTTP_PROXY: http://192.168.50.6:3142
```