---
title: Wstrzykiwanie zależności składników razor
author: guardrex
description: Zobacz, jak Blazor i składniki Razor aplikacje mogą używać usług wbudowanych Pozwól, które są wstrzykiwane do składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 6ce8fa74f20145f48797d267c20ef2593368b941
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159502"
---
# <a name="razor-components-dependency-injection"></a><span data-ttu-id="e4296-103">Wstrzykiwanie zależności składników razor</span><span class="sxs-lookup"><span data-stu-id="e4296-103">Razor Components dependency injection</span></span>

<span data-ttu-id="e4296-104">Przez [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="e4296-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="e4296-105">[Wstrzykiwanie zależności (DI)](/aspnet/core/fundamentals/dependency-injection) jest wbudowana.</span><span class="sxs-lookup"><span data-stu-id="e4296-105">[Dependency injection (DI)](/aspnet/core/fundamentals/dependency-injection) is built-in.</span></span> <span data-ttu-id="e4296-106">Aplikacje mogą używać usług wbudowanych Pozwól, które są wstrzykiwane do składników.</span><span class="sxs-lookup"><span data-stu-id="e4296-106">Apps can use built-in services by having them injected into components.</span></span> <span data-ttu-id="e4296-107">Aplikacje można zdefiniować niestandardowych usług i udostępnić je za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="e4296-107">Apps can also define custom services and make them available via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="e4296-108">Wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="e4296-108">Dependency injection</span></span>

<span data-ttu-id="e4296-109">DI to technika do uzyskiwania dostępu do usługi skonfigurowane w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="e4296-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="e4296-110">Może to być przydatne do:</span><span class="sxs-lookup"><span data-stu-id="e4296-110">This can be useful to:</span></span>

* <span data-ttu-id="e4296-111">Udostępnić pojedyncze wystąpienie klasy usługi między wiele składników (nazywane *pojedyncze* usługi).</span><span class="sxs-lookup"><span data-stu-id="e4296-111">Share a single instance of a service class across many components (known as a *singleton* service).</span></span>
* <span data-ttu-id="e4296-112">Odłączanie składników z określonej usługi konkretnych klas i odwoływać się tylko elementy abstrakcji.</span><span class="sxs-lookup"><span data-stu-id="e4296-112">Decouple components from particular concrete service classes and only reference abstractions.</span></span> <span data-ttu-id="e4296-113">Na przykład interfejs `IDataAccess` został zaimplementowany przez klasę konkretnych `DataAccess`.</span><span class="sxs-lookup"><span data-stu-id="e4296-113">For example, an interface `IDataAccess` is implemented by a concrete class `DataAccess`.</span></span> <span data-ttu-id="e4296-114">Gdy składnik używa DI do odbierania `IDataAccess` implementacji, składnik nie jest ściśle do konkretnego typu.</span><span class="sxs-lookup"><span data-stu-id="e4296-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="e4296-115">Wdrożenia można wymieniać, być może do implementację testową w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="e4296-115">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="e4296-116">DI system jest odpowiedzialne za dostarczanie wystąpienia usług dla składników.</span><span class="sxs-lookup"><span data-stu-id="e4296-116">The DI system is responsible for supplying instances of services to components.</span></span> <span data-ttu-id="e4296-117">DI rozwiązuje również rekursywnie zależności, aby uwierzytelnienia usługi może zależeć od dalszego usług.</span><span class="sxs-lookup"><span data-stu-id="e4296-117">DI also resolves dependencies recursively so that services themselves can depend on further services.</span></span> <span data-ttu-id="e4296-118">DI jest konfigurowany podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4296-118">DI is configured during startup of the app.</span></span> <span data-ttu-id="e4296-119">Przykład przedstawiono w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="e4296-119">An example is shown later in this topic.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="e4296-120">Dodawanie usług do DI</span><span class="sxs-lookup"><span data-stu-id="e4296-120">Add services to DI</span></span>

<span data-ttu-id="e4296-121">Po utworzeniu nowej aplikacji, należy zbadać `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="e4296-121">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="e4296-122">`ConfigureServices` Metody jest przekazywana [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), który znajduje się lista obiektów deskryptora usługi ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span><span class="sxs-lookup"><span data-stu-id="e4296-122">The `ConfigureServices` method is passed an [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), which is a list of service descriptor objects ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)).</span></span> <span data-ttu-id="e4296-123">Usługi są dodawane, zapewniając deskryptory usługi do kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="e4296-123">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="e4296-124">W poniższym przykładzie kodu pokazano pojęcia:</span><span class="sxs-lookup"><span data-stu-id="e4296-124">The following code sample demonstrates the concept:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="e4296-125">Usługi mogą być skonfigurowane przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="e4296-125">Services can be configured with the following lifetimes:</span></span>

| <span data-ttu-id="e4296-126">Metoda</span><span class="sxs-lookup"><span data-stu-id="e4296-126">Method</span></span>      | <span data-ttu-id="e4296-127">Opis</span><span class="sxs-lookup"><span data-stu-id="e4296-127">Description</span></span> |
| ----------- | ----------- |
| [<span data-ttu-id="e4296-128">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="e4296-128">Singleton</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | <span data-ttu-id="e4296-129">Tworzy DI *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="e4296-129">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="e4296-130">Wszystkie składniki, które wymagają zainstalowania tej usługi odbierać odwołanie do tego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="e4296-130">All components requiring this service receive a reference to this instance.</span></span> |
| [<span data-ttu-id="e4296-131">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="e4296-131">Transient</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | <span data-ttu-id="e4296-132">Zawsze, gdy składnik wymaga tej usługi, otrzymuje *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="e4296-132">Whenever a component requires this service, it receives a *new instance* of the service.</span></span> |
| [<span data-ttu-id="e4296-133">O określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="e4296-133">Scoped</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | <span data-ttu-id="e4296-134">Blazor po stronie klienta nie ma obecnie koncepcji DI zakresów.</span><span class="sxs-lookup"><span data-stu-id="e4296-134">Client-side Blazor doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="e4296-135">`Scoped` zachowuje się jak `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="e4296-135">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="e4296-136">Jednak obsługuje składniki programu ASP.NET Core Razor `Scoped` okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="e4296-136">However, ASP.NET Core Razor Components support the `Scoped` lifetime.</span></span> <span data-ttu-id="e4296-137">W składniku Razor rejestracji usługi o określonym zakresie jest ograniczony do połączenia.</span><span class="sxs-lookup"><span data-stu-id="e4296-137">In a Razor Component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="e4296-138">Z tego powodu przy użyciu usługi o określonym zakresie była preferowana dla usług, które powinien być ograniczony z bieżącym użytkownikiem (nawet, jeśli bieżącym celem jest do uruchomienia po stronie klienta w przeglądarce).</span><span class="sxs-lookup"><span data-stu-id="e4296-138">For this reason, using scoped services is preferred for services that should be scoped to the current user (even if the current intent is to run client-side in the browser).</span></span> |

<span data-ttu-id="e4296-139">DI system jest oparty na systemie DI, w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e4296-139">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="e4296-140">Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności w programie ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e4296-140">For more information, see [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span></span>

## <a name="default-services"></a><span data-ttu-id="e4296-141">Usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="e4296-141">Default services</span></span>

<span data-ttu-id="e4296-142">Usługi domyślne są automatycznie dodawane do kolekcji usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4296-142">Default services are automatically added to the service collection of an app.</span></span> <span data-ttu-id="e4296-143">W poniższej tabeli przedstawiono niektóre świadczonych usług przydatna.</span><span class="sxs-lookup"><span data-stu-id="e4296-143">The following table shows some of the useful default services provided.</span></span>

| <span data-ttu-id="e4296-144">Metoda</span><span class="sxs-lookup"><span data-stu-id="e4296-144">Method</span></span>       | <span data-ttu-id="e4296-145">Opis</span><span class="sxs-lookup"><span data-stu-id="e4296-145">Description</span></span> |
| ------------ | ----------- |
| [<span data-ttu-id="e4296-146">HttpClient</span><span class="sxs-lookup"><span data-stu-id="e4296-146">HttpClient</span></span>](/dotnet/api/system.net.http.httpclient) | <span data-ttu-id="e4296-147">Zawiera metody służące do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego z użyciem identyfikatora URI (pojedyncze wystąpienie).</span><span class="sxs-lookup"><span data-stu-id="e4296-147">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI (singleton).</span></span> <span data-ttu-id="e4296-148">Należy pamiętać, że to wystąpienie `HttpClient` korzysta z przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="e4296-148">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="e4296-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) automatycznie zostaje ustawiony poziom podstawowy prefiks identyfikatora URI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4296-149">[HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="e4296-150">`HttpClient` jest dostępna wyłącznie dla aplikacji Blazor po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e4296-150">`HttpClient` is only provided to client-side Blazor apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="e4296-151">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, do której może być wysyłane wywołania.</span><span class="sxs-lookup"><span data-stu-id="e4296-151">Represents an instance of a JavaScript runtime to which calls may be dispatched.</span></span> <span data-ttu-id="e4296-152">Aby uzyskać więcej informacji, zobacz <xref:razor-components/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="e4296-152">For more information, see <xref:razor-components/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="e4296-153">Pomocnicy do pracy ze stanem identyfikatory URI i nawigacji (pojedyncze wystąpienie).</span><span class="sxs-lookup"><span data-stu-id="e4296-153">Helpers for working with URIs and navigation state (singleton).</span></span> <span data-ttu-id="e4296-154">`IUriHelper` zapewnia obie aplikacje Blazor i ASP.NET Core Razor składniki po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="e4296-154">`IUriHelper` is provided to both client-side Blazor and ASP.NET Core Razor Components apps.</span></span> |

<span data-ttu-id="e4296-155">Należy zauważyć, że można użyć dostawcy niestandardowych usług zamiast domyślnego dostawcę usługi, który jest dodawany przez szablon domyślny.</span><span class="sxs-lookup"><span data-stu-id="e4296-155">Note that it is possible to use a custom services provider instead of the default service provider that's added by the default template.</span></span> <span data-ttu-id="e4296-156">Niestandardowe usługodawcy automatycznie nie dostarcza usługi domyślne wymienione w tabeli.</span><span class="sxs-lookup"><span data-stu-id="e4296-156">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="e4296-157">Tych usług należy jawnie dodać do nowego dostawcę usługi.</span><span class="sxs-lookup"><span data-stu-id="e4296-157">Those services must be added to the new service provider explicitly.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="e4296-158">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="e4296-158">Request a service in a component</span></span>

<span data-ttu-id="e4296-159">Gdy usługi są dodawane do kolekcji usługi, może zostać wprowadzona do składników programu szablony Razor przy użyciu `@inject` dyrektywy Razor.</span><span class="sxs-lookup"><span data-stu-id="e4296-159">Once services are added to the service collection, they can be injected into the components' Razor templates using the `@inject` Razor directive.</span></span> <span data-ttu-id="e4296-160">`@inject` ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="e4296-160">`@inject` has two parameters:</span></span>

* <span data-ttu-id="e4296-161">Nazwa typu: Typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="e4296-161">Type name: The type of the service to inject.</span></span>
* <span data-ttu-id="e4296-162">Nazwa właściwości: Nazwa właściwości odbieranie usługi wprowadzonego kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e4296-162">Property name: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="e4296-163">Należy pamiętać, że właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="e4296-163">Note that the property doesn't require manual creation.</span></span> <span data-ttu-id="e4296-164">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="e4296-164">The compiler creates the property.</span></span>

<span data-ttu-id="e4296-165">Wiele `@inject` instrukcji można używać do dodania do różnych usług.</span><span class="sxs-lookup"><span data-stu-id="e4296-165">Multiple `@inject` statements can be used to inject different services.</span></span>

<span data-ttu-id="e4296-166">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="e4296-166">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="e4296-167">Wdrażanie usługi `Services.IDataAccess` są wstrzykiwane do właściwości składnika `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="e4296-167">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="e4296-168">Należy zauważyć, jak kod jest wyłącznie przy użyciu `IDataAccess` abstrakcji:</span><span class="sxs-lookup"><span data-stu-id="e4296-168">Note how the code is only using the `IDataAccess` abstraction:</span></span>

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

<span data-ttu-id="e4296-169">Wewnętrznie, wygenerowana właściwość (`DataRepository`) zostanie nadany `InjectAttribute` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e4296-169">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="e4296-170">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="e4296-170">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="e4296-171">Jeśli klasa bazowa jest wymagana dla składników i właściwości wprowadzonego są również wymagane dla klasy bazowej, `InjectAttribute` można ręcznie dodać:</span><span class="sxs-lookup"><span data-stu-id="e4296-171">If a base class is required for components and injected properties are also required for the base class, `InjectAttribute` can be manually added:</span></span>

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="e4296-172">Składniki pochodzące z klasy bazowej `@inject` dyrektywy nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="e4296-172">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="e4296-173">`InjectAttribute` Klasy bazowej jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="e4296-173">The `InjectAttribute` of the base class is sufficient:</span></span>

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="e4296-174">Wstrzykiwanie zależności w usługach</span><span class="sxs-lookup"><span data-stu-id="e4296-174">Dependency injection in services</span></span>

<span data-ttu-id="e4296-175">Złożone usługi może wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="e4296-175">Complex services might require additional services.</span></span> <span data-ttu-id="e4296-176">W poprzednim przykładem `DataAccess` mogą wymagać `HttpClient` domyślnej usługi.</span><span class="sxs-lookup"><span data-stu-id="e4296-176">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="e4296-177">`@inject` lub `InjectAttribute` nie mogą być używane w usługach.</span><span class="sxs-lookup"><span data-stu-id="e4296-177">`@inject` or the `InjectAttribute` can't be used in services.</span></span> <span data-ttu-id="e4296-178">*Iniekcji konstruktora* należy użyć zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="e4296-178">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="e4296-179">Wymagane usługi są dodawane, dodając parametry do konstruktora tej usługi.</span><span class="sxs-lookup"><span data-stu-id="e4296-179">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="e4296-180">Gdy wstrzykiwanie zależności tworzy usługę, rozpoznaje usług wymaga w Konstruktorze i udostępnia je w związku z tym.</span><span class="sxs-lookup"><span data-stu-id="e4296-180">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

<span data-ttu-id="e4296-181">W poniższym przykładzie kodu pokazano pojęcia:</span><span class="sxs-lookup"><span data-stu-id="e4296-181">The following code sample demonstrates the concept:</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

<span data-ttu-id="e4296-182">Należy zwrócić uwagę następujących wymagań wstępnych dotyczących iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="e4296-182">Note the following prerequisites for constructor injection:</span></span>

* <span data-ttu-id="e4296-183">Musi to być jeden konstruktor, w której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności.</span><span class="sxs-lookup"><span data-stu-id="e4296-183">There must be one constructor whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="e4296-184">Należy pamiętać, że dozwolone są dodatkowe parametry, które nie są objęte DI, jeśli określono dla nich wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="e4296-184">Note that additional parameters not covered by DI are allowed if default values are specified for them.</span></span>
* <span data-ttu-id="e4296-185">Zastosowanie Konstruktor musi być *publicznych*.</span><span class="sxs-lookup"><span data-stu-id="e4296-185">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="e4296-186">Musi istnieć tylko jeden konstruktor dotyczy.</span><span class="sxs-lookup"><span data-stu-id="e4296-186">There must only be one applicable constructor.</span></span> <span data-ttu-id="e4296-187">W przypadku niejednoznaczności DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="e4296-187">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4296-188">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e4296-188">Additional resources</span></span>

* [<span data-ttu-id="e4296-189">Wstrzykiwanie zależności w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4296-189">Dependency injection in ASP.NET Core</span></span>](/aspnet/core/fundamentals/dependency-injection)
