---
title: Autoryzacja niestandardowa oparte na zasadach w ASP.NET Core
author: rick-anderson
description: "Informacje o sposobie tworzenia i używania programów obsługi zasad autoryzacji niestandardowej dla wymuszania wymagań autoryzacji w aplikacji platformy ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: c249985a6266483d47f447ac4a232546ed2b2708
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="fe0f6-103">Niestandardowe autoryzacji opartych na zasadach</span><span class="sxs-lookup"><span data-stu-id="fe0f6-103">Custom policy-based authorization</span></span>

<span data-ttu-id="fe0f6-104">Poniżej obejmuje [autoryzacji opartej na rolach](xref:security/authorization/roles) i [autoryzacji opartej na oświadczeniach](xref:security/authorization/claims) użyć wymaganie, obsługi wymagania i wstępnie skonfigurowanych zasad.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="fe0f6-105">Te bloki konstrukcyjne obsługi wyrażenie ocen autoryzacji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="fe0f6-106">Wynik jest strukturą bardziej rozbudowane, wielokrotnego użytku, testować autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="fe0f6-107">Zasady autoryzacji składa się z co najmniej jednego wymagania.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="fe0f6-108">Jest on zarejestrowany w ramach konfiguracji usługi autoryzacji, w `ConfigureServices` metody `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-108">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="fe0f6-109">W powyższym przykładzie tworzona jest zasada "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="fe0f6-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="fe0f6-110">Ma on jeden wymaganie, że o minimalnym wieku, które są udostępniane jako parametr wymogiem.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-110">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="fe0f6-111">Zasady są stosowane przy użyciu `[Authorize]` atrybutu o nazwie zasad.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="fe0f6-112">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="fe0f6-113">Wymagania</span><span class="sxs-lookup"><span data-stu-id="fe0f6-113">Requirements</span></span>

<span data-ttu-id="fe0f6-114">Wymaganie autoryzacji to zbiór parametrów danych, które zasady można użyć do oceny, bieżący podmiot zabezpieczeń użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="fe0f6-115">W zasadach "AtLeast21" to wymaganie jest pojedynczy parametr&mdash;minimalnym wieku.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="fe0f6-116">Implementuje wymagane `IAuthorizationRequirement`, która jest interfejsem znacznika puste.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-116">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="fe0f6-117">Sparametryzowane minimalny wymagany wiek można realizowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="fe0f6-118">Wymagania nie musi być danych lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="fe0f6-119">Programy obsługi autoryzacji</span><span class="sxs-lookup"><span data-stu-id="fe0f6-119">Authorization handlers</span></span>

<span data-ttu-id="fe0f6-120">Program obsługi autoryzacji jest odpowiedzialny za obliczania właściwości zapotrzebowania.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="fe0f6-121">Program obsługi autoryzacji ocenia wymagania względem podanego `AuthorizationHandlerContext` ustalenie, czy dostęp jest dozwolony.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-121">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="fe0f6-122">Wymóg może mieć [wielu obsług](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="fe0f6-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="fe0f6-123">Programy obsługi dziedziczą `AuthorizationHandler<T>`, gdzie `T` jest wymagane do obsługi.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-123">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="fe0f6-124">Program obsługi minimalnego wieku może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-124">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="fe0f6-125">Poprzedni kod określa, czy bieżący użytkownik główna ma datę urodzenia oświadczenia, które zostały wystawione przez znanego i zaufanego wystawcy.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-125">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="fe0f6-126">Autoryzacji nie może wystąpić w przypadku braku oświadczenia, w którym to przypadku jest zwracany o zakończeniu zadania.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-126">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="fe0f6-127">Gdy występuje oświadczenia są obliczane wieku użytkownika.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-127">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="fe0f6-128">Jeśli użytkownika spełnia minimalnego wieku zdefiniowane przez wymaganie, autoryzacji uważa się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-128">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="fe0f6-129">Po pomyślnym zakończeniu operacji, autoryzacji `context.Succeed` została wywołana z spełnione wymaganie jako parametr.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-129">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="fe0f6-130">Rejestracja programu obsługi</span><span class="sxs-lookup"><span data-stu-id="fe0f6-130">Handler registration</span></span>

<span data-ttu-id="fe0f6-131">Programy obsługi są zarejestrowane w kolekcji usługi podczas konfigurowania.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-131">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="fe0f6-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-132">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="fe0f6-133">Każdy program obsługi jest dodawany do kolekcji usług przez wywoływanie `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-133">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="fe0f6-134">Co powinna zwracać obsługi?</span><span class="sxs-lookup"><span data-stu-id="fe0f6-134">What should a handler return?</span></span>

