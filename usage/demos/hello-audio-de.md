# DIESE SEITE IST JETZT VERALTET

# Hallo Audio

Diese Demo demonstriert nur die Audioausgabe. Es spielt eine Sinuswelle, die eine Art „WOO WOO WOO“-Sound erzeugt.

```bash
cd ..
cd hello_audio
ls
```

Beachten Sie die grüne `.bin`-Datei? Starte es. Verstehst du das jetzt?

```bash
./hello_audio.bin
```

Dadurch wird der Ton über die Kopfhörerbuchse des Pi wiedergegeben. Wenn Sie einen HDMI-Monitor verwenden, können Sie ihn über HDMI ausgeben, indem Sie dem Befehl eine "1" hinzufügen:

```bash
./hello_audio.bin 1
```

Die Demo läuft für immer, bis Sie sie beenden. Um die Demo zu verlassen, drücken Sie `Strg + C`.
