---
title: Autoryzacja oparta na zasadach w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak tworzyć i używać programów obsługi zasad autoryzacji w celu wymuszania wymagań autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 60625944d4ba31da6b98bdf947991088dc75ed87
ms.sourcegitcommit: 7001657c00358b082734ba4273693b9b3ed35d2a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68669968"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="c1447-103">Autoryzacja oparta na zasadach w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1447-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c1447-104">Zgodnie z założeniami, [Autoryzacja oparta na rolach](xref:security/authorization/roles) i Autoryzacja oparta na [oświadczeniach](xref:security/authorization/claims) używają wymagań, obsługi wymagań i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="c1447-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="c1447-105">Te bloki konstrukcyjne obsługują wyrażenie oceny autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c1447-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="c1447-106">Wynikiem jest rozbudowana struktura autoryzacji do wielokrotnego użytku weryfikowalne.</span><span class="sxs-lookup"><span data-stu-id="c1447-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="c1447-107">Zasady autoryzacji składają się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="c1447-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="c1447-108">Jest ona zarejestrowana w ramach konfiguracji usługi autoryzacji w `Startup.ConfigureServices` ramach metody:</span><span class="sxs-lookup"><span data-stu-id="c1447-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="c1447-109">W poprzednim przykładzie tworzone są zasady "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="c1447-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="c1447-110">Ma jedno wymaganie&mdash;o minimalnym wieku, które jest dostarczane jako parametr do wymagania.</span><span class="sxs-lookup"><span data-stu-id="c1447-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="c1447-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="c1447-111">IAuthorizationService</span></span> 

<span data-ttu-id="c1447-112">Podstawowa usługa, która określa, czy autoryzacja powiodła się <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="c1447-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="c1447-113">Poprzedni kod wyróżnia dwie metody [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="c1447-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="c1447-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>to usługa znacznika bez metod i mechanizm śledzenia, czy autoryzacja zakończyła się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="c1447-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="c1447-115">Każdy <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> jest odpowiedzialny za sprawdzenie, czy są spełnione wymagania:</span><span class="sxs-lookup"><span data-stu-id="c1447-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
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

