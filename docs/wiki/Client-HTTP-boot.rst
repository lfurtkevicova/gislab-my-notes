HTTP boot is an alternative boot method for launching GIS.lab Desktop
clients, which offers some advanced features and allows to boot if
multiple DHCP boot servers or GIS.lab servers exists in LAN. Here is a
list of notable advantages of HTTP boot over PXE:

-  it is the only way to boot if multiple DHCP boot servers or GIS.lab
   servers exists in network
-  it allows to manually choose target GIS.lab server which is very
   handy if multiple GIS.lab servers are running in one network
-  it is easier to boot from HTTP (which is actually done by booting
   from USB stick) than to setup PXE boot on some new machines
-  boot process is faster
-  it allows to use para-virtualized network adapter for Virtual clients
   (VirtualBox), which is many times faster than network adapter used
   for PXE

There are two possible choices to choose from - *Automatic GIS.lab
detection* and *Manual GIS.lab selection*.

.. figure:: images/http-boot/http-boot-menu.png
   :alt: HTTP boot

   HTTP boot
Automatic detection
-------------------

This mode will run DHCP request to set initial network DNS server
configuration. It will use the first response from any DHCP server in
network. Then, it will try to boot from *http://boot.gis.lab*. It means,
that if DHCP server response was from GIS.lab server, client machine
will successfully launch. If that response was from some third-party
DHCP server running in LAN, it will fail unless DNS server provided by
that DHCP response will be aware of *boot.gis.lab*. It also means, that
if multiple GIS.lab server instances are running in one LAN, it is not
possible to predict which one will be used.

Manual selection
----------------

Manual GIS.lab server selection can be used to choose GIS.lab server by
entering its IP address. It means, that it is not vulnerable from
third-party DHCP responses and it is possible to choose particular
GIS.lab server, if multiple ones are running in LAN. GIS.lab server is
using multiple IP addresses - IP address from GISLAB\_NETWORK range
(GISLAB\_NETWORK.5) or IP address assigned by LAN. Both of them can be
used for choosing GIS.lab server to boot. |HTTP boot|

.. |HTTP boot| image:: images/http-boot/http-boot-network-selection.png
