---
layout: custom
title: "SPRINT 5: MONITORATGE, AUDITORIES I PROGRAMARI CLIENT SERVIDOR - Windows"
---

# Auditories de Seguretat amb Windows Server 2022

## Introducció

En un entorn empresarial, saber **qui ha accedit a quins recursos, quan i des d'on** és fonamental per garantir la seguretat dels sistemes. Les **auditories de seguretat** permeten als administradors de sistemes registrar i monitoritzar totes les activitats rellevants que es produeixen en un servidor o estació de treball: inicis de sessió, accessos a fitxers, canvis de configuració, creació i eliminació de comptes, etc.

Windows Server incorpora un sistema d'auditoria integrat basat en **polítiques de seguretat locals o de domini** que, quan s'activen, generen entrades al **Visor d'esdeveniments** (`eventvwr.msc`) sota el registre de **Seguretat**. Cada esdeveniment té un **Event ID** únic que identifica exactament quina acció s'ha produït.

---

## Teoria: El sistema d'auditoria de Windows

### Tipus d'auditories disponibles

| Directiva d'auditoria | Descripció | Event IDs principals |
|-----------------------|------------|----------------------|
| **Auditar eventos de inicio de sesión** | Registra tots els intents d'autenticació al sistema | 4624 (èxit), 4625 (fallada) |
| **Auditar el acceso a objetos** | Registra l'accés a fitxers, carpetes, claus de registre i altres objectes | 4663 (accés a objecte) |
| **Auditar el seguimiento de procesos** | Registra la creació i finalització de processos | 4688 (creat), 4689 (finalitzat) |
| **Auditar la administración de cuentas** | Registra operacions sobre comptes d'usuari | 4720 (creat), 4725 (deshabilitat), 4726 (eliminat) |
| **Auditar el cambio de directivas** | Registra canvis a les polítiques de seguretat | 4719 |
| **Auditar el uso de privilegios** | Registra l'ús de privilegis elevats | 4672 |

### Per què és important l'auditoria?

- **Detecció d'intrusions:** Identificar intents d'accés no autoritzats (Event 4625 repetits).
- **Compliment normatiu:** Moltes regulacions (ISO 27001, GDPR, PCI-DSS) exigeixen registres d'auditoria.
- **Investigació forense:** En cas d'incident de seguretat, els logs permeten reconstruir els fets.
- **Monitoratge intern:** Controlar l'accés a dades sensibles per part del personal intern.

### Com accedir al Visor d'esdeveniments

Per obrir el Visor d'esdeveniments: `Windows + R` → `eventvwr.msc` → **Registros de Windows** → **Seguridad**

Per filtrar per Event ID específic: clic dret sobre **Seguridad** → **Filtrar registro actual** → introduir el número d'event.

---

## Pràctica: Auditories pas a pas

### 1. Activar les auditories: Directiva de seguretat local

Obrim el diàleg d'execució amb **Windows + R** i escrivim `secpol.msc` per obrir la **Directiva de seguretat local**. Acceptem per executar-ho amb privilegis administratius.

![Windows + R → secpol.msc per obrir la Directiva de seguretat local](imatges-windows/1.png)

---

### 2. Activar l'auditoria d'inici de sessió

Dins de `secpol.msc`, naveguem a:
**Directivas locales → Directiva de auditoría → Auditar eventos de inicio de sesión**

Obrim les propietats i marquem tant **Correcto** com **Erróneo**. Això farà que Windows registri tots els inicis de sessió, tant els exitosos (4624) com els fallits (4625).

- ✅ **Correcto** → registra logins amb èxit (Event ID **4624**)
- ✅ **Erróneo** → registra intents fallits (Event ID **4625**)

![secpol.msc → Auditar eventos de inicio de sesión → Correcto i Erróneo activats](imatges-windows/2.png)

---

### 3. Comprovar l'Event ID 4624: inici de sessió correcte

