---
title: Autoryzacja oparta na zasadach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć i używać obsługi zasad autoryzacji do wymuszania wymagań autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837353"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autoryzacja oparta na zasadach w programie ASP.NET Core

Wewnętrznie [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) wymaganie, obsługi wymagań i wstępnie skonfigurowanymi zasadami. Te bloki konstrukcyjne obsługi wyrażenie oceny autoryzacji w kodzie. Wynik jest strukturą autoryzacji bardziej rozbudowane, wielokrotnego użytku, sprawdzalnego działa zgodnie.

Zasady autoryzacji składa się z co najmniej jednego wymagania. Jest on zarejestrowany jako część konfiguracji usługi autoryzacji w `Startup.ConfigureServices` metody:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

W poprzednim przykładzie tworzona jest zasada "AtLeast21". Ma ona pojedynczy wymaganie&mdash;z minimalnym wieku, który jest dostarczany jako parametr do wymagań.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Jest podstawowym usługa, która określa, czy autoryzacji jest pomyślne <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Poprzedni kod pokazuje dwie metody [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> to usługa znacznika żadnych metod i mechanizm śledzenia czy autoryzacja zakończy się pomyślnie.

Każdy <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> jest odpowiedzialny za sprawdzania, czy zostały spełnione określone wymagania:
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> Klasa jest program obsługi używa do oznaczania, czy zostały spełnione wymagania:

```csharp
 context.Succeed(requirement)
```

W poniższym kodzie pokazano uproszczony (i adnotacjami z komentarzami) Domyślna implementacja usługi autoryzacji:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

Poniższy kod przedstawia typową `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

Użyj <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> lub `[Authorize(Policy = "Something"]` autoryzacji.

## <a name="applying-policies-to-mvc-controllers"></a>Stosowanie zasad do kontrolerów MVC

Jeśli używasz stron Razor, zobacz [stosowanie zasad do stron Razor](#applying-policies-to-razor-pages) w tym dokumencie.

Zasady są stosowane do kontrolerów, za pomocą `[Authorize]` atrybutu o nazwie zasady. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Stosowanie zasad do stron Razor

Zasady są stosowane do strony Razor za pomocą `[Authorize]` atrybutu o nazwie zasady. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Zasady mogą również będą stosowane do strony Razor za pomocą [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Wymagania

Wymóg autoryzacji jest to zbiór parametrów danych, które zasady służą do oceny, bieżący podmiot zabezpieczeń użytkownika. W naszych zasadach "AtLeast21" wymagane jest pojedynczy parametr&mdash;minimalnym wieku. Implementuje wymagane [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest interfejsem pustego znacznika. Wymaganie sparametryzowane minimalny wiek może być wdrażany w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Jeśli zasady autoryzacji zawiera wiele wymagań autoryzacji, wszystkie wymagania należy przekazać w kolejności do oceny zasad powiodło się. Innymi słowy, wiele wymagań autoryzacji, dodać do zasad autoryzacji jednego są traktowane w **i** podstawy.

> [!NOTE]
> Wymaganie nie musi być danych lub właściwości.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Programy obsługi autoryzacji

Do obsługi autoryzacji jest odpowiedzialny za oceny właściwości to wymagane. Program obsługi autoryzacji ocenia wymagania względem podanego [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) ustalenie, jeśli dostęp jest dozwolony.

Wymagania mogą mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers). Program obsługi może dziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymagane do obsługi. Alternatywnie program obsługi może wdrożyć [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) do obsługi więcej niż jeden typ wymagania.

### <a name="use-a-handler-for-one-requirement"></a>Program obsługi na użytek jedno wymaganie dotyczące

<a name="security-authorization-handler-example"></a>

Oto przykład relacja jeden do jednego, w którym program obsługi minimalny wiek korzysta z jednego wymagania:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Powyższy kod określa, czy bieżący użytkownik podmiotu zabezpieczeń ma datę urodzenia oświadczenia, który został wystawiony przez znanego i zaufanego wystawcy. Autoryzacji nie może wystąpić, jeśli brakuje oświadczenie, w którym to przypadku jest zwracany ukończonego zadania. Jeśli występuje oświadczenie wieku użytkownika jest obliczana. Jeśli użytkownik spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacja uważa, że pomyślnie. Gdy autoryzacja zakończy się pomyślnie, `context.Succeed` jest wywoływana z spełnione wymaganie, jako jedyny parametr.

### <a name="use-a-handler-for-multiple-requirements"></a>Program obsługi na użytek wiele wymagań

Oto przykład relacji jeden do wielu, w którym program obsługi uprawnienia może obsłużyć trzy różne rodzaje wymagania:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Powyższy kod przechodzi przez [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwość nie zawiera wymagania oznaczona jako pomyślne. Aby uzyskać `ReadPermission` wymagań, użytkownik musi być właścicielem lub sponsora, dostępu do żądanego zasobu. W przypadku właściwości `EditPermission` lub `DeletePermission` wymaganie dany użytkownik, musisz być właścicielem dostępu do żądanego zasobu.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Rejestracja programu obsługi

Programy obsługi są rejestrowane w kolekcji usługi podczas konfiguracji. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Powyższy kod rejestruje `MinimumAgeHandler` jako pojedyncze wywołanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Programy obsługi można zarejestrować przy użyciu dowolnej z wbudowanych [usługi okresy istnienia](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Co powinna zwracać program obsługi?

Należy pamiętać, że `Handle` method in Class metoda [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości. Jaki jest stan powodzenia lub niepowodzenia wskazane?

* Program obsługi wskazuje wynik, wywołując `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, który został pomyślnie zweryfikowany.

* Program obsługi nie musi obsługiwać błędy ogólnie rzecz biorąc, zgodnie z innych programów obsługi, aby uzyskać te same wymagania może się powieść.

* Aby zagwarantować awarii, nawet wtedy, gdy powiedzie się w innych programach obsługi wymagań, należy wywołać `context.Fail`.

Wywołuje program obsługi `context.Succeed` lub `context.Fail`, nadal są wywoływane przez wszystkie inne programy obsługi. Dzięki temu wymagania wygenerować ubocznych, takich jak rejestrowanie, które odbywa się nawet w przypadku innego programu obsługi został pomyślnie zweryfikowany lub nie powiodło się wymagania. Po ustawieniu `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) właściwości (dostępny w programie ASP.NET Core 1.1 i nowszych) short-circuits wykonywania programów obsługi podczas `context.Fail` jest wywoływana. `InvokeHandlersAfterFailure` Wartość domyślna to `true`, w którym to przypadku wszystkie wywołania.

> [!NOTE]
> Są wywoływane programy obsługi autoryzacji, nawet w przypadku niepowodzenia uwierzytelniania.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Dlaczego należy wielu obsług dla wymagania?

W przypadkach, w której chcesz oceny na **lub** naliczana wdrożenia wielu obsług dla pojedynczego wymagania. Na przykład firma Microsoft ma drzwi otwierać tylko przy użyciu kluczy kart. Jeśli Twoja karta klucza pozostanie w domu, recepcjonista drukuje tymczasowe nalepkę i otwiera drzwi biblioteki. W tym scenariuszu miałby jednego wymagania, *BuildingEntry*, ale wielu obsług, każdy z nich badanie jedno zapotrzebowanie.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Upewnij się, że oba programy obsługi [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). W przypadku obu obsługi zakończy się pomyślnie, gdy zasady oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.

## <a name="using-a-func-to-fulfill-a-policy"></a>Aby spełnić zasady przy użyciu func

Mogą wystąpić sytuacje, w których wypełniając zasad jest proste wyrażenia w kodzie. Jest to możliwe, aby podać `Func<AuthorizationHandlerContext, bool>` podczas konfigurowania zasad `RequireAssertion` konstruktora zasad.

Na przykład poprzedniej `BadgeEntryHandler` można dopasować w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Uzyskiwanie dostępu do kontekstu żądania MVC w procedurach obsługi

`HandleRequirementAsync` Metody wdrożenia w obsłudze autoryzacji zawiera dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi. Struktur, takich jak MVC lub Jabbr są bezpłatne dowolny obiekt, aby dodać `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.

Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości. Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko inne podany, MVC i stron Razor.

Korzystanie z `Resource` właściwość to struktura określone. Korzystając z informacji w `Resource` właściwość ogranicza zasad autoryzacji do określonej struktury. Należy rzutować `Resource` właściwość za pomocą `is` słowo kluczowe, a następnie potwierdź rzutowanie zakończyła się pomyślnie, aby upewnić się, kod nie awarii przy użyciu `InvalidCastException` uruchamiania innych platform:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