<span data-ttu-id="c1447-116"><xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> Klasa jest wykorzystywana przez program obsługi do oznaczania, czy zostały spełnione wymagania:</span><span class="sxs-lookup"><span data-stu-id="c1447-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="c1447-117">Poniższy kod przedstawia uproszczoną (i adnotację z komentarzami) domyślną implementację usługi autoryzacji:</span><span class="sxs-lookup"><span data-stu-id="c1447-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="c1447-118">Poniższy kod przedstawia typowy `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c1447-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="c1447-119">Użyj <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> lub`[Authorize(Policy = "Something")]` do autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c1447-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="c1447-120">Stosowanie zasad do kontrolerów MVC</span><span class="sxs-lookup"><span data-stu-id="c1447-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="c1447-121">Jeśli używasz Razor Pages, zobacz [stosowanie zasad do Razor Pages](#applying-policies-to-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="c1447-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="c1447-122">Zasady są stosowane do kontrolerów przy użyciu `[Authorize]` atrybutu z nazwą zasad.</span><span class="sxs-lookup"><span data-stu-id="c1447-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1447-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c1447-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="c1447-124">Stosowanie zasad do Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c1447-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="c1447-125">Zasady są stosowane do Razor Pages przy użyciu `[Authorize]` atrybutu z nazwą zasad.</span><span class="sxs-lookup"><span data-stu-id="c1447-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1447-126">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c1447-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="c1447-127">Zasady mogą być również stosowane do Razor Pages przy użyciu [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="c1447-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="c1447-128">Wymagania</span><span class="sxs-lookup"><span data-stu-id="c1447-128">Requirements</span></span>

<span data-ttu-id="c1447-129">Wymaganie autoryzacji to zbiór parametrów danych, których zasady mogą użyć do oszacowania bieżącego podmiotu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c1447-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="c1447-130">W naszych zasadach "AtLeast21" wymagany jest jeden parametr&mdash;o minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="c1447-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="c1447-131">Wymaganie implementuje [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest pustym interfejsem znacznika.</span><span class="sxs-lookup"><span data-stu-id="c1447-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="c1447-132">Minimalny wymóg sparametryzowanego wieku można zaimplementować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c1447-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="c1447-133">Jeśli zasady autoryzacji zawierają wiele wymagań autoryzacji, wszystkie wymagania muszą zostać spełnione, aby Ocena zasad powiodła się.</span><span class="sxs-lookup"><span data-stu-id="c1447-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="c1447-134">Innymi słowy, wiele wymagań autoryzacji dodanych do pojedynczych zasad autoryzacji jest traktowanych **na zasadzie.**</span><span class="sxs-lookup"><span data-stu-id="c1447-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="c1447-135">Wymagania nie muszą mieć danych ani właściwości.</span><span class="sxs-lookup"><span data-stu-id="c1447-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="c1447-136">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="c1447-136">Authorization handlers</span></span>

<span data-ttu-id="c1447-137">Procedura obsługi autoryzacji jest odpowiedzialna za ocenę właściwości wymagania.</span><span class="sxs-lookup"><span data-stu-id="c1447-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="c1447-138">Procedura obsługi autoryzacji szacuje wymagania w odniesieniu do podanej [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) w celu określenia, czy dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="c1447-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="c1447-139">Wymaganie może mieć [wiele programów obsługi](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="c1447-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="c1447-140">Program obsługi może odziedziczyć [\<AuthorizationHandler TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), `TRequirement` gdzie to wymaganie do obsługi.</span><span class="sxs-lookup"><span data-stu-id="c1447-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="c1447-141">Alternatywnie program obsługi może zaimplementować [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , aby obsłużyć więcej niż jeden typ wymagania.</span><span class="sxs-lookup"><span data-stu-id="c1447-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="c1447-142">Użyj procedury obsługi dla jednego wymagania</span><span class="sxs-lookup"><span data-stu-id="c1447-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="c1447-143">Poniżej znajduje się przykład relacji jeden-do-jednego, w której program obsługi minimalnych okresów używa jednego wymagania:</span><span class="sxs-lookup"><span data-stu-id="c1447-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="c1447-144">Poprzedni kod określa, czy bieżący podmiot użytkownika ma datę żądania urodzenia, który został wystawiony przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="c1447-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="c1447-145">Autoryzacja nie może wystąpić w przypadku braku żądania. w takim przypadku zwracane jest zadanie wykonane.</span><span class="sxs-lookup"><span data-stu-id="c1447-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="c1447-146">W przypadku wystąpienia zgłoszenia jest naliczany wiek użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c1447-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="c1447-147">Jeśli użytkownik spełni minimalny wiek zdefiniowany przez to wymaganie, autoryzacja zostanie uznana za pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="c1447-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="c1447-148">Gdy autoryzacja powiedzie `context.Succeed` się, jest wywoływana z wymaganym parametrem.</span><span class="sxs-lookup"><span data-stu-id="c1447-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="c1447-149">Korzystanie z programu obsługi dla wielu wymagań</span><span class="sxs-lookup"><span data-stu-id="c1447-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="c1447-150">Poniżej znajduje się przykład relacji jeden-do-wielu, w której program obsługi uprawnień może obsłużyć trzy różne typy wymagań:</span><span class="sxs-lookup"><span data-stu-id="c1447-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="c1447-151">Poprzedni kod przechodzi przez [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwości zawierającej wymagania, które nie są oznaczone jako pomyślne.</span><span class="sxs-lookup"><span data-stu-id="c1447-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="c1447-152">`ReadPermission` Aby uzyskać dostęp do żądanego zasobu, użytkownik musi być właścicielem lub sponsorem.</span><span class="sxs-lookup"><span data-stu-id="c1447-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="c1447-153">W przypadku `EditPermission` lub `DeletePermission` wymagania musi być właścicielem, aby uzyskać dostęp do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="c1447-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="c1447-154">Rejestracja procedury obsługi</span><span class="sxs-lookup"><span data-stu-id="c1447-154">Handler registration</span></span>

<span data-ttu-id="c1447-155">Procedury obsługi są rejestrowane w kolekcji usług podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c1447-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="c1447-156">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c1447-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="c1447-157">Poprzedni kod rejestruje `MinimumAgeHandler` jako pojedyncze przez wywołanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="c1447-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="c1447-158">Programy obsługi można zarejestrować przy użyciu dowolnego z wbudowanych [okresów istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="c1447-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="c1447-159">Co ma zwrócić program obsługi?</span><span class="sxs-lookup"><span data-stu-id="c1447-159">What should a handler return?</span></span>

<span data-ttu-id="c1447-160">Należy zauważyć, `Handle` że metoda w [przykładzie procedury obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="c1447-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="c1447-161">Jak jest wskazywany stan sukcesu lub niepowodzenia?</span><span class="sxs-lookup"><span data-stu-id="c1447-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="c1447-162">Procedura obsługi wskazuje powodzenie przez wywołanie `context.Succeed(IAuthorizationRequirement requirement)`, przekazanie wymagania, które zostało pomyślnie zweryfikowane.</span><span class="sxs-lookup"><span data-stu-id="c1447-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="c1447-163">Program obsługi nie musi ogólnie obsługiwać błędów, ponieważ inne procedury obsługi tego samego wymagania mogą się powieść.</span><span class="sxs-lookup"><span data-stu-id="c1447-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="c1447-164">W celu zagwarantowania niepowodzenia, nawet w przypadku pomyślnego `context.Fail`zajścia innych wymagań.</span><span class="sxs-lookup"><span data-stu-id="c1447-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="c1447-165">Jeśli program obsługi wywołuje `context.Succeed` lub `context.Fail`, wszystkie inne procedury obsługi są nadal wywoływane.</span><span class="sxs-lookup"><span data-stu-id="c1447-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="c1447-166">Pozwala to na spełnienie wymagań związanych z generowaniem efektów ubocznych, takich jak rejestrowanie, które odbywa się nawet w przypadku pomyślnej weryfikacji lub niepowodzenia przez inną procedurę obsługi.</span><span class="sxs-lookup"><span data-stu-id="c1447-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="c1447-167">Po ustawieniu na `false`Właściwość [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (dostępna w ASP.NET Core 1,1 i nowszych) krótkie obwody wykonywania programów obsługi po `context.Fail` wywołaniu.</span><span class="sxs-lookup"><span data-stu-id="c1447-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="c1447-168">`InvokeHandlersAfterFailure`wartość domyślna `true`to, w którym to przypadku są wywoływane wszystkie programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="c1447-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="c1447-169">Procedury obsługi autoryzacji są wywoływane, nawet jeśli uwierzytelnianie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="c1447-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="c1447-170">Dlaczego chcesz mieć wiele programów obsługi wymagań?</span><span class="sxs-lookup"><span data-stu-id="c1447-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="c1447-171">W przypadkach, w których Ocena ma być przeprowadzana na podstawie **lub** , należy zaimplementować wiele programów obsługi dla jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="c1447-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="c1447-172">Na przykład firma Microsoft ma drzwi, które są otwierane tylko za pomocą kart kluczowych.</span><span class="sxs-lookup"><span data-stu-id="c1447-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="c1447-173">Jeśli opuścisz kartę kluczową w domu, recepcjonista drukuje tymczasowy naklejkę i otwiera drzwiczki.</span><span class="sxs-lookup"><span data-stu-id="c1447-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="c1447-174">W tym scenariuszu istnieje jedno wymaganie, *BuildingEntry*, ale wiele programów obsługi, każdy z nich bada pojedyncze wymaganie.</span><span class="sxs-lookup"><span data-stu-id="c1447-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="c1447-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="c1447-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="c1447-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1447-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="c1447-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1447-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="c1447-178">Upewnij się, że oba programy obsługi są [zarejestrowane](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="c1447-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="c1447-179">Jeśli jedna z programów obsługi powiedzie się `BuildingEntryRequirement`, gdy zasady oceniają, Ocena zasad zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="c1447-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="c1447-180">Korzystanie z funkcji Func do realizacji zasad</span><span class="sxs-lookup"><span data-stu-id="c1447-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="c1447-181">Mogą wystąpić sytuacje, w których spełnienie zasad jest proste do wyrażania w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c1447-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="c1447-182">Podczas konfigurowania zasad `RequireAssertion` przy użyciu konstruktora `Func<AuthorizationHandlerContext, bool>` zasad można podać wartość.</span><span class="sxs-lookup"><span data-stu-id="c1447-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="c1447-183">Na przykład poprzedni `BadgeEntryHandler` można napisać ponownie w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c1447-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="c1447-184">Uzyskiwanie dostępu do kontekstu żądania MVC w programach obsługi</span><span class="sxs-lookup"><span data-stu-id="c1447-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="c1447-185">Metoda zaimplementowana w procedurze obsługi autoryzacji ma dwa parametry `AuthorizationHandlerContext` : a i `TRequirement` obsługujące. `HandleRequirementAsync`</span><span class="sxs-lookup"><span data-stu-id="c1447-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="c1447-186">Struktury, takie jak MVC lub Jabbr, są bezpłatne, aby dodać każdy obiekt `Resource` do właściwości `AuthorizationHandlerContext` w celu przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="c1447-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="c1447-187">Na przykład, MVC przekazuje wystąpienie elementu [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) we `Resource` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c1447-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="c1447-188">Ta właściwość zapewnia dostęp do `HttpContext`elementów `RouteData`, i innych udostępnianych przez MVC i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c1447-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="c1447-189">Użycie `Resource` właściwości jest specyficzne dla struktury.</span><span class="sxs-lookup"><span data-stu-id="c1447-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="c1447-190">Użycie informacji we `Resource` właściwości ogranicza zasady autoryzacji do określonych struktur.</span><span class="sxs-lookup"><span data-stu-id="c1447-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="c1447-191">Należy rzutować `Resource` Właściwość `is` za pomocą słowa kluczowego, a następnie potwierdzić, że rzutowanie zakończyło się pomyślnie, aby upewnić się, że `InvalidCastException` kod nie ulegnie awarii z uruchomieniem w innych strukturach:</span><span class="sxs-lookup"><span data-stu-id="c1447-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
