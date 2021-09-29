# Kameraeinstellungen in config.txt

##disable_camera_led

Das Setzen von `disable_camera_led` auf `1` verhindert, dass die rote Kamera-LED beim Aufnehmen von Videos oder Standbildern aufleuchtet. Dies ist nützlich, um Reflexionen zu vermeiden, wenn die Kamera beispielsweise auf ein Fenster gerichtet ist.

Wenn `awb_auto_is_greyworld`auf `1` gesetzt wird, können Bibliotheken oder Anwendungen, die die Option greyworld intern nicht unterstützen, gültige Bilder und Videos mit NoIR-Kameras aufnehmen. Es schaltet den "auto" awb-Modus um, um den "greyworld" -Algorithmus zu verwenden. Dies sollte nur für NoIR-Kameras benötigt werden oder wenn der IR-Filter der High Quality-Kamera entfernt wurde. (siehe [hier](../../hardware/camera/hqcam_filter_removal.md).


*Dieser Artikel verwendet Inhalte der eLinux-Wiki-Seite [RPiconfig](http://elinux.org/RPiconfig), die unter der [Creative Commons Attribution-ShareAlike 3.0 Unported license](http://creativecommons.org/licenses/bis-sa/3.0/) geteilt wird.*