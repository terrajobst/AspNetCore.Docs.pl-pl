---
uid: webhooks/index
title: "Omówienie elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Wprowadzenie do elementów Webhook ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a>Przegląd elementów Webhook ASP.NET

Elementów Webhook to lekkie wzorzec HTTP zapewnienie modelu prostego pub/sub dla połączeń ze sobą usług interfejsów API sieci Web i SaaS. W przypadku zdarzeń w usłudze powiadomienie jest wysyłane w postaci żądanie HTTP POST do subskrybentów w zarejestrowany. Żądanie POST zawiera informacje dotyczące zdarzeń, dzięki czemu odbiorcy do działania w związku z tym.

Ze względu na ich uproszczenia elementów Webhook są już udostępniane przez wiele usług w tym [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)i o wiele więcej. Na przykład elementu WebHook może oznaczać, że plik został zmieniony w [Dropbox](http://dropbox.com/), lub zmień kod został zatwierdzony w serwisie GitHub lub płatność została zainicjowana w [PayPal](http://www.paypal.com/), lub karta została utworzona w [ Trello](http://www.trello.com/). Możliwości są nieograniczone!

Microsoft ASP.NET WebHooks ułatwia wysyłać i odbierać elementów Webhook jako część aplikacji ASP.NET:

* Po stronie odbiorczej udostępnia wspólnego modelu do odbierania i przetwarzania elementów Webhook z dowolnej liczby dostawców elementu WebHook. Pochodzi fabrycznej z obsługą [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pchacza](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) i [Zendesk](https://www.zendesk.com/) , ale ułatwia dodawanie pomocy technicznej, aby uzyskać więcej informacji.

* Po stronie wysyłającej zapewnia obsługę do zarządzania i przechowywania subskrypcje również, jak w przypadku wysyłania powiadomień o zdarzeniach do prawidłowego zestawu subskrybentów. Pozwala na definiowanie własnych zestawów wydarzeń, subskrybentów może subskrybować i powiadamiać użytkowników, w przypadku elementów.

Dwie części można siebie lub od siebie w zależności od danego scenariusza. Jeśli chcesz otrzymywać elementów Webhook innych usług, a następnie można użyć tylko część odbiornika; Jeśli chcesz ujawniać elementów Webhook dla innych użytkowników, aby korzystać z, następnie możesz to zrobić tylko.

Kod platformy ASP.NET Web API 2 i ASP.NET MVC 5 i jest dostępna jako [OSS w serwisie GitHub](https://github.com/aspnet/WebHooks).

## <a name="webhooks-overview"></a>Przegląd elementów Webhook

Elementów Webhook jest wzorzec, co oznacza, że zależy, jak jest używany z usługi do usługi, ale podstawową koncepcją jest taka sama. Można traktować elementów Webhook jako model prostego pub/sub której użytkownik może subskrybować zdarzenia pojawia się w innym miejscu. Powiadomienia o zdarzeniach są propagowane jako zawierający informacje o samym zdarzeniu żądania HTTP POST.

Zazwyczaj żądania HTTP POST zawiera obiekt JSON lub ustaleniami nadawcy elementu WebHook wraz z informacjami dotyczącymi zdarzeń, co powoduje elementu WebHook do wyzwolenia danych formularza HTML. Na przykład przykład treści żądania POST elementu WebHook z [GitHub](http://www.github.com/) wygląda podobnie do następującej wyniku nowy problem otwierany w szczególności repozytorium:

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

W celu zapewnienia elementu WebHook w rzeczywistości od zamierzonego nadawcy, żądania POST jest zabezpieczony w inny sposób i następnie weryfikowany przez odbiornik. Na przykład [elementów Webhook GitHub](https://developer.github.com/webhooks/) obejmuje *X Centrum podpisu* nagłówek HTTP o treści żądania, która jest sprawdzana przez odbiornika, tak aby nie trzeba martwić się o jego skrót.

Przepływ elementu WebHook przechodzi zazwyczaj mniej więcej tak:

* Nadawca elementu WebHook przedstawia zdarzenia, które klient może zasubskrybować. Zdarzenia opisano zauważalne zmiany w systemie, na przykład które nowy element danych został wstawiony, zakończenie procesu lub inny.

* Odbiornik WebHook subskrybuje rejestrując elementu WebHook składające się z czterech rzeczy:

     1. Identyfikator URI, dla których zaksięgowania powiadamianie o zdarzeniach w formularzu żądania HTTP POST;

     2. Zestaw filtrów opisujący konkretnego zdarzenia, dla których powinny być uruchamiane elementu WebHook;

     3. Klucz tajny, który jest używany do podpisywania żądania HTTP POST;

     4. Dodatkowe dane, które mają być zawarte w żądaniu POST protokołu HTTP. Może to być na przykład dodatkowe pola nagłówka HTTP lub właściwości zawarte w treści żądania HTTP POST.

* Po wystąpieniu zdarzenia znaleziono pasującego rejestracji elementu WebHook i są przesyłane żądania HTTP POST. Zazwyczaj generowania żądań POST protokołu HTTP, które są zwalniane kilka razy Jeżeli dla jakiegoś powodu nie odpowiada odbiorca lub wyniki żądania HTTP POST w odpowiedzi na błąd.

## <a name="webhooks-processing-pipeline"></a>Potok przetwarzania elementów Webhook

Microsoft ASP.NET WebHooks przetwarzania potoku dla przychodzących elementów Webhook wygląda następująco:

![Potok przetwarzania elementów Webhook ASP.NET](_static/WebHookReceivers.png)

Są dwa podstawowe pojęcia tutaj *odbiorcy* i *obsługi*:

* *Odbiorniki* są zobowiązani do obsługi określonego wersję elementu WebHook od danego nadawcy i wymuszania testy zabezpieczeń w celu zapewnienia, że żądania elementu WebHook jest w rzeczywistości od zamierzonego nadawcy.

* *Programy obsługi* są zazwyczaj, gdy kod użytkownika uruchamia przetwarzanie określonego elementu WebHook.

W następujących węzłach te pojęcia są opisane bardziej szczegółowo.
