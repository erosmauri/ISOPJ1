---
layout: custom
title: "SPRINT 2: INSTAL·LACIÓ, CONFIGURACIÓ DE PROGRAMARI DE BASE I GESTIÓ DE FITXERS"
---

# Sistemes de fitxers i particions

## Mida sector

El sector és la unitat mínima física del disc on es guarden les dades i per defecte són **512 bytes**. No es pot cambiar la mida.

## Mida block

És la unitat mínima lògica on es guarden les dades al SO, per defecte són 4096 bytes. I es pot canviar la mida quan es formata el disc.

La mida del block o cluster i el sistema de fitxers pot ser diferent a cada partició del mateix disc.

Exemple:

* Amb aquest cas podem veure amb la primera comanda el que pesa el text "Bon dia" (8 bytes), i amb la segona comanda podem observar la mida en disc, aquest es l'espai mínim que el sistema de fitxers reserva per a un fitxer.

![1](Imatges/1.png)

## Fragmentació interna

És quan es desaprofita espai perque els blocs són massa grans per al que s'ha de guardar dins.

## Fragmentació externa

És quan a mesura que vas treballant l'espai lliure total es va trencant en petits trossos separats.

## Tipus de formateig

- Baix nivell

Borra sistema de fitxers, borra formateig, etc. És a dir, que borra totes les dades i el deixa com a nou.
Des del sistema operatiu no es pot formatar, es necessiten programes adients.

- Mig nivell

Només borra sistema de fitxers pero si hi han sectors defectuosos els marca pero no els arregla.

- Alt nivell

El format d'alt nivell només borra el sistema de fitxers.

## Gestió de particions

Es una agrupacio logica de particions i/o discos, es posar una capa d'abstració damunt de les particions.

### Comandes

* Amb la comanda `fdisk -l` podem veure l'espai.

![2](image.png)

* Amb aquesta comanda podem mirar la mida del bloc de la partició, i filtrem amb grep per la paraula "Block".

![3](image-1.png)

* Per a la fragmentació externa podem fer-ho amb la comanda "e4defrag", aquí ens en indica si fa falta fragmentar alguna partició.

![4](Imatges/4.png)

* En cas de voler-ho fragmentar podem executar la comanda pero sense el parametre "-c".

![5](Imatges/5.png)

### GPARTED

* Primerament, diem que gparted es el editor de particions de GNOME per a crear, reorganitzar i eliminar particions de disc. 

* Permet triar el sistema de fitxers (FAT32, EXT4, NTFS…) pero no es pot modificar la mida del block.

#### Via interficie gràfica (GPARTED)

Els passos a seguir son primerament executem l'eina i seleccionem el disc dalt a la dreta.

![alt text](Imatges/20.png)

* Ara anirem a "Dispositivo" i "Crear tabla de particiones".

![alt text](Imatges/21.png)

* Aquí ens sortira una alerta i hem de canviar el tipo de tabla de particiones i posar-ho amb "gpt".

![alt text](Imatges/22.png)

* Un cop ja ho tenim podem fer clic dret sobre la particio per a crear una nova partició.

![alt text](Imatges/23.png)

* Aquí hem de posar-ho amb NTFS i podem canviar la mida de la partició.

![alt text](Imatges/24.png)

* Per finalitzar hem de aplicar els canvis, per fer-ho cliquem al tick verd i acceptem la confirmació que ens surt.

![alt text](Imatges/25.png)

#### Via CLI (Command Line Interface)

Per realitzar-ho ho farem amb la comanda **fdisk**.

Anteriorment com que ja indentificat quina es la meva partició, un cop ja ho sabem executem la comanda i seguim els passos que s'observen a la captura de pantalla.

![alt text](Imatges/7.png)

* Ara creem la partció.

![alt text](Imatges/8.png)

* Aqui podem observar que esta creat correctament.

![alt text](Imatges/9.png)

* Ara amb la comanda "mkfs.ext4" podem canviar la mida del bloc amb aquest cas ho posaré amb **2048**.

