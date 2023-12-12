---
lab:
  title: Erkunden der Formularerkennung
---

# Erkunden der Formularerkennung

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free?azure-portal=true), in dem Sie über Administratorzugriff verfügen.

Im Bereich der künstlichen Intelligenz (KI) wird die optische Zeichenerkennung (OCR) üblicherweise zum Lesen von gedruckten oder handgeschriebenen Dokumenten verwendet. Häufig wird der Text einfach aus den Dokumenten in ein Format extrahiert, das für die weitere Verarbeitung oder Analyse verwendet werden kann.

Ein komplexeres OCR-Szenario ist die Extraktion von Informationen aus Formularen, z. B. Bestellungen oder Rechnungen, mit einem semantischen Verständnis dessen, was die Felder im Formular darstellen. Der **Formularerkennungsdienst** ist speziell für diese Art von KI-Problem konzipiert.

Die Formularerkennung verwendet Machine Learning-Modelle, die darauf trainiert sind, Text aus Bildern von Rechnungen, Belegen usw. zu extrahieren. Während andere Modelle für maschinelles Sehen entsprechenden Text erfassen können, erfasst die Formularerkennung auch die Struktur des Texts, z. B. Schlüssel-Wert-Paare und Informationen in Tabellen. Auf diese Weise müssen Sie die Einträge aus einem Formular nicht mehr manuell in eine Datenbank eingeben, sondern können die Beziehungen zwischen den Texten aus der Originaldatei automatisch erfassen. 

Um die Fähigkeiten des Formularerkennungsdiensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell ausgeführt wird. Die gleichen Prinzipien und Funktionen gelten auch für reale Lösungen, wie Websites oder Smartphone-Apps.

## Erstellen einer *Azure KI Services*-Ressource

Sie können den Formularerkennungsdienst verwenden, indem Sie entweder eine Ressource für die **Formularerkennung** oder eine Ressource für **Azure KI Services** erstellen.

Wenn dies noch nicht erfolgt ist, erstellen Sie eine **Azure KI Services**-Ressource in Ihrem Azure-Abonnement.

1. Öffnen Sie auf einer anderen Browserregisterkarte das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto an.

1. Klicken Sie auf die Schaltfläche **＋Ressource erstellen** und suchen Sie nach *Azure KI Services*. Wählen Sie **Erstellen** eines **Azure KI Services**-Plans aus. Sie werden zu einer Seite weitergeleitet, um eine Azure KI Services-Ressource zu erstellen. Konfigurieren Sie sie mit den folgenden Einstellungen:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe mit einem eindeutigen Namen*.
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus.*
    - **Name**: *Geben Sie einen eindeutigen Namen ein*.
    - **Tarif**: Standard S0.
    - **Durch Aktivieren dieses Kontrollkästchens bestätige ich, dass ich die folgenden Bedingungen gelesen und verstanden habe**: Aktiviert.

1. Überprüfen und erstellen Sie die Ressource und warten Sie, bis die Bereitstellung abgeschlossen ist. Wechseln Sie dann zur bereitgestellten Ressource.

1. Zeigen Sie die Seite **Schlüssel und Endpunkt** für Ihre Azure KI Services-Ressource an. Sie benötigen den Endpunkt und die Schlüssel, um von Clientanwendungen aus eine Verbindung herzustellen.

## Ausführen von Cloud Shell

Um die Fähigkeiten des Formularerkennungsdiensts zu testen, verwenden wir eine einfache Befehlszeilenanwendung, die in der Cloud Shell in Azure ausgeführt wird. 

1. Wählen Sie im Azure-Portal die Schaltfläche **[>_]** (*Cloud Shell*) oben auf der Seite rechts neben dem Suchfeld aus. Dadurch wird am unteren Rand des Portals ein Cloud Shell-Bereich geöffnet. 

    ![Starten Sie Cloud Shell, indem Sie auf das Symbol rechts neben dem oberen Suchfeld klicken.](media/analyze-receipts/powershell-portal-guide-1.png)

1. Wenn Sie die Cloud Shell zum ersten Mal öffnen, werden Sie möglicherweise aufgefordert, die Art der Shell zu wählen, die Sie verwenden möchten (*Bash* oder *PowerShell*). Wählen Sie **PowerShell** aus. Wenn Sie diese Option nicht sehen, überspringen Sie den Schritt.  

