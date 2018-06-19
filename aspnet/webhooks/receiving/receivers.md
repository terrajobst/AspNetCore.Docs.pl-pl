---
uid: webhooks/receiving/receivers
title: Odbiorniki elementów Webhook ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Odbiorniki elementów Webhook ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896299"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="fd2ac-103">Odbiorniki elementów Webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fd2ac-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="fd2ac-104">Odbieranie elementów Webhook, zależy od tego, kto jest nadawcy.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="fd2ac-105">Czasami są dodatkowe kroki rejestrowania elementu WebHook weryfikowanie naprawdę nasłuchuje subskrybenta.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="fd2ac-106">Niektórych elementów Webhook Podaj modelu do wypychania, gdy żądanie HTTP POST tylko odwołuje się do informacji o zdarzeniu, który jest następnie zostać pobrane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="fd2ac-107">Często modelu zabezpieczeń różni się nieco dość.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="fd2ac-108">Microsoft ASP.NET WebHooks ma na celu zarówno prostszy i bardziej spójny na połączenie się Twój interfejs API nie poświęcając czasu ustaleniem, jak obsługiwać żadnych określonego wariantu elementów Webhook.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="fd2ac-109">Odbiornik elementu WebHook jest odpowiedzialny za akceptowanie i sprawdzania poprawności elementów Webhook od określonego nadawcy.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="fd2ac-110">Odbiornik WebHook może obsługiwać dowolną liczbę elementów Webhook, każdy z własnych konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="fd2ac-111">Na przykład odbiornika element GitHub WebHook może akceptować elementów Webhook z dowolnej liczby repozytoriów GitHub.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="fd2ac-112">Identyfikatory URI odbiornika elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="fd2ac-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="fd2ac-113">Instalowanie programu Microsoft ASP.NET WebHooks otrzymasz ogólne kontrolera elementu WebHook, który akceptuje żądania elementu WebHook z nieograniczoną liczbę usług.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="fd2ac-114">Gdy żądanie dociera, wybiera odpowiednią odbiornika zainstalowanej obsługi określonego nadawcy elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="fd2ac-115">Identyfikator URI tego kontrolera Identyfikator URI elementu WebHook rejestrowanie za pomocą usługi i ma postać:</span><span class="sxs-lookup"><span data-stu-id="fd2ac-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="fd2ac-116">Ze względów bezpieczeństwa wymagają identyfikator URI jest wiele odbiorników elementu WebHook *https* identyfikatora URI i w niektórych przypadkach może również zawierać parametr zapytania dodatkowe, które służy do wymuszania, które tylko do zamierzonego strony może wysyłać elementów Webhook do identyfikatora URI powyżej .</span><span class="sxs-lookup"><span data-stu-id="fd2ac-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="fd2ac-117"><em> <receiver> </em> Składnik jest nazwa odbiornika, na przykład <em>github</em> lub <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="fd2ac-118">*{Id}* jest opcjonalny identyfikator, który może służyć do identyfikowania określoną konfigurację odbiornika elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="fd2ac-119">Może to służyć do rejestrowania N elementów Webhook z określonego odbiornika.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="fd2ac-120">Na przykład następujące trzy identyfikatory URI mogą służyć do zarejestrowania dla trzech niezależnych elementów Webhook:</span><span class="sxs-lookup"><span data-stu-id="fd2ac-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="fd2ac-121">Instalowanie odbiornika elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="fd2ac-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="fd2ac-122">Aby otrzymywać przy użyciu programu Microsoft ASP.NET WebHooks elementów Webhook, należy najpierw zainstalować pakiet Nuget dla elementu WebHook lub dostawców ma być wyświetlany elementów Webhook z.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="fd2ac-123">Pakietów Nuget [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) gdzie ostatniej części wskazuje usługi obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="fd2ac-124">Na przykład</span><span class="sxs-lookup"><span data-stu-id="fd2ac-124">For example</span></span>

<span data-ttu-id="fd2ac-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) zapewnia obsługę odbieranie elementów Webhook z usługi GitHub i [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) zapewnia obsługę odbieranie elementów Webhook generowane przez program ASP. NET elementów Webhook.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="fd2ac-126">Poza pole można znaleźć obsługę Dropbox, GitHub, MailChimp, PayPal, pchacza, Salesforce, zapas czasu, Stripe, Trello i WordPress, ale istnieje możliwość obsługi dowolnej liczby innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="fd2ac-127">Konfigurowanie odbiornika elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="fd2ac-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="fd2ac-128">Element WebHook odbiorcy są skonfigurowane za pomocą [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfejs i konkretnego implementacje tego interfejsu może być zarejestrowany przy użyciu dowolnego modelu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="fd2ac-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="fd2ac-129">Domyślna implementacja używa ustawienia aplikacji, które można ustawić w pliku Web.config lub, jeśli za pomocą aplikacji sieci Web platformy Azure, można ustawić za pomocą [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fd2ac-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Ustawienia aplikacji Azure](_static/AzureAppSettings.png)

<span data-ttu-id="fd2ac-131">Format klucze ustawienia aplikacji jest następujący:</span><span class="sxs-lookup"><span data-stu-id="fd2ac-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="fd2ac-132">Wartość jest rozdzielana przecinkami lista wartości zgodne *{id}* wartości, dla których elementów Webhook zostały zarejestrowane, na przykład:</span><span class="sxs-lookup"><span data-stu-id="fd2ac-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="fd2ac-133">Podczas inicjowania odbiornika elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="fd2ac-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="fd2ac-134">Element WebHook odbiorcy są inicjowane przez zarejestrowanie ich, zwykle w *WebApiConfig* Klasa statyczna, na przykład:</span><span class="sxs-lookup"><span data-stu-id="fd2ac-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
