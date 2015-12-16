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
- bootovať môže buď v móde PXE alebo cez HTTP

PXE 
- system (Network boot order, pointing device PS/2 mouse)
- network (Bridged adapter, PCnet-FAST III (Am79C973) adapter type)

- po vytvorení klienta je potrebné prihlásiť sa na server cez `vagrant ssh`
  a pomocou `sudo gislab-machines -a <MAC-address>` (napr. `sudo gislab-machines 
  -a 08:00:27:85:04:59) pridať, aktivovať, povoliť mu prístup (MAC adresa je v 
  záložke Network advanced)


