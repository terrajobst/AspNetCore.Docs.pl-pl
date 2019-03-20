---
title: Autoryzacja oparta na zasadach w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak utworzyć i używać obsługi zasad autoryzacji do wymuszania wymagań autoryzacji w aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: e72f15c28fb7b62c671dd6475cc323cacce42de6
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208324"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="b54e4-103">Autoryzacja oparta na zasadach w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b54e4-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="b54e4-104">Wewnętrznie [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) wymaganie, obsługi wymagań i wstępnie skonfigurowanymi zasadami.</span><span class="sxs-lookup"><span data-stu-id="b54e4-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="b54e4-105">Te bloki konstrukcyjne obsługi wyrażenie oceny autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="b54e4-106">Wynik jest strukturą autoryzacji bardziej rozbudowane, wielokrotnego użytku, sprawdzalnego działa zgodnie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="b54e4-107">Zasady autoryzacji składa się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="b54e4-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="b54e4-108">Jest on zarejestrowany jako część konfiguracji usługi autoryzacji w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="b54e4-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="b54e4-109">W poprzednim przykładzie tworzona jest zasada "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="b54e4-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="b54e4-110">Ma ona pojedynczy wymaganie&mdash;z minimalnym wieku, który jest dostarczany jako parametr do wymagań.</span><span class="sxs-lookup"><span data-stu-id="b54e4-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="b54e4-111">Stosowanie zasad do kontrolerów MVC</span><span class="sxs-lookup"><span data-stu-id="b54e4-111">Applying policies to MVC controllers</span></span>