![alt text](Imatges/10.png)

* I podem comprovar-ho amb aquesta comanda.

![alt text](Imatges/11.png)

* I a l'altra partició com a **NTFS** per a que Windows ho reconeixi.

![alt text](Imatges/12.png)

* Finalment podem entrar al **GPARTED** i comprovar-ho.

![alt text](Imatges/13.png)

### Muntatge

Per fer aquest apartat primerament començarem creant una carpeta i arxiu a la ruta **/mnt**.

![alt text](Imatges/16.png)

* Muntem temporalment amb ```mount -t ext4 /dev/sdb1``` **/mnt/particio1**, i afegim un arxiu dintre.

![alt text](Imatges/15.png)

* Si reiniciem la particio que acabem de muntar ja no es trobara, pero els arxius que hem creat no se han borrat ja que encara estan emmagatzemades al disc.

* A continuació podem fer-ho de manera persistent. Per fer-ho editarem el fitxer **/etc/fstab**.

![alt text](Imatges/18.png)

* Si ara reiniciem amb aquest cas es persistent.

![alt text](Imatges/19.png)

## Gestió de procesos

Un procés és la instància d'un programa en execució. A cadascun se li assigna un identificador únic (PID), està associat a un usuari propietari i pot trobar-se en diversos estats (com ara en execució, en espera o aturat). El sistema operatiu és el responsable de la planificació i la distribució del temps de CPU entre tots els processos.

### Eines bàsiques de gestió de processos

Per gestionar els processos, disposem d'unes eines fonamentals:

    Per visualitzar-los:

        ps, top, htop → Mostren els processos actius.

    Per finalitzar-los:

        kill, pkill → Tanquen un procés pel seu PID o nom.

    Per gestionar la prioritat:

        nice, renice → Ajusten la prioritat d'execució.

    Per controlar serveis (daemons):

        systemctl, service → Inicien, aturen o reinicien serveis del sistema.

**Aspectes pràctics**: Cal recordar que un procés hereta els permisos de l'usuari que l'ha llançat i pot estar associat tant a un servei del sistema com a una sessió d'usuari.

* A continuació, veurem com utilitzar aquestes eines a nivell bàsic.

## Gestió d'usuaris i grups i permisos

El model de seguretat de Linux es basa en els conceptes d'usuaris i grups, que defineixen de manera precisa qui pot accedir, modificar o executar arxius i processos al sistema.

### Tipus d'Usuaris

**Usuari Normal**: Un usuari estàndard que pot iniciar sessió i treballar dins del seu entorn i espai personal. Els seus permisos són limitats per protegir la integritat del sistema.

**Superusuari (root)**: L'administrador del sistema. Té accés i control absolut sobre totes les operacions i arxius. S'ha d'utilitzar amb extrema cura.

**Usuari de Servei (Daemon)**: Comptes especials creats per a l'execució de serveis o aplicacions (com www-data per a un servidor web o mysql per a la base de dades). No poden iniciar sessió interactiva.

**Usuari de Sistema**: Són similars als usuaris de servei i solen tenir un UID (User ID) baix (normalment per sota de 1000). Estan reservats per a processos i funcions internes del sistema operatiu.

### Grups

Un grup és una col·lecció d'usuaris que comparteixen els mateixos permisos sobre certs arxius o directoris. Cada usuari pertany a:

    Un grup principal, que es defineix al crear l'usuari.

    Múltiples grups secundaris, als quals es pot afegir posteriorment.

Els grups són una eina essencial per a la gestió eficient de permisos, ja que permeten, per exemple, concedir accés a una carpeta compartida a tot un equip de treball d'una sola vegada, en lloc de configurar els permisos per a cada usuari individualment.

## Fitxers importants

* En Linux, la informació d'usuaris i grups es gestiona de manera centralitzada mitjançant fitxers de configuració de text ubicats dins del directori /etc.

Explicació **/etc/passwd**:

