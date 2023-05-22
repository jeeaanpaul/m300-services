# Toolumgebung aufsetzen 

Als erstes habe ich alle nötigen Tools installiert. 

- VirtualBox
- Git
- Vagrant

## Apache Webserver 
Nachdem ich die Ubuntu VM eingerichtet habe, habe ich den Apache Webserver installiert.

```
$ sudo apt-get install apache2
$ sudo systemctl status apache2 # status 
$ sudo systemctl enable apache2 # apache startet autom. nach reboot
```

