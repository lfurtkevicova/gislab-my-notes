***********
GIS.lab Web
***********

- webová aplikácia postavená na tých najmodernejších technológiách s moderným 
  GUI, optimalozovaná aj pre mobilné zariadenia
- je to oddelený projekt od GIS.lab technológie s cieľom vytvoriť všeobecne 
  použiteľné QGIS webové rozhranie, ktoré je možné použiť s alebo bez GIS.lab
  infraštruktúry, prípadne nahradiť aktuálnu verziu QGIS webového klienta
- v súčasnoti neexistujú žiadne balíčky pre QGIS zásuvný modul,
  ale je možné nainštalovať vývojové prostredie vo virtuálnom móde pomocou 
  Vagrantu, ktorý všetko potrebné nainštaluje
- pre GIS.lab Web nie je potrebné mať GIS.lab
- potrebný je Linux alebo Mac OS 

- dôležitou súčasťou je **GIS.lab QGIS plugin** 
- umožňuje vytvoriť GIS.lab Web-ovú reprezentáciu (balíček) z akéhokoľvek QGIS 
  projektu  
- je možné pridávať podkladové mapy, vytvárať témy zo zoznamu vrstiev, nastaviť
  prístupové obmedzenia alebo platnosť projektu
- softvérové požiadavky: Linux/Mac, Git, VirtualBox, Vagrant, Ansible 1.9

- ``$ export APT_PROXY=http://192.168.99.118:3142`` kvôli cashovaniu balíčkov;
  ak to použijem, pri ďalšom vytváraní servera je to rýchlejšie, pretože nemusím 
  opäť sťahovať balíčky

- po stiahnutí adresára gislab-web:

  ``vagrant up``

  ``vagrant provision``

- pri zásadných zmenách ``vagrant destroy -f && git pull && vagrant up``

- development services (mapový, webový server, django... )

- tmux rozdeli terminál na plochy, koré potrebujem (logy z webové serveru = vľavo 
  dolu; obecné správy, s PCmôžem komunikovať cez http ...  
  mapový server = vpravo dole, vykreslovanie geodát ...)
- vypublikované súbory ako nové súbory s timestamp-om

  ``vagrant ssh``

  ``/vagrant/utils/tmux-dev.sh``

  ``https://localhost:8000?PROJECT=vagrant/natural-earth/central-europe``

  ``tmux kill-session``

  ``vagrant halt``
