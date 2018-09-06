---
title: Dla platformy ASP.NET Core, bezpiecznej liście adresów IP klienta
author: damienbod
description: Dowiedz się, jak pisać oprogramowanie pośredniczące lub akcji filtry, aby sprawdzić poprawność zdalnych adresów IP na liście zatwierdzonych adresów IP.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040124"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="b8dba-103">Dla platformy ASP.NET Core, bezpiecznej liście adresów IP klienta</span><span class="sxs-lookup"><span data-stu-id="b8dba-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="b8dba-104">Przez [linkach Damien](https://twitter.com/damien_bod) i [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b8dba-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="b8dba-105">W tym artykule przedstawiono dwa sposoby zaimplementowania bezpiecznej liście adresów IP (Lista dozwolonych):</span><span class="sxs-lookup"><span data-stu-id="b8dba-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="b8dba-106">Za pomocą platformy ASP.NET Core oprogramowania pośredniczącego do sprawdzenia zdalny adres IP każdego żądania.</span><span class="sxs-lookup"><span data-stu-id="b8dba-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="b8dba-107">Za pomocą platformy ASP.NET Core filtry akcji do sprawdzenia zdalny adres IP żądań dla danego działania metod.</span><span class="sxs-lookup"><span data-stu-id="b8dba-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="b8dba-108">Przykładowa aplikacja pokazano oba podejścia.</span><span class="sxs-lookup"><span data-stu-id="b8dba-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="b8dba-109">W każdym przypadku ciąg zawierający adresy IP zatwierdzone klienta są przechowywane w ustawieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b8dba-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="b8dba-110">Oprogramowanie pośredniczące lub filtr analizuje ciąg w postaci listy i sprawdza, czy zdalny adres IP na liście.</span><span class="sxs-lookup"><span data-stu-id="b8dba-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="b8dba-111">W przeciwnym razie zostanie zwrócony kod stanu HTTP 403 — Dostęp zabroniony.</span><span class="sxs-lookup"><span data-stu-id="b8dba-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="b8dba-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b8dba-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="b8dba-113">Bezpiecznej liście</span><span class="sxs-lookup"><span data-stu-id="b8dba-113">The safelist</span></span>

<span data-ttu-id="b8dba-114">Lista jest skonfigurowana w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="b8dba-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="b8dba-115">Jest rozdzielaną średnikami listę i może zawierać adresów IPv4 i IPv6.</span><span class="sxs-lookup"><span data-stu-id="b8dba-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="b8dba-116">Oprogramowanie pośredniczące</span><span class="sxs-lookup"><span data-stu-id="b8dba-116">Middleware</span></span>

<span data-ttu-id="b8dba-117">`Configure` Metoda dodaje oprogramowanie pośredniczące i przekazuje do niego ciąg bezpiecznej liście parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="b8dba-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="b8dba-118">Oprogramowanie pośredniczące analizuje ciąg do tablicy, a także szuka zdalny adres IP w tablicy.</span><span class="sxs-lookup"><span data-stu-id="b8dba-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="b8dba-119">Jeśli zdalny adres IP nie zostanie znaleziony, oprogramowanie pośredniczące zwraca HTTP 401 — Dostęp zabroniony.</span><span class="sxs-lookup"><span data-stu-id="b8dba-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="b8dba-120">Ten proces sprawdzania poprawności jest pomijana dla żądań HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="b8dba-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="b8dba-121">Filtr akcji</span><span class="sxs-lookup"><span data-stu-id="b8dba-121">Action filter</span></span>

<span data-ttu-id="b8dba-122">Chcąc bezpiecznej liście tylko dla określonych kontrolery lub metody akcji, użyj filtru akcji.</span><span class="sxs-lookup"><span data-stu-id="b8dba-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="b8dba-123">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="b8dba-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="b8dba-124">Filtr akcji zostanie dodany do kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="b8dba-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="b8dba-125">Następnie można filtr dla metody kontrolera lub akcji.</span><span class="sxs-lookup"><span data-stu-id="b8dba-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="b8dba-126">Przykładowa aplikacja, jest stosowany filtr do `Get` metody.</span><span class="sxs-lookup"><span data-stu-id="b8dba-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="b8dba-127">Tak, podczas testowania aplikacji, wysyłając `Get` żądania interfejsu API, ten atrybut jest sprawdzanie poprawności adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="b8dba-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="b8dba-128">Podczas testowania, wywołując interfejs API przy użyciu dowolnej metody HTTP, oprogramowanie pośredniczące jest sprawdzanie poprawności adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="b8dba-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="b8dba-129">Filtrowanie stron razor</span><span class="sxs-lookup"><span data-stu-id="b8dba-129">Razor Pages filter</span></span> 

<span data-ttu-id="b8dba-130">Chcąc bezpiecznej liście stron Razor aplikacji, użyj filtru stron Razor.</span><span class="sxs-lookup"><span data-stu-id="b8dba-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="b8dba-131">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="b8dba-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="b8dba-132">Ten filtr jest włączona, dodając ją do kolekcji filtrów platformy MVC.</span><span class="sxs-lookup"><span data-stu-id="b8dba-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="b8dba-133">Po uruchomieniu aplikacji i żądania strony Razor, filtru strony Razor jest sprawdzanie poprawności adresu IP klienta.</span><span class="sxs-lookup"><span data-stu-id="b8dba-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8dba-134">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b8dba-134">Next steps</span></span>

<span data-ttu-id="b8dba-135">[Dowiedz się więcej na temat platformy ASP.NET Core w oprogramowaniu pośredniczącym](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b8dba-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
