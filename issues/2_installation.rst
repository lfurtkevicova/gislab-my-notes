**********
Inštalácia
**********

A) VIRTUAL (môj PC)
===================

Server
^^^^^^

- platia nejaké minimálne požiadavky: 4GB RAM, SW ako *Linux, Git, Ansible, 
  VirtualBox, Vagrant* (príslušných verzií) a stiahnutý *Vagrant box* a **GIS-lab** 
  repozitár.

- po `vagrant up`, t.j. inštalácii GIS.lab-u sa vytvorí superuser (gislab);
  po prihlásení na GIS.lab server cez SSH (vagrant ssh) možno pridať ďalších 
  používateľov napr. 
  `sudo gislab-adduser -g User -l GIS.lab -m lab1@gis.lab -p lab lab1` 
  (**lab1** je meno, lab je heslo). 

- zoznam používateľov sa dá vypísať cez `sudo gislab-listusers`

Klient
^^^^^^
- vytvorenie vo Virtual box-e
- bootovať môže buď v móde PXE (nastaviť treba v *System* záložke Network boot 
  order, pointing device PS/2 mouse a v *Network* záložke Bridged adapter a 
  PCnet-FAST III (Am79C973) adapter type) alebo cez HTTP

- po vytvorení klienta je potrebné prihlásiť sa na server cez `vagrant ssh`
  a pomocou `sudo gislab-machines -a <MAC-address>` (napr. `sudo gislab-machines 
  -a 08:00:27:85:04:59) pridať, aktivovať, povoliť mu prístup (MAC adresa je v 
  záložke Network advanced); 
- potom iba spustím klienta klasicky zelenou šípkou vo Virtual box-e; je lepšie 
  bootovať opačne ako je pripojenie na internet, resp. v prípade virtuálu vypnúť 
  internet (aby nehľadalo IP v sieti, lebo ak v okolí existuje nejaký GIS.lab, 
  bude sa snažiť bootovať z neho, ale nepodarí sa to, keďže registrovaná MAC tam
  nie je)

B) UNIT (krabička)
==================

- krabička (budúci server) musí spĺňať isté požiadavky (16 GB RAM, CPU Intel 
  Core i5, ...)
- PC (budúci klient) musí spĺňať isté požiadavky (8 GB, HDMI to DVI adapter, ...)
- ďalej je potrebný aktuálny GIS.lab repozitár a nainštalovaný SW Ansible 
  príslušnej verzie

Server
^^^^^^

- budeme inštalovať Ubuntu 12.04 Precise (stiahneme aktuálny ISO image (64 bit) 
  z oficiálnej stránky http://releases.ubuntu.com/precise)
- toto iso ešte upravíme pomocou skriptu *gislab-unit-iso.sh*; tento skript má 
  isté parametre -s -t -k -w a -i (pomocou prepínača *-h* ich zobrazíme); 
- spustíme customizačný skript napr. ako: 
  ``sudo ./providers/gislab-unit/gislab-unit-iso.sh -s CZ -t Europe/Prague -k ~/.ssh/id_rsa_gislab_unit.pub -w /tmp/ -i ~/Downloads/ubuntu-12.04.5-server-amd64.iso``;
  (skript zabezpečí, že sa napr. pri inštalácii nebude pýtať na jazyk, atď., ale
  bude to prednastavené) 
- potom použijem program MAKE STARTUP DISK, ktorý mi z USB spraví bootovateľné
  USB (dám tam 64 bit-ové upravené *iso*)
- pri inštalácii budem používať **ssh** (lepšie je vytvoriť si *ssh* konkrétne 
  pre gis.lab, aby som nepoužívala to, ktoré je napr. pre GitHub);
  príkazom ``ssh-keygen`` v priečinku *home/.ssh* vygenerujem nové *ssh*, 
  ktoré uložím napr. ako *id_rsa_gislab_unit* (pôjde o pár, verejný *.pub* a 
  privátny kľúč)
- potom do krabičky pripojím *zdroj*, *klávesnicu*, *monitor* a *USB s iso-om*;
  unit zapnem a stláčam *F10* (spustím aj bez siete), zadám usr ako *ubuntu*, 
  psw ako *ubuntu* a bootovanie z USB => potom mám na unit-e Ubuntu, ktoré chcem
  (GIS.lab)
 





