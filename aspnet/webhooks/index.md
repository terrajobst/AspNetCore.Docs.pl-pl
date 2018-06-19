---
uid: webhooks/index
title: Omówienie elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Wprowadzenie do elementów Webhook ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573320"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="9d490-103">Przegląd elementów Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9d490-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="9d490-104">Elementów Webhook to lekkie wzorzec HTTP zapewnienie modelu prostego pub/sub dla połączeń ze sobą usług interfejsów API sieci Web i SaaS.</span><span class="sxs-lookup"><span data-stu-id="9d490-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="9d490-105">W przypadku zdarzeń w usłudze powiadomienie jest wysyłane w postaci żądanie HTTP POST do subskrybentów w zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="9d490-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="9d490-106">Żądanie POST zawiera informacje dotyczące zdarzeń, dzięki czemu odbiorcy do działania w związku z tym.</span><span class="sxs-lookup"><span data-stu-id="9d490-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="9d490-107">Ze względu na ich uproszczenia elementów Webhook są już udostępniane przez wiele usług w tym [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)i o wiele więcej.</span><span class="sxs-lookup"><span data-stu-id="9d490-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="9d490-108">Na przykład elementu WebHook może oznaczać, że plik został zmieniony w [Dropbox](http://dropbox.com/), lub zmień kod został zatwierdzony w serwisie GitHub lub płatność została zainicjowana w [PayPal](http://www.paypal.com/), lub karta została utworzona w [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="9d490-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="9d490-109">Możliwości są nieograniczone!</span><span class="sxs-lookup"><span data-stu-id="9d490-109">The possibilities are endless!</span></span>

