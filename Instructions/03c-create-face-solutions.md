---
lab:
  title: Erkunden der Gesichtserkennung
---

# Erkunden der Gesichtserkennung

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free?azure-portal=true), in dem Sie über Administratorzugriff verfügen.

Lösungen für maschinelles Sehen erfordern oft eine Lösung mit künstlicher Intelligenz (KI), um Gesichter von Personen zu erkennen. Nehmen wir z. B. an, das Einzelhandelsunternehmen Northwind Traders möchte herausfinden, wo sich die Kunden in einem Geschäft befinden, um sie bestmöglich zu bedienen. Eine Möglichkeit, dies zu erreichen, besteht darin, festzustellen, ob sich auf den Bildern Gesichter befinden. Wenn dies der Fall ist, werden die Koordinaten des Begrenzungsrahmens ermittelt, der die Gesichter umgibt.

Um die Fähigkeiten des Gesichtserkennungsdiensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell ausgeführt wird. Die gleichen Prinzipien und Funktionen gelten auch für reale Lösungen, wie Websites oder Smartphone-Apps.

## Erstellen einer *Cognitive Services*-Ressource

Sie können den Gesichtserkennungsdienst nutzen, indem Sie entweder eine Ressource für die **Gesichtserkennung** oder eine **Cognitive Services**-Ressource erstellen.

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

## Ausführen von Cloud Shell

Um die Fähigkeiten des Gesichtserkennungsdiensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell in Azure ausgeführt wird. 

1. Wählen Sie im Azure-Portal die Schaltfläche **[>_]** (*Cloud Shell*) oben auf der Seite rechts neben dem Suchfeld aus. Dadurch wird am unteren Rand des Portals ein Cloud Shell-Bereich geöffnet. 

    ![Starten Sie Cloud Shell, indem Sie auf das Symbol rechts neben dem oberen Suchfeld klicken.](media/create-face-solutions/powershell-portal-guide-1.png)

1. Wenn Sie die Cloud Shell zum ersten Mal öffnen, werden Sie möglicherweise aufgefordert, die Art der Shell zu wählen, die Sie verwenden möchten (*Bash* oder *PowerShell*). Wählen Sie **PowerShell** aus. Wenn Sie diese Option nicht sehen, überspringen Sie den Schritt.  

1. Wenn Sie aufgefordert werden, Speicher für Ihre Cloud Shell zu erstellen, stellen Sie sicher, dass Ihr Abonnement angegeben ist, und wählen Sie **Speicher erstellen** aus. Warten Sie dann etwa eine Minute, bis der Speicher erstellt ist.

    ![Erstellen Sie einen Speicher, indem Sie auf „Bestätigen“ klicken.](media/create-face-solutions/powershell-portal-guide-2.png)       

1. Vergewissern Sie sich, dass der oben links im Cloud Shell-Bereich angezeigte Shelltyp zu *PowerShell* gewechselt ist. Wenn *Bash* angezeigt wird, wechseln Sie über das Dropdownmenü zu *PowerShell*.

    ![So finden Sie das Dropdownmenü auf der linken Seite, um zu PowerShell zu wechseln](media/create-face-solutions/powershell-portal-guide-3.png) 

1. Warten Sie, bis PowerShell gestartet wurde. Im Azure-Portal sollte der folgende Bildschirm angezeigt werden:  

    ![Warten Sie, bis PowerShell gestartet wurde.](media/create-face-solutions/powershell-prompt.png)

## Konfigurieren und Ausführen einer Clientanwendung

Nachdem Sie nun über ein benutzerdefiniertes Modell verfügen, können Sie eine einfache Clientanwendung ausführen, die den Gesichtserkennungsdienst nutzt.

1. Geben Sie in der Befehlsshell den folgenden Befehl ein, um die Beispielanwendung herunterzuladen und in einem Ordner namens „ai-900“ zu speichern.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    > **Tipp**: Wenn Sie diesen Befehl bereits in einem anderen Lab zum Klonen des Repositorys *ai-900* verwendet haben, können Sie diesen Schritt überspringen.

1. Die Dateien werden in einen Ordner namens **ai-900** heruntergeladen. Jetzt möchten wir alle Dateien in Ihrem Cloud Shell-Speicher anzeigen und mit ihnen arbeiten. Geben Sie den folgenden Befehl in die Shell ein:

     ```PowerShell
    code .
    ```

    Beachten Sie, dass sich dadurch ein Editor wie in der Abbildung unten öffnet: 

    ![Der Code-Editor.](media/create-face-solutions/powershell-portal-guide-4.png) 

