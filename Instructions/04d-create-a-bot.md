---
lab:
  title: Erkunden der Beantwortung von Fragen
---

# Erkunden der Beantwortung von Fragen

> **Hinweis**: Um dieses Lab abzuschließen, benötigen Sie ein [Azure-Abonnement](https://azure.microsoft.com/free?azure-portal=true), in dem Sie über Administratorzugriff verfügen.

Für Kundensupportszenarien ist es üblich, einen Bot zu erstellen, der häufig gestellte Fragen über ein Chatfenster, eine E-Mail-Adresse oder eine Sprachschnittstelle einer Website interpretieren und beantworten kann. Der Botschnittstelle liegt eine Wissensdatenbank mit Fragen und entsprechenden Antworten zugrunde, die der Bot nach geeigneten Antworten durchsuchen kann.

## Erstellen einer benutzerdefinierten „Fragen und Antworten“-Wissensdatenbank

Mit dem benutzerdefinierten „Fragen und Antworten“-Feature des Sprachdiensts können Sie schnell eine Wissensdatenbank erstellen, indem Sie Frage-Antwort-Paare eingeben oder sie aus einem vorhandenen Dokument oder einer Webseite übertragen. Er kann dann einige integrierte Funktionen zur Verarbeitung natürlicher Sprache nutzen, um Fragen zu interpretieren und passende Antworten zu finden.

1. Öffnen Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto an.

1. Klicken Sie auf die Schaltfläche **&#65291;Ressource erstellen**. Suchen Sie nach *Sprachdienst*, erstellen Sie eine **Sprachdienst**-Ressource mit den folgenden Einstellungen, und klicken Sie dann auf **Fortfahren, um Ihre Ressource zu erstellen**: **Zusätzliche Features auswählen**
    - **Standardfeatures**: *Behalten Sie die Standardfeatures bei*.
    - **Benutzerdefinierte Features**: *Wählen Sie benutzerdefinierte Beantwortung von Fragen aus*.

    ![Erstellen einer Sprachdienstressource mit aktivierter benutzerdefinierter Beantwortung von Fragen.](media/create-a-bot/create-language-service-resource.png)

1. Geben Sie auf der Seite **Sprache erstellen** die folgenden Einstellungen an:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue.*
    - **Name**: *Ein eindeutiger Name für Ihre Sprachressource*.
    - **Tarif**: S (1.000 Aufrufe pro Minute)
    - **Azure-Suchregion**: *Beliebiger verfügbarer Standort*.
    - **Azure-Suchtarif**: Free F (3 Indizes) (*Wenn dieser Tarif nicht verfügbar ist, wählen Sie „Standard S“ (50 Indizes) aus*).
    - **Durch Aktivieren dieses Kontrollkästchens bestätige ich, dass ich die Bedingungen im Hinweis zu verantwortungsvoller KI überprüft habe und diese anerkenne.** : *Ausgewählt*.

    > **Hinweis**: Wenn Sie bereits eine kostenlose **Azure Cognitive Search**-Ressource bereitgestellt haben, können Sie mit Ihrem Kontingent möglicherweise keine weitere derartige Ressource erstellen. In diesem Fall wählen Sie einen anderen Tarif als **Free F** aus.

1. Klicken Sie auf **Überprüfen und erstellen**, und klicken Sie dann auf **Erstellen**. Warten Sie auf die Bereitstellung des Sprachdiensts, der Ihre benutzerdefinierte „Fragen und Antworten“-Wissensdatenbank unterstützt.

1. Öffnen Sie auf einer neuen Browserregisterkarte das Language Studio-Portal unter [https://language.azure.com](https://language.azure.com?azure-portal=true), und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.

1. Wenn Sie zur Auswahl einer Sprachressource aufgefordert werden, wählen Sie die folgenden Einstellungen aus:
    - **Azure-Verzeichnis**: Das Azure-Verzeichnis, das Ihr Abonnement enthält.
    - **Azure-Abonnement**: Ihr Azure-Abonnement.
    - **Sprachressource**: Die Sprachressource, die Sie zuvor erstellt haben.

1. Sollten Sie ***nicht*** zur Auswahl einer Sprachressource aufgefordert, kann dies daran liegen, dass Ihr Abonnement mehrere Sprachressourcen enthält. Gehen Sie in diesem Fall folgendermaßen vor:
    1. Klicken Sie auf der Leiste oben auf die Schaltfläche **Einstellungen (&#9881;)**.
    2. Gehen Sie auf der Seite **Einstellungen** zur Registerkarte **Ressourcen**.
    3. Wählen Sie die soeben erstellte Sprachressource aus, und klicken Sie auf **Switch resource** (Ressource wechseln).
    4. Klicken Sie oben auf der Seite auf **Language Studio**, um zur Startseite von Language Studio zurückzukehren.

1. Wählen Sie oben im Language Studio-Portal im Menü **Neu erstellen** die Option **Custom question answering** (Benutzerdefiniertes „Fragen und Antworten“-Feature) aus.

1. Wählen Sie auf der Seite **Spracheinstellung für Ressource *Ihre Ressource* auswählen** die **Option Ich möchte die Sprache auswählen, wenn ich ein Projekt in dieser Ressource erstelle** aus, und klicken Sie dann auf **Weiter**.

1. Geben Sie auf der Seite **Enter basic information** (Grundlegende Informationen eingeben) die folgenden Informationen ein, und klicken Sie auf **Weiter**:
    - **Sprachressource**: *Wählen Sie Ihre Sprachressource aus*.  
    - **Azure Search-Ressource**: *Wählen Sie Ihre Azure Search-Ressource aus*.
    - **Name**: MargiesTravel
    - **Beschreibung**: Eine einfache Wissensdatenbank
    - **Ausgangssprache**: Englisch
    - **Standardantwort, wenn keine Antwort zurückgegeben wird**: Keine Antwort gefunden

1. Klicken Sie auf der Seite **Überprüfen und fertigstellen** auf **Projekt erstellen**.

1. Sie gelangen zur Seite **Quellen verwalten**. Klicken Sie auf **&#65291;Quelle hinzufügen**, und wählen Sie **URLs** aus.

1. Klicken Sie im Feld **URLs hinzufügen** auf **+ URL hinzufügen**. Geben Sie Folgendes ein, und wählen Sie **Alle hinzufügen** aus:
    - **Name der URL**: MargiesKB
    - **URL**: `https://raw.githubusercontent.com/MicrosoftLearning/AI-900-AIFundamentals/main/data/qna/margies_faq.docx`
    - **Klassifizieren der Dateistruktur**: *Automatische Erkennung*. 

## Bearbeiten der Wissensdatenbank

Ihre Wissensdatenbank basiert auf den Angaben im FAQ-Dokument und einigen vordefinierten Antworten. Sie können diese durch benutzerdefinierte Frage-Antwort-Paare ergänzen.

1. Klicken Sie im linken Bereich auf **Edit knowledge base** (Wissensdatenbank bearbeiten). Klicken Sie dann auf **+ Add question pair** (+ Fragepaar hinzufügen).

1. Geben Sie im Feld **Fragen** `Hello` ein, und klicken Sie dann auf **Änderungen übermitteln**.

1. Klicken Sie auf **+ Alternativen Ausdruck hinzufügen**, und geben Sie ein `Hi`. Klicken Sie dann auf **Änderungen übermitteln**.

1. Geben Sie im Feld **Answer and prompts** (Antworten und Äußerungen) `Hello` ein. Behalten Sie diese **Quelle** bei: Editorial.

1. Klicken Sie auf **Submit**(Senden). Klicken Sie dann oben auf der Seite auf **Änderungen speichern**. Möglicherweise müssen Sie die Größe Ihres Fensters ändern, um die Schaltfläche zu sehen.

## Trainieren und Testen der Wissensdatenbank

Da Sie nun über eine Wissensdatenbank verfügen, können Sie diese testen.

1. Klicken Sie oben auf der Seite auf die Option **Testen**, um Ihre Wissensdatenbank zu testen.

1. Geben Sie im Testbereich unten die Nachricht *Hi* ein. Die Antwort **Hello** sollte zurückgegeben werden.

1. Geben Sie im Testbereich unten die Meldung *I want to book a flight* (Ich möchte einen Flug buchen) ein. Es sollte eine entsprechende Antwort aus den häufig gestellten Fragen zurückgegeben werden.

    > **Hinweis**: Die Antwort enthält eine *kurze Antwort* sowie eine ausführlichere *Antwortpassage* – die Antwortpassage zeigt den vollständigen Text des FAQ-Dokuments für die am ehesten zutreffende Frage, während die Kurzantwort intelligent aus der Passage extrahiert wird. Sie können mithilfe des Kontrollkästchens **Display short answer** (Kurzantwort anzeigen) am oberen Rand des Testbereichs steuern, ob die Kurzantwort in der Antwort angezeigt wird.

1. Versuchen Sie eine andere Frage, z. B. *How can I cancel a reservation?* (Wie kann ich eine Reservierung stornieren?).

1. Wenn Sie mit dem Testen der Wissensdatenbank fertig sind, klicken Sie auf **Testen**, um den Testbereich zu schließen.

## Erstellen eines Bots für die Wissensdatenbank

Die Wissensdatenbank bietet einen Back-End-Dienst, den Clientanwendungen nutzen können, um Fragen über eine Art von Benutzeroberfläche zu beantworten. Bei diesen Clientanwendungen handelt es sich in der Regel um Bots. Um die Wissensdatenbank für einen Bot zur Verfügung zu stellen, müssen Sie sie als Dienst veröffentlichen, auf den über HTTP zugegriffen werden kann. Mit dem Azure Bot Service können Sie dann einen Bot erstellen und hosten, der die Wissensdatenbank nutzt, um Benutzerfragen zu beantworten.

1. Klicken Sie links auf der „Language Studio“-Seite auf **Deploy knowledge base** (Wissensdatenbank bereitstellen).

1. Klicken Sie oben auf der Seite auf **Bereitstellen**. In einem Dialogfeld wird gefragt, ob Sie das Projekt bereitstellen möchten. Klicken Sie auf **Bereitstellen**.

1. Nachdem der Dienst bereitgestellt wurde, klicken Sie auf **Einen Bot erstellen**. Dadurch wird das Azure-Portal auf einer neuen Browserregisterkarte geöffnet, sodass Sie einen Web-App-Bot in Ihrem Azure-Abonnement erstellen können.

1. Erstellen Sie im Azure-Portal einen Web-App-Bot. (Möglicherweise wird eine Warnmeldung angezeigt, um zu überprüfen, ob die Quelle der Vorlage vertrauenswürdig ist. Sie müssen keine Maßnahmen für diese Nachricht ergreifen.) Fahren Sie fort, indem Sie die folgenden Einstellungen aktualisieren:

    - **Projektdetails**
        - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
        - **Ressourcengruppe**: *Dies ist die Ressourcengruppe, die Ihre Sprachressource enthält.*
    - **Instanzendetails**
        - **Ressourcengruppenstandort:** *Der gleiche Standort wie für Ihren Sprachdienst*
    - **Azure Bot**
        - **Bot-Handle:** *Ein eindeutiger Name für Ihren Bot* (*vorab aufgefüllt*)
    - **Tarif auswählen**
        - **Tarif:** Free (F0) (Möglicherweise müssen Sie *Plan ändern* auswählen)
    - **Microsoft-App-ID**
        - **Erstellungstyp:** *Wählen Sie „Neue benutzerseitig zugewiesene verwaltete Identität erstellen“ aus.* 

5. Wählen Sie **Weiter: Web-App >** aus, um die Einstellungen weiter zu aktualisieren. 
    - **App Service**
        - **App-Name**: *Genau wie bei **Bot-Handle**, wobei **.azurewebsites.net** automatisch angefügt wird*.
        - **SDK-Sprache**: *Wählen Sie entweder C# oder Node.js aus*.
    - **App Service-Plan**
        - **Erstellungstyp:** *Wählen Sie „Neuen App Service-Plan erstellen“ aus.*
    - **App-Einstellungen**
        - **Sprachressourcenschlüssel:** *Sie müssen Ihren Sprachressourcenschlüssel kopieren und hier einfügen.* 
        
        > **Hinweis** Um zu Ihrem Sprachressourcenschlüssel zu navigieren, öffnen Sie [https://portal.azure.com](https://portal.azure.com?azure-portal=true). Klicken Sie auf der Startseite auf *Ressourcengruppen*, und suchen Sie die Ressourcengruppe, in der Sie Ihre Sprachressource erstellt haben. Wählen Sie Ihre Sprachressource aus, und navigieren Sie zum linken Menü. Wählen Sie dann *Schlüssel und Endpunkt* aus. Kopieren Sie einen der Schlüssel. 

    -  
        - **Projektname der Sprache:** MargiesTravel
        - **Hostname des Sprachdienstendpunkts:** *Vorab aufgefüllt mit Ihrem Sprachdienstendpunkt*
    - **Sprachdienstdetails**
        - **Abonnement-ID:** *Vorab aufgefüllt mit Ihrer Abonnement-ID*
        - **Ressourcengruppenname:** *Vorab aufgefüllt mit dem Namen Ihrer Ressourcengruppe*
        - **Kontoname:** *Vorab aufgefüllt mit Ihrem Kontonamen*

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Warten Sie darauf, dass Ihr Bot erstellt wird (das Benachrichtigungssymbol oben rechts, das wie eine Glocke aussieht, wird animiert, während Sie warten). Klicken Sie dann in der Benachrichtigung, dass die Bereitstellung abgeschlossen ist, auf **Go to resource** (Zur Ressource wechseln) (oder klicken Sie alternativ auf der Startseite auf **Resource groups** (Ressourcengruppen), öffnen Sie die Ressourcengruppe, in der Sie den Web-App-Bot erstellt haben, und klicken Sie darauf).

1. Suchen Sie im linken Bereich Ihres Bots nach den **Einstellungen**, klicken Sie auf **In Webchat testen**, und warten Sie, bis der Bot die Nachricht **Hello and Welcome** (Hallo und willkommen) anzeigt (die Initialisierung kann einige Sekunden dauern).

1. Verwenden Sie die Testchatschnittstelle, um sicherzustellen, dass Ihr Bot Fragen aus Ihrer Wissensdatenbank wie erwartet beantwortet. Versuchen Sie z. B. die Übermittlung von *I need to cancel my hotel* (Ich muss mein Hotel stornieren).

Experimentieren Sie mit dem Bot. Wahrscheinlich werden Sie feststellen, dass er Fragen aus den häufig gestellten Fragen recht genau beantworten kann, aber es wird nur begrenzt in der Lage sein, Fragen zu interpretieren, für die er nicht trainiert wurde. Sie können Language Studio jederzeit zur Bearbeitung der Wissensdatenbank verwenden, um sie zu verbessern und dann erneut zu veröffentlichen.

## Weitere Informationen

- Weitere Informationen zum „Fragen und Antworten“-Dienst finden Sie in [der Dokumentation.](https://docs.microsoft.com/azure/cognitive-services/language-service/question-answering/overview)
- Weitere Informationen zum Microsoft Bot Service finden Sie auf der [Azure Bot Service-Seite](https://azure.microsoft.com/services/bot-service/).
