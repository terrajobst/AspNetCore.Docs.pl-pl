---
title: Na podstawie zasad autoryzacji w ASP.NET Core
author: rick-anderson
description: Informacje o sposobie tworzenia i używania programów obsługi zasady autoryzacji dotyczące wymuszania wymagań autoryzacji w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072864"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="bee02-103">Na podstawie zasad autoryzacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bee02-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="bee02-104">Poniżej obejmuje [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) użyć wymaganie, obsługi wymagania i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="bee02-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="bee02-105">Te bloki konstrukcyjne obsługi wyrażenie ocen autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="bee02-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="bee02-106">Wynik jest strukturą bardziej rozbudowane, wielokrotnego użytku, testować autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="bee02-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="bee02-107">Zasady autoryzacji składa się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="bee02-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="bee02-108">Jest on zarejestrowany w ramach konfiguracji usługi autoryzacji, w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="bee02-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="bee02-109">W powyższym przykładzie tworzona jest zasada "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="bee02-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="bee02-110">Składa się z jednego wymaganie&mdash;z minimalnym wieku jest ona podawana jako parametr do wymagań.</span><span class="sxs-lookup"><span data-stu-id="bee02-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="bee02-111">Zasady są stosowane przy użyciu `[Authorize]` atrybutu o nazwie zasad.</span><span class="sxs-lookup"><span data-stu-id="bee02-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="bee02-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bee02-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="bee02-113">Wymagania</span><span class="sxs-lookup"><span data-stu-id="bee02-113">Requirements</span></span>

<span data-ttu-id="bee02-114">Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bee02-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="bee02-115">W zasadach "AtLeast21" to wymaganie jest pojedynczy parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="bee02-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="bee02-116">Implementuje wymagane [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), która jest interfejsem znacznika puste.</span><span class="sxs-lookup"><span data-stu-id="bee02-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="bee02-117">Sparametryzowane minimalny wymagany wiek można realizowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bee02-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="bee02-118">Wymagania nie musi być danych lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="bee02-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="bee02-119">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="bee02-119">Authorization handlers</span></span>

<span data-ttu-id="bee02-120">Program obsługi autoryzacji jest odpowiedzialny za obliczania właściwości zapotrzebowania.</span><span class="sxs-lookup"><span data-stu-id="bee02-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="bee02-121">Program obsługi autoryzacji ocenia wymagania względem podanego [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) ustalenie, czy dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="bee02-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="bee02-122">Wymóg może mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="bee02-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="bee02-123">Program obsługi może dziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymagane do obsługi.</span><span class="sxs-lookup"><span data-stu-id="bee02-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="bee02-124">Alternatywnie program obsługi może wdrożyć [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) obsłużyć więcej niż jeden typ wymaganie.</span><span class="sxs-lookup"><span data-stu-id="bee02-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="bee02-125">Użyj programu obsługi dla jednego wymagania</span><span class="sxs-lookup"><span data-stu-id="bee02-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="bee02-126">Poniżej przedstawiono przykład relacją, w którym program obsługi minimalnego wieku korzysta z jednego wymaganie:</span><span class="sxs-lookup"><span data-stu-id="bee02-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="bee02-127">Poprzedni kod określa, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="bee02-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="bee02-128">Autoryzacji nie może wystąpić w przypadku braku oświadczenia, w którym to przypadku jest zwracany o zakończeniu zadania.</span><span class="sxs-lookup"><span data-stu-id="bee02-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="bee02-129">Gdy występuje oświadczenia są obliczane wieku użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bee02-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="bee02-130">Jeśli użytkownika spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacji uważa się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="bee02-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="bee02-131">Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako jedyny parametr.</span><span class="sxs-lookup"><span data-stu-id="bee02-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="bee02-132">Użyj wiele wymagań dotyczących obsługi</span><span class="sxs-lookup"><span data-stu-id="bee02-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="bee02-133">Poniżej przedstawiono przykład relacji jeden do wielu, w którym program obsługi uprawnienia wykorzystuje trzy wymagania:</span><span class="sxs-lookup"><span data-stu-id="bee02-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="bee02-134">Poprzedni kod przechodzi przez [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;nie zawiera wymagania właściwość oznaczona jako powiodło się.</span><span class="sxs-lookup"><span data-stu-id="bee02-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="bee02-135">Jeśli użytkownik ma uprawnienie do odczytu, użytkownik musi być właścicielem lub sponsor, dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="bee02-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="bee02-136">Jeśli użytkownik ma Edytuj lub usuń uprawnienie, musi być właścicielem dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="bee02-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="bee02-137">Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako jedyny parametr.</span><span class="sxs-lookup"><span data-stu-id="bee02-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="bee02-138">Rejestracja programu obsługi</span><span class="sxs-lookup"><span data-stu-id="bee02-138">Handler registration</span></span>

