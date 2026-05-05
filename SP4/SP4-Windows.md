---
layout: custom
title: "Sprint 4: Configuració del Programari de Base i Sistemes d'Emmagatzematge en Windows"
---

# RAID amb Windows Server 2022

## Teoria: Què és un RAID?

Els **RAID** (*Redundant Array of Independent Disks*) són sistemes que permeten combinar diversos discos físics en una sola unitat lògica per aconseguir millores en rendiment, capacitat o seguretat. Aquests sistemes poden ser gestionats per **maquinari** (controladores RAID dedicades) o per **programari**, com en el cas dels sistemes operatius moderns com Windows Server.

Cada nivell de RAID ofereix característiques diferents segons l'objectiu: millorar velocitat, oferir redundància o combinar ambdues coses.

---

## Taula de nivells de RAID

| Nivell | Nom | Nº mínim de discos | Capacitat útil | Tolerància a fallades | Rendiment lectura | Rendiment escriptura | Ús típic |
|--------|-----|--------------------|----------------|-----------------------|-------------------|----------------------|----------|
| **RAID 0** | Striping | 2 | 100% (tots els discos) | ❌ Cap | ✅ Excel·lent | ✅ Excel·lent | Edició de vídeo, gaming |
| **RAID 1** | Mirroring | 2 | 50% (la meitat dels discos) | ✅ 1 disc | ✅ Bona | ⚠️ Normal | Servidors crítics, OS |
| **RAID 5** | Striping amb paritat distribuïda | 3 | N-1 discos | ✅ 1 disc | ✅ Bona | ⚠️ Moderada | Servidors de fitxers |
| **RAID 6** | Doble paritat | 4 | N-2 discos | ✅ 2 discos | ✅ Bona | ⚠️ Lenta | Emmagatzematge crític |
| **RAID 10** | Mirroring + Striping | 4 | 50% (la meitat dels discos) | ✅ 1 per parella | ✅ Excel·lent | ✅ Excel·lent | Bases de dades, ERP |

> **Nota:** Cap nivell de RAID substitueix una còpia de seguretat. El RAID protegeix contra fallades de disc, però no contra errors humans, corrupció de dades o desastres físics.

---

## El RAID 5 en detall

El **RAID 5** és un dels nivells més utilitzats en entorns professionals perquè aconsegueix un equilibri entre rendiment, capacitat i tolerància a fallades. Distribueix tant les dades com la informació de **paritat** entre tots els discos. La paritat serveix per reconstruir la informació en cas que un dels discos falli, i permet que el sistema segueixi funcionant encara que un disc deixi de funcionar.

**Característiques clau del RAID 5:**
- Necessita un **mínim de 3 discos**.
- La capacitat útil és la suma de tots els discos **menys un** (dedicat a paritat). Exemple: 3 × 10 GB = 20 GB útils.
- La paritat es calcula i escriu automàticament en cada operació d'escriptura (petita penalització).
- La lectura és molt eficient ja que es pot llegir en paral·lel de múltiples discos.
- **No tolera la fallada de 2 o més discos simultàniament.**

---

## Pràctica: RAID 5 pas a pas amb Windows Server 2022

### 1. Preparació: Afegir 3 discos nous a la màquina virtual

Abans d'iniciar Windows Server, apaguem completament la màquina virtual i anem a **Configuració → Emmagatzematge** a VirtualBox. Afegim 3 nous discos durs virtuals de **10 GB** cadascun, en format **VDI** amb reserva dinàmica:

- `W2022_1.vdi` — 10 GB
- `W2022_2.vdi` — 10 GB
- `W2022_3.vdi` — 10 GB

A la captura es pot veure com queden els tres discos afegits al controlador SATA de la màquina virtual, tots amb una mida virtual de 10,00 GB.

![Configuració VirtualBox - 3 discos nous afegits de 10GB cadascun](imatges-windows/1.png)

---

### 2. Obrir el Gestor de discos

Un cop iniciada la màquina, premem **Windows + R** per obrir el diàleg d'execució i escrivim `diskmgmt.msc`. Premem **Acceptar** per obrir l'**Administrador de discos lògics**.

![Windows + R → diskmgmt.msc per obrir el Gestor de discos](imatges-windows/2.png)

---

### 3. Inicialitzar els 3 discos nous

