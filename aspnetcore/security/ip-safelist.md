---
title: Dla platformy ASP.NET Core, bezpiecznej liście adresów IP klienta
author: damienbod
description: Dowiedz się, jak pisać oprogramowanie pośredniczące lub akcji filtry, aby sprawdzić poprawność zdalnych adresów IP na liście zatwierdzonych adresów IP.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: cfbb50ea33ae3af577f13b00bccc75fe0be57f79
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903373"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="3cc6b-103">Dla platformy ASP.NET Core, bezpiecznej liście adresów IP klienta</span><span class="sxs-lookup"><span data-stu-id="3cc6b-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="3cc6b-104">Przez [linkach Damien](https://twitter.com/damien_bod) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3cc6b-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="3cc6b-105">W tym artykule przedstawiono trzy sposoby, aby zaimplementować bezpiecznej liście adresów IP (Lista dozwolonych) w aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="3cc6b-106">Możesz użyć:</span><span class="sxs-lookup"><span data-stu-id="3cc6b-106">You can use:</span></span>

* <span data-ttu-id="3cc6b-107">Oprogramowanie pośredniczące do sprawdzenia zdalny adres IP każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="3cc6b-108">Filtry akcji, aby sprawdzić adres IP zdalnego żądań dotyczących określonego kontrolery lub metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="3cc6b-109">Filtry stron razor do sprawdzenia zdalny adres IP żądań dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="3cc6b-110">Przykładowa aplikacja pokazano oba podejścia.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-110">The sample app illustrates both approaches.</span></span> <span data-ttu-id="3cc6b-111">W każdym przypadku ciąg zawierający adresy IP zatwierdzone klienta są przechowywane w ustawieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="3cc6b-112">Oprogramowanie pośredniczące lub filtr analizuje ciąg w postaci listy i sprawdza, czy zdalny adres IP na liście.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-112">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="3cc6b-113">W przeciwnym razie zostanie zwrócony kod stanu HTTP 403 — Dostęp zabroniony.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-113">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="3cc6b-114">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3cc6b-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="3cc6b-115">Bezpiecznej liście</span><span class="sxs-lookup"><span data-stu-id="3cc6b-115">The safelist</span></span>

<span data-ttu-id="3cc6b-116">Lista jest skonfigurowana w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-116">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="3cc6b-117">Jest rozdzielaną średnikami listę i może zawierać adresów IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-117">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="3cc6b-118">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="3cc6b-118">Middleware</span></span>

<span data-ttu-id="3cc6b-119">`Configure` Metoda dodaje oprogramowanie pośredniczące i przekazuje do niego ciąg bezpiecznej liście parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-119">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="3cc6b-120">Oprogramowanie pośredniczące analizuje ciąg do tablicy, a także szuka zdalny adres IP w tablicy.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-120">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="3cc6b-121">Jeśli zdalny adres IP nie zostanie znaleziony, oprogramowanie pośredniczące zwraca HTTP 401 — Dostęp zabroniony.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-121">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="3cc6b-122">Ten proces sprawdzania poprawności jest pomijana dla żądań HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-122">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="3cc6b-123">Filtr akcji</span><span class="sxs-lookup"><span data-stu-id="3cc6b-123">Action filter</span></span>

<span data-ttu-id="3cc6b-124">Chcąc bezpiecznej liście tylko dla określonych kontrolery lub metody akcji, użyj filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-124">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="3cc6b-125">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="3cc6b-125">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="3cc6b-126">Filtr akcji zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-126">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="3cc6b-127">Następnie można filtr dla metody kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-127">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="3cc6b-128">Przykładowa aplikacja, jest stosowany filtr do `Get` metody.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-128">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="3cc6b-129">Tak, podczas testowania aplikacji, wysyłając `Get` żądania interfejsu API, ten atrybut jest sprawdzanie poprawności adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-129">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="3cc6b-130">Podczas testowania, wywołując interfejs API przy użyciu dowolnej metody HTTP, oprogramowanie pośredniczące jest sprawdzanie poprawności adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-130">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="3cc6b-131">Filtrowanie stron razor</span><span class="sxs-lookup"><span data-stu-id="3cc6b-131">Razor Pages filter</span></span> 

<span data-ttu-id="3cc6b-132">Chcąc bezpiecznej liście stron Razor aplikacji, użyj filtru stron Razor.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-132">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="3cc6b-133">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="3cc6b-133">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="3cc6b-134">Ten filtr jest włączona, dodając ją do kolekcji filtrów platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-134">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="3cc6b-135">Po uruchomieniu aplikacji i żądania strony Razor, filtru strony Razor jest sprawdzanie poprawności adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="3cc6b-135">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cc6b-136">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3cc6b-136">Next steps</span></span>

<span data-ttu-id="3cc6b-137">[Dowiedz się więcej na temat platformy ASP.NET Core w oprogramowaniu pośredniczącym](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="3cc6b-137">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
