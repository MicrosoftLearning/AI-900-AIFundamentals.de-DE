---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
---

# <a name="azure-ai-fundamentals-exercises"></a>Labs zu den Azure KI-Grundlagen

Diese Praxisübungen sind dafür konzipiert, die Schulungsinhalte auf [Microsoft Learn](https://docs.microsoft.com/training/) zu ergänzen.

Sie benötigen ein Microsoft Azure-Abonnement für diese Übungen. Sie können sich unter [https://azure.microsoft.com](https://azure.microsoft.com) für eine kostenlose Testversion registrieren.

## <a name="labs"></a>Labs

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %}
| Modul | Labor |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} – {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