En obrir el Gestor de discos, apareix automàticament l'assistent per inicialitzar els discos nous. Seleccionem els tres discos (**Disco 1**, **Disco 2**, **Disco 3**) i triem l'estil de partició **GPT** (*GUID Partition Table*), recomanat per a sistemes moderns i discos de més de 2 TB. Fem clic a **Aceptar**.

![Inicialitzar discos - Selecció Disco 1, 2, 3 amb estil GPT](imatges-windows/3.png)

---

### 4. Verificació: discos inicialitzats sense assignar

Després de la inicialització, el Gestor de discos mostra els tres discos com a **Bàsic**, amb **9,98 GB** cadascun i tot l'espai com a **No asignat**. Encara no s'ha creat cap partició ni volum.

![Disk Management - Disco 1, 2, 3 inicialitzats com a Bàsic i No asignat](imatges-windows/4.png)

---

### 5. Convertir els discos a dinàmics

Per poder crear un volum RAID 5 amb el Gestor de discos de Windows, els discos han de ser de tipus **Dinàmic**. Fem clic dret sobre el **Disco 1** i seleccionem **Convertir en disco dinámico...**

![Clic dret sobre Disco 1 → Convertir en disco dinámico](imatges-windows/5.png)

Al diàleg que apareix, marquem els tres discos (**Disco 1**, **Disco 2**, **Disco 3**) i fem clic a **Aceptar** per convertir-los tots d'un sol cop.

![Selecció dels 3 discos per convertir a dinàmics](imatges-windows/6.png)

---

### 6. Verificació: discos ara dinàmics

Després de la conversió, el Gestor de discos mostra els tres discos amb l'etiqueta **Dinámico** i tot l'espai segueix com a **No asignat**. Ja estan preparats per crear el RAID 5.

![Disk Management - Disco 1, 2, 3 ara com a Dinámico i No asignat](imatges-windows/7.png)

---

### 7. Crear el volum RAID 5

Fem clic dret sobre l'espai no assignat del **Disco 1** i seleccionem **Nuevo volumen RAID-5...** per iniciar l'assistent de creació.

![Clic dret → Nuevo volumen RAID-5...](imatges-windows/8.png)

---

### 8. Selecció dels discos per al RAID 5

A l'assistent, afegim els tres discos (**Disco 1**, **Disco 2**, **Disco 3**) a la columna de seleccionats. El sistema calcula automàticament:

- **Tamaño total del volumen:** 20.444 MB (~20 GB)
- **Espacio máximo disponible:** 10.222 MB per disc

Recordem que en RAID 5 la capacitat útil és `(N-1) × mida_disc` = 2 × 10 GB = **20 GB**.

![Selecció dels 3 discos pel RAID 5 - capacitat útil 20.444 MB](imatges-windows/9.png)

---

### 9. Assignar lletra d'unitat

Assignem la lletra **R:** al nou volum RAID 5, que és la lletra que utilitzarem per accedir als fitxers del RAID des de l'explorador d'arxius.

![Assignació de la lletra R: al volum RAID 5](imatges-windows/10.png)

---

### 10. Formatar el volum RAID 5

Configurem el format del volum:
- **Sistema de fitxers:** NTFS
- **Mida de la unitat d'assignació:** Predeterminada
- **Etiqueta del volum:** `RAID5-Astro`

Fem clic a **Siguiente** per continuar.

![Formatant el volum RAID 5 amb NTFS i etiqueta RAID5-Astro](imatges-windows/11.png)

---

### 11. Resum final de l'assistent

L'assistent mostra el resum de la configuració seleccionada abans de finalitzar:

- Tipus de volum: **RAID-5**
- Discos seleccionats: **Disco 1, Disco 2, Disco 3**
- Tamany del volum: **20.444 MB**
- Lletra d'unitat: **R:**
- Sistema de fitxers: **NTFS**
- Etiqueta: **RAID5-Astro**

Fem clic a **Finalizar** per crear el RAID.

![Resum final de la configuració RAID 5 - Finalitzar](imatges-windows/12.png)

---

### 12. Procés de format en curs

El Gestor de discos mostra els tres discos mentre s'estan formatant. El progrés indica que el format està al **92%**. Els tres discos estan sincronitzant les dades i la paritat.

![Disk Management - Format del RAID 5 en curs al 92%](imatges-windows/13.png)

---

### 13. RAID 5 creat i operatiu

Un cop completat el format, els tres discos mostren el volum **RAID5-Astro** amb estat **Correcto**. El RAID 5 ja és completament funcional.

