---
layout: custom
title: "SPRINT 5: MONITORATGE, AUDITORIES I PROGRAMARI CLIENT SERVIDOR"
---

## LOGS

### Exercici 1: Fer captures del monitor del sistema

Els logs del sistema es troben a `/var/log`. Tots els serveis del sistema generen logs, però algunes aplicacions tenen els logs a les seves pròpies carpetes.

Llistem el contingut de `/var/log` per veure tots els fitxers de log disponibles:

![ls /var/log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-03-25.png)

Visualitzem el contingut del log principal del sistema (`syslog`) amb `cat`:

![cat syslog](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-04-40.png)

Podem seguir en temps real els nous missatges del syslog amb `tail -f /var/log/syslog`:

![tail -f /var/log/syslog](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-14-13.png)

---

### Rotació de logs del sistema

La rotació de logs s'encarrega d'arxivar i eliminar els logs antics per evitar que el disc s'ompli. La configuració de logrotate es troba a `/etc/logrotate.d/`:

![ls /etc/logrotate.d](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-06-40.png)

Consultem la configuració de rotació per a rsyslog, que indica com es roten els logs del sistema (cada setmana, mantenint 4 còpies, amb compressió):

![cat /etc/logrotate.d/rsyslog](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-07-22.png)

Consultem la configuració de rotació per a apt (rotació mensual, 12 còpies):

![cat /etc/logrotate.d/apt](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-07-59.png)

Obrim el fitxer de configuració principal de rsyslog (`/etc/rsyslog.conf`) per veure els mòduls carregats i directives globals:

![nano /etc/rsyslog.conf - command](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-09-14.png)

![nano /etc/rsyslog.conf - contingut](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-10-37.png)

---

### Proves amb la comanda `logger`

La comanda `logger` permet enviar missatges als logs del sistema especificant la *facility* (origen) i la *priority* (nivell de gravetat).

**Prova 1 – kern.notice:** S'envia el missatge "Prova Ferran" amb facility `kern` i priority `notice`. El missatge apareix al syslog:

![logger kern.notice Prova Ferran](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-16-42.png)

**Prova 2 – mail.notice:** S'envia "Prova Ferran2" amb facility `mail` i priority `notice`. Comprovem que apareix a `/var/log/mail.log`:

![logger mail.notice Prova Ferran2 + cat mail.log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-18-34.png)

![cat /var/log/mail.log - resultat](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-18-52.png)

Per defecte, `mail.notice` no s'escriu a `mail.log` perquè la configuració de `50-default.conf` només hi redirigeix `mail.err`. Veiem la configuració actual:

![nano /etc/rsyslog.d/50-default.conf - mail.=err actiu](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-20-15.png)

**Prova 3 – mail.notice (Prova Ferran3):** Es comprova que el missatge "Prova Ferran3" no apareix ancora a `mail.log` perquè encara no tenim el `mail.notice` habilitat:

![logger mail.notice Prova Ferran3 + cat mail.log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-21-24.png)

![Resultat al syslog - Prova Ferran3](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-21-39.png)

**Prova 4 – mail.err:** S'envia "Prova Ferran4" amb `mail.err`. Ara sí apareix a `mail.log` perquè la regla `mail.err` hi apunta:

![logger mail.err Prova Ferran4 + cat mail.log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-24-25.png)

**Prova 5 – mail.crit:** S'envia "Prova Ferran5" amb `mail.crit`. Com que `crit` és de major gravetat que `err`, no s'escriu a `mail.log` (la regla és exactament `mail.err`):

![logger mail.crit Prova Ferran5 + cat mail.log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-24-50.png)

S'edita `/etc/rsyslog.d/50-default.conf` per afegir la regla `*.crit -/var/log/ferran.log`, de manera que tots els missatges de nivell `crit` o superior s'escriguin a un fitxer nou:

![nano 50-default.conf - afegim *.crit ferran.log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-25-45.png)

Reiniciem rsyslog i enviem "Prova Ferran6" amb `mail.crit`. Ara el missatge apareix a `mail.log` (perquè `crit > err`) i al nou `ferran.log`:

![systemctl restart rsyslog + logger mail.crit Prova Ferran6](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-26-18.png)

![nano 50-default.conf - regla *.crit ferran.log visible](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-27-52.png)

Reiniciem rsyslog, enviem un missatge `cron.crit` i comprovem que apareix al nou fitxer `/var/log/ferran.log`:

![logger cron.crit hola + cat /var/log/ferran.log](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-29-07.png)

---

### Consultes amb `journalctl`

`journalctl` permet consultar el journal del sistema (systemd). Podem filtrar per prioritat o per facility.

Filtrem tots els missatges de nivell `crit` o superior amb `journalctl -p crit`:

![journalctl -p crit](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-31-20.png)

Filtrem tots els missatges de la facility `mail` amb `journalctl --facility=mail`, on veiem totes les proves enviades (Ferran2 a Ferran6):

![journalctl --facility=mail](Exercici1/Captura%20de%20pantalla%20de%202026-03-02%2012-33-21.png)

---

### Exercici 2: Enviar logs remotament a una altra màquina

En aquest exercici configurem dues màquines:
- **ferranserver** (IP 10.0.2.15) → actua com a **servidor** de logs (rep els logs)
- **ferranbernis1-VirtualBox** → actua com a **client** (envia els logs)

#### Configuració del servidor (ferranserver)

Al servidor creem `/etc/rsyslog.d/10-remote.conf` per habilitar la recepció de logs per UDP i TCP al port 514 i desar-los a `/var/log/remote/<hostname>/syslog.log`:

![cat /etc/rsyslog.d/10-remote.conf](Exercici2/Captura%20de%20pantalla%20de%202026-03-03%2013-38-21.png)

Comprovem que rsyslog escolta al port 514 tant per UDP com per TCP amb `ss -tulpn | grep 514`:

![alt text](<Exercici2/Captura de pantalla de 2026-03-03 13-39-17.png>)

#### Configuració del client (ferranbernis1-VirtualBox)

Al client creem `/etc/rsyslog.d/90-forward.conf` amb la directiva `*.* @@10.0.2.15:514` per reenviar tots els logs al servidor via TCP:

![cat /etc/rsyslog.d/90-forward.conf](Exercici2/Captura%20de%20pantalla%20de%202026-03-03%2013-38-53.png)

#### Prova: enviar un log des del client

Des del client enviem un missatge de prova amb `logger "PROVA"`:

![logger "PROVA" des del client](Exercici2/Captura%20de%20pantalla%20de%202026-03-03%2013-39-38.png)

#### Verificació al servidor

Al servidor comprovem que s'ha creat la carpeta remota per a cada màquina client dins de `/var/log/remote/`:

![ls -la /var/log/remote](Exercici2/Captura%20de%20pantalla%20de%202026-03-03%2013-39-57.png)

Dins de la carpeta del client (`ferranbernis1-VirtualBox`) hi ha el fitxer `syslog.log` amb els logs rebuts:

![ls -la /var/log/remote/ferranbernis1-VirtualBox](Exercici2/Captura%20de%20pantalla%20de%202026-03-03%2013-40-17.png)

Finalment, visualitzem el contingut del `syslog.log` remot i confirmem que el missatge "PROVA" enviat pel client ha arribat correctament al servidor:

![cat syslog.log - missatge PROVA rebut](Exercici2/Captura%20de%20pantalla%20de%202026-03-03%2013-40-40.png)

---
