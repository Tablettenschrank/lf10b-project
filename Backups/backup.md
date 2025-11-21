# Backup erstellen & wiederherstellen

Backups werden via der WebUI  erstellt und oder konfiguriert wie automatische Backups zu einem gewissen Zeitpunkt.



###### Speicherort

* Backups werden auf der Festplatte auf der gleichen Maschine und auf einen Externen USB Stick Gepeichert
  * Der USB Stick hat 8 GB Speicherplatz
* Der USB  Stick muss aber dafür aktiv ge-mounted werden via `mount /dev/sdg1 /mnt/stick`
* Falls der USB stick nicht ge-mounted sein sollte, werden die Backups dennoch auf die Festplatte auf den gleichen Mount Point gespeichert.
  * Das Speichern ist auf dem Mountpoint somit leider nicht optimal, und daher empfehlen wir es via eines NFS Shares extern zu speichern.
  * Es Funktioniert nur soweit da wir in Proxmox ein Share als Directory angeben und dadurch ein Pfad local auf der platte genommen wird.



1. Container auswählen und zu Backup gehen

![](assets/ZinqdOjQf2dMsLoLpI6AM8HQwGHyS3-TMjVkT5Vhls0=.png)

1. Eingabe Notes zur Identifizierung des Backups

![](assets/EsFkRENg9EkXZBtu5NqOJ5cfSc9yQYSti17zBS9U9Fk=.png)

1. Backup erfolgreich wenn "TASK OK"

![](assets/cvGZps2WA_3W78kCOCehJhQ09ir2katqDTLdCz3Whso=.png)

1. Backup wird angezeigt

![](assets/EEuaGhcUCI8CBuj4jpkENmTdTI1Ha4gHaWmO4TzzP9I=.png)

1. Shutdown

![](assets/zaFUMQsvHU54gMkShmVHiXUNO_rfuFuatFD20A3XkaM=.png)

1. Delete des lxc Containers

![](assets/sGCwUY8mmILiuEoSqLleNON8TrpESeSSu76bGtbEaWU=.png)

1. Bestätigung zum remove

![](assets/mgvTO68ubwOsjGNPRbKX4eaZjV1QCClg833moPVw8ic=.png)

1. Eingabe ID

![](assets/FpHc-EYAh0-5TXP__CDwkRenei4s6tWXyMHGunNwF34=.png)

1. zu Backups gehen

![](assets/3WFLeiu0-ZoOpvGdIzFs7ZN8VIXRXSxM4Kb3IznyTqU=.png)

1. Backup auswählen und Restore klicken

![](assets/oV7xnFmhBlVZeolhiI6-ycosDjQTT7vp77jxTq8k4fY=.png)

1. CT ID eingeben 

![](assets/dalQpKYb-2AdzciQKCa3cIfN36sRSYF-M-8r88CVFFU=.png)

1. &#x20;Erfolgreich bei "TASK OK"

![](assets/ioQaJruke7E82-U3v9GQwHcTb8PkjM_WR0e7ZuTxpbI=.png)

1. &#x20;Container wurde aus dem Backup wiederhergestellt

![](assets/3O3Abj-k-fAJMNSP6N1J992KDYPjwnwsYD9-n-EAP3o=.png)

Docker checkmk sollte wieder erreichbar sein. 
