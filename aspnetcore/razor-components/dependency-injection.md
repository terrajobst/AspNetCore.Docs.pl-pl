---
title: Wstrzykiwanie zależności składników razor
author: guardrex
description: Zobacz, jak Blazor i składniki Razor aplikacje mogą używać usług przez iniekcję je na składniki.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 804fdf0254723f5e5913f23c815f485bbf094321
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978501"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="96a23-103">Wstrzykiwanie zależności składników razor</span><span class="sxs-lookup"><span data-stu-id="96a23-103">Razor Components dependency injection</span></span>

<span data-ttu-id="96a23-104">Przez [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="96a23-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="96a23-105">Obsługuje składniki razor [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="96a23-105">Razor Components supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="96a23-106">Aplikacje mogą używać wbudowanych usług przez iniekcję je na składniki.</span><span class="sxs-lookup"><span data-stu-id="96a23-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="96a23-107">Aplikacje można zdefiniować i zarejestrować niestandardowych usług i udostępnić je w całej aplikacji za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="96a23-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="96a23-108">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="96a23-108">Dependency injection</span></span>

<span data-ttu-id="96a23-109">DI to technika do uzyskiwania dostępu do usługi skonfigurowane w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="96a23-110">Może to być przydatne w aplikacjach Razor składników:</span><span class="sxs-lookup"><span data-stu-id="96a23-110">This can be useful in Razor Components apps to:</span></span>

* <span data-ttu-id="96a23-111">Udostępnić pojedyncze wystąpienie klasy usługi w wielu składników, znane jako *pojedyncze* usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="96a23-112">Oddzielenie składników od klas konkretnych usług, za pomocą abstrakcje odwołania.</span><span class="sxs-lookup"><span data-stu-id="96a23-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="96a23-113">Na przykład, należy wziąć pod uwagę interfejs `IDataAccess` do uzyskiwania dostępu do danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="96a23-114">Interfejs jest implementowany przez konkretny `DataAccess` klasy i zarejestrowany jako usługa w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="96a23-115">Gdy składnik używa DI do odbierania `IDataAccess` implementacji, składnik nie jest ściśle do konkretnego typu.</span><span class="sxs-lookup"><span data-stu-id="96a23-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="96a23-116">Wdrożenia można wymieniać, być może do implementację testową w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="96a23-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="96a23-117">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="96a23-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="96a23-118">Dodawanie usług do DI</span><span class="sxs-lookup"><span data-stu-id="96a23-118">Add services to DI</span></span>

<span data-ttu-id="96a23-119">Po utworzeniu nowej aplikacji, należy zbadać `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="96a23-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="96a23-120">`ConfigureServices` Metody jest przekazywana <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, który znajduje się lista obiektów deskryptora usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="96a23-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="96a23-121">Usługi są dodawane, zapewniając deskryptory usługi do kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="96a23-122">W poniższym przykładzie zademonstrowano pojęcia z `IDataAccess` interfejsu i jego konkretną implementację `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="96a23-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="96a23-123">Usługi mogą być skonfigurowane przy użyciu okresy istnienia pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="96a23-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="96a23-124">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="96a23-124">Lifetime</span></span> | <span data-ttu-id="96a23-125">Opis</span><span class="sxs-lookup"><span data-stu-id="96a23-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="96a23-126">Tworzy DI *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="96a23-127">Wszystkie składniki, które wymagają zainstalowania tej usługi odbierać odwołanie do tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="96a23-127">All components requiring this service receive a reference to this instance.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="96a23-128">Zawsze, gdy składnik wymaga tej usługi, otrzymuje *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-128">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="96a23-129">Blazor po stronie klienta nie ma obecnie koncepcji DI zakresów.</span><span class="sxs-lookup"><span data-stu-id="96a23-129">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="96a23-130">`Scoped` zachowuje się jak `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="96a23-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="96a23-131">Jednak obsługuje składniki programu ASP.NET Core Razor `Scoped` okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="96a23-131">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="96a23-132">W składniku Razor rejestracji usługi o określonym zakresie jest ograniczony do połączenia.</span><span class="sxs-lookup"><span data-stu-id="96a23-132">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="96a23-133">Z tego powodu przy użyciu usługi o określonym zakresie była preferowana dla usług, które powinien być ograniczony do bieżącego użytkownika, nawet jeśli bieżącym celem jest do uruchomienia po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="96a23-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="96a23-134">DI system jest oparty na systemie DI, w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96a23-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="96a23-135">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="96a23-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="96a23-136">Usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="96a23-136">Default services</span></span>

<span data-ttu-id="96a23-137">Usługi domyślne są automatycznie dodawane do kolekcji usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="96a23-138">Usługa</span><span class="sxs-lookup"><span data-stu-id="96a23-138">Service</span></span> | <span data-ttu-id="96a23-139">Opis</span><span class="sxs-lookup"><span data-stu-id="96a23-139">Description</span></span> |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="96a23-140">Zawiera metody służące do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego z użyciem identyfikatora URI (pojedyncze wystąpienie).</span><span class="sxs-lookup"><span data-stu-id="96a23-140">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="96a23-141">Należy pamiętać, że to wystąpienie `HttpClient` korzysta z przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="96a23-141">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="96a23-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) automatycznie zostaje ustawiony poziom podstawowy prefiks identyfikatora URI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-142">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="96a23-143">`HttpClient` jest dostępna wyłącznie dla aplikacji Blazor po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="96a23-143">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="96a23-144">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, do której może być wysyłane wywołania.</span><span class="sxs-lookup"><span data-stu-id="96a23-144">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="96a23-145">Aby uzyskać więcej informacji, zobacz <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="96a23-145">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="96a23-146">Zawiera pomocnicy do pracy ze stanem identyfikatory URI i nawigacji (pojedyncze wystąpienie).</span><span class="sxs-lookup"><span data-stu-id="96a23-146">Contains helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="96a23-147">`IUriHelper` podano zarówno Blazor, jak i Razor składników aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-147">`IUriHelper` is provided to both Blazor and Razor Components apps.</span></span> |

<span data-ttu-id="96a23-148">Istnieje możliwość używania dostawcy niestandardowego usługi zamiast domyślnego dostawcę usługi dodane przez szablon domyślny.</span><span class="sxs-lookup"><span data-stu-id="96a23-148">It's possible to use a custom service provider instead of the default service provider added by the default template.</span></span> <span data-ttu-id="96a23-149">Niestandardowe usługodawcy automatycznie nie dostarcza usługi domyślne wymienione w tabeli.</span><span class="sxs-lookup"><span data-stu-id="96a23-149">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="96a23-150">Jeśli używasz niestandardowego usługodawcy i wymaga żadnej usług przedstawionych w tabeli, Dodaj wymagane usługi, do nowego dostawcę usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-150">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="96a23-151">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="96a23-151">Request a service in a component</span></span>

<span data-ttu-id="96a23-152">Po dodaniu usługi do kolekcji usługi wstrzyknąć usług do składników programu szablony Razor przy użyciu [ @inject ](xref:mvc/views/razor#section-4) dyrektywy Razor.</span><span class="sxs-lookup"><span data-stu-id="96a23-152">After services are added to the service collection, inject the services into the components' Razor templates using the [@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="96a23-153">`@inject` ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="96a23-153">`@inject` has two parameters:</span></span>

* <span data-ttu-id="96a23-154">Nazwa typu: Typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="96a23-154">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="96a23-155">Nazwa właściwości: Nazwa właściwości odbieranie usługi wprowadzonego kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="96a23-155">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="96a23-156">Należy pamiętać, że właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="96a23-156">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="96a23-157">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="96a23-157">The compiler creates the property.</span></span>

<span data-ttu-id="96a23-158">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="96a23-158">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="96a23-159">Używanych jest wiele `@inject` instrukcje, aby wstawić różnych usług.</span><span class="sxs-lookup"><span data-stu-id="96a23-159">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="96a23-160">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="96a23-160">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="96a23-161">Wdrażanie usługi `Services.IDataAccess` są wstrzykiwane do właściwości składnika `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="96a23-161">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="96a23-162">Należy zauważyć, jak kod jest wyłącznie przy użyciu `IDataAccess` abstrakcji:</span><span class="sxs-lookup"><span data-stu-id="96a23-162">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

<span data-ttu-id="96a23-163">Wewnętrznie, wygenerowana właściwość (`DataRepository`) zostanie nadany `InjectAttribute` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="96a23-163">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="96a23-164">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="96a23-164">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="96a23-165">Jeśli klasa bazowa jest wymagana dla składników i właściwości wprowadzonego są również wymagane dla klasy bazowej, `InjectAttribute` można ręcznie dodać:</span><span class="sxs-lookup"><span data-stu-id="96a23-165">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="96a23-166">Składniki pochodzące z klasy bazowej `@inject` dyrektywy nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="96a23-166">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="96a23-167">`InjectAttribute` Klasy bazowej jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="96a23-167">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="96a23-168">Wstrzykiwanie zależności w usługach</span><span class="sxs-lookup"><span data-stu-id="96a23-168">Dependency injection in services</span></span>

<span data-ttu-id="96a23-169">Złożone usługi może wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="96a23-169">Complex services might require additional services.</span></span> <span data-ttu-id="96a23-170">W poprzednim przykładem `DataAccess` mogą wymagać `HttpClient` domyślnej usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-170">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="96a23-171">`@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach.</span><span class="sxs-lookup"><span data-stu-id="96a23-171">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="96a23-172">*Iniekcji konstruktora* należy użyć zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="96a23-172">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="96a23-173">Wymagane usługi są dodawane, dodając parametry do konstruktora tej usługi.</span><span class="sxs-lookup"><span data-stu-id="96a23-173">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="96a23-174">Gdy wstrzykiwanie zależności tworzy usługę, rozpoznaje usług wymaga w Konstruktorze i udostępnia je w związku z tym.</span><span class="sxs-lookup"><span data-stu-id="96a23-174">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="96a23-175">Wymagania wstępne dotyczące iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="96a23-175">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="96a23-176">Musi to być jeden konstruktor, w której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności.</span><span class="sxs-lookup"><span data-stu-id="96a23-176">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="96a23-177">Pamiętaj, że dodatkowe parametry, które nie są objęte DI są dozwolone, jeśli są określone wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="96a23-177">Note that additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="96a23-178">Zastosowanie Konstruktor musi być *publicznych*.</span><span class="sxs-lookup"><span data-stu-id="96a23-178">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="96a23-179">Musi istnieć tylko jeden konstruktor dotyczy.</span><span class="sxs-lookup"><span data-stu-id="96a23-179">There must only be one applicable constructor.</span></span> <span data-ttu-id="96a23-180">W przypadku niejednoznaczności DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="96a23-180">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96a23-181">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="96a23-181">Additional resources</span></span>

* <span data-ttu-id="96a23-182">< xref:fundamentals / wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="96a23-182"><xref:fundamentals/dependency-injection</span></span>
* <xref:mvc/views/dependency-injection>
