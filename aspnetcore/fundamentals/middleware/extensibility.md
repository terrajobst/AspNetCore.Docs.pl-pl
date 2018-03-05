---
title: "Aktywacji opartej na fabryki oprogramowanie pośredniczące w ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak użyć jednoznacznie oprogramowania pośredniczącego z aktywacji opartej na fabryki implementacja platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 2d5706f4a2b22980618f0c5c546e16774e0e16b0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Aktywacji opartej na fabryki oprogramowanie pośredniczące w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) punkt rozszerzalności dla [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) aktywacji.

`UseMiddleware` metody rozszerzenia, sprawdź, czy oprogramowanie pośredniczące w zarejestrowany typ implementuje `IMiddleware`. Jeśli tak, `IMiddlewareFactory` wystąpienia zarejestrowany w kontenerze jest używany do rozpoznawania `IMiddleware` implementacji zamiast logiki aktywacji opartych na konwencjach oprogramowania pośredniczącego. Oprogramowanie pośredniczące jest zarejestrowany jako usługa zakresie lub przejściowej w kontenerze usługi aplikacji.

Korzyści:

* Aktywacja na żądanie (iniekcji usługi w zakresie)
* Silne wpisywanie oprogramowania pośredniczącego

`IMiddleware` została aktywowana na żądanie, i usługi w zakresie mogą zostać dodane do konstruktora oprogramowania pośredniczącego.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

Przykładowa aplikacja ilustruje aktywowany przez oprogramowanie pośredniczące:

* Konwencja (`ConventionalMiddleware`). Aby uzyskać więcej informacji o aktywacji z konwencjonalnej oprogramowanie pośredniczące, zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) tematu.
* [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementacji (`IMiddlewareMiddleware`). Wartość domyślna [klasy MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) aktywuje oprogramowania pośredniczącego.

Implementacje oprogramowania pośredniczącego działają tak samo i rejestrowanie podana wartość parametru ciągu zapytania (`key`). Middlewares Użyj kontekstu wprowadzony bazy danych (zakresie usługa), aby zapisać wartość ciągu kwerendy w bazie danych w pamięci.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definiuje oprogramowanie pośredniczące do potoku żądania aplikacji. [InvokeAsync (Element HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) metoda obsługuje żądania i zwraca `Task` reprezentujący wykonywanie oprogramowania pośredniczącego.

Oprogramowanie pośredniczące aktywowany przez Konwencję:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Oprogramowanie pośredniczące aktywowany przez `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Rozszerzenia są tworzone dla middlewares:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nie można przekazać obiektów do oprogramowania pośredniczącego aktywowany fabryki z `UseMiddleware`:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

Oprogramowanie pośredniczące aktywowany fabryki jest dodawany do kontenera wbudowanych w *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

Zarówno middlewares są rejestrowane w potoku przetwarzania żądań w `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) udostępnia metody do tworzenia oprogramowania pośredniczącego. Wdrożenie fabryki oprogramowanie pośredniczące jest zarejestrowany w kontenerze w zakresie usługi.

Wartość domyślna `IMiddlewareFactory` implementacji [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), znajduje się w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu ([źródło odwołania](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Oprogramowanie pośredniczące aktywacji z kontenerem innych firm](xref:fundamentals/middleware/extensibility-third-party-container)
