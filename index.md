---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
---

# Labs zu den Azure KI-Grundlagen

Dieses Repository enthält die Übungen für das Praxislab zum Microsoft-Kurs [AI-900 *Microsoft Azure Artificial Intelligence*](https://docs.microsoft.com/de-de/learn/certifications/courses/ai-900t00) und den zugehörigen Selbstlernkursen in Microsoft Learn: [Einstieg in künstliche Intelligenz in Azure](https://docs.microsoft.com/learn/paths/get-started-with-artificial-intelligence-on-azure/), [Erstellen von Vorhersagemodellen ohne Code mit Azure Machine Learning](https://docs.microsoft.com/de-de/learn/paths/create-no-code-predictive-models-azure-machine-learning/), [Mehr über maschinelles Sehen in Microsoft Azure](https://docs.microsoft.com/learn/paths/explore-computer-vision-microsoft-azure/), [Verarbeitung natürlicher Sprache](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/), und [Erkunden der Gesprächs-KI](https://docs.microsoft.com/learn/paths/explore-conversational-ai/). Diese Übungen begleiten die Lernmaterialen und erleichtern die praktische Anwendung der beschriebenen Technologien. 

Sie benötigen ein Microsoft Azure-Abonnement für diese Übungen. Falls Sie kein Abonnement von Ihrem Kursleiter erhalten haben, können Sie sich unter [https://azure.microsoft.com](https://azure.microsoft.com) für eine kostenlose Testversion registrieren.

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Übungen |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
