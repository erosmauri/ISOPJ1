---
layout: default
title: "Sprint 1: Instal·lació i Configuració Inicial"
---

Crear maquina virtual per instalar un Ubuntu (Particions 80GB, 40GB Ubuntu 40GB lliures)

# Virtualització i instal·lació del SO Ubuntu

## Pas 1:

* Aquí creo la configuració inicial de la VM.

![Imatge 1](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge1.png)

## Pas 2:

* Aquí assigno els requisits per a la VM.

    1. 4 GB de RAM.
    2. 2 CPU.

![Imatge 2](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge2.png)

## Pas 3:

* Aquí asigno 80GB de emmagatzemament al disc.

![Imatge 3](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge3.png)

## Pas 4:

* Aquí una captura del resum de la VM.

![Imatge 4](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge4.png)

## Pas 5:

* Aquí creem la xarxa NAT per a després assignar aquest adaptador. He assignat xarxa NAT ja que:

    1. La VM pot sortir a Internet però, per defecte, ningú des de fora no hi pot entrar.

![Imatge 5](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge5.png)

## Pas 6:

* Aquí un altre resum de la VM + cambio l'adaptador a xarxa NAT.

![Imatge 6](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge6.png)

## Pas 7:

* Aquí afegeixo la ISO - Ubuntu Desktop 22.04.

![Imatge 7](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge7.png)

## Pas 8:

* Aquí començo a instal·lar Ubuntu Desktop via GUI.

![Imatge 8](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge8.png)

## Pas 9:

* Aquí instal·lem Ubuntu.

![Imatge 9](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge9.png)

## Pas 10:

* Aquí seleccionarem mes opcions per a configurar manualment les particions.

![Imatge 10](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge10.png)

## Pas 11:

* Aquí he creat dues particions.

    1. Partició ext4 a l'arrel de 40 GB.
    2. Partició swap de 2 GB. (He creat aquesta partició de swap de 2 GB per tenir un espai extra de memòria virtual i més estabilitat si la RAM s’omple.)
    3. No he creat la partició home, ja que considero que al ser Ubuntu Desktop per a ús personal, no vull limitar-ho, per amb aixo estan les quotes.

![Imatge 11](imatges/Virtualització%20i%20instal·lació%20del%20SO%20Ubuntu/Imatge11.png)

## Pas 12:

* Ara només ens queda esperar a que s'instal·li Ubuntu.

# Llicènciament

Ubuntu és un sistema operatiu lliure i gratuït, basat en GNU/Linux. Està distribuït sota la llicència GNU GPL i altres llicències de codi obert, cosa que permet utilitzar-lo, modificar-lo i redistribuir-lo sense cost. No requereix cap clau de producte ni pagament, i pot emprar-se tant en entorns personals com professionals.

# Gestors d'arrencada per a instal·lacions DUALS

## Pas 1:

* Primerament als paràmetres de la máquina desde el VirtualBox hem d'habilitar la següent opció:

![Imatge 1](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge1.png>)

## Pas 2:

* Fiquem la ISO de Windows 10 Enterprise i arranquem la maquina fins arribar amb aquest punt.

![Imatge 2](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge2.png>)

## Pas 3:

* Aquí a les particions seleccionarem el disc de 40 GB que esta buit i instal·larem el Windows 10 allí.

![Imatge 3](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge3.png>)

## Pas 4:

* I ens esperem a que se instal·li.

![Imatge 4](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge4.png>)

## Pas 5:

* Ara crearem una cuenta de domini a nivell local al Windows.

![Imatge 5](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge5.png>)

## Pas 6:

* I ja tenim Windows 10 instal·lat.

![Imatge 6](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge6.png>)

## Pas 7:

* Ara per recuperar el grub he utilitzat la ISO de Super-Grub2:

![Imatge 7](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge13.png>)

## Pas 8:

* I bootejem desde aquesta ISO.

![Imatge 8](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge14.png>)

## Pas 9:

* Aqui detectarem i visualitzarem els boot menus.

![alt text](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge15.png>)

## Pas 10:

* I hem de buscar el següent:
    1. Aixo bootejara el Ubuntu directament. 

![alt text](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge16.png>)

## Pas 11:

* Ara que hem pogut arrancar Ubuntu podem arreglar el grub executant les següents comandes:

* sudo grub-install /dev/sda
* sudo update-grub2

![alt text](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge17.png>)

## Pas 12:

* Fem un reboot i ja veurem el grub per poder bootejar desde el Ubuntu o Windows.

![alt text](<imatges/Gestors d'arrencada per a instal·lacions DUALS/Imatge18.png>)

# Punts de restauració
# Configuració de la xarxa
# Comandes generals i instal·lacions