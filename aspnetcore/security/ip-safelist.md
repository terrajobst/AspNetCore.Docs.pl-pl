---
title: Safelist IP klienta dla ASP.NET Core
author: damienbod
description: Dowiedz się, jak napisać oprogramowanie pośredniczące lub filtry akcji, aby zweryfikować zdalne adresy IP w odniesieniu do listy zatwierdzonych adresów IP.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 02e44135ab1742d44691cfda8c4167f21d6efa4e
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975646"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="5f5ed-103">Safelist IP klienta dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f5ed-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="5f5ed-104">Autorzy [Damien Bowden](https://twitter.com/damien_bod) i [Tomasz Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5f5ed-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="5f5ed-105">W tym artykule przedstawiono trzy sposoby implementacji Safelist IP (znanego również jako dozwolonych) w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="5f5ed-106">Możesz użyć:</span><span class="sxs-lookup"><span data-stu-id="5f5ed-106">You can use:</span></span>

* <span data-ttu-id="5f5ed-107">Oprogramowanie pośredniczące do sprawdzenia zdalnego adresu IP każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="5f5ed-108">Filtry akcji do sprawdzania zdalnego adresu IP żądań dla określonych kontrolerów lub metod akcji.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="5f5ed-109">Razor Pages filtrów, aby sprawdzić zdalny adres IP żądań dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="5f5ed-110">W każdym przypadku ciąg zawierający zatwierdzone adresy IP klienta jest przechowywany w ustawieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="5f5ed-111">Oprogramowanie pośredniczące lub Filtr analizuje ciąg w postaci listy i sprawdza, czy zdalny adres IP znajduje się na liście.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="5f5ed-112">W przeciwnym razie zwracany jest kod stanu zabroniony protokołu HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="5f5ed-113">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f5ed-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="5f5ed-114">Safelist</span><span class="sxs-lookup"><span data-stu-id="5f5ed-114">The safelist</span></span>

<span data-ttu-id="5f5ed-115">Lista jest konfigurowana w pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="5f5ed-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="5f5ed-116">Jest to rozdzielana średnikami lista i może zawierać adresy IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="5f5ed-117">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="5f5ed-117">Middleware</span></span>

<span data-ttu-id="5f5ed-118">`Configure` Metoda dodaje oprogramowanie pośredniczące i przekazuje do niego ciąg Safelist w parametrze konstruktora.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="5f5ed-119">Oprogramowanie pośredniczące analizuje ciąg w tablicę i wyszukuje zdalny adres IP w tablicy.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="5f5ed-120">Jeśli zdalny adres IP nie zostanie znaleziony, oprogramowanie pośredniczące zwróci niedozwolony protokół HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="5f5ed-121">Ten proces sprawdzania poprawności jest pomijany dla żądań HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="5f5ed-122">Filtr akcji</span><span class="sxs-lookup"><span data-stu-id="5f5ed-122">Action filter</span></span>

<span data-ttu-id="5f5ed-123">Jeśli chcesz, aby Safelist tylko dla określonych kontrolerów lub metod akcji, Użyj filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="5f5ed-124">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="5f5ed-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="5f5ed-125">Filtr akcji zostanie dodany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="5f5ed-126">Filtr może być następnie używany na kontrolerze lub metodzie akcji.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="5f5ed-127">W przykładowej aplikacji filtr jest stosowany do `Get` metody.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="5f5ed-128">Dlatego podczas testowania aplikacji przez wysłanie `Get` żądania interfejsu API ten atrybut sprawdza poprawność adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="5f5ed-129">Podczas testowania przez wywołanie interfejsu API z dowolną inną metodą HTTP, oprogramowanie pośredniczące sprawdza poprawność adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="5f5ed-130">Filtr Razor Pages</span><span class="sxs-lookup"><span data-stu-id="5f5ed-130">Razor Pages filter</span></span> 

<span data-ttu-id="5f5ed-131">Jeśli chcesz Safelist dla aplikacji Razor Pages, Użyj filtru Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="5f5ed-132">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="5f5ed-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="5f5ed-133">Ten filtr jest włączony przez dodanie go do kolekcji filtrów MVC.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="5f5ed-134">Po uruchomieniu aplikacji i zażądaniu strony Razor filtr Razor Pages sprawdza poprawność adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="5f5ed-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f5ed-135">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="5f5ed-135">Next steps</span></span>

<span data-ttu-id="5f5ed-136">[Dowiedz się więcej na temat ASP.NET Core oprogramowania pośredniczącego](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5f5ed-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
