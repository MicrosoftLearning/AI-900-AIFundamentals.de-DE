## Lokale Entwicklung 

Wenn Sie an Ihrem lokalen Computer arbeiten, können Sie Ihre Umgebung mit den folgenden Schritten für diese Labs vorbereiten.  

### C++ Redistributable 
1. Laden Sie das [Visual C++ Redistributable (x64)](https://aka.ms/vs/16/release/vc_redist.x64.exe)-Paket herunter, und installieren Sie es. 

### Python und erforderliche Pakete 
1. Installieren Sie [Python 3.6.1](https://www.python.org/downloads/release/python-361/)  
   - **Wichtig**: Wählen Sie die entsprechenden Optionen aus, um Python zur PATH-Variablen hinzuzufügen und als standardmäßige Python-Umgebung zu registrieren. 
2. Öffnen Sie nach der Installation eine *Eingabeaufforderung*, und geben Sie den folgenden Befehl ein, um die erforderlichen Pakete zu installieren: 

> pip install ipython jupyter matplotlib pillow requests azure-cognitiveservices-vision-computervision azure-cognitiveservices-vision-customvision azure-cognitiveservices-vision-face azure-cognitiveservices-language-textanalytics azure.cognitiveservices.speech azure_ai_formrecognizer 

### Visual Studio Code 
1. Falls Sie Visual Studio Code noch nicht installiert haben, können Sie es [hier herunterladen](https://code.visualstudio.com/Download). Starten Sie Visual Studio Code nach der Installation, suchen Sie auf der Registerkarte „Erweiterungen“ (STRG+UMSCHALT+X) nach der **Python**-Erweiterung von Microsoft, und installieren Sie sie.

2. Öffnen Sie in Visual Studio Code ein neues Terminal, geben Sie **git clone https://github.com/MicrosoftLearning/AI-900DE-Microsoft-Azure-AI-Fundamentals** ein, und drücken Sie die *Eingabetaste*. 

