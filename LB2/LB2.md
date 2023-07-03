LB2
===

## Übersicht

Die LB2 deckt die Kapitel [20-Infrastuktur](../20-Infrastruktur/) und [25-Sicherheit](../25-Sicherheit/). <br>

Für die Automatisierung eines Serverdienstes habe ich mich für CUPS entschieden. Mit CUPS können wir einen Printserver einrichten, der Printjobs annimmt und sie an die jeweiligen Printerqueues sendet. <br>

### Aktueller Wissenstand:
Ich habe bereits während meines zweiten Lehrjahrs im Unix Team verschiedene Serverdienste zum Testen installieren müssen. Dort habe ich auchschon einmal den CUPS Printerdienst installiert und einen Print2PDF Queue eingerichtet, mit dem ich dann von einem anderen Gerät Dateien als PDF speichern konnte.

Das Vagrantfile befindet sich im Ordner ``Vagrant\LB2``. <br>

Als Hilfe, habe ich folgende Dokumentation verwendet: <br>
https://www.fosslinux.com/61850/how-to-set-up-cups-print-server-on-ubuntu.htm
*** 

## Installation

### Vorbereitung:

Für diese LB habe ich das Verzeichnis ``LB2`` erstellt. <br>
Ich verwende Ubunut als Distribution:

```pws
vagrant init ubuntu/jammy64
```

Jetzt erstelle ich das Verzeichnis ``.\config``. <br>
Dort speichere ich dann das Config File für CUPS, damit beim Starten der Vagrantbox die VM bereits konfiguriert ist.

```pws
mkdir config
```
> ### VM einmal manuell erstellen & das Vagrantfile vorbereiten:
```pws
vagrant up
```
<img src="../images/LB2-1.png"  width="600">

> Alle ausgeführten Commands dokumentiere ich mir nebenbei im Notepad und fasse sie dann im Vagrantfile zusammen...
> <br>
> **Wichtig:** Bei manchen Commands braucht es Userinput zur Bestätigung wie z.B ein `y` bei apt install xx. Damit ich bei der Automatisierung den Userinput umgehen kann, gebe ich die richtigen Parameter gleich mit wie zum Beispiel `-y` bei `sudo apt install cups`.
<br>

<img src="../images/LB2-2.png"  width="600">

So, der CUPS Dienst ist jetzt installiert, gestartet und er läuft einwandfrei <br>

Als nächstes können wir CUPS so konfigureren, damit wir ihn auch über das Webinterface erreichen und einrichten können:

```bash
vagrant@ubuntu-jammy:~$ sudo vi /etc/cups/cupsd.conf
```

Hier ist der Output von den Blöcken, welche ich bearbeitet habe:

```bash
vagrant@ubuntu-jammy:~$ cat /etc/cups/cupsd.conf
...
# Only listen for connections from the local machine.
port 631
Listen /run/cups/cups.sock

# Show shared printers on the local network.
Browsing On
BrowseLocalProtocols dnssd

# Show shared printers on the local network.
Browsing On
BrowseLocalProtocols dnssd

# Restrict access to the server...
<Location />
  Order allow,deny
  Allow @LOCAL
</Location>

# Restrict access to the admin pages...
<Location /admin>
 AuthType Default
 Require valid-user
 Order allow,deny
 Allow @LOCAL
</Location>
...
```

Danach habe ich den Dienst nochmals neu gestartet:

> ## Erstes Problem:
> Ich habe versucht von meinem Hostsystem aus auf das CUPS Interfac zuzugreiffen, jedoch ohne Erfolg. <br>
> Das Problem war die Portweiterleitung. Damit ich auf das Webinterface von CUPS zugreifen kann muss ich das erst im Vagrantfile definieren.
> <img src="../images/LB2-3.png"  width="600">

Jetzt wo ich das Konfigurationsfile angepasst habe und das Vagrantfile auch stimmt, versuche ich auf das Webinterface zuzugreiffen:

<img src="../images/LB2-4.png"  width="600">

Super, es hat funktioniert!

### Vagrant File und Config vorbereiten:
Im nächsten Schritt habe ich das CUPS config file vollständig kopiert und im Verzeichnis ``.\Vagrant\config\cupsd.conf`` abgelegt. <br>
Jetzt muss ich noch alle wichtigen Commands dem Vagrantfile hinzufügen und überprüfen, dass das Config file ersetzt wird. <br>

```ruby
config.vm.provision "shell", inline: <<-SHELL
  sudo apt-get update
  sudo apt-get install -y cups
  sudo systemctl start cups
  sudo systemctl enable cups
  sudo cp /vagrant/config/cupsd.conf /etc/cups/cupsd.conf
  sudo systemctl restart cups

SHELL
end
```

### Testen ob alles funktioniert:

Für den Test ob alles läuft, habe ich die VM nochmals destroyed mit `vagrant destroy -f` und anschliessend nochmals neu erstellt mit `vagrant up`.

<img src="../images/LB2-5.png"  width="600">

Und wie wir im folgenden Screenshot sehen funktioniert alles:

<img src="../images/LB2-6.png"  width="600">


## Sicherheit
