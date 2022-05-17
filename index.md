---
title: Online gehostete Anweisungen
permalink: index.html
layout: home
ms.openlocfilehash: b85af520a10e63a2f9a5696db03bfd946aff968f
ms.sourcegitcommit: 1ef64e3008a439d0d0bb3d93a27d3df68d3d64a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2022
ms.locfileid: "140688691"
---
# <a name="azure-ai-fundamentals-exercises"></a>Labs zu den Azure KI-Grundlagen

Verwenden Sie die nachstehenden Links, um die Übungen für das Praxislab zum Microsoft-Kurs [AI-900 *Microsoft Azure – KI-Grundlagen*](https://docs.microsoft.com/learn/certifications/courses/ai-900t00) durchzuführen.

Sie benötigen ein Microsoft Azure-Abonnement für diese Übungen. Falls Sie kein Abonnement von Ihrem Kursleiter erhalten haben, können Sie sich unter [https://azure.microsoft.com](https://azure.microsoft.com) für eine kostenlose Testversion registrieren.

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions'" %}
| Übungen |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
