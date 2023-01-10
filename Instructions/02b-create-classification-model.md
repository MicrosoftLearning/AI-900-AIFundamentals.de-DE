---
lab:
  title: Untersuchen der Klassifizierung mit dem Azure Machine Learning-Designer
---

# <a name="explore-classification-with-azure-machine-learning-designer"></a>Untersuchen der Klassifizierung mit dem Azure Machine Learning-Designer

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free?azure-portal=true), in dem Sie über Administratorzugriff verfügen.

## <a name="create-an-azure-machine-learning-workspace"></a>Erstellen eines Azure Machine Learning-Arbeitsbereichs  

1. Melden Sie sich mit Ihren Microsoft-Anmeldeinformationen beim [Azure-Portal](https://portal.azure.com?azure-portal=true) an.

1. Klicken Sie auf **+ Ressource erstellen**, suchen Sie nach *Machine Learning*, und erstellen Sie eine neue **Azure Machine Learning**-Ressource mit einem *Azure Machine Learning*-Plan. Verwenden Sie folgende Einstellungen:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine Ressourcengruppe aus*.
    - **Arbeitsbereichsname**: *Geben Sie einen eindeutigen Namen für den Arbeitsbereich ein*.
    - **Region**: *Wählen Sie die geografisch nächstgelegene Region aus.*
    - **Speicherkonto**: *Für Ihren Arbeitsbereich wird standardmäßig ein neues Speicherkonto erstellt*.
    - **Schlüsseltresor**: *Für Ihren Arbeitsbereich wird standardmäßig ein neuer Schlüsseltresor erstellt*.
    - **Application Insights**: *Für Ihren Arbeitsbereich wird standardmäßig eine neue Application Insights-Ressource erstellt*.
    - **Containerregistrierung:** Keine (*wird automatisch erstellt, wenn Sie das erste Mal ein Modell in einem Container bereitstellen*)

1. Klicken Sie auf**Überprüfen + erstellen** und dann auf **Erstellen**. Warten Sie, bis Ihr Arbeitsbereich erstellt wurde (dies kann einige Minuten dauern), und wechseln Sie dann zur bereitgestellten Ressource.

1. Wählen Sie **Studio starten** aus (oder öffnen Sie eine neue Browserregisterkarte. Navigieren Sie dann zu [https://ml.azure.com](https://ml.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto bei Azure Machine Learning Studio an).

1. In Azure Machine Learning Studio sollte Ihr neu erstellter Arbeitsbereich angezeigt werden. Wenn dies nicht der Fall ist, klicken Sie im linken Menü auf **Microsoft**. Wählen Sie dann im neuen Menü auf der linken Seite, in dem alle Arbeitsbereiche aufgeführt werden, die Ihrem Abonnement zugeordnet sind, **Arbeitsbereiche** aus. Wählen Sie den für diese Übung erstellten Arbeitsbereich aus. 

> **Hinweis**: Dieses Modul ist eines von vielen, in denen ein Azure Machine Learning-Arbeitsbereich verwendet wird (einschließlich der anderen Module im Lernpfad [Microsoft Azure KI-Grundlagen: Erkunden visueller Tools für maschinelles Lernen](https://docs.microsoft.com/learn/paths/create-no-code-predictive-models-azure-machine-learning/)). Wenn Sie Ihr eigenes Azure-Abonnement verwenden, sollten Sie den Arbeitsbereich einmal erstellen und in anderen Modulen wiederverwenden. Ihrem Azure-Abonnement wird eine kleine Menge an Datenspeicher in Rechnung gestellt, solange der Azure Machine Learning-Arbeitsbereich in Ihrem Abonnement vorhanden ist. Daher wird empfohlen, den Azure Machine Learning-Arbeitsbereich zu löschen, wenn er nicht mehr benötigt wird.

## <a name="create-compute"></a>Erstellen von Computeressourcen

1. Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) die drei Zeilen oben links aus, um die verschiedenen Seiten in der Benutzeroberfläche anzuzeigen (möglicherweise müssen Sie die Größe des Bildschirms maximieren). Sie können diese Seiten im linken Bereich verwenden, um die Ressourcen in Ihrem Arbeitsbereich zu verwalten. Wählen die Seite **Compute** (unter **Verwalten**) aus.

1. Wählen Sie auf der Seite **Compute** die Registerkarte **Computecluster** aus, und fügen Sie einen neuen Computecluster mit den folgenden Einstellungen hinzu. Sie verwenden diesen zum Trainieren eines Machine Learning-Modells:
    - **Standort**: *Wählen Sie denselben Standort wie für Ihren Arbeitsbereich aus. Wenn dieser Standort nicht aufgeführt wird, wählen Sie den nächstgelegenen Standort aus*.
    - **VM-Dienstebene**: Dediziert.
    - **VM-Typ:** CPU
    - **VM-Größe:**
        - Klicken Sie auf **Aus allen Optionen auswählen**.
        - Suchen Sie **Standard_DS11_v2**, und wählen Sie den Eintrag aus.
    - Wählen Sie **Weiter** aus.
    - **Computename**: *Geben Sie einen eindeutigen Namen ein*.
    - **Mindestanzahl von Knoten:** 0
    - **Maximale Knotenanzahl:** 2
    - **Leerlauf in Sekunden vor dem Herunterskalieren:** 120
    - **SSH-Zugriff aktivieren**: Deaktiviert.
    - Klicken Sie auf **Erstellen**.

> **Hinweis**: Compute-Instanzen und -cluster basieren auf Azure VM-Standardimages. Für dieses Modul wird das Image *Standard_DS11_v2* empfohlen, um ein optimales Gleichgewicht zwischen Kosten und Leistung zu erzielen. Wenn Ihr Abonnement über ein Kontingent verfügt, das dieses Image nicht enthält, wählen Sie ein alternatives Image aus. Beachten Sie jedoch, dass ein größeres Image höhere Kosten verursachen kann und ein kleineres Image möglicherweise nicht ausreicht, um die Aufgaben auszuführen. Bitten Sie alternativ Ihren Azure-Administrator, Ihr Kontingent zu erhöhen.

Die Erstellung des Computeclusters nimmt einige Zeit in Anspruch. Sie können mit dem nächsten Schritt fortfahren, während Sie warten.

## <a name="create-a-pipeline-in-designer"></a>Erstellen einer Pipeline im Designer

Für den Einstieg in den Azure Machine Learning-Designer müssen Sie zunächst eine Pipeline erstellen und das Dataset hinzufügen, mit dem Sie arbeiten möchten.

1. Erweitern Sie im [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) den linken Bereich, indem Sie die drei Zeilen oben links auf dem Bildschirm auswählen. Zeigen Sie die Seite **Designer** (unter **Autor**) an, und wählen Sie **+** aus, um eine neue Pipeline zu erstellen.

1. Wählen Sie oben rechts auf dem Bildschirm **Einstellungen** aus. Wenn der Bereich **Einstellungen** nicht sichtbar ist, wählen Sie das Radsymbol neben dem Pipelinenamen oben.

1. Unter **Einstellungen** müssen Sie ein Computeziel angeben, auf dem die Pipeline ausgeführt werden soll. Wählen Sie unter **Computetyp auswählen** die Option **Computecluster** aus. Wählen Sie dann unter **Azure ML-Computecluster auswählen** den zuvor erstellten Computecluster aus.

1. Ändern Sie in **Einstellungen** unter **Draft Details** (Entwurfsdetails) den Entwurfsnamen (**Pipeline-Created-on-* date***) in **Diabetes Training**.

1. Wählen Sie oben rechts im Bereich **Einstellungen** das Symbol zum Schließen aus, um den Bereich zu schließen. Wählen Sie dann **Speichern** aus.

    ![Screenshot des Einstellungsbereichs in Machine Learning Studio](media/create-classification-model/create-pipeline-help.png)

## <a name="create-a-dataset"></a>Erstellen eines Datasets

1. Erweitern Sie in [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) den linken Bereich, indem Sie die drei Zeilen oben links auf dem Bildschirm auswählen. Zeigen Sie die Seite **Daten** an (unter **Ressourcen**). Die Seite „Daten“ enthält bestimmte Datendateien oder Tabellen, mit denen Sie in Azure Machine Learning arbeiten möchten. Sie können auch auf dieser Seite Datasets erstellen.

1. Wählen Sie auf der Seite **Daten** auf der Registerkarte **Datenressourcen** die Option **Erstellen** aus. Konfigurieren Sie dann eine Datenressource mit den folgenden Einstellungen:
    * **Datentyp**:
        * **Name:** diabetes-data
        * **Beschreibung:** Diabetesdaten
        * **Datasettyp:** Tabellarisch
    * **Datenquelle**: Aus Webdateien
    * **Web-URL:** 
        * **Web-URL:** https://aka.ms/diabetes-data
        * **Skip data validation** (Datenüberprüfung überspringen): *Nicht auswählen*
    * **Einstellungen**:
        * **Dateiformat:** Zeichengetrennt
        * **Trennzeichen:** Komma
        * **Codierung:** UTF-8
        * **Spaltenüberschriften**: Nur erste Datei enthält Header
        * **Zeilen überspringen:** Keine
        * **Dataset contains multi-line data** (Dataset enthält mehrzeilige Daten): *Nicht auswählen*
    * **Schema:**
        * Alle Spalten einschließen außer **Pfad**
        * Überprüfen der automatisch erkannten Typen
    * **Überprüfung**
        * Klicken Sie auf **Erstellen**.

1. Nachdem das Dataset erstellt wurde, öffnen Sie es, und zeigen Sie die Seite **Erkunden** an, um eine Stichprobe der Daten anzuzeigen. Diese Daten stellen Details von Patienten dar, die auf Diabetes getestet wurden.

### <a name="load-data-to-canvas"></a>Laden Sie Daten im Canvas-Panel,

1. Navigieren Sie zurück zu Ihrer Pipeline, indem Sie im Menü auf der linken Seite auf **Designer** klicken. Wählen Sie auf der Seite **Designer** die Pipeline **Diabetes Training** aus.

1. Klicken Sie dann im Projekt neben dem Pipelinenamen auf das Pfeilsymbol auf der linken Seite, um den Bereich zu erweitern, sofern er nicht bereits erweitert ist. Der Bereich sollte standardmäßig im Bereich **Ressourcenbibliothek** geöffnet werden. Dies wird durch das Büchersymbol am oberen Rand des Bereichs gekennzeichnet. Beachten Sie, dass eine Suchleiste für die Suche nach Objekten vorhanden ist. Beachten Sie die beiden Schaltflächen **Daten** und **Komponente**.

    ![Screenshot der Position der Ressourcenbibliothek für den Designer, die Suchleiste und das Datensymbol](media/create-classification-model/designer-asset-library-data.png)

1. Klicken Sie auf **Daten**. Suchen Sie den Datensatz **diabetes-data** und platzieren Sie ihn auf deM Canvas.

1. Klicken Sie mit der rechten Maustaste (auf einem Mac drücken Sie die CTRL-Taste und klicken) auf das Dataset **diabetes-data** auf der Canvas, und klicken Sie dann auf **Datenvorschau anzeigen**.

1. Überprüfen Sie das Schema der Daten auf der Registerkarte *Profil* und beachten Sie, dass Sie die Verteilungen der verschiedenen Spalten als Histogramme angezeigt bekommen.

1. Scrollen Sie nach unten, und wählen Sie die Überschrift der Spalte **Diabetic** aus. Beachten Sie, dass sie zwei Werte enthält **0** und **1**. Diese Werte stellen die beiden möglichen Klassen für die *Bezeichnung* dar, die Ihr Modell vorhersagen soll. Dabei bedeutet der Wert **0**, dass der Patient keinen Diabetes hat, und der Wert **1**, dass es sich um einen Diabetiker handelt.

1. Scrollen Sie zurück nach oben, und überprüfen Sie die anderen Spalten, die die zum Vorhersagen der Bezeichnung verwendeten *Features* darstellen. Beachten Sie, dass die meisten dieser Spalten numerisch sind, aber jedes Feature eine eigene Skalierung aufweist. Die Werte für **Age** liegen z. B. zwischen 21 und 77, während die Werte von **DiabetesPedigree** zwischen 0,078 und 2,3016 liegen. Beim Trainieren eines Machine Learning-Modells ist es manchmal möglich, dass größere Werte die resultierende Vorhersagefunktion dominieren und dadurch der Einfluss von Features auf eine kleinere Skalierung verringert wird. Data Scientists mindern diese mögliche Abweichung in der Regel, indem sie numerische Spalten *normalisieren*, damit sie eine ähnliche Skalierung aufweisen.

1. Schließen Sie die Registerkarte mit der **Ergebnisvisualisierung von diabetes-data**, damit das Dataset wie folgt auf dem Zeichenbereich angezeigt wird:

    ![Screenshot: Dataset „diabetes-data“ im dem Designer-Canvas](media/create-classification-model/diabetes-data.png)

## <a name="add-transformations"></a>Transformationen hinzufügen

Bevor Sie ein Modell trainieren können, müssen Sie in der Regel einige Vorverarbeitungstransformationen auf die Daten anwenden.

1. Klicken Sie links im Bereich **Ressourcenbibliothek** auf **Komponenten**. Diese enthalten verschiedenste Module, die Sie für Datentransformationen und Modelltrainings verwenden können. Für die schnelle Suche nach Modulen können Sie auch die Suchleiste verwenden.

    ![Screenshot der Position der Ressourcenbibliothek für den Designer, die Suchleiste und das Komponentensymbol](media/create-classification-model/designer-asset-library-components.png)

1. Suchen Sie nach dem Modul **Daten normalisieren**, und platzieren Sie es in das Canvas-Panel unterhalb des Datasets **diabetes-data**. Verbinden Sie anschließend den Ausgang vom unteren Rand des Datasets **diabetes-data** mit dem Eingang am oberen Rand des Moduls **Daten normalisieren**, wie im Folgenden gezeigt:

    ![Screenshot: Pipeline mit Verbindung zwischen dem Dataset und dem Modul „Normalize Data“ (Daten normalisieren)](media/create-classification-model/dataset-normalize.png)

1. Doppelklicken Sie auf das Modul **Daten normalisieren**, um dessen Einstellungen anzuzeigen. Beachten Sie, dass Sie die Transformationsmethode und die zu transformierenden Spalten angeben müssen. 

1. Legen Sie die *Transformationsmethode* auf **MinMax** und *Use 0 for constant columns when checked* (Bei der Überprüfung 0 für konstante Spalten verwenden) auf **TRUE** fest. Bearbeiten Sie die Spalten, um die folgenden Spalten wie in der Abbildung dargestellt nach Namen einzuschließen:
    - **Pregnancies** (Schwangerschaften)
    - **PlasmaGlucose**
    - **DiastolicBloodPressure**
    - **TricepsThickness**
    - **SerumInsulin**
    - **BMI**
    - **DiabetesPedigree**
    - **Age** (Alter)

    ![Screenshot: Für die Normalisierung ausgewählte Spalten](media/create-classification-model/normalize-data.png)

Bei der Datentransformation werden die numerischen Spalten normalisiert, um ihre Skalierung anzugleichen. Dadurch soll verhindert werden, dass Spalten mit großen Werten das Modelltraining dominieren. Sie würden normalerweise eine ganze Reihe von Transformationen wie diese zur Vorverarbeitung anwenden, um Ihre Daten für das Training vorzubereiten, aber wir halten diese Übung einfach.

## <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.

Zum Anwenden der Datentransformationen müssen Sie die Pipeline als Experiment ausführen.

1. Wählen Sie **Übermitteln** aus, und führen Sie die Pipeline als neues Experiment mit dem Namen **mslearn-diabetes-training** auf Ihrem Computecluster aus.

1. Warten Sie einige Minuten, bis die Ausführung abgeschlossen ist.

    ![Screenshot der Ressourcenbibliothek des Designers mit dem abgeschlossenen Auftrag sowie der Schaltfläche für die Auftragsdetails darunter](media/create-classification-model/completed-job.png)

    Beachten Sie, dass sich der linke Bereich jetzt im Bereich **Übermittelte Aufträge** befindet. Sie werden wissen, wann die Ausführung abgeschlossen ist, da sich der Status des Auftrags zu **Abgeschlossen** ändert.

## <a name="view-the-transformed-data"></a>Anzeigen der transformierten Daten

1. Wenn die Ausführung abgeschlossen ist, wird das Dataset nun für das Modelltraining vorbereitet. Klicken Sie auf **Auftragsdetails**. Eine neue Registerkarte wird angezeigt.

1. Klicken Sie mit der rechten Maustaste (STRG+KLICK auf einem Mac) auf das Modul **Daten normalisieren** auf dem Canvas, und klicken Sie auf **Datenvorschau**. Wählen Sie **Transformiertes Dataset**.

1. Zeigen Sie die Daten an, und beachten Sie, dass die ausgewählten numerischen Spalten auf eine gemeinsame Skalierung normalisiert wurden.

1. Schließen Sie die Visualisierung der Ergebnisse der Datennormalisierung. Kehren Sie zur vorherigen Registerkarte zurück.

Nachdem Sie die Daten mithilfe von Datentransformationen vorbereitet haben, können Sie sie zum Trainieren eines Machine Learning-Modells verwenden.

## <a name="add-training-modules"></a>Hinzufügen von Trainingsmodulen

Es ist üblich, das Modell mit einer Teilmenge der Daten zu trainieren und einige Daten zurückzuhalten, mit denen das trainierte Modell anschließend getestet werden kann. Dadurch können Sie die vom Modell vorhergesagten Bezeichnungen mit den tatsächlichen bekannten Bezeichnungen im ursprünglichen Dataset vergleichen.

In dieser Übung werden Sie die Schritte zur Erweiterung der Pipeline **Diabetes Training** wie hier gezeigt durcharbeiten:

![Screenshot: Aufteilen von Daten, anschließendes Trainieren der Daten mit logistischer Regression und Bewerten](media/create-classification-model/train-score-pipeline.png)

Führen Sie die folgenden Schritte aus, und verwenden Sie die obige Abbildung als Referenz für das Hinzufügen und Konfigurieren der erforderlichen Module.

1. Öffnen Sie die Pipeline **Diabetes Training**, die Sie in der vorherigen Einheit erstellt haben, sofern sie nicht bereits geöffnet ist.

1. Suchen Sie im Bereich **Objektbibliothek** auf der linken Seite unter **Komponente** nach einem Modul **Daten aufteilen**, und platzieren Sie es auf der Canvas unter dem Modul **Daten normalisieren**. Verbinden Sie dann die (linke) Ausgabe *Transformiertes Dataset* des Moduls **Daten normalisieren** mit der Eingabe des Moduls **Daten teilen**.

    >**Tipp**: Für die schnelle Suche nach Modulen verwenden Sie die Suchleiste.

1. Wählen Sie das Modul **Daten teilen** aus, und konfigurieren Sie dessen Einstellungen wie folgt:
    * **Aufteilungsmodus:** Zeilen aufteilen
    * **Bruchteil von Zeilen im ersten Ausgabedataset:** 0,7
    * **Zufällige Aufteilung**: True
    * **Zufälliger Ausgangswert:** 123
    * **Geschichtete Aufteilung:** FALSE

1. Suchen Sie in der **Ressourcenbibliothek** nach einem Modul vom Typ **Modell trainieren**, und platzieren Sie es auf der Canvas unter dem Modul **Daten teilen**. Verbinden Sie dann die (linke) Ausgabe *Ergebnisse Dataset1* des Moduls **Daten teilen** mit der (rechten) Eingabe *Dataset* des Moduls **Modell trainieren**.

1. Das Modell, das wir trainieren, sagt den Wert **Diabetic** vorher. Wählen Sie also das Modul **Modell trainieren** aus, und ändern Sie dessen Einstellungen, um die Spalte **Bezeichnung** auf **Diabetic** festzulegen.

    Die Bezeichnung **Diabetic**, die das Modell vorhersagt, ist eine Klasse (0 oder 1). Daher müssen Sie das Modell mithilfe eines Algorithmus zur *Klassifizierung* trainieren. Es gibt zwei mögliche Klassen, daher benötigen Sie einen Algorithmus zur *binären Klassifizierung*.

1. Suchen Sie in der **Ressourcenbibliothek** nach dem Modul **Logistische Zwei-Klassen-Regression** und platzieren Sie es im Canvas links neben dem Modul **Daten teilen** und über dem Modul **Modell trainieren**. Verbinden Sie anschließend seinen Ausgang mit dem (linken) Eingang **Untrainiertes Modell** des Moduls **Modell trainieren**.

   Sie können das trainierte Modell testen, indem Sie es für die *Bewertung* des Validierungsdatasets verwenden, das Sie bei der Aufteilung der ursprünglichen Daten zurückgehalten haben. Sie sollen also die Bezeichnungen für die Features im Validierungsdataset vorhersagen.

1. Suchen Sie in der **Ressourcenbibliothek** nach einem Modul vom Typ **Modell bewerten**, und platzieren Sie es auf der Canvas unter dem Modul **Modell trainieren**. Verbinden Sie anschließend den Ausgang des Moduls **Modell trainieren** mit dem (linken) Eingang **Trainiertes Modell** des Moduls **Modell bewerten**. Verbinden Sie den (rechten) Ausgang **Ergebnisse Dataset2** des Moduls **Daten teilen** mit dem (rechten) Eingang **Dataset** des Moduls **Modell bewerten**.

## <a name="run-the-training-pipeline"></a>Ausführen der Trainingspipeline

Nun können Sie die Trainingspipeline ausführen und das Modell trainieren.

1. Wählen Sie **Übermitteln** aus, und führen Sie die Pipeline mithilfe des vorhandenen Experiments **mslearn-diabetes-training** aus.

1. Warten Sie, bis die Experimentausführung abgeschlossen ist. Dies kann 5 Minuten oder länger dauern.

1. Wenn die Ausführung des Experiments beendet ist, wählen Sie **Auftragsdetails** aus. Sie werden zu einer neuen Registerkarte weitergeleitet.

1. Klicken Sie auf der neuen Registerkarte mit der rechten Maustaste (STRG+KLICK auf einem Mac) auf das Modul **Modell auswerten** auf der Canvas, und klicken Sie dann auf **Datenvorschau**. Wählen Sie **Bewertetes Dataset** aus, um die Ergebnisse anzuzeigen.

1. Scrollen Sie nach rechts, und beachten Sie, dass neben der Spalte **Diabetic** (die die bekannten wahren Werte der Bezeichnung enthält) eine neue Spalte mit dem Namen **Scored Labels** (Bewertete Bezeichnungen) vorhanden ist, die die vorhergesagten Bezeichnungswerte enthält, und eine Spalte **Scored Probabilities** (Bewertete Wahrscheinlichkeiten), die einen Wahrscheinlichkeitswert zwischen 0 und 1 enthält. Dies gibt die Wahrscheinlichkeit einer *positiven* Vorhersage an. Wahrscheinlichkeiten größer als 0,5 führen also zu einer vorhergesagten Bezeichnung von ***1*** (Diabetiker), während Wahrscheinlichkeiten zwischen 0 und 0,5 zu einer vorhergesagten Bezeichnung von ***0*** (kein Diabetiker) führen.

1. Schließen Sie die Registerkarte **Visualisierung der Modellbewertungsergebnisse**.

Das Modell prognostiziert Werte für die Bezeichnung **Diabetic**, aber wie zuverlässig sind die Vorhersagen? Um dies zu bewerten, müssen Sie das Modell auswerten.

Die Validierungsdaten, die Sie zurückgehalten und zum Bewerten des Modells verwendet haben, enthalten die bekannten Werte für die Bezeichnung. Um das Modell zu überprüfen, können Sie die tatsächlichen Werte für die Bezeichnung daher mit den Bezeichnungswerten vergleichen, die vorhergesagt wurden, als Sie das Validierungsdataset bewertet haben. Basierend auf diesem Vergleich können Sie verschiedene Metriken berechnen, die beschreiben, wie gut das Modell ist.

## <a name="add-an-evaluate-model-module"></a>Hinzufügen eines Moduls „Modell auswerten“

1. Öffnen Sie die Pipeline **Diabetes Training**, die Sie erstellt haben.

1. Suchen Sie in der **Ressourcenbibliothek** nach einem Modul vom Typ **Modell auswerten** und platzieren Sie es auf der Canvas unter dem Modul **Modell bewerten**. Verbinden Sie die Ausgabe des Moduls **Modell bewerten** mit der Eingabe **Bewertetes Dataset** (links) des Moduls **Modell auswerten**.

1. Vergewissern Sie sich, dass Ihre Pipeline wie folgt aussieht:

    ![Screenshot: Zum Modul „Score Model“ (Modell bewerten) hinzugefügtes Modul „Evaluate Model“ (Modell auswerten)](media/create-classification-model/evaluate-pipeline.png)

1. Wählen Sie **Übermitteln** aus, und führen Sie die Pipeline mithilfe des vorhandenen Experiments **mslearn-diabetes-training** aus.

1. Warten Sie, bis die Experimentausführung abgeschlossen ist.

1. Wenn die Ausführung des Experiments beendet ist, wählen Sie **Auftragsdetails** aus. Sie werden zu einer neuen Registerkarte weitergeleitet.

1. Klicken Sie auf der neuen Registerkarte mit der rechten Maustaste (STRG+KLICK auf einem Mac) auf das Modul **Modell auswerten** auf der Canvas, und klicken Sie dann auf **Datenvorschau**. Wählen Sie **Auswertungsergebnisse** aus, um die Leistungsmetriken anzuzeigen. Mithilfe dieser Metriken können Data Scientists bewerten, wie gut die Vorhersagen des Modells basierend auf den Validierungsdaten sind.

1. Scrollen Sie nach unten, um die *Konfusionsmatrix* für das Modell anzuzeigen. Beobachten Sie die vorhergesagten und tatsächlichen Werte für jede mögliche Klasse. 

1. Überprüfen Sie die Metriken auf der linken Seite der Konfusionsmatrix, die Folgendes umfassen:
    - **Genauigkeit:** Mit anderen Worten: Welchen Anteil von Diabetesfällen hat das Modell richtig vorhergesagt?
    - **Genauigkeit:** Mit anderen Worten: Welcher Prozentsatz der Patienten, für die das *Modell eine Diabeteserkrankung vorhergesagt hat*, wurde richtig vorhergesagt? 
    - **Abruf:** Mit anderen Worten: Wie viele der Patienten, die *tatsächlich Diabetes haben*, hat das Modell richtig identifiziert?
    - **F1-Bewertung**

1. Verwenden Sie den Schieberegler **Grenzwert** oberhalb der Liste der Metriken. Verschieben Sie den Schwellenwert-Schieberegler, und beobachten Sie die Auswirkung auf die Konfusionsmatrix. Wenn Sie ihn ganz nach links verschieben (0), wird die Metrik „Trefferquote“ 1, und wenn Sie ihn ganz nach rechts verschieben (1), wird die Metrik „Trefferquote“ 0.

1. Über dem Schieberegler „Grenzwert“ befinden sich die **ROC-Kurve** und die **AUC**-Metrik zusammen mit den anderen Metriken unten. Um eine Vorstellung davon zu erhalten, wie dieser Bereich die Leistung des Modells darstellt, stellen Sie sich eine gerade diagonale Linie von links unten nach rechts oben im ROC-Diagramm vor. Diese stellt die erwartete Leistung dar, wenn Sie für jeden Patienten raten oder eine Münze werfen. Sie könnten dabei davon ausgehen, dass Sie ungefähr die Hälfte richtig und die Hälfte falsch hätten, sodass der Bereich unter der diagonalen Linie einen AUC-Score von 0,5 darstellt. Wenn der AUC-Score Ihres Modell für ein binäres Klassifizierungsmodell höher als dieser Wert ist, ist das Modell besser als zufälliges Raten.

1. Schließen Sie die Registerkarte **Visualisierung der Modellauswertungsergebnisse**.

Die Leistung dieses Modells ist nicht besonders gut, da Sie nur eine minimale Featurisierung und Vorverarbeitung durchgeführt haben. Sie könnten einen anderen Klassifizierungsalgorithmus verwenden, z. B. einen **zweiklassigen Entscheidungswald**, und die Ergebnisse vergleichen. Sie können die Ausgänge des Moduls **Daten teilen** mit mehreren Modulen der Typen **Modell trainieren** und **Modell bewerten** verbinden, und Sie können ein zweites Modul **Modell bewerten** mit dem Modul **Modell auswerten** verbinden, um einen direkten Vergleich zu erhalten. Bei dieser Übung geht es lediglich um eine Einführung in die Klassifizierung und die Benutzeroberfläche des Azure Machine Learning-Designers, nicht um das Trainieren eines perfekten Modells.

## <a name="create-an-inference-pipeline"></a>Erstellen einer Rückschlusspipeline

1. Klicken Sie links oben auf dem Bildschirm auf die drei Linien, um in Azure Machine Learning Studio den linken Bereich zu erweitern. Klicken Sie unter **Ressourcen** auf **Aufträge**, um alle von Ihnen ausgeführten Aufträge anzuzeigen. Wählen Sie das Experiment **mslearn-diabetes-training** und anschließend die Pipeline **Diabetes Training** aus.

1. Klicken Sie im Menü oberhalb der Canvas auf **Rückschlusspipeline erstellen**. Möglicherweise müssen Sie hierfür in den Vollbildmodus wechseln und rechts oben auf das Symbol mit den drei Punkten **...** klicken, damit die Option **Rückschlusspipeline erstellen** im Menü angezeigt wird.  

    ![Screenshot: Position der Option „Rückschlusspipeline erstellen“](media/create-classification-model/create-inference-pipeline.png)

1. Klicken Sie in der Dropdownliste **Rückschlusspipeline erstellen** auf **Echtzeit-Rückschlusspipeline**. Nach einigen Sekunden wird eine neue Version Ihrer Pipeline mit dem Namen **Diabetes Training-real time inference** (Diabetesschulung – Echtzeitrückschlüsse) geöffnet.

1. Navigieren Sie im oberen rechten Menü zu **Einstellungen**. Benennen Sie die neue Pipeline unter **Entwurfsdetails** in **Diabetes vorhersagen** um, und überprüfen Sie die neue Pipeline dann. Einige der Transformationen und Trainingsschritte sind Teil dieser Pipeline. Das trainierte Modell wird zur Bewertung der neuen Daten verwendet. Die Pipeline enthält auch eine Webdienstausgabe zum Zurückgeben von Ergebnissen. 

    Nehmen Sie an der Rückschlusspipeline die folgenden Änderungen vor:

    ![Screenshot: Rückschlusspipeline mit hervorgehobenen Änderungen](media/create-classification-model/inference-changes.png)
    
    - Fügen Sie eine **Webdiensteingabekomponente** für neue Daten hinzu, die übermittelt werden sollen.
    - Ersetzen Sie das Dataset **diabetes-data** durch ein Modul **Daten manuell eingeben**, das die Bezeichnungsspalte (**Diabetic**) nicht enthält.
    - Entfernen Sie das Modul **Modell auswerten**.
    - Fügen Sie vor dem Webdienstausgang ein Modul **Python-Skript ausführen** ein, damit nur die Patienten-ID, der vorhergesagte Bezeichnungswert und die Wahrscheinlichkeit zurückgegeben werden.

1. Die Pipeline enthält nicht automatisch eine **Webdiensteingabekomponente** für Modelle, die aus benutzerdefinierten Datasets erstellt werden. Suchen Sie in der Objektbibliothek nach einer **Webdiensteingabekomponente**, und platzieren Sie diese oben in der Pipeline. Verbinden Sie die Ausgabe der **Webdiensteingabekomponente** mit der Eingabe auf der rechten Seite der Komponente **Transformation anwenden**, die sich bereits auf der Canvas befindet.

1. Die Rückschlusspipeline geht davon aus, dass neue Daten dem Schema der ursprünglichen Trainingsdaten entsprechen, sodass das Dataset **diabetes-data** aus der Trainingspipeline eingeschlossen wird. Diese Eingabedaten enthalten jedoch die Bezeichnung **Diabetic**, die vom Modell vorhergesagt wird. Diese Bezeichnung ist in neuen Patientendaten, für die noch keine Vorhersage über den Diabetes ausgeführt wurde, nicht enthalten. Löschen Sie dieses Modul und ersetzen Sie es durch ein Modul **Daten manuell eingeben**, das die folgenden CSV-Daten enthält, die Merkmalswerte ohne Beschriftungen für drei neue Patientenbeobachtungen enthalten:

    ```CSV
    PatientID,Pregnancies,PlasmaGlucose,DiastolicBloodPressure,TricepsThickness,SerumInsulin,BMI,DiabetesPedigree,Age
    1882185,9,104,51,7,24,27.36983156,1.350472047,43
    1662484,6,73,61,35,24,18.74367404,1.074147566,75
    1228510,4,115,50,29,243,34.69215364,0.741159926,59
    ```

1. Verbinden Sie das neue Modul **Daten manuell eingeben** mit demselben Eingang des **Datasets** des Moduls **Transformation anwenden** wie beim **Webdiensteingang**.

1. Die Rückschlusspipeline umfasst das Modell **Modul auswerten**, das bei der Vorhersage aus neuen Daten nicht nützlich ist. Löschen Sie daher dieses Modul.

1. Die Ausgabe des Moduls **Modell bewerten** umfasst alle Eingabefeatures sowie die vorhergesagte Bezeichnung und den Wahrscheinlichkeitsscore. So beschränken Sie die Ausgabe nur auf die Vorhersage und die Wahrscheinlichkeit
    - Löschen Sie die Verbindung zwischen dem Modul **Modell bewerten** und dem **Webdienstausgang**.
    - Fügen Sie ein Modul **Python-Skript ausführen** hinzu und ersetzen Sie das gesamte Standard-Python-Skript durch den folgenden Code (der nur die Spalten **PatientID**, **Bewertete Etiketten** und **Bewertete Wahrscheinlichkeiten** auswählt und sie entsprechend umbenennt):

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    scored_results = dataframe1[['PatientID', 'Scored Labels', 'Scored Probabilities']]
    scored_results.rename(columns={'Scored Labels':'DiabetesPrediction',
                                'Scored Probabilities':'Probability'},
                        inplace=True)
    return scored_results
```

1. Verbinden Sie die Ausgabe des Moduls **Modell bewerten** mit der (ganz linken) Eingabe **Dataset1** von **Python-Skript ausführen**, und verbinden Sie die Ausgabe des Moduls **Python-Skript ausführen** mit der **Webdienstausgabe**.

1. Ihre Pipeline sollte etwa wie die folgende aussehen:

    ![Screenshot: Vollständige Rückschlusspipeline](media/create-classification-model/visual-inference.png)

1. Führen Sie die Pipeline als neues Experiment mit dem Namen **mslearn-diabetes-inference** auf Ihrem Computecluster aus. Die Ausführung des Experiments kann einige Minuten dauern.

1. Wenn die Pipeline abgeschlossen ist, wählen Sie **Auftragsdetails** aus. Klicken Sie auf der neuen Registerkarte mit der rechten Maustaste auf das Modul **Python-Skript ausführen**. Wählen Sie die **Datenvorschau** und dann **Ergebnisdataset** aus, um die vorhergesagten Bezeichnungen und Wahrscheinlichkeiten für die drei Patientenbeobachtungen in den Eingabedaten anzuzeigen.

Ihre Rückschlusspipeline sagt basierend auf den Features der Patienten vorher, ob für diese eine Gefahr für Diabetes vorliegt. Sie können die Pipeline nun so veröffentlichen, dass sie von Clientanwendungen verwendet werden kann.

Nachdem Sie eine Rückschlusspipeline für Echtzeitrückschlüsse erstellt und getestet haben, können Sie sie als Dienst veröffentlichen, der von Clientanwendungen verwendet werden kann.

> **Hinweis**: In dieser Übung stellen Sie den Webdienst in einer Azure-Containerinstanz bereit. Solche Computeressourcen werden dynamisch erstellt und sind für Entwicklungs- und Testzwecke nützlich. Für Produktionszwecke sollten Sie einen *Rückschlusscluster* erstellen, der einen AKS-Cluster (Azure Kubernetes Service) mit verbesserter Skalierbarkeit und Sicherheit bereitstellt.

## <a name="deploy-a-service"></a>Bereitstellen eines Diensts

1. Zeigen Sie die Rückschlusspipeline **Diabetes vorhersagen** an, die Sie in der vorherigen Einheit erstellt haben.

1. Wählen Sie im linken Bereich die Option **Auftragsdetails** aus. Dadurch wird ein weiteres Fenster geöffnet.

    ![Screenshot: Auftragsdetails neben dem abgeschlossenen Auftrag](media/create-classification-model/completed-job-inference.png)

1. Wählen Sie in diesem neuen Fenster die Option **Bereitstellen** aus.

    ![Screenshot: Schaltfläche „Bereitstellen“ für die Rückschlusspipeline „Predict Auto Price“](media/create-classification-model/deploy-screenshot.png)

1. Klicken Sie oben rechts auf **Bereitstellen**, und stellen Sie einen **neuen Echtzeitendpunkt** bereit. Legen Sie dabei die folgenden Einstellungen fest: 
    -  **Name**: predict-diabetes
    -  **Beschreibung**: Diabetes klassifizieren
    - **Computetyp:** Azure Container Instances

1. Warten Sie, bis der Webdienst bereitgestellt wurde. Dieser Vorgang kann einige Minuten in Anspruch nehmen. Der Bereitstellungsstatus wird links oben auf der Benutzeroberfläche des Designers angezeigt.

## <a name="test-the-service"></a>Testen des Diensts

1. Öffnen Sie auf der Seite **Endpunkte** den Echtzeitendpunkt **predict-diabetes**.

    ![Screenshot: Position der Option „Endpunkte“ im linken Bereich](media/create-classification-model/endpoints-screenshot.png)

1. Wenn der Endpunkt **predict-diabetes** geöffnet wird, wählen Sie die Registerkarte **Test** aus. Hier testen wir unser Modell mit neuen Daten. Löschen Sie die vorhandenen Daten unter **Input data to test real-time endpoint** (Eingabedaten zum Testen des Echtzeitendpunkts). Kopieren Sie die folgenden Daten, und fügen Sie sie in den Abschnitt „Daten“ ein:  

    ```JSON
    {
      "Inputs": {
        "input1":
          [
            { "PatientID": 1882185,
              "Pregnancies": 9,
              "PlasmaGlucose": 104,
              "DiastolicBloodPressure": 51,
              "TricepsThickness": 7,
              "SerumInsulin": 24,
              "BMI": 27.36983156,
              "DiabetesPedigree": 1.3504720469999998,
              "Age": 43 }
            ]
          },
      "GlobalParameters":  {}
    }
    ```

    > **Hinweis**: Der JSON-Code oben definiert Funktionen für einen Patienten und verwendet den von Ihnen erstellten Dienst **predict-diabetes**, um eine Diabetesdiagnose vorherzusagen.

1. Klicken Sie auf **Test**. Auf der rechten Seite des Bildschirms sollte die Ausgabe **'DiabetesPrediction'** angezeigt werden. Die Ausgabe ist 1, wenn für den Patienten Diabetes vorhergesagt wird, und 0, wenn für den Patienten kein Diabetes vorhergesagt wird.  

    ![Screenshot: Bereich „Testen“](media/create-classification-model/test-interface.png)

    Sie haben gerade einen Dienst getestet, der mit einer Clientanwendung eine Verbindung herstellen kann, indem die Anmeldeinformationen auf der Registerkarte **Consume** (Verbrauchen) verwendet werden. Das Lab endet hier. Sie können gern weiter mit dem Dienst experimentieren, den Sie gerade eingerichtet haben.

## <a name="clean-up"></a>Bereinigung

Der von Ihnen erstellte Webdienst wird in einer *Azure-Containerinstanz* gehostet. Wenn Sie nicht weiter experimentieren möchten, sollten Sie den Endpunkt löschen, um eine unnötige Azure-Nutzung zu vermeiden.

1. Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) auf der Registerkarte **Endpunkte** den Endpunkt **predict-diabetes** aus. Klicken Sie dann auf **Löschen**, und bestätigen Sie, dass Sie den Endpunkt löschen möchten.

1. Wählen Sie auf der Seite **Compute** auf der Registerkarte **Computecluster** Ihren Computecluster aus, und klicken Sie dann auf **Löschen**.

>**Hinweis**: Durch das Beenden Ihrer Compute-Instanz wird sichergestellt, dass Ihrem Abonnement keine Computeressourcen in Rechnung gestellt werden. Ihnen wird jedoch eine geringe Datenspeichermenge in Rechnung gestellt, solange der Azure Machine Learning-Arbeitsbereich in Ihrem Abonnement enthalten ist. Wenn Sie mit dem Erkunden von Azure Machine Learning fertig sind, können Sie Ihren Azure Machine Learning-Arbeitsbereich und die zugehörigen Ressourcen löschen. Wenn Sie jedoch andere Labs in dieser Reihe abschließen möchten, müssen Sie ihn neu erstellen.
>
> So löschen Sie Ihren Arbeitsbereich:
>
> 1. Öffnen Sie im [Azure-Portal](https://portal.azure.com?azure-portal=true) auf der Seite **Ressourcengruppen** die Ressourcengruppe, die Sie beim Erstellen des Azure Machine Learning-Arbeitsbereichs angegeben haben.
> 1. Klicken Sie auf **Ressourcengruppe löschen**, geben Sie den Ressourcengruppennamen ein, um zu bestätigen, dass Sie ihn löschen möchten, und klicken Sie dann auf **Löschen**.