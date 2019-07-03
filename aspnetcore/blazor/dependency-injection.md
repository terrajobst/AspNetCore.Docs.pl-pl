---
title: Wstrzykiwanie zależności programu ASP.NET Core Blazor
author: guardrex
description: Zobacz, jak aplikacje Blazor można wstawić usług do składników.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 394d99656ba52f6c561007121e63634c7cb84f37
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538473"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="419f1-103">Wstrzykiwanie zależności programu ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="419f1-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="419f1-104">Przez [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="419f1-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="419f1-105">Obsługuje Blazor [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="419f1-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="419f1-106">Aplikacje mogą używać wbudowanych usług przez iniekcję je na składniki.</span><span class="sxs-lookup"><span data-stu-id="419f1-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="419f1-107">Aplikacje można zdefiniować i zarejestrować niestandardowych usług i udostępnić je w całej aplikacji za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="419f1-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="419f1-108">DI to technika do uzyskiwania dostępu do usługi skonfigurowane w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="419f1-109">Może to być przydatne w aplikacjach Blazor:</span><span class="sxs-lookup"><span data-stu-id="419f1-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="419f1-110">Udostępnić pojedyncze wystąpienie klasy usługi w wielu składników, znane jako *pojedyncze* usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="419f1-111">Oddzielenie składników od klas konkretnych usług, za pomocą abstrakcje odwołania.</span><span class="sxs-lookup"><span data-stu-id="419f1-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="419f1-112">Na przykład, należy wziąć pod uwagę interfejs `IDataAccess` do uzyskiwania dostępu do danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="419f1-113">Interfejs jest implementowany przez konkretny `DataAccess` klasy i zarejestrowany jako usługa w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="419f1-114">Gdy składnik używa DI do odbierania `IDataAccess` implementacji, składnik nie jest ściśle do konkretnego typu.</span><span class="sxs-lookup"><span data-stu-id="419f1-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="419f1-115">Wdrożenia można wymieniać, być może uzyskać implementację testową w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="419f1-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="419f1-116">Usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="419f1-116">Default services</span></span>

<span data-ttu-id="419f1-117">Usługi domyślne są automatycznie dodawane do kolekcji usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="419f1-118">Usługa</span><span class="sxs-lookup"><span data-stu-id="419f1-118">Service</span></span> | <span data-ttu-id="419f1-119">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="419f1-119">Lifetime</span></span> | <span data-ttu-id="419f1-120">Opis</span><span class="sxs-lookup"><span data-stu-id="419f1-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="419f1-121">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="419f1-121">Singleton</span></span> | <span data-ttu-id="419f1-122">Zawiera metody służące do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego z użyciem identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="419f1-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="419f1-123">Należy pamiętać, że to wystąpienie `HttpClient` korzysta z przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="419f1-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="419f1-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) automatycznie zostaje ustawiony poziom podstawowy prefiks identyfikatora URI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="419f1-125">Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="419f1-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="419f1-126">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="419f1-126">Singleton</span></span> | <span data-ttu-id="419f1-127">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript gdzie wysłaniem wywołania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="419f1-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="419f1-128">Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="419f1-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="419f1-129">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="419f1-129">Singleton</span></span> | <span data-ttu-id="419f1-130">Zawiera pomocnicy do pracy ze stanem identyfikatory URI i nawigacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="419f1-131">Aby uzyskać więcej informacji, zobacz [identyfikator URI i nawigacji pomocników stanu](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="419f1-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="419f1-132">Niestandardowe usługodawcy automatycznie nie dostarcza usługi domyślne wymienione w tabeli.</span><span class="sxs-lookup"><span data-stu-id="419f1-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="419f1-133">Jeśli używasz niestandardowego usługodawcy i wymaga żadnej usług przedstawionych w tabeli, Dodaj wymagane usługi, do nowego dostawcę usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="419f1-134">Dodawanie usług do aplikacji</span><span class="sxs-lookup"><span data-stu-id="419f1-134">Add services to an app</span></span>

