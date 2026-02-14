---
layout: custom
title: "SPRINT 3: ADMINISTRACIÓ DE DOMINIS I SEGURETAT"
---

# Administració de dominis i seguretat

## Configuració LDAP - Servidor

Primerament al servidor canviarem el hostname i ho assignarem a **"server"**.

![alt text](imatges/1.png)

Ara editarem el **/etc/hosts** i ho editarem amb aquests valors, d'aquesta manera.

![alt text](imatges/2.png)

A continuació instal·larem el paquet **ldap-utils** i **slapd**.

![alt text](imatges/3.png)

Ara passem a la gestió de la configuració del paquet **ldap-utils**, aquí assignarem la contrasenya del Administrador, amb aquest cas alumne i fiquem tots els valors de la instal·lació per defecte.

![alt text](imatges/4.png)

Executem la comanda slapcat per veure que ho tenim configurat correctament, amb aquest cas ara mateix no tenim cap valor ni cap OU ni res de res.

![alt text](imatges/5.png)

Ara com que tinc un servidor sense interficie gràfica m'he passat el .zip i despres l'he descomprimit al servidor.

![alt text](imatges/6.png)

Ara hem de fer un reconfigure del slapcat, adjunto captures de els valors nous.
Primer li diem que no volem ometre la configuració del servidor OpenLDAP.

![alt text](imatges/7.png)

Establim el nom del domini DNS, en aquest cas `fje.clot` (o el que correspongui segons la imatge).

![alt text](imatges/8.png)

Posem el nom de l'organització.

![alt text](imatges/9.png)

Establim la contrasenya per a l'administrador del directori.

![alt text](imatges/10.png)

Triem el motor de base de dades, utilitzarem **MDB**.

![alt text](imatges/12.png)

Diem que **no** volem que s'esborri la base de dades quan es purgui el paquet.

![alt text](imatges/13.png)

Finalment, diem que **sí** volem moure la base de dades antiga.

![alt text](imatges/11.png)

## Població i Verificació del Directori LDAP

Un cop configurat el servidor, procedim a poblar el directori amb les unitats organitzatives i els usuaris.

Visualitzem l'estat actual o comprovem els fitxers LDIF que utilitzarem.

![alt text](imatges/14.png)

![alt text](imatges/15.png)

![alt text](imatges/16.png)

![alt text](imatges/17.png)

Afegim el contingut al directori LDAP utilitzant la comanda `ldapadd` i verifiquem que s'hagin creat correctament.

![alt text](imatges/18.png)

![alt text](imatges/19.png)

Realitzem consultes amb `ldapsearch` per verificar que els usuaris i grups estan ben definits.

![alt text](imatges/20.png)

![alt text](imatges/21.png)

![alt text](imatges/22.png)

![alt text](imatges/23.png)

![alt text](imatges/24.png)

![alt text](imatges/25.png)

## Configuració de Clients i Proves d'Autenticació LDAP

Un cop tenim el servidor funcionant i poblat, és hora de configurar l'autenticació perquè el mateix servidor (o altres clients) reconeguin els usuaris LDAP.

Instal·lem els paquets necessaris per a la integració amb NSS i PAM (`libnss-ldapd`, `libpam-ldapd` i `nscd`).

![alt text](imatges/26.png)

Durant la instal·lació, ens demanarà configurar el servidor LDAP. Introduïm la URI del servidor (`ldap://localhost` o la IP corresponent).

![alt text](imatges/27.png)

Indiquem la base de cerca del directori (Base DN), per exemple `dc=fje,dc=clot`.

![alt text](imatges/28.png)

Seleccionem quins serveis han d'utilitzar LDAP per a la resolució de noms. Marquem **passwd**, **group** i **shadow**.

![alt text](imatges/29.png)

Si fem un `getent passwd`, hauríem de veure els usuaris del sistema **i també** els usuaris definits a l'LDAP.

![alt text](imatges/30.png)

Finalment, per assegurar que es crea automàticament el directori personal (`home directory`) quan un usuari LDAP inicia sessió, cal configurar PAM. Editem `/etc/pam.d/common-session` i afegim la línia `session required pam_mkhomedir.so skel=/etc/skel umask=0077`.

![alt text](imatges/31.png)

![alt text](imatges/32.png)