<span data-ttu-id="9d490-110">Microsoft ASP.NET WebHooks ułatwia wysyłać i odbierać elementów Webhook jako część aplikacji ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="9d490-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="9d490-111">Po stronie odbiorczej udostępnia wspólnego modelu do odbierania i przetwarzania elementów Webhook z dowolnej liczby dostawców elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="9d490-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="9d490-112">Pochodzi fabrycznej z obsługą [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pchacza](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) i [Zendesk](https://www.zendesk.com/) , ale ułatwia dodawanie pomocy technicznej, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9d490-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="9d490-113">Po stronie wysyłającej zapewnia obsługę do zarządzania i przechowywania subskrypcje również, jak w przypadku wysyłania powiadomień o zdarzeniach do prawidłowego zestawu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="9d490-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="9d490-114">Pozwala na definiowanie własnych zestawów wydarzeń, subskrybentów może subskrybować i powiadamiać użytkowników, w przypadku elementów.</span><span class="sxs-lookup"><span data-stu-id="9d490-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="9d490-115">Dwie części można siebie lub od siebie w zależności od danego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="9d490-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="9d490-116">Jeśli chcesz otrzymywać elementów Webhook innych usług, a następnie można użyć tylko część odbiornika; Jeśli chcesz ujawniać elementów Webhook dla innych użytkowników, aby korzystać z, następnie możesz to zrobić tylko.</span><span class="sxs-lookup"><span data-stu-id="9d490-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="9d490-117">Kod platformy ASP.NET Web API 2 i ASP.NET MVC 5 i jest dostępna jako [OSS w serwisie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="9d490-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="9d490-118">Przegląd elementów Webhook</span><span class="sxs-lookup"><span data-stu-id="9d490-118">WebHooks Overview</span></span>

<span data-ttu-id="9d490-119">Elementów Webhook jest wzorzec, co oznacza, że zależy, jak jest używany z usługi do usługi, ale podstawową koncepcją jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="9d490-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="9d490-120">Można traktować elementów Webhook jako model prostego pub/sub której użytkownik może subskrybować zdarzenia pojawia się w innym miejscu.</span><span class="sxs-lookup"><span data-stu-id="9d490-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="9d490-121">Powiadomienia o zdarzeniach są propagowane jako zawierający informacje o samym zdarzeniu żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9d490-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="9d490-122">Zazwyczaj żądania HTTP POST zawiera obiekt JSON lub ustaleniami nadawcy elementu WebHook wraz z informacjami dotyczącymi zdarzeń, co powoduje elementu WebHook do wyzwolenia danych formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="9d490-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="9d490-123">Na przykład przykład treści żądania POST elementu WebHook z [GitHub](http://www.github.com/) wygląda podobnie do następującej wyniku nowy problem otwierany w szczególności repozytorium:</span><span class="sxs-lookup"><span data-stu-id="9d490-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="9d490-124">W celu zapewnienia elementu WebHook w rzeczywistości od zamierzonego nadawcy, żądania POST jest zabezpieczony w inny sposób i następnie weryfikowany przez odbiornik.</span><span class="sxs-lookup"><span data-stu-id="9d490-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="9d490-125">Na przykład [elementów Webhook GitHub](https://developer.github.com/webhooks/) obejmuje *X Centrum podpisu* nagłówek HTTP o treści żądania, która jest sprawdzana przez odbiornika, tak aby nie trzeba martwić się o jego skrót.</span><span class="sxs-lookup"><span data-stu-id="9d490-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="9d490-126">Przepływ elementu WebHook przechodzi zazwyczaj mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="9d490-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="9d490-127">Nadawca elementu WebHook przedstawia zdarzenia, które klient może zasubskrybować.</span><span class="sxs-lookup"><span data-stu-id="9d490-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="9d490-128">Zdarzenia opisano zauważalne zmiany w systemie, na przykład które nowy element danych został wstawiony, zakończenie procesu lub inny.</span><span class="sxs-lookup"><span data-stu-id="9d490-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="9d490-129">Odbiornik WebHook subskrybuje rejestrując elementu WebHook składające się z czterech rzeczy:</span><span class="sxs-lookup"><span data-stu-id="9d490-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="9d490-130">Identyfikator URI, dla których zaksięgowania powiadamianie o zdarzeniach w formularzu żądania HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="9d490-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="9d490-131">Zestaw filtrów opisujący konkretnego zdarzenia, dla których powinny być uruchamiane elementu WebHook;</span><span class="sxs-lookup"><span data-stu-id="9d490-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="9d490-132">Klucz tajny, który jest używany do podpisywania żądania HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="9d490-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="9d490-133">Dodatkowe dane, które mają być zawarte w żądaniu POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="9d490-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="9d490-134">Może to być na przykład dodatkowe pola nagłówka HTTP lub właściwości zawarte w treści żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9d490-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="9d490-135">Po wystąpieniu zdarzenia znaleziono pasującego rejestracji elementu WebHook i są przesyłane żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9d490-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="9d490-136">Zazwyczaj generowania żądań POST protokołu HTTP, które są zwalniane kilka razy Jeżeli dla jakiegoś powodu nie odpowiada odbiorca lub wyniki żądania HTTP POST w odpowiedzi na błąd.</span><span class="sxs-lookup"><span data-stu-id="9d490-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="9d490-137">Potok przetwarzania elementów Webhook</span><span class="sxs-lookup"><span data-stu-id="9d490-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="9d490-138">Microsoft ASP.NET WebHooks przetwarzania potoku dla przychodzących elementów Webhook wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="9d490-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Potok przetwarzania elementów Webhook ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="9d490-140">Są dwa podstawowe pojęcia tutaj *odbiorcy* i *obsługi*:</span><span class="sxs-lookup"><span data-stu-id="9d490-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="9d490-141">*Odbiorniki* są zobowiązani do obsługi określonego wersję elementu WebHook od danego nadawcy i wymuszania testy zabezpieczeń w celu zapewnienia, że żądania elementu WebHook jest w rzeczywistości od zamierzonego nadawcy.</span><span class="sxs-lookup"><span data-stu-id="9d490-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="9d490-142">*Programy obsługi* są zazwyczaj, gdy kod użytkownika uruchamia przetwarzanie określonego elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="9d490-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="9d490-143">W następujących węzłach te pojęcia są opisane bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="9d490-143">In the following nodes these concepts are described in more details.</span></span>
