---
title: ASP.NET Core Blazor iniekcji zależności
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: 17dd0f927064ae7c2b1e3e439fd93e2cb220a5a4
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879782"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="bc070-103">ASP.NET Core Blazor iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="bc070-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="bc070-104">Autor [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="bc070-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="bc070-105"> obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bc070-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bc070-106">Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu.</span><span class="sxs-lookup"><span data-stu-id="bc070-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="bc070-107">Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="bc070-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="bc070-108">DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="bc070-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="bc070-109">Może to być przydatne w aplikacjach Blazor, aby:</span><span class="sxs-lookup"><span data-stu-id="bc070-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="bc070-110">Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako *pojedyncze usługi.*</span><span class="sxs-lookup"><span data-stu-id="bc070-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="bc070-111">Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań.</span><span class="sxs-lookup"><span data-stu-id="bc070-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="bc070-112">Rozważmy na przykład `IDataAccess` interfejsu do uzyskiwania dostępu do danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bc070-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="bc070-113">Interfejs jest implementowany przez konkretną klasę `DataAccess` i zarejestrowany jako usługa w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bc070-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="bc070-114">Gdy składnik używa elementu DI do odbierania implementacji `IDataAccess`, składnik nie jest połączony z konkretnym typem.</span><span class="sxs-lookup"><span data-stu-id="bc070-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="bc070-115">Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="bc070-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="bc070-116">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="bc070-116">Default services</span></span>

<span data-ttu-id="bc070-117">Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bc070-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="bc070-118">NDES</span><span class="sxs-lookup"><span data-stu-id="bc070-118">Service</span></span> | <span data-ttu-id="bc070-119">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="bc070-119">Lifetime</span></span> | <span data-ttu-id="bc070-120">Opis</span><span class="sxs-lookup"><span data-stu-id="bc070-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="bc070-121">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="bc070-121">Singleton</span></span> | <span data-ttu-id="bc070-122">Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="bc070-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="bc070-123">Wystąpienie `HttpClient` w aplikacji Blazor webassembly używa przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="bc070-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="bc070-124">aplikacje serwera Blazor nie zawierają domyślnie `HttpClient` skonfigurowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="bc070-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="bc070-125">Podaj `HttpClient` do aplikacji Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="bc070-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="bc070-126">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="bc070-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="bc070-127">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="bc070-127">Singleton</span></span> | <span data-ttu-id="bc070-128">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bc070-128">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="bc070-129">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="bc070-129">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="bc070-130">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="bc070-130">Singleton</span></span> | <span data-ttu-id="bc070-131">Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji.</span><span class="sxs-lookup"><span data-stu-id="bc070-131">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="bc070-132">Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="bc070-132">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="bc070-133">Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli.</span><span class="sxs-lookup"><span data-stu-id="bc070-133">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="bc070-134">W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="bc070-134">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="bc070-135">Dodawanie usług do aplikacji</span><span class="sxs-lookup"><span data-stu-id="bc070-135">Add services to an app</span></span>

<span data-ttu-id="bc070-136">Po utworzeniu nowej aplikacji należy zapoznać się z `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="bc070-136">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="bc070-137">Metoda `ConfigureServices` jest przenoszona <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, która jest listą obiektów deskryptorów usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="bc070-137">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="bc070-138">Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług.</span><span class="sxs-lookup"><span data-stu-id="bc070-138">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="bc070-139">Poniższy przykład ilustruje koncepcję z interfejsem `IDataAccess` i jego konkretną implementacją `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="bc070-139">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="bc070-140">Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="bc070-140">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="bc070-141">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="bc070-141">Lifetime</span></span> | <span data-ttu-id="bc070-142">Opis</span><span class="sxs-lookup"><span data-stu-id="bc070-142">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="bc070-143"> aplikacje webassembly nie mają obecnie koncepcji DI Scopes.</span><span class="sxs-lookup"><span data-stu-id="bc070-143"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="bc070-144">usługi zarejestrowane `Scoped`zachowują się jak usługi `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="bc070-144">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="bc070-145">Jednak model hostingu serwera Blazor obsługuje okres istnienia `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="bc070-145">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="bc070-146">W aplikacjach Blazor Server Rejestracja usługi w zakresie została objęta zakresem *połączenia*.</span><span class="sxs-lookup"><span data-stu-id="bc070-146">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="bc070-147">Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="bc070-147">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="bc070-148">DI tworzy *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="bc070-148">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="bc070-149">Wszystkie składniki wymagające usługi `Singleton` otrzymują wystąpienie tej samej usługi.</span><span class="sxs-lookup"><span data-stu-id="bc070-149">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="bc070-150">Za każdym razem, gdy składnik uzyskuje wystąpienie usługi `Transient` z kontenera usługi, otrzymuje *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="bc070-150">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="bc070-151">System DI jest oparty na systemie DI w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc070-151">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="bc070-152">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="bc070-152">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="bc070-153">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="bc070-153">Request a service in a component</span></span>

<span data-ttu-id="bc070-154">Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą dyrektywy [wstrzykiwania Razor\@](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="bc070-154">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="bc070-155">`@inject` ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="bc070-155">`@inject` has two parameters:</span></span>

* <span data-ttu-id="bc070-156">Wpisz &ndash; typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="bc070-156">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="bc070-157">Właściwość &ndash; nazwą właściwości otrzymującej wstrzykiwaną usługę App Service.</span><span class="sxs-lookup"><span data-stu-id="bc070-157">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="bc070-158">Właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="bc070-158">The property doesn't require manual creation.</span></span> <span data-ttu-id="bc070-159">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="bc070-159">The compiler creates the property.</span></span>

<span data-ttu-id="bc070-160">Aby uzyskać więcej informacji, zobacz temat <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="bc070-160">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="bc070-161">Użyj wielu instrukcji `@inject`, aby wstrzyknąć różne usługi.</span><span class="sxs-lookup"><span data-stu-id="bc070-161">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="bc070-162">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="bc070-162">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="bc070-163">Usługa implementująca `Services.IDataAccess` jest wstrzykiwana do `DataRepository`właściwości składnika.</span><span class="sxs-lookup"><span data-stu-id="bc070-163">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="bc070-164">Zwróć uwagę, jak kod używa wyłącznie abstrakcji `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="bc070-164">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="bc070-165">Wewnętrznie wygenerowana Właściwość (`DataRepository`) używa atrybutu `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="bc070-165">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="bc070-166">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="bc070-166">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="bc070-167">Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="bc070-167">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="bc070-168">W składnikach pochodnych z klasy bazowej dyrektywa `@inject` nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="bc070-168">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="bc070-169">`InjectAttribute` klasy podstawowej jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="bc070-169">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="bc070-170">Korzystanie z usług DI w</span><span class="sxs-lookup"><span data-stu-id="bc070-170">Use DI in services</span></span>

<span data-ttu-id="bc070-171">Złożone usługi mogą wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="bc070-171">Complex services might require additional services.</span></span> <span data-ttu-id="bc070-172">W poprzednim przykładzie `DataAccess` może wymagać `HttpClient` domyślnej usługi.</span><span class="sxs-lookup"><span data-stu-id="bc070-172">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="bc070-173">`@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach.</span><span class="sxs-lookup"><span data-stu-id="bc070-173">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="bc070-174">Zamiast tego należy użyć *iniekcji konstruktora* .</span><span class="sxs-lookup"><span data-stu-id="bc070-174">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="bc070-175">Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi.</span><span class="sxs-lookup"><span data-stu-id="bc070-175">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="bc070-176">Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="bc070-176">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="bc070-177">Wymagania wstępne dotyczące iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="bc070-177">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="bc070-178">Jeden Konstruktor musi istnieć, którego argumenty mogą być zrealizowane przez DI.</span><span class="sxs-lookup"><span data-stu-id="bc070-178">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="bc070-179">Dodatkowe parametry, które nie są objęte przez DI, są dozwolone, jeśli określają wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="bc070-179">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="bc070-180">Odpowiedni Konstruktor musi być *publiczny*.</span><span class="sxs-lookup"><span data-stu-id="bc070-180">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="bc070-181">Musi istnieć jeden odpowiedni Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="bc070-181">One applicable constructor must exist.</span></span> <span data-ttu-id="bc070-182">W przypadku niejednoznaczności, polecenie DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="bc070-182">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="bc070-183">Klasy składników podstawowych narzędzi do zarządzania DI zakresem</span><span class="sxs-lookup"><span data-stu-id="bc070-183">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="bc070-184">W przypadku aplikacji ASP.NET Core usługi o określonym zakresie są zwykle objęte zakresem bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="bc070-184">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="bc070-185">Po zakończeniu żądania wszystkie usługi w zakresie lub przejściowym są usuwane przez system DI.</span><span class="sxs-lookup"><span data-stu-id="bc070-185">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="bc070-186">W Blazor aplikacji serwerowych zakres żądań jest stosowany przez czas trwania połączenia klienta, co może spowodować, że usługi przejściowe i objęte zakresem będą dużo dłużej niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="bc070-186">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="bc070-187">Aby zasięgać usługi do okresu istnienia składnika, można użyć klas podstawowych `OwningComponentBase` i `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="bc070-187">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="bc070-188">Te klasy bazowe uwidaczniają Właściwość `ScopedServices` typu `IServiceProvider`, które rozwiązują usługi objęte zakresem czasu istnienia składnika.</span><span class="sxs-lookup"><span data-stu-id="bc070-188">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="bc070-189">Aby utworzyć składnik, który dziedziczy z klasy podstawowej w Razor, użyj dyrektywy `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="bc070-189">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

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
> <span data-ttu-id="bc070-190">Usługi wprowadzone do składnika przy użyciu `@inject` lub `InjectAttribute` nie są tworzone w zakresie składnika i są powiązane z zakresem żądania.</span><span class="sxs-lookup"><span data-stu-id="bc070-190">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc070-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bc070-191">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
