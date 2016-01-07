**************
Základné pojmy
**************

**Ansible** 

- automatizačný nástroj na správu systémov; používa sa pri unite; konfigurácia 
  je v *yaml* súboroch

**Barrel folder**

- hlavný priečinok zdieľania sietí

**Bridged adapter**

- ako prepojiť sieť klienta s VB

**customizácia** 

- zmena, resp. prispôsobenie systému individuálnym podmienkam používateľa

**DHCP server**

- GIS.lab má vlastný DHCP server, ku ktorému majú prístup len klienti GIS.lab
  na základe MAC adresy 

**IP adresa**

- sieťová adresa; IP adresy prideľuje DHCP server; to, či vôbec IP adresu 
  pridelí sa rozhodne na základe MAC adresy; pri VB sa MAC adresa generuje 
  automaticky a je treba ju 
  zadať `gislab-machines -a "<ma:ca:dd:re:ss:aa>"`; jeden PC má rôzne IP na rôznych
  miestach (podľa toho, k akej sieti je pripojený); nemôžu byť rovnaké IP adresy
  v rámci jednej podsiete (to isté platí aj pre MAC adresy); IP je vyššia vrstva 
  ako MAC

**konfigurácia** 

- usporiadanie, zoskupenie

**MAC adresa**

- hardvérová adresa, MAC adresa je adresa konkrétneho PC, nemôžu byť rovnaké 
  MAC adresy; MAC používa hardware sieťovky na to, aby vedel čo má prijať 
  (xx:xx:xx:xx:xx:xx)

**PXE (Preboot Execution Environment)**

- technológia pre bootovanie (štart PC z PC siete); pre tenkých klientov, ktorí 
  nemajú pevný disk; využíva sa IP z rodiny protokolov TCP/IP; komunikácia 
  pomocou DHCP (Dynamic Host Configuration Protocol): DHSPDISCOVER, DHCPOFFER,
  DHCPREQUEST, DHSPACK   

**Projects folder**

- uložené projekty klienta

**Publish folder**

- klient; umožňuje publikovanie do web-u

**Repository folder**

- hlavne administrátori

**Travis CI**

- webová služba podporujúca priebežnú integráciu projektov, resp. repozitárov 
  na GitHub-e (Continuous Integration); zaisťuje **zostavenie** a **otestovanie** 
  projektu; dôležitý je súbor *.travis.yml*, ktorý musí byť v hlavnom adresári 
  na GitHub-e; urýchľuje vývoj SW (a rovnako aj spoluprácu v tíme) 

**Vagrant** 

- nástroj, ktorý umožňuje modifikovať virtuálny stroj podľa mojich 
  predstáv pomocou nejakého predpisu

**Vagrant box**

- image (predpis, schéma, vzor operačného systému), aby sa virtuálny stroj 
  nevytváral od nuly; je to zabalený minimálny operačný systém, ktorý sa dá 
  spustiť; je to oveľa rýchlejšie
