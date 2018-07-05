---
uid: webhooks/receiving/receivers
title: Odbiorniki elementów Webhook programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: Odbiorniki elementów Webhook programu ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.openlocfilehash: be1f7dcef60642231af9c593eb7329ede7c2094f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374746"
---
# <a name="aspnet-webhooks-receivers"></a>Odbiorniki elementów Webhook programu ASP.NET

Odbieranie elementów Webhook, zależy od tego, kto jest nadawcy. Czasami są dodatkowe kroki rejestracji elementu WebHook, weryfikowanie, czy naprawdę nasłuchuje subskrybenta. Niektóre elementy Webhook zapewnić model do wypychania, gdzie żądania HTTP POST zawiera tylko odwołanie do informacji o zdarzeniu, który jest następnie zostać pobrane niezależnie. Często model zabezpieczeń różni się jeszcze chwilę.

Microsoft ASP.NET WebHooks ma na celu stał się prostszy i bardziej spójną w celu podłączenia do interfejsu API, nie tracąc dużo czasu na myśleniu o tym jak obsługiwać dowolnego określonego wariantu elementów Webhook.

Odbiornik elementu WebHook jest odpowiedzialny za akceptowanie i weryfikowanie elementów Webhook od określonego nadawcy. Odbiornik elementu WebHook może obsługiwać dowolną liczbę elementów Webhook, każda z ich konfiguracją. Na przykład odbiorcy element WebHook usługi GitHub może akceptować elementów Webhook z dowolnej liczby repozytoriów GitHub.

## <a name="webhook-receiver-uris"></a>Identyfikatory URI odbiorcy elementu WebHook

Po zainstalowaniu programu Microsoft ASP.NET WebHooks uzyskasz ogólne kontrolera elementu WebHook, który akceptuje żądania elementu WebHook z nieograniczoną liczbę usług. Po odebraniu żądania wybiera odpowiednie odbiorcy, zainstalowanym do obsługi określonego nadawcy elementu WebHook.

Identyfikator URI tego kontrolera jest identyfikator URI elementu WebHook, który należy zarejestrować w usłudze i ma postać:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Ze względów bezpieczeństwa wiele odbiorników elementu WebHook wymagają, że identyfikator URI jest *https* identyfikatora URI i w niektórych przypadkach musi zawierać parametr zapytania dodatkowych, która służy do wymuszania, które tylko zamierzony ze stron może wysyłać elementów Webhook do powyższego identyfikatora URI .

<em> <receiver> </em> Składnik to nazwa odbiorcy, na przykład <em>github</em> lub <em>slack</em>.

*{Id}* jest opcjonalny identyfikator, który może służyć do identyfikowania konkretnej konfiguracji odbiornika elementu WebHook. To może służyć do rejestrowania N elementów Webhook przy użyciu określonego odbiorcy. Na przykład następujące trzy identyfikatory URI mogą służyć do zarejestrowania dla trzech niezależnych elementy Webhook:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalowanie odbiorcy elementu WebHook

Aby odbierać elementy Webhook przy użyciu programu Microsoft ASP.NET WebHooks, należy najpierw zainstalować pakiet Nuget dla dostawcy elementu WebHook lub dostawców, które chcesz otrzymywać elementów Webhook z. Pakiety Nuget są nazywane [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) gdzie ostatnia część wskazuje usługi obsługiwane. Na przykład

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) zapewnia obsługę do odbierania elementów Webhook z usługi GitHub i [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) zapewnia obsługę do odbierania elementów Webhook, generowane przez ASP. NET elementów Webhook.

Gotowych można znaleźć obsługę usługi Dropbox, GitHub, MailChimp, PayPal, usługi Pusher, Salesforce, Slack, usługi Stripe, Trello i WordPress, ale istnieje możliwość obsługi dowolnej liczby innych dostawców.

## <a name="configuring-a-webhook-receiver"></a>Konfigurowanie odbiornika elementu WebHook

Odbiorniki element WebHook można skonfigurować za pomocą [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfejsem konkretnej implementacji interfejsu można rejestrować i przy użyciu dowolnego modelu iniekcji zależności. Domyślna implementacja używa ustawienia aplikacji, które można ustawić w pliku Web.config lub, jeśli za pomocą usługi Azure Web Apps, można ustawić za pomocą [witryny Azure Portal](https://portal.azure.com/).

![Ustawienia aplikacji platformy Azure](_static/AzureAppSettings.png)

Format klucze ustawienia aplikacji jest w następujący sposób:

```
MS_WebHookReceiverSecret_<receiver>
```

Wartość rozdzielaną przecinkami listę wartości odpowiadających *{id}* wartości, dla których elementy Webhook zostały zarejestrowane, na przykład:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inicjowanie odbiornika elementu WebHook

Odbiorniki elementu WebHook są inicjowane przez zarejestrowanie ich, zazwyczaj w *WebApiConfig* statyczne klasy, na przykład:

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
