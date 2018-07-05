---
uid: webhooks/index
title: Omówienie elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Wprowadzenie do elementów Webhook programu ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.openlocfilehash: abb24398f6db3b01921e314a4288294fed41a5ef
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380320"
---
# <a name="aspnet-webhooks-overview"></a>Omówienie elementów Webhook programu ASP.NET

Elementy Webhook jest lekkiego wzorca HTTP, podając modelu publikowania/subskrybowania proste dołączenie razem interfejsów API sieci Web i usług SaaS. Przypadku wystąpienia zdarzenia w usłudze, powiadomienie jest wysyłane w formularzu żądania HTTP POST do zarejestrowanych subskrybentów. Żądanie POST zawiera informacje o zdarzeniu, co umożliwia odbiorcy podejmowanie odpowiednich działań.

Ze względu na ich prostotę, elementy Webhook są już udostępniane przez dużą liczbę usług w tym [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [usługi MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)i wiele innych. Na przykład element WebHook można wskazać, że plik został zmieniony w [Dropbox](http://dropbox.com/), zmiany kodu został zatwierdzony w usłudze GitHub lub płatność została zainicjowana w [PayPal](http://www.paypal.com/), lub karta została utworzona w [ Trello](http://www.trello.com/). Możliwości są nieograniczone!

Microsoft ASP.NET WebHooks sprawia, że łatwiej wysyłać i odbierać elementy Webhook w ramach aplikacji ASP.NET:

* Po stronie odbierającej zapewnia wspólny model odbieranie i przetwarzanie elementów Webhook z dowolnej liczby dostawców elementu WebHook. Chodzi gotowych dzięki obsłudze [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Usługi pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) i [Zendesk](https://www.zendesk.com/) , ale można łatwo dodać więcej obsługę.

* Po stronie wysyłającej zapewnia obsługę do zarządzania i przechowywania subskrypcje również, jak w przypadku wysyłania powiadomień o zdarzeniach do odpowiedniego zestawu subskrybentów. Dzięki temu można zdefiniować własny zestaw zdarzeń czy subskrybenci można zasubskrybować i powiadamiaj ich sytuacji rzeczy.

Dwie części można ze sobą lub od siebie w zależności od scenariusza. Jeśli chcesz odbierać elementy Webhook z innych usług, wówczas można użyć tylko części odbiorcy; Jeśli chcesz udostępnić elementy Webhook dla innych użytkowników do korzystania, następnie należy to.

Kod jest przeznaczony dla platformy ASP.NET Web API 2 i ASP.NET MVC 5 i jest dostępna jako [OSS w serwisie GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Omówienie elementów Webhook

Elementy Webhook jest wzorzec, co oznacza, że zależy to korzystania z usługi do usługi, ale podstawowa koncepcja jest taka sama. Można traktować elementów Webhook jako modelu publikowania/subskrybowania proste której użytkownik może subskrybować zdarzenia dzieje się w innym miejscu. Powiadomienia o zdarzeniach są propagowane na żądania HTTP POST zawierających informacje o samym zdarzeniu.

Żądanie HTTP POST zawiera zazwyczaj obiekt JSON lub danych formularza HTML, określane przez nadawcę elementu WebHook, w tym informacje o zdarzeniu, powodując elementu WebHook do wyzwalania. Na przykład przykład treści żądania POST elementu WebHook z [GitHub](http://www.github.com/) wygląda podobnie do następującego wyniku nowy problem jest otwarty w określonym repozytorium:

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

Aby upewnij się, że element WebHook w rzeczywistości od zamierzonego nadawcy, żądania POST jest zabezpieczony w jakiś sposób i następnie weryfikowany przez odbiorcę. Na przykład [elementy Webhook GitHub](https://developer.github.com/webhooks/) obejmuje *X-Hub-podpisu* nagłówka HTTP z treści żądania, która jest sprawdzana przez odbiornika, więc nie trzeba już martwić się o jego skrót.

Przepływ elementu WebHook wykracza ogólnie podobny do poniższego:

* Nadawca elementu WebHook uwidacznia zdarzenia, które może być subskrybowana przez klienta. Zdarzenia opisują zauważalne zmiany w systemie, na przykład które nowy element danych został wstawiony, że proces został zakończony lub coś innego.

* Subskrybuje odbiorcy elementu WebHook, rejestrując element WebHook, składający się z czterech elementów:

     1. Identyfikator URI, dla których zaksięgowania powiadomień o zdarzeniach w formularzu żądania HTTP POST;

     2. Zestaw filtrów opisujące określonych zdarzeń, dla których powinny być uruchamiane elementu WebHook;

     3. Klucz tajny, który jest używany do podpisywania żądań POST protokołu HTTP;

     4. Dodatkowe dane, które mają być zawarte w żądaniu POST protokołu HTTP. Może to być na przykład dodatkowe pola nagłówka HTTP lub właściwości zawarte w treści żądania HTTP POST.

* Po wystąpieniu zdarzenia zostaną znalezione dopasowania rejestracji elementu WebHook i przesłaniu żądania HTTP POST. Zazwyczaj generowanie żądań HTTP POST zostaną ponowione kilka razy, jeśli dla jakiegoś powodu, że odbiorca nie odpowiada lub wyniki żądania HTTP POST w odpowiedzi na błąd.

## <a name="webhooks-processing-pipeline"></a>Potok przetwarzania elementów Webhook

Potok przetwarzania Microsoft ASP.NET WebHooks dla przychodzących elementów Webhook wygląda następująco:

![Potok przetwarzania elementów Webhook programu ASP.NET](_static/WebHookReceivers.png)

Są dwa kluczowe pojęcia tutaj *odbiorniki* i *obsługi*:

* *Odbiorniki* jest odpowiedzialny za obsługę określoną wersję elementu WebHook z danym nadawcy i wymuszanie sprawdza zabezpieczeń, upewnij się, że żądanie elementu WebHook w rzeczywistości od zamierzonego nadawcy.

* *Programy obsługi* są zazwyczaj, gdy kod użytkownika uruchamia przetwarzanie określonego elementu WebHook.

W następujących węzłach te pojęcia są opisane bardziej szczegółowo.