<span data-ttu-id="419f1-135">Po utworzeniu nowej aplikacji, należy zbadać `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="419f1-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="419f1-136">`ConfigureServices` Metody jest przekazywana <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, który znajduje się lista obiektów deskryptora usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="419f1-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="419f1-137">Usługi są dodawane, zapewniając deskryptory usługi do kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="419f1-138">W poniższym przykładzie zademonstrowano pojęcia z `IDataAccess` interfejsu i jego konkretną implementację `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="419f1-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="419f1-139">Usługi mogą być skonfigurowane przy użyciu okresy istnienia pokazano w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="419f1-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="419f1-140">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="419f1-140">Lifetime</span></span> | <span data-ttu-id="419f1-141">Opis</span><span class="sxs-lookup"><span data-stu-id="419f1-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="419f1-142">Po stronie klienta Blazor aktualnie nie ma koncepcji DI zakresów.</span><span class="sxs-lookup"><span data-stu-id="419f1-142">Blazor client-side doesn't currently have a concept of DI scopes.</span></span> <span data-ttu-id="419f1-143">`Scoped`-zarejestrowane usługi zachowują się jak `Singleton` usług.</span><span class="sxs-lookup"><span data-stu-id="419f1-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="419f1-144">Jednak model hostowania po stronie serwera obsługuje `Scoped` okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="419f1-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="419f1-145">W składniku Razor rejestracji usługi o określonym zakresie jest ograniczony do połączenia.</span><span class="sxs-lookup"><span data-stu-id="419f1-145">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="419f1-146">Z tego powodu przy użyciu usługi o określonym zakresie była preferowana dla usług, które powinien być ograniczony do bieżącego użytkownika, nawet jeśli bieżącym celem jest do uruchomienia po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="419f1-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="419f1-147">Tworzy DI *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="419f1-148">Wszystkie składniki wymagające `Singleton` usługa otrzymywać wystąpienia tej samej usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="419f1-149">Zawsze, gdy składnik uzyskuje wystąpienie `Transient` usługi z kontenera usługi odbiera *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="419f1-150">DI system jest oparty na systemie DI, w programie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="419f1-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="419f1-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="419f1-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="419f1-152">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="419f1-152">Request a service in a component</span></span>

<span data-ttu-id="419f1-153">Po dodaniu usługi do kolekcji usługi wstrzyknąć usług do składników za pomocą [ \@wstrzyknąć](xref:mvc/views/razor#section-4) dyrektywy Razor.</span><span class="sxs-lookup"><span data-stu-id="419f1-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="419f1-154">`@inject` ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="419f1-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="419f1-155">Typ &ndash; typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="419f1-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="419f1-156">Właściwość &ndash; nazwa właściwości odbieranie usługi wprowadzonego kodu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="419f1-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="419f1-157">Właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="419f1-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="419f1-158">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="419f1-158">The compiler creates the property.</span></span>

<span data-ttu-id="419f1-159">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="419f1-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="419f1-160">Używanych jest wiele `@inject` instrukcje, aby wstawić różnych usług.</span><span class="sxs-lookup"><span data-stu-id="419f1-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="419f1-161">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="419f1-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="419f1-162">Wdrażanie usługi `Services.IDataAccess` są wstrzykiwane do właściwości składnika `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="419f1-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="419f1-163">Należy zauważyć, jak kod jest wyłącznie przy użyciu `IDataAccess` abstrakcji:</span><span class="sxs-lookup"><span data-stu-id="419f1-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="419f1-164">Wewnętrznie, wygenerowana właściwość (`DataRepository`) zostanie nadany `InjectAttribute` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="419f1-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="419f1-165">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="419f1-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="419f1-166">Jeśli klasa bazowa jest wymagana dla składników i właściwości wprowadzonego są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="419f1-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="419f1-167">Składniki pochodzące z klasy bazowej `@inject` dyrektywy nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="419f1-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="419f1-168">`InjectAttribute` Klasy bazowej jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="419f1-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="419f1-169">Użyj DI w usługach</span><span class="sxs-lookup"><span data-stu-id="419f1-169">Use DI in services</span></span>

<span data-ttu-id="419f1-170">Złożone usługi może wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="419f1-170">Complex services might require additional services.</span></span> <span data-ttu-id="419f1-171">W poprzednim przykładem `DataAccess` mogą wymagać `HttpClient` domyślnej usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="419f1-172">`@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach.</span><span class="sxs-lookup"><span data-stu-id="419f1-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="419f1-173">*Iniekcji konstruktora* należy użyć zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="419f1-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="419f1-174">Wymagane usługi są dodawane, dodając parametry do konstruktora tej usługi.</span><span class="sxs-lookup"><span data-stu-id="419f1-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="419f1-175">Gdy DI tworzy usługę, rozpoznaje usług wymaga w Konstruktorze i udostępnia je w związku z tym.</span><span class="sxs-lookup"><span data-stu-id="419f1-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="419f1-176">Wymagania wstępne dotyczące iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="419f1-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="419f1-177">Jeden konstruktor musi istnieć, której argumenty mogą wszystkie zostać spełnione przez DI.</span><span class="sxs-lookup"><span data-stu-id="419f1-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="419f1-178">Dodatkowe parametry, które nie są objęte DI są dozwolone, gdy ich określanie wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="419f1-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="419f1-179">Zastosowanie Konstruktor musi być *publicznych*.</span><span class="sxs-lookup"><span data-stu-id="419f1-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="419f1-180">Musi istnieć jeden konstruktor dotyczy.</span><span class="sxs-lookup"><span data-stu-id="419f1-180">One applicable constructor must exist.</span></span> <span data-ttu-id="419f1-181">W przypadku niejednoznaczności DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="419f1-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="419f1-182">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="419f1-182">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
