Physical client mode is preferred way of launching GIS.lab client,
because it provides best performance. It will run GIS.lab client session
on client machine instead of original operating system installed (if
any) on hard drive. Original operating system and local data will stay
untouched and will be ready to run again after GIS.lab client is shut
down.

To run physical client, it is required to connect machine running
GIS.lab server and client machines via Gigabit switch and cables (CAT 5e
or higher).

Booting
-------

PXE boot
~~~~~~~~

PXE boot is a default boot mode for GIS.lab clients. Booting from PXE
requires to instruct client machine to boot from other device as it is
usually doing so. On newer computers it is also required to disable
*Secure boot* and/or enable *Legacy Mode*.

There are multiple possibilities how to instruct client machine to boot
from PXE: \* press some F-key (depends on vendor) at machine start which
will temporary instruct machine to boot from PXE \* press some F-key
(depends on vendor) at machine start to launch boot manager, choose
*PXE* or *PCI LAN* in boot menu to boot from PXE \* set *PXE* or *LAN*
as first boot device in BIOS configuration, save and restart machine to
boot from PXE

Here is an example procedure of enabling PXE boot for Lenovo ThinkPad \*
boot up computer \* press F2 -> press Enter -> press F1. This should
take you to the BIOS screen. Select Security -> select Secure Boot ->
set to Disable -> select Start Up -> select UEFI/Legacy Boot -> set to
Legacy Only -> press F10 \* once you press F10, reboot then press F12.
You should now be at the boot menu. Select PCI LAN. Press Enter

HTTP boot
~~~~~~~~~

In addition to default PXE boot method, GIS.lab clients can boot over
HTTP, which can provide some advantages. See `HTTP boot
page <Client-HTTP-boot>`__ for more details.

To enable HTTP boot, it is needed to create bootable USB stick from
special ISO image, which exists in *http-boot* directory.

Insert free USB stick in to your Linux workstation machine, if it is
automatically mounted, unmount it. Run ``dmesg`` command to detect
device assigned to your USB stick by operating system (it should be
something like */dev/sd[x]*).

Burn GIS.lab Desktop bootloader it to your USB stick. Be careful to
choose correct output device without a partition number.

::

    $ sudo dd if=http-boot/gislab-bootloader.iso of=/dev/sd[x]

Insert prepared USB stick in to client machine and instruct it to boot
from it.

Enable client machine
---------------------

By default, no client machines are allowed to boot from server. To allow
client machine, run following command on server:

::

    $ gislab-machines -a "<MAC address>"

| Tip:
| Good way to collect MAC addresses of client machines, is to simply let
them try to boot and than run following command to get list of denied
MAC addresses on server.

``$ sudo grep -e 'DHCPDISCOVER.*no free leases' /var/log/syslog``

Continue with `GIS Project Creation <GIS-Project>`__.
