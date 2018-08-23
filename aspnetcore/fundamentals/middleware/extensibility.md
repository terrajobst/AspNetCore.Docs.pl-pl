---
title: Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowania pośredniczącego z implementacją oparte na fabryce aktywacji w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2018
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 346f5e7b28a9cd17a03a864772ed8b2e4be9455b
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2018
ms.locfileid: "41752664"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) jest punktem rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji.

`UseMiddleware` metody rozszerzenia, sprawdź, jeśli oprogramowanie pośredniczące, zarejestrowany typ implementuje `IMiddleware`. Jeśli tak jest, `IMiddlewareFactory` wystąpieniu zarejestrowana w kontenerze jest używany do rozpoznawania `IMiddleware` implementacji zamiast przy użyciu logiki aktywacji oprogramowanie pośredniczące oparte na Konwencji. Oprogramowanie pośredniczące jest zarejestrowany jako usługa o określonym zakresie lub przejściowy w kontenerze usługi aplikacji.

Korzyści:

* Aktywacja na żądanie (iniekcji usługi o określonym zakresie)
* Silne wpisywanie oprogramowania pośredniczącego

`IMiddleware` aktywowano na żądanie, dzięki czemu usługi o określonym zakresie można wstrzyknięte do konstruktora oprogramowania pośredniczącego.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowa aplikacja pokazuje aktywowany przez oprogramowanie pośredniczące:

* Konwencja. Aby uzyskać więcej informacji na temat aktywacji są konwencjonalne funkcje oprogramowania pośredniczącego, zobacz [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) tematu.
* [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementacji. Wartość domyślna [klasy MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktywuje oprogramowania pośredniczącego.

Implementacje oprogramowania pośredniczącego działać identycznie i Zapisz wartość dostarczona przez parametr ciągu zapytania (`key`). Middlewares użyć kontekstu wprowadzonego bazy danych (usługi o określonym zakresie) do rejestrowania wartości ciągu zapytania w bazie danych w pamięci.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji. [InvokeAsync (HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda obsługuje żądania i zwraca `Task` reprezentujący wykonywania oprogramowania pośredniczącego.

Oprogramowanie pośredniczące aktywowane zgodnie z Konwencją:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Oprogramowanie pośredniczące aktywowany przez `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Rozszerzenia są tworzone dla middlewares:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nie można przekazać obiekty do aktywacji fabryki oprogramowania pośredniczącego z `UseMiddleware`:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Oprogramowanie pośredniczące aktywacji fabryki jest dodawany do kontenerze wbudowane w *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=12)]

Zarówno middlewares są rejestrowane w potoku przetwarzania żądań w `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=14-15)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego. Wdrożenie fabryki oprogramowanie pośredniczące jest zarejestrowany w kontenerze w zakresie usługi.

Wartość domyślna `IMiddlewareFactory` implementacji [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), znajduje się w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
