---
title: Pisanie niestandardowych platformy ASP.NET Core oprogramowania pośredniczącego.
author: rick-anderson
description: Dowiedz się, jak napisać niestandardowy platformy ASP.NET Core oprogramowania pośredniczącego.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 352db93dd7061070c76e34f6c03883f68e2041ee
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67167106"
---
# <a name="write-custom-aspnet-core-middleware"></a>Pisanie niestandardowych platformy ASP.NET Core oprogramowania pośredniczącego.

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Steve Smith](https://ardalis.com/)

Oprogramowanie pośredniczące to oprogramowanie, które jest umieszczone w potoku aplikacji do obsługi żądań i odpowiedzi. Platforma ASP.NET Core oferuje rozbudowanego zestawu składników oprogramowania pośredniczącego wbudowanych, ale w niektórych scenariuszach możesz chcieć napisać niestandardowego oprogramowania pośredniczącego.

## <a name="middleware-class"></a>Klasa oprogramowania pośredniczącego

Oprogramowanie pośredniczące jest zazwyczaj hermetyzowane w klasie i widoczne z metodą rozszerzenia. Należy wziąć pod uwagę następujące oprogramowanie pośredniczące, które ustawia kulturę bieżącego żądania z ciągu zapytania:

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

Poprzedni przykładowy kod jest używany do zademonstrowania tworzenia składników oprogramowania pośredniczącego. Obsługę wbudowanych lokalizacja platformy ASP.NET Core, zobacz <xref:fundamentals/localization>.

Testowanie oprogramowania pośredniczącego, przekazując w kulturze. Na przykład żądania `https://localhost:5001/?culture=no`.

Poniższy kod powoduje delegata oprogramowania pośredniczącego do klasy:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

Klasa oprogramowania pośredniczącego musi zawierać:

* Konstruktor publiczny z parametrem typu <xref:Microsoft.AspNetCore.Http.RequestDelegate>.
* Publiczną metodę o nazwie `Invoke` lub `InvokeAsync`. Ta metoda musi:
  * Zwróć `Task`.
  * Zaakceptuj pierwszy parametr typu <xref:Microsoft.AspNetCore.Http.HttpContext>.
  
Dodatkowe parametry dla konstruktora i `Invoke` / `InvokeAsync` są wypełniane przez [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).

## <a name="middleware-dependencies"></a>Zależności oprogramowania pośredniczącego

Oprogramowanie pośredniczące powinno wykonać [jawne zależności zasady](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) uwidaczniając jego zależności w jego konstruktorze. Oprogramowanie pośredniczące jest tworzona raz na *okres istnienia aplikacji*. Zobacz [zależności oprogramowania pośredniczącego żądania](#per-request-middleware-dependencies) sekcji, jeśli zachodzi potrzeba udostępniania usług za pomocą oprogramowania pośredniczącego w żądaniu.

Składniki oprogramowania pośredniczącego może rozpoznać ich zależności z [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) za pośrednictwem parametry konstruktora. [UseMiddleware&lt;T&gt; ](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) mogą również akceptować dodatkowe parametry bezpośrednio.

## <a name="per-request-middleware-dependencies"></a>Zależności oprogramowania pośredniczącego żądania

Ponieważ oprogramowanie pośredniczące jest konstruowany przy uruchamianiu aplikacji, nie dla poszczególnych żądań, *zakresie* okres istnienia, obejmujący usługi używane przez oprogramowanie pośredniczące konstruktory nie są udostępniane przy użyciu innych typów zależności, wprowadzony podczas każdego żądania. Jeśli musisz udostępnić *zakresie* usługi między oprogramowania pośredniczącego a innymi typami danych, należy dodać tych usług `Invoke` podpis metody. `Invoke` Metoda może obsługiwać dodatkowe parametry, które są wypełniane przez DI:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="middleware-extension-method"></a>Metoda rozszerzenia oprogramowania pośredniczącego

Następujące metody rozszerzenia udostępnia oprogramowanie pośredniczące za pośrednictwem <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

Poniższy kod wywołuje oprogramowanie pośredniczące z `Startup.Configure`:

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
