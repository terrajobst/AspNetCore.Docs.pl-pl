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
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="b46dc-103">Omówienie elementów Webhook programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b46dc-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="b46dc-104">Elementy Webhook jest lekkiego wzorca HTTP, podając modelu publikowania/subskrybowania proste dołączenie razem interfejsów API sieci Web i usług SaaS.</span><span class="sxs-lookup"><span data-stu-id="b46dc-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="b46dc-105">Przypadku wystąpienia zdarzenia w usłudze, powiadomienie jest wysyłane w formularzu żądania HTTP POST do zarejestrowanych subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="b46dc-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="b46dc-106">Żądanie POST zawiera informacje o zdarzeniu, co umożliwia odbiorcy podejmowanie odpowiednich działań.</span><span class="sxs-lookup"><span data-stu-id="b46dc-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="b46dc-107">Ze względu na ich prostotę, elementy Webhook są już udostępniane przez dużą liczbę usług w tym [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [usługi MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)i wiele innych.</span><span class="sxs-lookup"><span data-stu-id="b46dc-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="b46dc-108">Na przykład element WebHook można wskazać, że plik został zmieniony w [Dropbox](http://dropbox.com/), zmiany kodu został zatwierdzony w usłudze GitHub lub płatność została zainicjowana w [PayPal](http://www.paypal.com/), lub karta została utworzona w [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="b46dc-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="b46dc-109">Możliwości są nieograniczone!</span><span class="sxs-lookup"><span data-stu-id="b46dc-109">The possibilities are endless!</span></span>

<span data-ttu-id="b46dc-110">Microsoft ASP.NET WebHooks sprawia, że łatwiej wysyłać i odbierać elementy Webhook w ramach aplikacji ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b46dc-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="b46dc-111">Po stronie odbierającej zapewnia wspólny model odbieranie i przetwarzanie elementów Webhook z dowolnej liczby dostawców elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="b46dc-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="b46dc-112">Chodzi gotowych dzięki obsłudze [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Usługi pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) i [Zendesk](https://www.zendesk.com/) , ale można łatwo dodać więcej obsługę.</span><span class="sxs-lookup"><span data-stu-id="b46dc-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="b46dc-113">Po stronie wysyłającej zapewnia obsługę do zarządzania i przechowywania subskrypcje również, jak w przypadku wysyłania powiadomień o zdarzeniach do odpowiedniego zestawu subskrybentów.</span><span class="sxs-lookup"><span data-stu-id="b46dc-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="b46dc-114">Dzięki temu można zdefiniować własny zestaw zdarzeń czy subskrybenci można zasubskrybować i powiadamiaj ich sytuacji rzeczy.</span><span class="sxs-lookup"><span data-stu-id="b46dc-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="b46dc-115">Dwie części można ze sobą lub od siebie w zależności od scenariusza.</span><span class="sxs-lookup"><span data-stu-id="b46dc-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="b46dc-116">Jeśli chcesz odbierać elementy Webhook z innych usług, wówczas można użyć tylko części odbiorcy; Jeśli chcesz udostępnić elementy Webhook dla innych użytkowników do korzystania, następnie należy to.</span><span class="sxs-lookup"><span data-stu-id="b46dc-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="b46dc-117">Kod jest przeznaczony dla platformy ASP.NET Web API 2 i ASP.NET MVC 5 i jest dostępna jako [OSS w serwisie GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="b46dc-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="b46dc-118">Omówienie elementów Webhook</span><span class="sxs-lookup"><span data-stu-id="b46dc-118">WebHooks Overview</span></span>

<span data-ttu-id="b46dc-119">Elementy Webhook jest wzorzec, co oznacza, że zależy to korzystania z usługi do usługi, ale podstawowa koncepcja jest taka sama.</span><span class="sxs-lookup"><span data-stu-id="b46dc-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="b46dc-120">Można traktować elementów Webhook jako modelu publikowania/subskrybowania proste której użytkownik może subskrybować zdarzenia dzieje się w innym miejscu.</span><span class="sxs-lookup"><span data-stu-id="b46dc-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="b46dc-121">Powiadomienia o zdarzeniach są propagowane na żądania HTTP POST zawierających informacje o samym zdarzeniu.</span><span class="sxs-lookup"><span data-stu-id="b46dc-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="b46dc-122">Żądanie HTTP POST zawiera zazwyczaj obiekt JSON lub danych formularza HTML, określane przez nadawcę elementu WebHook, w tym informacje o zdarzeniu, powodując elementu WebHook do wyzwalania.</span><span class="sxs-lookup"><span data-stu-id="b46dc-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="b46dc-123">Na przykład przykład treści żądania POST elementu WebHook z [GitHub](http://www.github.com/) wygląda podobnie do następującego wyniku nowy problem jest otwarty w określonym repozytorium:</span><span class="sxs-lookup"><span data-stu-id="b46dc-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="b46dc-124">Aby upewnij się, że element WebHook w rzeczywistości od zamierzonego nadawcy, żądania POST jest zabezpieczony w jakiś sposób i następnie weryfikowany przez odbiorcę.</span><span class="sxs-lookup"><span data-stu-id="b46dc-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="b46dc-125">Na przykład [elementy Webhook GitHub](https://developer.github.com/webhooks/) obejmuje *X-Hub-podpisu* nagłówka HTTP z treści żądania, która jest sprawdzana przez odbiornika, więc nie trzeba już martwić się o jego skrót.</span><span class="sxs-lookup"><span data-stu-id="b46dc-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="b46dc-126">Przepływ elementu WebHook wykracza ogólnie podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="b46dc-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="b46dc-127">Nadawca elementu WebHook uwidacznia zdarzenia, które może być subskrybowana przez klienta.</span><span class="sxs-lookup"><span data-stu-id="b46dc-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="b46dc-128">Zdarzenia opisują zauważalne zmiany w systemie, na przykład które nowy element danych został wstawiony, że proces został zakończony lub coś innego.</span><span class="sxs-lookup"><span data-stu-id="b46dc-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="b46dc-129">Subskrybuje odbiorcy elementu WebHook, rejestrując element WebHook, składający się z czterech elementów:</span><span class="sxs-lookup"><span data-stu-id="b46dc-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="b46dc-130">Identyfikator URI, dla których zaksięgowania powiadomień o zdarzeniach w formularzu żądania HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="b46dc-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="b46dc-131">Zestaw filtrów opisujące określonych zdarzeń, dla których powinny być uruchamiane elementu WebHook;</span><span class="sxs-lookup"><span data-stu-id="b46dc-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="b46dc-132">Klucz tajny, który jest używany do podpisywania żądań POST protokołu HTTP;</span><span class="sxs-lookup"><span data-stu-id="b46dc-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="b46dc-133">Dodatkowe dane, które mają być zawarte w żądaniu POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="b46dc-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="b46dc-134">Może to być na przykład dodatkowe pola nagłówka HTTP lub właściwości zawarte w treści żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b46dc-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="b46dc-135">Po wystąpieniu zdarzenia zostaną znalezione dopasowania rejestracji elementu WebHook i przesłaniu żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b46dc-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="b46dc-136">Zazwyczaj generowanie żądań HTTP POST zostaną ponowione kilka razy, jeśli dla jakiegoś powodu, że odbiorca nie odpowiada lub wyniki żądania HTTP POST w odpowiedzi na błąd.</span><span class="sxs-lookup"><span data-stu-id="b46dc-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="b46dc-137">Potok przetwarzania elementów Webhook</span><span class="sxs-lookup"><span data-stu-id="b46dc-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="b46dc-138">Potok przetwarzania Microsoft ASP.NET WebHooks dla przychodzących elementów Webhook wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="b46dc-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Potok przetwarzania elementów Webhook programu ASP.NET](_static/WebHookReceivers.png)

<span data-ttu-id="b46dc-140">Są dwa kluczowe pojęcia tutaj *odbiorniki* i *obsługi*:</span><span class="sxs-lookup"><span data-stu-id="b46dc-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="b46dc-141">*Odbiorniki* jest odpowiedzialny za obsługę określoną wersję elementu WebHook z danym nadawcy i wymuszanie sprawdza zabezpieczeń, upewnij się, że żądanie elementu WebHook w rzeczywistości od zamierzonego nadawcy.</span><span class="sxs-lookup"><span data-stu-id="b46dc-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="b46dc-142">*Programy obsługi* są zazwyczaj, gdy kod użytkownika uruchamia przetwarzanie określonego elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="b46dc-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="b46dc-143">W następujących węzłach te pojęcia są opisane bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="b46dc-143">In the following nodes these concepts are described in more details.</span></span>
