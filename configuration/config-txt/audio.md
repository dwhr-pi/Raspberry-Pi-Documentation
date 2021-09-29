# Onboard-Analog-Audio-Optionen in config.txt (3,5-mm-Klinke)

Der integrierte Audioausgang verwendet Konfigurationsoptionen, um die Art und Weise zu ändern, wie das analoge Audio gesteuert wird und ob einige Firmware-Funktionen aktiviert sind oder nicht.

##disable_audio_dither

Standardmäßig wird ein 1.0LSB-Dither auf den Audiostream angewendet, wenn er an den analogen Audioausgang geleitet wird. Dies kann in einigen Situationen ein hörbares "Zischen" im Hintergrund erzeugen, zum Beispiel wenn die ALSA-Lautstärke auf einen niedrigen Pegel eingestellt ist. Setzen Sie `disable_audio_dither` auf `1`, um die Dither-Anwendung zu deaktivieren.

## enable_audio_dither

Audio-Dither (siehe disable_audio_dither oben) wird normalerweise deaktiviert, wenn die Audio-Samples größer als 16 Bit sind. Setzen Sie diese Option auf „1“, um die Verwendung von Dithering für alle Bittiefen zu erzwingen.

##pwm_sample_bits

Der Befehl `pwm_sample_bits` passt die Bittiefe der analogen Audioausgabe an. Die Standardbittiefe ist '11'. Die Auswahl von Bittiefen unter "8" führt zu nicht funktionierendem Audio, da Einstellungen unter "8" dazu führen, dass eine PLL-Frequenz zu niedrig ist, um unterstützt zu werden. Dies ist im Allgemeinen nur nützlich, um zu demonstrieren, wie sich die Bittiefe auf das Quantisierungsrauschen auswirkt.




*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses/bis-sa/3.0/) geteilt wird.*