Obrim el **Visor d'esdeveniments** (`eventvwr.msc`) i naveguem a **Registros de Windows → Seguridad**. Filtrem per l'Event ID **4624** i veiem totes les entrades d'inici de sessió correctes. Seleccionem-ne una i al panell inferior veiem el detall:

- **"Se inició sesión correctamente en una cuenta"**
- Compte: `WIN-0TEAPO54328$` del domini `EROS.CAT`
- Tipus d'inici de sessió: `3` (xarxa)

Això confirma que l'auditoria d'inici de sessió funciona correctament.

![Visor d'esdeveniments → Seguridad → Event 4624 - Inici de sessió correcte](imatges-windows/3.png)

---

### 4. Activar l'auditoria d'accés a objectes

Tornem a `secpol.msc` i activem **Auditar el acceso a objetos** marcant **Correcto** i **Erróneo**. Aquesta directiva és necessària per registrar accessos a fitxers i carpetes específiques, però **no és suficient per ella sola**: cal configurar l'auditoria també a nivell de cada carpeta o fitxer.

![secpol.msc → Auditar el acceso a objetos → Correcto i Erróneo activats](imatges-windows/4.png)

---

### 5. Crear la carpeta d'auditoria

Creem una carpeta nova a l'arrel del disc C: anomenada `ProvaAuditoria`. Serà la carpeta sobre la qual configurarem l'auditoria i sobre la qual farem proves d'accés, lectura i modificació de fitxers.

![Carpeta C:\ProvaAuditoria creada a l'Explorador d'arxius](imatges-windows/5.png)

---

### 6. Configurar l'auditoria sobre la carpeta

Fem clic dret sobre `ProvaAuditoria` → **Propiedades → Seguridad → Opciones avanzadas → Auditoría → Agregar**.

Afegim l'usuari **Administrador** (`ASTRO\Administrador`) amb tipus d'accés **Control total** perquè quedi registrada qualsevol acció que l'Administrador faci sobre la carpeta. La configuració es veu reflectida a la pestanya **Auditoría** de la finestra de seguretat avançada.

![Configuració de seguretat avançada - Auditoría → Administrador amb Control total](imatges-windows/6.png)

---

### 7. Generar accions dins la carpeta

Creem un fitxer de text `hola.txt` dins de `C:\ProvaAuditoria`. Després l'obrim, el modifiquem i l'eliminem. Cada una d'aquestes accions genera un Event ID 4663 al Visor d'esdeveniments.

![Fitxer hola.txt creat dins de C:\ProvaAuditoria](imatges-windows/7.png)

---

### 8. Filtrar per Event ID 4663

Al Visor d'esdeveniments, fem clic dret sobre **Seguridad** → **Filtrar registro actual** i introduïm el codi **4663** per mostrar únicament els events d'accés a objectes.

![Filtrar el registre de Seguridad per l'Event ID 4663](imatges-windows/8.png)

---

### 9. Comprovar l'Event ID 4663: accés a objecte

Veiem múltiples entrades amb Event ID **4663** (categoria: File System) generades al moment que vam crear i manipular el fitxer. El detall de l'event mostra:

- **"Se intentó tener acceso a un objeto"**
- Firmant: `ASTRO\Administrador`
- Objecte: `C:\ProvaAuditoria` (tipus File)
- Nom del procés: `C:\Windows\explorer.exe`

Això confirma que Windows ha registrat correctament l'accés a la carpeta auditada.

![Event ID 4663 - Accés a l'objecte C:\ProvaAuditoria per l'Administrador](imatges-windows/9.png)

---

### 10. Activar l'auditoria de seguiment de processos

Tornem a `secpol.msc` i activem **Auditar el seguimiento de procesos** amb **Correcto** i **Erróneo**. Amb aquesta directiva, Windows registrarà cada vegada que s'iniciï o finalitzi un procés en el sistema.

![secpol.msc → Auditar el seguimiento de procesos → Correcto i Erróneo activats](imatges-windows/10.png)

---

### 11. Obrir el Bloc de notes per generar un Event 4688

Per provar l'auditoria de processos, cerquem **notepad** al menú d'inici i l'obrim. Aquesta acció ha de generar l'Event ID **4688** (procés creat) al registre de Seguretat.

![Cerca de notepad al menú d'inici per generar un Event 4688](imatges-windows/11.png)

---

### 12. Filtrar per Event ID 4688

Al Visor d'esdeveniments, filtrem per l'Event ID **4688** per veure tots els processos creats des que es va activar l'auditoria.

![Filtrar el registre de Seguridad per l'Event ID 4688](imatges-windows/12.png)

---

### 13. Comprovar l'Event ID 4688: procés creat

L'Event **4688** registrat mostra:

- **"Se creó un nuevo proceso"**
- Firmant creador: `ASTRO\Administrador`
- **Nom del nou procés: `C:\Windows\System32\notepad.exe`**
- Tipus d'elevació de token: `TokenElevationTypeDefault (1)`
- Registrat a les `21:02:37`

Queda documentat exactament qui ha iniciat el procés, quan i quin executable s'ha executat.

![Event ID 4688 - Procés creat: notepad.exe per l'Administrador](imatges-windows/13.png)

---

### 14. Filtrar per Event ID 4689

Tanquem el Bloc de notes. Per comprovar que s'ha registrat la finalització, filtrem per l'Event ID **4689** (procés finalitzat).

![Filtrar el registre de Seguridad per l'Event ID 4689](imatges-windows/14.png)

---

### 15. Comprovar l'Event ID 4689: procés finalitzat

L'Event **4689** confirma:

- **"Se salió de un proceso"**
- Firmant: `ASTRO\Administrador`
- **Nom del procés: `C:\Windows\System32\notepad.exe`**
- ID del procés: `0x8e0` (el mateix que quan es va crear)
- **Estat de sortida: `0x0`** (finalització correcta)

Amb els events 4688 i 4689 podem saber exactament quins programes s'han executat al servidor i durant quant de temps.

![Event ID 4689 - Procés finalitzat: notepad.exe](imatges-windows/15.png)

---

### 16. Activar l'auditoria d'administració de comptes

Tornem a `secpol.msc` i activem **Auditar la administración de cuentas** amb **Correcto** i **Erróneo**. Aquesta directiva registra totes les operacions sobre comptes d'usuari: creació, activació, deshabilitació i eliminació.

![secpol.msc → Auditar la administración de cuentas → Correcto i Erróneo activats](imatges-windows/16.png)

---

### 17. Obrir la gestió d'usuaris i equips d'AD

Premem **Windows + R** i escrivim `lusrmgr.msc` per accedir a la consola de gestió de comptes del domini **Usuarios y equipos de Active Directory**.

![Windows + R → lusrmgr.msc per gestionar usuaris](imatges-windows/17.png)

---

### 18. Crear un nou usuari de prova

A la consola d'AD, fem clic dret sobre el contenidor **Users** i seleccionem **Nuevo → Usuario**. Creem un usuari de prova amb el nom `testt`, que serà el compte que utilitzarem per provar la generació d'events d'administració de comptes.

![Crear nou usuari: Nuevo → Usuario a la consola d'AD](imatges-windows/18.png)

---

### 19. Dades del nou usuari testt

Omplim les dades del nou usuari:

- **Nombre de pila:** `testt`
- **Nombre de inicio de sesión:** `testt@eros.cat`
- **Nom anterior a Windows 2000:** `ASTRO\testt`

Fem clic a **Siguiente** per continuar i finalitzem la creació.

![Dades del nou usuari testt@eros.cat](imatges-windows/19.png)

---

### 20. Filtrar per Event ID 4720

Al Visor d'esdeveniments, filtrem per l'Event ID **4720** per verificar que la creació de l'usuari s'ha registrat.

![Filtrar el registre de Seguridad per l'Event ID 4720](imatges-windows/20.png)

---

### 21. Comprovar l'Event ID 4720: usuari creat

L'Event **4720** confirma:

- **"Se creó una cuenta de usuario"**
- Firmant (qui l'ha creat): `ASTRO\Administrador`
- **Nova compte: `ASTRO\testt`**
- Nom de compte SAM: `testt`
- Principal d'usuari: `testt@eros.cat`
- Categoria de tasca: **User Account Management**

Queda registrat qui va crear el compte, quan i amb quins atributs.

![Event ID 4720 - Usuari testt creat per l'Administrador](imatges-windows/21.png)

---

### 22. Deshabilitar el compte d'usuari

Tornem a la consola d'AD. Fem clic dret sobre l'usuari **testt** i seleccionem **Deshabilitar cuenta** per simular la desactivació temporal d'un compte.

![Clic dret sobre testt → Deshabilitar cuenta](imatges-windows/22.png)

---

### 23. Confirmació de deshabilitació

AD mostra el missatge de confirmació: **"El objeto testt ha sido deshabilitado"**. Acceptem.

![Confirmació: El objeto testt ha sido deshabilitado](imatges-windows/23.png)

---

### 24. Filtrar per Event ID 4725

Al Visor d'esdeveniments, filtrem per l'Event ID **4725** per verificar que la deshabilitació s'ha registrat.

![Filtrar el registre de Seguridad per l'Event ID 4725](imatges-windows/24.png)

---

### 25. Comprovar l'Event ID 4725: usuari deshabilitat

L'Event **4725** mostra:

- **"Se deshabilitó una cuenta de usuario"**
- Firmant: `ASTRO\Administrador`
- **Compte de destí: `ASTRO\testt`**
- Registrat a les `21:27:10`
- Categoria: **User Account Management**

El registre permet saber exactament quan i qui va deshabilitar el compte.

![Event ID 4725 - Usuari testt deshabilitat per l'Administrador](imatges-windows/25.png)

---

### 26. Eliminar el compte d'usuari

Tornem a la consola d'AD. Fem clic dret sobre l'usuari **testt** i seleccionem **Eliminar** per suprimir definitivament el compte del domini.

![Clic dret sobre testt → Eliminar per suprimir el compte](imatges-windows/26.png)

---

### 27. Filtrar per Event ID 4726

Al Visor d'esdeveniments, filtrem per l'Event ID **4726** per verificar que l'eliminació del compte ha quedat registrada.

![Filtrar el registre de Seguridad per l'Event ID 4726](imatges-windows/27.png)

---

### 28. Comprovar l'Event ID 4726: usuari eliminat

L'Event **4726** confirma:

- **"Se eliminó una cuenta de usuario"**
- Firmant: `ASTRO\Administrador`
- **Compte eliminat: `ASTRO\testt`**
- Registrat a les `21:27:58`
- Categoria: **User Account Management**

Amb aquest darrer event, el cicle de vida complet del compte `testt` queda documentat al registre de seguretat: creació (4720) → deshabilitació (4725) → eliminació (4726).

![Event ID 4726 - Usuari testt eliminat per l'Administrador](imatges-windows/28.png)

---

## Resum dels Event IDs treballats

| Event ID | Categoria | Descripció | Activat per... |
|----------|-----------|------------|----------------|
| **4624** | Logon | Inici de sessió correcte | Auditar eventos de inicio de sesión |
| **4625** | Logon | Inici de sessió fallit | Auditar eventos de inicio de sesión |
| **4663** | File System | Accés a un objecte (fitxer/carpeta) | Auditar el acceso a objetos + config. carpeta |
| **4688** | Process Creation | Nou procés creat | Auditar el seguimiento de procesos |
| **4689** | Process Termination | Procés finalitzat | Auditar el seguimiento de procesos |
| **4720** | User Account Management | Compte d'usuari creat | Auditar la administración de cuentas |
| **4722** | User Account Management | Compte d'usuari activat | Auditar la administración de cuentas |
| **4725** | User Account Management | Compte d'usuari deshabilitat | Auditar la administración de cuentas |
| **4726** | User Account Management | Compte d'usuari eliminat | Auditar la administración de cuentas |

---

## Conclusions i Observacions

### Conclusions generals

- **L'auditoria de Windows és una eina potent i granular:** Permet registrar amb precisió qui fa cada acció, quan i des d'on, sense necessitat de programari de tercers.

- **La doble configuració és necessària per a objectes:** Per registrar accés a fitxers i carpetes (Event 4663), cal activar la política a `secpol.msc` **i** configurar l'auditoria a les propietats de la carpeta concreta. Sense les dues parts, l'auditoria no funciona.

- **El Visor d'esdeveniments és la finestra a la seguretat del sistema:** Centralitza tots els registres i permet filtrar per Event ID, data, usuari o equip per trobar ràpidament la informació rellevant.

- **El seguiment de processos pot generar molt de volum:** Activar l'auditoria de processos en un servidor en producció pot omplir ràpidament el registre de seguretat. Cal calibrar bé quines auditories s'activen i configurar una mida adequada per al registre.

### Observacions tècniques

- **L'Event ID 4624 es genera molt freqüentment:** Inclou autenticacions del sistema operatiu, serveis i comptes de màquina. Per trobar inicis de sessió d'usuaris humans, cal filtrar per tipus d'inici de sessió `2` (interactiu) o `10` (remot).

- **Els logs no duren per sempre:** Per defecte, Windows sobreescriu el registre quan es plena. En entorns professionals s'han d'enviar els logs a un **SIEM** (*Security Information and Event Management*) per emmagatzemar-los i analitzar-los centralment.

- **L'auditoria d'AD és especialment crítica:** Qualsevol creació, modificació o eliminació de comptes al domini queda registrada, la qual cosa permet detectar comportaments anòmals com la creació de comptes no autoritzats.

- **L'auditoria és una capa de visibilitat, no una capa de protecció:** Cal combinar-la amb còpies de seguretat, polítiques d'accés mínims i plans de resposta a incidents per tenir una postura de seguretat completa.

---

# Monitorització del sistema amb Windows Server 2022

## Introducció a la monitorització

La **monitorització del sistema** consisteix a observar en temps real el comportament dels recursos del servidor: CPU, memòria RAM, disc i xarxa. A diferència de les auditories (que registren *qui* ha fet *què*), la monitorització ens diu *com* es troba el sistema en cada moment i permet detectar problemes de rendiment, colls d'ampolla i processos que consumeixen massa recursos.

Windows Server ofereix dues eines principals per a aquesta tasca:

| Eina | Accés | Nivell de detall |
|------|-------|-----------------|
| **Administrador de tasques** | `Ctrl + Shift + Esc` | Visió general ràpida |
| **Monitor de recursos** | `resmon` o des de l'Administrador de tasques | Anàlisi detallada per procés |

---

## Pràctica: Monitorització pas a pas

### Pas 1. Obrir l'Administrador de tasques i revisar els processos

Premem **Ctrl + Shift + Esc** per obrir l'**Administrador de tasques**. A la pestanya **Procesos** podem veure tots els processos actius del sistema agrupats per categoria:

- **Aplicaciones (2):** `Administrador de tareas` (17,4 MB) i `Server Manager` (62,6 MB)
- **Procesos en segundo plano (36):** serveis del sistema operatiu com `AggregatorHost.exe`, `AntiMalware Definition Update`, `Experiencia de entrada de Windows`, etc.

En el moment de la captura, el sistema presenta un **ús de CPU del 65%** i un **consum de memòria del 42%**. Això indica que el servidor té una càrrega moderada-alta. El procés `Server Manager` és el que consumeix més memòria entre les aplicacions obertes (62,6 MB).

![Administrador de tasques - Pestanya Processos amb CPU 65% i Memòria 42%](monitoritzaciowindows/1.png)

---

### Pas 2. Monitoritzar la CPU

Anem a la pestanya **Rendimiento → CPU**. El gràfic mostra l'evolució de l'ús del processador en els darrers 60 segons.

Dades observades:
- **Processador:** Intel(R) Core(TM) i7-10700 @ 2,90 GHz
- **Ús actual:** 9%
- **Velocitat:** 2,90 GHz
- **Processos actius:** 119
- **Subprocessos (fils):** 1.398
- **Identificadors:** 51.610
- **Temps d'activitat:** 0:00:03:23
- **Processadors virtuals:** 2 (màquina virtual: Sí)

La CPU mostra el percentatge d'ús del processador. El gràfic amb pics indica que el sistema ha tingut moments de càrrega alta però en el moment actual es troba en un ús baix (9%). Si un procés utilitza molta CPU durant molt de temps, pot indicar una sobrecàrrega o un problema en el sistema.

![Administrador de tasques - Rendiment CPU: Intel i7-10700, 9% ús, 119 processos](monitoritzaciowindows/2.png)

---

### Pas 3. Monitoritzar la memòria RAM

Anem a **Rendimiento → Memoria**. El gràfic mostra l'ús de la memòria en el temps.

Dades observades:
- **Memòria total instal·lada:** 4,0 GB
- **En ús (comprimida):** 1,6 GB (40%)
- **Disponible:** 2,4 GB
- **Confirmada:** 1,6 / 5,4 GB
- **En caché:** 1,9 GB
- **Bloque paginado:** 120 MB
- **Bloque no paginado:** 97,4 MB

La memòria RAM indica quanta memòria està utilitzant el sistema i quanta queda disponible. En aquest cas, el 40% d'ús (1,6 GB de 4 GB) és un valor acceptable. La memòria en caché (1,9 GB) indica que Windows reserva una bona part per accelerar l'accés a dades freqüentment usades. Si la memòria disponible fos molt baixa, el sistema hauria de recórrer al fitxer de paginació al disc, cosa que reduiria significativament el rendiment.

![Administrador de tasques - Rendiment Memòria: 1,6/4,0 GB en ús (40%), 2,4 GB disponibles](monitoritzaciowindows/3.png)

---

### Pas 4. Monitoritzar la xarxa

Anem a **Rendimiento → Ethernet**. El gràfic mostra el rendiment de la connexió de xarxa en temps real.

Dades observades:
- **Adaptador:** Intel(R) PRO/1000 MT Desktop Adapter
- **Enviament:** 80,0 Kbps
- **Recepció:** 9,2 Mbps
- **Velocitat màxima:** 11 Mbps
- **Direcció IPv4:** 10.0.2.17
- **Direcció IPv6:** fe80::81c9:f99a:5180:5eea%9
- **Tipus de connexió:** Ethernet

La xarxa mostra les dades enviades i rebudes pel servidor. S'observa que la recepció (9,2 Mbps) és molt superior a l'enviament (80 Kbps), cosa típica en un servidor que descarrega actualitzacions o rep dades de clients. El pic en el gràfic indica un moment de trànsit intens. Amb aquesta informació es pot detectar si alguna aplicació fa un ús excessiu de la connexió.

![Administrador de tasques - Rendiment Ethernet: E: 80 Kbps, R: 9,2 Mbps, IP 10.0.2.17](monitoritzaciowindows/5.png)

---

### Pas 5. Obrir el Monitor de recursos

Des de l'Administrador de tasques, fem clic a **Abrir el Monitor de recursos** (a la part inferior de la pestanya Rendimiento), o premem **Windows + R** i escrivim `resmon`. El Monitor de recursos ofereix una visió molt més detallada que l'Administrador de tasques, amb informació per procés, PID, serveis associats i activitat de disc i xarxa.

---

### Pas 6. Revisar la CPU al Monitor de recursos

Dins del **Monitor de recursos**, anem a la pestanya **CPU**. Veiem dues seccions:

**Processos** (Ús de CPU: 35%):
- `svchost.exe (netsvcs -p)` — PID 2664, 23 subprocessos, **9,63% CPU**
- `perfmon.exe` — PID 5816, monitor de rendiment actiu, 7,28% CPU
- `svchost.exe (LocalServiceNo...)` — PID 1264, 1,06% CPU
- `SearchApp.exe` i `ShellExperienceHost.exe` — suspesos (0% CPU)

**Serveis associats** (Ús de CPU: 23%):
- `wuauserv` (Windows Update) — PID 2664, **9,63% CPU** — és el servei que més CPU consumeix
- `mpssvc` (Firewall de Windows) — PID 1264, 1,06% CPU

El Monitor de recursos permet veure quins serveis concrets estan associats a cada procés `svchost.exe`, cosa que no és possible amb l'Administrador de tasques. En aquest cas, queda clar que **Windows Update** és el responsable del consum elevat de CPU.

![Monitor de recursos - CPU: svchost/wuauserv al 9,63% CPU, 35% ús total](monitoritzaciowindows/8.png)

---

### Pas 7. Revisar la memòria al Monitor de recursos

Anem a la pestanya **Memoria** del Monitor de recursos.

**Processos per consum de RAM (Privada):**
- `MsMpEng.exe` (Windows Defender) — 270.448 KB assignats, **161.508 KB privada**
- `dns.exe` (servei DNS) — 130.892 KB assignats, 120.676 KB privada
- `lsass.exe` (autenticació) — 56.672 KB assignats, 32.840 KB privada
- `dwm.exe` (gestor finestres) — 35.420 KB assignats, 31.460 KB privada

**Resum de memòria física:**
- **Total instal·lada:** 4.076 MB
- **En ús:** 1.526 MB
- **Disponible:** 2.483 MB
- **En caché:** 2.135 MB
- **En espera:** 2.068 MB | **Lliure:** 415 MB

El procés que més memòria privada consumeix és `MsMpEng.exe` (Windows Defender), amb 161 MB. Això és normal en un servidor amb protecció antivirus activa. Els 2.483 MB disponibles indiquen que el servidor té marge de memòria suficient per a les tasques actuals.

![Monitor de recursos - Memòria: 37% usada, 1526 MB en ús, 2483 MB disponibles](monitoritzaciowindows/9.png)

---

### Pas 8. Revisar el disc al Monitor de recursos

Anem a la pestanya **Disco** del Monitor de recursos.

**Processos amb activitat de disc:**
- `System` — PID 4, Lectura: **41.062 B/s**, Escriptura: **898.662 B/s**, Total: 939.725 B/s
- `perfmon.exe` — PID 5816, Lectura: 185.958 B/s (monitorització activa)
- `svchost.exe (LocalSystemNet...)` — PID 3880, Lectura: 106.496 B/s
- `fontdrvhost.exe` — 100.864 B/s de lectura

**Activitat de disc global:** 139.264 B/s | 5% de temps actiu

**Emmagatzematge:**
- Disc lògic `C:` — Temps de resposta: **4,72 ms** | Espai usat: 68,1 GB | Espai total: 81,2 GB

L'apartat de disc permet veure si algun procés està llegint o escrivint moltes dades. En aquest cas, el procés `System` és el que més escriu (898 KB/s), probablement per operacions del sistema de fitxers i la memòria virtual. El temps de resposta de 4,72 ms és acceptable per a un disc virtual.

![Monitor de recursos - Disco: System 939 KB/s, perfmon 185 KB/s, disc C 81 GB total](monitoritzaciowindows/4.png)

---

### Pas 9. Revisar la xarxa al Monitor de recursos

Anem a la pestanya **Red** del Monitor de recursos. Aquesta és la vista més completa de l'activitat de xarxa.

**Processos amb activitat de xarxa:**
- `svchost.exe (NetworkService...)` — PID 5760, Recepció: **1.103.428 B/s** — màxim consum
- `lsass.exe` — PID 676, 749 B/s enviat, 2.449 B/s rebut (autenticació AD)
- `Microsoft.ActiveDirectory.W...` — PID 2980, trànsit bidireccional
- `dns.exe` — PID 3044, 7 B/s enviat, 9 B/s rebut

**Activitat de xarxa global:** E/S de xarxa: 8 Mbps | 0% de percentatge d'ús

**Connexions TCP actives:**
- `svchost.exe` (PID 5760) — múltiples connexions cap a `193.5.x.x:80` (actualitzacions de Windows)

La pestanya Red permet detectar quins processos estan fent ús de la xarxa i cap a quines IPs es connecten. El `svchost.exe` amb 1,1 MB/s de recepció correspon al servei de Windows Update descarregant actualitzacions. Les connexions cap al port 80 de servidors Microsoft ho confirmen.

![Monitor de recursos - Red: svchost 1.1 MB/s, connexions TCP actives cap a servidors Microsoft](monitoritzaciowindows/6.png)

---

### Pas 10. Ports d'escolta al servidor

A la secció inferior de la pestanya **Red** del Monitor de recursos, veiem els **Puertos de escucha**, que indiquen quins ports té oberts el servidor esperant connexions entrants:

- `dns.exe` (PID 3044) — Port **53** TCP i UDP per IPv4 i IPv6 (servei DNS)
- `lsass.exe` (PID 676) — Port **88** TCP (Kerberos, autenticació del domini)

Aquests ports oberts corresponen als serveis normals d'un controlador de domini Active Directory. El port 53 és el DNS i el port 88 és Kerberos. En un anàlisi de seguretat, aquesta llista permet detectar ports oberts inesperats que podrien indicar programari maliciós o configuració incorrecta.

![Monitor de recursos - Puertos de escucha: dns.exe port 53, lsass port 88](monitoritzaciowindows/7.png)

---

## Conclusions de la monitorització

Amb aquesta pràctica hem après a utilitzar l'**Administrador de tasques** i el **Monitor de recursos** de Windows per analitzar el consum de CPU, memòria RAM, disc i xarxa. Aquestes eines són imprescindibles per a qualsevol administrador de sistemes.

### Resum de l'estat del sistema observat

| Recurs | Eina | Valor observat | Valoració |
|--------|------|----------------|-----------|
| CPU | Administrador de tasques | 9% (Intel i7-10700) | ✅ Baix |
| Memòria RAM | Administrador de tasques | 1,6 / 4,0 GB (40%) | ✅ Acceptable |
| Xarxa (Ethernet) | Administrador de tasques | R: 9,2 Mbps / E: 80 Kbps | ⚠️ Descàrrega activa |
| CPU (detall) | Monitor de recursos | 35%, wuauserv 9,63% | ⚠️ Windows Update actiu |
| Memòria (detall) | Monitor de recursos | 37%, MsMpEng 161 MB | ✅ Acceptable |
| Disc | Monitor de recursos | System 939 KB/s, 5% actiu | ✅ Normal |
| Xarxa (detall) | Monitor de recursos | svchost 1,1 MB/s rebut | ⚠️ Actualitzacions |
| Ports oberts | Monitor de recursos | 53 (DNS), 88 (Kerberos) | ✅ Normals per AD |

### Observacions clau

- **L'Administrador de tasques ofereix una visió ràpida** però no permet identificar quin servei concret dins de `svchost.exe` és el responsable del consum. Per això cal el Monitor de recursos.

- **Windows Update** (`wuauserv`) era la causa principal del consum de CPU i xarxa en el moment de la monitorització: descarregava i instal·lava actualitzacions, consumint CPU (9,63%) i ample de banda (9,2 Mbps de recepció).

- **Windows Defender** (`MsMpEng.exe`) és el procés que més memòria privada consumeix (161 MB), cosa completament normal en un servidor amb protecció antivirus.

- **Els ports oberts** (53 i 88) corresponen exactament als serveis esperats d'un controlador de domini Active Directory, sense ports inesperats que puguin indicar amenaces.

- **La monitorització és proactiva, l'auditoria és reactiva:** El Monitor de recursos ens diu l'estat actual del sistema; el Visor d'esdeveniments ens diu el que ha passat. Ambdues eines juntes proporcionen una visió completa de la salut i la seguretat del servidor.
