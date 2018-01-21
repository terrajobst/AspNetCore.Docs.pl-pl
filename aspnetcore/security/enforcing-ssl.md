---
title: "Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core"
author: rick-anderson
description: "Pokazuje, jak wymaga protokołu SSL w ASP.NET Core aplikacji sieci web"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten dokument zawiera jak:

- Wymagaj protokołu SSL dla wszystkich żądań (tylko żądania HTTPS).
- Przekieruj żądania HTTP, HTTPS.

## <a name="require-ssl"></a>Wymagaj protokołu SSL

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu SSL. Można dekoracji kontrolerów lub metody z tym atrybutem lub można go zastosować globalnie, jak pokazano poniżej:

Dodaj następujący kod do `ConfigureServices` w `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

Wyróżniony kod powyżej wymaga wszystkie żądania przy użyciu `HTTPS`, dlatego żądania HTTP są ignorowane. Następujący wyróżniony kod przekierowuje żądania HTTP, https:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting) Aby uzyskać więcej informacji.

Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa. Stosowanie `[RequireHttps]` atrybut do wszystkich kontrolera nie jest uważana za należycie zabezpieczone globalnie wymagających protokołu HTTPS. Nie można zagwarantować nowych kontrolerów dodany do aplikacji będzie Pamiętaj, aby zastosować `[RequireHttps]` atrybutu.
