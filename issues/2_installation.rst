**********
Inštalácia
**********

A) VIRTUAL (môj PC)
===================

Server
^^^^^^

- platia nejaké minimálne požiadavky: 4GB RAM, SW ako *Linux, Git, Ansible, 
  VirtualBox, Vagrant* príslušných verzií, *Vagrant box* a *GIS-lab repozitár*.
- po ``vagrant up``, t.j. inštalácii GIS.lab-u sa vytvorí superuser **gislab**;
  po prihlásení na GIS.lab server cez SSH ``vagrant ssh`` možno pridať ďalších 
  používateľov napr. 
  ``sudo gislab-adduser -g User -l GIS.lab -m lab1@gis.lab -p lab lab1`` 
  (**lab1** je meno, **lab** je heslo). 
- zoznam používateľov sa dá vypísať cez ``sudo gislab-listusers``
- príklad:

.. code::
	
   $ sudo gislab-listusers | grep dn:
   dn: uid=gislab,ou=People,dc=gis,dc=lab
   dn: uid=lab1,ou=People,dc=gis,dc=lab
   $ sudo gislab-adduser -g User -l GIS.lab -m furtkevicova@gis.lab -p ludmila furtkevicova
   $ sudo gislab-listusers | grep dn:
   dn: uid=gislab,ou=People,dc=gis,dc=lab
   dn: uid=lab1,ou=People,dc=gis,dc=lab
   dn: uid=furtkevicova,ou=People,dc=gis,dc=lab

Klient
^^^^^^
- vytvorenie vo Virtual box-e
- bootovať možno buď v móde **PXE** (predvolený mód) - v záložke *System* treba 
  nastaviť *Network boot order, pointing device PS/2 mouse* a v *Network* záložke 
  *Bridged adapter* a *PCnet-FAST III (Am79C973) adapter type*; alebo možno 
  bootovať cez **HTTP** (mód s ***.iso image** súborom, ktorý je súčasťou 
  repozitára, v adresári ``http-boot``)
- po vytvorení klienta je potrebné prihlásiť sa na server cez ``vagrant ssh``
  a pomocou ``sudo gislab-machines -a <MAC-address>`` 
  (napr. ``sudo gislab-machines -a 08:00:27:85:04:59``) pridať, aktivovať, 
  povoliť mu prístup (MAC adresa je v 
  záložke *Network advanced* v prostredí *VirtualBox*); 
- potom spustíme klienta klasicky zelenou šípkou vo *VirtualBox*; je lepšie 
  bootovať opačne ako je pripojenie na internet, resp. v prípade virtuálu vypnúť 
  internet, aby nehľadalo IP v sieti; totiž ak v okolí existuje nejaký GIS.lab, 
  bude sa snažiť bootovať z neho, ale nepodarí sa to, keďže registrovaná MAC tam
  nie je)

B) UNIT (krabička)
==================

- krabička (budúci server) musí spĺňať isté požiadavky (16 GB RAM, CPU Intel 
  Core i5, ...)
- PC (budúci klient) musí spĺňať isté požiadavky (8 GB, HDMI to DVI adapter, ...)
- ďalej je potrebný aktuálny *GIS.lab repozitár* a nainštalovaný SW *Ansible* 
  príslušnej verzie

Server
^^^^^^

**1. krok:** inštalácia základného operačného systému

- budeme inštalovať *Ubuntu 12.04 Precise*; stiahneme aktuálny ``*.iso`` **image**, 
  64 bit pre server z oficiálnej stránky http://releases.ubuntu.com/precise, napr.
  *64-bit PC (AMD64) server install CD*
- toto ``*.iso`` ešte upravíme pomocou skriptu ``gislab-unit-iso.sh``; tento 
  skript má isté parametre ``-s -t -k -w`` a ``-i`` (pomocou prepínača ``-h`` 
  ich zobrazíme); 
