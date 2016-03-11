This page tries to collect documentation to some of the most common network configurations used for GIS.lab deployment. We assume, that in all cases, machines are connected to Ethernet network with Gigabit switch and at least _CAT5 e_ Ethernet cables.

# A. GIS.lab server running in VirtualBox
We assume, that GIS.lab is installed on Linux laptop in VirtualBox virtual machine using Vagrant as it is documented on [Quick Start](Quick-Start) page.

## Existing LAN with DHCP server
GIS.lab is deployed in existing LAN (192.168.1.0/24), which already contains DHCP server and many non GIS.lab machines and network is connected to Internet.

### Configuration
**Laptop - wired adapter**: automatic IP address assignment (Network Manager)  
**Laptop - wireless adapter**: disabled (Network Manager)  
**GISLAB_NETWORK**: 192.168.50  

## Separate network
GIS.lab is deployed in separate network, specially created by GIS.lab vendor, where only GIS.lab machines are connected. Internet access is provided by host laptop's WiFi connection and it is connected to GIS.lab network via Ethernet cable. Network contains only GIS.lab machines.

### Configuration
**Laptop - wired adapter**: static IP address: IP: 192.168.5.1, mask: 255.255.255.0, gateway: 0.0.0.0, DNS: 8.8.8.8 (Network Manager)  
**Laptop - wireless adapter**: connected to Internet (Network Manager)  
**GISLAB_NETWORK**: 192.168.50  


# B. GIS.lab Unit
We assume, that GIS.lab Unit machine is installed as it is documented on [GIS.lab Unit](GIS.lab-Unit) page.

## Existing LAN with DHCP server
GIS.lab Unit is deployed in existing LAN (192.168.1.0/24), which already contains DHCP server and many non GIS.lab machines and network is connected to Internet.

### Configuration
**GISLAB_NETWORK**: 192.168.50  

## Separate network
GIS.lab Unit is deployed in separate network, specially created by GIS.lab vendor, where only GIS.lab machines are connected. Internet access is provided by laptop running Linux, which is connected to Internet via WiFi and to GIS.lab network via Ethernet cable. Network contains only GIS.lab machines.

In this case, it is required to change GIS.lab Unit's wired network adapter configuration to static IP address and allow connection forwarding on laptop.

### Configuration
**Laptop - wired adapter**: static IP address: IP: 192.168.5.1, mask: 255.255.255.0, gateway: 0.0.0.0, DNS: 8.8.8.8 (Network Manager)  
**Laptop - wireless adapter**: connected to Internet (Network Manager)  
**GISLAB_NETWORK**: 192.168.50  
**GISLAB_SERVER_INTEGRATION_FALLBACK_IP_ADDRESS**: 192.168.5.5  
**GISLAB_SERVER_INTEGRATION_FALLBACK_GATEWAY**: 192.168.5.1  

* to allow using laptop as Internet gateway, run following commands on laptop:
```
$ sudo sysctl -w net.ipv4.ip_forward=1
$ sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
```