<span data-ttu-id="bee02-139">Programy obsługi są zarejestrowane w kolekcji usługi podczas konfigurowania.</span><span class="sxs-lookup"><span data-stu-id="bee02-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="bee02-140">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="bee02-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="bee02-141">Każdy program obsługi jest dodawany do kolekcji usług przez wywoływanie `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="bee02-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="bee02-142">Co powinna zwracać obsługi?</span><span class="sxs-lookup"><span data-stu-id="bee02-142">What should a handler return?</span></span>

<span data-ttu-id="bee02-143">Należy pamiętać, że `Handle` metody w [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="bee02-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="bee02-144">Jaki jest stan powodzenia lub niepowodzenia wskazanych?</span><span class="sxs-lookup"><span data-stu-id="bee02-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="bee02-145">Program obsługi oznacza Powodzenie przez wywołanie metody `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, która została pomyślnie zweryfikowana.</span><span class="sxs-lookup"><span data-stu-id="bee02-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="bee02-146">Program obsługi nie musi obsługiwać awarie ogólnie rzecz biorąc, zgodnie z innych programów obsługi na ten sam wymóg może się powieść.</span><span class="sxs-lookup"><span data-stu-id="bee02-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="bee02-147">Aby zagwarantować awarii, nawet w przypadku innych programów obsługi wymagań powiedzie się, należy wywołać `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="bee02-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="bee02-148">Wartość `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) właściwości (dostępne w ASP.NET Core 1.1 lub nowszej) short-circuits wykonywania programów obsługi, gdy `context.Fail` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bee02-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="bee02-149">`InvokeHandlersAfterFailure` Domyślnie `true`, w którym to przypadku wszystkie wywołania.</span><span class="sxs-lookup"><span data-stu-id="bee02-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="bee02-150">Dzięki temu wymagania powodować efekty uboczne, takich jak rejestrowanie, które zawsze mają miejsce nawet wtedy, gdy `context.Fail` została wywołana w innego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="bee02-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="bee02-151">Dlaczego może być wielu obsług dla wymagania?</span><span class="sxs-lookup"><span data-stu-id="bee02-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="bee02-152">W przypadkach, w którym ma oceny na **lub** podstawę, wdrożenie wielu obsług dla pojedynczego wymagania.</span><span class="sxs-lookup"><span data-stu-id="bee02-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="bee02-153">Na przykład firma Microsoft ma drzwi, które mogły być otwierane tylko w przypadku kart klucza.</span><span class="sxs-lookup"><span data-stu-id="bee02-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="bee02-154">Jeśli karta klucza pozostanie w domu, recepcjonista drukuje naklejce tymczasowego i otwiera drzwi.</span><span class="sxs-lookup"><span data-stu-id="bee02-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="bee02-155">W tym scenariuszu należy wymaganiem pojedynczego *BuildingEntry*, ale wielu obsług, każda z nich badanie pojedynczego wymaganie.</span><span class="sxs-lookup"><span data-stu-id="bee02-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="bee02-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="bee02-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="bee02-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="bee02-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="bee02-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="bee02-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="bee02-159">Upewnij się, że oba programy obsługi są [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="bee02-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="bee02-160">Jeśli albo obsługi zakończy się powodzeniem, gdy zasada oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="bee02-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="bee02-161">Przy użyciu func do zrealizowania zasady</span><span class="sxs-lookup"><span data-stu-id="bee02-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="bee02-162">Mogą wystąpić sytuacje, w których spełnienie jest proste express w kodzie zasady.</span><span class="sxs-lookup"><span data-stu-id="bee02-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="bee02-163">Można podać `Func<AuthorizationHandlerContext, bool>` przy konfigurowaniu zasad z `RequireAssertion` konstruktora zasad.</span><span class="sxs-lookup"><span data-stu-id="bee02-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="bee02-164">Na przykład poprzedniej `BadgeEntryHandler` można ponownie zapisać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bee02-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="bee02-165">Uzyskiwanie dostępu do kontekstu żądania MVC w obsłudze</span><span class="sxs-lookup"><span data-stu-id="bee02-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="bee02-166">`HandleRequirementAsync` Metoda implementuje obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi.</span><span class="sxs-lookup"><span data-stu-id="bee02-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="bee02-167">Struktury, np. MVC lub Jabbr są także dodać dowolny obiekt, aby `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="bee02-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="bee02-168">Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości.</span><span class="sxs-lookup"><span data-stu-id="bee02-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="bee02-169">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko else zapewniane przez MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="bee02-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="bee02-170">Korzystanie z `Resource` właściwość jest tylko dla struktury.</span><span class="sxs-lookup"><span data-stu-id="bee02-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="bee02-171">Korzystając z informacji w `Resource` właściwości ogranicza zasad autoryzacji do określonej struktury.</span><span class="sxs-lookup"><span data-stu-id="bee02-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="bee02-172">Należy rzutować `Resource` za pomocą właściwości `as` — słowo kluczowe, a następnie potwierdź rzutowanie ma powiedzie się, aby upewnić się, kod nie awarii z `InvalidCastException` uruchomienia na innych platformach:</span><span class="sxs-lookup"><span data-stu-id="bee02-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