<span data-ttu-id="b54e4-112">Jeśli używasz stron Razor, zobacz [stosowanie zasad do stron Razor](#applying-policies-to-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-112">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="b54e4-113">Zasady są stosowane do kontrolerów, za pomocą `[Authorize]` atrybutu o nazwie zasady.</span><span class="sxs-lookup"><span data-stu-id="b54e4-113">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="b54e4-114">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b54e4-114">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="b54e4-115">Stosowanie zasad do stron Razor</span><span class="sxs-lookup"><span data-stu-id="b54e4-115">Applying policies to Razor Pages</span></span>

<span data-ttu-id="b54e4-116">Zasady są stosowane do strony Razor za pomocą `[Authorize]` atrybutu o nazwie zasady.</span><span class="sxs-lookup"><span data-stu-id="b54e4-116">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="b54e4-117">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b54e4-117">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="b54e4-118">Zasady mogą również będą stosowane do strony Razor za pomocą [Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="b54e4-118">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="b54e4-119">Wymagania</span><span class="sxs-lookup"><span data-stu-id="b54e4-119">Requirements</span></span>

<span data-ttu-id="b54e4-120">Wymóg autoryzacji jest to zbiór parametrów danych, które zasady służą do oceny, bieżący podmiot zabezpieczeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="b54e4-120">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="b54e4-121">W naszych zasadach "AtLeast21" wymagane jest pojedynczy parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="b54e4-121">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="b54e4-122">Implementuje wymagane [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), który jest interfejsem pustego znacznika.</span><span class="sxs-lookup"><span data-stu-id="b54e4-122">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="b54e4-123">Wymaganie sparametryzowane minimalny wiek może być wdrażany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b54e4-123">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="b54e4-124">Jeśli zasady autoryzacji zawiera wiele wymagań autoryzacji, wszystkie wymagania należy przekazać w kolejności do oceny zasad powiodło się.</span><span class="sxs-lookup"><span data-stu-id="b54e4-124">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="b54e4-125">Innymi słowy, wiele wymagań autoryzacji, dodać do zasad autoryzacji jednego są traktowane w **i** podstawy.</span><span class="sxs-lookup"><span data-stu-id="b54e4-125">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="b54e4-126">Wymaganie nie musi być danych lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="b54e4-126">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="b54e4-127">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="b54e4-127">Authorization handlers</span></span>

<span data-ttu-id="b54e4-128">Do obsługi autoryzacji jest odpowiedzialny za oceny właściwości to wymagane.</span><span class="sxs-lookup"><span data-stu-id="b54e4-128">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="b54e4-129">Program obsługi autoryzacji ocenia wymagania względem podanego [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) ustalenie, jeśli dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="b54e4-129">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="b54e4-130">Wymagania mogą mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="b54e4-130">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="b54e4-131">Program obsługi może dziedziczyć [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), gdzie `TRequirement` jest wymagane do obsługi.</span><span class="sxs-lookup"><span data-stu-id="b54e4-131">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="b54e4-132">Alternatywnie program obsługi może wdrożyć [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) do obsługi więcej niż jeden typ wymagania.</span><span class="sxs-lookup"><span data-stu-id="b54e4-132">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="b54e4-133">Program obsługi na użytek jedno wymaganie dotyczące</span><span class="sxs-lookup"><span data-stu-id="b54e4-133">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="b54e4-134">Oto przykład relacja jeden do jednego, w którym program obsługi minimalny wiek korzysta z jednego wymagania:</span><span class="sxs-lookup"><span data-stu-id="b54e4-134">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="b54e4-135">Powyższy kod określa, czy bieżący użytkownik podmiotu zabezpieczeń ma datę urodzenia oświadczenia, który został wystawiony przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="b54e4-135">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="b54e4-136">Autoryzacji nie może wystąpić, jeśli brakuje oświadczenie, w którym to przypadku jest zwracany ukończonego zadania.</span><span class="sxs-lookup"><span data-stu-id="b54e4-136">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="b54e4-137">Jeśli występuje oświadczenie wieku użytkownika jest obliczana.</span><span class="sxs-lookup"><span data-stu-id="b54e4-137">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="b54e4-138">Jeśli użytkownik spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacja uważa, że pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-138">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="b54e4-139">Gdy autoryzacja zakończy się pomyślnie, `context.Succeed` jest wywoływana z spełnione wymaganie, jako jedyny parametr.</span><span class="sxs-lookup"><span data-stu-id="b54e4-139">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="b54e4-140">Program obsługi na użytek wiele wymagań</span><span class="sxs-lookup"><span data-stu-id="b54e4-140">Use a handler for multiple requirements</span></span>

<span data-ttu-id="b54e4-141">Oto przykład relacji jeden do wielu, w którym program obsługi uprawnienia może obsłużyć trzy różne rodzaje wymagania:</span><span class="sxs-lookup"><span data-stu-id="b54e4-141">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="b54e4-142">Powyższy kod przechodzi przez [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;właściwość nie zawiera wymagania oznaczona jako pomyślne.</span><span class="sxs-lookup"><span data-stu-id="b54e4-142">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="b54e4-143">Aby uzyskać `ReadPermission` wymagań, użytkownik musi być właścicielem lub sponsora, dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="b54e4-143">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="b54e4-144">W przypadku właściwości `EditPermission` lub `DeletePermission` wymaganie dany użytkownik, musisz być właścicielem dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="b54e4-144">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="b54e4-145">Rejestracja programu obsługi</span><span class="sxs-lookup"><span data-stu-id="b54e4-145">Handler registration</span></span>

<span data-ttu-id="b54e4-146">Programy obsługi są rejestrowane w kolekcji usługi podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="b54e4-146">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="b54e4-147">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b54e4-147">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="b54e4-148">Powyższy kod rejestruje `MinimumAgeHandler` jako pojedyncze wywołanie `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="b54e4-148">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="b54e4-149">Programy obsługi można zarejestrować przy użyciu dowolnej z wbudowanych [usługi okresy istnienia](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="b54e4-149">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="b54e4-150">Co powinna zwracać program obsługi?</span><span class="sxs-lookup"><span data-stu-id="b54e4-150">What should a handler return?</span></span>

<span data-ttu-id="b54e4-151">Należy pamiętać, że `Handle` method in Class metoda [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="b54e4-151">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="b54e4-152">Jaki jest stan powodzenia lub niepowodzenia wskazane?</span><span class="sxs-lookup"><span data-stu-id="b54e4-152">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="b54e4-153">Program obsługi wskazuje wynik, wywołując `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, który został pomyślnie zweryfikowany.</span><span class="sxs-lookup"><span data-stu-id="b54e4-153">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="b54e4-154">Program obsługi nie musi obsługiwać błędy ogólnie rzecz biorąc, zgodnie z innych programów obsługi, aby uzyskać te same wymagania może się powieść.</span><span class="sxs-lookup"><span data-stu-id="b54e4-154">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="b54e4-155">Aby zagwarantować awarii, nawet wtedy, gdy powiedzie się w innych programach obsługi wymagań, należy wywołać `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="b54e4-155">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="b54e4-156">Po ustawieniu `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) właściwości (dostępny w programie ASP.NET Core 1.1 i nowszych) short-circuits wykonywania programów obsługi podczas `context.Fail` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="b54e4-156">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="b54e4-157">`InvokeHandlersAfterFailure` Wartość domyślna to `true`, w którym to przypadku wszystkie wywołania.</span><span class="sxs-lookup"><span data-stu-id="b54e4-157">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="b54e4-158">Dzięki temu wymagania wygenerować ubocznych, takich jak rejestrowanie, które zawsze mają miejsce nawet wtedy, gdy `context.Fail` została wywołana w innego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="b54e4-158">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="b54e4-159">Dlaczego należy wielu obsług dla wymagania?</span><span class="sxs-lookup"><span data-stu-id="b54e4-159">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="b54e4-160">W przypadkach, w której chcesz oceny na **lub** naliczana wdrożenia wielu obsług dla pojedynczego wymagania.</span><span class="sxs-lookup"><span data-stu-id="b54e4-160">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="b54e4-161">Na przykład firma Microsoft ma drzwi otwierać tylko przy użyciu kluczy kart.</span><span class="sxs-lookup"><span data-stu-id="b54e4-161">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="b54e4-162">Jeśli Twoja karta klucza pozostanie w domu, recepcjonista drukuje tymczasowe nalepkę i otwiera drzwi biblioteki.</span><span class="sxs-lookup"><span data-stu-id="b54e4-162">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="b54e4-163">W tym scenariuszu miałby jednego wymagania, *BuildingEntry*, ale wielu obsług, każdy z nich badanie jedno zapotrzebowanie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-163">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="b54e4-164">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="b54e4-164">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="b54e4-165">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="b54e4-165">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="b54e4-166">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="b54e4-166">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="b54e4-167">Upewnij się, że oba programy obsługi [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="b54e4-167">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="b54e4-168">W przypadku obu obsługi zakończy się pomyślnie, gdy zasady oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-168">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="b54e4-169">Aby spełnić zasady przy użyciu func</span><span class="sxs-lookup"><span data-stu-id="b54e4-169">Using a func to fulfill a policy</span></span>

<span data-ttu-id="b54e4-170">Mogą wystąpić sytuacje, w których wypełniając zasad jest proste wyrażenia w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b54e4-170">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="b54e4-171">Jest to możliwe, aby podać `Func<AuthorizationHandlerContext, bool>` podczas konfigurowania zasad `RequireAssertion` konstruktora zasad.</span><span class="sxs-lookup"><span data-stu-id="b54e4-171">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="b54e4-172">Na przykład poprzedniej `BadgeEntryHandler` można dopasować w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b54e4-172">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="b54e4-173">Uzyskiwanie dostępu do kontekstu żądania MVC w procedurach obsługi</span><span class="sxs-lookup"><span data-stu-id="b54e4-173">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="b54e4-174">`HandleRequirementAsync` Metody wdrożenia w obsłudze autoryzacji zawiera dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi.</span><span class="sxs-lookup"><span data-stu-id="b54e4-174">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="b54e4-175">Struktur, takich jak MVC lub Jabbr są bezpłatne dowolny obiekt, aby dodać `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="b54e4-175">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="b54e4-176">Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości.</span><span class="sxs-lookup"><span data-stu-id="b54e4-176">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="b54e4-177">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko inne podany, MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="b54e4-177">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="b54e4-178">Korzystanie z `Resource` właściwość to struktura określone.</span><span class="sxs-lookup"><span data-stu-id="b54e4-178">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="b54e4-179">Korzystając z informacji w `Resource` właściwość ogranicza zasad autoryzacji do określonej struktury.</span><span class="sxs-lookup"><span data-stu-id="b54e4-179">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="b54e4-180">Należy rzutować `Resource` właściwość za pomocą `is` słowo kluczowe, a następnie potwierdź rzutowanie zakończyła się pomyślnie, aby upewnić się, kod nie awarii przy użyciu `InvalidCastException` uruchamiania innych platform:</span><span class="sxs-lookup"><span data-stu-id="b54e4-180">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
