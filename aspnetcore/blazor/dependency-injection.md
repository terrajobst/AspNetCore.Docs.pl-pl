---
title: ASP.NET Core Blazor iniekcji zależności
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 859fd484fc00104575f176fa7d3bf752895475a0
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885502"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="ff055-103">ASP.NET Core iniekcja zależności Blazor</span><span class="sxs-lookup"><span data-stu-id="ff055-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="ff055-104">Autor [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="ff055-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="ff055-105">Blazor obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ff055-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ff055-106">Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu.</span><span class="sxs-lookup"><span data-stu-id="ff055-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="ff055-107">Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="ff055-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="ff055-108">DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="ff055-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="ff055-109">Może to być przydatne w aplikacjach Blazor do:</span><span class="sxs-lookup"><span data-stu-id="ff055-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="ff055-110">Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako *pojedyncze usługi.*</span><span class="sxs-lookup"><span data-stu-id="ff055-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="ff055-111">Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań.</span><span class="sxs-lookup"><span data-stu-id="ff055-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="ff055-112">Rozważmy na przykład `IDataAccess` interfejsu do uzyskiwania dostępu do danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ff055-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="ff055-113">Interfejs jest implementowany przez konkretną klasę `DataAccess` i zarejestrowany jako usługa w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ff055-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="ff055-114">Gdy składnik używa elementu DI do odbierania implementacji `IDataAccess`, składnik nie jest połączony z konkretnym typem.</span><span class="sxs-lookup"><span data-stu-id="ff055-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="ff055-115">Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="ff055-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="ff055-116">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="ff055-116">Default services</span></span>

<span data-ttu-id="ff055-117">Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ff055-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="ff055-118">NDES</span><span class="sxs-lookup"><span data-stu-id="ff055-118">Service</span></span> | <span data-ttu-id="ff055-119">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="ff055-119">Lifetime</span></span> | <span data-ttu-id="ff055-120">Opis</span><span class="sxs-lookup"><span data-stu-id="ff055-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="ff055-121">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="ff055-121">Singleton</span></span> | <span data-ttu-id="ff055-122">Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="ff055-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="ff055-123">Wystąpienie `HttpClient` w aplikacji webassembly Blazor używa przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="ff055-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="ff055-124">Aplikacje serwera Blazor nie domyślnie zawierają `HttpClient` skonfigurowany jako usługa.</span><span class="sxs-lookup"><span data-stu-id="ff055-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="ff055-125">Podaj `HttpClient` do aplikacji serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="ff055-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="ff055-126">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="ff055-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="ff055-127">Singleton (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="ff055-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="ff055-128">W zakresie (serwer Blazor)</span><span class="sxs-lookup"><span data-stu-id="ff055-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="ff055-129">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff055-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="ff055-130">Aby uzyskać więcej informacji, zobacz temat <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="ff055-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="ff055-131">Singleton (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="ff055-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="ff055-132">W zakresie (serwer Blazor)</span><span class="sxs-lookup"><span data-stu-id="ff055-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="ff055-133">Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji.</span><span class="sxs-lookup"><span data-stu-id="ff055-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="ff055-134">Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="ff055-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="ff055-135">Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli.</span><span class="sxs-lookup"><span data-stu-id="ff055-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="ff055-136">W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="ff055-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="ff055-137">Dodawanie usług do aplikacji</span><span class="sxs-lookup"><span data-stu-id="ff055-137">Add services to an app</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="ff055-138">Zestaw WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="ff055-138">Blazor WebAssembly</span></span>

<span data-ttu-id="ff055-139">Skonfiguruj usługi dla kolekcji usług aplikacji w `Main` metodzie *program.cs*.</span><span class="sxs-lookup"><span data-stu-id="ff055-139">Configure services for the app's service collection in the `Main` method of *Program.cs*.</span></span> <span data-ttu-id="ff055-140">W poniższym przykładzie zarejestrowano implementację `MyDependency` dla `IMyDependency`:</span><span class="sxs-lookup"><span data-stu-id="ff055-140">In the following example, the `MyDependency` implementation is registered for `IMyDependency`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="ff055-141">Po skompilowaniu hosta usługi można uzyskać dostęp z poziomu głównego DI zakresu przed renderowaniem wszystkich składników.</span><span class="sxs-lookup"><span data-stu-id="ff055-141">Once the host is built, services can be accessed from the root DI scope before any components are rendered.</span></span> <span data-ttu-id="ff055-142">Może to być przydatne do uruchamiania logiki inicjowania przed renderowaniem zawartości:</span><span class="sxs-lookup"><span data-stu-id="ff055-142">This can be useful for running initialization logic before rendering content:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

<span data-ttu-id="ff055-143">Host udostępnia również centralne wystąpienie konfiguracji dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ff055-143">The host also provides a central configuration instance for the app.</span></span> <span data-ttu-id="ff055-144">W poprzednim przykładzie adres URL usługi Pogoda jest przesyłany z domyślnego źródła konfiguracji (na przykład *appSettings. JSON*) do `InitializeWeatherAsync`:</span><span class="sxs-lookup"><span data-stu-id="ff055-144">Building on the preceding example, the weather service's URL is passed from a default configuration source (for example, *appsettings.json*) to `InitializeWeatherAsync`:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a><span data-ttu-id="ff055-145">Serwer Blazor</span><span class="sxs-lookup"><span data-stu-id="ff055-145">Blazor Server</span></span>

<span data-ttu-id="ff055-146">Po utworzeniu nowej aplikacji należy zapoznać się z `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="ff055-146">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="ff055-147">Metoda `ConfigureServices` jest przenoszona <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, która jest listą obiektów deskryptorów usługi (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="ff055-147">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="ff055-148">Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług.</span><span class="sxs-lookup"><span data-stu-id="ff055-148">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="ff055-149">Poniższy przykład ilustruje koncepcję z interfejsem `IDataAccess` i jego konkretną implementacją `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="ff055-149">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a><span data-ttu-id="ff055-150">Okres istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="ff055-150">Service lifetime</span></span>

<span data-ttu-id="ff055-151">Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="ff055-151">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="ff055-152">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="ff055-152">Lifetime</span></span> | <span data-ttu-id="ff055-153">Opis</span><span class="sxs-lookup"><span data-stu-id="ff055-153">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="ff055-154"> aplikacje webassembly nie mają obecnie koncepcji DI Scopes.</span><span class="sxs-lookup"><span data-stu-id="ff055-154"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="ff055-155">usługi zarejestrowane `Scoped`zachowują się jak usługi `Singleton` Services.</span><span class="sxs-lookup"><span data-stu-id="ff055-155">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="ff055-156">Jednak model hostingu serwera Blazor obsługuje okres istnienia `Scoped`.</span><span class="sxs-lookup"><span data-stu-id="ff055-156">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="ff055-157">W aplikacjach Blazor Server Rejestracja usługi w zakresie została objęta zakresem *połączenia*.</span><span class="sxs-lookup"><span data-stu-id="ff055-157">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="ff055-158">Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="ff055-158">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="ff055-159">DI tworzy *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="ff055-159">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="ff055-160">Wszystkie składniki wymagające usługi `Singleton` otrzymują wystąpienie tej samej usługi.</span><span class="sxs-lookup"><span data-stu-id="ff055-160">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="ff055-161">Za każdym razem, gdy składnik uzyskuje wystąpienie usługi `Transient` z kontenera usługi, otrzymuje *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="ff055-161">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="ff055-162">System DI jest oparty na systemie DI w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff055-162">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="ff055-163">Aby uzyskać więcej informacji, zobacz temat <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ff055-163">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="ff055-164">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="ff055-164">Request a service in a component</span></span>

<span data-ttu-id="ff055-165">Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą dyrektywy [wstrzykiwania Razor\@](xref:mvc/views/razor#inject) .</span><span class="sxs-lookup"><span data-stu-id="ff055-165">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="ff055-166">`@inject` ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="ff055-166">`@inject` has two parameters:</span></span>

* <span data-ttu-id="ff055-167">Wpisz &ndash; typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="ff055-167">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="ff055-168">Właściwość &ndash; nazwą właściwości otrzymującej wstrzykiwaną usługę App Service.</span><span class="sxs-lookup"><span data-stu-id="ff055-168">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="ff055-169">Właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="ff055-169">The property doesn't require manual creation.</span></span> <span data-ttu-id="ff055-170">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="ff055-170">The compiler creates the property.</span></span>

<span data-ttu-id="ff055-171">Aby uzyskać więcej informacji, zobacz temat <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="ff055-171">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="ff055-172">Użyj wielu instrukcji `@inject`, aby wstrzyknąć różne usługi.</span><span class="sxs-lookup"><span data-stu-id="ff055-172">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="ff055-173">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="ff055-173">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="ff055-174">Usługa implementująca `Services.IDataAccess` jest wstrzykiwana do `DataRepository`właściwości składnika.</span><span class="sxs-lookup"><span data-stu-id="ff055-174">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="ff055-175">Zwróć uwagę, jak kod używa wyłącznie abstrakcji `IDataAccess`:</span><span class="sxs-lookup"><span data-stu-id="ff055-175">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="ff055-176">Wewnętrznie wygenerowana Właściwość (`DataRepository`) używa atrybutu `InjectAttribute`.</span><span class="sxs-lookup"><span data-stu-id="ff055-176">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="ff055-177">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="ff055-177">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="ff055-178">Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, ręcznie Dodaj `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="ff055-178">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="ff055-179">W składnikach pochodnych z klasy bazowej dyrektywa `@inject` nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="ff055-179">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="ff055-180">`InjectAttribute` klasy podstawowej jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="ff055-180">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="ff055-181">Korzystanie z usług DI w</span><span class="sxs-lookup"><span data-stu-id="ff055-181">Use DI in services</span></span>

<span data-ttu-id="ff055-182">Złożone usługi mogą wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="ff055-182">Complex services might require additional services.</span></span> <span data-ttu-id="ff055-183">W poprzednim przykładzie `DataAccess` może wymagać `HttpClient` domyślnej usługi.</span><span class="sxs-lookup"><span data-stu-id="ff055-183">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="ff055-184">`@inject` (lub `InjectAttribute`) nie jest dostępny do użytku w usługach.</span><span class="sxs-lookup"><span data-stu-id="ff055-184">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="ff055-185">Zamiast tego należy użyć *iniekcji konstruktora* .</span><span class="sxs-lookup"><span data-stu-id="ff055-185">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="ff055-186">Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi.</span><span class="sxs-lookup"><span data-stu-id="ff055-186">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="ff055-187">Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="ff055-187">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="ff055-188">Wymagania wstępne dotyczące iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="ff055-188">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="ff055-189">Jeden Konstruktor musi istnieć, którego argumenty mogą być zrealizowane przez DI.</span><span class="sxs-lookup"><span data-stu-id="ff055-189">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="ff055-190">Dodatkowe parametry, które nie są objęte przez DI, są dozwolone, jeśli określają wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="ff055-190">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="ff055-191">Odpowiedni Konstruktor musi być *publiczny*.</span><span class="sxs-lookup"><span data-stu-id="ff055-191">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="ff055-192">Musi istnieć jeden odpowiedni Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ff055-192">One applicable constructor must exist.</span></span> <span data-ttu-id="ff055-193">W przypadku niejednoznaczności, polecenie DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="ff055-193">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="ff055-194">Klasy składników podstawowych narzędzi do zarządzania DI zakresem</span><span class="sxs-lookup"><span data-stu-id="ff055-194">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="ff055-195">W przypadku aplikacji ASP.NET Core usługi o określonym zakresie są zwykle objęte zakresem bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="ff055-195">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="ff055-196">Po zakończeniu żądania wszystkie usługi w zakresie lub przejściowym są usuwane przez system DI.</span><span class="sxs-lookup"><span data-stu-id="ff055-196">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="ff055-197">W Blazor aplikacji serwerowych zakres żądań jest stosowany przez czas trwania połączenia klienta, co może spowodować, że usługi przejściowe i objęte zakresem będą dużo dłużej niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="ff055-197">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="ff055-198">Aby ograniczyć zakres usług do okresu istnienia składnika, można użyć klas podstawowych `OwningComponentBase` i `OwningComponentBase<TService>`.</span><span class="sxs-lookup"><span data-stu-id="ff055-198">To scope services to the lifetime of a component, you can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="ff055-199">Te klasy bazowe uwidaczniają Właściwość `ScopedServices` typu `IServiceProvider`, które rozwiązują usługi objęte zakresem czasu istnienia składnika.</span><span class="sxs-lookup"><span data-stu-id="ff055-199">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="ff055-200">Aby utworzyć składnik, który dziedziczy z klasy podstawowej w Razor, użyj dyrektywy `@inherits`.</span><span class="sxs-lookup"><span data-stu-id="ff055-200">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```razor
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
> <span data-ttu-id="ff055-201">Usługi wprowadzone do składnika przy użyciu `@inject` lub `InjectAttribute` nie są tworzone w zakresie składnika i są powiązane z zakresem żądania.</span><span class="sxs-lookup"><span data-stu-id="ff055-201">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff055-202">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ff055-202">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