- spustíme customizačný skript napr. ako: 
  ``sudo ./providers/gislab-unit/gislab-unit-iso.sh -s CZ -t Europe/Prague -k ~/.ssh/id_rsa_gislab_unit.pub -w /tmp/ -i ~/Downloads/ubuntu-12.04.5-server-amd64.iso``;
  skript zabezpečí, že sa napr. pri inštalácii nebude pýtať na jazyk, atď., ale
  bude to prednastavené; ďalej balíčky budú sťahované z najbližších archívov,
  napr. pri zadaní "CZ" to bude cz.ubuntu.com .. informácia, akú URL má použiť 
- potom použijeme program *Make Startup Disk*, ktorý z USB spraví bootovateľné
  USB (dáme tam 64 bit-ové upravené ``*.iso``)
  alebo v termináli použijem ``usb-creator-gtk``
- pri inštalácii budem používať **ssh** (lepšie je vytvoriť si *ssh* konkrétne 
  pre GIS.lab, aby som nepoužívala to, ktoré je napr. pre GitHub);
  príkazom ``ssh-keygen`` v priečinku ``home/.ssh`` vygenerujem nové *ssh*, 
  ktoré uložím napr. ako *id_rsa_gislab_unit* (pôjde o pár, verejný *.pub* a 
  privátny kľúč)
- potom do krabičky pripojím zdroj, klávesnicu, monitor, USB s ``*.iso``
  a internetový kábel;
- unit zapnem a stláčam ``F10`` (spustím aj bez siete), zadám **usr** ako *ubuntu*, 
  **psw** ako *ubuntu* a bootovanie z USB => potom mám na unit-e *Ubuntu*, ktoré 
  chcem vytvorené na základe súboru ``*.iso``
- sú dva spôsoby, ako sa dá prihlásiť na unit; zatiaľ sa prihlasujem len cez 
  heslo (*ubuntu/ubuntu*); to sa dá iba pomocou klávesnice a monitoru pripojených 
  k unit-u
- pri GIS.lab-e je to nastavené tak, že z cudzieho PC sa dá prihlásiť len cez 
  *ssh*, nie cez heslo; na to sme generovali *ssh*
 
  *pozn.:* unit musí byť v sieti registrovaný, musí mu byť pridelená IP adresa; 
  na to, aby zariadenie mohlo dostať IP, musí byť registrovaná jeho MAC adresa; 
  tú zistíme ako ``ip a`` po prihlásení cez heslo (ubuntu, ubuntu), 
  resp. ``ip a | grep eth0``; po zapnutí krabičky po inštalácii ubuntu z USB sa 
  buď unit registruje automaticky alebo je potrebné spustiť DHCP klienta 
  ``sudo dhclient eth0 -v``, ktorý požiada o IP a krabička má internet 
  (prepínač -v ako *verbous*; čím viac ``v``, tým podrobnejší popis, napr. ``-vvvv``)
- ak chceme zistiť z ľubovoľného PC, či sa môžeme prihlásiť a či je unit na sieti, 
  zadáme ``ssh ubuntu@ip.ad.re.sa``

**2. krok:** inicializácia unitu

- vytvoríme Ansible súbor, názov závisí od toho, ako sa bude unit volať, 
  napr. pre *gislab-unit-italy* to bude ``gislab-unit-italy.iventory``, je to 
  identifikátor vzdialeného PC
- jeho obsahom bude *názov GIS.lab* unit-u, *Ansible ssh host* s IP unit-u a 
  *názov užívateľa*, pod akým sa budeme prihlasovať k unit-u
  ``gislab-unit-italy ansible_ssh_host=00.00.00.00 ansible_ssh_user=ubuntu``