![Disk Management - RAID5-Astro en estat Correcto als 3 discos](imatges-windows/14.png)

---

### 14. El RAID 5 visible a l'Explorador d'arxius

Obrim l'Explorador d'arxius i podem veure que el nou volum **RAID5-Astro (R:)** apareix com a unitat del sistema amb **19,9 GB disponibles**. Confirma que el sistema operatiu reconeix i pot accedir al RAID 5.

![Explorador d'arxius - RAID5-Astro (R:) amb 19.9 GB disponibles](imatges-windows/15.png)

---

### 15. Crear una carpeta de prova al RAID

Entrem a la unitat `R:\` i creem la carpeta **Prova-Raid5Astro** per provar que podem escriure dades al volum RAID 5.

![Carpeta Prova-Raid5Astro creada a R:\\](imatges-windows/16.png)

---

### 16. Copiar fitxers al RAID

Copiem fitxers dins de la carpeta `Prova-Raid5Astro`. En aquest cas s'han copiat diverses aplicacions i carpetes de dotnet. Comprovem que tots els fitxers s'han copiat correctament i es poden obrir.

![Fitxers copiats correctament dins de R:\Prova-Raid5Astro](imatges-windows/17.png)

---

## Simulació de fallades

### 17. Primera fallada: posar el Disco 1 fora de línia

Tornem al Gestor de discos. Fem clic dret sobre el **Disco 1** i seleccionem **Sin conexión** per simular la fallada física d'un disc del RAID.

![Clic dret sobre Disco 1 → Sin conexión (primera fallada)](imatges-windows/18.png)

---

### 18. RAID en estat degradat (1 disc fallat)

Amb el Disco 1 desactivat, el Gestor de discos mostra tots els membres del RAID amb l'estat **"Error de redundancia"**. El sistema detecta que ha perdut un disc i opera en mode degradat. A la llista superior es pot veure que el volum `RAID5-Astro (R:)` figura com a **Error de re...** (redundàncies).

> **Important:** Malgrat el mode degradat, el RAID 5 **segueix funcionant** gràcies a la paritat distribuïda.

![RAID 5 en estat degradat - Error de redundancia amb Disco 1 desactivat](imatges-windows/19.png)

---

### 19. Verificació: els fitxers segueixen accessibles

Amb un disc fora de línia, comprovem que podem accedir a `R:\Prova-Raid5Astro` i obrir els fitxers sense cap problema. La vista dividida mostra el Gestor de discos (amb errors de redundància) i l'Explorador d'arxius (amb els fitxers accessibles). Això demostra la **tolerància a fallades del RAID 5**.

![Comprovació: fitxers accessibles malgrat la fallada del Disco 1](imatges-windows/20.png)

---

### 20. Segona fallada: posar el Disco 2 fora de línia

Ara simulem una **segona fallada simultània** posant el **Disco 2** també fora de línia. Fem clic dret i seleccionem **Sin conexión**.

![Clic dret sobre Disco 2 → Sin conexión (segona fallada)](imatges-windows/21.png)

---

### 21. RAID col·lapsat (2 discos fallats)

Amb dos discos fora de línia, el Gestor de discos mostra tots els membres amb estat **"Error"**. El Disco 1 i el Disco 2 figuren com a **Desactivada**, i apareixen dos membres addicionals marcats com a **Falta**. El RAID 5 **ja no és capaç de reconstruir les dades** i el volum `R:\` ha deixat de ser accessible.

> **El RAID 5 només tolera la fallada d'UN disc.** Amb dos discos fallats, es perd l'accés a totes les dades.

![RAID 5 col·lapsat - Disco 1 i Disco 2 desactivats, estat Error a tots](imatges-windows/22.png)

---

## Recuperació

### 22. Tornar a posar el Disco 1 en línia

Per recuperar el RAID, tornem a posar els discos en línia. Fem clic dret sobre el **Disco 1** (en estat "Desactivada") i seleccionem **En línea**.

![Clic dret Disco 1 → En línea per iniciar la recuperació](imatges-windows/23.png)

---

### 23. RAID parcialment recuperat

Després de posar el Disco 1 en línia, el Gestor de discos mostra el Disco 1 i el Disco 3 **en pantalla** (operatius), però el Disco 2 encara figura amb avís. L'estat dels discos segueix mostrant **"Error de redundancia"** perquè el Disco 2 encara no s'ha resincronitzat.

![Disco 1 en línia - RAID parcial, Disco 2 encara amb advertència](imatges-windows/24.png)

---

### 24. Reactivar el Disco 2

Per completar la recuperació, fem clic dret sobre el **Disco 2** i seleccionem **Reactivar disco**. Això ordena a Windows que torni a sincronitzar la paritat i les dades del disc recuperat amb la resta del RAID.

![Clic dret Disco 2 → Reactivar disco per completar la recuperació](imatges-windows/25.png)

---

### 25. Resincronització en curs

El Gestor de discos mostra els tres discos amb l'estat **"Volviendo a sincronizar: (63%)"**. Windows Server està recalculant la paritat i verificant la coherència de les dades entre els tres discos. Aquest procés pot trigar uns minuts depenent de la mida del RAID.

![Resincronització del RAID 5 al 63%](imatges-windows/26.png)

---

### 26. RAID 5 totalment recuperat

Un cop finalitzada la resincronització, els tres discos tornen a mostrar l'estat **Correcto**. El volum RAID5-Astro ha recuperat tota la seva redundància i funciona completament.

![Disk Management - Tots els discos en estat Correcto després de la recuperació](imatges-windows/27.png)

---

### 27. Verificació final: fitxers intactes

Comprovem que tots els fitxers de `R:\Prova-Raid5Astro` segueixen intactes i accessibles. La vista dividida mostra el Gestor de discos (tots **Correcto**) i l'Explorador d'arxius amb tots els fitxers originals presents i en perfecte estat.

![Verificació final - RAID recuperat i fitxers intactes a R:\Prova-Raid5Astro](imatges-windows/28.png)

---

## Conclusions i Observacions

### Resultats de la pràctica

| Pas | Situació | Estat del RAID | Accés a fitxers |
|-----|----------|---------------|-----------------|
| Inicial | 3 discos operatius | ✅ Correcto | ✅ Accessible |
| 1 disc fora de línia | Mode degradat | ⚠️ Error de redundancia | ✅ Accessible |
| 2 discos fora de línia | RAID col·lapsat | ❌ Error total | ❌ Inaccessible |
| 1 disc recuperat | Recuperació parcial | ⚠️ Error de redundancia | ✅ Accessible |
| 2 discos recuperats + resync | Recuperació completa | ✅ Correcto | ✅ Accessible |

### Observacions tècniques

- **El RAID 5 distribueix la paritat entre tots els discos:** No hi ha un disc dedicat exclusivament a paritat (com en RAID 3/4), sinó que la paritat es reparteix cíclicament, cosa que equilibra la càrrega d'escriptura.

- **Tolerància a una sola fallada:** Tal com hem comprovat, el RAID 5 pot continuar funcionant i servint dades amb un disc fora de línia, gràcies al càlcul de paritat. Amb dos discos fallats, la paritat ja no és suficient per reconstruir tota la informació.

- **La capacitat útil és N-1:** Amb 3 discos de ~10 GB, la capacitat útil ha estat d'aproximadament **20 GB** (un disc equivalent queda dedicat a paritat).

- **El procés de resincronització pot ser llarg i estressant per als discos:** En un entorn real amb discos de grans capacitats (TB), la reconstrucció pot durar hores i suposa una càrrega addicional sobre els discos restants, augmentant el risc de fallada d'un segon disc.

- **El RAID 5 NO substitueix una còpia de seguretat:** Protegeix contra la fallada física d'un disc, però no contra errors humans (esborrat accidental), corrupció lògica de dades, ransomware o desastres físics (incendi, inundació).

- **Windows Server implementa RAID per programari:** En entorns professionals és preferible usar controladores RAID per maquinari per aconseguir millor rendiment i gestió independent del sistema operatiu.

### Quan és adequat el RAID 5?

El RAID 5 és una bona opció per a:
- Servidors de fitxers que requereixen accés continu amb un cert nivell de seguretat.
- Entorns amb pressupost limitat que necessiten redundància.
- Sistemes on les lectures són més freqüents que les escriptures.

No és adequat per a:
- Entorns on es requereix la màxima disponibilitat (preferir RAID 6 o RAID 10).
- Sistemes amb moltes operacions d'escriptura intensiva.
- Com a única mesura de protecció de dades (cal combinar-lo amb còpies de seguretat).
