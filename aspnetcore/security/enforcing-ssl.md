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
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="c365c-103">Wymuszanie protokołu HTTPS w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c365c-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="c365c-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c365c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c365c-105">Ten dokument zawiera jak:</span><span class="sxs-lookup"><span data-stu-id="c365c-105">This document shows how to:</span></span>

- <span data-ttu-id="c365c-106">Wymagać protokołu HTTPS dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="c365c-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="c365c-107">Przekieruj żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c365c-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="c365c-108">Czy **nie** użyj `RequireHttpsAttribute` na interfejsów API sieci Web, czy odbierać poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="c365c-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="c365c-109">`RequireHttpsAttribute` używa kodów stanu HTTP do przekierowania przeglądarki z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c365c-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="c365c-110">Klientów interfejsu API nie może zrozumieć lub przestrzegać przekierowania z protokołu HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c365c-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="c365c-111">Tacy klienci mogą wysłać informacje za pośrednictwem protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c365c-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="c365c-112">Interfejsy API sieci Web powinien:</span><span class="sxs-lookup"><span data-stu-id="c365c-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="c365c-113">Nie nasłuchiwania protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="c365c-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="c365c-114">Zamknięcie połączenia z kodem stanu 400 (nieprawidłowe żądanie), a nie obsługiwać żądania.</span><span class="sxs-lookup"><span data-stu-id="c365c-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="c365c-115">Wymagać protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="c365c-115">Require HTTPS</span></span>

<span data-ttu-id="c365c-116">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) jest używana, aby wymagać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c365c-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="c365c-117">`[RequireHttpsAttribute]` można dekoracji kontrolerów lub metody lub mogą być stosowane globalnie.</span><span class="sxs-lookup"><span data-stu-id="c365c-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="c365c-118">Aby zastosować atrybut globalny, Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c365c-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="c365c-119">Poprzedni wyróżniony kod wymaga wszystkie żądania przy użyciu `HTTPS`; w związku z tym żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="c365c-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="c365c-120">Następujący wyróżniony kod przekierowuje żądania HTTP, https:</span><span class="sxs-lookup"><span data-stu-id="c365c-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="c365c-121">Aby uzyskać więcej informacji, zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="c365c-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="c365c-122">Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="c365c-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="c365c-123">Stosowanie `[RequireHttps]` atrybut, aby wszystkie kontrolery/Razor strony nie jest uznawane za należycie zabezpieczone globalnie wymagających protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c365c-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="c365c-124">Nie można zagwarantować `[RequireHttps]` atrybut jest stosowany podczas dodawania nowych kontrolerów i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c365c-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>