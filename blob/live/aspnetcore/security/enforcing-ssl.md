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
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="3072a-103">Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3072a-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="3072a-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3072a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3072a-105">Ten dokument zawiera jak:</span><span class="sxs-lookup"><span data-stu-id="3072a-105">This document shows how to:</span></span>

- <span data-ttu-id="3072a-106">Wymagaj protokołu SSL dla wszystkich żądań (tylko żądania HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3072a-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="3072a-107">Przekieruj żądania HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3072a-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="3072a-108">Wymagaj protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="3072a-108">Require SSL</span></span>

<span data-ttu-id="3072a-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) służy do wymagania protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="3072a-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="3072a-110">Można dekoracji kontrolerów lub metody z tym atrybutem lub można go zastosować globalnie, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3072a-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="3072a-111">Dodaj następujący kod do `ConfigureServices` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3072a-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="3072a-112">Wyróżniony kod powyżej wymaga wszystkie żądania przy użyciu `HTTPS`, dlatego żądania HTTP są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="3072a-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="3072a-113">Następujący wyróżniony kod przekierowuje żądania HTTP, https:</span><span class="sxs-lookup"><span data-stu-id="3072a-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="3072a-114">Zobacz [ponowne zapisywanie adresów URL w oprogramowaniu pośredniczącym](xref:fundamentals/url-rewriting) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="3072a-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="3072a-115">Globalny wymagających protokołu HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) jest ze względów bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="3072a-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="3072a-116">Stosowanie `[RequireHttps]` atrybut do wszystkich kontrolera nie jest uważana za należycie zabezpieczone globalnie wymagających protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3072a-116">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="3072a-117">Nie można zagwarantować nowych kontrolerów dodany do aplikacji będzie Pamiętaj, aby zastosować `[RequireHttps]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="3072a-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
