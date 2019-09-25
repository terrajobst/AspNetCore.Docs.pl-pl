---
title: Wstrzykiwanie zależności w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób ASP.NET Core implementuje iniekcję zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: fefd0b9df71d5b0e7c30a31620292fd37eeecfa4
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248264"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="f1a9a-103">Wstrzykiwanie zależności w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1a9a-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="f1a9a-104">[Steve Kowalski](https://ardalis.com/), [Scott Addie](https://scottaddie.com)i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1a9a-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f1a9a-105">ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia [niewersji kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="f1a9a-106">Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="f1a9a-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1a9a-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="f1a9a-108">Przegląd iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="f1a9a-108">Overview of dependency injection</span></span>

<span data-ttu-id="f1a9a-109">*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="f1a9a-110">Zapoznaj się `MyDependency` `WriteMessage` z następującą klasą, korzystając z metody, na której zależą inne klasy w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="f1a9a-111">Wystąpienie `MyDependency` klasy można utworzyć w celu `WriteMessage` udostępnienia metody klasie.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="f1a9a-112">Klasa jest zależnością `IndexModel`klasy: `MyDependency`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="f1a9a-113">Klasa tworzy i bezpośrednio zależy `MyDependency` od wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="f1a9a-114">Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="f1a9a-115">Aby zastąpić `MyDependency` inną implementacją, należy zmodyfikować klasę.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="f1a9a-116">Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="f1a9a-117">W dużym projekcie z wieloma klasami `MyDependency`, w zależności od tego, kod konfiguracji jest rozmieszczany w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="f1a9a-118">Ta implementacja jest trudna do testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="f1a9a-119">Aplikacja powinna używać klasy imitacji lub zastępczej `MyDependency` , która nie jest możliwa w przypadku tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="f1a9a-120">Iniekcja zależności eliminuje te problemy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="f1a9a-121">Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="f1a9a-122">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="f1a9a-123">ASP.NET Core udostępnia wbudowany kontener usługi, <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="f1a9a-124">Usługi są zarejestrowane w `Startup.ConfigureServices` metodzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="f1a9a-125">*Iniekcja* usługi do konstruktora klasy, w której jest używana.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="f1a9a-126">Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="f1a9a-127">W [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) `IMyDependency` interfejs definiuje metodę dostarczaną przez usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1a9a-128">Ten interfejs jest implementowany przez konkretny typ `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1a9a-129">`MyDependency`<xref:Microsoft.Extensions.Logging.ILogger`1> żąda w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="f1a9a-130">Użycie iniekcji zależności w łańcuchu nie jest nietypowe.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="f1a9a-131">Każda żądana zależność z kolei żąda własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="f1a9a-132">Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="f1a9a-133">Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="f1a9a-134">`IMyDependency`i `ILogger<TCategoryName>` musi być zarejestrowany w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="f1a9a-135">`IMyDependency`jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f1a9a-136">`ILogger<TCategoryName>`jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona przez platformę](#framework-provided-services) zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="f1a9a-137">Kontener jest rozpoznawany `ILogger<TCategoryName>` przez wykorzystanie [(rodzajowe) otwartych typów](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) konstruowanego typu](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="f1a9a-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="f1a9a-138">W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana w konkretnym typie `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="f1a9a-139">Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="f1a9a-140">[Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="f1a9a-141">Każda `services.Add{SERVICE_NAME}` Metoda rozszerzenia dodaje usługi (i potencjalnie konfiguruje).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="f1a9a-142">Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="f1a9a-143">Zalecamy, aby aplikacje były zgodne z tą konwencją.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="f1a9a-144">Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="f1a9a-145">Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`,, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="f1a9a-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="f1a9a-146">Wystąpienie usługi jest wymagane za pośrednictwem konstruktora klasy, w której jest używana usługa i jest przypisywana do pola prywatnego.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="f1a9a-147">To pole jest używane w celu uzyskania dostępu do usługi w zależności od potrzeb w całej klasie.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="f1a9a-148">W przykładowej aplikacji `IMyDependency` wystąpienie jest wymagane i używane do wywołania `WriteMessage` metody usługi:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

## <a name="services-injected-into-startup"></a><span data-ttu-id="f1a9a-149">Usługi wstrzykiwane do uruchamiania</span><span class="sxs-lookup"><span data-stu-id="f1a9a-149">Services injected into Startup</span></span>

<span data-ttu-id="f1a9a-150">Tylko następujące typy usług można wstrzyknąć do konstruktora, `Startup` gdy jest używany host generyczny (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span><span class="sxs-lookup"><span data-stu-id="f1a9a-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="f1a9a-151">Usługi można wstrzyknąć do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="f1a9a-152">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="f1a9a-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="f1a9a-153">Framework-provided services</span></span>

<span data-ttu-id="f1a9a-154">`Startup.ConfigureServices` Metoda jest odpowiedzialna za Definiowanie usług, z których korzysta aplikacja, w tym funkcje platformy, takie jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="f1a9a-155">Początkowo `IServiceCollection` dostarczone z `ConfigureServices` usługami zdefiniowanymi przez platformę, w zależności od [konfiguracji hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="f1a9a-156">Aplikacja oparta na ASP.NET Core szablonie nie jest nietypowa, aby mieć setki usług zarejestrowanych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="f1a9a-157">W poniższej tabeli wymieniono niewielki przykład usług zarejestrowanych w ramach platformy.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-157">A small sample of framework-registered services is listed in the following table.</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="f1a9a-158">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="f1a9a-158">Service Type</span></span> | <span data-ttu-id="f1a9a-159">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="f1a9a-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="f1a9a-160">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="f1a9a-161">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="f1a9a-162">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="f1a9a-163">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="f1a9a-164">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="f1a9a-165">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="f1a9a-166">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="f1a9a-167">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="f1a9a-168">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="f1a9a-169">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="f1a9a-170">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="f1a9a-171">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="f1a9a-172">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="f1a9a-173">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-173">Singleton</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="f1a9a-174">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="f1a9a-174">Service Type</span></span> | <span data-ttu-id="f1a9a-175">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="f1a9a-175">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="f1a9a-176">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-176">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="f1a9a-177">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-177">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="f1a9a-178">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-178">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="f1a9a-179">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-179">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="f1a9a-180">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-180">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="f1a9a-181">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-181">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="f1a9a-182">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-182">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="f1a9a-183">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-183">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="f1a9a-184">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-184">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="f1a9a-185">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-185">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="f1a9a-186">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-186">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="f1a9a-187">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-187">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="f1a9a-188">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-188">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="f1a9a-189">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-189">Singleton</span></span> |

::: moniker-end

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="f1a9a-190">Rejestrowanie dodatkowych usług przy użyciu metod rozszerzających</span><span class="sxs-lookup"><span data-stu-id="f1a9a-190">Register additional services with extension methods</span></span>

<span data-ttu-id="f1a9a-191">Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać `Add{SERVICE_NAME}` pojedynczej metody rozszerzającej do rejestrowania wszystkich usług wymaganych przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-191">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="f1a9a-192">Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [\<AddDbContext TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) i: <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*></span><span class="sxs-lookup"><span data-stu-id="f1a9a-192">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="f1a9a-193">Aby uzyskać więcej informacji, zapoznaj się z <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> klasą w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-193">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="f1a9a-194">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="f1a9a-194">Service lifetimes</span></span>

<span data-ttu-id="f1a9a-195">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-195">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="f1a9a-196">Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-196">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="f1a9a-197">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="f1a9a-197">Transient</span></span>

<span data-ttu-id="f1a9a-198">Przejściowe usługi<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>okresu istnienia są tworzone za każdym razem, gdy zażądają one kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-198">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="f1a9a-199">Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-199">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="f1a9a-200">Zakresie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-200">Scoped</span></span>

<span data-ttu-id="f1a9a-201">Usługi okresu istnienia w<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>zakresie () są tworzone raz dla każdego żądania klienta (połączenie).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-201">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="f1a9a-202">W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do `Invoke` metody `InvokeAsync` lub.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-202">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="f1a9a-203">Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-203">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="f1a9a-204">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-204">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="f1a9a-205">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-205">Singleton</span></span>

<span data-ttu-id="f1a9a-206">Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub gdy `Startup.ConfigureServices` jest uruchamiany, a wystąpienie jest określone przy rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-206">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="f1a9a-207">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-207">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="f1a9a-208">Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-208">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="f1a9a-209">Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-209">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="f1a9a-210">Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-210">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="f1a9a-211">Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-211">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="f1a9a-212">Metody rejestracji usług</span><span class="sxs-lookup"><span data-stu-id="f1a9a-212">Service registration methods</span></span>

<span data-ttu-id="f1a9a-213">Każda metoda rozszerzenia rejestracji usługi oferuje przeciążenia, które są przydatne w określonych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-213">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="f1a9a-214">Metoda</span><span class="sxs-lookup"><span data-stu-id="f1a9a-214">Method</span></span> | <span data-ttu-id="f1a9a-215">Automatyczne</span><span class="sxs-lookup"><span data-stu-id="f1a9a-215">Automatic</span></span><br><span data-ttu-id="f1a9a-216">object</span><span class="sxs-lookup"><span data-stu-id="f1a9a-216">object</span></span><br><span data-ttu-id="f1a9a-217">myśl</span><span class="sxs-lookup"><span data-stu-id="f1a9a-217">disposal</span></span> | <span data-ttu-id="f1a9a-218">Wielokrotne</span><span class="sxs-lookup"><span data-stu-id="f1a9a-218">Multiple</span></span><br><span data-ttu-id="f1a9a-219">implementacje</span><span class="sxs-lookup"><span data-stu-id="f1a9a-219">implementations</span></span> | <span data-ttu-id="f1a9a-220">Przekaż argumenty</span><span class="sxs-lookup"><span data-stu-id="f1a9a-220">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="f1a9a-221">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-221">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="f1a9a-222">Tak</span><span class="sxs-lookup"><span data-stu-id="f1a9a-222">Yes</span></span> | <span data-ttu-id="f1a9a-223">Yes</span><span class="sxs-lookup"><span data-stu-id="f1a9a-223">Yes</span></span> | <span data-ttu-id="f1a9a-224">Nie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-224">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="f1a9a-225">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-225">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="f1a9a-226">Tak</span><span class="sxs-lookup"><span data-stu-id="f1a9a-226">Yes</span></span> | <span data-ttu-id="f1a9a-227">Yes</span><span class="sxs-lookup"><span data-stu-id="f1a9a-227">Yes</span></span> | <span data-ttu-id="f1a9a-228">Tak</span><span class="sxs-lookup"><span data-stu-id="f1a9a-228">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="f1a9a-229">Przykład:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-229">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="f1a9a-230">Tak</span><span class="sxs-lookup"><span data-stu-id="f1a9a-230">Yes</span></span> | <span data-ttu-id="f1a9a-231">Nie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-231">No</span></span> | <span data-ttu-id="f1a9a-232">Nie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-232">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="f1a9a-233">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-233">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="f1a9a-234">Nie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-234">No</span></span> | <span data-ttu-id="f1a9a-235">Yes</span><span class="sxs-lookup"><span data-stu-id="f1a9a-235">Yes</span></span> | <span data-ttu-id="f1a9a-236">Tak</span><span class="sxs-lookup"><span data-stu-id="f1a9a-236">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="f1a9a-237">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-237">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="f1a9a-238">Nie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-238">No</span></span> | <span data-ttu-id="f1a9a-239">Nie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-239">No</span></span> | <span data-ttu-id="f1a9a-240">Tak</span><span class="sxs-lookup"><span data-stu-id="f1a9a-240">Yes</span></span> |

<span data-ttu-id="f1a9a-241">Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) .</span><span class="sxs-lookup"><span data-stu-id="f1a9a-241">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="f1a9a-242">Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-242">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="f1a9a-243">`TryAdd{LIFETIME}`Metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-243">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="f1a9a-244">W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency`. `IMyDependency`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-244">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="f1a9a-245">Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` ma już zarejestrowaną implementację:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-245">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="f1a9a-246">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-246">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="f1a9a-247">Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-247">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="f1a9a-248">Wiele usług jest rozpoznawanych `IEnumerable<{SERVICE}>`za pośrednictwem.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-248">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="f1a9a-249">Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-249">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="f1a9a-250">Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-250">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="f1a9a-251">W poniższym przykładzie pierwszy wiersz rejestruje `MyDep`. `IMyDep1`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-251">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="f1a9a-252">Drugi wiersz rejestruje `MyDep`. `IMyDep2`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-252">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="f1a9a-253">Trzeci wiersz nie działa, ponieważ `IMyDep1` ma już zarejestrowana `MyDep`implementacja:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-253">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="f1a9a-254">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="f1a9a-254">Constructor injection behavior</span></span>

<span data-ttu-id="f1a9a-255">Usługi mogą być rozwiązywane przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-255">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="f1a9a-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>&ndash; Zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="f1a9a-257">`ActivatorUtilities`jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-257">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="f1a9a-258">Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-258">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="f1a9a-259">Gdy usługi są rozwiązane `IServiceProvider` przez `ActivatorUtilities`lub, iniekcja konstruktora wymaga konstruktora *publicznego* .</span><span class="sxs-lookup"><span data-stu-id="f1a9a-259">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="f1a9a-260">Gdy usługi są rozwiązane `ActivatorUtilities`przez, iniekcja konstruktora wymaga, aby istnieje tylko jeden odpowiedni Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-260">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="f1a9a-261">Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-261">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="f1a9a-262">Konteksty Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f1a9a-262">Entity Framework contexts</span></span>

<span data-ttu-id="f1a9a-263">Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-263">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="f1a9a-264">Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez Przeciążenie [\<AddDbContext TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Overload podczas rejestrowania kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-264">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="f1a9a-265">Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-265">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="f1a9a-266">Opcje okresu istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="f1a9a-266">Lifetime and registration options</span></span>

<span data-ttu-id="f1a9a-267">Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje `OperationId`z unikatowym identyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-267">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="f1a9a-268">W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-268">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1a9a-269">Interfejsy są zaimplementowane w `Operation` klasie.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-269">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="f1a9a-270">`Operation` Konstruktor generuje identyfikator GUID, jeśli nie został podany:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-270">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1a9a-271">Zarejestrowano, który zależy od poszczególnych `Operation` typów. `OperationService`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-271">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="f1a9a-272">Gdy `OperationService` żądanie jest wykonywane za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-272">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="f1a9a-273">Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` `IOperationTransient` `OperationId` usługa różni się od `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-273">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="f1a9a-274">`OperationService`odbiera nowe wystąpienie `IOperationTransient` klasy.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-274">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="f1a9a-275">Nowe wystąpienie daje inną `OperationId`wartość.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-275">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="f1a9a-276">Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` `IOperationScoped` usługa jest taka sama jak `OperationService` w ramach żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-276">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="f1a9a-277">W przypadku żądań klientów obie te usługi współdzielą inną `OperationId` wartość.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-277">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="f1a9a-278">Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane przez wszystkie żądania klientów i wszystkie usługi, `OperationId` jest to stała między wszystkimi żądaniami obsługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-278">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1a9a-279">W `Startup.ConfigureServices`programie każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-279">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="f1a9a-280">Usługa używa określonego wystąpienia o znanym `Guid.Empty`identyfikatorze. `IOperationSingletonInstance`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-280">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="f1a9a-281">Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-281">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="f1a9a-282">Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-282">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="f1a9a-283">Przykładowa aplikacja `IndexModel` wysyła żądania każdego `IOperation` rodzaju typu i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-283">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="f1a9a-284">Następnie na stronie zostaną wyświetlone wszystkie `OperationId` wartości klasy modelu strony i usługi za pomocą przypisań właściwości:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-284">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

<span data-ttu-id="f1a9a-285">Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-285">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="f1a9a-286">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="f1a9a-286">**First request:**</span></span>

<span data-ttu-id="f1a9a-287">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-287">Controller operations:</span></span>

<span data-ttu-id="f1a9a-288">Przejściowy: d233e165-f417-469B-A866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="f1a9a-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="f1a9a-289">Zakresie 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="f1a9a-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="f1a9a-290">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="f1a9a-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="f1a9a-291">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="f1a9a-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="f1a9a-292">`OperationService`składowa</span><span class="sxs-lookup"><span data-stu-id="f1a9a-292">`OperationService` operations:</span></span>

<span data-ttu-id="f1a9a-293">Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="f1a9a-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="f1a9a-294">Zakresie 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="f1a9a-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="f1a9a-295">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="f1a9a-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="f1a9a-296">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="f1a9a-296">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="f1a9a-297">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="f1a9a-297">**Second request:**</span></span>

<span data-ttu-id="f1a9a-298">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-298">Controller operations:</span></span>

<span data-ttu-id="f1a9a-299">Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="f1a9a-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="f1a9a-300">Zakresie 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="f1a9a-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="f1a9a-301">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="f1a9a-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="f1a9a-302">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="f1a9a-302">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="f1a9a-303">`OperationService`składowa</span><span class="sxs-lookup"><span data-stu-id="f1a9a-303">`OperationService` operations:</span></span>

<span data-ttu-id="f1a9a-304">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="f1a9a-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="f1a9a-305">Zakresie 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="f1a9a-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="f1a9a-306">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="f1a9a-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="f1a9a-307">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="f1a9a-307">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="f1a9a-308">Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-308">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="f1a9a-309">Obiekty *przejściowe* są zawsze różne.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-309">*Transient* objects are always different.</span></span> <span data-ttu-id="f1a9a-310">Wartość przejściowa `OperationId` dla pierwszego i drugiego żądania klienta różni się `OperationService` w zależności od operacji i między żądaniami klientów.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-310">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="f1a9a-311">Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-311">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="f1a9a-312">Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-312">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="f1a9a-313">*Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, `Operation` czy wystąpienie jest dostarczone `Startup.ConfigureServices`w.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-313">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="f1a9a-314">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="f1a9a-314">Call services from main</span></span>

<span data-ttu-id="f1a9a-315">Utwórz element <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory. isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-315">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="f1a9a-316">Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-316">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="f1a9a-317">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService`: `Program.Main`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-317">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="f1a9a-318">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="f1a9a-318">Scope validation</span></span>

<span data-ttu-id="f1a9a-319">Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-319">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="f1a9a-320">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-320">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="f1a9a-321">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-321">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="f1a9a-322">Dostawca usług głównych jest tworzony, gdy <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-322">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="f1a9a-323">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-323">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="f1a9a-324">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-324">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="f1a9a-325">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-325">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="f1a9a-326">Sprawdzanie poprawności zakresów usług przechwytuje te `BuildServiceProvider` sytuacje, gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-326">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="f1a9a-327">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-327">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="f1a9a-328">Usługi żądania</span><span class="sxs-lookup"><span data-stu-id="f1a9a-328">Request Services</span></span>

<span data-ttu-id="f1a9a-329">Usługi dostępne w ramach żądania `HttpContext` ASP.NET Core są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .</span><span class="sxs-lookup"><span data-stu-id="f1a9a-329">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="f1a9a-330">Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-330">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="f1a9a-331">Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, nie. `ApplicationServices`</span><span class="sxs-lookup"><span data-stu-id="f1a9a-331">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="f1a9a-332">Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-332">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="f1a9a-333">Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-333">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="f1a9a-334">To daje klasy, które są łatwiejsze do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-334">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="f1a9a-335">Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp `RequestServices` do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-335">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="f1a9a-336">Projektowanie usług do iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="f1a9a-336">Design services for dependency injection</span></span>

<span data-ttu-id="f1a9a-337">Najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-337">Best practices are to:</span></span>

* <span data-ttu-id="f1a9a-338">Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-338">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="f1a9a-339">Unikaj stanowych wywołań metod statycznych.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-339">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="f1a9a-340">Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-340">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="f1a9a-341">Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-341">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="f1a9a-342">Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-342">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="f1a9a-343">Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-343">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="f1a9a-344">Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-344">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="f1a9a-345">Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-345">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="f1a9a-346">Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-346">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="f1a9a-347">Usuwanie usług</span><span class="sxs-lookup"><span data-stu-id="f1a9a-347">Disposal of services</span></span>

<span data-ttu-id="f1a9a-348">Kontener wywołuje <xref:System.IDisposable.Dispose*> typy, które <xref:System.IDisposable> tworzy.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-348">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="f1a9a-349">Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-349">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="f1a9a-350">Zastępowanie kontenera usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="f1a9a-350">Default service container replacement</span></span>

<span data-ttu-id="f1a9a-351">Wbudowany kontener usług został zaprojektowany z myślą o potrzebach platformy i większości aplikacji konsumenckich.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-351">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="f1a9a-352">Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, która nie jest obsługiwana przez wbudowany kontener, na przykład:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-352">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="f1a9a-353">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="f1a9a-353">Property injection</span></span>
* <span data-ttu-id="f1a9a-354">Iniekcja oparta na nazwie</span><span class="sxs-lookup"><span data-stu-id="f1a9a-354">Injection based on name</span></span>
* <span data-ttu-id="f1a9a-355">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="f1a9a-355">Child containers</span></span>
* <span data-ttu-id="f1a9a-356">Niestandardowe zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="f1a9a-356">Custom lifetime management</span></span>
* <span data-ttu-id="f1a9a-357">`Func<T>`Obsługa inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="f1a9a-357">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="f1a9a-358">Za pomocą aplikacji ASP.NET Core można używać następujących kontenerów innych firm:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-358">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="f1a9a-359">Autofac</span><span class="sxs-lookup"><span data-stu-id="f1a9a-359">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="f1a9a-360">DryIoc</span><span class="sxs-lookup"><span data-stu-id="f1a9a-360">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="f1a9a-361">Prolongaty</span><span class="sxs-lookup"><span data-stu-id="f1a9a-361">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="f1a9a-362">LightInject</span><span class="sxs-lookup"><span data-stu-id="f1a9a-362">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="f1a9a-363">Lamar</span><span class="sxs-lookup"><span data-stu-id="f1a9a-363">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="f1a9a-364">Stashbox</span><span class="sxs-lookup"><span data-stu-id="f1a9a-364">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="f1a9a-365">Unity</span><span class="sxs-lookup"><span data-stu-id="f1a9a-365">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="f1a9a-366">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="f1a9a-366">Thread safety</span></span>

<span data-ttu-id="f1a9a-367">Twórz bezpieczne dla wątków usługi pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-367">Create thread-safe singleton services.</span></span> <span data-ttu-id="f1a9a-368">Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-368">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="f1a9a-369">Metoda fabryki pojedynczej usługi, taka jak drugi argument dla [\<AddSingleton TService >\<(IServiceCollection, Func IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-369">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="f1a9a-370">Podobnie jak w przypadku`static`konstruktora typu (), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-370">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="f1a9a-371">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="f1a9a-371">Recommendations</span></span>

* <span data-ttu-id="f1a9a-372">`async/await`Rozpoznawanie `Task` usług na podstawie nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-372">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="f1a9a-373">C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-373">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="f1a9a-374">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-374">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="f1a9a-375">Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-375">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="f1a9a-376">Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-376">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="f1a9a-377">Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-377">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="f1a9a-378">Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-378">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="f1a9a-379">Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-379">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="f1a9a-380">Unikaj używania *wzorca lokalizatora usługi*.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-380">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="f1a9a-381">Na przykład nie wywołuj <xref:System.IServiceProvider.GetService*> , aby uzyskać wystąpienie usługi, gdy można użyć di zamiast:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-381">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="f1a9a-382">**Prawidłowy**</span><span class="sxs-lookup"><span data-stu-id="f1a9a-382">**Incorrect:**</span></span>

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

  <span data-ttu-id="f1a9a-383">**Poprawne**:</span><span class="sxs-lookup"><span data-stu-id="f1a9a-383">**Correct**:</span></span>

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

* <span data-ttu-id="f1a9a-384">Inna odmiana lokalizatora usługi, aby uniknąć, wprowadza fabrykę, która rozwiązuje zależności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-384">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="f1a9a-385">Obie te praktyki mieszają się [z niewersjami strategii kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="f1a9a-385">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="f1a9a-386">Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="f1a9a-386">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="f1a9a-387">Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-387">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="f1a9a-388">Wyjątki są rzadko&mdash;w większości specjalnych przypadków w ramach samej struktury.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-388">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="f1a9a-389">DI jest *alternatywą* dla wzorców dostępu do obiektów static/Global.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-389">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="f1a9a-390">Możesz nie być w stanie korzystać z zalet programu DI w przypadku jego mieszania z dostępem do obiektów statycznych.</span><span class="sxs-lookup"><span data-stu-id="f1a9a-390">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1a9a-391">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f1a9a-391">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="f1a9a-392">Pisanie czystego kodu w ASP.NET Core z iniekcją zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="f1a9a-392">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="f1a9a-393">Zasada jawnych zależności</span><span class="sxs-lookup"><span data-stu-id="f1a9a-393">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="f1a9a-394">Niewersja kontenerów sterowania i wzorzec iniekcji zależności (Martin Fowlera)</span><span class="sxs-lookup"><span data-stu-id="f1a9a-394">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="f1a9a-395">Jak zarejestrować usługę z wieloma interfejsami w ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="f1a9a-395">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
