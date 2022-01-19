# GPIO in Scratch 2

## Einstieg

Öffnen Sie das Bedienfeld **Weitere Blöcke**, klicken Sie auf **Erweiterung hinzufügen** und wählen Sie **Pi GPIO** aus. Sie sollten dann zwei neue Blöcke sehen:

![](images/scratch2-gpio.png)

Sie können diese beiden violetten Blöcke verwenden, um Ausgangspins zu steuern oder Eingangspins zu lesen, indem Sie die Pin-Nummer in das Feld eingeben oder eine Variable verwenden, die die Pin-Nummer enthält:

![](images/scratch2-gpio-pin-number.png)

![](images/scratch2-gpio-variable.png)

## LED

Um eine an GPIO10 angeschlossene LED zu steuern, können Sie diese Blöcke verwenden:

![](images/led-blink.png)

Klicken Sie auf die grüne Flagge, und die LED blinkt wiederholt ein und aus.

## Taste

Um den Zustand einer mit GPIO10 verbundenen Taste zu lesen, können Sie diese Blöcke verwenden:

![](images/button.png)

Beachten Sie, dass die Taste nach oben gezogen wird, sodass der GPIO-Pin "high" anzeigt, wenn er nicht gedrückt wird, und "low", wenn er gedrückt wird.

## Taste + LED

Um die LED und die Taste miteinander zu verbinden, können Sie diese Blöcke verwenden:

![](images/led-button.png)
