---
uid: webhooks/receiving/receivers
title: "Odbiorniki elementów Webhook ASP.NET | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Odbiorniki elementów Webhook ASP.NET"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a>Odbiorniki elementów Webhook ASP.NET

Odbieranie elementów Webhook, zależy od tego, kto jest nadawcy. Czasami są dodatkowe kroki rejestrowania elementu WebHook weryfikowanie naprawdę nasłuchuje subskrybenta. Niektórych elementów Webhook Podaj modelu do wypychania, gdy żądanie HTTP POST tylko odwołuje się do informacji o zdarzeniu, który jest następnie zostać pobrane niezależnie. Często modelu zabezpieczeń różni się nieco dość.

Microsoft ASP.NET WebHooks ma na celu zarówno prostszy i bardziej spójny na połączenie się Twój interfejs API nie poświęcając czasu ustaleniem, jak obsługiwać żadnych określonego wariantu elementów Webhook.

Odbiornik elementu WebHook jest odpowiedzialny za akceptowanie i sprawdzania poprawności elementów Webhook od określonego nadawcy. Odbiornik WebHook może obsługiwać dowolną liczbę elementów Webhook, każdy z własnych konfiguracją. Na przykład odbiornika element GitHub WebHook może akceptować elementów Webhook z dowolnej liczby repozytoriów GitHub.

## <a name="webhook-receiver-uris"></a>Identyfikatory URI odbiornika elementu WebHook

Instalowanie programu Microsoft ASP.NET WebHooks otrzymasz ogólne kontrolera elementu WebHook, który akceptuje żądania elementu WebHook z nieograniczoną liczbę usług. Gdy żądanie dociera, wybiera odpowiednią odbiornika zainstalowanej obsługi określonego nadawcy elementu WebHook.

Identyfikator URI tego kontrolera Identyfikator URI elementu WebHook rejestrowanie za pomocą usługi i ma postać:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Ze względów bezpieczeństwa wymagają identyfikator URI jest wiele odbiorników elementu WebHook *https* identyfikatora URI i w niektórych przypadkach może również zawierać parametr zapytania dodatkowe, które służy do wymuszania, które tylko do zamierzonego strony może wysyłać elementów Webhook do identyfikatora URI powyżej .

 *<receiver>*  Składnik jest nazwa odbiornika, na przykład *github* lub *slack*.

*{Id}* jest opcjonalny identyfikator, który może służyć do identyfikowania określoną konfigurację odbiornika elementu WebHook. Może to służyć do rejestrowania N elementów Webhook z określonego odbiornika. Na przykład następujące trzy identyfikatory URI mogą służyć do zarejestrowania dla trzech niezależnych elementów Webhook:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Instalowanie odbiornika elementu WebHook

Aby otrzymywać przy użyciu programu Microsoft ASP.NET WebHooks elementów Webhook, należy najpierw zainstalować pakiet Nuget dla elementu WebHook lub dostawców ma być wyświetlany elementów Webhook z. Pakietów Nuget [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) gdzie ostatniej części wskazuje usługi obsługiwane. Na przykład

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) zapewnia obsługę odbieranie elementów Webhook z usługi GitHub i [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) zapewnia obsługę odbieranie elementów Webhook generowane przez program ASP. NET elementów Webhook.

Poza pole można znaleźć obsługę Dropbox, GitHub, MailChimp, PayPal, pchacza, Salesforce, zapas czasu, Stripe, Trello i WordPress, ale istnieje możliwość obsługi dowolnej liczby innych dostawców.

## <a name="configuring-a-webhook-receiver"></a>Konfigurowanie odbiornika elementu WebHook

Element WebHook odbiorcy są skonfigurowane za pomocą [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) interfejs i konkretnego implementacje tego interfejsu może być zarejestrowany przy użyciu dowolnego modelu iniekcji zależności. Domyślna implementacja używa ustawienia aplikacji, które można ustawić w pliku Web.config lub, jeśli za pomocą aplikacji sieci Web platformy Azure, można ustawić za pomocą [Azure Portal](https://portal.azure.com/).

![Ustawienia aplikacji Azure](_static/AzureAppSettings.png)

Format klucze ustawienia aplikacji jest następujący:

```
MS_WebHookReceiverSecret_<receiver>
```

Wartość jest rozdzielana przecinkami lista wartości zgodne *{id}* wartości, dla których elementów Webhook zostały zarejestrowane, na przykład:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Podczas inicjowania odbiornika elementu WebHook

Element WebHook odbiorcy są inicjowane przez zarejestrowanie ich, zwykle w *WebApiConfig* Klasa statyczna, na przykład:

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
