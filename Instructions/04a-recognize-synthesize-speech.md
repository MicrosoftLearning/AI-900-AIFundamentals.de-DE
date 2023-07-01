---
lab:
  title: Erkunden von Sprache
---

# Erkunden von Sprache

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free?azure-portal=true), in dem Sie über Administratorzugriff verfügen.

Um Software zu entwickeln, die gesprochene Sprache interpretieren und entsprechend reagieren kann, können Sie den Cognitive Service für **Speech** nutzen, der eine einfache Möglichkeit bietet, gesprochene Sprache in Text zu transkribieren und umgekehrt.

Angenommen, Sie möchten ein intelligentes Gerät entwickeln, das auf gesprochene Fragen wie „Wie spät ist es?“ antwortet. Als Antwort sollte die Ortszeit bereitgestellt werden.

Um die Fähigkeiten des Speech-Diensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell ausgeführt wird. Die gleichen Prinzipien und Funktionen gelten auch für reale Lösungen, wie Websites oder Smartphone-Apps.

## Erstellen einer *Cognitive Services*-Ressource

Sie können den Speech-Dienst nutzen, indem Sie entweder eine **Speech**Ressource oder eine **Cognitive Services**-Ressource erstellen.

Wenn dies noch nicht erfolgt ist, erstellen Sie eine **Cognitive Services**-Ressource in Ihrem Azure-Abonnement.

1. Öffnen Sie auf einer anderen Browserregisterkarte das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto an.

1. Klicken Sie auf die Schaltfläche **&#65291;Ressource erstellen**, suchen Sie nach *Cognitive Services*, und erstellen Sie eine **Cognitive Services**-Ressource mit den folgenden Einstellungen:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe mit einem eindeutigen Namen*.
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus.*
    - **Name**: *Geben Sie einen eindeutigen Namen ein*.
    - **Tarif**: Standard S0.
    - **Durch Aktivieren dieses Kontrollkästchens bestätige ich, dass ich die folgenden Bedingungen gelesen und verstanden habe**: Aktiviert.

1. Überprüfen und erstellen Sie die Ressource.

### Rufen Sie den Schlüssel und den Standort für Ihre Cognitive Services-Ressource ab.

1. Warten Sie, bis die Bereitstellung abgeschlossen ist. Wechseln Sie dann zu Ihrer Cognitive Services-Ressource, und klicken Sie auf der Seite **Übersicht** auf den Link zur Verwaltung der Schlüssel für den Dienst. Sie benötigen den Endpunkt und die Schlüssel, um von Clientanwendungen aus eine Verbindung mit Ihrer Cognitive Services-Ressource herzustellen.

1. Zeigen Sie die Seite **Schlüssel und Endpunkt** für Ihre Ressource an. Sie benötigen **Standort/Region** und **Schlüssel** für die Verbindung von Clientanwendungen.

## Ausführen von Cloud Shell

Um die Fähigkeiten des Speech-Diensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell in Azure ausgeführt wird.

1. Wählen Sie im Azure-Portal die Schaltfläche **[>_]** (*Cloud Shell*) oben auf der Seite rechts neben dem Suchfeld aus. Dadurch wird am unteren Rand des Portals ein Cloud Shell-Bereich geöffnet.

    ![Starten Sie Cloud Shell, indem Sie auf das Symbol rechts neben dem oberen Suchfeld klicken.](media/recognize-synthesize-speech/powershell-portal-guide-1.png)

1. Wenn Sie die Cloud Shell zum ersten Mal öffnen, werden Sie möglicherweise aufgefordert, die Art der Shell zu wählen, die Sie verwenden möchten (*Bash* oder *PowerShell*). Wählen Sie **PowerShell** aus. Wenn Sie diese Option nicht sehen, überspringen Sie den Schritt.  

1. Wenn Sie aufgefordert werden, Speicher für Ihre Cloud Shell zu erstellen, stellen Sie sicher, dass Ihr Abonnement angegeben ist, und wählen Sie **Speicher erstellen** aus. Warten Sie dann etwa eine Minute, bis der Speicher erstellt ist.

    ![Erstellen Sie einen Speicher, indem Sie auf „Bestätigen“ klicken.](media/recognize-synthesize-speech/powershell-portal-guide-2.png)

1. Vergewissern Sie sich, dass der oben links im Cloud Shell-Bereich angezeigte Shelltyp zu *PowerShell* gewechselt ist. Wenn *Bash* angezeigt wird, wechseln Sie über das Dropdownmenü zu *PowerShell*.

    ![So finden Sie das Dropdownmenü auf der linken Seite, um zu PowerShell zu wechseln](media/recognize-synthesize-speech/powershell-portal-guide-3.png)

