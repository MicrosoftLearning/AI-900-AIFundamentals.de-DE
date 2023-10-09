Mit Content Safety Studio können Sie untersuchen, wie Text- und Bildinhalte moderiert werden können. Sie können Tests für Beispieltext oder -bilder ausführen und für jede Kategorie einen Schweregrad von sicher bis hoch erhalten. Sie können schnell herausfinden, wie der Content Safety KI-Dienst funktioniert und was er kann. 

In dieser Labübung erstellen Sie eine Azure KI Services-Ressource mit mehreren Diensten im Azure-Portal und überprüfen deren Endpunkt und Schlüssel. Anschließend verwenden Sie Content Safety Studio, um die Funktionalität des Content Safety KI-Diensts zu erkunden. 

## Erstellen einer Azure KI Services-Ressource

1.  Öffnen Sie auf einer anderen Browserregisterkarte das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com?azure-portal=true), und melden Sie sich mit Ihrem Microsoft-Konto an.
1.  Klicken Sie auf die Schaltfläche **&#65291;Ressource erstellen** und suchen Sie nach *Azure KI-Dienste*. Wählen Sie **Erstellen** und dann **Azure KI Services**-Plan aus. Sie werden zu einer Seite weitergeleitet, um eine Azure KI Services-Ressource zu erstellen. Konfigurieren Sie sie mit den folgenden Einstellungen:
- **Abonnement**: Ihr Azure-Abonnement.
- **Ressourcengruppe**: Wählen Sie eine geeignete Ressourcengruppe aus oder erstellen Sie sie.
- **Region**: Wählen Sie eine beliebige verfügbare Region aus.
- **Name**: Geben Sie einen eindeutigen Namen ein.
- **Tarif**: F0 
2.  Überprüfen und erstellen Sie die Ressource und warten Sie, bis die Bereitstellung abgeschlossen ist. 

## Zuordnen der Ressource zu Content Safety Studio 
Öffnen Sie Content Safety Studio in einem separaten Browser-Tab und melden Sie sich an. Der Bildschirm „Erste Schritte“ wird angezeigt.

1.  Klicken Sie oben rechts im Menü auf das Zahnradsymbol für die Einstellungen.
2.  Wählen Sie die soeben erstellte Azure KI Services-Ressource aus und klicken Sie dann auf Ressource verwenden.
3.  Zeigen Sie den Endpunkt und Schlüssel für Ihre Ressource an. Scrollen Sie dafür bei Bedarf nach unten. Wenn Sie im Azure-Portal nachsehen, werden Sie feststellen, dass dies der Endpunkt und der Schlüssel für Ihre Ressource ist. Das bedeutet, dass Sie Content Safety Studio Ihre Ressource erfolgreich zugeordnet haben.
4.  Klicken Sie auf Content Safety Studio, um zur Startseite zurückzukehren. Klicken Sie unter Textinhalt Moderation auf Ausprobieren.
5.  Klicken Sie unter Ausführen eines einfachen Tests auf Sichere Inhalte. Beachten Sie, dass im Feld darunter Text angezeigt wird. 
6.  Klicken Sie auf Run test. 
7.  Überprüfen Sie im Bereich Ergebnisse die Ergebnisse. Es gibt vier Schweregrade von sicher bis hoch und vier Arten von schädlichen Inhalten. Stuft der Content Safety KI-Dienst dieses Beispiel als akzeptabel ein oder nicht? 
8.  Versuchen Sie es mit einem anderen Beispiel. Wählen Sie den Text unter Gewalttätiger Inhalt mit Schreibfehlern aus. Überprüfen Sie, ob der Inhalt im Feld darunter angezeigt wird.
9.  Klicken Sie auf Test ausführen und überprüfen Sie die Ergebnisse im Ergebnisbereich unten. Ist dieses Beispiel akzeptabel? Woran liegt es, wenn dies nicht der Fall ist?

Sie können Tests für alle bereitgestellten Beispiele ausführen und dann die Ergebnisse überprüfen.

Wenn Sie fertig sind, löschen Sie die Azure KI Services-Ressource im Azure-Portal. 
