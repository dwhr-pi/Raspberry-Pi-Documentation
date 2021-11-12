# Kernel

Der Raspberry Pi-Kernel ist in GitHub gespeichert und kann unter [github.com/raspberrypi/linux](https://github.com/raspberrypi/linux) eingesehen werden; es folgt hinter dem Haupt-[Linux-Kernel](https://github.com/torvalds/linux).

Der Haupt-Linux-Kernel wird ständig aktualisiert; wir nehmen Langzeit-Releases des Kernels, die auf der Titelseite erwähnt werden, und integrieren die Änderungen in den Raspberry-Pi-Kernel. Wir erstellen dann einen 'nächsten' Zweig, der eine instabile Portierung des Kernels enthält; nach umfangreichen Tests und Diskussionen schieben wir dies in den Hauptzweig.

- [Aktualisieren Ihres Kernels](update.md)
- [Erstellen eines neuen Kernels](building.md)
- [Kernel konfigurieren](configuring.md)
- [Anwenden von Patches auf den Kernel](patching.md)
- [Kernel-Header abrufen](headers.md)

## Code in den Kernel einbinden

Es gibt viele Gründe, warum Sie etwas in den Kernel einbauen möchten:

- Sie haben einen Raspberry Pi-spezifischen Code geschrieben, von dem alle profitieren sollen
- Sie haben einen generischen Linux-Kernel-Treiber für ein Gerät geschrieben und möchten, dass jeder ihn verwendet
- Sie haben einen generischen Kernel-Fehler behoben
- Sie haben einen Raspberry Pi-spezifischen Kernel-Fehler behoben

Zunächst sollten Sie das Linux-Repository forken und auf Ihrem Build-System klonen. Dies kann entweder auf dem Raspberry Pi oder auf einem Linux-Computer sein, den Sie zum Cross-Kompilieren verwenden. Anschließend können Sie Ihre Änderungen vornehmen, testen und in Ihren Fork übertragen.

Als nächstes, je nachdem, ob der Code Raspberry Pi-spezifisch ist oder nicht:

- Senden Sie für Pi-spezifische Änderungen oder Fehlerbehebungen einen Pull-Request an den Kernel.

- Für allgemeine Linux-Kernel-Änderungen (z. B. einen neuen Treiber) müssen diese zuerst im Upstream eingereicht werden. Sobald sie im Upstream eingereicht und akzeptiert wurden, senden Sie den Pull-Request und wir erhalten ihn.