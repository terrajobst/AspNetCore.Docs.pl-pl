---
title: Autoryzacja oparta na zasadach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć i używać obsługi zasad autoryzacji do wymuszania wymagań autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: c2bc626b2dd341dda878a151def6b405884357d7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665405"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="2ebb0-103">Autoryzacja oparta na zasadach w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ebb0-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="2ebb0-104">Wewnętrznie [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) wymaganie, obsługi wymagań i wstępnie skonfigurowanymi zasadami.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="2ebb0-105">Te bloki konstrukcyjne obsługi wyrażenie oceny autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="2ebb0-106">Wynik jest strukturą autoryzacji bardziej rozbudowane, wielokrotnego użytku, sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="2ebb0-107">Zasady autoryzacji składa się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="2ebb0-108">Jest on zarejestrowany jako część konfiguracji usługi autoryzacji w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="2ebb0-109">W poprzednim przykładzie tworzona jest zasada "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="2ebb0-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="2ebb0-110">Ma ona pojedynczy wymaganie&mdash;z minimalnym wieku, który jest dostarczany jako parametr do wymagań.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="2ebb0-111">Zasady są stosowane przy użyciu `[Authorize]` atrybutu o nazwie zasady.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="2ebb0-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="2ebb0-113">Wymagania</span><span class="sxs-lookup"><span data-stu-id="2ebb0-113">Requirements</span></span>

