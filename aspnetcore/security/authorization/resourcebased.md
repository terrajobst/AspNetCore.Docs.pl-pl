---
title: Autoryzacja oparta na zasobach w ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zaimplementować autoryzację opartą na zasobach w aplikacji ASP.NET Core, gdy atrybut Autoryzuj nie wystarcza.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 2be611c754583d996db7107f341b1be03cef73cf
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664802"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="d1ada-103">Autoryzacja oparta na zasobach w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d1ada-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="d1ada-104">Strategia autoryzacji zależy od zasobów, do których uzyskuje się dostęp.</span><span class="sxs-lookup"><span data-stu-id="d1ada-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="d1ada-105">Rozważ dokument, który ma właściwość Author.</span><span class="sxs-lookup"><span data-stu-id="d1ada-105">Consider a document that has an author property.</span></span> <span data-ttu-id="d1ada-106">Tylko autor może zaktualizować dokument.</span><span class="sxs-lookup"><span data-stu-id="d1ada-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="d1ada-107">W związku z tym dokument musi zostać pobrany z magazynu danych, zanim będzie można przeprowadzić ocenę autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="d1ada-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="d1ada-108">Obliczanie atrybutu występuje przed powiązaniem danych i przed wykonaniem procedury obsługi stron lub akcji ładującej dokument.</span><span class="sxs-lookup"><span data-stu-id="d1ada-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="d1ada-109">Z tych powodów niewystarczająca jest autoryzacja deklaratywna z atrybutem `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="d1ada-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="d1ada-110">Zamiast tego można wywołać niestandardową metodę autoryzacji&mdash;stylu znanej jako samodzielna *autoryzacja*.</span><span class="sxs-lookup"><span data-stu-id="d1ada-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="d1ada-111">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d1ada-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
<span data-ttu-id="d1ada-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d1ada-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d1ada-113">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d1ada-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

<span data-ttu-id="d1ada-114">[Tworzenie aplikacji ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data) zawiera przykładową aplikację, która korzysta z autoryzacji opartej na zasobach.</span><span class="sxs-lookup"><span data-stu-id="d1ada-114">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="d1ada-115">Używanie bezwzględnej autoryzacji</span><span class="sxs-lookup"><span data-stu-id="d1ada-115">Use imperative authorization</span></span>

