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

- v prípade, že ``host_vars`` neobsahuje súbor ``gislab_vagrant``, pri vytváraní 
  klienta sa použije len súbor ``all``; súbor ``gislab_vagrant`` by mal obsahovať 
  len zmeny
- konfigurácia inštalácie sa prejaví po aktualizácii pomocou ``vagrant provision``,
  prípadne je potrebné server reštartovať ako ``vagrant reload`` alebo 
  ``vagrant halt`` a ``vagrant up``
- po novom bootovaní klienta sa zmeny konfiguračné nastavenia prejavia
- GIS.lab má vlastnú sieť (predvolene 192.168.50.0/24), je to vlastne premenná
  GISLAB_NETWORK v súbore ``all``; je to nastavené tak, že server má vždy číslo 
  ``5`` a prvý klient číslo ``50``, t.j. 192.168.50.5 a 192.168.50.50; túto 
  informáciu je dobré vedieť pri manuálnom bootovaní cez HTTP, kde je potrebné
  zadať IP adresu servera

KLIENT
^^^^^^

1) nainštalujeme GIS.lab po stiahnutí *repozitára gis-lab*: ``vagrant up``
2) prihlásim sa na vagrant server: ``vagrant ssh``
3) pridám užívateľa: ``sudo gislab-adduser -g User -l GIS.lab -m user@gis.lab -p user user``
4) prepnem sa do klientskeho root-a: ``sudo gislab-client-shell -i``
5) urobím si tam čo chcem,napr. nainštalujem gedit 

   ``sudo apt-get update``

   ``sudo apt-get install gedit``

6) prepnem sa naspäť na server pomocou ``exit``
7) skomprimujem klientsky root adresár, čím vytvorím nový image, z neho budú 
   bootovať klienti: ``gislab-client-image`` (na základe aktuálneho chroot)
8) vo VirtualBox vytvorím klienta, ktorý bootuje z nového image-u

   **pozn.:**: niektoré zmeny sa prejavia iba pri vytvorené nového klienta
   (v budúcnosti by to malo byť doriešené) 

B) UNIT (krabička, škola)
=========================

- použijeme vopred pripravené customizačné skripty a spustíme ich pomocou 
  ``ansible-playbook``, napríklad spustenie súboru pre customizáciu servera 
  s názvom ``gislab-server-customize.yml``,
  ktorý máme uložený v adresári ``gislab-customization`` by vyzeralo

  ``ansible-playbook --inventory=../gislab/gislab-unit-fem.inventory --private-key=~/.ssh/id_rsa_gislab_unit gislab-server-customize.yml``

  spustenie súboru pre kompiláciu GRASS GIS

  ``ansible-playbook --inventory=../gislab/gislab-unit-fem.inventory --private-key=~/.ssh/id_rsa_gislab_unit gislab-grass-trunk.yml``

  a pre customizáciu klienta

  ``ansible-playbook --inventory=../gislab/gislab-unit-fem.inventory --private-key=~/.ssh/id_rsa_gislab_unit gislab-client-customize.yml``

  tento script (scripty) sa od inštalácie líši ``*.yml`` súborom na konci

- prihlásenie cez ssh, najprv užívateľ (user) ``ssh gislab@<IP od FEM> -p 12345 -X``, 
  potom bežiaci klient ``ssh gislab@c.50 -X``

- pri customizácii klienta musia byť príkazy customizácie zadávané v *chroot*; 
  príkazom `sudo gislab-client-shell -i` sa dostanem do chroot-a, tam niečo 
  napríklad nainštalujem, odhlásim sa klasicky ako `exit` a skriptom 
  `sudo gislab-client-image` 
  vytvorím nový image, t.j. skomprimovaný chroot (pri každej aktualizácii 
  GIS.lab-u sú klientsky root - *chroot* a *image* prepísané na východzí - originál)
- v ``host_vars`` pri unit-e sa konfiguračný súbor inštalácie nevolá 
  ``gislab_vagrant``, ale volá sa rovnako ako unit (v tomto súbore je dobré 
  editovať hlavne IP rozsah, na ktorej bude fungovať GIS.lab sieť; klienti budú
  potom so štvormiestnym číslom 192.168.50.xx)

*PRÁCA ADMINISTRÁTORA spravujúceho GIS.lab server:*

1. záloha *chroot* a *image*: je dobré zálohovať si klientsky root 
  (skomprimujem do adresára *backup* - vidí
  iba administrátor); je viac chroot-ov, klient môže byť desktop-ový alebo web-ový, 
  atď.; tu je príklad pre desktop-ový chroot a tiež image - binárny súbor, 
  ktorý Ivan nazval ako ``*.img``; image vlastne ani nemusím zálohovať, lebo 
  ten vždy vytvorím z chroot-a

   ``sudo tar cjf /mnt/backup/client-desktop-root-`date -I`.tar.bz2 /opt/gislab/system/clients/desktop/root``
 
   ``sudo cp -a /opt/gislab/system/clients/desktop/image /mnt/backup/client-desktop-image-`date -I` ``
  
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

  **pozn.:** v prípade, že užívateľ nabootuje s predvolenou customizáciou (*image*)
  a následne zmeníme image, pri odhlásení je upozornený na to, že existuje nový

  **pozn.:** môže sa stať, že niektoré ikony v lište užívateľa ostanú ako 
  nefunkčné (akoby stopa po predchádzajúcom *image*, z ktorého klient bootoval); 
  pri vytvorení nového užívateľa je všetko v poriadku (podľa aktuálneho *image*)
