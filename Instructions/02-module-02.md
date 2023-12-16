---
lab:
  title: Erkunden des automatisierten maschinellen Lernens in Azure ML
---

# Erkunden des automatisierten maschinellen Lernens in Azure ML

In dieser Übung verwenden Sie das Feature für automatisiertes maschinelles Lernen in Azure Machine Learning, um ein Machine Learning-Modell zu trainieren und auszuwerten. Anschließend stellen Sie das trainierte Modell bereit und testen es.

Diese Übung dauert ca. **30** Minuten.

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free), in dem Sie über Administratorzugriff verfügen.

## Erstellen eines Azure Machine Learning-Arbeitsbereichs

Um Azure Machine Learning verwenden zu können, müssen Sie einen Azure Machine Learning-Arbeitsbereich in Ihrem Azure-Abonnement bereitstellen. Anschließend können Sie Azure Machine Learning Studio verwenden, um mit den Ressourcen in Ihrem Arbeitsbereich zu arbeiten.

> **Tipp**: Wenn Sie bereits über einen Azure Machine Learning-Arbeitsbereich verfügen, können Sie diesen verwenden und mit der nächsten Aufgabe fortfahren.

1. Melden Sie sich mit Ihren Microsoft-Anmeldeinformationen beim [Azure-Portal](https://portal.azure.com) in `https://portal.azure.com` an.

1. Klicken Sie auf **＋Ressource erstellen**, suchen Sie nach *Machine Learning*, und erstellen Sie eine neue **Azure Machine Learning**-Ressource mit den folgenden Einstellungen:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine Ressourcengruppe aus*.
    - **Name**: *Geben Sie einen eindeutigen Namen für den Arbeitsbereich ein.*
    - **Region**: *Wählen Sie die geografisch nächstgelegene Region aus.*
    - **Speicherkonto**: *Für Ihren Arbeitsbereich wird standardmäßig ein neues Speicherkonto erstellt*.
    - **Schlüsseltresor**: *Für Ihren Arbeitsbereich wird standardmäßig ein neuer Schlüsseltresor erstellt*.
    - **Application Insights**: *Für Ihren Arbeitsbereich wird standardmäßig eine neue Application Insights-Ressource erstellt*.
    - **Containerregistrierung**: Keine (*wird automatisch erstellt, wenn Sie das erste Mal ein Modell in einem Container bereitstellen*).

1. Klicken Sie auf**Überprüfen + erstellen** und dann auf **Erstellen**. Warten Sie, bis Ihr Arbeitsbereich erstellt wurde (dies kann einige Minuten dauern), und wechseln Sie dann zur bereitgestellten Ressource.

1. Wählen Sie **Studio starten** aus (oder öffnen Sie eine neue Browserregisterkarte. Navigieren Sie dann zu [https://ml.azure.com](https://ml.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto bei Azure Machine Learning Studio an). Schließen Sie alle angezeigten Nachrichten.

1. In Azure Machine Learning Studio sollte Ihr neu erstellter Arbeitsbereich angezeigt werden. Andernfalls wählen Sie im linken Menü **Alle Arbeitsbereiche** aus, und wählen Sie dann den soeben erstellten Arbeitsbereich aus.

## Aktivieren von Previewfunktionen

Einige Funktionen von Azure Machine Learning befinden sich in der Vorschau und müssen in Ihrem Arbeitsbereich explizit aktiviert werden.

1. Klicken Sie in Azure Machine Learning Studio auf **Previewfunktionen verwalten** (Lautsprechersymbol - &#128363;).

    ![Screenshot: Schaltfläche „Previewfunktionen verwalten“ im Menü.](../instructions/media/use-automated-machine-learning/severless-compute-1.png)

1. Aktivieren Sie die folgende Previewfunktion:

    - *Geführte Benutzeroberfläche zum Übermitteln von Trainingsaufträgen mit serverlosem Computing*

## Verwenden von automatisiertem maschinellem Lernen zum Trainieren eines Modells

Automatisiertes maschinelles Lernen ermöglicht es Ihnen, mehrere Algorithmen und Parameter zu testen, um mehrere Modelle zu trainieren und das beste Modell für Ihre Daten zu identifizieren. In dieser Übung verwenden Sie ein Dataset mit Verlaufsdetails zu Fahrradvermietungen, um ein Modell zu trainieren, das basierend auf saisonalen und meteorologischen Merkmalen die Anzahl der Fahrradvermietungen vorhersagt, die an einem bestimmten Tag zu erwarten sind.

> **Quellenangaben**: *Die in dieser Übung verwendeten Daten werden von [Capital Bikeshare](https://www.capitalbikeshare.com/system-data) abgeleitet und in Übereinstimmung mit der [Lizenzvereinbarung](https://www.capitalbikeshare.com/data-license-agreement) für veröffentlichte Daten verwendet.*


1. Zeigen Sie in [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) die Seite **Automated ML** (Automatisiertes ML) (unter **Dokumenterstellung**) an.

1. Erstellen Sie einen neuen automatisierten ML-Auftrag mit den folgenden Einstellungen, und verwenden Sie nach Bedarf **Weiter**, um die Benutzeroberfläche zu durchlaufen:

    **Grundeinstellungen**:

    - **Auftragsname**: mslearn-bike-automl
    - **Neuer Experimentname:** mslearn-bike-rental
    - **Beschreibung**: Automatisiertes maschinelles Lernen für die Vorhersage des Fahrradverleihs
    - **Tags**: *keine*

   **Vorgangsart und Daten**:

    - **Tasktyp auswählen**: Regression
    - **Dataset auswählen**: Erstellen Sie ein neues Dataset mit den folgenden Einstellungen:
        - **Datentyp**:
            - **Name:** bike-rentals
            - **Beschreibung**: Historische Daten zum Fahrradverleih
            - **Typ**: Tabellarisch
        - **Datenquelle**:
            - Wählen Sie **Aus Webdateien** aus
        - **Web-URL:**
            - **Web-URL:** `https://aka.ms/bike-rentals`
            - **Skip data validation** (Datenüberprüfung überspringen): *Nicht auswählen*
        - **Einstellungen**:
            - **Dateiformat:** Zeichengetrennt
            - **Trennzeichen:** Komma
            - **Codierung:** UTF-8
            - **Spaltenüberschriften**: Nur erste Datei enthält Header
            - **Zeilen überspringen:** Keine
            - **Dataset contains multi-line data** (Dataset enthält mehrzeilige Daten): *Nicht auswählen*
        - **Schema:**
            - Alle Spalten einschließen außer **Pfad**
            - Überprüfen der automatisch erkannten Typen

        Wählen Sie das Dataset **bike-rentals** aus, nachdem Sie es erstellt haben.

    **Task-Einstellungen**:

    - **Tasktyp**: Regression
    - **Dataset:** bike-rentals
    - **Zielspalte**: Verleihe (Integer)
    - **Zusätzliche Konfigurationseinstellungen**:
        - **Primäre Metrik**: Wurzel der mittleren Fehlerquadratsumme
        - **Bestes Modell erklären**: *Nicht ausgewählt*
        - **Alle unterstützten Modelle verwenden**: <u>Nicht</u> ausgewählt. *Sie beschränken den Auftrag darauf, nur einige bestimmte Algorithmen auszuprobieren*.
        - **Zulässige Modelle**: *Wählen Sie nur **RandomForest** und **LightGBM** aus. Normalerweise sollten Sie so viele Modelle wie möglich ausprobieren, aber jedes hinzugefügte Modell verlängert die Zeitspanne, die zum Ausführen des Auftrags benötigt wird*.
    - **Grenzwerte**: *Erweitern Sie diesen Abschnitt*
        - **Max. Testversionen**: 3
        - **Maximale Anzahl gleichzeitiger Testversionen**: 3
        - **Maximale Knotenanzahl**: 3
        - **Metrischer Bewertungsschwellenwert**: 0,85 (*sodass wenn ein Modell eine normalisierte Wurzel der mittleren Fehlerquadratsumme von 0,085 oder weniger erreicht, der Auftrag beendet wird*.)
        - **Timeout**: 15
        - **Iterationstimeout**: 5
        - **Vorzeitige Beendigung aktivieren**: *Ausgewählt*
    - **Validierung und Test**:
        - **Validierungstyp**: Aufteilung der Train-Validation
        - **Prozentsatz der Validierungsdaten**: 10
        - **Testdataset**: Keins

    **Compute:**

    - **Computetyp auswählen**: Serverlos
    - **VM-Typ:** CPU
    - **VM-Dienstebene**: Dediziert.
    - **VM-Größe**: Standard_DS3_V2
    - **Anzahl von Instanzen**: 1

1. Übermitteln des Trainingsauftrags Wird automatisch gestartet.

1. Warten Sie auf den Abschluss des Auftrags. Dies kann einige Zeit in Anspruch nehmen und ist möglicherweise ein guter Zeitpunkt für eine Kaffeepause.

## Überprüfen des besten Modells

Wenn der Auftrag für automatisiertes maschinelles Lernen abgeschlossen ist, können Sie das am besten trainierte Modell überprüfen.

1. Beachten Sie auf der Registerkarte **Übersicht** des automatisierten ML-Auftrags die Zusammenfassung des besten Modells.
    ![Screenshot: Zusammenfassung des besten Modells des automatisierten Machine Learning-Auftrags mit einem Rahmen um den Algorithmusnamen.](media/use-automated-machine-learning/complete-run.png)

    > **Hinweis** Möglicherweise wird eine Meldung „Warnung: Vom Benutzer angegebene Exitbewertung wurde erreicht...“ unter dem Status angezeigt. Dies ist eine erwartete Meldung. Fahren Sie mit dem nächsten Schritt fort.
  
1. Wählen Sie den Text unter **Algorithmusname**, um das beste Modell und seine Details anzuzeigen.

1. Klicken Sie auf die Registerkarte **Metriken**, und wählen Sie die Diagramme **residuals** und **predicted_true** aus, wenn diese nicht bereits ausgewählt sind. 

    Überprüfen Sie die Diagramme, die die Leistung des Modells anzeigen. Das **Residualwertediagramm** zeigt die *Residualwerte* (die Unterschiede zwischen vorhergesagten und tatsächlichen Werten) als Histogramm an. Das Diagramm **predicted_true** vergleicht die vorhergesagten Werte mit den tatsächlichen Werten. 

## Bereitstellen und Testen des Modells

1. Wählen Sie auf der Registerkarte **Modell** für das beste Modell, das von Ihrem Auftrag für automatisiertes maschinelles Lernen trainiert wurde, die Option **Bereitstellen** aus und verwenden Sie die Option **Webdienst**, um das Modell mit den folgenden Einstellungen bereitzustellen:
    - **Name:** predict-rentals
    - **Beschreibung:** Vorhersage Fahrradvermietung
    - **Computetyp:** Azure Container Instances
    - **Authentifizierung aktivieren**: *ausgewählt*

1. Warten Sie, bis die Bereitstellung gestartet wurde. Dieser Vorgang kann einige Sekunden in Anspruch nehmen. Der **Bereitstellungsstatus** für den Endpunkt **predict-rentals** wird im Hauptteil der Seite als *Wird ausgeführt* angegeben.
1. Warten Sie, bis der **Bereitstellungsstatus** in *Erfolgreich* geändert wird. Dies kann 5-10 Minuten dauern.

## Testen des bereitgestellten Diensts

Jetzt können Sie den bereitgestellten Dienst testen.

1. Wählen Sie in Azure Machine Learning Studio im Menü auf der linken Seite **Endpunkte** aus, und öffnen Sie den Echtzeitendpunkt **predict-rentals**.

1. Zeigen Sie auf der Seite des Echtzeitendpunkts **predict-rentals** die Registerkarte **Test** an.

1. Ersetzen Sie im Bereich **Input data to test endpoint** (Eingabedaten zum Testen des Endpunkts) den JSON-Code der Vorlage durch die folgenden Eingabedaten:

    ```JSON
    {
      "Inputs": { 
        "data": [
          {
            "day": 1,
            "mnth": 1,   
            "year": 2022,
            "season": 2,
            "holiday": 0,
            "weekday": 1,
            "workingday": 1,
            "weathersit": 2, 
            "temp": 0.3, 
            "atemp": 0.3,
            "hum": 0.3,
            "windspeed": 0.3 
          }
        ]    
      },   
      "GlobalParameters": 1.0
    }
    ```

1. Klicken Sie auf die Schaltfläche **Testen**.

1. Überprüfen Sie die Testergebnisse, die eine vorhergesagte Anzahl von Vermietungen basierend auf den Eingabefeatures enthalten, ähnlich wie hier:

    ```JSON
    {
      "Results": [
        444.27799000000000
      ]
    }
    ```

    Im Testbereich wurden die Eingabedaten erfasst und das von Ihnen trainierte Modell verwendet, um die vorhergesagte Anzahl von Vermietungen zurückzugeben.

Sehen wir uns an, was Sie getan haben. Sie haben ein Dataset mit Fahrradverleih-Verlaufsdaten verwendet, um ein Modell zu trainieren. Das Modell sagt auf der Grundlage von saisonalen und meteorologischen *Merkmalen*F vorher, die ausgeliehen werden.

## Bereinigung

Der von Ihnen erstellte Webdienst wird in einer *Azure-Containerinstanz* gehostet. Wenn Sie nicht weiter experimentieren möchten, sollten Sie den Endpunkt löschen, um eine unnötige Azure-Nutzung zu vermeiden.

1. Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) auf der Registerkarte **Endpunkte** den Endpunkt **predict-rentals** aus. Klicken Sie dann auf **Löschen**, und bestätigen Sie, dass Sie den Endpunkt löschen möchten.

    Durch das Löschen Ihrer Compute-Instanz wird sichergestellt, dass Ihrem Abonnement keine Computeressourcen in Rechnung gestellt werden. Ihnen wird jedoch eine geringe Datenspeichermenge in Rechnung gestellt, solange der Azure Machine Learning-Arbeitsbereich in Ihrem Abonnement enthalten ist. Wenn Sie mit dem Erkunden von Azure Machine Learning fertig sind, können Sie Ihren Azure Machine Learning-Arbeitsbereich und die zugehörigen Ressourcen löschen.

So löschen Sie Ihren Arbeitsbereich:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com?azure-portal=true) auf der Seite **Ressourcengruppen** die Ressourcengruppe, die Sie beim Erstellen des Azure Machine Learning-Arbeitsbereichs angegeben haben.
2. Klicken Sie auf **Ressourcengruppe löschen**, geben Sie den Ressourcengruppennamen ein, um zu bestätigen, dass Sie ihn löschen möchten, und klicken Sie dann auf **Löschen**.