Ara provem d'iniciar sessió amb un usuari de l'LDAP (per exemple `su - pere` o el nom d'usuari creat) i verifiquem que entrem correctament i es crea el seu directori home.

![alt text](imatges/33.png)

## Entorn Gràfic

Per a configurar LDAP en un entorn gràfic tenim moltes opcions com ara:

* phpldapadmin
* apache directory stdio
* jxplorer
* ldap account manager (LAM)

Amb aquest cas he escollit LAM ja que em sembla molt fàcil d'utilitzar i molt intuitiu.

### Requeriments Prèvis

Primerament hem d'instalar tots aquests paquets ja que LAM utilitza php i hem d'instalar tots els seus requeriments per al seu funcionament.

![alt text](EntornGrafic/2026-02-14_16-15.png)

I ara descarregaré el binari **.deb**.

![alt text](EntornGrafic/2026-02-14_16-20.png)

Un cop ja instal·lat podem accedir via la IP al directori **/lam**. Aqui ficarem una contrasenya per entrar al panel administratiu.

![alt text](image.png)

## Configuració del entorn gràfic

I una vegada ficada ja estem aquí dins per poder gestionar el **LDAP**.

![alt text](image.png)

Ara quan guardesim ens "expulsará" i ens fará iniciar sessió.

![alt text](EntornGrafic/2026-02-14_16-25.png)

Ara hem de crear un usuari per a la gestió d'aquest per tant anirem a **"Edit Server Profiles"**.

![alt text](EntornGrafic/2026-02-14_16-28.png)

I aquí farem clic amb aquest opció.

![alt text](EntornGrafic/2026-02-14_16-29.png)

I li assignem una contrassenya nova.

![alt text](EntornGrafic/2026-02-14_16-30.png)

I ja estem dins com a l'usuari IAM.

![alt text](EntornGrafic/2026-02-14_16-30_1.png)

Aquí ficarem la nostra configuració LDAP.

![alt text](EntornGrafic/2026-02-14_16-32.png)

També haurem de anar al **Account Types** i canviar aquesta configuració d'aquí.

![alt text](EntornGrafic/2026-02-14_16-35.png)

Ara ja podem iniciar sessió amb el usuari admin del nostre domini LDAP.

![alt text](EntornGrafic/2026-02-14_16-36.png)

Per aquí ja podem veure els usuaris que tenim creats.

![alt text](EntornGrafic/2026-02-14_16-37.png)

### Creació de una nova OU

Per a la creació d'una nova OU primerament hem d'anar a **"Tools"** i **"OU Editor"**.

![alt text](EntornGrafic/2026-02-14_16-40.png)

Aquí fiquem el nom de com volem que es digui aquesta OU.

![alt text](EntornGrafic/2026-02-14_16-41.png)

I fem OK. I ja la tenim creada.

![alt text](EntornGrafic/2026-02-14_16-42.png)

### Creació d'un nou grup

Per a crear un nou grup primerament hem d'anar a **"Accounts"** i **"Groups"**.

![alt text](EntornGrafic/2026-02-14_16-43.png)

Una vegada aquí anirem a **"New Group"**.

![alt text](EntornGrafic/2026-02-14_16-45.png)

I finalment crearem el grup, amb aquest cas li fiquem un nom i un UID.

![alt text](EntornGrafic/2026-02-14_16-46.png)

I s'ha creat correctament el grup.

![alt text](EntornGrafic/2026-02-14_16-46_1.png)

Comprovació:

![alt text](EntornGrafic/2026-02-14_16-47.png)

### Creació d'un nou usuari

Per a crear un nou usuari primerament hem d'anar a **"Accounts"** i després a **"Users"**.

![alt text](EntornGrafic/2026-02-14_16-48.png)

Aquí anirem a **"New User"**.

![alt text](EntornGrafic/2026-02-14_16-49.png)

Un cop aquí pasarem a la gestió personal.

![alt text](EntornGrafic/2026-02-14_16-50.png)

I ara la gestió UNIX.

![alt text](EntornGrafic/2026-02-14_16-51.png)

Finalment podem ficarli una contrasenya de la següent manera.

![alt text](EntornGrafic/2026-02-14_16-51_1.png)

I ja tenim el usuari creat correctament.

![alt text](EntornGrafic/2026-02-14_16-51_2.png)

Verificació:

![alt text](EntornGrafic/2026-02-14_16-52.png)

### Accedir desde el client amb aquest nou usuari creat

Per fer-ho he obert el client i he accedit via GUI per comprovar he executat la comanda `id`.

![alt text](EntornGrafic/2026-02-14_17-03.png)

## Gestió del domini mitjançant comandes

Moltes vegades no tindrem interfície gràfica com ara el LAM i ho haurem de fer tot via comandes. Per amb aquest apartat estaré seguint exercicis demanats:

![alt text](GestioDelDominiViaComandes/2026-02-14_17-08.png)

### Requisits previs

* Fes un dpkg-reconfigure slapd al servidor per tal de deixar la base de dades buida i només amb el domini i l’usuari admin creat. Comprova-ho amb un slapcat.

![alt text](GestioDelDominiViaComandes/2026-02-14_17-10.png)

* Descarrega l'arxiu dades_pt10.ldif del moodle i amb la comanda ldapadd carrega els usuaris, grups i uos (Compte que el domini és vesper.cat, hauràs de modificar-lo pel teu)

![alt text](GestioDelDominiViaComandes/2026-02-14_17-13.png)

I afegim el **.ldif** amb **ldapadd**.

``` bash
ldapadd -x -D "cn=admin,dc=eros,dc=cat" -W -f dades_pt10.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-15.png)

* Fes un altre slapcat per tal de comprovar que les dades s'han carregat correctament.

![alt text](GestioDelDominiViaComandes/2026-02-14_17-16.png)

### Exercicis

* Quantes uos hi ha? **Hi ha 2 UO** Quants usuaris hi ha al domini? **Hi ha 3 usuaris**

``` bash
ldapsearch -x -LLL -b "dc=eros,dc=cat" "(objectClass=organizationalUnit)" dn | grep "^dn:"

ldapsearch -x -LLL -b "dc=eros,dc=cat" "(objectClass=inetOrgPerson)" dn | grep "^dn:"
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-20.png)

