Customizácia
------------

A) VIRTUAL (môj PC)
===================

SERVER
^^^^^^

- adresár ``system``; defaultný súbor s konfiguračnými nastaveniami pre 
  inštakáciu, t.j súbor ``all`` je v adresári **group-vars**; ak chceme niečo 
  meniť, napríklad ``GISLAB_CLIENT_KEYBOARDS`` na ``sk`` a `cz`, vytvoríme súbor 
  *gislab-vagrant* v adresári **host-vars** a nastavíme 
  .. code::
     GISLAB_CLIENT_KEYBOARDS:
     - layout: sk
       variant: qwerty
     - layout: cz
       variant: qwerty 
- v prípade, že ``host_vars`` neobsahuje súbor *gislab_vagrant*, pri vytváraní 
  klienta sa použije súbor ``all``
- konfigurácia inštalácie sa prejaví po aktualizácii pomocou ``vagrant provision``,
  prípadne je potrebné server reštartovať ako ``vagrant reload`` alebo 
  ``vagrant halt`` a ``vagrant up``
- po novom bootovaní klienta sa zmeny konfiguračné nastavenia prejavia
- GIS.lab má vlastnú sieť (predvolene 192.168.50.0/24), je to vlastne premenná
  GISLAB_NETWORK v súbore ``all`` 

KLIENT
^^^^^^

1) nainštalujeme GIS.lab po stiahnutí *repozitára gis-lab*: ``vagrant up``
2) prihlásim sa na vagrant server: ``vagrant ssh``
3) pridám užívateľa: ``gislab-adduser ...``
4) prepnem sa do klientskeho root-a: ``gislab-client-shell -i``
5) urobím si tam čo chcem,napr. nainštalujem grass
6) prepnem sa naspäť na server (exit-om)
7) skomprimujem klientsky root adresár, čím vytvorím nový image (z neho budú 
   bootovať klienti): ``gislab-client-image`` (na základe aktuálneho root-a)
8) vo VirtualBox vytvorím klienta, ktorý bootuje z nového image-u

B) UNIT (krabička, škola)
=========================
- prihlásenie cez ssh, najprv server ``ssh gislab@147.32.131.60 -p 12345 -X``, 
  potom bežiaci klient ``ssh user@192.168.17.50 -X``

- pri customizácii klienta musia byť príkazy customizácie zadávané v *chroot*; 
  príkazom `sudo gislab-client-shell -i` sa dostanem do chroot-a, tam niečo 
  napríklad nainštalujem, odhlásim sa klasicky ako `exit` a skriptom 
  `sudo gislab-client-image` 
  vytvorím nový image, t.j. skomprimovaný chroot (pri každej aktualizácii 
  GIS.lab-u sú klientsky root a image prepísané na východzí - originál)
- v ``host_vars`` pri unit-e sa konfiguračný súbor inštalácie nevolá 
  ``gislab_vagrant``, ale volá sa rovnako ako unit (v tomto súbore je dobré 
  editovať hlavne IP rozsah, na ktorej bude fungovať GIS.lab sieť; klienti budú
  potom so štvormiestnym číslom 192.168.50.xx)

PRÁCA ADMINISTRÁTORA spravujúceho GIS.lab server:
-------------------------------------------------

1. záloha *chroot* a *image*

- je dobré zálohovať si klientsky root (skomprimujem do adresára *backup* - vidí
  iba administrátor); je viac chroot-ov, klient môže byť desktop-ový alebo web-ový, 
  atď.; tu je príklad pre desktop-ový chroot:
  ``sudo tar cjf /mnt/backup/client-desktop-root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root`` 
  a tiež image (binárny súbor, ktorý Ivan nazval ako *.img) 
  ``sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/client-desktop-image-`date -I``
  (image vlastne ani nemusím zálohovať, lebo ten vždy vytvorím z chroot-a)

2. vymazanie *chroot* a *image*
   ``sudo rm -r /opt/gislab/system/clients/desktop/root``
   ``sudo rm -r /opt/gislab/system/clients/desktop/image``
   
3. obnovenie zo zálohy
   ``sudo tar xjf /mnt/backup/client-desktop-root-0000-00-00.tar.bz2 -C``
   ``sudo cp -a /mnt/backup/client-desktop-image-0000-00-00/ /opt/gislab/system/clients/desktop/image`` 

- dôležitý je teda súbor `gislab-server-customize.yml`, čo je súbor formátu pre 
  Ansible; jeho obsah je dobré meniť pomocou skriptov, kde sú záležitosti týkajúce
  sa customizácie pre server, ale aj užívateľov (napríklad proces pri vytváraní, 
  resp. mazaní užívateľov); 
- v `/opt/gislab/custom/accounts/` sú adresáre `before-add`, `after-add`, 
  `before-delete`, `after-delete` a `files`; napr. to vo `files` sa pri novom 
  užívateľovi prekopíruje do jeho `home`

*Prepínanie image-ov*:

- v `opt/.../client/` vytvorím link pomocou `ln -s` ako `sudo ln -s 'cesta_kde' 'cesta_image'`

 

