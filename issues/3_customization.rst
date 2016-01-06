Customizácia
------------

A) VIRTUAL (môj PC)
===================

SERVER
^^^^^^

- adresár **system**; defaultný súbor s konfiguračnými nastaveniami, t.j súbor 
  *all* je v adresári **group-vars**; ak chcem niečo meniť, vytvorím súbor 
  *gislab-vagrant* v adresári **host-vars** (v prípade, že **host_vars** 
  neobsahuje súbor *gislab_vagrant*, pri vytváraní klienta sa použije súbor *all*)
- GIS.lab má vlastnú sieť (predvolene 192.168.50.0/24), je to vlastne premenná
  GISLAB_NETWORK v súbore *all* 

KLIENT
^^^^^^

1) nainštalujem GIS.lab po stiahnutí repozitára **gis-lab**: ``vagrant up``
2) prihlásim sa na vagrant server: ``vagrant ssh``
3) pridám užívateľa: ``gislab-adduser ...``
4) prepnem sa do klientskeho root-a: ``gislab-client-shell -i``
5) urobím si tam čo chcem,napr. nainštalujem grass
6) prepnem sa naspäť na server (exit-om)
7) skomprimujem klientsky root adresár, čím vytvorím nový image (z neho budú 
   bootovať klienti): ``gislab-client-image`` (na základe aktuálneho root-a)
8) vo virtualbox-e vytvorím klienta, ktorý bootuje z nového image-u

B) UNIT (krabička, škola)
=========================
- prihlásenie cez ssh, najprv server ``ssh gislab@147.32.131.60 -p 12345 -X``, 
  potom bežiaci klient ``ssh user@192.168.17.50 -X``
