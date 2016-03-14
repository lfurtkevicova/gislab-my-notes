**************
Základné pojmy
**************

**Ansible** 

- automatizačný nástroj na správu systémov, používa sa pri unite, konfigurácia 
  je v tzv. **yaml** súboroch

**Barrel folder**

- hlavný priečinok zdieľania sietí

**Bridged adapter**

- ako prepojiť sieť klienta s VB

**customizácia** 

- zmena, resp. prispôsobenie systému individuálnym podmienkam používateľa

**chroot**

- je to akoby *root*, koreňový adresár, ale ako "podpriestor"; zmení koreňový 
  adresár pre práve bežiaci proces a jeho potomkov

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

- nástroj, ktorý umožňuje modifikovať virtuálny stroj (mašinku) podľa mojich 
  predstáv pomocou nejakého predpisu; dôležitý je *Vagrant file* + jednoduché 
  príkazy v shell-i; je to multiplatformový nástroj; kľúčovými sú poskytovateľ
  (provisioner) a prevádzkar (provider)

**Vagrant box**

- image (predpis, schéma, vzor operačného systému), aby sa virtuálny stroj 
  nevytváral od nuly; je to zabalený minimálny operačný systém, ktorý sa dá 
  spustiť; je to oveľa rýchlejšie

*INÉ*

**virtualenv**

- nástroj, ktorým možno vytvoriť python prostredie oddelené alebo doplňujúce to,
  čo je na serveri; do toho prostredia sa dá nainštalovať ľubovoľné knižnice
  ľubovoľnej verzie; prostredie aplikácie je oddelené od prostredia servera; 
  je to fyzický adresár, ktorý obsahuje unix-ovú štruktúru s niekoľkými
  binárkami, knižnicami, python modulmi 

**chmod** 

- slúži v systéme Unix na zmenu prístupových práv k súboru; zmeniť práva môže 
  len vlastník alebo *root*, meniť vlastníka môže len *root*
- práva sú udávené ako číslo v osmičkovej sústave buď absolútnym zápisom
  alebo symbolickým zápisom
- *absolútny zápis* sa najčastejšie zapisuje ako 3-miestne číslo **rwx** (prvá 
  číslica = vlastník, druhá číslica = práva skupiny, tretia číslica = ostatní)
- spustenie prispieva váhou "1", zápis váhou "2" a čítanie váhou "4", kombinácia
  všetkých je potom "721" .. chápem :)

**pozn.:**

- ak chcem zistiť, či funguje pripojenie na internet, jednoduchý príkaz je 
  napríklad ``ping 8.8.8.8`` (google)

