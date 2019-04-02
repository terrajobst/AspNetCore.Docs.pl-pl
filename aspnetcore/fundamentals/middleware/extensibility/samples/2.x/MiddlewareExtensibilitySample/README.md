---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809410"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a>Przykład rozszerzeń oprogramowania pośredniczącego platformy ASP.NET Core

W tym przykładzie przedstawiono scenariuszy opisanych w temacie [oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).

Przykładowa aplikacja pokazuje aktywowany przez oprogramowanie pośredniczące:

* Konwencja. Aby uzyskać więcej informacji na temat aktywacji są konwencjonalne funkcje oprogramowania pośredniczącego, zobacz [oprogramowania pośredniczącego](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) tematu.
* [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementacji. Wartość domyślna [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) klasy aktywuje oprogramowania pośredniczącego.

Implementacje oprogramowania pośredniczącego działać identycznie i Zapisz wartość dostarczona przez parametr ciągu zapytania (`key`). Middlewares użyć kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.
