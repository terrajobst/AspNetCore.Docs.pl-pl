---
title: Autoryzacja niestandardowa oparte na zasadach w ASP.NET Core
author: rick-anderson
description: "Informacje o sposobie tworzenia i używania programów obsługi zasad autoryzacji niestandardowej dla wymuszania wymagań autoryzacji w aplikacji platformy ASP.NET Core."
keywords: Platformy ASP.NET Core autoryzacji, niestandardowych zasad, zasady autoryzacji
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="60af4-104">Niestandardowe autoryzacji opartych na zasadach</span><span class="sxs-lookup"><span data-stu-id="60af4-104">Custom policy-based authorization</span></span>

<span data-ttu-id="60af4-105">Poniżej obejmuje [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) użyć wymaganie, obsługi wymagania i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="60af4-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="60af4-106">Te bloki konstrukcyjne obsługi wyrażenie ocen autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="60af4-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="60af4-107">Wynik jest strukturą bardziej rozbudowane, wielokrotnego użytku, testować autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="60af4-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="60af4-108">Zasady autoryzacji składa się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="60af4-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="60af4-109">Jest on zarejestrowany w ramach konfiguracji usługi autoryzacji, w `ConfigureServices` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="60af4-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="60af4-110">W powyższym przykładzie tworzona jest zasada "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="60af4-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="60af4-111">Ma on jeden wymaganie, że o minimalnym wieku, które są udostępniane jako parametr wymogiem.</span><span class="sxs-lookup"><span data-stu-id="60af4-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="60af4-112">Zasady są stosowane przy użyciu `[Authorize]` atrybutu o nazwie zasad.</span><span class="sxs-lookup"><span data-stu-id="60af4-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="60af4-113">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="60af4-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="60af4-114">Wymagania</span><span class="sxs-lookup"><span data-stu-id="60af4-114">Requirements</span></span>

<span data-ttu-id="60af4-115">Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="60af4-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="60af4-116">W zasadach "AtLeast21" to wymaganie jest pojedynczy parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="60af4-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="60af4-117">Implementuje wymagane `IAuthorizationRequirement`, która jest interfejsem znacznika puste.</span><span class="sxs-lookup"><span data-stu-id="60af4-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="60af4-118">Sparametryzowane minimalny wymagany wiek można realizowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="60af4-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="60af4-119">Wymagania nie musi być danych lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="60af4-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="60af4-120">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="60af4-120">Authorization handlers</span></span>

<span data-ttu-id="60af4-121">Program obsługi autoryzacji jest odpowiedzialny za obliczania właściwości zapotrzebowania.</span><span class="sxs-lookup"><span data-stu-id="60af4-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="60af4-122">Program obsługi autoryzacji ocenia wymagania względem podanego `AuthorizationHandlerContext` ustalenie, czy dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="60af4-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="60af4-123">Wymóg może mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="60af4-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="60af4-124">Programy obsługi dziedziczą `AuthorizationHandler<T>`, gdzie `T` jest wymagane do obsługi.</span><span class="sxs-lookup"><span data-stu-id="60af4-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="60af4-125">Program obsługi minimalnego wieku może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="60af4-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="60af4-126">Poprzedni kod określa, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="60af4-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="60af4-127">Autoryzacji nie może wystąpić w przypadku braku oświadczenia, w którym to przypadku jest zwracany o zakończeniu zadania.</span><span class="sxs-lookup"><span data-stu-id="60af4-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="60af4-128">Gdy występuje oświadczenia są obliczane wieku użytkownika.</span><span class="sxs-lookup"><span data-stu-id="60af4-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="60af4-129">Jeśli użytkownika spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacji uważa się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="60af4-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="60af4-130">Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako parametr.</span><span class="sxs-lookup"><span data-stu-id="60af4-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="60af4-131">Rejestracja programu obsługi</span><span class="sxs-lookup"><span data-stu-id="60af4-131">Handler registration</span></span>

<span data-ttu-id="60af4-132">Programy obsługi są zarejestrowane w kolekcji usługi podczas konfigurowania.</span><span class="sxs-lookup"><span data-stu-id="60af4-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="60af4-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="60af4-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="60af4-134">Każdy program obsługi jest dodawany do kolekcji usług przez wywoływanie `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="60af4-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="60af4-135">Co powinna zwracać obsługi?</span><span class="sxs-lookup"><span data-stu-id="60af4-135">What should a handler return?</span></span>