* Crea una nova uo anomenada asix

Per a fer-ho hem de crear un arxiu anomenat **asix.ldif** amb el següent contingut:

``` bash
dn: ou=asix,dc=eros,dc=cat
objectClass: organizationalUnit
ou: asix
description: Unitat Organitzativa per al grup ASIX
```

I podem afegir-ho de la següent manera:

``` bash
ldapadd -x -D "cn=admin,dc=eros,dc=cat" -W -f asix.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-22.png)

* Esborra l’atribut roomnumber i homeDirectory de l’usuari ejohnson

Amb aquest cas no existeix cap usuari **ejohnson** i cap dels usuaris te assignat roomNumber i homeDirectory per tant primer ho afegiré i despres ho esborraré.

Com que l'usuari esta present no permet eliminar el homeDirectory pero si el seu roomNumber, output:

``` bash
dn: cn=xavier,ou=rrhh,dc=eros,dc=cat
changetype: modify
delete: roomNumber
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f delete_attrs.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-31.png)

* L’usuari kvaughan en quants grups el trobem com a uniqueMember i quins són?

Utilitzant l’usuari xavier com a substitut de kvaughan:

Nombre de grups: 1

Grups on apareix com a memberUid: informatica

``` bash
ldapsearch -x -LLL -b "dc=eros,dc=cat" "(&(objectClass=posixGroup)(memberUid=xavier))" cn
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-34.png)

* Trau de la uo People a 3 usuaris i afegeix-los a la uo asix

``` bash
dn: cn=xavier,ou=People,dc=eros,dc=cat
changetype: moddn
newrdn: cn=xavier
deleteoldrdn: 1
newsuperior: ou=asix,dc=eros,dc=cat

dn: cn=enric,ou=People,dc=eros,dc=cat
changetype: moddn
newrdn: cn=enric
deleteoldrdn: 1
newsuperior: ou=asix,dc=eros,dc=cat

dn: cn=sergi,ou=People,dc=eros,dc=cat
changetype: moddn
newrdn: cn=sergi
deleteoldrdn: 1
newsuperior: ou=asix,dc=eros,dc=cat
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f move_to_asix.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-39.png)

