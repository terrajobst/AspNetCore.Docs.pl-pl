---
title: Aktywacja oprogramowania pośredniczącego oparta na fabryce w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak używać silnego typu oprogramowania pośredniczącego z implementacją aktywacji opartej na fabryce w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: abc6268584d12fe43d972c79a99316b94e8bee4b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663094"
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Aktywacja oprogramowania pośredniczącego oparta na fabryce w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> jest punktem rozszerzalności dla aktywacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) .

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> metody rozszerzenia sprawdzają, czy typ zarejestrowanego oprogramowania pośredniczącego implementuje <xref:Microsoft.AspNetCore.Http.IMiddleware>. W takim przypadku wystąpienie <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> zarejestrowane w kontenerze służy do rozpoznawania <xref:Microsoft.AspNetCore.Http.IMiddleware> implementacji zamiast korzystania z logiki aktywacji oprogramowania pośredniczącego opartego na Konwencji. Oprogramowanie pośredniczące jest rejestrowane jako [Usługa w zakresie lub przejściowym](xref:fundamentals/dependency-injection#service-lifetimes) w kontenerze usługi aplikacji.

Korzysta

* Aktywacja na żądanie klienta (iniekcja usług objętych zakresem)
* Silne wpisywanie oprogramowania pośredniczącego

<xref:Microsoft.AspNetCore.Http.IMiddleware> jest uaktywniana dla żądania klienta (połączenie), więc usługi w zakresie mogą być wstrzykiwane do konstruktora oprogramowania pośredniczącego.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> definiuje oprogramowanie pośredniczące dla potoku żądania aplikacji. Metoda [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) obsługuje żądania i zwraca <xref:System.Threading.Tasks.Task>, które reprezentuje wykonanie oprogramowania pośredniczącego.

Oprogramowanie pośredniczące aktywowane zgodnie z konwencją:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Oprogramowanie pośredniczące aktywowane przez <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Dla middlewares są tworzone rozszerzenia:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nie jest możliwe przekazanie obiektów do oprogramowania pośredniczącego aktywowanego przez fabrykę przy użyciu <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Oprogramowanie pośredniczące aktywowane fabrycznie jest dodawane do wbudowanego kontenera w `Startup.ConfigureServices`:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Oba middlewares są rejestrowane w potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](extensibility/samples/3.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> udostępnia metody tworzenia oprogramowania pośredniczącego. Implementacja fabryki oprogramowania pośredniczącego jest zarejestrowana w kontenerze jako usługa objęta zakresem.

Domyślna implementacja <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, znajduje się w pakiecie [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>/<xref:Microsoft.AspNetCore.Http.IMiddleware> jest punktem rozszerzalności dla aktywacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) .

<xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*> metody rozszerzenia sprawdzają, czy typ zarejestrowanego oprogramowania pośredniczącego implementuje <xref:Microsoft.AspNetCore.Http.IMiddleware>. W takim przypadku wystąpienie <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> zarejestrowane w kontenerze służy do rozpoznawania <xref:Microsoft.AspNetCore.Http.IMiddleware> implementacji zamiast korzystania z logiki aktywacji oprogramowania pośredniczącego opartego na Konwencji. Oprogramowanie pośredniczące jest rejestrowane jako [Usługa w zakresie lub przejściowym](xref:fundamentals/dependency-injection#service-lifetimes) w kontenerze usługi aplikacji.

Korzysta

* Aktywacja na żądanie klienta (iniekcja usług objętych zakresem)
* Silne wpisywanie oprogramowania pośredniczącego

<xref:Microsoft.AspNetCore.Http.IMiddleware> jest uaktywniana dla żądania klienta (połączenie), więc usługi w zakresie mogą być wstrzykiwane do konstruktora oprogramowania pośredniczącego.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="imiddleware"></a>IMiddleware

<xref:Microsoft.AspNetCore.Http.IMiddleware> definiuje oprogramowanie pośredniczące dla potoku żądania aplikacji. Metoda [InvokeAsync (HttpContext, RequestDelegate)](xref:Microsoft.AspNetCore.Http.IMiddleware.InvokeAsync*) obsługuje żądania i zwraca <xref:System.Threading.Tasks.Task>, które reprezentuje wykonanie oprogramowania pośredniczącego.

Oprogramowanie pośredniczące aktywowane zgodnie z konwencją:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Oprogramowanie pośredniczące aktywowane przez <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/FactoryActivatedMiddleware.cs?name=snippet1)]

Dla middlewares są tworzone rozszerzenia:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Nie jest możliwe przekazanie obiektów do oprogramowania pośredniczącego aktywowanego przez fabrykę przy użyciu <xref:Microsoft.AspNetCore.Builder.UseMiddlewareExtensions.UseMiddleware*>:

```csharp
public static IApplicationBuilder UseFactoryActivatedMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<FactoryActivatedMiddleware>(option);
}
```

Oprogramowanie pośredniczące aktywowane fabrycznie jest dodawane do wbudowanego kontenera w `Startup.ConfigureServices`:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet1&highlight=6)]

Oba middlewares są rejestrowane w potoku przetwarzania żądań w `Startup.Configure`:

[!code-csharp[](extensibility/samples/2.x/MiddlewareExtensibilitySample/Startup.cs?name=snippet2&highlight=13-14)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

<xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> udostępnia metody tworzenia oprogramowania pośredniczącego. Implementacja fabryki oprogramowania pośredniczącego jest zarejestrowana w kontenerze jako usługa objęta zakresem.

Domyślna implementacja <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory>, <xref:Microsoft.AspNetCore.Http.MiddlewareFactory>, znajduje się w pakiecie [Microsoft. AspNetCore. http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) .

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/middleware/index>
* <xref:fundamentals/middleware/extensibility-third-party-container>
