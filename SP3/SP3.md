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

## Configuració NFS

A continuació, configurarem el servidor NFS per compartir directoris a la xarxa. Instal·lem el paquet `nfs-kernel-server`.

![alt text](imatges/26.png)

Creem els directoris que volem compartir i els assignem els permisos adequats.

![alt text](imatges/27.png)

Editem el fitxer `/etc/exports` per definir quins directoris compartim i amb qui.

![alt text](imatges/28.png)

![alt text](imatges/29.png)

Reiniciem el servei NFS i comprovem que els directoris s'estan exportant correctament.

![alt text](imatges/30.png)

Verifiquem des del client o el mateix servidor que els muntatges funcionen.

![alt text](imatges/31.png)

![alt text](imatges/32.png)

![alt text](imatges/33.png)

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