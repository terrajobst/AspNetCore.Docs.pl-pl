---
uid: webhooks/source
title: "Kod źródłowy elementów Webhook ASP.NET i pakiety NuGet | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Łącza do kodu źródłowego elementów Webhook ASP.NET i pakiety NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Kod źródłowy elementów Webhook ASP.NET i pakiety NuGet

Microsoft ASP.NET WebHooks jest częścią rodziny Microsoft ASP.NET modułów i jest obsługiwany jako [Otwórz projekt źródłowy w serwisie GitHub](https://github.com/aspnet/WebHooks). To oznacza, że możemy zaakceptować wkładów, ale można znaleźć w [wytyczne wkład](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.

Ten dokumentację w trybie online, które są odczytywanie teraz jest również obsługiwane jako [typu Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , a także akceptuje udziały.

## <a name="nuget-packages"></a>Pakiety NuGet

[Pakietów NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są podzielone na trzy części:

* [Typowe](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): typowe pakietu, który jest udostępniana między nadawcami a odbiornikami.

* [Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): zestaw pakietów obsługi wysyłania własnych elementów Webhook do innych użytkowników. Funkcja wysyłania elementów Webhook jest opisany bardziej szczegółowo w [wysyłania elementów Webhook](sending/index.md).

* [Odbiorniki](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): zestaw pakietów obsługujący odbieranie elementów Webhook od innych użytkowników. Funkcja odbierania elementów Webhook jest opisany bardziej szczegółowo w [odbieranie elementów Webhook](receiving/index.md).