<span data-ttu-id="60af4-136">Należy pamiętać, że `Handle` metody w [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="60af4-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="60af4-137">Jaki jest stan powodzenia lub niepowodzenia wskazanych?</span><span class="sxs-lookup"><span data-stu-id="60af4-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="60af4-138">Program obsługi oznacza Powodzenie przez wywołanie metody `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, która została pomyślnie zweryfikowana.</span><span class="sxs-lookup"><span data-stu-id="60af4-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="60af4-139">Program obsługi nie musi do obsługi błędów ogólnie rzecz biorąc, zgodnie z innych programów obsługi na ten sam wymóg może się powieść.</span><span class="sxs-lookup"><span data-stu-id="60af4-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="60af4-140">Aby zagwarantować awarii, nawet w przypadku innych programów obsługi wymagań powiedzie się, należy wywołać `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="60af4-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="60af4-141">Niezależnie od wywołana wewnątrz obsługi sieci obsługi wszystkie wymagane będzie wywoływany, gdy zasady wymaga to wymaganie.</span><span class="sxs-lookup"><span data-stu-id="60af4-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="60af4-142">Dzięki temu wymagania dotyczące efekty uboczne, takich jak rejestrowanie, które będą zawsze miały miejsce nawet wtedy, gdy `context.Fail()` została wywołana w innego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="60af4-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="60af4-143">Dlaczego może być wielu obsług dla wymagania?</span><span class="sxs-lookup"><span data-stu-id="60af4-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="60af4-144">W przypadkach, w którym ma oceny na **lub** podstawę, wdrożenie wielu obsług dla pojedynczego wymagania.</span><span class="sxs-lookup"><span data-stu-id="60af4-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="60af4-145">Na przykład firma Microsoft ma drzwi, które mogły być otwierane tylko w przypadku kart klucza.</span><span class="sxs-lookup"><span data-stu-id="60af4-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="60af4-146">Jeśli karta klucza pozostanie w domu, recepcjonista drukuje naklejce tymczasowego i otwiera drzwi.</span><span class="sxs-lookup"><span data-stu-id="60af4-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="60af4-147">W tym scenariuszu należy wymaganiem pojedynczego *BuildingEntry*, ale wielu obsług, każda z nich badanie pojedynczego wymaganie.</span><span class="sxs-lookup"><span data-stu-id="60af4-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="60af4-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="60af4-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="60af4-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="60af4-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="60af4-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="60af4-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="60af4-151">Upewnij się, że oba programy obsługi są [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="60af4-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="60af4-152">Jeśli albo obsługi zakończy się powodzeniem, gdy zasada oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="60af4-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="60af4-153">Przy użyciu func do zrealizowania zasady</span><span class="sxs-lookup"><span data-stu-id="60af4-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="60af4-154">Mogą wystąpić sytuacje, w których spełnienie jest proste express w kodzie zasady.</span><span class="sxs-lookup"><span data-stu-id="60af4-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="60af4-155">Można podać `Func<AuthorizationHandlerContext, bool>` przy konfigurowaniu zasad z `RequireAssertion` konstruktora zasad.</span><span class="sxs-lookup"><span data-stu-id="60af4-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="60af4-156">Na przykład poprzedniej `BadgeEntryHandler` można ponownie zapisać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="60af4-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="60af4-157">Uzyskiwanie dostępu do kontekstu żądania MVC w obsłudze</span><span class="sxs-lookup"><span data-stu-id="60af4-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="60af4-158">`HandleRequirementAsync` Metoda implementuje obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi.</span><span class="sxs-lookup"><span data-stu-id="60af4-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="60af4-159">Struktury, np. MVC lub Jabbr są także dodać dowolny obiekt, aby `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="60af4-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="60af4-160">Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości.</span><span class="sxs-lookup"><span data-stu-id="60af4-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="60af4-161">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko else zapewniane przez MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="60af4-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="60af4-162">Korzystanie z `Resource` właściwość jest tylko dla struktury.</span><span class="sxs-lookup"><span data-stu-id="60af4-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="60af4-163">Korzystając z informacji w `Resource` właściwości ogranicza zasad autoryzacji do określonej struktury.</span><span class="sxs-lookup"><span data-stu-id="60af4-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="60af4-164">Należy rzutować `Resource` za pomocą właściwości `as` — słowo kluczowe, a następnie potwierdź rzutowanie ma powiedzie się, aby upewnić się, kod nie awarii z `InvalidCastException` uruchomienia na innych platformach:</span><span class="sxs-lookup"><span data-stu-id="60af4-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
