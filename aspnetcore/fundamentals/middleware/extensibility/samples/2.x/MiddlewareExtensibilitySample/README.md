---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809410"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a><span data-ttu-id="1477f-101">Przykład rozszerzeń oprogramowania pośredniczącego platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1477f-101">ASP.NET Core Middleware Extensibility Sample</span></span>

<span data-ttu-id="1477f-102">W tym przykładzie przedstawiono scenariuszy opisanych w temacie [oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span><span class="sxs-lookup"><span data-stu-id="1477f-102">This sample demonstrates the scenarios described in [Factory-based middleware activation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span></span>

<span data-ttu-id="1477f-103">Przykładowa aplikacja pokazuje aktywowany przez oprogramowanie pośredniczące:</span><span class="sxs-lookup"><span data-stu-id="1477f-103">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="1477f-104">Konwencja.</span><span class="sxs-lookup"><span data-stu-id="1477f-104">Convention.</span></span> <span data-ttu-id="1477f-105">Aby uzyskać więcej informacji na temat aktywacji są konwencjonalne funkcje oprogramowania pośredniczącego, zobacz [oprogramowania pośredniczącego](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) tematu.</span><span class="sxs-lookup"><span data-stu-id="1477f-105">For more information on conventional middleware activation, see the [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) topic.</span></span>
* <span data-ttu-id="1477f-106">[IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementacji.</span><span class="sxs-lookup"><span data-stu-id="1477f-106">An [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="1477f-107">Wartość domyślna [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) klasy aktywuje oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="1477f-107">The default [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) class activates the middleware.</span></span>

<span data-ttu-id="1477f-108">Implementacje oprogramowania pośredniczącego działać identycznie i Zapisz wartość dostarczona przez parametr ciągu zapytania (`key`).</span><span class="sxs-lookup"><span data-stu-id="1477f-108">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="1477f-109">Middlewares użyć kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="1477f-109">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>
