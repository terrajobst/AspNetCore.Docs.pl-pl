---
title: "Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core"
author: rick-anderson
description: "Pokazuje, jak wymaga protokołu SSL w ASP.NET Core aplikacji sieci web"
keywords: "Platformy ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, usługi IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="110d1-104">Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="110d1-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="110d1-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="110d1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="110d1-106">Ten dokument zawiera jak:</span><span class="sxs-lookup"><span data-stu-id="110d1-106">This document shows how to:</span></span>

- <span data-ttu-id="110d1-107">Wymagaj protokołu SSL dla wszystkich żądań (tylko żądania HTTPS).</span><span class="sxs-lookup"><span data-stu-id="110d1-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="110d1-108">Przekieruj żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="110d1-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="110d1-109">Wymagaj protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="110d1-109">Require SSL</span></span>

<span data-ttu-id="110d1-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="110d1-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="110d1-111">Można dekoracji kontrolerów lub metody z tym atrybutem lub można go zastosować globalnie, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="110d1-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="110d1-112">Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="110d1-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="110d1-113">Wyróżniony kod powyżej wymaga wszystkie żądania przy użyciu `HTTPS`, dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="110d1-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="110d1-114">Następujący wyróżniony kod przekierowuje żądania HTTP, https:</span><span class="sxs-lookup"><span data-stu-id="110d1-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="110d1-115">Zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="110d1-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="110d1-116">Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="110d1-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="110d1-117">Stosowanie `[RequireHttps]` atrybut do wszystkich kontrolera nie jest uważana za należycie zabezpieczone globalnie wymagających protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="110d1-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="110d1-118">Nie można zagwarantować nowych kontrolerów dodany do aplikacji będzie Pamiętaj, aby zastosować `[RequireHttps]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="110d1-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
