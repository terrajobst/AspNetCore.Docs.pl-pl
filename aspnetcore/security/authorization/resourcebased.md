---
title: Autoryzacja oparta na zasób w programie ASP.NET Core
author: scottaddie
description: Dowiedz się, jak zaimplementować autoryzacja na podstawie zasobów w aplikacji ASP.NET Core, gdy atrybut autoryzacji nie wystarczyć.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 2cb3844a38f7482c27fb471343109d51a516ea20
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206699"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="ae5e9-103">Autoryzacja oparta na zasób w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae5e9-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="ae5e9-104">Strategia autoryzacji zależy od zasobu, do którego uzyskiwany jest dostęp.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="ae5e9-105">Należy wziąć pod uwagę dokumentu, który ma właściwość autora.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-105">Consider a document which has an author property.</span></span> <span data-ttu-id="ae5e9-106">Tylko autor może zaktualizować dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="ae5e9-107">W związku z tym dokument musi zostać pobrany z magazynu danych, zanim nastąpi oceny autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="ae5e9-108">Atrybut występuje przed powiązanie danych oraz przed wykonaniem tej procedury obsługi strony lub akcji, która ładuje dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="ae5e9-109">Z tego względu deklaratywne autoryzacją za pomocą `[Authorize]` atrybutu nie wystarczyć.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="ae5e9-110">Zamiast tego należy wywołać metodę autoryzacja niestandardowa&mdash;stylu, znane jako imperatywne autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="ae5e9-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ae5e9-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ae5e9-112">[Tworzenie aplikacji platformy ASP.NET Core przy użyciu danych użytkownika chronionych przez autoryzację](xref:security/authorization/secure-data) zawiera przykładową aplikację, która używa autoryzacja na podstawie zasobów.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="ae5e9-113">Użycie imperatywnych autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ae5e9-113">Use imperative authorization</span></span>

<span data-ttu-id="ae5e9-114">Autoryzacja jest implementowany jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) usługi i nie jest zarejestrowany w kolekcji usługi w ramach `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="ae5e9-115">Usługa jest udostępniana za pośrednictwem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) do obsługi strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="ae5e9-116">`IAuthorizationService` ma dwa `AuthorizeAsync` przeciążenia metody: przyjmuje jeden zasób i nazwę zasady i innych akceptowanie zasobu i zawiera listę wymagań do oceny.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae5e9-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae5e9-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="ae5e9-119">W poniższym przykładzie zasobu, który ma zostać zabezpieczony jest ładowany do niestandardowego `Document` obiektu.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="ae5e9-120">`AuthorizeAsync` Przeciążenie jest wywoływane w celu ustalenia, czy bieżący użytkownik może edytować podany dokument.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="ae5e9-121">Niestandardowe zasady autoryzacji "EditPolicy" jest brane pod uwagę decyzji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="ae5e9-122">Zobacz [niestandardowego na podstawie zasad autoryzacji](xref:security/authorization/policies) Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="ae5e9-123">Poniższy kod przykładów przyjęto założenie, uwierzytelnianie zostało uruchomione i ustaw `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae5e9-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae5e9-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="ae5e9-126">Pisanie programu obsługi opartego na zasobach</span><span class="sxs-lookup"><span data-stu-id="ae5e9-126">Write a resource-based handler</span></span>

<span data-ttu-id="ae5e9-127">Pisanie programu obsługi, do autoryzacji opartej na zasób nie jest znacznie różni się od [pisanie programu obsługi wymagań zwykły](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="ae5e9-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="ae5e9-128">Utwórz niestandardowe wymagania klasę i zaimplementować klasę programu obsługi wymagań.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="ae5e9-129">Klasy obsługi określa wymagań i typu zasobu.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="ae5e9-130">Na przykład program obsługi przy użyciu `SameAuthorRequirement` wymagań i `Document` zasobów wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="ae5e9-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae5e9-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae5e9-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="ae5e9-133">Zarejestruj wymagania oraz program obsługi w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="ae5e9-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="ae5e9-134">Wymagania operacyjne</span><span class="sxs-lookup"><span data-stu-id="ae5e9-134">Operational requirements</span></span>

<span data-ttu-id="ae5e9-135">Jeśli wprowadzasz decyzji w oparciu o wyniki operacji CRUD (tworzenia, odczytu, Update, Delete), użyj [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Klasa pomocy.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="ae5e9-136">Ta klasa umożliwia pisanie pojedynczy program obsługi, a nie poszczególnych klas dla każdego typu operacji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="ae5e9-137">Aby go użyć, należy podać nazwy niektórych operacji:</span><span class="sxs-lookup"><span data-stu-id="ae5e9-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="ae5e9-138">Program obsługi jest implementowana w następujący sposób, przy użyciu `OperationAuthorizationRequirement` wymagań i `Document` zasobów:</span><span class="sxs-lookup"><span data-stu-id="ae5e9-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae5e9-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae5e9-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="ae5e9-141">Poprzedni program obsługi sprawdza poprawność tej operacji z użyciem zasobu, tożsamość użytkownika i wymogów `Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="ae5e9-142">Aby wywołać procedurę obsługi operacyjnej zasobów, określać podczas wywoływania operacji `AuthorizeAsync` obsługi strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="ae5e9-143">Poniższy przykład określa, czy uwierzytelniony użytkownik jest uprawniony do wyświetlenia podany dokument.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="ae5e9-144">Poniższy kod przykładów przyjęto założenie, uwierzytelnianie zostało uruchomione i ustaw `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae5e9-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="ae5e9-146">Jeśli autoryzacja zakończy się powodzeniem, zwracany jest strona do wyświetlania dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="ae5e9-147">Jeśli autoryzacji kończy się niepowodzeniem, ale użytkownik jest uwierzytelniony, zwracając `ForbidResult` informuje oprogramowanie pośredniczące uwierzytelniania, który Autoryzacja nie powiodła się.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="ae5e9-148">Element `ChallengeResult` jest zwracana, gdy można dokonać uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="ae5e9-149">Dla klientów w przeglądarkach interaktywne jego może być przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae5e9-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae5e9-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="ae5e9-151">Jeśli autoryzacja zakończy się powodzeniem, zwracany jest widok dla dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="ae5e9-152">W przypadku niepowodzenia autoryzacji zwracanie `ChallengeResult` dowolnego oprogramowania pośredniczącego uwierzytelniania informuje, że autoryzacja nie powiodła się, a oprogramowanie pośredniczące może potrwać właściwą odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="ae5e9-153">Właściwa odpowiedź może zwracać kod stanu 401 lub 403.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="ae5e9-154">Dla klientów w przeglądarkach interaktywne może oznaczać, że przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="ae5e9-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