1. Warten Sie, bis PowerShell gestartet wurde. Im Azure-Portal sollte der folgende Bildschirm angezeigt werden:  

    ![Warten Sie, bis PowerShell gestartet wurde.](media/recognize-synthesize-speech/powershell-prompt.png)

## Konfigurieren und Ausführen einer Clientanwendung

Nachdem Sie nun über ein benutzerdefiniertes Modell verfügen, können Sie eine einfache Clientanwendung ausführen, die den Speech-Dienst nutzt.

1. Geben Sie in der Befehlsshell den folgenden Befehl ein, um die Beispielanwendung herunterzuladen und in einem Ordner namens „ai-900“ zu speichern.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Tipp**: Wenn Sie diesen Befehl bereits in einem anderen Lab zum Klonen des Repositorys *ai-900* verwendet haben, können Sie diesen Schritt überspringen.

1. Die Dateien werden in einen Ordner namens **ai-900** heruntergeladen. Jetzt möchten wir alle Dateien in Ihrem Cloud Shell-Speicher anzeigen und mit ihnen arbeiten. Geben Sie den folgenden Befehl in die Shell ein:

     ```PowerShell
    code .
    ```

    Beachten Sie, dass sich dadurch ein Editor wie in der Abbildung unten öffnet:

    ![Der Code-Editor.](media/recognize-synthesize-speech/powershell-portal-guide-4.png)

1. Erweitern Sie im Bereich **Dateien** auf der linken Seite die Option **ai-900**, und wählen Sie **speaking-clock.ps1** aus. Diese Datei enthält Code, der den Speech-Dienst verwendet, um Sprache zu erkennen und zu synthetisieren:

    ![Editor, der den Code zur Nutzung des Speech-Diensts enthält](media/recognize-synthesize-speech/speaking-clock-code.png)

1. Machen Sie sich nicht zu viele Gedanken über die Details des Codes, wichtig ist, dass er die Region/den Standort und einen der Schlüssel für Ihre Cognitive Services-Ressource benötigt. Kopieren Sie diese von der Seite **Schlüssel und Endpunkte** für Ihre Ressource aus dem Azure-Portal, und fügen Sie sie in den Code-Editor ein. Ersetzen Sie hierbei die Platzhalterwerte **YOUR_KEY** und **YOUR_LOCATION**.

    > **Tipp**: Möglicherweise müssen Sie die Trennlinie verwenden, um den Bildschirmbereich anzupassen, während Sie mit den Bereichen **Schlüssel und Endpunkt** und **Editor** arbeiten.

    Nach dem Einfügen der Schlüssel- und Regions-/Standortwerte sollten die ersten Codezeilen etwa wie folgt aussehen:

    ```PowerShell
    $key = "1a2b3c4d5e6f7g8h9i0j...."
    $region="somelocation"
    ```

1. Verwenden Sie oben rechts im Editor-Bereich die Schaltfläche **...**, um das Menü zu öffnen, und wählen Sie **Speichern** aus, um Ihre Änderungen zu speichern. Öffnen Sie dann das Menü erneut, und wählen Sie **Editor schließen** aus.

    Die Beispielclientanwendung verwendet Ihren Speech-Dienst, um gesprochene Eingaben zu transkribieren und eine angemessene gesprochene Antwort zu synthetisieren. Eine reale Anwendung könnte die Eingabe von einem Mikrofon entgegennehmen und die Antwort an einen Lautsprecher senden, aber in diesem einfachen Beispiel verwenden wir eine vorab aufgezeichnete Eingabe in einer Audiodatei und speichern die Antwort in einer anderen Datei.

    Verwenden Sie den folgenden Videoplayer, um sich die Audioeingabe anzuhören, die die Anwendung verarbeiten wird:

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWMAvi" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>

1. Geben Sie im PowerShell-Bereich den folgenden Befehl ein, um den Code auszuführen:

    ```PowerShell
    cd ai-900
    ./speaking-clock.ps1
    ```

1. Überprüfen Sie die Ausgabe, die den Text „What time is it?“ erfolgreich erkannt und eine angemessene Antwort in einer Datei namens *output.wav* aufgezeichnet haben sollte.

    Verwenden Sie den folgenden Videoplayer, um sich die von der Anwendung generierte gesprochene Ausgabe anzuhören:

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWMSIU" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>

## Weitere Informationen

Diese einfache App zeigt nur einen Ausschnitt der Funktionalität des Speech-Dienstes. Weitere Informationen über die Möglichkeiten dieses Diensts finden Sie auf der Seite [Sprachdienste](https://azure.microsoft.com/services/cognitive-services/speech-services/).