1. Wenn Sie aufgefordert werden, Speicher für Ihre Cloud Shell zu erstellen, stellen Sie sicher, dass Ihr Abonnement angegeben ist, und wählen Sie **Speicher erstellen** aus. Warten Sie dann etwa eine Minute, bis der Speicher erstellt ist.

    ![Erstellen Sie einen Speicher, indem Sie auf „Bestätigen“ klicken.](media/analyze-receipts/powershell-portal-guide-2.png)

1. Vergewissern Sie sich, dass der oben links im Cloud Shell-Bereich angezeigte Shelltyp zu *PowerShell* gewechselt ist. Wenn *Bash* angezeigt wird, wechseln Sie über das Dropdownmenü zu *PowerShell*.

    ![So finden Sie das Dropdownmenü auf der linken Seite, um zu PowerShell zu wechseln](media/analyze-receipts/powershell-portal-guide-3.png) 

1. Warten Sie, bis PowerShell gestartet wurde. Im Azure-Portal sollte der folgende Bildschirm angezeigt werden:  

    ![Warten Sie, bis PowerShell gestartet wurde.](media/analyze-receipts/powershell-prompt.png) 

## Konfigurieren und Ausführen einer Clientanwendung

Nachdem Sie nun über ein benutzerdefiniertes Modell verfügen, können Sie eine einfache Clientanwendung ausführen, die den Formularerkennungsdienst nutzt.

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

    ![Der Code-Editor.](media/analyze-receipts/powershell-portal-guide-4.png)

1. Erweitern Sie im Bereich **Dateien** auf der linken Seite die Option **ai-900**, und wählen Sie **form-recognizer.ps1** aus. Diese Datei enthält Code, der den Formularerkennungsdienst verwendet, um die Felder eines Belegs zu analysieren, wie hier gezeigt:

    ![Der Editor, der den Code für die Analyse der Felder eines Belegs enthält.](media/analyze-receipts/recognize-receipt-code.png)

1. Machen Sie sich nicht zu viele Gedanken über die Details des Codes. Wichtig ist, dass er die Endpunkt-URL und einen der Schlüssel für Ihre Azure KI Services-Ressource benötigt. Kopieren Sie diese von der Seite **Schlüssel und Endpunkte** für Ihre Ressource aus dem Azure-Portal, und fügen Sie sie in den Code-Editor ein, wobei Sie die Platzhalterwerte **YOUR_KEY** und **YOUR_ENDPOINT** ersetzen.

    > **Tipp**: Möglicherweise müssen Sie die Trennlinie verwenden, um den Bildschirmbereich anzupassen, während Sie mit den Bereichen **Schlüssel und Endpunkt** und **Editor** arbeiten.

    Nach dem Einfügen der Schlüssel- und Endpunktwerte sollten die ersten beiden Codezeilen etwa wie folgt aussehen:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Verwenden Sie oben rechts im Editor-Bereich die Schaltfläche **...**, um das Menü zu öffnen, und wählen Sie **Speichern** aus, um Ihre Änderungen zu speichern. Öffnen Sie dann das Menü erneut, und wählen Sie **Editor schließen** aus. Nachdem Sie nun den Schlüssel und den Endpunkt eingerichtet haben, können Sie Ihre Ressource verwenden, um die Felder eines Belegs zu analysieren. In diesem Fall werden Sie das integrierte Modell der Formularerkennung verwenden, um einen Beleg für das fiktive Einzelhandelsunternehmen Northwind Traders zu analysieren.

    Die Beispielclientanwendung analysiert das folgende Bild:

    ![Dies ist ein Bild eines Belegs.](media/analyze-receipts/receipt.jpg)

1. Geben Sie im PowerShell-Bereich die folgenden Befehle ein, um den Code zum Lesen des Texts auszuführen:

    ```PowerShell
    cd ai-900
    ./form-recognizer.ps1
    ```

1. Überprüfen Sie die zurückgegebenen Ergebnisse. Vergewissern Sie sich, dass die Formularerkennung in der Lage ist, die Daten im Formular zu interpretieren und die Adresse und Telefonnummer des Händlers, das Datum und die Uhrzeit der Transaktion sowie die Einzelposten, die Zwischensumme, die Steuer und die Gesamtbeträge ordnungsgemäß zu identifizieren.

## Weitere Informationen

Diese einfache App veranschaulicht nur einige der Funktionen der Formularerkennung des Diensts für maschinelles Sehen. Weitere Informationen über die Möglichkeiten dieses Diensts finden Sie auf der Seite für die [Formularerkennung](https://docs.microsoft.com/azure/applied-ai-services/form-recognizer/overview).

