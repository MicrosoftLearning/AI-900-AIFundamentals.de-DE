---
lab:
  title: Erkunden der optischen Zeichenerkennung
---

# <a name="explore-optical-character-recognition"></a>Erkunden der optischen Zeichenerkennung

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free?azure-portal=true), in dem Sie über Administratorzugriff verfügen.

Eine häufige Herausforderung beim maschinellen Sehen ist die Erkennung und Interpretation von Text in einem Bild. Diese Art der Verarbeitung wird oft als *optische Zeichenerkennung* (Optical Character Recognition, OCR) bezeichnet. Die Lese-API von Microsoft bietet Zugriff auf OCR-Funktionen. 

Um die Fähigkeiten der Lese-API zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in Cloud Shell ausgeführt wird. Die gleichen Prinzipien und Funktionen gelten auch für reale Lösungen, wie Websites oder Smartphone-Apps.

## <a name="use-the-computer-vision-service-to-read-text-in-an-image"></a>Verwenden des Diensts „Maschinelles Sehen“ zum Lesen von Text in einem Bild

Der kognitive Dienst für **maschinelles Sehen** bietet Unterstützung für OCR-Aufgaben, einschließlich:

- Einer **Lese**-API, die für größere Dokumente optimiert ist. Diese API wird asynchron verwendet und kann sowohl für gedruckten als auch für handschriftlichen Text verwendet werden.

## <a name="create-a-cognitive-services-resource"></a>Erstellen einer *Cognitive Services*-Ressource

Sie können den Dienst für maschinelles Sehen verwenden, indem Sie entweder eine Ressource für **maschinelles Sehen** oder eine **Cognitive Services**-Ressource erstellen.

Wenn dies noch nicht erfolgt ist, erstellen Sie eine **Cognitive Services**-Ressource in Ihrem Azure-Abonnement.

1. Öffnen Sie auf einer anderen Browserregisterkarte das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto an.

1. Klicken Sie auf die Schaltfläche **&#65291;Ressource erstellen**, suchen Sie nach *Cognitive Services*, und erstellen Sie eine **Cognitive Services**-Ressource mit den folgenden Einstellungen:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe mit einem eindeutigen Namen*.
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus.*
    - **Name**: *Geben Sie einen eindeutigen Namen ein*.
    - **Tarif**: Standard S0.
    - **Durch Aktivieren dieses Kontrollkästchens bestätige ich, dass ich die folgenden Bedingungen gelesen und verstanden habe**: Aktiviert.

1. Überprüfen und erstellen Sie die Ressource und warten Sie, bis die Bereitstellung abgeschlossen ist. Wechseln Sie dann zur bereitgestellten Ressource.

1. Zeigen Sie die Seite **Schlüssel und Endpunkt** für Ihre Cognitive Services-Ressource an. Sie benötigen den Endpunkt und die Schlüssel, um von Clientanwendungen aus eine Verbindung herzustellen.

## <a name="run-cloud-shell"></a>Ausführen von Cloud Shell

Um die Fähigkeiten des Custom Vision-Diensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell in Azure ausgeführt wird.

1. Wählen Sie im Azure-Portal die Schaltfläche **[>_]** (*Cloud Shell*) oben auf der Seite rechts neben dem Suchfeld aus. Dadurch wird am unteren Rand des Portals ein Cloud Shell-Bereich geöffnet. 

    ![Starten Sie Cloud Shell, indem Sie auf das Symbol rechts neben dem oberen Suchfeld klicken.](media/read-text-computer-vision/powershell-portal-guide-1.png)

1. Wenn Sie die Cloud Shell zum ersten Mal öffnen, werden Sie möglicherweise aufgefordert, die Art der Shell zu wählen, die Sie verwenden möchten (*Bash* oder *PowerShell*). Wählen Sie **PowerShell** aus. Wenn Sie diese Option nicht sehen, überspringen Sie den Schritt.  

1. Wenn Sie aufgefordert werden, Speicher für Ihre Cloud Shell zu erstellen, stellen Sie sicher, dass Ihr Abonnement angegeben ist, und wählen Sie **Speicher erstellen** aus. Warten Sie dann etwa eine Minute, bis der Speicher erstellt ist.

    ![Erstellen Sie einen Speicher, indem Sie auf „Bestätigen“ klicken.](media/read-text-computer-vision/powershell-portal-guide-2.png)

1. Vergewissern Sie sich, dass der oben links im Cloud Shell-Bereich angezeigte Shelltyp zu *PowerShell* gewechselt ist. Wenn *Bash* angezeigt wird, wechseln Sie über das Dropdownmenü zu *PowerShell*.

    ![So finden Sie das Dropdownmenü auf der linken Seite, um zu PowerShell zu wechseln](media/read-text-computer-vision/powershell-portal-guide-3.png) 

