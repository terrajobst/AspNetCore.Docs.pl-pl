---
uid: webhooks/receiving/receivers
title: Odbiorniki elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Odbiorniki elementów Webhook programu ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: 376cb3e3fdc0bc7bd248da1f57e1064fb27b3cef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752461"
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="7bf88-103">Odbiorniki elementów Webhook programu ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7bf88-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="7bf88-104">Odbieranie elementów Webhook, zależy od tego, kto jest nadawcy.</span><span class="sxs-lookup"><span data-stu-id="7bf88-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="7bf88-105">Czasami są dodatkowe kroki rejestracji elementu WebHook, weryfikowanie, czy naprawdę nasłuchuje subskrybenta.</span><span class="sxs-lookup"><span data-stu-id="7bf88-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="7bf88-106">Niektóre elementy Webhook zapewnić model do wypychania, gdzie żądania HTTP POST zawiera tylko odwołanie do informacji o zdarzeniu, który jest następnie zostać pobrane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="7bf88-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="7bf88-107">Często model zabezpieczeń różni się jeszcze chwilę.</span><span class="sxs-lookup"><span data-stu-id="7bf88-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="7bf88-108">Microsoft ASP.NET WebHooks ma na celu stał się prostszy i bardziej spójną w celu podłączenia do interfejsu API, nie tracąc dużo czasu na myśleniu o tym jak obsługiwać dowolnego określonego wariantu elementów Webhook.</span><span class="sxs-lookup"><span data-stu-id="7bf88-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="7bf88-109">Odbiornik elementu WebHook jest odpowiedzialny za akceptowanie i weryfikowanie elementów Webhook od określonego nadawcy.</span><span class="sxs-lookup"><span data-stu-id="7bf88-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="7bf88-110">Odbiornik elementu WebHook może obsługiwać dowolną liczbę elementów Webhook, każda z ich konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="7bf88-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="7bf88-111">Na przykład odbiorcy element WebHook usługi GitHub może akceptować elementów Webhook z dowolnej liczby repozytoriów GitHub.</span><span class="sxs-lookup"><span data-stu-id="7bf88-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="7bf88-112">Identyfikatory URI odbiorcy elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="7bf88-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="7bf88-113">Po zainstalowaniu programu Microsoft ASP.NET WebHooks uzyskasz ogólne kontrolera elementu WebHook, który akceptuje żądania elementu WebHook z nieograniczoną liczbę usług.</span><span class="sxs-lookup"><span data-stu-id="7bf88-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="7bf88-114">Po odebraniu żądania wybiera odpowiednie odbiorcy, zainstalowanym do obsługi określonego nadawcy elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="7bf88-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="7bf88-115">Identyfikator URI tego kontrolera jest identyfikator URI elementu WebHook, który należy zarejestrować w usłudze i ma postać:</span><span class="sxs-lookup"><span data-stu-id="7bf88-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="7bf88-116">Ze względów bezpieczeństwa wiele odbiorników elementu WebHook wymagają, że identyfikator URI jest *https* identyfikatora URI i w niektórych przypadkach musi zawierać parametr zapytania dodatkowych, która służy do wymuszania, które tylko zamierzony ze stron może wysyłać elementów Webhook do powyższego identyfikatora URI .</span><span class="sxs-lookup"><span data-stu-id="7bf88-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="7bf88-117"><em> <receiver> </em> Składnik to nazwa odbiorcy, na przykład <em>github</em> lub <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="7bf88-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="7bf88-118">*{Id}* jest opcjonalny identyfikator, który może służyć do identyfikowania konkretnej konfiguracji odbiornika elementu WebHook.</span><span class="sxs-lookup"><span data-stu-id="7bf88-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="7bf88-119">To może służyć do rejestrowania N elementów Webhook przy użyciu określonego odbiorcy.</span><span class="sxs-lookup"><span data-stu-id="7bf88-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="7bf88-120">Na przykład następujące trzy identyfikatory URI mogą służyć do zarejestrowania dla trzech niezależnych elementy Webhook:</span><span class="sxs-lookup"><span data-stu-id="7bf88-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="7bf88-121">Instalowanie odbiorcy elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="7bf88-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="7bf88-122">Aby odbierać elementy Webhook przy użyciu programu Microsoft ASP.NET WebHooks, należy najpierw zainstalować pakiet Nuget dla dostawcy elementu WebHook lub dostawców, które chcesz otrzymywać elementów Webhook z.</span><span class="sxs-lookup"><span data-stu-id="7bf88-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="7bf88-123">Pakiety Nuget są nazywane [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) gdzie ostatnia część wskazuje usługi obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="7bf88-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="7bf88-124">Na przykład</span><span class="sxs-lookup"><span data-stu-id="7bf88-124">For example</span></span>

<span data-ttu-id="7bf88-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) zapewnia obsługę do odbierania elementów Webhook z usługi GitHub i [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) zapewnia obsługę do odbierania elementów Webhook, generowane przez ASP. NET elementów Webhook.</span><span class="sxs-lookup"><span data-stu-id="7bf88-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="7bf88-126">Gotowych można znaleźć obsługę usługi Dropbox, GitHub, MailChimp, PayPal, usługi Pusher, Salesforce, Slack, usługi Stripe, Trello i WordPress, ale istnieje możliwość obsługi dowolnej liczby innych dostawców.</span><span class="sxs-lookup"><span data-stu-id="7bf88-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="7bf88-127">Konfigurowanie odbiornika elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="7bf88-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="7bf88-128">Odbiorniki element WebHook można skonfigurować za pomocą [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfejsem konkretnej implementacji interfejsu można rejestrować i przy użyciu dowolnego modelu iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7bf88-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="7bf88-129">Domyślna implementacja używa ustawienia aplikacji, które można ustawić w pliku Web.config lub, jeśli za pomocą usługi Azure Web Apps, można ustawić za pomocą [witryny Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7bf88-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Ustawienia aplikacji platformy Azure](_static/AzureAppSettings.png)

<span data-ttu-id="7bf88-131">Format klucze ustawienia aplikacji jest w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7bf88-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="7bf88-132">Wartość rozdzielaną przecinkami listę wartości odpowiadających *{id}* wartości, dla których elementy Webhook zostały zarejestrowane, na przykład:</span><span class="sxs-lookup"><span data-stu-id="7bf88-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="7bf88-133">Inicjowanie odbiornika elementu WebHook</span><span class="sxs-lookup"><span data-stu-id="7bf88-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="7bf88-134">Odbiorniki elementu WebHook są inicjowane przez zarejestrowanie ich, zazwyczaj w *WebApiConfig* statyczne klasy, na przykład:</span><span class="sxs-lookup"><span data-stu-id="7bf88-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
