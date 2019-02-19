---
title: Wstrzykiwanie zależności w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak platformy ASP.NET Core implementuje wstrzykiwanie zależności i jak z niej korzystać.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: 5e5b9746da9bbc13a147b807aabfd3d9ab90a0ca
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410511"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="90f43-103">Wstrzykiwanie zależności w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90f43-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="90f43-104">Przez [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="90f43-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="90f43-105">Platforma ASP.NET Core obsługuje zależności wzorzec projektowy oprogramowania iniekcji (DI), czyli technikę do osiągnięcia [Inwersja kontroli (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="90f43-106">Aby uzyskać więcej informacji specyficznych dla wstrzykiwanie zależności w ramach kontrolerów MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="90f43-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="90f43-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="90f43-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="90f43-108">Omówienie wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="90f43-108">Overview of dependency injection</span></span>

<span data-ttu-id="90f43-109">A *zależności* jest dowolny obiekt, który wymaga innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="90f43-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="90f43-110">Sprawdź następujące `MyDependency` klasy `WriteMessage` metody, które zależą od innych klas w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="90f43-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="90f43-111">Wystąpienie `MyDependency` klasy mogą być tworzone się `WriteMessage` klasy dostępnej metody.</span><span class="sxs-lookup"><span data-stu-id="90f43-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="90f43-112">`MyDependency` Klasa jest zależą od elementu `IndexModel` klasy:</span><span class="sxs-lookup"><span data-stu-id="90f43-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="90f43-113">Wystąpienie `MyDependency` klasy mogą być tworzone się `WriteMessage` klasy dostępnej metody.</span><span class="sxs-lookup"><span data-stu-id="90f43-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="90f43-114">`MyDependency` Klasa jest zależą od elementu `HomeController` klasy:</span><span class="sxs-lookup"><span data-stu-id="90f43-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _dependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="90f43-115">Klasa tworzy i zależy od bezpośrednio `MyDependency` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="90f43-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="90f43-116">Zależności w kodzie (na przykład w poprzednim przykładzie) są problematyczne i należy unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="90f43-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="90f43-117">Aby zastąpić `MyDependency` z inną implementacją klasy muszą zostać zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="90f43-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="90f43-118">Jeśli `MyDependency` ma zależności, musi być skonfigurowany przez klasę.</span><span class="sxs-lookup"><span data-stu-id="90f43-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="90f43-119">W dużym projekcie z wieloma klasami w zależności od `MyDependency`, kod konfiguracji staje się znajdują się na aplikację.</span><span class="sxs-lookup"><span data-stu-id="90f43-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="90f43-120">Ta implementacja jest trudny do testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="90f43-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="90f43-121">Aplikację należy użyć pozorny lub namiastki `MyDependency` klasy, która nie jest możliwe w przypadku tej metody.</span><span class="sxs-lookup"><span data-stu-id="90f43-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="90f43-122">Wstrzykiwanie zależności rozwiązuje te problemy za pomocą:</span><span class="sxs-lookup"><span data-stu-id="90f43-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="90f43-123">Użycie interfejsu tworzących warstwę abstrakcji implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="90f43-124">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="90f43-125">Platforma ASP.NET Core zapewnia kontener wbudowanej usługi [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="90f43-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="90f43-126">Usługi są zarejestrowane w usłudze aplikacji `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="90f43-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="90f43-127">*Iniekcja* usługi do konstruktora klasy, w których jest używany.</span><span class="sxs-lookup"><span data-stu-id="90f43-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="90f43-128">Struktura przejmuje odpowiedzialność za tworzenie wystąpienia zależności i usuwania je, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="90f43-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="90f43-129">W [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` interfejs definiuje metodę, która udostępnia usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="90f43-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="90f43-130">Ten interfejs jest implementowany przez konkretny typ `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="90f43-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="90f43-131">`MyDependency` żądania [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="90f43-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="90f43-132">Nie jest niczym niezwykłym używać wstrzykiwanie zależności w sposób połączonych.</span><span class="sxs-lookup"><span data-stu-id="90f43-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="90f43-133">Poszczególne zależności żądanego żądań z kolei swoje własne zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="90f43-134">Jest rozpoznawana jako zależności na wykresie i zwraca w pełni rozpoznać usługę kontenera.</span><span class="sxs-lookup"><span data-stu-id="90f43-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="90f43-135">Zbiorczy zestaw zależności, które muszą być rozwiązane jest zwykle nazywany *drzewo zależności*, *wykres zależności*, lub *wykresu obiektu*.</span><span class="sxs-lookup"><span data-stu-id="90f43-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="90f43-136">`IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowana w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="90f43-137">`IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="90f43-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="90f43-138">`ILogger<TCategoryName>` jest on zarejestrowany infrastruktury abstrakcje rejestrowanie, dlatego ma [usługi dostarczane przez framework](#framework-provided-services) zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="90f43-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="90f43-139">W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana przy użyciu konkretnego typu `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="90f43-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="90f43-140">Rejestracja zakresów okres istnienia usługi przez cały czas trwania pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="90f43-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="90f43-141">[Okresy istnienia usługi](#service-lifetimes) są opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="90f43-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="90f43-142">Każdy `services.Add{SERVICE_NAME}` — metoda rozszerzenia dodaje (i potencjalnie konfiguruje) usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-142">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="90f43-143">Na przykład `services.AddMvc()` dodaje usług, stronami Razor i wymagają MVC.</span><span class="sxs-lookup"><span data-stu-id="90f43-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="90f43-144">Zaleca się, że aplikacje stosują taką Konwencję.</span><span class="sxs-lookup"><span data-stu-id="90f43-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="90f43-145">Metody rozszerzające w miejscu [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) przestrzeni nazw w celu hermetyzacji grupy rejestracji usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="90f43-146">Jeśli Konstruktor usługi wymaga elementu podstawowego, takich jak `string`, element pierwotny może wprowadzone za pomocą [konfiguracji](xref:fundamentals/configuration/index) lub [wzorzec opcje](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="90f43-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="90f43-147">Za pośrednictwem konstruktora klasy, gdzie usługa jest używana i przypisane do pola prywatnego, wymagane jest wystąpienie usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="90f43-148">Pole jest używane do uzyskania dostępu do usługi zgodnie z potrzebami w całej klasy.</span><span class="sxs-lookup"><span data-stu-id="90f43-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="90f43-149">W przykładowej aplikacji `IMyDependency` wystąpienie jest wymagane i używane do wywołania tej usługi `WriteMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="90f43-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="90f43-150">Dostarczone do struktury usługi</span><span class="sxs-lookup"><span data-stu-id="90f43-150">Framework-provided services</span></span>

<span data-ttu-id="90f43-151">`Startup.ConfigureServices` Metoda jest odpowiedzialna definiowanie usługi, którego korzysta aplikacja, łącznie z funkcjami platformy, takie jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="90f43-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="90f43-152">Początkowo `IServiceCollection` udostępniane `ConfigureServices` ma następujące usługi, które są zdefiniowane (w zależności od [konfiguracji hosta](xref:fundamentals/host/index)):</span><span class="sxs-lookup"><span data-stu-id="90f43-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="90f43-153">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="90f43-153">Service Type</span></span> | <span data-ttu-id="90f43-154">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="90f43-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="90f43-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="90f43-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="90f43-156">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="90f43-156">Transient</span></span> |
| [<span data-ttu-id="90f43-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="90f43-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="90f43-158">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-158">Singleton</span></span> |
| [<span data-ttu-id="90f43-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="90f43-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="90f43-160">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-160">Singleton</span></span> |
| [<span data-ttu-id="90f43-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="90f43-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="90f43-162">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-162">Singleton</span></span> |
| [<span data-ttu-id="90f43-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="90f43-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="90f43-164">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="90f43-164">Transient</span></span> |
| [<span data-ttu-id="90f43-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="90f43-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="90f43-166">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-166">Singleton</span></span> |
| [<span data-ttu-id="90f43-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="90f43-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="90f43-168">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="90f43-168">Transient</span></span> |
| [<span data-ttu-id="90f43-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="90f43-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="90f43-170">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-170">Singleton</span></span> |
| [<span data-ttu-id="90f43-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="90f43-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="90f43-172">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-172">Singleton</span></span> |
| [<span data-ttu-id="90f43-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="90f43-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="90f43-174">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-174">Singleton</span></span> |
| [<span data-ttu-id="90f43-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="90f43-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="90f43-176">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="90f43-176">Transient</span></span> |
| [<span data-ttu-id="90f43-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="90f43-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="90f43-178">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-178">Singleton</span></span> |
| [<span data-ttu-id="90f43-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="90f43-179">System.Diagnostics.DiagnosticSource</span></span>](/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="90f43-180">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-180">Singleton</span></span> |
| [<span data-ttu-id="90f43-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="90f43-181">System.Diagnostics.DiagnosticListener</span></span>](/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="90f43-182">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="90f43-182">Singleton</span></span> |

<span data-ttu-id="90f43-183">Po udostępnieniu register a service (i jej usługi zależne, jeśli jest to wymagane) metody rozszerzenia kolekcji usługi Konwencji jest użycie pojedynczego `Add{SERVICE_NAME}` metodę rozszerzenia, aby zarejestrować wszystkich usług wymaganych przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="90f43-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="90f43-184">Poniższy kod jest przykładem sposobu dodawania dodatkowych usług do kontenera przy użyciu metody rozszerzenia [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), i [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span><span class="sxs-lookup"><span data-stu-id="90f43-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="90f43-185">Aby uzyskać więcej informacji, zobacz [klasy ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="90f43-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="90f43-186">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="90f43-186">Service lifetimes</span></span>

<span data-ttu-id="90f43-187">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="90f43-188">Usługi ASP.NET Core mogą być skonfigurowane przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="90f43-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="90f43-189">**Przejściowe**</span><span class="sxs-lookup"><span data-stu-id="90f43-189">**Transient**</span></span>

<span data-ttu-id="90f43-190">Przejściowych okres istnienia usługi są tworzone w każdym razem, gdy są one wymagane.</span><span class="sxs-lookup"><span data-stu-id="90f43-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="90f43-191">Ten okres istnienia najlepiej uproszczone, bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="90f43-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="90f43-192">**O określonym zakresie**</span><span class="sxs-lookup"><span data-stu-id="90f43-192">**Scoped**</span></span>

<span data-ttu-id="90f43-193">Okres istnienia w zakresie usług są tworzone raz na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="90f43-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="90f43-194">Korzystając z usługi o określonym zakresie w oprogramowaniu pośredniczącym, wprowadzić usługę do `Invoke` lub `InvokeAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="90f43-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="90f43-195">Nie wstrzyknąć przy użyciu iniekcji konstruktora, ponieważ wymusza usługę, aby zachowywać się jak wzorzec singleton.</span><span class="sxs-lookup"><span data-stu-id="90f43-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="90f43-196">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="90f43-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="90f43-197">**pojedyncze**</span><span class="sxs-lookup"><span data-stu-id="90f43-197">**Singleton**</span></span>

<span data-ttu-id="90f43-198">Pojedyncze okres istnienia usługi są tworzone po raz pierwszy masz żądanej (lub gdy `ConfigureServices` jest uruchamiany i wystąpienie jest określony za pomocą rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="90f43-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="90f43-199">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="90f43-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="90f43-200">Jeśli aplikacja wymaga pojedynczego zachowanie, umożliwiając kontener usługi zarządzać okresem istnienia usługi jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="90f43-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="90f43-201">Nie implementuje wzorzec projektowy pojedyncze i podać kod użytkownika do zarządzania okres istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="90f43-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="90f43-202">Niebezpiecznie usługi o określonym zakresie z pojedynczego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="90f43-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="90f43-203">Może to spowodować usługi, aby nieprawidłowym stanie podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="90f43-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="90f43-204">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="90f43-204">Constructor injection behavior</span></span>

<span data-ttu-id="90f43-205">Usługi można rozwiązać przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="90f43-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="90f43-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; pozwala na tworzenie obiektów bez rejestracji usługi w kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="90f43-207">`ActivatorUtilities` jest używana z abstrakcji widocznych dla użytkownika, takich jak pomocnicy tagów, integratorów modeli i kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="90f43-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="90f43-208">Konstruktory może akceptować argumenty, które nie są dostarczane przez wstrzykiwanie zależności, ale argumenty należy przypisać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="90f43-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="90f43-209">Gdy usługi są rozwiązywane przez `IServiceProvider` lub `ActivatorUtilities`, wymaga iniekcji konstruktora *publicznych* konstruktora.</span><span class="sxs-lookup"><span data-stu-id="90f43-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="90f43-210">Gdy usługi są rozwiązywane przez `ActivatorUtilities`, iniekcji konstruktora wymaga, że tylko jeden konstruktor dotyczy istnieje.</span><span class="sxs-lookup"><span data-stu-id="90f43-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="90f43-211">Konstruktor przeciążenia są obsługiwane, ale może istnieć tylko jednego przeciążenia, której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="90f43-212">Entity Framework kontekstów</span><span class="sxs-lookup"><span data-stu-id="90f43-212">Entity Framework contexts</span></span>

<span data-ttu-id="90f43-213">Entity Framework kontekstów powinna być dodana do kontenera usługi przy użyciu zakresu okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="90f43-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="90f43-214">Jest to obsługiwane automatycznie przy użyciu wywołania do [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) metody podczas rejestrowania kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90f43-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="90f43-215">Usługi, które używa kontekstu bazy danych należy używać również o określonym zakresie okresu istnienia.</span><span class="sxs-lookup"><span data-stu-id="90f43-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="90f43-216">Opcje okres istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="90f43-216">Lifetime and registration options</span></span>

<span data-ttu-id="90f43-217">Aby zademonstrować różnica między opcjami okres istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacja o unikatowym identyfikatorze `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="90f43-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="90f43-218">W zależności od tego, jak okres istnienia usługi operations jest skonfigurowany dla następujących interfejsów kontener zawiera takie same lub inne wystąpienie usługi zleconą przez klasę:</span><span class="sxs-lookup"><span data-stu-id="90f43-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="90f43-219">Interfejsy są implementowane w `Operation` klasy.</span><span class="sxs-lookup"><span data-stu-id="90f43-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="90f43-220">`Operation` Konstruktor generuje identyfikator GUID, jeśli jeden nie został dostarczony:</span><span class="sxs-lookup"><span data-stu-id="90f43-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="90f43-221">`OperationService` Jest zarejestrowany, zależy od wszystkich innych `Operation` typów.</span><span class="sxs-lookup"><span data-stu-id="90f43-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="90f43-222">Gdy `OperationService` żądania za pomocą iniekcji zależności odbierze nowe wystąpienie klasy poszczególnych usług albo istniejącego wystąpienia oparte na okres istnienia usług zależnych.</span><span class="sxs-lookup"><span data-stu-id="90f43-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="90f43-223">Jeśli przejściowy usługi są tworzone, gdy żądanie, `OperationId` z `IOperationTransient` usługi jest inny niż `OperationId` z `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="90f43-223">If transient services are created when requested, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="90f43-224">`OperationService` odbiera nowe wystąpienie klasy `IOperationTransient` klasy.</span><span class="sxs-lookup"><span data-stu-id="90f43-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="90f43-225">Nowe wystąpienie daje innym `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="90f43-225">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="90f43-226">Jeśli usługi o określonym zakresie są tworzone na żądanie, `OperationId` z `IOperationScoped` usługi jest taka sama jak w przypadku `OperationService` w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="90f43-226">If scoped services are created per request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="90f43-227">Żądań, obie usługi udostępniania innym `OperationId` wartość.</span><span class="sxs-lookup"><span data-stu-id="90f43-227">Across requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="90f43-228">Jeśli utworzono raz i stosować w przypadku wszystkich żądań i wszystkie usługi singleton i pojedyncze wystąpienie usługi `OperationId` jest stały we wszystkich żądań obsługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="90f43-229">W `Startup.ConfigureServices`, każdego typu jest dodawany do kontenera, zgodnie z jego nazwany okres istnienia:</span><span class="sxs-lookup"><span data-stu-id="90f43-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="90f43-230">`IOperationSingletonInstance` Usługa korzysta z określonego wystąpienia o identyfikatorze znanych `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="90f43-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="90f43-231">Może to być oczywiste, gdy ten typ jest w użyciu (jego identyfikator GUID jest zer).</span><span class="sxs-lookup"><span data-stu-id="90f43-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="90f43-232">Przykładowa aplikacja pokazuje czasów istnienia obiektów wewnątrz i pomiędzy poszczególnych żądań.</span><span class="sxs-lookup"><span data-stu-id="90f43-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="90f43-233">Przykładowa aplikacja `IndexModel` żądań każdego rodzaju `IOperation` typu i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="90f43-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="90f43-234">Na stronie zostanie wyświetlony, wszystkie klasy modelu strony i usługi `OperationId` wartości za pomocą przypisania właściwości:</span><span class="sxs-lookup"><span data-stu-id="90f43-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="90f43-235">Przykładowa aplikacja pokazuje czasów istnienia obiektów wewnątrz i pomiędzy poszczególnych żądań.</span><span class="sxs-lookup"><span data-stu-id="90f43-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="90f43-236">Przykładowa aplikacja zawiera `OperationsController` że żądania każdego rodzaju `IOperation` typu i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="90f43-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="90f43-237">`Index` Akcji ustawia usług do `ViewBag` do wyświetlenia usługi `OperationId` wartości:</span><span class="sxs-lookup"><span data-stu-id="90f43-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="90f43-238">Następujące dwa dane wyjściowe wyświetla wyniki dwa żądania:</span><span class="sxs-lookup"><span data-stu-id="90f43-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="90f43-239">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="90f43-239">**First request:**</span></span>

<span data-ttu-id="90f43-240">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="90f43-240">Controller operations:</span></span>

<span data-ttu-id="90f43-241">Przejściowy: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="90f43-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="90f43-242">O określonym zakresie: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="90f43-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="90f43-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="90f43-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="90f43-244">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="90f43-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="90f43-245">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="90f43-245">`OperationService` operations:</span></span>

<span data-ttu-id="90f43-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="90f43-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="90f43-247">O określonym zakresie: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="90f43-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="90f43-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="90f43-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="90f43-249">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="90f43-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="90f43-250">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="90f43-250">**Second request:**</span></span>

<span data-ttu-id="90f43-251">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="90f43-251">Controller operations:</span></span>

<span data-ttu-id="90f43-252">Przejściowy: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="90f43-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="90f43-253">O określonym zakresie: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="90f43-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="90f43-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="90f43-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="90f43-255">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="90f43-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="90f43-256">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="90f43-256">`OperationService` operations:</span></span>

<span data-ttu-id="90f43-257">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="90f43-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="90f43-258">O określonym zakresie: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="90f43-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="90f43-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="90f43-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="90f43-260">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="90f43-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="90f43-261">Sprawdź, które `OperationId` wartości różnią się w ramach żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="90f43-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="90f43-262">*Przejściowy* obiektów zawsze są różne.</span><span class="sxs-lookup"><span data-stu-id="90f43-262">*Transient* objects are always different.</span></span> <span data-ttu-id="90f43-263">Należy pamiętać, że przejściowego `OperationId` wartość dla pierwszego i drugiego żądania są różne dla obu `OperationService` operacje wielu żądań.</span><span class="sxs-lookup"><span data-stu-id="90f43-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="90f43-264">Nowe wystąpienie znajduje się na wszystkie usługi i żądania.</span><span class="sxs-lookup"><span data-stu-id="90f43-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="90f43-265">*Zakres* obiekty są takie same, w żądaniu, ale o różnych żądań.</span><span class="sxs-lookup"><span data-stu-id="90f43-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="90f43-266">*Pojedyncze* obiekty są takie same dla każdego obiektu, a każde żądanie, niezależnie od tego, czy `Operation` wystąpienie znajduje się w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="90f43-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="90f43-267">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="90f43-267">Call services from main</span></span>

<span data-ttu-id="90f43-268">Tworzenie [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) z [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) rozpoznawanie zakresu usługi w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90f43-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="90f43-269">Takie podejście jest przydatne do dostępu do usługi o określonym zakresie przy uruchamianiu do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="90f43-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="90f43-270">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="90f43-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="90f43-271">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="90f43-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="90f43-272">Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług wykonuje testy, aby sprawdzić, czy:</span><span class="sxs-lookup"><span data-stu-id="90f43-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="90f43-273">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="90f43-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="90f43-274">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="90f43-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="90f43-275">Dostawcy usług główny jest tworzone, gdy [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="90f43-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="90f43-276">Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90f43-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="90f43-277">Usługi o określonym zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="90f43-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="90f43-278">Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="90f43-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="90f43-279">Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="90f43-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="90f43-280">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="90f43-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="90f43-281">Żądanie usługi</span><span class="sxs-lookup"><span data-stu-id="90f43-281">Request Services</span></span>

<span data-ttu-id="90f43-282">Usługi dostępne w ramach platformy ASP.NET Core poprosić `HttpContext` są udostępniane za pośrednictwem [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) kolekcji.</span><span class="sxs-lookup"><span data-stu-id="90f43-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="90f43-283">Żądanie usługi reprezentują usługi skonfigurowane, a żądane w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90f43-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="90f43-284">Gdy obiekty określić zależności, są one spełnione przez typów znalezionych w `RequestServices`, a nie `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="90f43-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="90f43-285">Ogólnie rzecz biorąc aplikacja nie należy bezpośrednio używać tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="90f43-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="90f43-286">Zamiast tego żądania typy, klasy wymagają za pomocą konstruktorów klas i zezwolić na platformę iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="90f43-287">Daje to klasy, które są łatwiejsze do testowania.</span><span class="sxs-lookup"><span data-stu-id="90f43-287">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="90f43-288">Preferuj żądania z zależności jako parametry konstruktora do uzyskiwania dostępu do `RequestServices` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="90f43-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="90f43-289">Projekt usług do wstrzykiwania zależności</span><span class="sxs-lookup"><span data-stu-id="90f43-289">Design services for dependency injection</span></span>

<span data-ttu-id="90f43-290">Najlepsze rozwiązania są:</span><span class="sxs-lookup"><span data-stu-id="90f43-290">Best practices are to:</span></span>

* <span data-ttu-id="90f43-291">Projektowanie usług na potrzeby uzyskiwania ich zależności iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="90f43-292">Należy unikać wywołania metody statycznej, stanowe.</span><span class="sxs-lookup"><span data-stu-id="90f43-292">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="90f43-293">Należy unikać bezpośredniego wystąpienia klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="90f43-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="90f43-294">Bezpośrednie utworzenie wystąpienia couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="90f43-294">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="90f43-295">Małe, dobrze uwarunkowaną i łatwo przetestowane, należy utworzyć klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90f43-295">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="90f43-296">Jeśli klasa ma zbyt wiele zależności wprowadzonego, zwykle jest to znak, że klasa ma zbyt wiele obowiązki i narusza [pojedynczej odpowiedzialności zasady (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="90f43-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="90f43-297">Próba Refaktoryzuj klasy przez przeniesienie niektórych jego obowiązki do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="90f43-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="90f43-298">Należy pamiętać, który stron Razor strony modelu klasy i klas kontrolera MVC należy skoncentrować się na kwestie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="90f43-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="90f43-299">Business reguł oraz danych dostęp do szczegółów implementacji powinny być przechowywane w odpowiednich do tych klas [oddzielić wątpliwości](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="90f43-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="90f43-300">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="90f43-300">Disposal of services</span></span>

<span data-ttu-id="90f43-301">Wywołania kontenera `Dispose` dla `IDisposable` tworzy typy.</span><span class="sxs-lookup"><span data-stu-id="90f43-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="90f43-302">Jeśli wystąpienie zostanie dodany do kontenera przez kod użytkownika, nie są usuwane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="90f43-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="90f43-303">W programie ASP.NET Core 1.0 kontener wywołuje metodę dispose dla *wszystkich* `IDisposable` obiektów, w tym te nie zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="90f43-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="90f43-304">Domyślna usługa kontenera zastąpienia</span><span class="sxs-lookup"><span data-stu-id="90f43-304">Default service container replacement</span></span>

<span data-ttu-id="90f43-305">Kontener Wbudowane usługi jest przeznaczona do potrzebami ramach i większość aplikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="90f43-305">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="90f43-306">Zalecamy używanie kontenerze Wbudowane, chyba że potrzebujesz określonych funkcji, która nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="90f43-306">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="90f43-307">Niektóre z funkcji obsługiwanych w 3 kontenerach ze stron nie można odnaleźć w kontenerze Wbudowane:</span><span class="sxs-lookup"><span data-stu-id="90f43-307">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="90f43-308">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="90f43-308">Property injection</span></span>
* <span data-ttu-id="90f43-309">Iniekcja na podstawie nazwy</span><span class="sxs-lookup"><span data-stu-id="90f43-309">Injection based on name</span></span>
* <span data-ttu-id="90f43-310">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="90f43-310">Child containers</span></span>
* <span data-ttu-id="90f43-311">Zarządzanie okresem istnienia niestandardowe</span><span class="sxs-lookup"><span data-stu-id="90f43-311">Custom lifetime management</span></span>
* <span data-ttu-id="90f43-312">`Func<T>` Obsługa inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="90f43-312">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="90f43-313">Zobacz [pliku readme.md wstrzykiwanie zależności](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) listę niektórych kontenerów, które obsługują kart.</span><span class="sxs-lookup"><span data-stu-id="90f43-313">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="90f43-314">Poniższy przykład zastępuje wbudowanych kontenerów za pomocą [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="90f43-314">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="90f43-315">Zainstaluj pakiety odpowiedniego kontenera:</span><span class="sxs-lookup"><span data-stu-id="90f43-315">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="90f43-316">Autofac</span><span class="sxs-lookup"><span data-stu-id="90f43-316">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="90f43-317">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="90f43-317">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="90f43-318">Konfiguruj kontener w `Startup.ConfigureServices` i zwracają `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="90f43-318">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="90f43-319">Do użycia kontener firm 3 `Startup.ConfigureServices` musi zwracać `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="90f43-319">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="90f43-320">Konfigurowanie Autofac w `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="90f43-320">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="90f43-321">W czasie wykonywania Autofac umożliwia rozwiązanie typów oraz wstrzykiwania zależności.</span><span class="sxs-lookup"><span data-stu-id="90f43-321">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="90f43-322">Aby dowiedzieć się więcej o korzystaniu z Autofac za pomocą programu ASP.NET Core, zobacz [dokumentacji Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="90f43-322">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="90f43-323">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="90f43-323">Thread safety</span></span>

<span data-ttu-id="90f43-324">Usługami Singleton muszą być bezpieczne dla wątków.</span><span class="sxs-lookup"><span data-stu-id="90f43-324">Singleton services need to be thread safe.</span></span> <span data-ttu-id="90f43-325">Jeśli usługi singleton ma zależność od usługi przejściowy, przejściowe usługi również może być konieczne zapewniać bezpieczeństwo wątkowe zależności od sposobu ich wykorzystania przez wzorzec singleton.</span><span class="sxs-lookup"><span data-stu-id="90f43-325">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="90f43-326">Metoda fabryki pojedynczej usługi, takie jak drugi argument [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), nie musi być metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="90f43-326">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="90f43-327">Typ, takich jak (`static`) konstruktora, jego ma gwarantuje lze volat pouze jednou przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="90f43-327">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="90f43-328">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="90f43-328">Recommendations</span></span>

* <span data-ttu-id="90f43-329">`async/await` i `Task` rozpoznawanie na podstawie usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="90f43-329">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="90f43-330">C# nie obsługuje konstruktorów asynchroniczny, w związku z tym zalecany wzorzec jest użycie metod asynchronicznych po usunięciu synchronicznie usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-330">C# does not support asynchronous constructors, therefore the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="90f43-331">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-331">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="90f43-332">Na przykład koszyka użytkownika zwykle nie należy dodać do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="90f43-332">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="90f43-333">Należy użyć konfiguracji [wzorzec opcje](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="90f43-333">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="90f43-334">Podobnie należy unikać obiektów "symbol zastępczy danych", które istnieją tylko w celu umożliwienia dostępu do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="90f43-334">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="90f43-335">Jest lepszym rozwiązaniem rzeczywisty element za pośrednictwem DI żądania.</span><span class="sxs-lookup"><span data-stu-id="90f43-335">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="90f43-336">Unikaj statycznych dostęp do usług (na przykład statycznie — wpisanie [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) użytku innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="90f43-336">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="90f43-337">Unikaj używania *wzorzec lokalizatora usług*.</span><span class="sxs-lookup"><span data-stu-id="90f43-337">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="90f43-338">Na przykład nie wywołać <xref:System.IServiceProvider.GetService*> uzyskać wystąpienie usługi, gdy zamiast tego użyj DI.</span><span class="sxs-lookup"><span data-stu-id="90f43-338">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead.</span></span> <span data-ttu-id="90f43-339">Inna wersja locator service, aby uniknąć wprowadza fabryki, który jest rozpoznawany jako zależności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="90f43-339">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="90f43-340">Oba te rozwiązania mieszanego [Inwersja kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategii.</span><span class="sxs-lookup"><span data-stu-id="90f43-340">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="90f43-341">Unikaj statycznych dostęp do `HttpContext` (na przykład [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="90f43-341">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="90f43-342">Podobnie jak wszystkie zestawy zaleceń mogą wystąpić sytuacje, w których wymagane jest ignorowanie zaleceniem.</span><span class="sxs-lookup"><span data-stu-id="90f43-342">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="90f43-343">Wyjątki występują rzadko&mdash;przede wszystkim specjalne przypadki w samej strukturze.</span><span class="sxs-lookup"><span data-stu-id="90f43-343">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="90f43-344">DI jest *alternatywnych* do wzorce dostępu i statyczne/globalne obiektu.</span><span class="sxs-lookup"><span data-stu-id="90f43-344">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="90f43-345">Nie można korzystać z zalet DI, jeśli można łączyć z dostępem do obiektu statycznego.</span><span class="sxs-lookup"><span data-stu-id="90f43-345">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90f43-346">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="90f43-346">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="90f43-347">Pisanie czystego kodu w programie ASP.NET Core przy użyciu iniekcji zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="90f43-347">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="90f43-348">Projekt aplikacji zarządzanych przez kontener, Prelude: Gdzie jest należą kontenera?</span><span class="sxs-lookup"><span data-stu-id="90f43-348">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="90f43-349">Zasada jawne zależności</span><span class="sxs-lookup"><span data-stu-id="90f43-349">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="90f43-350">Inwersja kontroli kontenerów i wzorzec wstrzykiwanie zależności (Martina Fowlera)</span><span class="sxs-lookup"><span data-stu-id="90f43-350">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="90f43-351">Jak zarejestrować usługi z wieloma interfejsami w programie ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="90f43-351">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