- potom spustíme *ansible-playbook* spolu s názvom *inventory* súboru, s ssh kľúčom 
  na pripojenie ku vzdialenému PC a so súborom ``*yml``, ktorý chceme spustiť 
  ``ansible-playbook --inventory=gislab-unit-italy.inventory --private-key=<private-SSH-key-file> providers/gislab-unit/gislab-unit.yml``
  s konkrétnymi cestami pre súbory ``*.inventory``, *privatekey* a ``gislab-unit.yml`` 
  (od tejto chvíle potrebujeme zdrojáky GIS.lab-u); týmto sa public časť ssh
  prekopíruje na unit a prístup bude možný už len cez *ssh*
  
  **pozn.:** *gislab-unit.yml* zabezpečí optimalizáciu napr. SSD, súborového systému, 
  swap, sieťové záležitosti, reštartuje krabičku, atď.; ide o to, že Ansible
  sa cez ssh prihlási na krabičku a pustí všetky príkazy v súbore ``*.yml``

  **pozn.:** v adresári *providers* sú skripty závislé na platforme; inicializačné
  súbory sú rôzne pre unit a rôzne pre AWS (Amazon web cloud)

**3. krok:** inštalácia unitu

- po inštalácii Ubuntu z USB sa GIS.lab sám vypne; vyberieme USB a krabičku 
  zapneme
- pred samotnou inštaláciou sa odporúča nastaviť aspoň základnú konfiguráciu
  inštalácie; konfiguračné súbory sú v adresári ``system`` a customizujú server 
  aj klientov; prípadne aj užívateľov, napríklad automatické veci pri vytvorení, 
  zmazaní užívateľov; ak chceme niečo meniť a nevyhovujú nám východzie nastavenia v 
  ``system/group_vars/all``, vytvoríme súbor s názvom unit-u, napr. 
  ``gislab-unit-italy`` s požadovanými nastaveniami
- po nakomfigurovaní GIS.lab-u môžeme pristúpiť k inštalácii; spustíme príkaz
  s príslušnými cestami k súborom ``*.inventory``, *privatekey* a ``*gislab.yml*``
  ``ansible-playbook --inventory=gislab-unit.inventory --private-key=<private-SSH-key-file> system/gislab.yml``

- objaví sa jediná záležitosť súvisiaca s kešovanými balíčkami (totiž, ak vieme,
  že budeme mať viac GIS.lab unit-ov a budeme ich inštalovať viackrát, je výhodné, 
  aby sa balíčky nesťahovali z internetu (napr. QGIS, GRASS, atď.), ale zo 
  zálohy; ak takúto zálohu balíčkov máme, zadáme, kde ich treba hľadať)

- po inštalácii sa na GIS.lab prihlásime z PC, z ktorého sme GIS.lab inštalovali
  cez ``ssh gislab@147.32.131 -i <cesta-k-suboru-ssh-*.pub,-ktoru-som-pouzila-pri-instalacii>``, 
  IP je uvedené v *inventory* súbore, je to IP pridelené od hlavného servera 
  pre prístup na internet, napr. od fakulty
- povolím prístup PC-om, ktorým chcem pomocou MAC adresy (príkaz na vypísanie
  MAC a IP adresy je ``ip a``)
- pri bootovaní PC napríklad klávesnicou ``F12`` musíme bootovať zo siete, 
  nie z DISK-u, z CD alebo inak; objaví sa MAC adresa a PC sa snaží požiadať najbližší server o IP
- túto MAC adresu zadáme ako administrátor GIS.lab-u pri povoľovaní prístupu do 
  siete GIS.lab v tvare ``sudo gislab-machines -a 00:00:00:00:00:00``
- po tomto zadaní, dostane PC od DHCP IP adresu a pri bootovaní zo siete sa 
  prihlási do siete GIS.lab
- z pozície administrátora ďalej zaregistrujem užívateľa ``sudo gislab-adduser``
  + prepínače (``-g -e -m -p``)

  **pozn.:** ak zadáme **-p**, ale nezadáme argument a ak je tento prepínač zadaný 
  ako posledný pred menom užívateľa, na heslo sa nás opýta
- užívateľa vymažem príkazom ``sudo gislab-deluser <meno-uzivatela>``
