---
uid: webhooks/source
title: Kod źródłowy elementów Webhook programu ASP.NET i pakiety NuGet | Dokumentacja firmy Microsoft
author: rick-anderson
description: Łącza do elementów Webhook programu ASP.NET w kodzie źródłowym oraz pakietów NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.openlocfilehash: 49a6d3e92e8d6365bea6594a616922aff9d0b4ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375852"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Kod źródłowy elementów Webhook programu ASP.NET i pakiety NuGet

Microsoft ASP.NET WebHooks jest częścią rodziny Microsoft ASP.NET, modułów i jest hostowany jako [Open Source Project w serwisie GitHub](https://github.com/aspnet/WebHooks). To oznacza, że firma Microsoft akceptuje wkładów, ale można znaleźć w [wytyczne dotyczące zamieszczania treści](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) przed przesłaniem żądania ściągnięcia.

Tej dokumentacji online, w którym czytasz teraz również znajduje się jako [typu Open Source w serwisie GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) oraz akceptuje wkładów.

## <a name="nuget-packages"></a>Pakiety NuGet

[Pakiety NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) są podzielone na trzy części:

* [Typowe](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): typowe pakiet, który jest udostępniany między nadawcami a odbiornikami.

* [Nadawca](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): zestaw pakietów obsługi wysyłania własne elementy Webhook dla innych użytkowników. Funkcja wysyłania elementów Webhook jest opisany bardziej szczegółowo w [wysyłania elementów Webhook](sending/index.md).

* [Odbiorniki](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): zestaw pakietów obsługi odbieranie elementów Webhook od innych. Funkcje do odbierania elementów Webhook jest opisany bardziej szczegółowo w [odbieranie elementów Webhook](receiving/index.md).
