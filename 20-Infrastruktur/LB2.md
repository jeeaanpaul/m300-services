LB2
===

## Übersicht

Ich habe mich dazu entschieden für die LB2 automatisiert einen Webserver zu erstellen. Zusätzlich werde ich die Webseite ändern und von aussen zugänglich machen. <br>

Das Vagrantfile befindet sich im Ordner ``Vagrant\LB2``. <br>

*** 

## Installation

Als erstes habe ich das Verzeichnis ``LB2`` erstellt.

```pws
vagrant init ubuntu/jammy64
```

Der Server soll später über host-only erreichbaer sein. Dafür habe ich die folgende Zeile im Vagrantfile auskommentiert:

````ruby
config.vm.network "private_network", ip: "192.168.33.10
````

Jetzt erstelle ich das Verzeichnis ``.\config``. <br>
Dort liegt nacher das Index File des Webservers, welches ich ins Document Root kopieren kann.

```pws
mkdir config
```
