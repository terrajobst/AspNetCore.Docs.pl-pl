---
title: Wstrzykiwanie zależności w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób ASP.NET Core implementuje iniekcję zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: c46e7322e86c2836a15bd0720995a8634bb185be
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634009"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="c11fd-103">Wstrzykiwanie zależności w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c11fd-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="c11fd-104">[Steve Kowalski](https://ardalis.com/), [Scott Addie](https://scottaddie.com)i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c11fd-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c11fd-105">ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia [niewersji kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.</span><span class="sxs-lookup"><span data-stu-id="c11fd-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="c11fd-106">Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="c11fd-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c11fd-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="c11fd-108">Przegląd iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="c11fd-108">Overview of dependency injection</span></span>

<span data-ttu-id="c11fd-109">*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="c11fd-110">Zapoznaj się z następującą klasą `MyDependency` przy użyciu metody `WriteMessage`, z której zależą inne klasy w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c11fd-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="c11fd-111">Wystąpienie klasy `MyDependency` można utworzyć w celu udostępnienia metody `WriteMessage` dla klasy.</span><span class="sxs-lookup"><span data-stu-id="c11fd-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="c11fd-112">Klasa `MyDependency` jest zależnością klasy `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="c11fd-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="c11fd-113">Klasa tworzy i bezpośrednio zależy od wystąpienia `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="c11fd-114">Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="c11fd-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="c11fd-115">Aby zastąpić `MyDependency` z inną implementacją, należy zmodyfikować klasę.</span><span class="sxs-lookup"><span data-stu-id="c11fd-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="c11fd-116">Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="c11fd-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="c11fd-117">W dużym projekcie z wieloma klasami, w zależności od `MyDependency`, kod konfiguracji zostanie rozłożona przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="c11fd-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="c11fd-118">Ta implementacja jest trudna do testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="c11fd-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="c11fd-119">Aplikacja powinna korzystać z klasy imitacji lub stub `MyDependency`, co nie jest możliwe w przypadku tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="c11fd-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="c11fd-120">Iniekcja zależności eliminuje te problemy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c11fd-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="c11fd-121">Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="c11fd-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="c11fd-122">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="c11fd-123">ASP.NET Core zawiera wbudowany kontener usługi <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="c11fd-124">Usługi są zarejestrowane w metodzie `Startup.ConfigureServices` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c11fd-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="c11fd-125">*Iniekcja* usługi do konstruktora klasy, w której jest używana.</span><span class="sxs-lookup"><span data-stu-id="c11fd-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="c11fd-126">Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="c11fd-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="c11fd-127">W [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)interfejs `IMyDependency` definiuje metodę dostarczaną przez usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c11fd-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c11fd-128">Ten interfejs jest implementowany przez konkretny typ, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="c11fd-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c11fd-129">`MyDependency` żąda <xref:Microsoft.Extensions.Logging.ILogger`1> w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="c11fd-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="c11fd-130">Użycie iniekcji zależności w łańcuchu nie jest nietypowe.</span><span class="sxs-lookup"><span data-stu-id="c11fd-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="c11fd-131">Każda żądana zależność z kolei żąda własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="c11fd-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="c11fd-132">Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="c11fd-133">Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.</span><span class="sxs-lookup"><span data-stu-id="c11fd-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="c11fd-134">w kontenerze usługi `IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="c11fd-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="c11fd-135">`IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c11fd-136">`ILogger<TCategoryName>` jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona przez platformę](#framework-provided-services) zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="c11fd-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="c11fd-137">Kontener rozwiązuje `ILogger<TCategoryName>` dzięki wykorzystaniu [typów otwartych (rodzajowych)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) typu konstruowanego](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="c11fd-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="c11fd-138">W przykładowej aplikacji usługa `IMyDependency` jest zarejestrowana z typem konkretnym `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="c11fd-139">Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="c11fd-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="c11fd-140">[Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="c11fd-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="c11fd-141">Każda metoda rozszerzenia `services.Add{SERVICE_NAME}` dodaje usługi (i potencjalnie konfiguruje).</span><span class="sxs-lookup"><span data-stu-id="c11fd-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="c11fd-142">Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="c11fd-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="c11fd-143">Zalecamy, aby aplikacje były zgodne z tą konwencją.</span><span class="sxs-lookup"><span data-stu-id="c11fd-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="c11fd-144">Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="c11fd-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="c11fd-145">Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="c11fd-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="c11fd-146">Wystąpienie usługi jest wymagane za pośrednictwem konstruktora klasy, w której jest używana usługa i jest przypisywana do pola prywatnego.</span><span class="sxs-lookup"><span data-stu-id="c11fd-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="c11fd-147">To pole jest używane w celu uzyskania dostępu do usługi w zależności od potrzeb w całej klasie.</span><span class="sxs-lookup"><span data-stu-id="c11fd-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="c11fd-148">W przykładowej aplikacji jest wymagane wystąpienie `IMyDependency` i używane do wywołania metody `WriteMessage` usługi:</span><span class="sxs-lookup"><span data-stu-id="c11fd-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

## <a name="services-injected-into-startup"></a><span data-ttu-id="c11fd-149">Usługi wstrzykiwane do uruchamiania</span><span class="sxs-lookup"><span data-stu-id="c11fd-149">Services injected into Startup</span></span>

<span data-ttu-id="c11fd-150">Tylko następujące typy usług można wstrzyknąć do konstruktora `Startup`, gdy jest używany host generyczny (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span><span class="sxs-lookup"><span data-stu-id="c11fd-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="c11fd-151">Usługi można wstrzyknąć do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c11fd-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="c11fd-152">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="c11fd-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="c11fd-153">Framework-provided services</span></span>

<span data-ttu-id="c11fd-154">Metoda `Startup.ConfigureServices` jest odpowiedzialna za Definiowanie usług, z których korzysta aplikacja, w tym funkcji platformy, takich jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c11fd-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="c11fd-155">Początkowo `IServiceCollection` zapewniane `ConfigureServices` ma usługi zdefiniowane przez platformę w zależności od [konfiguracji hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="c11fd-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="c11fd-156">Aplikacja oparta na ASP.NET Core szablonie nie jest nietypowa, aby mieć setki usług zarejestrowanych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="c11fd-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="c11fd-157">W poniższej tabeli wymieniono niewielki przykład usług zarejestrowanych w ramach platformy.</span><span class="sxs-lookup"><span data-stu-id="c11fd-157">A small sample of framework-registered services is listed in the following table.</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="c11fd-158">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="c11fd-158">Service Type</span></span> | <span data-ttu-id="c11fd-159">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="c11fd-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="c11fd-160">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="c11fd-161">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="c11fd-162">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="c11fd-163">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="c11fd-164">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="c11fd-165">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="c11fd-166">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="c11fd-167">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="c11fd-168">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="c11fd-169">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="c11fd-170">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="c11fd-171">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="c11fd-172">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="c11fd-173">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-173">Singleton</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="c11fd-174">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="c11fd-174">Service Type</span></span> | <span data-ttu-id="c11fd-175">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="c11fd-175">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="c11fd-176">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-176">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="c11fd-177">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-177">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="c11fd-178">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-178">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="c11fd-179">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-179">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="c11fd-180">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-180">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="c11fd-181">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-181">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="c11fd-182">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-182">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="c11fd-183">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-183">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="c11fd-184">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-184">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="c11fd-185">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-185">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="c11fd-186">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-186">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="c11fd-187">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-187">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="c11fd-188">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-188">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="c11fd-189">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-189">Singleton</span></span> |

::: moniker-end

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="c11fd-190">Rejestrowanie dodatkowych usług przy użyciu metod rozszerzających</span><span class="sxs-lookup"><span data-stu-id="c11fd-190">Register additional services with extension methods</span></span>

<span data-ttu-id="c11fd-191">Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać pojedynczej metody rozszerzenia `Add{SERVICE_NAME}`, aby zarejestrować wszystkie usługi wymagane przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="c11fd-191">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="c11fd-192">Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) i <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span><span class="sxs-lookup"><span data-stu-id="c11fd-192">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="c11fd-193">Aby uzyskać więcej informacji, zapoznaj się z klasą <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c11fd-193">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="c11fd-194">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="c11fd-194">Service lifetimes</span></span>

<span data-ttu-id="c11fd-195">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-195">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="c11fd-196">Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="c11fd-196">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="c11fd-197">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="c11fd-197">Transient</span></span>

<span data-ttu-id="c11fd-198">Przejściowe usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) są tworzone za każdym razem, gdy zażądają one kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="c11fd-198">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="c11fd-199">Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="c11fd-199">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="c11fd-200">Zakresie</span><span class="sxs-lookup"><span data-stu-id="c11fd-200">Scoped</span></span>

<span data-ttu-id="c11fd-201">Usługi okresu istnienia w zakresie (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) są tworzone raz dla każdego żądania klienta (połączenie).</span><span class="sxs-lookup"><span data-stu-id="c11fd-201">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="c11fd-202">W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do metody `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-202">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="c11fd-203">Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej.</span><span class="sxs-lookup"><span data-stu-id="c11fd-203">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="c11fd-204">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-204">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="c11fd-205">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="c11fd-205">Singleton</span></span>

<span data-ttu-id="c11fd-206">Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub po uruchomieniu `Startup.ConfigureServices`, a wystąpienie jest określone przy rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="c11fd-206">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="c11fd-207">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c11fd-207">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="c11fd-208">Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-208">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="c11fd-209">Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="c11fd-209">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="c11fd-210">Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="c11fd-210">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="c11fd-211">Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="c11fd-211">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="c11fd-212">Metody rejestracji usług</span><span class="sxs-lookup"><span data-stu-id="c11fd-212">Service registration methods</span></span>

<span data-ttu-id="c11fd-213">Metody rozszerzenia rejestracji usług oferują przeciążenia, które są przydatne w określonych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="c11fd-213">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="c11fd-214">Metoda</span><span class="sxs-lookup"><span data-stu-id="c11fd-214">Method</span></span> | <span data-ttu-id="c11fd-215">Automatyczne</span><span class="sxs-lookup"><span data-stu-id="c11fd-215">Automatic</span></span><br><span data-ttu-id="c11fd-216">object</span><span class="sxs-lookup"><span data-stu-id="c11fd-216">object</span></span><br><span data-ttu-id="c11fd-217">myśl</span><span class="sxs-lookup"><span data-stu-id="c11fd-217">disposal</span></span> | <span data-ttu-id="c11fd-218">Wiele</span><span class="sxs-lookup"><span data-stu-id="c11fd-218">Multiple</span></span><br><span data-ttu-id="c11fd-219">implementacje</span><span class="sxs-lookup"><span data-stu-id="c11fd-219">implementations</span></span> | <span data-ttu-id="c11fd-220">Przekaż argumenty</span><span class="sxs-lookup"><span data-stu-id="c11fd-220">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="c11fd-221">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c11fd-221">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="c11fd-222">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-222">Yes</span></span> | <span data-ttu-id="c11fd-223">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-223">Yes</span></span> | <span data-ttu-id="c11fd-224">Nie</span><span class="sxs-lookup"><span data-stu-id="c11fd-224">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="c11fd-225">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="c11fd-225">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="c11fd-226">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-226">Yes</span></span> | <span data-ttu-id="c11fd-227">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-227">Yes</span></span> | <span data-ttu-id="c11fd-228">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-228">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="c11fd-229">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c11fd-229">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="c11fd-230">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-230">Yes</span></span> | <span data-ttu-id="c11fd-231">Nie</span><span class="sxs-lookup"><span data-stu-id="c11fd-231">No</span></span> | <span data-ttu-id="c11fd-232">Nie</span><span class="sxs-lookup"><span data-stu-id="c11fd-232">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="c11fd-233">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="c11fd-233">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="c11fd-234">Nie</span><span class="sxs-lookup"><span data-stu-id="c11fd-234">No</span></span> | <span data-ttu-id="c11fd-235">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-235">Yes</span></span> | <span data-ttu-id="c11fd-236">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-236">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="c11fd-237">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="c11fd-237">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="c11fd-238">Nie</span><span class="sxs-lookup"><span data-stu-id="c11fd-238">No</span></span> | <span data-ttu-id="c11fd-239">Nie</span><span class="sxs-lookup"><span data-stu-id="c11fd-239">No</span></span> | <span data-ttu-id="c11fd-240">Tak</span><span class="sxs-lookup"><span data-stu-id="c11fd-240">Yes</span></span> |

<span data-ttu-id="c11fd-241">Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) .</span><span class="sxs-lookup"><span data-stu-id="c11fd-241">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="c11fd-242">Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="c11fd-242">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="c11fd-243">`TryAdd{LIFETIME}` metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.</span><span class="sxs-lookup"><span data-stu-id="c11fd-243">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="c11fd-244">W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency` dla `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-244">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="c11fd-245">Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` już ma zarejestrowanej implementacji:</span><span class="sxs-lookup"><span data-stu-id="c11fd-245">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="c11fd-246">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="c11fd-246">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="c11fd-247">Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*.</span><span class="sxs-lookup"><span data-stu-id="c11fd-247">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="c11fd-248">Wiele usług jest rozpoznawanych za pośrednictwem `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-248">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="c11fd-249">Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="c11fd-249">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="c11fd-250">Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="c11fd-250">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="c11fd-251">W poniższym przykładzie pierwszy wiersz rejestruje `MyDep` dla `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-251">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="c11fd-252">Drugi wiersz rejestruje `MyDep` dla `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-252">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="c11fd-253">Trzeci wiersz nie działa, ponieważ `IMyDep1` już ma zarejestrowanej implementacji `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="c11fd-253">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="c11fd-254">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="c11fd-254">Constructor injection behavior</span></span>

<span data-ttu-id="c11fd-255">Usługi mogą być rozwiązywane przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="c11fd-255">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="c11fd-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="c11fd-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="c11fd-257">`ActivatorUtilities` jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modeli.</span><span class="sxs-lookup"><span data-stu-id="c11fd-257">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="c11fd-258">Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="c11fd-258">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="c11fd-259">Gdy usługi są rozwiązane przez `IServiceProvider` lub `ActivatorUtilities`, iniekcja konstruktora wymaga konstruktora *publicznego* .</span><span class="sxs-lookup"><span data-stu-id="c11fd-259">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="c11fd-260">Gdy usługi są rozpoznawane przez `ActivatorUtilities`, iniekcja konstruktora wymaga, aby tylko jeden odpowiedni Konstruktor istniał.</span><span class="sxs-lookup"><span data-stu-id="c11fd-260">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="c11fd-261">Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.</span><span class="sxs-lookup"><span data-stu-id="c11fd-261">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="c11fd-262">Konteksty Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c11fd-262">Entity Framework contexts</span></span>

<span data-ttu-id="c11fd-263">Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="c11fd-263">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="c11fd-264">Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Przeciążenie podczas rejestrowania kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c11fd-264">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="c11fd-265">Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.</span><span class="sxs-lookup"><span data-stu-id="c11fd-265">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="c11fd-266">Opcje okresu istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="c11fd-266">Lifetime and registration options</span></span>

<span data-ttu-id="c11fd-267">Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje z unikatowym identyfikatorem, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-267">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="c11fd-268">W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:</span><span class="sxs-lookup"><span data-stu-id="c11fd-268">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c11fd-269">Interfejsy są zaimplementowane w klasie `Operation`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-269">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="c11fd-270">Konstruktor `Operation` generuje identyfikator GUID, jeśli nie został podany:</span><span class="sxs-lookup"><span data-stu-id="c11fd-270">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c11fd-271">Zarejestrowano `OperationService`, który zależy od poszczególnych typów `Operation`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-271">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="c11fd-272">Gdy żądanie `OperationService` za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.</span><span class="sxs-lookup"><span data-stu-id="c11fd-272">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="c11fd-273">Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` usługi `IOperationTransient` są inne niż `OperationId` `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-273">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="c11fd-274">`OperationService` otrzymuje nowe wystąpienie klasy `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-274">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="c11fd-275">Nowe wystąpienie daje różne `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-275">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="c11fd-276">Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` usługi `IOperationScoped` jest taka sama jak w przypadku `OperationService` w ramach żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="c11fd-276">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="c11fd-277">W przypadku żądań klientów obie usługi współdzielą inną wartość `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-277">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="c11fd-278">Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane między wszystkimi żądaniami klientów i wszystkimi usługami, `OperationId` jest stałą we wszystkich żądaniach obsługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-278">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="c11fd-279">W `Startup.ConfigureServices`każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:</span><span class="sxs-lookup"><span data-stu-id="c11fd-279">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="c11fd-280">Usługa `IOperationSingletonInstance` używa określonego wystąpienia o znanym IDENTYFIKATORze `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-280">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="c11fd-281">Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).</span><span class="sxs-lookup"><span data-stu-id="c11fd-281">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="c11fd-282">Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-282">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="c11fd-283">`IndexModel` Przykładowa aplikacja żąda każdego rodzaju typu `IOperation` i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-283">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="c11fd-284">Na stronie zostaną wyświetlone wszystkie wartości `OperationId` klasy modelu strony i usługi za pomocą przypisań właściwości:</span><span class="sxs-lookup"><span data-stu-id="c11fd-284">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

<span data-ttu-id="c11fd-285">Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:</span><span class="sxs-lookup"><span data-stu-id="c11fd-285">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="c11fd-286">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="c11fd-286">**First request:**</span></span>

<span data-ttu-id="c11fd-287">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="c11fd-287">Controller operations:</span></span>

<span data-ttu-id="c11fd-288">Przejściowy: d233e165-f417-469B-A866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="c11fd-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="c11fd-289">Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="c11fd-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="c11fd-290">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c11fd-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c11fd-291">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c11fd-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c11fd-292">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="c11fd-292">`OperationService` operations:</span></span>

<span data-ttu-id="c11fd-293">Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="c11fd-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="c11fd-294">Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="c11fd-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="c11fd-295">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c11fd-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c11fd-296">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c11fd-296">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c11fd-297">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="c11fd-297">**Second request:**</span></span>

<span data-ttu-id="c11fd-298">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="c11fd-298">Controller operations:</span></span>

<span data-ttu-id="c11fd-299">Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="c11fd-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="c11fd-300">Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="c11fd-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="c11fd-301">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c11fd-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c11fd-302">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c11fd-302">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c11fd-303">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="c11fd-303">`OperationService` operations:</span></span>

<span data-ttu-id="c11fd-304">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="c11fd-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="c11fd-305">Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="c11fd-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="c11fd-306">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c11fd-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c11fd-307">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c11fd-307">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c11fd-308">Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="c11fd-308">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="c11fd-309">Obiekty *przejściowe* są zawsze różne.</span><span class="sxs-lookup"><span data-stu-id="c11fd-309">*Transient* objects are always different.</span></span> <span data-ttu-id="c11fd-310">Przejściowa wartość `OperationId` dla pierwszych i drugich żądań klienta różni się w zależności od operacji `OperationService` i między żądaniami klientów.</span><span class="sxs-lookup"><span data-stu-id="c11fd-310">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="c11fd-311">Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="c11fd-311">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="c11fd-312">Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.</span><span class="sxs-lookup"><span data-stu-id="c11fd-312">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="c11fd-313">*Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, czy wystąpienie `Operation` jest dostępne w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-313">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="c11fd-314">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="c11fd-314">Call services from main</span></span>

<span data-ttu-id="c11fd-315">Utwórz <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> przy użyciu [IServiceScopeFactory. Isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c11fd-315">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="c11fd-316">Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="c11fd-316">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="c11fd-317">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c11fd-317">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

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
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
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
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="scope-validation"></a><span data-ttu-id="c11fd-318">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="c11fd-318">Scope validation</span></span>

<span data-ttu-id="c11fd-319">Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="c11fd-319">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="c11fd-320">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="c11fd-320">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="c11fd-321">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="c11fd-321">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="c11fd-322">Główny dostawca usług jest tworzony, gdy zostanie wywołane <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-322">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="c11fd-323">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c11fd-323">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="c11fd-324">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="c11fd-324">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="c11fd-325">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="c11fd-325">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="c11fd-326">Sprawdzanie poprawności zakresów usług przechwytuje te sytuacje w przypadku wywołania `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-326">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="c11fd-327">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-327">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="c11fd-328">Usługi żądania</span><span class="sxs-lookup"><span data-stu-id="c11fd-328">Request Services</span></span>

<span data-ttu-id="c11fd-329">Usługi dostępne w ramach żądania ASP.NET Core od `HttpContext` są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .</span><span class="sxs-lookup"><span data-stu-id="c11fd-329">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="c11fd-330">Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c11fd-330">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="c11fd-331">Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, a nie `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-331">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="c11fd-332">Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="c11fd-332">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="c11fd-333">Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę.</span><span class="sxs-lookup"><span data-stu-id="c11fd-333">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="c11fd-334">To daje klasy, które są łatwiejsze do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="c11fd-334">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="c11fd-335">Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp do kolekcji `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-335">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="c11fd-336">Projektowanie usług do iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="c11fd-336">Design services for dependency injection</span></span>

<span data-ttu-id="c11fd-337">Najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="c11fd-337">Best practices are to:</span></span>

* <span data-ttu-id="c11fd-338">Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.</span><span class="sxs-lookup"><span data-stu-id="c11fd-338">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="c11fd-339">Unikaj stanowych, statycznych klas i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="c11fd-339">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="c11fd-340">Zaprojektuj aplikacje do korzystania z pojedynczych usług, aby uniknąć tworzenia stanu globalnego.</span><span class="sxs-lookup"><span data-stu-id="c11fd-340">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="c11fd-341">Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="c11fd-341">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="c11fd-342">Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="c11fd-342">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="c11fd-343">Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.</span><span class="sxs-lookup"><span data-stu-id="c11fd-343">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="c11fd-344">Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="c11fd-344">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="c11fd-345">Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="c11fd-345">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="c11fd-346">Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c11fd-346">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="c11fd-347">Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="c11fd-347">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="c11fd-348">Usuwanie usług</span><span class="sxs-lookup"><span data-stu-id="c11fd-348">Disposal of services</span></span>

<span data-ttu-id="c11fd-349">Kontener wywołuje <xref:System.IDisposable.Dispose*> dla typów <xref:System.IDisposable>, które tworzy.</span><span class="sxs-lookup"><span data-stu-id="c11fd-349">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="c11fd-350">Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.</span><span class="sxs-lookup"><span data-stu-id="c11fd-350">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="c11fd-351">Zastępowanie kontenera usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="c11fd-351">Default service container replacement</span></span>

<span data-ttu-id="c11fd-352">Wbudowany kontener usług został zaprojektowany z myślą o potrzebach platformy i większości aplikacji konsumenckich.</span><span class="sxs-lookup"><span data-stu-id="c11fd-352">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="c11fd-353">Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, która nie jest obsługiwana przez wbudowany kontener, na przykład:</span><span class="sxs-lookup"><span data-stu-id="c11fd-353">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="c11fd-354">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="c11fd-354">Property injection</span></span>
* <span data-ttu-id="c11fd-355">Iniekcja oparta na nazwie</span><span class="sxs-lookup"><span data-stu-id="c11fd-355">Injection based on name</span></span>
* <span data-ttu-id="c11fd-356">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="c11fd-356">Child containers</span></span>
* <span data-ttu-id="c11fd-357">Niestandardowe zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="c11fd-357">Custom lifetime management</span></span>
* <span data-ttu-id="c11fd-358">Obsługa `Func<T>` w przypadku inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="c11fd-358">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="c11fd-359">Za pomocą aplikacji ASP.NET Core można używać następujących kontenerów innych firm:</span><span class="sxs-lookup"><span data-stu-id="c11fd-359">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="c11fd-360">Autofac</span><span class="sxs-lookup"><span data-stu-id="c11fd-360">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="c11fd-361">DryIoc</span><span class="sxs-lookup"><span data-stu-id="c11fd-361">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="c11fd-362">Prolongaty</span><span class="sxs-lookup"><span data-stu-id="c11fd-362">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="c11fd-363">LightInject</span><span class="sxs-lookup"><span data-stu-id="c11fd-363">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="c11fd-364">Lamar</span><span class="sxs-lookup"><span data-stu-id="c11fd-364">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="c11fd-365">Stashbox</span><span class="sxs-lookup"><span data-stu-id="c11fd-365">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="c11fd-366">Unity</span><span class="sxs-lookup"><span data-stu-id="c11fd-366">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="c11fd-367">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="c11fd-367">Thread safety</span></span>

<span data-ttu-id="c11fd-368">Twórz bezpieczne dla wątków usługi pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="c11fd-368">Create thread-safe singleton services.</span></span> <span data-ttu-id="c11fd-369">Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="c11fd-369">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="c11fd-370">Metoda fabryki pojedynczej usługi, taka jak drugi argument dla [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="c11fd-370">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="c11fd-371">Podobnie jak w przypadku konstruktora typu (`static`), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="c11fd-371">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="c11fd-372">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="c11fd-372">Recommendations</span></span>

* <span data-ttu-id="c11fd-373">Rozpoznawanie usług `async/await` i `Task` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="c11fd-373">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="c11fd-374">C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-374">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="c11fd-375">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-375">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="c11fd-376">Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="c11fd-376">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="c11fd-377">Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="c11fd-377">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="c11fd-378">Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="c11fd-378">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="c11fd-379">Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="c11fd-379">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="c11fd-380">Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="c11fd-380">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="c11fd-381">Unikaj używania *wzorca lokalizatora usługi*.</span><span class="sxs-lookup"><span data-stu-id="c11fd-381">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="c11fd-382">Na przykład nie należy wywoływać <xref:System.IServiceProvider.GetService*>, aby uzyskać wystąpienie usługi, gdy można użyć DI zamiast:</span><span class="sxs-lookup"><span data-stu-id="c11fd-382">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="c11fd-383">**Prawidłowy**</span><span class="sxs-lookup"><span data-stu-id="c11fd-383">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="c11fd-384">**Poprawne**:</span><span class="sxs-lookup"><span data-stu-id="c11fd-384">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="c11fd-385">Inna odmiana lokalizatora usługi, aby uniknąć, wprowadza fabrykę, która rozwiązuje zależności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="c11fd-385">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="c11fd-386">Obie te praktyki mieszają się [z niewersjami strategii kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="c11fd-386">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="c11fd-387">Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="c11fd-387">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="c11fd-388">Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="c11fd-388">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="c11fd-389">Wyjątki są rzadkimi&mdash;w większości specjalnych przypadków w ramach samej struktury.</span><span class="sxs-lookup"><span data-stu-id="c11fd-389">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="c11fd-390">DI jest *alternatywą* dla wzorców dostępu do obiektów static/Global.</span><span class="sxs-lookup"><span data-stu-id="c11fd-390">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="c11fd-391">Możesz nie być w stanie korzystać z zalet programu DI w przypadku jego mieszania z dostępem do obiektów statycznych.</span><span class="sxs-lookup"><span data-stu-id="c11fd-391">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c11fd-392">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c11fd-392">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="c11fd-393">Pisanie czystego kodu w ASP.NET Core z iniekcją zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="c11fd-393">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="c11fd-394">Zasada jawnych zależności</span><span class="sxs-lookup"><span data-stu-id="c11fd-394">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="c11fd-395">Niewersja kontenerów sterowania i wzorzec iniekcji zależności (Martin Fowlera)</span><span class="sxs-lookup"><span data-stu-id="c11fd-395">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="c11fd-396">Jak zarejestrować usługę z wieloma interfejsami w ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="c11fd-396">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