* Quants grups hi ha dintre de la uo Groups?

Amb aquest cas 0

``` bash
ldapsearch -x -LLL -b "ou=Groups,dc=eros,dc=cat" "(objectClass=posixGroup)" dn | grep "^dn:"
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-40.png)

* Esborra la uo People

``` bash
dn: ou=People,dc=eros,dc=cat
changetype: delete
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f delete_people_ou.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-41.png)

* Modifica el uid de l’usuari hmiller a hamiller

Com que el usuari hamiller no exiteix ho faré amb l'usuari xavier.

``` bash
dn: cn=xavier,ou=asix,dc=eros,dc=cat
changetype: modify
replace: uid
uid: xavier1
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f modify_uid_xavier.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-43.png)

* Crea un nou usuari amb dos atributs opcionals per a  la classe posixAccount

``` bash
dn: cn=marc,ou=asix,dc=eros,dc=cat
objectClass: inetOrgPerson
objectClass: organizationalPerson
objectClass: person
objectClass: posixAccount
objectClass: top
cn: marc
sn: Serra
uid: marc
uidNumber: 1010
gidNumber: 1001
homeDirectory: /home/marc
loginShell: /bin/bash
userPassword: marc123
gecos: Marc Serra
description: Usuari de prova amb atributs opcionals
```

``` bash
ldapadd -x -D "cn=admin,dc=eros,dc=cat" -W -f new_user.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-44.png)

* Afegeix un parell més d’opcionals a l’usuari anterior

``` bash
dn: cn=marc,ou=asix,dc=eros,dc=cat
changetype: modify
add: roomNumber
roomNumber: 202
-
add: telephoneNumber
telephoneNumber: 555-1234
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f add_optional_attrs.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-45.png)

* Modifica el mail de l’usuari jburrell per jburrell@gmail.com

Com que el usuari jburrell no existeix ho he fet amb marc.

``` bash
dn: cn=marc,ou=asix,dc=eros,dc=cat
changetype: modify
replace: mail
mail: marc@gmail.com
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f modify_mail.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-47.png)

* Crea un nou grup dintre de la uo Groups i afegeix 3 usuaris

Com que prèviament he mostrat que no existeix la UO Groups ho faré sobre la de asix.

``` bash
dn: cn=projecte,ou=asix,dc=eros,dc=cat
objectClass: posixGroup
objectClass: top
cn: projecte
gidNumber: 2001
memberUid: xavier
memberUid: enric
memberUid: sergi
```

``` bash
ldapadd -x -D "cn=admin,dc=eros,dc=cat" -W -f new_group_asix.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-49.png)

* Treu del grup creat anteriorment a un usuari

``` bash
dn: cn=projecte,ou=asix,dc=eros,dc=cat
changetype: modify
delete: memberUid
memberUid: enric
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f remove_user_from_group.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-50.png)

* Mostra tots els usuaris de la uo Asix que el seu uid comenci per la lletra x i formin part també de la uo Recursos Humans

``` bash
ldapsearch -x -LLL -b "ou=asix,dc=eros,dc=cat" "(uid=x*)" dn uid cn
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-53.png)

* Mostra tots els usuaris del domini on el seu uidNumber estigui entre 1010 i 1030 (inclosos). Quants en son?

``` bash
ldapsearch -x -LLL -b "dc=eros,dc=cat" "(&(objectClass=posixAccount)(uidNumber>=1010)(uidNumber<=1030))" dn uid cn
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-53.png)

* Usuaris on el seu telèfon acabi en un 2 o el seu cognom en una n. Quants?

Amb aquest cas son 0 usuaris.

``` bash
ldapsearch -x -LLL -b "dc=eros,dc=cat" "(| (telephoneNumber=*2) (sn=*n) )" dn uid cn telephoneNumber sn
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-54.png)

* D’un sol cop, a l’usuari que tu vulguis, esborra un atribut, afegeix-ne un altre i modifica un tercer.

``` bash
dn: cn=marc,ou=asix,dc=eros,dc=cat
changetype: modify

