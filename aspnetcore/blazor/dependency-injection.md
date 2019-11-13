---
title: ASP.NET Core Blazor iniekcji zależności
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: a39d913636afc55ac9d831de923ba7ae8db1216b
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963077"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="6ff67-103">ASP.NET Core Blazor iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="6ff67-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="6ff67-104">Autor [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="6ff67-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="6ff67-105"> obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6ff67-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="6ff67-106">Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu.</span><span class="sxs-lookup"><span data-stu-id="6ff67-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="6ff67-107">Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="6ff67-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="6ff67-108">DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="6ff67-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="6ff67-109">Może to być przydatne w aplikacjach Blazor, aby:</span><span class="sxs-lookup"><span data-stu-id="6ff67-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="6ff67-110">Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako *pojedyncze usługi.*</span><span class="sxs-lookup"><span data-stu-id="6ff67-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="6ff67-111">Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań.</span><span class="sxs-lookup"><span data-stu-id="6ff67-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="6ff67-112">Rozważmy na przykład interfejs `IDataAccess` w celu uzyskania dostępu do danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ff67-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="6ff67-113">Interfejs jest implementowany przez konkretną klasę `DataAccess` i zarejestrowana jako usługa w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ff67-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="6ff67-114">Gdy składnik używa elementu DI do otrzymania implementacji `IDataAccess`, składnik nie jest połączony z konkretnym typem.</span><span class="sxs-lookup"><span data-stu-id="6ff67-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="6ff67-115">Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="6ff67-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="6ff67-116">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="6ff67-116">Default services</span></span>

<span data-ttu-id="6ff67-117">Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ff67-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="6ff67-118">Usługa</span><span class="sxs-lookup"><span data-stu-id="6ff67-118">Service</span></span> | <span data-ttu-id="6ff67-119">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="6ff67-119">Lifetime</span></span> | <span data-ttu-id="6ff67-120">Opis</span><span class="sxs-lookup"><span data-stu-id="6ff67-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="6ff67-121">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="6ff67-121">Singleton</span></span> | <span data-ttu-id="6ff67-122">Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="6ff67-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="6ff67-123">Należy zauważyć, że to wystąpienie `HttpClient` używa przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="6ff67-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="6ff67-124">[HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) jest automatycznie ustawiany na podstawowy prefiks identyfikatora URI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6ff67-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="6ff67-125">Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="6ff67-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="6ff67-126">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="6ff67-126">Singleton</span></span> | <span data-ttu-id="6ff67-127">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ff67-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="6ff67-128">Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="6ff67-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="6ff67-129">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="6ff67-129">Singleton</span></span> | <span data-ttu-id="6ff67-130">Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji.</span><span class="sxs-lookup"><span data-stu-id="6ff67-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="6ff67-131">Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="6ff67-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="6ff67-132">Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6ff67-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="6ff67-133">W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="6ff67-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="6ff67-134">Dodawanie usług do aplikacji</span><span class="sxs-lookup"><span data-stu-id="6ff67-134">Add services to an app</span></span>

<span data-ttu-id="6ff67-135">Po utworzeniu nowej aplikacji należy przejrzeć metodę `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6ff67-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="6ff67-136">Metoda `ConfigureServices` jest przenoszona do <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, który jest listą obiektów deskryptorów usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="6ff67-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="6ff67-137">Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług.</span><span class="sxs-lookup"><span data-stu-id="6ff67-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="6ff67-138">Poniższy przykład ilustruje koncepcję z interfejsem `IDataAccess` i jego konkretną implementacją `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="6ff67-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="6ff67-139">Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="6ff67-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="6ff67-140">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="6ff67-140">Lifetime</span></span> | <span data-ttu-id="6ff67-141">Opis</span><span class="sxs-lookup"><span data-stu-id="6ff67-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="6ff67-142"> aplikacje webassembly nie mają obecnie koncepcji DI Scopes.</span><span class="sxs-lookup"><span data-stu-id="6ff67-142"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="6ff67-143">usługi zarejestrowane `Scoped`zachowują się jak usługi `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="6ff67-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="6ff67-144">Jednak model hostingu serwera Blazor obsługuje okres istnienia `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-144">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="6ff67-145">W aplikacjach Blazor Server Rejestracja usługi w zakresie została objęta zakresem *połączenia*.</span><span class="sxs-lookup"><span data-stu-id="6ff67-145">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="6ff67-146">Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="6ff67-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="6ff67-147">DI tworzy *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="6ff67-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="6ff67-148">Wszystkie składniki wymagające usługi `Singleton` odbierają wystąpienie tej samej usługi.</span><span class="sxs-lookup"><span data-stu-id="6ff67-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="6ff67-149">Za każdym razem, gdy składnik uzyskuje wystąpienie usługi `Transient` z kontenera usług, otrzymuje *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="6ff67-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="6ff67-150">System DI jest oparty na systemie DI w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6ff67-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="6ff67-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="6ff67-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="6ff67-152">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="6ff67-152">Request a service in a component</span></span>

<span data-ttu-id="6ff67-153">Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą dyrektywy [wstrzykiwania Razor\@](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="6ff67-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="6ff67-154">`@inject` ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="6ff67-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="6ff67-155">Wpisz &ndash; typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="6ff67-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="6ff67-156">Właściwość &ndash; nazwa właściwości otrzymującej wstrzykiwaną usługę App Service.</span><span class="sxs-lookup"><span data-stu-id="6ff67-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="6ff67-157">Właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="6ff67-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="6ff67-158">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="6ff67-158">The compiler creates the property.</span></span>

<span data-ttu-id="6ff67-159">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="6ff67-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="6ff67-160">Użyj wielu instrukcji `@inject`, aby wstrzyknąć różne usługi.</span><span class="sxs-lookup"><span data-stu-id="6ff67-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="6ff67-161">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="6ff67-162">Usługa implementująca `Services.IDataAccess` jest wstrzykiwana do właściwości składnika `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="6ff67-163">Zwróć uwagę, jak kod używa wyłącznie abstrakcji `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="6ff67-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="6ff67-164">Wewnętrznie wygenerowana Właściwość (`DataRepository`) ma atrybut `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="6ff67-165">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="6ff67-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="6ff67-166">Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="6ff67-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="6ff67-167">W składnikach pochodnych klasy bazowej dyrektywa `@inject` nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="6ff67-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="6ff67-168">`InjectAttribute` klasy podstawowej jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="6ff67-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="6ff67-169">Korzystanie z usług DI w</span><span class="sxs-lookup"><span data-stu-id="6ff67-169">Use DI in services</span></span>

<span data-ttu-id="6ff67-170">Złożone usługi mogą wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="6ff67-170">Complex services might require additional services.</span></span> <span data-ttu-id="6ff67-171">W poprzednim przykładzie `DataAccess` może wymagać domyślnej usługi `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="6ff67-172">`@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach.</span><span class="sxs-lookup"><span data-stu-id="6ff67-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="6ff67-173">Zamiast tego należy użyć *iniekcji konstruktora* .</span><span class="sxs-lookup"><span data-stu-id="6ff67-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="6ff67-174">Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi.</span><span class="sxs-lookup"><span data-stu-id="6ff67-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="6ff67-175">Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="6ff67-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="6ff67-176">Wymagania wstępne dotyczące iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="6ff67-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="6ff67-177">Jeden Konstruktor musi istnieć, którego argumenty mogą być zrealizowane przez DI.</span><span class="sxs-lookup"><span data-stu-id="6ff67-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="6ff67-178">Dodatkowe parametry, które nie są objęte przez DI, są dozwolone, jeśli określają wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="6ff67-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="6ff67-179">Odpowiedni Konstruktor musi być *publiczny*.</span><span class="sxs-lookup"><span data-stu-id="6ff67-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="6ff67-180">Musi istnieć jeden odpowiedni Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="6ff67-180">One applicable constructor must exist.</span></span> <span data-ttu-id="6ff67-181">W przypadku niejednoznaczności, polecenie DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="6ff67-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="6ff67-182">Klasy składników podstawowych narzędzi do zarządzania DI zakresem</span><span class="sxs-lookup"><span data-stu-id="6ff67-182">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="6ff67-183">W przypadku aplikacji ASP.NET Core usługi o określonym zakresie są zwykle objęte zakresem bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="6ff67-183">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="6ff67-184">Po zakończeniu żądania wszystkie usługi w zakresie lub przejściowym są usuwane przez system DI.</span><span class="sxs-lookup"><span data-stu-id="6ff67-184">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="6ff67-185">W Blazor aplikacji serwerowych zakres żądań jest stosowany przez czas trwania połączenia klienta, co może spowodować, że usługi przejściowe i objęte zakresem będą dużo dłużej niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="6ff67-185">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="6ff67-186">Aby ograniczyć zakres usług do okresu istnienia składnika, można użyć klas podstawowych `OwningComponentBase` i `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-186">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="6ff67-187">Te klasy bazowe uwidaczniają Właściwość `ScopedServices` typu `IServiceProvider`, które rozwiązują usługi objęte zakresem czasu istnienia składnika.</span><span class="sxs-lookup"><span data-stu-id="6ff67-187">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="6ff67-188">Aby utworzyć składnik, który dziedziczy z klasy podstawowej w Razor, użyj dyrektywy `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="6ff67-188">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```cshtml
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="6ff67-189">Usługi wprowadzone do składnika przy użyciu `@inject` lub `InjectAttribute` nie są tworzone w zakresie składnika i są powiązane z zakresem żądania.</span><span class="sxs-lookup"><span data-stu-id="6ff67-189">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ff67-190">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6ff67-190">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