1. Erweitern Sie im Bereich **Dateien** auf der linken Seite die Option **ai-900**, und wählen Sie **find-faces.ps1** aus. Diese Datei enthält einen Code, der den Gesichtserkennungsdienst verwendet, um Gesichter in einem Bild zu erkennen und zu analysieren, wie hier gezeigt:

    ![Der Editor enthält Code zur Erkennung von Gesichtern in einem Bild.](media/create-face-solutions/find-faces-code.png)

1. Machen Sie sich nicht zu viele Gedanken über die Details des Codes, wichtig ist, dass er die Endpunkt-URL und einen der Schlüssel für Ihre Cognitive Services-Ressource benötigt. Kopieren Sie diese von der Seite **Schlüssel und Endpunkte** für Ihre Ressource aus dem Azure-Portal, und fügen Sie sie in den Code-Editor ein, wobei Sie die Platzhalterwerte **YOUR_KEY** und **YOUR_ENDPOINT** ersetzen.

    > **Tipp**: Möglicherweise müssen Sie die Trennlinie verwenden, um den Bildschirmbereich anzupassen, während Sie mit den Bereichen **Schlüssel und Endpunkt** und **Editor** arbeiten.

    Nach dem Einfügen der Schlüssel- und Endpunktwerte sollten die ersten beiden Codezeilen etwa wie folgt aussehen:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Verwenden Sie oben rechts im Editor-Bereich die Schaltfläche **...**, um das Menü zu öffnen, und wählen Sie **Speichern** aus, um Ihre Änderungen zu speichern. Öffnen Sie dann das Menü erneut, und wählen Sie **Editor schließen** aus.

    Die Beispielclientanwendung verwendet Ihren Gesichtserkennungsdienst, um das folgende Bild zu analysieren, das von einer Kamera im Northwind Traders Store aufgenommen wurde:

    ![Abbildung eines Elternteils, das die Kamera seines Mobiltelefons verwendet, um ein Bild eines Kinds in einem Geschäft aufzunehmen](media/create-face-solutions/store-camera-1.jpg)

1. Geben Sie im PowerShell-Bereich die folgenden Befehle ein, um den Code auszuführen:

    ```PowerShell
    cd ai-900
    ./find-faces.ps1 store-camera-1.jpg
    ```

1. Überprüfen Sie die zurückgegebenen Informationen, die die Position des Gesichts auf dem Bild umfassen. Die Position eines Gesichts wird durch die Koordinaten oben links und die Breite und Höhe eines *Begrenzungsrahmens* angegeben, wie hier gezeigt:

    ![Ein Bild einer Person mit umrandetem Gesicht.](media/create-face-solutions/store-camera-1-face.jpg)

    >**Hinweis**: Funktionen des Gesichterkennungsdiensts, die personenbezogene Merkmale zurückgeben, sind eingeschränkt. Einzelheiten dazu finden Sie unter https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/.

1. Versuchen wir es nun mit einem anderen Bild:

    ![Abbildung einer Person mit Einkaufskorb](media/create-face-solutions/store-camera-2.jpg)

    Geben Sie den folgenden Befehl ein, um das zweite Bild zu analysieren:

    ```PowerShell
    ./find-faces.ps1 store-camera-2.jpg
    ```

1. Überprüfen Sie die Ergebnisse der Gesichtsanalyse für das zweite Bild.

1. Versuchen wir ein weiteres:

    ![Abbildung einer Person mit Einkaufswagen](media/create-face-solutions/store-camera-3.jpg)

    Geben Sie den folgenden Befehl ein, um das dritte Bild zu analysieren:

    ```PowerShell
    ./find-faces.ps1 store-camera-3.jpg
    ```

1. Überprüfen Sie die Ergebnisse der Gesichtsanalyse für das dritte Bild.

## Weitere Informationen

Diese einfache App zeigt nur einen Teil der Möglichkeiten des Gesichtserkennungsdiensts. Weitere Informationen zu den Möglichkeiten dieses Diensts finden Sie auf der Seite für die [Gesichtserkennungs-API](https://azure.microsoft.com/services/cognitive-services/face/).
