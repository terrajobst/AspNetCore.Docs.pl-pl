---
title: Autoryzacja oparta na zasadach w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzyć i używać programów obsługi zasad autoryzacji w celu wymuszania wymagań autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661813"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="88c70-103">Autoryzacja oparta na zasadach w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88c70-103">Policy-based authorization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88c70-104">Zgodnie z założeniami, [Autoryzacja oparta na rolach](xref:security/authorization/roles) i [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims) używają wymagań, obsługi wymagań i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="88c70-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="88c70-105">Te bloki konstrukcyjne obsługują wyrażenie oceny autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="88c70-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="88c70-106">Wynikiem jest rozbudowana struktura autoryzacji do wielokrotnego użytku weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="88c70-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="88c70-107">Zasady autoryzacji składają się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="88c70-108">Jest ona zarejestrowana w ramach konfiguracji usługi autoryzacji w metodzie `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="88c70-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

<span data-ttu-id="88c70-109">W poprzednim przykładzie tworzone są zasady "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="88c70-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="88c70-110">Jest to jedyne wymaganie&mdash;o minimalnym wieku, który jest dostarczany jako parametr do wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="88c70-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="88c70-111">IAuthorizationService</span></span> 

<span data-ttu-id="88c70-112">Podstawowa usługa określająca, czy Autoryzacja jest pomyślna <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="88c70-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="88c70-113">Poprzedni kod wyróżnia dwie metody [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="88c70-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="88c70-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> to usługa znacznika bez metod i mechanizm śledzenia, czy autoryzacja zakończyła się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="88c70-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="88c70-115">Każdy <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> jest odpowiedzialny za sprawdzenie, czy są spełnione wymagania:</span><span class="sxs-lookup"><span data-stu-id="88c70-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
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

<span data-ttu-id="88c70-116">Klasa <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> jest wykorzystywana przez program obsługi do oznaczania, czy zostały spełnione wymagania:</span><span class="sxs-lookup"><span data-stu-id="88c70-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="88c70-117">Poniższy kod przedstawia uproszczoną (i adnotację z komentarzami) domyślną implementację usługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="88c70-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="88c70-118">Poniższy kod przedstawia typowy `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="88c70-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="88c70-119">Użyj <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> lub `[Authorize(Policy = "Something")]` do autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="88c70-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="88c70-120">Stosowanie zasad do kontrolerów MVC</span><span class="sxs-lookup"><span data-stu-id="88c70-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="88c70-121">Jeśli używasz Razor Pages, zobacz [stosowanie zasad do Razor Pages](#applying-policies-to-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="88c70-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="88c70-122">Zasady są stosowane do kontrolerów przy użyciu atrybutu `[Authorize]` z nazwą zasad.</span><span class="sxs-lookup"><span data-stu-id="88c70-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="88c70-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88c70-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="88c70-124">Stosowanie zasad do Razor Pages</span><span class="sxs-lookup"><span data-stu-id="88c70-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="88c70-125">Zasady są stosowane do Razor Pages przy użyciu atrybutu `[Authorize]` z nazwą zasad.</span><span class="sxs-lookup"><span data-stu-id="88c70-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="88c70-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88c70-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="88c70-127">Zasady mogą być również stosowane do Razor Pages przy użyciu [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="88c70-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="88c70-128">Wymagania</span><span class="sxs-lookup"><span data-stu-id="88c70-128">Requirements</span></span>