# Esborrem l'atribut roomNumber
delete: roomNumber
-
# Afegim un atribut nou title
add: title
title: Enginyer LDAP
-
# Modifiquem el mail
replace: mail
mail: marc.serra@eros.cat
```

``` bash
ldapmodify -x -D "cn=admin,dc=eros,dc=cat" -W -f modify_user_all.ldif
```

![alt text](GestioDelDominiViaComandes/2026-02-14_17-56.png)

### Tal i com planteja la activitat: Tots els exercicis en captures de pantalla i amb la comanda per tal de poder fer un copia/pegar

## Configuració Samba

Finalment, configurarem Samba per permetre l'accés a recursos compartits amb autenticació LDAP o local.

Primer instal·lem el paquet `samba`.

![alt text](sambaimg/2026-01-29_12-59.png)

Comprovem la instal·lació i els serveis.

![alt text](sambaimg/2026-01-29_13-21.png)

![alt text](sambaimg/2026-01-29_13-21_1.png)

Editem el fitxer de configuració `/etc/samba/smb.conf` per definir el grup de treball i els recursos compartits.

![alt text](sambaimg/2026-01-29_13-22.png)

![alt text](sambaimg/2026-01-29_13-30.png)

Configurem les opcions globals i els recursos específics, assegurant-nos que els permisos siguin correctes.

![alt text](sambaimg/2026-01-29_13-31.png)

![alt text](sambaimg/2026-01-29_13-32.png)

![alt text](sambaimg/2026-01-29_13-33.png)

![alt text](sambaimg/2026-01-29_13-33_1.png)

![alt text](sambaimg/2026-01-29_13-33_2.png)

Verifiquem la configuració amb `testparm` i reiniciem el servei.

![alt text](sambaimg/2026-01-29_13-34.png)

Finalment, comprovem l'accés als recursos Samba.

![alt text](sambaimg/2026-01-29_13-35.png)

![alt text](sambaimg/2026-01-29_13-35_1.png)

Mes comprovacions amb els usuaris creats:

#### Usuari Eros

Si intento crear una carpeta desde l'usuari Eros no me ho permetrá fer-ho.

![alt text](sambaimg/2026-02-14_18-12.png)

### Integració OpenLDAP

Primerament començarem descarregant el paquet.

![alt text](sambaimg/2026-02-14_18-16.png)

Ara passem a la configuració, primer de tot hem d'editar el **/etc/samba/smb.conf** i afegir el següent:

``` bash
# per indicar a on buscar les contrasenyes
passdb backend = ldapsam:ldap://10.0.2.7 

# Aqui l'informació del domini ldap 'eros.cat'
ldap suffix = dc=eros,dc=cat

ldap admin dn = cn=admin,dc=eros,dc=cat
# Sense SSL  perque ho vaig configurar així
ldap ssl = no