![alt text](<Gestió d'usuaris i grups i permisos img/1.png>)

Cada línia representa un usuari i conté 7 camps separats per dos punts:

**nom_usuari:x:UID:GID:GECOS:directori_home:shell**

Descripció detallada de cada camp

1. **nom_usuari**

    Exemple: root, anna, mysql

    Descripció:

        Nom únic que identifica l'usuari al sistema

        És el que s'utilitza per iniciar sessió

        Normalment té un màxim de 32 caràcters

2. **x (camp de contrasenya)**

    Exemple: x, *, !

    Descripció:

        x indica que la contrasenya està emmagatzemada a /etc/shadow

        * o ! vol dir que el compte està blocat

        Si està buit, l'usuari no té contrasenya (insicur)

3. **UID (User ID)**

    Exemple: 0, 1000, 33

    Descripció:

        Número d'identificació únic de l'usuari

        0 = usuari root (superusuari)

        1-999 = usuaris del sistema (serveis)

        1000+ = usuaris normals

4. **GID (Group ID)**

    Exemple: 0, 1000, 33

    Descripció:

        Número del grup principal de l'usuari

        Defineix els permisos per defecte per a nous arxius

5. **GECOS (Informació addicional)**

    Exemple: Anna Garcia,,,, Pere Lopez,Vendes,555-1234

    Descripció:

        Informació opcional sobre l'usuari

        Normalment només s'inclou el nom complet

        Format: Nom complet,Despatx,Telefon,Altres

6. **directori_home**

    Exemple: /home/anna, /root, /var/www

    Descripció:

        Directori personal de l'usuari

        On s'emmagatzemen els seus arxius personals

        Directori per defecte en iniciar sessió

7. **shell**

    Exemple: /bin/bash, /bin/sh, /usr/sbin/nologin

    Descripció:

        Intèrpret d'ordres que s'executa en iniciar sessió

        /bin/bash = shell Bash normal

        /usr/sbin/nologin o /bin/false = no permet inici de sessió (comptes de servei)

Explicació **/etc/shadow**:

![alt text](<Gestió d'usuaris i grups i permisos img/2.png>)

L'arxiu /etc/shadow conté la informació de les contrasenyes dels usuaris i les polítiques d'expiració. És un arxiu segur que només pot llegir l'usuari root.

Cada línia representa un usuari i conté 9 camps separats per dos punts:

**nom_usuari:contrasenya_encryptada:darrers_canvis:minims:maxims:avis:inactiu:caducitat:camp_reserva**

Descripció detallada de cada camp

1. **nom_usuari**

    Exemple: root, anna, mysql

    Descripció:

        Nom de l'usuari (ha de coincidir amb /etc/passwd)

        Serveix com a clau d'enllaç entre els dos arxius

2. **contrasenya_encryptada**

    Exemple: $6$rounds=5000$t..., *, !!

    Descripció:

        Contrasenya encryptada amb hash

        * o !! = compte blocat o sense contrasenya

        Format: $algoritme$salt$hash

        Algoritmes comuns: $1$ (MD5), $5$ (SHA-256), $6$ (SHA-512)

3. **darrers_canvis (last change)**

    Exemple: 19157, 0

    Descripció:

        Data de l'últim canvi de contrasenya en dies des de l'1/1/1970

        0 = ha de canviar-la en el proper login

        19157 = 19,157 dies des de l'1/1/1970

4. **minims (minimum days)**

    Exemple: 0, 7

    Descripció:

        Dies mínims que han de passar abans de poder canviar la contrasenya

        0 = es pot canviar en qualsevol moment

5. **maxims (maximum days)**

    Exemple: 99999, 90

    Descripció:

        Dies màxims que la contrasenya és vàlida

        99999 = quasi etern (273 anys)

        90 = ha de canviar-la cada 90 dies

6. **avis (warning days)**

    Exemple: 7, 0

    Descripció:

        Quants dies abans de la caducitat s'envia un avís

        7 = avisa 7 dies abans que caduqui

7. **inactiu (inactive days)**

    Exemple: -1, 30

    Descripció:

        Dies de gràcia després de caducar abans que el compte es desactivi

        -1 = sense període d'inactivitat

8. **caducitat (expiration date)**

    Exemple: ``, 20000

    Descripció:

        Data absoluta de caducitat del compte en dies des de l'1/1/1970

        Buit = el compte no caduca mai

9. **camp_reserva (reserved field)**

    Exemple: (buit)

    Descripció:

        Camp reservat per a ús futur

        Normalment està buit

Explicació **/etc/group**:

![alt text](<Gestió d'usuaris i grups i permisos img/14.png>)

L'arxiu /etc/group conté la informació dels grups del sistema i els seus membres. Defineix els grups d'usuaris i les seves relacions.

Estructura de cada línia

Cada línia representa un grup i conté 4 camps separats per dos punts:

**nom_grup:contrasenya_grup:GID:llista_membres**

Descripció detallada de cada camp

1. **nom_grup**

    Exemple: root, users, sudo, www-data

    Descripció:

        Nom del grup

        Ha de ser únic al sistema

        Normalment en minúscules

2. **contrasenya_grup**

    Exemple: x, *

    Descripció:

        x indica que la contrasenya del grup està a /etc/gshadow

        * o buit = no hi ha contrasenya de grup

        Rarament s'utilitza en sistemes moderns

3. **GID (Group ID)**

    Exemple: 0, 100, 1000, 33

    Descripció:

        Número d'identificació únic del grup

        0 = grup root

        1-999 = grups del sistema

        1000+ = grups d'usuaris normals

4. **llista_membres**

    Exemple: anna,pere,marta, root, (buit)

    Descripció:

        Llista d'usuaris que són membres del grup, separats per comes

        No inclou l'usuari que té aquest grup com a grup primari

        Buit = cap usuari addicional al grup

Explicació **/etc/gshadow**:

![alt text](<Gestió d'usuaris i grups i permisos img/15.png>)

L'arxiu **/etc/gshadow** conté la informació segura dels grups, incloent contrasenyes de grup i administradors. És la contrapart segura de **/etc/group**.

Estructura de cada línia

Cada línia representa un grup i conté 4 camps separats per dos punts:

**nom_grup:contrasenya_encryptada:administradors:membres**

Descripció detallada de cada camp

1. **nom_grup**

    Exemple: root, sudo, developers

    Descripció:

        Nom del grup (ha de coincidir amb /etc/group)

        Serveix com a clau d'enllaç entre els dos arxius

2. **contrasenya_encryptada**

    Exemple: !, $6$rounds=5000$..., *

    Descripció:

        Contrasenya encryptada per canviar al grup amb newgrp

        ! o * = no hi ha contrasenya de grup

        Contrasenya vàlida = hash encryptat

        Rarament s'utilitza en sistemes moderns

3. **administradors**

    Exemple: anna,root, pere, (buit)

    Descripció:

        Llista d'usuaris que poden gestionar el grup

        Poden afegir/eliminar membres i canviar la contrasenya del grup

        Separats per comes

4. **membres**

    Exemple: marta,jordi, user1,user2, (buit)

    Descripció:

        Llista d'usuaris que són membres del grup

        Ha de coincidir amb el camp de membres de /etc/group

        Separats per comes

També tenim l’utilitat que ve en instal·lar **gnome-system-tools**. Que permet un poquet més.

![alt text](<Gestió d'usuaris i grups i permisos img/16.png>)

## Comandes bàsiques

### Adduser

![alt text](<Gestió d'usuaris i grups i permisos img/3.png>)

### Userdel

* Aqui elimino el usuari.

![alt text](<Gestió d'usuaris i grups i permisos img/4.png>)

* Aqui creo l'usuari amb useradd i faig les comprovacions adients. Tambe canvio el tipus de shell.

![alt text](<Gestió d'usuaris i grups i permisos img/5.png>)

* Aqui canvio la contraseña amb la comanada **passwd**.

![alt text](<Gestió d'usuaris i grups i permisos img/6.png>)

* De aquest manera podem bloquejar el usuari, si observem el shadow podem veure que es veu de manera diferent.

![alt text](<Gestió d'usuaris i grups i permisos img/7.png>)

* I amb el parametre "-U" ho podem llevar.

![alt text](<Gestió d'usuaris i grups i permisos img/8.png>)

* Comanda addgroup per crear un grup. I amb groupmod podem canviar coses dels grups.

![alt text](<Gestió d'usuaris i grups i permisos img/9.png>)

* De aquesta manera podem assignar usuaris a grups.

![alt text](<Gestió d'usuaris i grups i permisos img/10.png>)

* Comprovació

![alt text](<Gestió d'usuaris i grups i permisos img/11.png>)

* Amb **deluser** podem també eliminar o desasignar del grup un usuari.

![alt text](<Gestió d'usuaris i grups i permisos img/12.png>)

* Amb **su** ens podem connectar al usuari.

![alt text](<Gestió d'usuaris i grups i permisos img/13.png>)

### Permisos

* Per fer aquest apartat he creat la carpeta palomes. Tambe un arxiu prova i he cambiat el grup propietari.

![alt text](Permisos/1.png)

* Aqui podem observar la jerarquia dels permisos

![alt text](Permisos/2.png)

Aqui estic configurant els permisos de la carpeta /var/palomes:

    Canvio el grup propietari de nick a paloma

    Afegeixo l'usuari ctre al grup paloma

    Estableixo permisos 750 a la carpeta

![alt text](Permisos/3.png)

Si em connecto desde el usuari nick puc comprovar que puc fer de tot.

![alt text](Permisos/4.png)

En canvi si ho faig desde el usuari cire no puc fer-ho.

![alt text](Permisos/5.png)

I si ho faig desde el usuari ferran no puc ni entrar dins.

![alt text](Permisos/6.png)

Aqui estic ampliant els permisos de la carpeta /var/palomes:

    Afegeixo l'usuari ferran al grup paloma

    Afegeixo l'usuari detvy al grup paloma

    Dono permisos d'escriptura al grup amb chmod g+w

![alt text](Permisos/7.png)

Aqui estic verificant els permisos de la carpeta /var/palomes:

    Com l'usuari deivy (del grup paloma):

        Puc accedir a la carpeta

        Puc crear fitxers (touch ptl)

    Com l'usuari ferran (també del grup paloma):

        Puc accedir a la carpeta i llistar contingut

        Puc eliminar fitxers (demana confirmació perquè el fitxer és protegit)

Resultat que confirmo:

    Tots dos usuaris del grup paloma poden accedir i gestionar fitxers

    Els fitxers existents tenen permisos restringits (protegits contra escriptura)

    La carpeta té permisos correctes perquè el grup pugui treballar

![alt text](Permisos/8.png)

Aqui estic comprovant els permisos de la carpeta **/var/palomes** amb diferents usuaris del grup:

Com a deivy (grup paloma):

    Puc entrar a la carpeta

    Puc crear fitxers nous (ptl)

Com a ferran (grup paloma):

    Puc entrar i veure el contingut

    Puc eliminar fitxers (demana confirmació)

Confirmo que:

    La carpeta té els permisos correctes per al grup

    Tots els membres del grup poden crear i eliminar fitxers

    Els fitxers estan protegits contra escriptura (per això demana confirmació)

![alt text](Permisos/9.png)

Aqui estic veient un problema amb els permisos de la carpeta **/var/palomes**:

El que funciona:

    deivy pot crear fitxers (ptlvaras)

    ferran pot crear fitxers (ddd)

    Tots dos poden accedir a la carpeta

El problema:

    ferran NO pot eliminar el fitxer ptlvaras de deivy

    La carpeta mostra drwxrwx--T (el sticky bit T està actiu)

La causa:

El sticky bit (la T al final dels permisos) està activat. Això vol dir que:

    Els usuaris només poden eliminar els seus propis fitxers

    No poden eliminar fitxers d'altres usuaris, encara que siguin del mateix grup

El sticky bit està limitant la capacitat dels usuaris del grup per eliminar fitxers d'altres membres.

![alt text](Permisos/10.png)

Finalment per als permisos **SUID**, he fet un metode que serveix per a escalar privilegis amb aquest cas he donat permisos SUID a la /bin/bash, el que em permet aixo es desde qualsevol usuari no privilegiat poden aconseguir una shell privilegiada, ja que quan ho executem ho estem fent com a root.

## ACL

### Importància de les ACL a Ubuntu

Raons principals per utilitzar ACL

1. Flexibilitat en gestió de permisos

    Superen les limitacions del model usuari/grup/altres

    Permeten assignar múltiples usuaris i grups al mateix recurs

    Ofereixen control granular d'accés

2. Escalabilitat en entorns complexos

    Necessàries en sistemes amb múltiples usuaris i grups

    Essencials en servidors compartits

    Importants en entorns corporatius

3. Seguretat més precisa

    Permeten implementar polítiques d'accés detallades

    Milloren el principi de mínim privilegi

    Faciliten l'auditoria d'accés

![alt text](ACL/1.png)

![alt text](ACL/2.png)

![alt text](ACL/3.png)

## Umask

Què és la umask?

Màscara que determina els permisos per defecte per a nous arxius i directoris.

On es configura a Ubuntu?

Arxius principals:

    Sistema: /etc/profile i /etc/bash.bashrc

    Usuari: ~/.bashrc

Comprovar umask actual:

**umask**

![alt text](<Gestió d'usuaris i grups i permisos img/17.png>)

Valors per defecte a Ubuntu

**Usuaris normals:**
    umask: 0002

    Arxius: 664 (rw-rw-r--)

    Directoris: 775 (rwxrwxr-x)

**Usuari root:**

    umask: 0022

    Arxius: 644 (rw-r--r--)

    Directoris: 755 (rwxr-xr-x)

![alt text](<Gestió d'usuaris i grups i permisos img/18.png>)

**Com funciona el càlcul?**

Permisos base:

    Arxius: 666 (rw-rw-rw-)

    Directoris: 777 (rwxrwxrwx)

Exemple umask 002:

Arxiu:   666 - 002 = 664 (rw-rw-r--)
Directori: 777 - 002 = 775 (rwxrwxr-x)

* Aqui he canviat temporalment el umask i he fet una provat creant una carpeta i un arxiu.

![alt text](<Gestió d'usuaris i grups i permisos img/19.png>)

* Per a fer-ho permanenment podem fer-ho modificant l'arxiu **login.defs**

![alt text](<Gestió d'usuaris i grups i permisos img/21.png>)

## Gestió de processos

Els processos són programes en execució dins del sistema. Cada procés té un PID (Identificador de Procés), un usuari propietari i pot trobar-se en diferents estats (actiu, en espera, aturat…). El sistema operatiu planifica i reparteix el temps de CPU entre ells.
Eines bàsiques per gestionar-los

    ps, top, htop: veure processos actius.

    kill, pkill: finalitzar un procés per PID o nom.

    nice, renice: ajustar la prioritat d'execució.

    systemctl, service: controlar serveis (daemons). No l'abordarem aquí específicament.

A nivell pràctic, cada procés hereta permisos de l'usuari que l'ha iniciat i pot estar vinculat a un servei o a una sessió d'usuari.

A continuació, veurem com utilitzar-les de manera bàsica.

**Ús de pstree**

```
Paràmetre	Funció
-p	Mostra el PID de cada procés.
-u	Mostra l'usuari propietari de cada procés.
-h	Ressalta el procés actual (útil quan es filtra).
-n	Ordena processos per PID dins de cada arbre.
-a	Mostra els arguments complets del procés (línia de comandes).
```

Per filtrar un procés, podem utilitzar grep en combinació amb altres eines.

Aquí he filtrat per els processos del usuari alumnat.

![alt text](Processos/10.png)

I aquí ho he fet com a root.

![alt text](Processos/11.png)

**ps** Aquesta comanda, mostra informació sobre una selecció dels processos actius. Si volem una actualització repetitiva de la selecció i la informació mostrada, hauriem de usar top en comptes d’això.

Alguns dels parametres mes comuns són:
```
a: mostra processos de tots els usuaris, no només del terminal actual.
u: mostra informació en format d’usuari, amb columnes com %CPU, %MEM, USER.
x: inclou processos sense terminal associat (daemons i serveis).
-e: Mostra tots els processos del sistema, equivalent a -A.
-o: Permet personalitzar exactament quines columnes vols que surti.
i molts més
```

![alt text](Processos/12.png)

Podem filtrar per obtenir les terminals que l’usuari fa servir amb ps aux | grep usuari | grep tty

Aixó, mostra els processos d’un usuari concret que s’estan executant en terminals.

```
ps aux: mostra tots els processos amb informació detallada.
grep usuario: filtra només els processos propietat de l’usuari “usuario”.
grep tty: filtra només els processos que tenen un terminal associat (tty).
```

![alt text](Processos/13.png)

Si volem matar un proces, podem fer servir kill, te diversos modes de terminar:

```
Tipus de Kill 	Senyal 	Descripció 	Comanda

Kill suau 	SIGTERM 	Demana al procés finalitzar netament 	kill PID
Kill forçat 	SIGKILL 	Mata immediatament, sense netejar recursos 	kill -9 PID
Recarregar config 	SIGHUP 	Demana al procés que recarregui la configuració 	kill -1 PID
Pausa 	SIGSTOP 	Pausa l’execució del procés 	kill -STOP PID
Continuar 	SIGCONT 	Continua un procés pausat 	kill -CONT PID
Interrupció Ctrl-C 	SIGINT 	Senyal d’interrupció (Ctrl+C) 	kill -2 PID
Abortar 	SIGABRT 	Senyal d’error abortat, sovint genera core dump 	kill -6 PID
```

Aqui tenim un exemple obrint xclock al fons amb el “&” i matant-lo suau, mentres comprovem amb ps aux que s’ha mort.

![alt text](Processos/14.png)

També tenim la comanda **top**

**top** és una comanda que mostra informació en temps real sobre processos i l'ús del sistema.

![alt text](Processos/4.png)

```
Part superior (resum del sistema):

    Temps: Temps d'execució del sistema

    Usuaris: Nombre d'usuaris connectats

    Load average: Càrrega mitjana (1, 5, 15 minuts)

    Tasques: Total, en execució, dormint, aturades, zombie

    %CPU: Ús del processador (us, sy, ni, id, wa, hi, si, st)

    Memòria: Total, lliure, usada, memòria buffer/cache

    Swap: Memòria d'intercanvi (swap) total i usada

Part inferior (llista de processos):

    PID: Identificador del procés

    USUARI: Propietari del procés

    PR: Prioritat

    NI: Valor "nice" (prioritat ajustable)

    VIRT: Memòria virtual utilitzada

    RES: Memòria resident (física)

    SHR: Memòria compartida

    %CPU: Percentatge d'ús de CPU

    %MEM: Percentatge d'ús de memòria

    TEMPS+: Temps total d'execució

    COMANDAMENT: Nom de la comanda
```

També tenim htop que es el mateix pero de manera interactiva.

![alt text](Processos/6.png)

Estats principals

Codi	Estat (Català)	Descripció
R	En execució (Running)	El procés està actiu o llest per ser assignat a la CPU
W	En espera (Waiting)	El procés espera un recurs o un esdeveniment
S	Aturat (Stopped)	El procés ha estat detingut, normalment per un senyal, sovint durant depuració
Z	Zombi (Zombie)	El procés ha finalitzat però encara conserva una entrada a la taula de processos
T	Trencat	Procés aturat per depuració o per senyal de trencament
D	Dormint	Procés inactiu, esperant I/O, no pot ser interromput
I	Inactiu (Idle)	El procés està completament inactiu, sense consumir CPU; molt habitual en fils del kernel

Ara amb la comanda renice podem modificar la prioritat de un procés

![alt text](Processos/15.png)

Mostra la llista de feines (processos) que tens en execució o aturades dins de la sessió actual del terminal.

Exemple de sortida:

[1]+  Aturat     nano fitxer.txt
[2]-  Executant  sleep 100 &


Això vol dir:

[1] i [2] són els números de feina

Aturat → el procés està pausat

Executant → el procés està funcionant en segon pla

🔹 fg %1

Serveix per portar una feina del segon pla o pausada al primer pla (foreground).

fg = foreground

%1 indica la feina número 1 (segons el que mostra jobs)

En aquest cas:

fg %1

Recupera la feina número 1 i la torna a executar ocupant el terminal.

Llencar processos amb &

Teoria copies de seguretat

teoria comandes backups

1. cp -> Es una copia simple no inteligent nomes transfereix fitxers localment es molt simple de utilitzar pero no optimitzar
2. rsync -> Es una eina inteligent que nomes copia els fitxers modificats i la sincronitzacio pot ser local o en remot via ssh
3. dd -> Es una eina per a clonar discs o particions i no es inteligent copia tots els sectors

3. Practica comandes backups

1. cp
2. rsync
3. dd

4. Practica programes backups

1. deja -up
2. Duplicity

5. Teoria automatitzacio scripts, cron i anacron
6. Practica automatitzacio

1. cron
2. anacron

Que es una copia
que es una imatge (iso)
que es una instantania

Tipus de copies

diferencial (descripcio)
completa (descripcio)
incremental (descripcio)

I que es necesita per recuperar (mini taula)

fdisk comprovar i formatar els 2 discos de 1 GB mkfs.ext4

Entrem a documents

mkdir prova, touch prova2

cd /var, mkdir copies, mount -t ext4 /dev/sdb1 /var/copies

cp -R /home/<user>/Documents/* /var/copies/

ls copies/

touch /home/<user>/Documents/hola
rm -r /home/<user>/Documents/prova

cp -R /home/<user>/Documents/* /var/copies/

ls copies/

mkdir /home/<user>/Documents/adeu
rm /home/<user>/Documents/hola

rsync -av --delete /home/<user>/Documents/ /var/copies/

ls copies/

mount -t ext4 /dev/sdb1 /var/copies

cd /var/

mkdir clonacio

mount -t ext4 /dev/sdc1 /var/clonacio

dd if=/dev/sdb1 of=/dev/sdc1 bs=1M status=progress

md5sum /dev/sdb1 /dev/sdc1

Han de sortir iguals captura

cron i anacron son 2 eines de automatitzacio que permeten executar tasques periodiques

cron executa tasques programades en una data i hora especifiques si el sistema esta apagat la tasca es perd es ideal per a tasques en dates i hores concretes i per accions especifiques d'un usuari.

anacron es ideal per executar tasques periodiques on no cal una hora i data especific normalment se utilitza per a tasques de manteniment del sistema i no requereix que el sistema estigui obert perque quan se obri ja l'executara

Programar cron anacron

## Gestió avançada



## PAM

## Còpies de seguretat i automatització de tasques

## Quotes d'usuari

Que es una quota?

edquota -u usuari -> veure quotes un usuari

setquota -u usuari -> establir quotes 1 usuari

repquota /dev/sdc1 -> informe quotes de tots els usuaris el que ocupen

quotaon /mnt/dades -> activar

quotaoff /mnt/dades -> desactivar

quotacheck -cug /mnt/dades -> crear arxius per a quotes usuari i grup si no estan per defecte