<span data-ttu-id="fe0f6-135">Należy pamiętać, że `Handle` metody w [przykład obsługi](#security-authorization-handler-example) nie zwraca żadnej wartości.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-135">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="fe0f6-136">Jaki jest stan powodzenia lub niepowodzenia wskazanych?</span><span class="sxs-lookup"><span data-stu-id="fe0f6-136">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="fe0f6-137">Program obsługi oznacza Powodzenie przez wywołanie metody `context.Succeed(IAuthorizationRequirement requirement)`, przekazywanie wymaganie, która została pomyślnie zweryfikowana.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-137">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="fe0f6-138">Program obsługi nie musi obsługiwać awarie ogólnie rzecz biorąc, zgodnie z innych programów obsługi na ten sam wymóg może się powieść.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-138">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="fe0f6-139">Aby zagwarantować awarii, nawet w przypadku innych programów obsługi wymagań powiedzie się, należy wywołać `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-139">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="fe0f6-140">Niezależnie od wywołana wewnątrz obsługi sieci obsługi wszystkie wymagane będzie wywoływany, gdy zasady wymaga to wymaganie.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-140">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="fe0f6-141">Dzięki temu wymagania dotyczące efekty uboczne, takich jak rejestrowanie, które będą zawsze miały miejsce nawet wtedy, gdy `context.Fail()` została wywołana w innego programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-141">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="fe0f6-142">Dlaczego może być wielu obsług dla wymagania?</span><span class="sxs-lookup"><span data-stu-id="fe0f6-142">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="fe0f6-143">W przypadkach, w którym ma oceny na **lub** podstawę, wdrożenie wielu obsług dla pojedynczego wymagania.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-143">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="fe0f6-144">Na przykład firma Microsoft ma drzwi, które mogły być otwierane tylko w przypadku kart klucza.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-144">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="fe0f6-145">Jeśli karta klucza pozostanie w domu, recepcjonista drukuje naklejce tymczasowego i otwiera drzwi.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-145">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="fe0f6-146">W tym scenariuszu należy wymaganiem pojedynczego *BuildingEntry*, ale wielu obsług, każda z nich badanie pojedynczego wymaganie.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-146">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="fe0f6-147">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="fe0f6-147">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="fe0f6-148">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fe0f6-148">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="fe0f6-149">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fe0f6-149">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="fe0f6-150">Upewnij się, że oba programy obsługi są [zarejestrowany](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="fe0f6-150">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="fe0f6-151">Jeśli albo obsługi zakończy się powodzeniem, gdy zasada oblicza `BuildingEntryRequirement`, oceny zasad zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-151">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="fe0f6-152">Przy użyciu func do zrealizowania zasady</span><span class="sxs-lookup"><span data-stu-id="fe0f6-152">Using a func to fulfill a policy</span></span>

<span data-ttu-id="fe0f6-153">Mogą wystąpić sytuacje, w których spełnienie jest proste express w kodzie zasady.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-153">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="fe0f6-154">Można podać `Func<AuthorizationHandlerContext, bool>` przy konfigurowaniu zasad z `RequireAssertion` konstruktora zasad.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-154">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="fe0f6-155">Na przykład poprzedniej `BadgeEntryHandler` można ponownie zapisać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-155">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="fe0f6-156">Uzyskiwanie dostępu do kontekstu żądania MVC w obsłudze</span><span class="sxs-lookup"><span data-stu-id="fe0f6-156">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="fe0f6-157">`HandleRequirementAsync` Metoda implementuje obsługi autoryzacji ma dwa parametry: `AuthorizationHandlerContext` i `TRequirement` są obsługi.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-157">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="fe0f6-158">Struktury, np. MVC lub Jabbr są także dodać dowolny obiekt, aby `Resource` właściwość `AuthorizationHandlerContext` do przekazania dodatkowych informacji.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-158">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="fe0f6-159">Na przykład MVC przekazuje wystąpienie [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) w `Resource` właściwości.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-159">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="fe0f6-160">Ta właściwość zapewnia dostęp do `HttpContext`, `RouteData`, a wszystko else zapewniane przez MVC i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-160">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="fe0f6-161">Korzystanie z `Resource` właściwość jest tylko dla struktury.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-161">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="fe0f6-162">Korzystając z informacji w `Resource` właściwości ogranicza zasad autoryzacji do określonej struktury.</span><span class="sxs-lookup"><span data-stu-id="fe0f6-162">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="fe0f6-163">Należy rzutować `Resource` za pomocą właściwości `as` — słowo kluczowe, a następnie potwierdź rzutowanie ma powiedzie się, aby upewnić się, kod nie awarii z `InvalidCastException` uruchomienia na innych platformach:</span><span class="sxs-lookup"><span data-stu-id="fe0f6-163">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