# I que sincronitzi les contrasenyes
ldap passwd sync = yes
```

![alt text](sambaimg/2026-02-14_18-23.png)

Ara a l'usuari alumne l'hi he posat contrasenya amb smbpasswd.

![alt text](sambaimg/2026-02-14_18-25.png)

I finalment reiniciem el servei. (El primer cop tarda una mica ja que esta interactuant amb el LDAP)

També hem d'executar la comanda

``` bash
sudo ldapadd -Q -Y EXTERNAL -H ldapi:/// -f /usr/share/doc/samba/examples/LDAP/samba.ldif
```

Per a que OpenLDAP es pugui utilitzar com a backend per a Samba, el DIT haurà d'utilitzar atributs que puguin descriure correctament les dades de Samba. Aquests atributs es poden obtenir introduint un esquema LDAP de Samba.

L'esquema es troba al paquet Samba ara instal·lat i ja està en format LDIF. El podem importar amb una simple ordre:

![alt text](sambaimg/2026-02-14_18-28.png)

### Proves client via CLI

#### User Enric

![alt text](sambaimg/2026-02-14_18-40.png)

#### User Marc

![alt text](sambaimg/2026-02-14_18-41.png)

## NFS

Es un protocol que ens permet compartir fitxers, directoris (no impressores) a traves de una xarxa local. L'autenticació es fa a través de host no usuari, a diferència de samba. Poden accedir tants clients Windows com Linux.

### NFS sense LDAP

En aquest apartat configurarem un servidor NFS per compartir directoris amb un client, sense utilitzar autenticació LDAP inicialment.

Primer, al **servidor**, instal·lem el paquet `nfs-kernel-server`.

![Instal·lació nfs-kernel-server](nfs/2026-02-10_12-52.png)

Creem el directori que volem compartir i li assignem els permisos necessaris. En aquest cas, creem `/1exercici`.

![Creació directori compartit](nfs/2026-02-10_12-54.png)

Editem el fitxer `/etc/exports` per definir qui pot accedir al recurs i amb quins permisos. Afegim la línia corresponent al nostre directori i xarxa/client.

![Configuració /etc/exports](nfs/2026-02-10_12-57.png)

Apliquem la nova configuració i reiniciem el servei `nfs-kernel-server` per assegurar-nos que els canvis s'apliquen.

![Reinici servei NFS](nfs/2026-02-10_12-58.png)

Ara crearem un arxiu anomenat `hola` a la carpeta `/1exercici`.

![Verificació exports](nfs/2026-02-10_13-00.png)

Ara passem al **client**. Instal·lem el paquet `nfs-common, rpcbind` i creem el punt de muntatge on vincularem el directori remot.

![Instal·lació nfs-common al client](nfs/2026-02-10_13-16.png)

Muntem manualment el recurs compartit NFS al punt de muntatge creat. Utilitzem la IP del servidor i la ruta del directori exportat.

![Muntatge manual NFS](nfs/2026-02-10_13-33.png)

Verifiquem que tenim accés d'escriptura (si així ho hem configurat) creant un fitxer de prova dins del directori muntat.

![Prova escriptura](nfs/2026-02-10_13-36.png)

Per fer que el muntatge sigui permanent i es mantingui després de reiniciar, afegim l'entrada corresponent al fitxer `/etc/fstab`.

![Configuració fstab](nfs/2026-02-10_13-37.png)

Finalment, podem reiniciar el client o fer un `mount -a` per comprovar que el recurs es munta automàticament sense errors.

![Verificació final fstab](nfs/2026-02-10_13-39.png)

### NFS amb LDAP

Primerament al nostre servidor prepararem el directori.

![alt text](nfs/2026-02-10_13-51.png)

I tal com hem fet anteriorment ficarem aquesta ruta **/homes** al **/etc/exports**.

![alt text](nfs/2026-02-10_13-53.png)

Un cop fet reiniciem el servei.

![alt text](nfs/2026-02-10_13-56.png)

Ara anirem al nostre client i farem el següent:

![alt text](nfs/2026-02-10_13-57_1.png)

Al fstab ficarem aquesta línia tal i com hem fet anteriorment.

![alt text](image-1.png)

Guardem i tornem a la part del servidor, ara crearem l'usuari Marcel. MOLT IMPORTANT INDICA EL SEU HOME amb aquest cas **/homes**

![alt text](nfs/2026-02-10_14-01.png)

I amb ldapadd l'afegim.

![alt text](nfs/2026-02-10_14-02.png)

Un cop fet aquesta gestió per part del client i el servidor, reiniciarem el client i entrarem com a l'usuari marcel. Si tot ha funcionat correctament dins de **/homes/marcel** hauriem de veure les carpetes basiques com ara **Descargas**, **Documentos** etc...

I si.

![alt text](nfs/2026-02-14_19-28.png)

![alt text](nfs/2026-02-14_19-28_1.png)

### NFS amb Windows

Primer de tot hem de anar **programas i caracterísiticas** i instal·lar el següent paquet.

![alt text](nfs/2026-02-14_19-32.png)

Ara busquem **nfs** i entrem.

![alt text](nfs/2026-02-14_19-33.png)

Ara tornem al servidor, ja que per fer aquesta prova crearé una nova carpeta.

![alt text](nfs/2026-02-14_19-37.png)

I el ficaré al **/etc/exports**.

![alt text](nfs/2026-02-14_19-37_1.png)

Seguidament reiniciem el servei.

![alt text](nfs/2026-02-14_19-38.png)

Ara passem a la part del client i obrirem una powershell com Administrador i executarem la següent comanda:

![alt text](nfs/2026-02-14_19-48.png)

Ara per visualitzar el share aquest hem d'anar al nostre explorador d'arxius i ja apareixerà.

![alt text](nfs/2026-02-14_19-49.png)

![alt text](nfs/2026-02-14_19-50.png)

## END

![alt text](theend.jpg)