<span data-ttu-id="88c70-129">Wymaganie autoryzacji to zbiór parametrów danych, których zasady mogą użyć do oszacowania bieżącego podmiotu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="88c70-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="88c70-130">W naszych zasadach "AtLeast21" wymagany jest jeden parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="88c70-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="88c70-131">Wymaganie implementuje [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest pustym interfejsem znacznika.</span><span class="sxs-lookup"><span data-stu-id="88c70-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="88c70-132">Minimalny wymóg sparametryzowanego wieku można zaimplementować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="88c70-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="88c70-133">Jeśli zasady autoryzacji zawierają wiele wymagań autoryzacji, wszystkie wymagania muszą zostać spełnione, aby Ocena zasad powiodła się.</span><span class="sxs-lookup"><span data-stu-id="88c70-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="88c70-134">Innymi słowy, wiele wymagań autoryzacji dodanych do pojedynczych zasad autoryzacji jest traktowanych **na zasadzie.**</span><span class="sxs-lookup"><span data-stu-id="88c70-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="88c70-135">Wymagania nie muszą mieć danych ani właściwości.</span><span class="sxs-lookup"><span data-stu-id="88c70-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="88c70-136">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="88c70-136">Authorization handlers</span></span>

<span data-ttu-id="88c70-137">Procedura obsługi autoryzacji jest odpowiedzialna za ocenę właściwości wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="88c70-138">Procedura obsługi autoryzacji szacuje wymagania w odniesieniu do podanej [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) w celu określenia, czy dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="88c70-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="88c70-139">Wymaganie może mieć [wiele programów obsługi](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="88c70-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="88c70-140">Program obsługi może odziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymaganiem do obsługi.</span><span class="sxs-lookup"><span data-stu-id="88c70-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="88c70-141">Alternatywnie program obsługi może zaimplementować [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , aby obsłużyć więcej niż jeden typ wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="88c70-142">Użyj procedury obsługi dla jednego wymagania</span><span class="sxs-lookup"><span data-stu-id="88c70-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="88c70-143">Poniżej znajduje się przykład relacji jeden-do-jednego, w której program obsługi minimalnych okresów używa jednego wymagania:</span><span class="sxs-lookup"><span data-stu-id="88c70-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="88c70-144">Poprzedni kod określa, czy bieżący podmiot użytkownika ma datę żądania urodzenia, który został wystawiony przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="88c70-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="88c70-145">Autoryzacja nie może wystąpić w przypadku braku żądania. w takim przypadku zwracane jest zadanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="88c70-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="88c70-146">W przypadku wystąpienia zgłoszenia jest naliczany wiek użytkownika.</span><span class="sxs-lookup"><span data-stu-id="88c70-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="88c70-147">Jeśli użytkownik spełni minimalny wiek zdefiniowany przez to wymaganie, autoryzacja zostanie uznana za pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="88c70-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="88c70-148">Po pomyślnym uwierzytelnieniu `context.Succeed` jest wywoływana z wymaganym parametrem.</span><span class="sxs-lookup"><span data-stu-id="88c70-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="88c70-149">Korzystanie z programu obsługi dla wielu wymagań</span><span class="sxs-lookup"><span data-stu-id="88c70-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="88c70-150">Poniżej znajduje się przykład relacji jeden-do-wielu, w której program obsługi uprawnień może obsłużyć trzy różne typy wymagań:</span><span class="sxs-lookup"><span data-stu-id="88c70-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="88c70-151">Poprzedni kod przechodzi [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwości zawierającej wymagania, które nie są oznaczone jako pomyślne.</span><span class="sxs-lookup"><span data-stu-id="88c70-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="88c70-152">W przypadku wymaganego `ReadPermission` użytkownik musi być właścicielem lub sponsorem, aby uzyskać dostęp do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="88c70-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="88c70-153">W przypadku `EditPermission` lub `DeletePermission` wymaganie musi być właścicielem, aby uzyskać dostęp do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="88c70-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="88c70-154">Rejestracja procedury obsługi</span><span class="sxs-lookup"><span data-stu-id="88c70-154">Handler registration</span></span>

<span data-ttu-id="88c70-155">Procedury obsługi są rejestrowane w kolekcji usług podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="88c70-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="88c70-156">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88c70-156">For example:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

<span data-ttu-id="88c70-157">Poprzedni kod rejestruje `MinimumAgeHandler` jako pojedyncze przez wywoływanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="88c70-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="88c70-158">Programy obsługi można zarejestrować przy użyciu dowolnego z wbudowanych [okresów istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="88c70-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="88c70-159">Co ma zwrócić program obsługi?</span><span class="sxs-lookup"><span data-stu-id="88c70-159">What should a handler return?</span></span>

<span data-ttu-id="88c70-160">Należy zauważyć, że metoda `Handle` w [przykładzie procedury obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="88c70-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="88c70-161">Jak jest wskazywany stan sukcesu lub niepowodzenia?</span><span class="sxs-lookup"><span data-stu-id="88c70-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="88c70-162">Procedura obsługi wskazuje powodzenie przez wywołanie `context.Succeed(IAuthorizationRequirement requirement)`, przekazanie pomyślnie zweryfikowanego wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="88c70-163">Program obsługi nie musi ogólnie obsługiwać błędów, ponieważ inne procedury obsługi tego samego wymagania mogą się powieść.</span><span class="sxs-lookup"><span data-stu-id="88c70-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="88c70-164">Aby zagwarantować awarię, nawet jeśli inne programy obsługi wymagań zakończą się powodzeniem, wywołaj `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="88c70-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="88c70-165">Jeśli program obsługi wywołuje `context.Succeed` lub `context.Fail`, wszystkie pozostałe procedury obsługi nadal są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="88c70-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="88c70-166">Pozwala to na spełnienie wymagań związanych z generowaniem efektów ubocznych, takich jak rejestrowanie, które odbywa się nawet w przypadku pomyślnej weryfikacji lub niepowodzenia przez inną procedurę obsługi.</span><span class="sxs-lookup"><span data-stu-id="88c70-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="88c70-167">Po ustawieniu na `false`Właściwość [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (dostępna w ASP.NET Core 1,1 i nowszych) skraca obwody wykonywanie programów obsługi po wywołaniu `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="88c70-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="88c70-168">`InvokeHandlersAfterFailure` domyślne do `true`, w którym przypadku są wywoływane wszystkie programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="88c70-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="88c70-169">Procedury obsługi autoryzacji są wywoływane, nawet jeśli uwierzytelnianie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="88c70-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="88c70-170">Dlaczego chcesz mieć wiele programów obsługi wymagań?</span><span class="sxs-lookup"><span data-stu-id="88c70-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="88c70-171">W przypadkach, w których Ocena ma być przeprowadzana na podstawie **lub** , należy zaimplementować wiele programów obsługi dla jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="88c70-172">Na przykład firma Microsoft ma drzwi, które są otwierane tylko za pomocą kart kluczowych.</span><span class="sxs-lookup"><span data-stu-id="88c70-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="88c70-173">Jeśli opuścisz kartę kluczową w domu, recepcjonista drukuje tymczasowy naklejkę i otwiera drzwiczki.</span><span class="sxs-lookup"><span data-stu-id="88c70-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="88c70-174">W tym scenariuszu istnieje jedno wymaganie, *BuildingEntry*, ale wiele programów obsługi, każdy z nich bada pojedyncze wymaganie.</span><span class="sxs-lookup"><span data-stu-id="88c70-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="88c70-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="88c70-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="88c70-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="88c70-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="88c70-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="88c70-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="88c70-178">Upewnij się, że oba programy obsługi są [zarejestrowane](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="88c70-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="88c70-179">Jeśli jedna z programów obsługi powiedzie się, gdy zasady oceniają `BuildingEntryRequirement`, Ocena zasad powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="88c70-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="88c70-180">Korzystanie z funkcji Func do realizacji zasad</span><span class="sxs-lookup"><span data-stu-id="88c70-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="88c70-181">Mogą wystąpić sytuacje, w których spełnienie zasad jest proste do wyrażania w kodzie.</span><span class="sxs-lookup"><span data-stu-id="88c70-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="88c70-182">Podczas konfigurowania zasad przy użyciu konstruktora zasad `RequireAssertion` można podać `Func<AuthorizationHandlerContext, bool>`.</span><span class="sxs-lookup"><span data-stu-id="88c70-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="88c70-183">Na przykład poprzednie `BadgeEntryHandler` można napisać ponownie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="88c70-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="88c70-184">Uzyskiwanie dostępu do kontekstu żądania MVC w programach obsługi</span><span class="sxs-lookup"><span data-stu-id="88c70-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="88c70-185">Metoda `HandleRequirementAsync` zaimplementowana w procedurze obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement`, które są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="88c70-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="88c70-186">Struktury, takie jak MVC lub Jabbr, są bezpłatne, aby dodać dowolny obiekt do właściwości `Resource` na `AuthorizationHandlerContext` w celu przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="88c70-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="88c70-187">Na przykład, MVC przekazuje wystąpienie elementu [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) we właściwości `Resource`.</span><span class="sxs-lookup"><span data-stu-id="88c70-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="88c70-188">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`i wszystkich innych danych udostępnianych przez MVC i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="88c70-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="88c70-189">Użycie właściwości `Resource` jest specyficzne dla platformy.</span><span class="sxs-lookup"><span data-stu-id="88c70-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="88c70-190">Użycie informacji w `Resource` Właściwość ogranicza zasady autoryzacji do określonych struktur.</span><span class="sxs-lookup"><span data-stu-id="88c70-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="88c70-191">Należy rzutować Właściwość `Resource` za pomocą słowa kluczowego `is`, a następnie upewnić się, że rzutowanie zakończyło się pomyślnie, aby upewnić się, że kod nie ulegnie awarii z `InvalidCastException` w przypadku uruchamiania w innych strukturach:</span><span class="sxs-lookup"><span data-stu-id="88c70-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

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

<span data-ttu-id="88c70-192">Zgodnie z założeniami, [Autoryzacja oparta na rolach](xref:security/authorization/roles) i [Autoryzacja oparta na oświadczeniach](xref:security/authorization/claims) używają wymagań, obsługi wymagań i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="88c70-192">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="88c70-193">Te bloki konstrukcyjne obsługują wyrażenie oceny autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="88c70-193">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="88c70-194">Wynikiem jest rozbudowana struktura autoryzacji do wielokrotnego użytku weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="88c70-194">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="88c70-195">Zasady autoryzacji składają się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-195">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="88c70-196">Jest ona zarejestrowana w ramach konfiguracji usługi autoryzacji w metodzie `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="88c70-196">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="88c70-197">W poprzednim przykładzie tworzone są zasady "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="88c70-197">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="88c70-198">Jest to jedyne wymaganie&mdash;o minimalnym wieku, który jest dostarczany jako parametr do wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-198">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="88c70-199">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="88c70-199">IAuthorizationService</span></span> 

<span data-ttu-id="88c70-200">Podstawowa usługa określająca, czy Autoryzacja jest pomyślna <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="88c70-200">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="88c70-201">Poprzedni kod wyróżnia dwie metody [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="88c70-201">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="88c70-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> to usługa znacznika bez metod i mechanizm śledzenia, czy autoryzacja zakończyła się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="88c70-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="88c70-203">Każdy <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> jest odpowiedzialny za sprawdzenie, czy są spełnione wymagania:</span><span class="sxs-lookup"><span data-stu-id="88c70-203">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
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

<span data-ttu-id="88c70-204">Klasa <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> jest wykorzystywana przez program obsługi do oznaczania, czy zostały spełnione wymagania:</span><span class="sxs-lookup"><span data-stu-id="88c70-204">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="88c70-205">Poniższy kod przedstawia uproszczoną (i adnotację z komentarzami) domyślną implementację usługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="88c70-205">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="88c70-206">Poniższy kod przedstawia typowy `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="88c70-206">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="88c70-207">Użyj <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> lub `[Authorize(Policy = "Something")]` do autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="88c70-207">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="88c70-208">Stosowanie zasad do kontrolerów MVC</span><span class="sxs-lookup"><span data-stu-id="88c70-208">Applying policies to MVC controllers</span></span>

<span data-ttu-id="88c70-209">Jeśli używasz Razor Pages, zobacz [stosowanie zasad do Razor Pages](#applying-policies-to-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="88c70-209">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="88c70-210">Zasady są stosowane do kontrolerów przy użyciu atrybutu `[Authorize]` z nazwą zasad.</span><span class="sxs-lookup"><span data-stu-id="88c70-210">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="88c70-211">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88c70-211">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="88c70-212">Stosowanie zasad do Razor Pages</span><span class="sxs-lookup"><span data-stu-id="88c70-212">Applying policies to Razor Pages</span></span>

<span data-ttu-id="88c70-213">Zasady są stosowane do Razor Pages przy użyciu atrybutu `[Authorize]` z nazwą zasad.</span><span class="sxs-lookup"><span data-stu-id="88c70-213">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="88c70-214">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88c70-214">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="88c70-215">Zasady mogą być również stosowane do Razor Pages przy użyciu [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="88c70-215">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="88c70-216">Wymagania</span><span class="sxs-lookup"><span data-stu-id="88c70-216">Requirements</span></span>

<span data-ttu-id="88c70-217">Wymaganie autoryzacji to zbiór parametrów danych, których zasady mogą użyć do oszacowania bieżącego podmiotu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="88c70-217">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="88c70-218">W naszych zasadach "AtLeast21" wymagany jest jeden parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="88c70-218">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="88c70-219">Wymaganie implementuje [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest pustym interfejsem znacznika.</span><span class="sxs-lookup"><span data-stu-id="88c70-219">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="88c70-220">Minimalny wymóg sparametryzowanego wieku można zaimplementować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="88c70-220">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="88c70-221">Jeśli zasady autoryzacji zawierają wiele wymagań autoryzacji, wszystkie wymagania muszą zostać spełnione, aby Ocena zasad powiodła się.</span><span class="sxs-lookup"><span data-stu-id="88c70-221">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="88c70-222">Innymi słowy, wiele wymagań autoryzacji dodanych do pojedynczych zasad autoryzacji jest traktowanych **na zasadzie.**</span><span class="sxs-lookup"><span data-stu-id="88c70-222">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="88c70-223">Wymagania nie muszą mieć danych ani właściwości.</span><span class="sxs-lookup"><span data-stu-id="88c70-223">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="88c70-224">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="88c70-224">Authorization handlers</span></span>

<span data-ttu-id="88c70-225">Procedura obsługi autoryzacji jest odpowiedzialna za ocenę właściwości wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-225">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="88c70-226">Procedura obsługi autoryzacji szacuje wymagania w odniesieniu do podanej [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) w celu określenia, czy dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="88c70-226">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="88c70-227">Wymaganie może mieć [wiele programów obsługi](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="88c70-227">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="88c70-228">Program obsługi może odziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymaganiem do obsługi.</span><span class="sxs-lookup"><span data-stu-id="88c70-228">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="88c70-229">Alternatywnie program obsługi może zaimplementować [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , aby obsłużyć więcej niż jeden typ wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-229">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="88c70-230">Użyj procedury obsługi dla jednego wymagania</span><span class="sxs-lookup"><span data-stu-id="88c70-230">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="88c70-231">Poniżej znajduje się przykład relacji jeden-do-jednego, w której program obsługi minimalnych okresów używa jednego wymagania:</span><span class="sxs-lookup"><span data-stu-id="88c70-231">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="88c70-232">Poprzedni kod określa, czy bieżący podmiot użytkownika ma datę żądania urodzenia, który został wystawiony przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="88c70-232">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="88c70-233">Autoryzacja nie może wystąpić w przypadku braku żądania. w takim przypadku zwracane jest zadanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="88c70-233">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="88c70-234">W przypadku wystąpienia zgłoszenia jest naliczany wiek użytkownika.</span><span class="sxs-lookup"><span data-stu-id="88c70-234">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="88c70-235">Jeśli użytkownik spełni minimalny wiek zdefiniowany przez to wymaganie, autoryzacja zostanie uznana za pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="88c70-235">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="88c70-236">Po pomyślnym uwierzytelnieniu `context.Succeed` jest wywoływana z wymaganym parametrem.</span><span class="sxs-lookup"><span data-stu-id="88c70-236">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="88c70-237">Korzystanie z programu obsługi dla wielu wymagań</span><span class="sxs-lookup"><span data-stu-id="88c70-237">Use a handler for multiple requirements</span></span>

<span data-ttu-id="88c70-238">Poniżej znajduje się przykład relacji jeden-do-wielu, w której program obsługi uprawnień może obsłużyć trzy różne typy wymagań:</span><span class="sxs-lookup"><span data-stu-id="88c70-238">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="88c70-239">Poprzedni kod przechodzi [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwości zawierającej wymagania, które nie są oznaczone jako pomyślne.</span><span class="sxs-lookup"><span data-stu-id="88c70-239">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="88c70-240">W przypadku wymaganego `ReadPermission` użytkownik musi być właścicielem lub sponsorem, aby uzyskać dostęp do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="88c70-240">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="88c70-241">W przypadku `EditPermission` lub `DeletePermission` wymaganie musi być właścicielem, aby uzyskać dostęp do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="88c70-241">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="88c70-242">Rejestracja procedury obsługi</span><span class="sxs-lookup"><span data-stu-id="88c70-242">Handler registration</span></span>

<span data-ttu-id="88c70-243">Procedury obsługi są rejestrowane w kolekcji usług podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="88c70-243">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="88c70-244">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88c70-244">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="88c70-245">Poprzedni kod rejestruje `MinimumAgeHandler` jako pojedyncze przez wywoływanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="88c70-245">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="88c70-246">Programy obsługi można zarejestrować przy użyciu dowolnego z wbudowanych [okresów istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="88c70-246">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="88c70-247">Co ma zwrócić program obsługi?</span><span class="sxs-lookup"><span data-stu-id="88c70-247">What should a handler return?</span></span>

<span data-ttu-id="88c70-248">Należy zauważyć, że metoda `Handle` w [przykładzie procedury obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="88c70-248">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="88c70-249">Jak jest wskazywany stan sukcesu lub niepowodzenia?</span><span class="sxs-lookup"><span data-stu-id="88c70-249">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="88c70-250">Procedura obsługi wskazuje powodzenie przez wywołanie `context.Succeed(IAuthorizationRequirement requirement)`, przekazanie pomyślnie zweryfikowanego wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-250">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="88c70-251">Program obsługi nie musi ogólnie obsługiwać błędów, ponieważ inne procedury obsługi tego samego wymagania mogą się powieść.</span><span class="sxs-lookup"><span data-stu-id="88c70-251">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="88c70-252">Aby zagwarantować awarię, nawet jeśli inne programy obsługi wymagań zakończą się powodzeniem, wywołaj `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="88c70-252">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="88c70-253">Jeśli program obsługi wywołuje `context.Succeed` lub `context.Fail`, wszystkie pozostałe procedury obsługi nadal są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="88c70-253">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="88c70-254">Pozwala to na spełnienie wymagań związanych z generowaniem efektów ubocznych, takich jak rejestrowanie, które odbywa się nawet w przypadku pomyślnej weryfikacji lub niepowodzenia przez inną procedurę obsługi.</span><span class="sxs-lookup"><span data-stu-id="88c70-254">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="88c70-255">Po ustawieniu na `false`Właściwość [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (dostępna w ASP.NET Core 1,1 i nowszych) skraca obwody wykonywanie programów obsługi po wywołaniu `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="88c70-255">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="88c70-256">`InvokeHandlersAfterFailure` domyślne do `true`, w którym przypadku są wywoływane wszystkie programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="88c70-256">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="88c70-257">Procedury obsługi autoryzacji są wywoływane, nawet jeśli uwierzytelnianie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="88c70-257">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="88c70-258">Dlaczego chcesz mieć wiele programów obsługi wymagań?</span><span class="sxs-lookup"><span data-stu-id="88c70-258">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="88c70-259">W przypadkach, w których Ocena ma być przeprowadzana na podstawie **lub** , należy zaimplementować wiele programów obsługi dla jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="88c70-259">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="88c70-260">Na przykład firma Microsoft ma drzwi, które są otwierane tylko za pomocą kart kluczowych.</span><span class="sxs-lookup"><span data-stu-id="88c70-260">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="88c70-261">Jeśli opuścisz kartę kluczową w domu, recepcjonista drukuje tymczasowy naklejkę i otwiera drzwiczki.</span><span class="sxs-lookup"><span data-stu-id="88c70-261">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="88c70-262">W tym scenariuszu istnieje jedno wymaganie, *BuildingEntry*, ale wiele programów obsługi, każdy z nich bada pojedyncze wymaganie.</span><span class="sxs-lookup"><span data-stu-id="88c70-262">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="88c70-263">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="88c70-263">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="88c70-264">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="88c70-264">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="88c70-265">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="88c70-265">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="88c70-266">Upewnij się, że oba programy obsługi są [zarejestrowane](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="88c70-266">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="88c70-267">Jeśli jedna z programów obsługi powiedzie się, gdy zasady oceniają `BuildingEntryRequirement`, Ocena zasad powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="88c70-267">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="88c70-268">Korzystanie z funkcji Func do realizacji zasad</span><span class="sxs-lookup"><span data-stu-id="88c70-268">Using a func to fulfill a policy</span></span>

<span data-ttu-id="88c70-269">Mogą wystąpić sytuacje, w których spełnienie zasad jest proste do wyrażania w kodzie.</span><span class="sxs-lookup"><span data-stu-id="88c70-269">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="88c70-270">Podczas konfigurowania zasad przy użyciu konstruktora zasad `RequireAssertion` można podać `Func<AuthorizationHandlerContext, bool>`.</span><span class="sxs-lookup"><span data-stu-id="88c70-270">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="88c70-271">Na przykład poprzednie `BadgeEntryHandler` można napisać ponownie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="88c70-271">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="88c70-272">Uzyskiwanie dostępu do kontekstu żądania MVC w programach obsługi</span><span class="sxs-lookup"><span data-stu-id="88c70-272">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="88c70-273">Metoda `HandleRequirementAsync` zaimplementowana w procedurze obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement`, które są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="88c70-273">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="88c70-274">Struktury, takie jak MVC lub Jabbr, są bezpłatne, aby dodać dowolny obiekt do właściwości `Resource` na `AuthorizationHandlerContext` w celu przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="88c70-274">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="88c70-275">Na przykład, MVC przekazuje wystąpienie elementu [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) we właściwości `Resource`.</span><span class="sxs-lookup"><span data-stu-id="88c70-275">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="88c70-276">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`i wszystkich innych danych udostępnianych przez MVC i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="88c70-276">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="88c70-277">Użycie właściwości `Resource` jest specyficzne dla platformy.</span><span class="sxs-lookup"><span data-stu-id="88c70-277">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="88c70-278">Użycie informacji w `Resource` Właściwość ogranicza zasady autoryzacji do określonych struktur.</span><span class="sxs-lookup"><span data-stu-id="88c70-278">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="88c70-279">Należy rzutować Właściwość `Resource` za pomocą słowa kluczowego `is`, a następnie upewnić się, że rzutowanie zakończyło się pomyślnie, aby upewnić się, że kod nie ulegnie awarii z `InvalidCastException` w przypadku uruchamiania w innych strukturach:</span><span class="sxs-lookup"><span data-stu-id="88c70-279">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end