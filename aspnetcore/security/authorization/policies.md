---
title: Autoryzacja oparta na zasadach w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzyć i używać programów obsługi zasad autoryzacji w celu wymuszania wymagań autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828961"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autoryzacja oparta na zasadach w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Zgodnie z założeniami, [Autoryzacja oparta na rolach](xref:security/authorization/roles) i [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims) używają wymagań, obsługi wymagań i wstępnie skonfigurowanych zasad. Te bloki konstrukcyjne obsługują wyrażenie oceny autoryzacji w kodzie. Wynikiem jest rozbudowana struktura autoryzacji do wielokrotnego użytku weryfikowalne.

Zasady autoryzacji składają się z co najmniej jednego wymagania. Jest ona zarejestrowana w ramach konfiguracji usługi autoryzacji w metodzie `Startup.ConfigureServices`:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

W poprzednim przykładzie tworzone są zasady "AtLeast21". Jest to jedyne wymaganie&mdash;o minimalnym wieku, który jest dostarczany jako parametr do wymagania.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Podstawowa usługa określająca, czy Autoryzacja jest pomyślna <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Poprzedni kod wyróżnia dwie metody [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> to usługa znacznika bez metod i mechanizm śledzenia, czy autoryzacja zakończyła się pomyślnie.

Każdy <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> jest odpowiedzialny za sprawdzenie, czy są spełnione wymagania:
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

Klasa <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> jest wykorzystywana przez program obsługi do oznaczania, czy zostały spełnione wymagania:

```csharp
 context.Succeed(requirement)
```

Poniższy kod przedstawia uproszczoną (i adnotację z komentarzami) domyślną implementację usługi autoryzacji:

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

Poniższy kod przedstawia typowy `ConfigureServices`:

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


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

Użyj <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> lub `[Authorize(Policy = "Something")]` do autoryzacji.

## <a name="applying-policies-to-mvc-controllers"></a>Stosowanie zasad do kontrolerów MVC

Jeśli używasz Razor Pages, zobacz [stosowanie zasad do Razor Pages](#applying-policies-to-razor-pages) w tym dokumencie.

Zasady są stosowane do kontrolerów przy użyciu atrybutu `[Authorize]` z nazwą zasad. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Stosowanie zasad do Razor Pages

Zasady są stosowane do Razor Pages przy użyciu atrybutu `[Authorize]` z nazwą zasad. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Zasady mogą być również stosowane do Razor Pages przy użyciu [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Wymagania

Wymaganie autoryzacji to zbiór parametrów danych, których zasady mogą użyć do oszacowania bieżącego podmiotu użytkownika. W naszych zasadach "AtLeast21" wymagany jest jeden parametr&mdash;minimalnym wieku. Wymaganie implementuje [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest pustym interfejsem znacznika. Minimalny wymóg sparametryzowanego wieku można zaimplementować w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Jeśli zasady autoryzacji zawierają wiele wymagań autoryzacji, wszystkie wymagania muszą zostać spełnione, aby Ocena zasad powiodła się. Innymi słowy, wiele wymagań autoryzacji dodanych do pojedynczych zasad autoryzacji jest traktowanych **na zasadzie.**

> [!NOTE]
> Wymagania nie muszą mieć danych ani właściwości.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Programy obsługi autoryzacji

Procedura obsługi autoryzacji jest odpowiedzialna za ocenę właściwości wymagania. Procedura obsługi autoryzacji szacuje wymagania w odniesieniu do podanej [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) w celu określenia, czy dostęp jest dozwolony.

Wymaganie może mieć [wiele programów obsługi](#security-authorization-policies-based-multiple-handlers). Program obsługi może odziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymaganiem do obsługi. Alternatywnie program obsługi może zaimplementować [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , aby obsłużyć więcej niż jeden typ wymagania.

### <a name="use-a-handler-for-one-requirement"></a>Użyj procedury obsługi dla jednego wymagania

<a name="security-authorization-handler-example"></a>

Poniżej znajduje się przykład relacji jeden-do-jednego, w której program obsługi minimalnych okresów używa jednego wymagania:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Poprzedni kod określa, czy bieżący podmiot użytkownika ma datę żądania urodzenia, który został wystawiony przez znanego i zaufanego wystawcy. Autoryzacja nie może wystąpić w przypadku braku żądania. w takim przypadku zwracane jest zadanie wykonane. W przypadku wystąpienia zgłoszenia jest naliczany wiek użytkownika. Jeśli użytkownik spełni minimalny wiek zdefiniowany przez to wymaganie, autoryzacja zostanie uznana za pomyślnie. Po pomyślnym uwierzytelnieniu `context.Succeed` jest wywoływana z wymaganym parametrem.

### <a name="use-a-handler-for-multiple-requirements"></a>Korzystanie z programu obsługi dla wielu wymagań

Poniżej znajduje się przykład relacji jeden-do-wielu, w której program obsługi uprawnień może obsłużyć trzy różne typy wymagań:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Poprzedni kod przechodzi [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwości zawierającej wymagania, które nie są oznaczone jako pomyślne. W przypadku wymaganego `ReadPermission` użytkownik musi być właścicielem lub sponsorem, aby uzyskać dostęp do żądanego zasobu. W przypadku `EditPermission` lub `DeletePermission` wymaganie musi być właścicielem, aby uzyskać dostęp do żądanego zasobu.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Rejestracja procedury obsługi

Procedury obsługi są rejestrowane w kolekcji usług podczas konfiguracji. Na przykład:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

Poprzedni kod rejestruje `MinimumAgeHandler` jako pojedyncze przez wywoływanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Programy obsługi można zarejestrować przy użyciu dowolnego z wbudowanych [okresów istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Co ma zwrócić program obsługi?

Należy zauważyć, że metoda `Handle` w [przykładzie procedury obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości. Jak jest wskazywany stan sukcesu lub niepowodzenia?

* Procedura obsługi wskazuje powodzenie przez wywołanie `context.Succeed(IAuthorizationRequirement requirement)`, przekazanie pomyślnie zweryfikowanego wymagania.

* Program obsługi nie musi ogólnie obsługiwać błędów, ponieważ inne procedury obsługi tego samego wymagania mogą się powieść.

* Aby zagwarantować awarię, nawet jeśli inne programy obsługi wymagań zakończą się powodzeniem, wywołaj `context.Fail`.

Jeśli program obsługi wywołuje `context.Succeed` lub `context.Fail`, wszystkie pozostałe procedury obsługi nadal są wywoływane. Pozwala to na spełnienie wymagań związanych z generowaniem efektów ubocznych, takich jak rejestrowanie, które odbywa się nawet w przypadku pomyślnej weryfikacji lub niepowodzenia przez inną procedurę obsługi. Po ustawieniu na `false`Właściwość [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (dostępna w ASP.NET Core 1,1 i nowszych) skraca obwody wykonywanie programów obsługi po wywołaniu `context.Fail`. `InvokeHandlersAfterFailure` domyślne do `true`, w którym przypadku są wywoływane wszystkie programy obsługi.

> [!NOTE]
> Procedury obsługi autoryzacji są wywoływane, nawet jeśli uwierzytelnianie nie powiedzie się.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Dlaczego chcesz mieć wiele programów obsługi wymagań?

W przypadkach, w których Ocena ma być przeprowadzana na podstawie **lub** , należy zaimplementować wiele programów obsługi dla jednego wymagania. Na przykład firma Microsoft ma drzwi, które są otwierane tylko za pomocą kart kluczowych. Jeśli opuścisz kartę kluczową w domu, recepcjonista drukuje tymczasowy naklejkę i otwiera drzwiczki. W tym scenariuszu istnieje jedno wymaganie, *BuildingEntry*, ale wiele programów obsługi, każdy z nich bada pojedyncze wymaganie.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Upewnij się, że oba programy obsługi są [zarejestrowane](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Jeśli jedna z programów obsługi powiedzie się, gdy zasady oceniają `BuildingEntryRequirement`, Ocena zasad powiedzie się.

## <a name="using-a-func-to-fulfill-a-policy"></a>Korzystanie z funkcji Func do realizacji zasad

Mogą wystąpić sytuacje, w których spełnienie zasad jest proste do wyrażania w kodzie. Podczas konfigurowania zasad przy użyciu konstruktora zasad `RequireAssertion` można podać `Func<AuthorizationHandlerContext, bool>`.

Na przykład poprzednie `BadgeEntryHandler` można napisać ponownie w następujący sposób:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Uzyskiwanie dostępu do kontekstu żądania MVC w programach obsługi

Metoda `HandleRequirementAsync` zaimplementowana w procedurze obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement`, które są obsługiwane. Struktury, takie jak MVC lub Jabbr, są bezpłatne, aby dodać dowolny obiekt do właściwości `Resource` na `AuthorizationHandlerContext` w celu przekazania dodatkowych informacji.

Na przykład, MVC przekazuje wystąpienie elementu [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) we właściwości `Resource`. Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`i wszystkich innych danych udostępnianych przez MVC i Razor Pages.

Użycie właściwości `Resource` jest specyficzne dla platformy. Użycie informacji w `Resource` Właściwość ogranicza zasady autoryzacji do określonych struktur. Należy rzutować Właściwość `Resource` za pomocą słowa kluczowego `is`, a następnie upewnić się, że rzutowanie zakończyło się pomyślnie, aby upewnić się, że kod nie ulegnie awarii z `InvalidCastException` w przypadku uruchamiania w innych strukturach:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

Zgodnie z założeniami, [Autoryzacja oparta na rolach](xref:security/authorization/roles) i [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims) używają wymagań, obsługi wymagań i wstępnie skonfigurowanych zasad. Te bloki konstrukcyjne obsługują wyrażenie oceny autoryzacji w kodzie. Wynikiem jest rozbudowana struktura autoryzacji do wielokrotnego użytku weryfikowalne.

Zasady autoryzacji składają się z co najmniej jednego wymagania. Jest ona zarejestrowana w ramach konfiguracji usługi autoryzacji w metodzie `Startup.ConfigureServices`:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

W poprzednim przykładzie tworzone są zasady "AtLeast21". Jest to jedyne wymaganie&mdash;o minimalnym wieku, który jest dostarczany jako parametr do wymagania.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Podstawowa usługa określająca, czy Autoryzacja jest pomyślna <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Poprzedni kod wyróżnia dwie metody [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> to usługa znacznika bez metod i mechanizm śledzenia, czy autoryzacja zakończyła się pomyślnie.

Każdy <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> jest odpowiedzialny za sprawdzenie, czy są spełnione wymagania:
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

Klasa <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> jest wykorzystywana przez program obsługi do oznaczania, czy zostały spełnione wymagania:

```csharp
 context.Succeed(requirement)
```

Poniższy kod przedstawia uproszczoną (i adnotację z komentarzami) domyślną implementację usługi autoryzacji:

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

Poniższy kod przedstawia typowy `ConfigureServices`:

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

Użyj <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> lub `[Authorize(Policy = "Something")]` do autoryzacji.

## <a name="applying-policies-to-mvc-controllers"></a>Stosowanie zasad do kontrolerów MVC

Jeśli używasz Razor Pages, zobacz [stosowanie zasad do Razor Pages](#applying-policies-to-razor-pages) w tym dokumencie.

Zasady są stosowane do kontrolerów przy użyciu atrybutu `[Authorize]` z nazwą zasad. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Stosowanie zasad do Razor Pages

Zasady są stosowane do Razor Pages przy użyciu atrybutu `[Authorize]` z nazwą zasad. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Zasady mogą być również stosowane do Razor Pages przy użyciu [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Wymagania

Wymaganie autoryzacji to zbiór parametrów danych, których zasady mogą użyć do oszacowania bieżącego podmiotu użytkownika. W naszych zasadach "AtLeast21" wymagany jest jeden parametr&mdash;minimalnym wieku. Wymaganie implementuje [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest pustym interfejsem znacznika. Minimalny wymóg sparametryzowanego wieku można zaimplementować w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Jeśli zasady autoryzacji zawierają wiele wymagań autoryzacji, wszystkie wymagania muszą zostać spełnione, aby Ocena zasad powiodła się. Innymi słowy, wiele wymagań autoryzacji dodanych do pojedynczych zasad autoryzacji jest traktowanych **na zasadzie.**

> [!NOTE]
> Wymagania nie muszą mieć danych ani właściwości.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Programy obsługi autoryzacji

Procedura obsługi autoryzacji jest odpowiedzialna za ocenę właściwości wymagania. Procedura obsługi autoryzacji szacuje wymagania w odniesieniu do podanej [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) w celu określenia, czy dostęp jest dozwolony.

Wymaganie może mieć [wiele programów obsługi](#security-authorization-policies-based-multiple-handlers). Program obsługi może odziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymaganiem do obsługi. Alternatywnie program obsługi może zaimplementować [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , aby obsłużyć więcej niż jeden typ wymagania.

### <a name="use-a-handler-for-one-requirement"></a>Użyj procedury obsługi dla jednego wymagania

<a name="security-authorization-handler-example"></a>

Poniżej znajduje się przykład relacji jeden-do-jednego, w której program obsługi minimalnych okresów używa jednego wymagania:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Poprzedni kod określa, czy bieżący podmiot użytkownika ma datę żądania urodzenia, który został wystawiony przez znanego i zaufanego wystawcy. Autoryzacja nie może wystąpić w przypadku braku żądania. w takim przypadku zwracane jest zadanie wykonane. W przypadku wystąpienia zgłoszenia jest naliczany wiek użytkownika. Jeśli użytkownik spełni minimalny wiek zdefiniowany przez to wymaganie, autoryzacja zostanie uznana za pomyślnie. Po pomyślnym uwierzytelnieniu `context.Succeed` jest wywoływana z wymaganym parametrem.

### <a name="use-a-handler-for-multiple-requirements"></a>Korzystanie z programu obsługi dla wielu wymagań

Poniżej znajduje się przykład relacji jeden-do-wielu, w której program obsługi uprawnień może obsłużyć trzy różne typy wymagań:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Poprzedni kod przechodzi [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwości zawierającej wymagania, które nie są oznaczone jako pomyślne. W przypadku wymaganego `ReadPermission` użytkownik musi być właścicielem lub sponsorem, aby uzyskać dostęp do żądanego zasobu. W przypadku `EditPermission` lub `DeletePermission` wymaganie musi być właścicielem, aby uzyskać dostęp do żądanego zasobu.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Rejestracja procedury obsługi

Procedury obsługi są rejestrowane w kolekcji usług podczas konfiguracji. Na przykład:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Poprzedni kod rejestruje `MinimumAgeHandler` jako pojedyncze przez wywoływanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Programy obsługi można zarejestrować przy użyciu dowolnego z wbudowanych [okresów istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Co ma zwrócić program obsługi?

Należy zauważyć, że metoda `Handle` w [przykładzie procedury obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości. Jak jest wskazywany stan sukcesu lub niepowodzenia?

* Procedura obsługi wskazuje powodzenie przez wywołanie `context.Succeed(IAuthorizationRequirement requirement)`, przekazanie pomyślnie zweryfikowanego wymagania.

* Program obsługi nie musi ogólnie obsługiwać błędów, ponieważ inne procedury obsługi tego samego wymagania mogą się powieść.

* Aby zagwarantować awarię, nawet jeśli inne programy obsługi wymagań zakończą się powodzeniem, wywołaj `context.Fail`.

Jeśli program obsługi wywołuje `context.Succeed` lub `context.Fail`, wszystkie pozostałe procedury obsługi nadal są wywoływane. Pozwala to na spełnienie wymagań związanych z generowaniem efektów ubocznych, takich jak rejestrowanie, które odbywa się nawet w przypadku pomyślnej weryfikacji lub niepowodzenia przez inną procedurę obsługi. Po ustawieniu na `false`Właściwość [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (dostępna w ASP.NET Core 1,1 i nowszych) skraca obwody wykonywanie programów obsługi po wywołaniu `context.Fail`. `InvokeHandlersAfterFailure` domyślne do `true`, w którym przypadku są wywoływane wszystkie programy obsługi.

> [!NOTE]
> Procedury obsługi autoryzacji są wywoływane, nawet jeśli uwierzytelnianie nie powiedzie się.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Dlaczego chcesz mieć wiele programów obsługi wymagań?

W przypadkach, w których Ocena ma być przeprowadzana na podstawie **lub** , należy zaimplementować wiele programów obsługi dla jednego wymagania. Na przykład firma Microsoft ma drzwi, które są otwierane tylko za pomocą kart kluczowych. Jeśli opuścisz kartę kluczową w domu, recepcjonista drukuje tymczasowy naklejkę i otwiera drzwiczki. W tym scenariuszu istnieje jedno wymaganie, *BuildingEntry*, ale wiele programów obsługi, każdy z nich bada pojedyncze wymaganie.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Upewnij się, że oba programy obsługi są [zarejestrowane](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Jeśli jedna z programów obsługi powiedzie się, gdy zasady oceniają `BuildingEntryRequirement`, Ocena zasad powiedzie się.

## <a name="using-a-func-to-fulfill-a-policy"></a>Korzystanie z funkcji Func do realizacji zasad

Mogą wystąpić sytuacje, w których spełnienie zasad jest proste do wyrażania w kodzie. Podczas konfigurowania zasad przy użyciu konstruktora zasad `RequireAssertion` można podać `Func<AuthorizationHandlerContext, bool>`.

Na przykład poprzednie `BadgeEntryHandler` można napisać ponownie w następujący sposób:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Uzyskiwanie dostępu do kontekstu żądania MVC w programach obsługi

Metoda `HandleRequirementAsync` zaimplementowana w procedurze obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement`, które są obsługiwane. Struktury, takie jak MVC lub Jabbr, są bezpłatne, aby dodać dowolny obiekt do właściwości `Resource` na `AuthorizationHandlerContext` w celu przekazania dodatkowych informacji.

Na przykład, MVC przekazuje wystąpienie elementu [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) we właściwości `Resource`. Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`i wszystkich innych danych udostępnianych przez MVC i Razor Pages.

Użycie właściwości `Resource` jest specyficzne dla platformy. Użycie informacji w `Resource` Właściwość ogranicza zasady autoryzacji do określonych struktur. Należy rzutować Właściwość `Resource` za pomocą słowa kluczowego `is`, a następnie upewnić się, że rzutowanie zakończyło się pomyślnie, aby upewnić się, że kod nie ulegnie awarii z `InvalidCastException` w przypadku uruchamiania w innych strukturach:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end