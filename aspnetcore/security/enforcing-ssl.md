---
title: "Wymuszanie protokołu HTTPS w aplikacji platformy ASP.NET Core"
author: rick-anderson
description: "Pokazuje, jak będą musieli HTTPS/TLS w ASP.NET Core aplikacji sieci web."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Wymuszanie protokołu HTTPS w aplikacji platformy ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten dokument zawiera jak:

- Wymagać protokołu HTTPS dla wszystkich żądań.
- Przekieruj żądania HTTP, HTTPS.

> [!WARNING]
> Czy **nie** użyj `RequireHttpsAttribute` na interfejsów API sieci Web, czy odbierać poufne informacje. `RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP, HTTPS. Klientów interfejsu API nie może zrozumieć lub przestrzegać przekierowania z protokołu HTTP, HTTPS. Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP. Interfejsy API sieci Web powinien:
>
>* Nie nasłuchiwania protokołu HTTP.
>* Zamknięcie połączenia z kodem stanu 400 (nieprawidłowe żądanie), a nie obsługiwać żądania.

## <a name="require-https"></a>Wymagać protokołu HTTPS

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) jest używana, aby wymagać protokołu HTTPS. `[RequireHttpsAttribute]` można dekoracji kontrolerów lub metody lub mogą być stosowane globalnie. Aby zastosować atrybut globalny, Dodaj następujący kod do `ConfigureServices` w `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Poprzedni wyróżniony kod wymaga wszystkie żądania przy użyciu `HTTPS`; w związku z tym żądania HTTP są ignorowane. Następujący wyróżniony kod przekierowuje żądania HTTP, https:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).

Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa. Stosowanie `[RequireHttps]` atrybut, aby wszystkie kontrolery/Razor strony nie jest uznawane za należycie zabezpieczone globalnie wymagających protokołu HTTPS. Nie można zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.