<span data-ttu-id="d1ada-116">Autoryzacja jest zaimplementowana jako usługa [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) i jest zarejestrowana w kolekcji usług w ramach klasy `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d1ada-116">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="d1ada-117">Usługa jest udostępniana za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection) do obsługi stron lub akcji.</span><span class="sxs-lookup"><span data-stu-id="d1ada-117">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="d1ada-118">`IAuthorizationService` ma dwa przeciążania metody `AuthorizeAsync`: jeden z nich akceptuje zasób i nazwę zasad, a pozostałe zaakceptują zasób i listę wymagań do obliczenia.</span><span class="sxs-lookup"><span data-stu-id="d1ada-118">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="d1ada-119">W poniższym przykładzie zasób do zabezpieczenia jest ładowany do niestandardowego obiektu `Document`.</span><span class="sxs-lookup"><span data-stu-id="d1ada-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="d1ada-120">Zostanie wywołane Przeciążenie `AuthorizeAsync`, aby określić, czy bieżący użytkownik może edytować podany dokument.</span><span class="sxs-lookup"><span data-stu-id="d1ada-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="d1ada-121">Niestandardowe zasady autoryzacji "EditPolicy" są uwzględniane w decyzji.</span><span class="sxs-lookup"><span data-stu-id="d1ada-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="d1ada-122">Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji, zobacz [niestandardową autoryzację opartą na zasadach](xref:security/authorization/policies) .</span><span class="sxs-lookup"><span data-stu-id="d1ada-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="d1ada-123">W poniższych przykładach kodu założono, że uwierzytelnianie zostało uruchomione i ustawiono właściwość `User`.</span><span class="sxs-lookup"><span data-stu-id="d1ada-123">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="d1ada-124">Napisz procedurę obsługi opartą na zasobach</span><span class="sxs-lookup"><span data-stu-id="d1ada-124">Write a resource-based handler</span></span>

<span data-ttu-id="d1ada-125">Pisanie procedury obsługi autoryzacji opartej na zasobach nie jest znacznie inne niż [pisanie procedury obsługi zwykłego wymagania](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="d1ada-125">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="d1ada-126">Utwórz niestandardową klasę wymagania i zaimplementuj klasę obsługi wymagań.</span><span class="sxs-lookup"><span data-stu-id="d1ada-126">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="d1ada-127">Aby uzyskać więcej informacji na temat tworzenia klasy wymagań, zobacz [wymagania](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="d1ada-127">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="d1ada-128">Klasa obsługi określa typ wymagania i zasobu.</span><span class="sxs-lookup"><span data-stu-id="d1ada-128">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="d1ada-129">Na przykład program obsługi wykorzystujący `SameAuthorRequirement` i zasób `Document`:</span><span class="sxs-lookup"><span data-stu-id="d1ada-129">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="d1ada-130">W poprzednim przykładzie Załóżmy, że `SameAuthorRequirement` jest szczególnym przypadkiem bardziej generycznej `SpecificAuthorRequirement` klasy.</span><span class="sxs-lookup"><span data-stu-id="d1ada-130">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="d1ada-131">Klasa `SpecificAuthorRequirement` (niepokazywana) zawiera właściwość `Name` reprezentującą nazwę autora.</span><span class="sxs-lookup"><span data-stu-id="d1ada-131">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="d1ada-132">Właściwość `Name` może zostać ustawiona na bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="d1ada-132">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="d1ada-133">Zarejestrowanie wymagania i procedury obsługi w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d1ada-133">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=4-8,10)]
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/2_2/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

### <a name="operational-requirements"></a><span data-ttu-id="d1ada-134">Wymagania operacyjne</span><span class="sxs-lookup"><span data-stu-id="d1ada-134">Operational requirements</span></span>

<span data-ttu-id="d1ada-135">Jeśli podejmujesz decyzje w oparciu o wyniki operacji CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), użyj klasy pomocnika [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) .</span><span class="sxs-lookup"><span data-stu-id="d1ada-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="d1ada-136">Ta klasa umożliwia pisanie pojedynczej procedury obsługi zamiast pojedynczej klasy dla każdego typu operacji.</span><span class="sxs-lookup"><span data-stu-id="d1ada-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="d1ada-137">Aby go użyć, Podaj nazwy niektórych operacji:</span><span class="sxs-lookup"><span data-stu-id="d1ada-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="d1ada-138">Procedura obsługi jest implementowana w następujący sposób przy użyciu wymagania `OperationAuthorizationRequirement` i zasobu `Document`:</span><span class="sxs-lookup"><span data-stu-id="d1ada-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

 ::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="d1ada-139">Poprzednia procedura obsługi sprawdza poprawność operacji przy użyciu zasobu, tożsamości użytkownika i właściwości `Name` wymaganie.</span><span class="sxs-lookup"><span data-stu-id="d1ada-139">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="d1ada-140">Wyzwania i Zabroń przy użyciu obsługi zasobów operacyjnych</span><span class="sxs-lookup"><span data-stu-id="d1ada-140">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="d1ada-141">W tej sekcji pokazano, jak są przetwarzane wyniki akcji wezwanie i zabraniające działania oraz jak wyzwania i zabraniają się.</span><span class="sxs-lookup"><span data-stu-id="d1ada-141">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="d1ada-142">Aby wywołać procedurę obsługi zasobów operacyjnych, określ operację podczas wywoływania `AuthorizeAsync` w obsłudze stron lub akcji.</span><span class="sxs-lookup"><span data-stu-id="d1ada-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="d1ada-143">Poniższy przykład określa, czy uwierzytelniony użytkownik może wyświetlić podany dokument.</span><span class="sxs-lookup"><span data-stu-id="d1ada-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="d1ada-144">W poniższych przykładach kodu założono, że uwierzytelnianie zostało uruchomione i ustawiono właściwość `User`.</span><span class="sxs-lookup"><span data-stu-id="d1ada-144">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="d1ada-145">Jeśli autoryzacja powiedzie się, zostanie zwrócona Strona do wyświetlania dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d1ada-145">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="d1ada-146">Jeśli autoryzacja nie powiedzie się, ale użytkownik zostanie uwierzytelniony, zwrócenie `ForbidResult` informuje wszystkie oprogramowanie pośredniczące uwierzytelniania, które nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="d1ada-146">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="d1ada-147">Podczas uwierzytelniania należy zwrócić `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="d1ada-147">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="d1ada-148">W przypadku klientów interakcyjnej przeglądarki może być konieczne przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="d1ada-148">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="d1ada-149">Jeśli autoryzacja powiedzie się, zostanie zwrócony widok dla dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d1ada-149">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="d1ada-150">Jeśli autoryzacja nie powiedzie się, zwraca `ChallengeResult` informuje inne oprogramowanie pośredniczące uwierzytelniania, że autoryzacja nie powiodła się, a oprogramowanie pośredniczące może pobrać odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="d1ada-150">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="d1ada-151">Odpowiednia odpowiedź może zwrócić kod stanu 401 lub 403.</span><span class="sxs-lookup"><span data-stu-id="d1ada-151">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="d1ada-152">W przypadku klientów interakcyjnych przeglądarki może to oznaczać przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="d1ada-152">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