<span data-ttu-id="2ebb0-114">Wymóg autoryzacji jest to zbiór parametrów danych, które zasady służą do oceny, bieżący podmiot zabezpieczeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="2ebb0-115">W naszych zasadach "AtLeast21" wymagane jest pojedynczy parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="2ebb0-116">Implementuje wymagane [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest interfejsem pustego znacznika.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="2ebb0-117">Wymaganie sparametryzowane minimalny wiek może być wdrażany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="2ebb0-118">Jeśli zasady autoryzacji zawiera wiele wymagań autoryzacji, wszystkie wymagania należy przekazać w kolejności do oceny zasad powiodło się.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="2ebb0-119">Innymi słowy, wiele wymagań autoryzacji, dodać do zasad autoryzacji jednego są traktowane w **i** podstawy.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="2ebb0-120">Wymaganie nie musi być danych lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="2ebb0-121">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="2ebb0-121">Authorization handlers</span></span>

<span data-ttu-id="2ebb0-122">Do obsługi autoryzacji jest odpowiedzialny za oceny właściwości to wymagane.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="2ebb0-123">Program obsługi autoryzacji ocenia wymagania względem podanego [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) ustalenie, jeśli dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="2ebb0-124">Wymagania mogą mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="2ebb0-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="2ebb0-125">Program obsługi może dziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymagane do obsługi.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="2ebb0-126">Alternatywnie program obsługi może wdrożyć [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) do obsługi więcej niż jeden typ wymagania.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="2ebb0-127">Program obsługi na użytek jedno wymaganie dotyczące</span><span class="sxs-lookup"><span data-stu-id="2ebb0-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="2ebb0-128">Oto przykład relacja jeden do jednego, w którym program obsługi minimalny wiek korzysta z jednego wymagania:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="2ebb0-129">Powyższy kod określa, czy bieżący użytkownik podmiotu zabezpieczeń ma datę urodzenia oświadczenia, który został wystawiony przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="2ebb0-130">Autoryzacji nie może wystąpić, jeśli brakuje oświadczenie, w którym to przypadku jest zwracany ukończonego zadania.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="2ebb0-131">Jeśli występuje oświadczenie wieku użytkownika jest obliczana.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="2ebb0-132">Jeśli użytkownik spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacja uważa, że pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="2ebb0-133">Gdy autoryzacja zakończy się pomyślnie, `context.Succeed` jest wywoływana z spełnione wymaganie, jako jedyny parametr.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="2ebb0-134">Program obsługi na użytek wiele wymagań</span><span class="sxs-lookup"><span data-stu-id="2ebb0-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="2ebb0-135">Oto przykład relacji jeden do wielu, w którym program obsługi uprawnienia może obsłużyć trzy różne rodzaje wymagania:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="2ebb0-136">Powyższy kod przechodzi przez [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwość nie zawiera wymagania oznaczona jako pomyślne.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="2ebb0-137">Aby uzyskać `ReadPermission` wymagań, użytkownik musi być właścicielem lub sponsora, dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="2ebb0-138">W przypadku właściwości `EditPermission` lub `DeletePermission` wymaganie dany użytkownik, musisz być właścicielem dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="2ebb0-139">Rejestracja programu obsługi</span><span class="sxs-lookup"><span data-stu-id="2ebb0-139">Handler registration</span></span>

<span data-ttu-id="2ebb0-140">Programy obsługi są rejestrowane w kolekcji usługi podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="2ebb0-141">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="2ebb0-142">Powyższy kod rejestruje `MinimumAgeHandler` jako pojedyncze wywołanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-142">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="2ebb0-143">Programy obsługi można zarejestrować przy użyciu dowolnej z wbudowanych [usługi okresy istnienia](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="2ebb0-143">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="2ebb0-144">Co powinna zwracać program obsługi?</span><span class="sxs-lookup"><span data-stu-id="2ebb0-144">What should a handler return?</span></span>

<span data-ttu-id="2ebb0-145">Należy pamiętać, że `Handle` method in Class metoda [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-145">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="2ebb0-146">Jaki jest stan powodzenia lub niepowodzenia wskazane?</span><span class="sxs-lookup"><span data-stu-id="2ebb0-146">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="2ebb0-147">Program obsługi wskazuje wynik, wywołując `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, który został pomyślnie zweryfikowany.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-147">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="2ebb0-148">Program obsługi nie musi obsługiwać błędy ogólnie rzecz biorąc, zgodnie z innych programów obsługi, aby uzyskać te same wymagania może się powieść.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-148">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="2ebb0-149">Aby zagwarantować awarii, nawet wtedy, gdy powiedzie się w innych programach obsługi wymagań, należy wywołać `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-149">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="2ebb0-150">Po ustawieniu `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) właściwości (dostępny w programie ASP.NET Core 1.1 i nowszych) short-circuits wykonywania programów obsługi podczas `context.Fail` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-150">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="2ebb0-151">`InvokeHandlersAfterFailure` Wartość domyślna to `true`, w którym to przypadku wszystkie wywołania.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-151">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="2ebb0-152">Dzięki temu wymagania wygenerować ubocznych, takich jak rejestrowanie, które zawsze mają miejsce nawet wtedy, gdy `context.Fail` została wywołana w innego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-152">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="2ebb0-153">Dlaczego należy wielu obsług dla wymagania?</span><span class="sxs-lookup"><span data-stu-id="2ebb0-153">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="2ebb0-154">W przypadkach, w której chcesz oceny na **lub** naliczana wdrożenia wielu obsług dla pojedynczego wymagania.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-154">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="2ebb0-155">Na przykład firma Microsoft ma drzwi otwierać tylko przy użyciu kluczy kart.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-155">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="2ebb0-156">Jeśli Twoja karta klucza pozostanie w domu, recepcjonista drukuje tymczasowe nalepkę i otwiera drzwi biblioteki.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-156">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="2ebb0-157">W tym scenariuszu miałby jednego wymagania, *BuildingEntry*, ale wielu obsług, każdy z nich badanie jedno zapotrzebowanie.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-157">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="2ebb0-158">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="2ebb0-158">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="2ebb0-159">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="2ebb0-159">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="2ebb0-160">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="2ebb0-160">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="2ebb0-161">Upewnij się, że oba programy obsługi [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="2ebb0-161">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="2ebb0-162">W przypadku obu obsługi zakończy się pomyślnie, gdy zasady oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-162">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="2ebb0-163">Aby spełnić zasady przy użyciu func</span><span class="sxs-lookup"><span data-stu-id="2ebb0-163">Using a func to fulfill a policy</span></span>

<span data-ttu-id="2ebb0-164">Mogą wystąpić sytuacje, w których wypełniając zasad jest proste wyrażenia w kodzie.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-164">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="2ebb0-165">Jest to możliwe, aby podać `Func<AuthorizationHandlerContext, bool>` podczas konfigurowania zasad `RequireAssertion` konstruktora zasad.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-165">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="2ebb0-166">Na przykład poprzedniej `BadgeEntryHandler` można dopasować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-166">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="2ebb0-167">Uzyskiwanie dostępu do kontekstu żądania MVC w procedurach obsługi</span><span class="sxs-lookup"><span data-stu-id="2ebb0-167">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="2ebb0-168">`HandleRequirementAsync` Metody wdrożenia w obsłudze autoryzacji zawiera dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-168">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="2ebb0-169">Struktur, takich jak MVC lub Jabbr są bezpłatne dowolny obiekt, aby dodać `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-169">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="2ebb0-170">Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-170">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="2ebb0-171">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko inne podany, MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-171">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="2ebb0-172">Korzystanie z `Resource` właściwość to struktura określone.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-172">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="2ebb0-173">Korzystając z informacji w `Resource` właściwość ogranicza zasad autoryzacji do określonej struktury.</span><span class="sxs-lookup"><span data-stu-id="2ebb0-173">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="2ebb0-174">Należy rzutować `Resource` właściwość za pomocą `is` słowo kluczowe, a następnie potwierdź rzutowanie zakończyła się pomyślnie, aby upewnić się, kod nie awarii przy użyciu `InvalidCastException` uruchamiania innych platform:</span><span class="sxs-lookup"><span data-stu-id="2ebb0-174">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
