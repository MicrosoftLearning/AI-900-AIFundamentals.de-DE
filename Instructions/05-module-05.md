---
lab:
  title: Erkunden von generativer KI mit Bing Copilot
---

In dieser Übung erkunden Sie generative KI mit Bing Copilot. 

## Vorbereitung
Sie benötigen ein persönliches Microsoft-Konto. Wenn Sie über keines verfügen, wechseln Sie zu [signup.live.com](https://signup.live.com/signup?azure-portal=true), um sich zu registrieren.

# Erkunden von generativer KI mit Bing

1. Öffnen Sie [Bing.com](https://www.bing.com?azure-portal=true) und melden Sie sich mit Ihrem persönlichen Microsoft-Konto an.

**Hinweis**: Sie können sich zwar mit Ihrem Geschäfts-, Schul- oder Unikonto anmelden, jedoch wird dann eine etwas andere Benutzeroberfläche angezeigt als bei einer Anmeldung mit Ihrem persönlichen Konto. Wenn Sie Ihr Geschäfts-, Schul- oder Unikonto verwenden, wird Bing Enterprise-Chat angezeigt. 

1. Wählen Sie im Menü im oberen Bildschirmbereich **Chat** aus. Danach gelangen Sie zu Bing Copilot, der generative KI verwendet, um Suchergebnisse zu erhalten. Dies bedeutet, dass Bing Copilot im Gegensatz zur normalen Suche, die vorhandene Inhalte zurückgibt, neue Antworten basierend auf natürlicher Sprache und den Informationen des Webs zusammenstellen kann.  
    
1. Am unteren Rand des Bildschirms sehen Sie das Fenster **Fragen Sie mich etwas**. Wenn Sie Prompts in das Fenster eingeben, verwendet Bing Copilot den gesamten Unterhaltungsthread, um Antworten zurückzugeben. Versuchen wir beispielsweise, mehrere Fragen zum Thema Reisen zu stellen. 

## Verwenden von Prompts zum Generieren von Antworten

1. Geben Sie einen Prompt ein: `What are 3 pros and cons of traveling in the winter?`. Vor der Antwort werden die Optionen *Suchen nach:* und *Antworten werden für Sie generiert...* angezeigt. Das Modell verwendet die gesuchten Antworten als Basis, um ursprüngliche Antworten zu generieren. Beachten Sie, dass am Ende der Antwort Links zu den jeweiligen Quellen angezeigt werden. 

![Ein Screenshot der Antwort von Bing Copilot auf einen Prompt zum Thema Reisen mit drei Aufzählungszeichen für Vorteile und drei Aufzählungszeichen für Nachteile.](../media/generative-ai/bing-copilot-response-traveling.png) 

**Hinweis**: Wenn die Nachricht *Antworten werden für Sie generiert...* nicht angezeigt oder keine Aufzählung angezeigt wird, wurde Bing Copilot noch nicht gestartet. Sie müssen zum Anmeldemenü zurückkehren und das aktuelle Konto, das Sie verwenden, mit einem persönliches Konto verbinden. 
 
1. Geben Sie einen Prompt ein: `Find me 3 more pros`. Mit diesem Prompt möchten Sie drei weitere Vorteile des Reisens im Winter sehen, die noch nicht aufgeführt wurden. Beachten Sie, dass Sie mit diesem Prompt Bing Copilot auffordern, zwei Dinge zu tun, die eine normale Suche nicht kann: die vorherige Chatantwort zu nutzen, um die entsprechenden Elemente in der neuen Antwort auszuschließen, und das Thema des vorherigen Chats aufzugreifen, ohne dass es explizit angegeben war. 

1. Geben Sie einen Prompt ein: `Where are 3 places I can go to find fewer crowds?`. 

**Hinweis**: Während Bing Copilot eine zugehörige Antwort gibt, kann er frühere „Erinnerungen“ des Unterhaltungsthreads löschen, während er fortgesetzt wird. Daher beziehen sich die Antworten, die Sie erhalten, möglicherweise nicht auf Reisen im Winter. Dies liegt vor allem an Einschränkungen bei der Tokeneingabe. Wenn sich der Chat an frühere Teile einer Unterhaltung „erinnert“, liegt dies daran, dass eine bestimmte Anzahl an Token aus der Unterhaltung gespeichert wurde. Wenn neue Token über Ihren neuen Prompt und weitere Antworten hinzugefügt werden, löscht der Chat ältere Token. 

1. Die Schaltfläche **Neues Thema** neben dem Chatfenster ist nützlich, um den vorherigen Unterhaltungsthread zu löschen, sodass Ihre neuen Antworten nicht auf dem vorherigen Thema basieren. Verwenden Sie das Symbol **Neues Thema** neben dem Chatfenster, um den Nachrichtenverlauf zu löschen. 

## Erkunden der Bildgenerierung

1. Sehen wir uns nun ein Beispiel für die Bildgenerierung an. Geben Sie einen Prompt ein: `Create an image of an elephant eating a hamburger`. Beachten Sie, dass die Meldung *Ich werde versuchen, das zu erstellen* angezeigt wird, bevor Bing Copilot eine Antwort zurückgibt. 

![Ein Screenshot von Elefanten, die Hamburger essen.](../media/generative-ai/dall-e-elephant.png)

Beachten Sie, dass die Antwort möglicherweise ähnlich, aber nicht gleich aussieht. Dies liegt daran, dass die Antworten unterschiedlich sind.  

1. In der Antwort befindet sich unten der Text „Unterstützt von DALL-E“. DALL-E basiert auf großen Sprachmodellen, während Ihre Eingabe in natürlicher Sprache Bilder generiert. 

1. Kehren Sie zum Bing Copilot-Chat zurück, indem Sie auf das Microsoft Bing-Symbol oben rechts im Bildschirm klicken. 

## Erkunden der Codegenerierung

1. Nun sehen wir uns ein Beispiel für die Codegenerierung und Übersetzung an. Geben Sie einen Prompt ein: `Use Python to create a list`. 

1. Geben Sie den Prompt ein: `Translate that into C#`. Beachten Sie, dass Sie nicht angeben mussten, was „das“ ist, da Bing Copilot weiß, dass es sich auf den Unterhaltungsthread bezieht. 

## Bonus 

1. Geben Sie einen Prompt ein: `What are 3 examples of generative AI helping people?`. Jetzt können Sie Ihre eigenen Copilot-Ideen brainstormen!  

