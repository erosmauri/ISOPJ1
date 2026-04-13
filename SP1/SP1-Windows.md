---
layout: custom
title: "Sprint 1: Instal·lació i Configuració Inicial – Windows"
---

Crear màquina virtual per instal·lar un Windows 11 Pro i dur a terme la configuració inicial del sistema.

---

# Fase 1 – Instal·lació del sistema operatiu

## Pas 1 – Crear la màquina virtual amb VirtualBox

* Aquí inicio la creació de la nova màquina virtual a VirtualBox. Anomeno la màquina **Windows 11 Pro**, selecciono el tipus **Microsoft Windows** i la versió **Windows 11 (64-bit)**. En aquest punt la ISO encara no hi és seleccionada, la carregaré al pas 3.

![Pas 1 – Configuració inicial de la VM](imatges-windows/1.png)

---

## Pas 2 – Assignar recursos (RAM mínima 4 GB, disc mínim 40 GB)

* Aquí assigno els recursos de maquinari a la màquina virtual:

    1. **Memòria base: 4096 MB (4 GB)** – el mínim recomanat per a Windows 11.
    2. **Processadors: 4 CPUs** – per garantir un rendiment fluid.
    3. **EFI habilitat** – Windows 11 requereix arrencada EFI (UEFI).

![Pas 2 – Hardware: 4 GB RAM i 4 CPUs amb EFI](imatges-windows/2.png)

---

## Pas 3 – Carregar la ISO de Windows 11

* En aquest pas assigno **40 GB** de disc virtual a la màquina i selecciono la ISO de **Windows 11 25H2 (Spanish x64)** com a unitat òptica. Queda tot llest per arrencar la instal·lació.

![Pas 3 – Disc virtual de 40 GB i ISO de Windows 11 carregada](imatges-windows/3.png)

* A continuació puc veure el **resum final** de la VM abans de crear-la. Confirmo que tot és correcte:
    - **Nom:** Windows 11 Pro
    - **Memòria:** 4096 MB | **Processadors:** 4 | **EFI:** activat
    - **Disc:** 40 GB
    - **ISO:** Win11_25H2_Spanish_x64.iso

![Pas 3 – Resum final de la configuració de la VM](imatges-windows/4.png)

---

## Pas 4 – Instal·lar el sistema (idioma, usuari, contrasenya)

* Un cop arrencada la VM des de la ISO, apareix la pantalla de configuració d'idioma de Windows 11. Selecciono **Español (España, internacional)** com a idioma d'instal·lació i format d'hora.

![Pas 4a – Selecció d'idioma d'instal·lació](imatges-windows/7.png)

* Selecciono **Instal·lar Windows 11** i marco la casella per acceptar que tot el contingut del disc serà eliminat.

