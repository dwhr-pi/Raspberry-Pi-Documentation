# USB-Host-Boot-Modus

**USB-Host-Boot ist nur auf Raspberry Pi 3B, 3B+, 3A+ und 2B v1.2 verfügbar. Raspberry Pi 3A+ unterstützt nur den Massenspeicher-Boot, keinen Netzwerk-Boot.**

Der Boot-Modus des USB-Hosts folgt dieser Reihenfolge:

* Aktivieren Sie den USB-Port und warten Sie, bis die D+-Leitung hochgezogen wird, was auf ein USB 2.0-Gerät hinweist (wir unterstützen nur USB 2.0).
* Wenn das Gerät ein Hub ist:
    * Aktivieren Sie die Stromversorgung aller Downstream-Ports des Hubs
    * Schleife für jeden Port maximal zwei Sekunden lang (oder fünf Sekunden, wenn `program_usb_boot_timeout=1` gesetzt wurde)
        * Lassen Sie den Reset los und warten Sie, bis D+ hochgefahren wird, um anzuzeigen, dass ein Gerät angeschlossen ist
        * Wenn ein Gerät erkannt wird:
            * Senden Sie "Get Device Descriptor"
                * Wenn VID == SMSC && PID == 9500
                    * Gerät zur Ethernet-Geräteliste hinzufügen
            * Wenn Klassenschnittstelle == Massenspeicherklasse
                * Gerät zur Liste der Massenspeichergeräte hinzufügen
* Anders
    * Einzelgerät aufzählen
* Liste der Massenspeichergeräte durchgehen
    * [Booten von Massenspeichergerät](msd.md)
* Gehen Sie durch die Ethernet-Geräteliste
    * [Booten von Ethernet](net.md)
