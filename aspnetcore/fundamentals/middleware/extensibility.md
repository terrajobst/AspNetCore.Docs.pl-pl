---
title: Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak używać silnie typizowane oprogramowania pośredniczącego z implementacją oparte na fabryce aktywacji w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: b4d71c2c7f09acb58b73e84080e8574d77f8b326
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086999"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Oprogramowanie pośredniczące oparte na fabryce aktywacji w programie ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> jest punktem rozszerzalności dla [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) aktywacji.

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> metody rozszerzenia, sprawdź, jeśli oprogramowanie pośredniczące, zarejestrowany typ implementuje <xref:Microsoft.AspNetCore.Http.IMiddleware>. Jeśli tak jest, <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> wystąpieniu zarejestrowana w kontenerze jest używany do rozpoznawania <xref:Microsoft.AspNetCore.Http.IMiddleware> implementacji zamiast przy użyciu logiki aktywacji oprogramowanie pośredniczące oparte na Konwencji. Oprogramowanie pośredniczące jest zarejestrowany jako [usługi o określonym zakresie lub przejściowy](xref:fundamentals/dependency-injection#service-lifetimes) w kontenerze usługi aplikacji.

Korzyści:

* Aktywacji dla każdego żądania klienta (iniekcji usługi o określonym zakresie)
* Silne wpisywanie oprogramowania pośredniczącego

<xref:Microsoft.AspNetCore.Http.IMiddleware> została aktywowana dla każdego żądania klienta (połączenie), usługi o określonym zakresie może wprowadzone do konstruktora oprogramowania pośredniczącego.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> Definiuje oprogramowanie pośredniczące do potoku żądania aplikacji. [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) metoda obsługuje żądania i zwraca <xref:System.Threading.Tasks.Task> reprezentujący wykonywania oprogramowania pośredniczącego.

Oprogramowanie pośredniczące aktywowane zgodnie z Konwencją:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Oprogramowanie pośredniczące aktywowany przez <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Rozszerzenia są tworzone dla middlewares:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nie można przekazać obiekty do aktywacji fabryki oprogramowania pośredniczącego z <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Oprogramowanie pośredniczące aktywacji fabryki jest dodawany do kontenerze wbudowane w `Startup.ConfigureServices`:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Zarówno middlewares są rejestrowane w potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> udostępnia metody do tworzenia oprogramowania pośredniczącego. Wdrożenie fabryki oprogramowanie pośredniczące jest zarejestrowany w kontenerze w zakresie usługi.

Wartość domyślna <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> implementacji <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, znajduje się w [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pakietu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