![Pas 4b – Opció d'instal·lació nova](imatges-windows/8.png)

* Selecciono l'edició **Windows 11 Pro** de les disponibles a la ISO.

![Pas 4c – Selecció d'edició Windows 11 Pro](imatges-windows/9.png)

* Selecciono el disc de **40 GB** (sense assignar) com a ubicació on instal·lar Windows 11.

![Pas 4d – Selecció del disc de destí (40 GB)](imatges-windows/10.png)

* Apareix la pantalla de resum **"Listo para instalar"**. Confirmo que instal·larà **Windows 11 Pro** i que no es mantindrà cap arxiu. Clico **Instal·lar**.

![Pas 4e – Confirmació i inici de la instal·lació](imatges-windows/11.png)

* Comença la instal·lació de Windows 11. La barra de progrés avança mentre es copien els arxius al disc.

![Pas 4f – Instal·lació en curs (5% completat)](imatges-windows/12.png)

* Un cop instal·lat, Windows arrenca el procés de configuració inicial (OOBE). Selecciono **España** com a país o regió.

![Pas 4g – Selecció de país: España](imatges-windows/13.png)

* Introdueixo el nom del dispositiu: **Astro**.

![Pas 4h – Nom del dispositiu: Astro](imatges-windows/14.png)

* Creo el compte d'usuari local: nom **Eros**.

![Pas 4i – Creació de l'usuari local: Eros](imatges-windows/15.png)

* Estableixo la contrasenya per a l'usuari **Eros**: `eros1234`.

![Pas 4j – Creació de contrasenya de l'usuari](imatges-windows/16.png)

---

## Pas 5 – Comprovar que arrenca correctament

* Windows 11 arrenca correctament i apareix l'escriptori. Es pot veure la barra de tasques, la icona de la Paperera de reciclatge i l'accés directe a Microsoft Edge. La instal·lació ha estat exitosa.

![Pas 5 – Windows 11 arrencat correctament](imatges-windows/17.png)

---

# Fase 2 – Punts de restauració

## Pas 6 – Cercar "Crear un punt de restauració"

* Al cercador de Windows busco **"Crear un punto de restauración"** i clico sobre el resultat del Panel de control per obrir-lo.

![Pas 6 – Cerca de "Crear un punto de restauración"](imatges-windows/18.png)

---

## Pas 7 – Activar la protecció del sistema al disc C:

* A la finestra de **Propietats del sistema > Protecció del sistema**, veig que la protecció del disc **C:** apareix com a **Desactivada**. Clico **Configurar...** per activar-la.

![Pas 7a – Protecció desactivada: cal clicar Configurar](imatges-windows/19.png)

* A la finestra de configuració selecciono **Activar protecció del sistema** ①, aplico ② i accepto ③. Ara el disc C: ja té protecció activa.

![Pas 7b – Activar protecció del sistema i aplicar](imatges-windows/20.png)

* Comprovo que la protecció del disc **C: (Sistema)** ara apareix com a **Activada**.

![Pas 7c – Protecció del sistema activada correctament](imatges-windows/21.png)

---

## Pas 8 – Crear un punt de restauració manual

* Clico **Crear...** per crear un nou punt de restauració manualment.

![Pas 8a – Botó Crear un punt de restauració](imatges-windows/22.png)

* Introdueixo la descripció **"Eros - 12/04/2026: 15:53"** per identificar el punt. Windows afegirà automàticament la data i hora exactes.

![Pas 8b – Descripció del punt de restauració](imatges-windows/23.png)

---

## Pas 9 – Fer un canvi (crear un arxiu de prova)

* Com a canvi de sistema, creo un arxiu anomenat **PROVA** a l'escriptori. Amb la protecció activa al disc C:, Windows pot revertir aquest canvi si es fa una restauració.

![Pas 9 – Arxiu PROVA creat a l'escriptori com a canvi de prova](imatges-windows/24.png)

---

## Pas 10 – Restaurar i comprovar

* Clico **Restaurar sistema...** per iniciar el procés de restauració al punt creat prèviament.

![Pas 10a – Botó Restaurar sistema](imatges-windows/25.png)

* Selecciono el punt de restauració **"Eros - 12/04/2026: 15:53"** creat manualment (tipus: Manual) i clico Següent per continuar.

![Pas 10b – Selecció del punt de restauració a aplicar](imatges-windows/26.png)

* Apareix un avís confirmant que el procés no es pot interrompre un cop iniciat. Clico **Sí** per confirmar la restauració.

![Pas 10c – Confirmació per iniciar la restauració](imatges-windows/27.png)

* Windows reinicia i comença a restaurar els arxius i la configuració. Es pot veure la pantalla de "Restaurar sistema se está inicializando...".

![Pas 10d – Restauració del sistema en curs](imatges-windows/28.png)

* Un cop acabada la restauració, Windows torna a l'escriptori en l'estat anterior. L'arxiu PROVA ha desaparegut, confirmant que la restauració ha funcionat correctament.

![Pas 10e – Escriptori restaurat: l'arxiu PROVA ja no hi és](imatges-windows/29.png)

---

# Fase 3 – Llicències de Windows

## Pas 11 – Obrir Configuració → Sistema → Activació

* Obro **Configuració** i vaig a **Sistema** ① → **Activació** ② per consultar l'estat de la llicència de Windows.

![Pas 11 – Configuració > Sistema > Activació](imatges-windows/30.png)

---

## Pas 12 – Veure si Windows està activat

* La pantalla d'activació mostra que **Windows 11 Pro** té l'estat **"No está activo"**. L'error `0xC004F213` indica que no s'ha trobat cap clau de producte al dispositiu. Això és normal en una instal·lació de prova sense llicència comprada.

![Pas 12 – Windows 11 Pro no activat (codi error 0xC004F213)](imatges-windows/31.png)

---

## Pas 13 – Executar al CMD: `slmgr /xpr`

* Obro el symbol del sistema i executo la comanda `slmgr /xpr`. Apareix una finestra de **Windows Script Host** que indica: *"Windows(R), Professional edition: Windows está en modo de notificación"*. Això confirma que Windows funciona però sense llicència activa.

![Pas 13 – Resultat de slmgr /xpr: mode de notificació](imatges-windows/32.png)

---

## Pas 14 – Tipus de llicenciament de Windows

> **Tipus de llicències de Windows més habituals:**
>
> | Tipus | Descripció |
> |---|---|
> | **OEM** | Preinstal·lada per fabricants (Dell, HP...). Lligada al maquinari, no es pot transferir. |
> | **Retail** | Comprada per l'usuari directament. Es pot transferir entre equips. |
> | **Volume (MAK/KMS)** | Per empreses amb molts equips. S'activen contra un servidor intern o clau mestra. |
> | **Digital License** | S'associa al compte Microsoft. S'activa automàticament al connectar-se. |
>
> En aquest cas el sistema es troba en **mode de notificació** (sense llicència vàlida), cosa habitual en entorns de laboratori i instal·lacions de prova.

---

## Pas 15 – Preu aproximat d'una llicència Windows

> **Preus oficials de Microsoft (abril 2026):**
>
> | Edició | Preu aprox. (web oficial) |
> |---|---|
> | Windows 11 Home | ~145 € |
> | Windows 11 Pro | ~259 € |
> | Windows 11 Pro for Workstations | ~439 € |
>
> Es poden trobar llicències a preus inferiors a botigues de programari autoritzades com Amazon, El Corte Inglés Digital o el propi Microsoft Store. Les llicències OEM (per a PC nous) solen ser més barates perquè van lligades al maquinari.

---

# Fase 4 – Gestor d'arrencada

## Pas 16 – Obrir Command Prompt com a administrador

* Al cercador de Windows busco **"cmd"** (Símbolo del sistema) i clico **"Ejecutar como administrador"** per tenir tots els permisos necessaris per executar `bcdedit`.

![Pas 16 – Obrir CMD com administrador](imatges-windows/33.png)

---

## Pas 17 – Executar bcdedit

* Executo la comanda `bcdedit` que mostra la configuració completa del gestor d'arrencada de Windows (BCD – Boot Configuration Data).

![Pas 17 – Resultat de bcdedit amb els blocs Boot Manager i Boot Loader](imatges-windows/34.png)

---

## Pas 18 – Identificar els blocs Boot Manager i Boot Loader

A la sortida de `bcdedit` s'identifiquen dos blocs principals:

**Bloc 1 – Administrador de arranque de Windows (Boot Manager):**
```
Identificador    {bootmgr}
device           partition=\Device\HarddiskVolume1
path             \EFI\Microsoft\Boot\bootmgfw.efi
description      Windows Boot Manager
default          {current}
timeout          30
```

**Bloc 2 – Cargador de arranque de Windows (Boot Loader):**
```
Identificador    {current}
device           partition=C:
path             \WINDOWS\system32\winload.efi
description      Windows 11
```

---

## Pas 19 – Interpretar dades concretes

> **Del bloc Boot Manager:**
>
> | Camp | Valor | Significat |
> |---|---|---|
> | `default` | `{current}` | El sistema que arrenca per defecte és el Windows 11 actual |
> | `timeout` | `30` | Espera 30 segons al menú d'arrencada abans d'iniciar automàticament |
>
> **Del bloc Boot Loader:**
>
> | Camp | Valor | Significat |
> |---|---|---|
> | `device` | `partition=C:` | Windows està instal·lat a la partició C: |
> | `path` | `\WINDOWS\system32\winload.efi` | Fitxer que carrega el sistema operatiu |
> | `description` | `Windows 11` | Nom del sistema operatiu |

---

## Pas 20 – Respostes a les preguntes curtes

> **Quin sistema s'està arrencant?**
> → **Windows 11** (identificador `{current}`).
>
> **A quin disc o partició està instal·lat?**
> → A la partició **C:** del primer disc dur.
>
> **Quant temps espera abans d'arrencar?**
> → **30 segons** (`timeout 30`).
>
> **Quin fitxer inicia Windows?**
> → `\WINDOWS\system32\winload.efi`

---

## Pas 21 – Interpretació final

> **Qui decideix l'arrencada (Boot Manager):**
> El **Windows Boot Manager** (`bootmgr`) és el responsable de mostrar el menú d'arrencada i decidir quin sistema operatiu s'iniciarà, en funció de la configuració del BCD.
>
> **Qui carrega el sistema (Boot Loader):**
> El **Windows Boot Loader** (`winload.efi`) és el fitxer que realment carrega el nucli de Windows a la memòria i inicia el sistema operatiu un cop el Boot Manager ha pres la decisió.

---

# Fase 5 – Xarxa bàsica

## Pas 22 – Obrir la configuració de xarxa

* Obro **Configuració > Red e Internet** ① i entro a **Ethernet** ② per veure i modificar la configuració de la connexió de xarxa.

![Pas 22 – Configuració > Red e Internet > Ethernet](imatges-windows/35.png)

---

## Pas 23 – Consultar la IP amb `ipconfig`

* Obro el CMD i executo `ipconfig`. Es mostren les dades de la interfície Ethernet:
    - **Adreça IPv4:** `10.0.2.5`
    - **Màscara de subxarxa:** `255.255.255.0`
    - **Porta d'enllaç (gateway):** `10.0.2.2`

![Pas 23 – Resultat d'ipconfig: IP 10.0.2.5](imatges-windows/37.png)

---

## Pas 24 – Configurar IP dinàmica (DHCP automàtic)

* A la configuració d'Ethernet, verifico que l'assignació d'IP és **Automático (DHCP)**. Amb DHCP, el router assigna automàticament una IP al dispositiu cada vegada que es connecta, sense necessitat de configuració manual.

![Pas 24 – Configuració de IP dinàmica (DHCP) activa](imatges-windows/38.png)

---

## Pas 25 – Configurar IP fixa (manual)

* A la pantalla d'informació de l'adaptador Ethernet, es pot veure la configuració actual obtinguda per DHCP que establiré de forma estàtica:
    - **IP:** `10.0.2.5`
    - **Màscara:** `255.255.255.0` (prefixe /24)
    - **Gateway:** `10.0.2.2`
    - **DNS:** `10.0.2.3`

  Per configurar una IP fixa, des d'aquest panell es pot clicar **Editar** als camps "Asignación de IP" i canviar-ho a **Manual**, introduint els mateixos valors però de forma estàtica.

![Pas 25 – Detalls de la IP obtinguda per DHCP (base per IP fixa)](imatges-windows/36.png)

---

## Pas 26 – Comprovar la connexió amb `ping google.com`

* Executo `ping google.com` al CMD. El resultat mostra que els 4 paquets enviats han estat rebuts amb 0 pèrdues i un temps de resposta d'aproximadament **23-24 ms**, confirmant que la connexió a Internet és correcta.

![Pas 26 – ping google.com: connexió verificada correctament](imatges-windows/39.png)

---

# Fase 6 – Comandes generals

## Pas 27 – Obrir PowerShell

* Busco **"Windows PowerShell"** al cercador i el selecciono per obrir-lo.

![Pas 27a – Cerca i selecció de Windows PowerShell](imatges-windows/40.png)

* El terminal de **Windows PowerShell** s'obre correctament, identificat per el prefix `PS` i la ruta actual `C:\WINDOWS\system32>`.

![Pas 27b – Terminal de PowerShell obert](imatges-windows/41.png)

---

## Pas 28 – Diferenciar CMD i PowerShell

> | Característica | CMD (Símbol del Sistema) | PowerShell |
> |---|---|---|
> | **Propòsit** | Comandes bàsiques i clàssiques de Windows | Automatització avançada i scripting |
> | **Objectes** | Traballa amb text pla | Treballa amb **objectes** .NET |
> | **Potència** | Limitada, herència de MS-DOS | Molt potent, permet pipelines d'objectes |
> | **Scripts** | Arxius `.bat` | Arxius `.ps1` |
> | **Exemple** | `dir`, `copy`, `del` | `Get-ChildItem`, `Copy-Item`, `Remove-Item` |
>
> **Quan usar cadascun:**
> - **CMD** → Per a tasques ràpides, scripts simples o compatibilitat amb aplicacions antigues.
> - **PowerShell** → Per automatitzar tasques, gestionar el sistema, treballar amb serveis, registres i objectes complexos.

---

## Pas 29 – Comandes bàsiques (provades)

* Executo `dir` per veure els fitxers i carpetes del directori arrel `C:\`. Es mostren carpetes com `Program Files`, `Users`, `Windows`, etc.

![Pas 29a – dir: llistat de fitxers i carpetes de C:\](imatges-windows/42.png)

* Executo `cd Eros` per moure'm a la carpeta de l'usuari, `mkdir prova` per crear una nova carpeta i `echo hola > fitxer.txt` per crear un fitxer de text.

![Pas 29b – cd, mkdir prova i echo hola > fitxer.txt](imatges-windows/43.png)

* Torno a executar `dir` per confirmar que tant la carpeta **prova** com el fitxer **fitxer.txt** (14 bytes) han estat creats correctament.

![Pas 29c – dir: confirmació de la carpeta prova i fitxer.txt creats](imatges-windows/44.png)

* Finalment executo `del fitxer.txt` per eliminar el fitxer i torno a fer `dir` per confirmar que ja no apareix al llistat.

![Pas 29d – del fitxer.txt i comprovació amb dir](imatges-windows/45.png)

---

## Pas 30 – Comandes útils del sistema

* `tasklist` → Mostra tots els **processos actius** al sistema amb el seu PID, sessió i consum de memòria. Es poden veure processos com `System`, `svchost.exe`, `wininit.exe`, etc.

![Pas 30a – tasklist: llistat de processos actius](imatges-windows/46.png)

* `taskkill /IM notepad.exe /F` → Força el tancament del procés **Notepad**. El sistema confirma: *"se terminó el proceso Notepad.exe con PID 4316"*.

![Pas 30b – taskkill: tancament forçat del Notepad](imatges-windows/47.png)

* `systeminfo` → Mostra informació detallada del sistema: nom de l'equip (**ASTRO**), sistema operatiu (**Windows 11 Pro**), fabricant (**VirtualBox**), memòria, processador, data d'instal·lació, etc.

![Pas 30c – systeminfo: informació completa del sistema](imatges-windows/48.png)

* `hostname` i `whoami` → `hostname` retorna el nom de l'equip (**Astro**) i `whoami` retorna l'usuari atual (**astro\eros**).

![Pas 30d – hostname i whoami](imatges-windows/49.png)

---

## Pas 31 – Comandes de xarxa

* `ipconfig` mostra la configuració IP de l'adaptador Ethernet: **IP 10.0.2.5**, màscara **255.255.255.0** i gateway **10.0.2.2**.

  `ping google.com` envia paquets a Google i el resultat confirma connexió a Internet amb **0% de pèrdua** i temps de resposta ~23 ms.

![Pas 31a – ipconfig i ping google.com](imatges-windows/50.png)

* `netstat -an` mostra totes les **connexions de xarxa obertes** al sistema, tant les que estan en estat LISTENING (escoltant) com les ESTABLISHED (connectades). És útil per detectar connexions actives o ports oberts.

![Pas 31b – netstat -an: connexions obertes i ports en escolta](imatges-windows/51.png)

---

## Pas 32 – Comandes interessants (avançades)

* `tree` → Mostra l'estructura de carpetes en forma d'arbre des del directori actual. En aquest cas mostra totes les subcarpetes de l'usuari Eros com `Desktop`, `Documents`, `Downloads`, `prova`, etc.

![Pas 32a – tree: estructura de carpetes en arbre](imatges-windows/52.png)

* `cls` → Neteja completament la pantalla del terminal. La pantalla queda buida i el cursor torna a l'inici.

![Pas 32b – cls: pantalla netejada](imatges-windows/53.png)

* `help` → Mostra el sistema d'ajuda de PowerShell amb informació sobre els cmdlets, funcions i conceptes disponibles.

![Pas 32c – help: sistema d'ajuda de PowerShell](imatges-windows/54.png)

> **Nota sobre `shutdown /s /t 0`:** Aquesta comanda apaga l'equip immediatament (temps = 0 segons). No he fet captura per evitar apagar la màquina virtual durant la sessió de treball.

---

## Pas 33 – Mini interpretació

> **Què mostra `tasklist`?**
> → Mostra tots els **processos actius** en el moment d'executar la comanda, amb el nom del procés, el seu **PID** (identificador de procés), la sessió on s'executa i el consum de memòria. Permet veure quins programes estan en marxa i consumint recursos.
>
> **Què mostra `ipconfig`?**
> → Mostra la **configuració de xarxa** de totes les interfícies del sistema: adreça IP, màscara de subxarxa, porta d'enllaç predeterminada i adreces IPv6. Molt útil per diagnosticar problemes de connectivitat.
>
> **Què mostra `systeminfo`?**
> → Mostra un **resum complet de la informació del sistema**: nom de l'equip, versió del sistema operatiu, fabricant del maquinari, quantitat de RAM, processador instal·lat, data d'instal·lació de Windows i molt més. Equivalent a "Sobre aquest equip" però molt més detallat.

---

# Fase 7 – Instal·lació d'aplicacions

## Pas 34 – Descarregar un programa des del navegador (VS Code)

* Obro el navegador Microsoft Edge i busco **"vscode"** a Bing. Clico a l'enllaç de descàrrega oficial de **Visual Studio Code**.

![Pas 34a – Cerca de VS Code a Bing](imatges-windows/55.png)

* A la pàgina oficial `code.visualstudio.com/Download` selecciono la versió per **Windows (Windows 10, 11)** i inicio la descàrrega de l'instal·lador.

![Pas 34b – Pàgina de descàrrega de VS Code per a Windows](imatges-windows/56.png)

---

## Pas 35 – Instal·lar seguint l'assistent

* Segueixo l'assistent d'instal·lació. Primer accepto el **acord de llicència** de Microsoft per a VS Code i clico Següent.

![Pas 35a – Acord de llicència de VS Code](imatges-windows/57.png)

* Confirmo la **carpeta de destinació** on s'instal·larà VS Code: `C:\Users\Eros\AppData\Local\Programs\Microsoft VS Code` (requereix mínim 576,5 MB).

![Pas 35b – Selecció de carpeta de destinació](imatges-windows/58.png)

* Selecciono les **tasques addicionals**: registro VS Code com a editor per a tipus de fitxers admesos i l'afegeixo al PATH del sistema (disponible després de reiniciar).

![Pas 35c – Tasques addicionals: PATH i registre d'editor](imatges-windows/59.png)

* L'instal·lació de VS Code s'executa i la barra de progrés avança mentre s'extreuen els arxius.

![Pas 35d – Instal·lació de VS Code en curs](imatges-windows/60.png)

---

## Pas 36 – Obrir i comprovar que funciona

* L'instal·lador finalitza correctament. Apareix la pantalla de **"Completando la instalación de Visual Studio Code"** amb l'opció d'executar VS Code immediatament. Clico **Finalizar**.

![Pas 36a – Instal·lació completada. Llest per obrir VS Code](imatges-windows/61.png)

* **Visual Studio Code** s'obre correctament i mostra la pantalla de benvinguda amb les opcions de **Walkthrough: Setup VS Code**. L'aplicació funciona perfectament a Windows 11.

![Pas 36b – VS Code obert i funcionant correctament](imatges-windows/62.png)

---

## Pas 37 – Instal·lar una aplicació des de Microsoft Store

* Obro la **Microsoft Store** i busco **"Python 3"**. Apareix **Python 3.13** de la Python Software Foundation (valoració 4.6★). Clico **Obtener** per instal·lar-lo.

![Pas 37 – Python 3.13 a la Microsoft Store](imatges-windows/63.png)

---

## Pas 38 – Obrir i comprovar el funcionament

* Un cop instal·lat, obro el CMD i executo `python3`. L'intèrpret de **Python 3.13.12** s'inicia correctament indicant la versió i la plataforma (`win32`). El prompt `>>>` confirma que Python està llest per executar codi.

![Pas 38 – Python 3.13 instal·lat i funcionant al CMD](imatges-windows/64.png)

---

## Pas 39 – Desinstal·lar una aplicació

* Obro **Configuració > Aplicaciones** ① i clico **Aplicaciones instaladas** ② per veure la llista de programes instal·lats.

![Pas 39a – Configuració > Aplicaciones > Aplicaciones instaladas](imatges-windows/65.png)

* Busco **"visual"** al cercador. Apareix **Microsoft Visual Studio Code (User)** v1.115.0 (577 MB). Clico els tres punts (**···**) per veure les opcions.

![Pas 39b – Localitzar VS Code a les aplicacions instal·lades](imatges-windows/66.png)

* Al menú desplegable selecciono **Desinstalar** per iniciar el procés d'eliminació de VS Code.

![Pas 39c – Opció Desinstalar de VS Code](imatges-windows/67.png)

* L'assistent de desinstal·lació s'executa i mostra la pantalla **"Estado de la Desinstalación"** mentre elimina tots els arxius de VS Code del sistema.

![Pas 39d – Procés de desinstal·lació de VS Code en curs](imatges-windows/68.png)

---

## Pas 40 – Verificació: comprovar que ja no apareix al sistema

* Faig una cerca de **"Visual Studio"** al cercador de Windows. Abans apareixia VS Code com a millor coincidència; ara ja **no apareix cap aplicació instal·lada**, només suggeriments de cerca web. Això confirma que VS Code ha estat desinstal·lat correctament.

![Pas 40 – Verificació: VS Code ja no apareix al sistema](imatges-windows/69.png)