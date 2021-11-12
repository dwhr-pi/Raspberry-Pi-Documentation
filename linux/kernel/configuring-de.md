# Kernel konfigurieren

Der Linux-Kernel ist hochgradig konfigurierbar; fortgeschrittene Benutzer möchten möglicherweise die Standardkonfiguration ändern, um sie an ihre Bedürfnisse anzupassen, z. B. ein neues oder experimentelles Netzwerkprotokoll aktivieren oder die Unterstützung für neue Hardware aktivieren.

Die Konfiguration erfolgt am häufigsten über die Schnittstelle `make menuconfig`. Alternativ können Sie Ihre `.config`-Datei manuell ändern, aber dies kann für neue Benutzer schwieriger sein.

## Konfiguration des Kernels vorbereiten

Das `menuconfig`-Tool benötigt die `ncurses`-Entwicklungsheader, um richtig zu kompilieren. Diese können mit folgendem Befehl installiert werden:

```
$ sudo apt install libncurses5-dev
```

Außerdem müssen Sie Ihre Kernel-Quellen herunterladen und vorbereiten, wie in der [Build-Anleitung](building.md#choosing_sources) beschrieben. Stellen Sie insbesondere sicher, dass Sie die [Standardkonfiguration](building.md#default_configuration) installiert haben.

## Verwenden von menuconfig

Sobald Sie alles eingerichtet und einsatzbereit haben, können Sie das Dienstprogramm `menuconfig` wie folgt kompilieren und ausführen:

```
$ make menuconfig
```

Wenn Sie einen 32-Bit-Kernel kreuzkompilieren:

```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig
```

Oder, wenn Sie einen 64-Bit-Kernel querkompilieren:

```
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig
```

Das Dienstprogramm `menuconfig` verfügt über eine einfache Tastaturnavigation. Nach einer kurzen Zusammenstellung wird Ihnen eine Liste von Untermenüs mit allen konfigurierbaren Optionen angezeigt; Es gibt eine Menge, also nehmen Sie sich Zeit, sie durchzulesen und sich kennenzulernen.

Verwenden Sie die Pfeiltasten zum Navigieren, die Eingabetaste zum Aufrufen eines Untermenüs (gekennzeichnet durch `--->`), Escape zweimal, um eine Ebene nach oben zu gehen oder zu verlassen, und die Leertaste, um den Status einer Option zu durchlaufen. Einige Optionen haben mehrere Auswahlmöglichkeiten. In diesem Fall werden sie als Untermenü angezeigt und die Eingabetaste wählt eine Option aus. Sie können bei den meisten Einträgen `h` drücken, um Hilfe zu dieser speziellen Option oder diesem Menü zu erhalten.

Widerstehen Sie der Versuchung, viele Dinge beim ersten Versuch zu aktivieren oder zu deaktivieren; Es ist relativ einfach, Ihre Konfiguration zu zerstören. Fangen Sie also klein an und machen Sie sich mit dem Konfigurations- und Build-Prozess vertraut.

## Konfigurationen verlassen, speichern und laden

Wenn Sie die gewünschten Änderungen vorgenommen haben, drücken Sie Escape, bis Sie aufgefordert werden, Ihre neue Konfiguration zu speichern. Standardmäßig wird dies in der Datei `.config` gespeichert. Sie können Konfigurationen speichern und laden, indem Sie diese Datei herumkopieren.