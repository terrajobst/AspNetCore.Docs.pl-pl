---
title: Autoryzacja oparta na zasób w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zaimplementować autoryzacja na podstawie zasobów w aplikacji ASP.NET Core, gdy atrybut autoryzacji nie wystarczyć.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: afc152ea677cab42d57bd642b4821159f125117e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898342"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="9fbb4-103">Autoryzacja oparta na zasób w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9fbb4-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="9fbb4-104">Strategia autoryzacji zależy od zasobu, do którego uzyskiwany jest dostęp.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="9fbb4-105">Należy wziąć pod uwagę dokument, który ma właściwość autora.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-105">Consider a document that has an author property.</span></span> <span data-ttu-id="9fbb4-106">Tylko autor może zaktualizować dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="9fbb4-107">W związku z tym dokument musi zostać pobrany z magazynu danych, zanim nastąpi oceny autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="9fbb4-108">Atrybut występuje przed powiązanie danych oraz przed wykonaniem tej procedury obsługi strony lub akcji, która ładuje dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="9fbb4-109">Z tego względu deklaratywne autoryzacją za pomocą `[Authorize]` nie wystarcza atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="9fbb4-110">Zamiast tego należy wywołać metodę autoryzacja niestandardowa&mdash;stylu, znane jako *imperatywne autoryzacji*.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="9fbb4-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="9fbb4-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="9fbb4-112">[Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data) zawiera przykładową aplikację, która używa autoryzacja na podstawie zasobów.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="9fbb4-113">Użycie imperatywnych autoryzacji</span><span class="sxs-lookup"><span data-stu-id="9fbb4-113">Use imperative authorization</span></span>

<span data-ttu-id="9fbb4-114">Autoryzacja jest implementowany jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) usługi i nie jest zarejestrowany w kolekcji usługi w ramach `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="9fbb4-115">Usługa jest udostępniana za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) do obsługi strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="9fbb4-116">`IAuthorizationService` ma dwa `AuthorizeAsync` przeciążenia metody: przyjmuje jeden zasób i nazwę zasady i innych akceptowanie zasobu i zawiera listę wymagań do oceny.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="9fbb4-117">W poniższym przykładzie zasobu, który ma zostać zabezpieczony jest ładowany do niestandardowego `Document` obiektu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="9fbb4-118">`AuthorizeAsync` Przeciążenie jest wywoływane w celu ustalenia, czy bieżący użytkownik może edytować podany dokument.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="9fbb4-119">Niestandardowe zasady autoryzacji "EditPolicy" jest brane pod uwagę decyzji.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="9fbb4-120">Zobacz [niestandardowego na podstawie zasad autoryzacji](xref:security/authorization/policies) Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="9fbb4-121">Poniższy kod przykładów przyjęto założenie, uwierzytelnianie zostało uruchomione i ustaw `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="9fbb4-122">Pisanie programu obsługi opartego na zasobach</span><span class="sxs-lookup"><span data-stu-id="9fbb4-122">Write a resource-based handler</span></span>

<span data-ttu-id="9fbb4-123">Pisanie programu obsługi, do autoryzacji opartej na zasób nie jest znacznie różni się od [pisanie programu obsługi wymagań zwykły](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="9fbb4-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="9fbb4-124">Utwórz niestandardowe wymagania klasę i zaimplementować klasę programu obsługi wymagań.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="9fbb4-125">Aby uzyskać więcej informacji na temat tworzenia klasy wymagań, zobacz [wymagania](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="9fbb4-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="9fbb4-126">Klasy obsługi określa wymagań i typu zasobu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="9fbb4-127">Na przykład program obsługi przy użyciu `SameAuthorRequirement` i `Document` zasobów w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9fbb4-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="9fbb4-128">W powyższym przykładzie załóżmy, że `SameAuthorRequirement` stanowią specjalny przypadek więcej ogólny `SpecificAuthorRequirement` klasy.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="9fbb4-129">`SpecificAuthorRequirement` Klasy (niewyświetlany) zawiera `Name` właściwość reprezentuje nazwę autora.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="9fbb4-130">`Name` Można ustawić właściwości dla bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="9fbb4-131">Zarejestruj wymagania oraz program obsługi w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9fbb4-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="9fbb4-132">Wymagania operacyjne</span><span class="sxs-lookup"><span data-stu-id="9fbb4-132">Operational requirements</span></span>

<span data-ttu-id="9fbb4-133">Jeśli wprowadzasz decyzji w oparciu o wyniki operacji CRUD (tworzenia, odczytu, Update, Delete), użyj [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Klasa pomocy.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="9fbb4-134">Ta klasa umożliwia pisanie pojedynczy program obsługi, a nie poszczególnych klas dla każdego typu operacji.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="9fbb4-135">Aby go użyć, należy podać nazwy niektórych operacji:</span><span class="sxs-lookup"><span data-stu-id="9fbb4-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="9fbb4-136">Program obsługi jest implementowana w następujący sposób, przy użyciu `OperationAuthorizationRequirement` wymagań i `Document` zasobów:</span><span class="sxs-lookup"><span data-stu-id="9fbb4-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="9fbb4-137">Poprzedni program obsługi sprawdza poprawność tej operacji z użyciem zasobu, tożsamość użytkownika i wymogów `Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="9fbb4-138">Aby wywołać procedurę obsługi operacyjnej zasobów, określać podczas wywoływania operacji `AuthorizeAsync` obsługi strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="9fbb4-139">Poniższy przykład określa, czy uwierzytelniony użytkownik jest uprawniony do wyświetlenia podany dokument.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="9fbb4-140">Poniższy kod przykładów przyjęto założenie, uwierzytelnianie zostało uruchomione i ustaw `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="9fbb4-141">Jeśli autoryzacja zakończy się powodzeniem, zwracany jest strona do wyświetlania dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="9fbb4-142">Jeśli autoryzacji kończy się niepowodzeniem, ale użytkownik jest uwierzytelniony, zwracając `ForbidResult` informuje oprogramowanie pośredniczące uwierzytelniania, który Autoryzacja nie powiodła się.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="9fbb4-143">Element `ChallengeResult` jest zwracana, gdy można dokonać uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="9fbb4-144">Dla klientów w przeglądarkach interaktywne jego może być przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="9fbb4-145">Jeśli autoryzacja zakończy się powodzeniem, zwracany jest widok dla dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="9fbb4-146">W przypadku niepowodzenia autoryzacji zwracanie `ChallengeResult` dowolnego oprogramowania pośredniczącego uwierzytelniania informuje, że autoryzacja nie powiodła się, a oprogramowanie pośredniczące może potrwać właściwą odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="9fbb4-147">Właściwa odpowiedź może zwracać kod stanu 401 lub 403.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="9fbb4-148">Dla klientów w przeglądarkach interaktywne może oznaczać, że przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="9fbb4-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