1. Warten Sie, bis PowerShell gestartet wurde. Im Azure-Portal sollte der folgende Bildschirm angezeigt werden:  

    ![Warten Sie, bis PowerShell gestartet wurde.](media/read-text-computer-vision/powershell-prompt.png) 

## <a name="configure-and-run-a-client-application"></a>Konfigurieren und Ausführen einer Clientanwendung

Nachdem Sie nun über ein benutzerdefiniertes Modell verfügen, können Sie eine einfache Clientanwendung ausführen, die den OCR-Dienst nutzt.

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

    ![Der Code-Editor.](media/read-text-computer-vision/powershell-portal-guide-4.png)

1. Erweitern Sie im Bereich **Dateien** auf der linken Seite die Option **ai-900**, und wählen Sie **ocr.ps1** aus. Diese Datei enthält einen Code, der den Dienst für maschinelles Sehen verwendet, um Text in einem Bild zu erkennen und zu analysieren, wie hier gezeigt:

    ![Der Editor enthält Code zur Analyse von Text in Bildern.](media/read-text-computer-vision/ocr-code.png)

1. Machen Sie sich nicht zu viele Gedanken über die Details des Codes, wichtig ist, dass er die Endpunkt-URL und einen der Schlüssel für Ihre Cognitive Services-Ressource benötigt. Kopieren Sie diese von der Seite **Schlüssel und Endpunkte** für Ihre Ressource aus dem Azure-Portal, und fügen Sie sie in den Code-Editor ein, wobei Sie die Platzhalterwerte **YOUR_KEY** und **YOUR_ENDPOINT** ersetzen.

    > **Tipp**: Möglicherweise müssen Sie die Trennlinie verwenden, um den Bildschirmbereich anzupassen, während Sie mit den Bereichen **Schlüssel und Endpunkt** und **Editor** arbeiten.

    Nach dem Einfügen der Schlüssel- und Endpunktwerte sollten die ersten beiden Codezeilen etwa wie folgt aussehen:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Verwenden Sie oben rechts im Editor-Bereich die Schaltfläche **...**, um das Menü zu öffnen, und wählen Sie **Speichern** aus, um Ihre Änderungen zu speichern. Öffnen Sie dann das Menü erneut, und wählen Sie **Editor schließen** aus. Nachdem Sie nun den Schlüssel und den Endpunkt eingerichtet haben, können Sie Ihre Cognitive Services-Ressource verwenden, um Text aus einem Bild zu extrahieren.

    Verwenden wir nun die **Lese**-API. In diesem Fall handelt es sich um ein Werbebild für das fiktive Einzelhandelsunternehmen Northwind Traders, das etwas Text enthält.

    Die Beispielclientanwendung analysiert das folgende Bild:

    ![Bild einer Werbung für das Lebensmittelgeschäft Northwind Traders.](media/read-text-computer-vision/advert.jpg)

1. Geben Sie im PowerShell-Bereich die folgenden Befehle ein, um den Code zum Lesen des Texts auszuführen:

    ```PowerShell
    cd ai-900
    ./ocr.ps1 advert.jpg
    ```

1. Überprüfen Sie die Details auf dem Bild. Der im Bild gefundene Text ist in einer hierarchischen Struktur von Regionen, Zeilen und Wörtern organisiert, und der Code liest diese, um die Ergebnisse abzurufen.

    Beachten Sie, dass die Position des Texts durch die Koordinaten oben links und die Breite und Höhe eines * Begrenzungsrahmens* angegeben wird, wie hier gezeigt:

    ![Eine Abbildung des Texts, der auf dem Bild umrandet ist.](media/read-text-computer-vision/lab-05-bounding-boxes.png)

1. Versuchen wir es nun mit einem anderen Bild:

    ![Ein Bild eines getippten Briefs.](media/read-text-computer-vision/letter.jpg)

    Geben Sie den folgenden Befehl ein, um das zweite Bild zu analysieren:

    ```PowerShell
    ./ocr.ps1 letter.jpg
    ```

1. Überprüfen Sie die Ergebnisse der Analyse für das zweite Bild. Außerdem sollten der Text und die Begrenzungsrahmen des Texts zurückgegeben werden.

## <a name="learn-more"></a>Weitere Informationen

Diese einfache App veranschaulicht nur einige der OCR-Funktionen des Diensts für maschinelles Sehen. Weitere Informationen über die Möglichkeiten dieses Diensts finden Sie auf der Seite für die [OCR](https://docs.microsoft.com/azure/cognitive-services/computer-vision/overview-ocr).
