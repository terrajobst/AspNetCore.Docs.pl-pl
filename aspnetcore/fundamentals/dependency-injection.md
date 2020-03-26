---
title: Wstrzykiwanie zależności w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób ASP.NET Core implementuje iniekcję zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
uid: fundamentals/dependency-injection
ms.openlocfilehash: d24807d1ea675448536ab865d41ae46f80043666
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306522"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="cc851-103">Wstrzykiwanie zależności w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc851-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="cc851-104">[Steve Kowalski](https://ardalis.com/) i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="cc851-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cc851-105">ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia [niewersji kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.</span><span class="sxs-lookup"><span data-stu-id="cc851-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="cc851-106">Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="cc851-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="cc851-107">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc851-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="cc851-108">Przegląd iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="cc851-108">Overview of dependency injection</span></span>

<span data-ttu-id="cc851-109">*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="cc851-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="cc851-110">Zapoznaj się z następującą klasą `MyDependency` przy użyciu metody `WriteMessage`, z której zależą inne klasy w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc851-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="cc851-111">Wystąpienie klasy `MyDependency` można utworzyć w celu udostępnienia metody `WriteMessage` dla klasy.</span><span class="sxs-lookup"><span data-stu-id="cc851-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="cc851-112">Klasa `MyDependency` jest zależnością klasy `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="cc851-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="cc851-113">Klasa tworzy i bezpośrednio zależy od wystąpienia `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="cc851-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="cc851-114">Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="cc851-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="cc851-115">Aby zastąpić `MyDependency` z inną implementacją, należy zmodyfikować klasę.</span><span class="sxs-lookup"><span data-stu-id="cc851-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="cc851-116">Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="cc851-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="cc851-117">W dużym projekcie z wieloma klasami, w zależności od `MyDependency`, kod konfiguracji zostanie rozłożona przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="cc851-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="cc851-118">Ta implementacja jest trudna do testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="cc851-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="cc851-119">Aplikacja powinna korzystać z klasy imitacji lub stub `MyDependency`, co nie jest możliwe w przypadku tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="cc851-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="cc851-120">Iniekcja zależności eliminuje te problemy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cc851-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="cc851-121">Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="cc851-122">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="cc851-123">ASP.NET Core zawiera wbudowany kontener usługi <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="cc851-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="cc851-124">Usługi są zarejestrowane w metodzie `Startup.ConfigureServices` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="cc851-125">*Iniekcja* usługi do konstruktora klasy, w której jest używana.</span><span class="sxs-lookup"><span data-stu-id="cc851-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="cc851-126">Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="cc851-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="cc851-127">W [przykładowej aplikacji](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)interfejs `IMyDependency` definiuje metodę dostarczaną przez usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc851-127">In the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="cc851-128">Ten interfejs jest implementowany przez konkretny typ, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="cc851-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="cc851-129">`MyDependency` żąda <xref:Microsoft.Extensions.Logging.ILogger`1> w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="cc851-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="cc851-130">Użycie iniekcji zależności w łańcuchu nie jest nietypowe.</span><span class="sxs-lookup"><span data-stu-id="cc851-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="cc851-131">Każda żądana zależność z kolei żąda własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="cc851-132">Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="cc851-133">Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.</span><span class="sxs-lookup"><span data-stu-id="cc851-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="cc851-134">w kontenerze usługi `IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="cc851-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="cc851-135">`IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cc851-136">`ILogger<TCategoryName>` jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona przez platformę](#framework-provided-services) zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="cc851-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="cc851-137">Kontener rozwiązuje `ILogger<TCategoryName>` dzięki wykorzystaniu [typów otwartych (rodzajowych)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) typu konstruowanego](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="cc851-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="cc851-138">W przykładowej aplikacji usługa `IMyDependency` jest zarejestrowana z typem konkretnym `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="cc851-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="cc851-139">Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="cc851-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="cc851-140">[Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="cc851-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="cc851-141">Każda metoda rozszerzenia `services.Add{SERVICE_NAME}` dodaje usługi (i potencjalnie konfiguruje).</span><span class="sxs-lookup"><span data-stu-id="cc851-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="cc851-142">Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="cc851-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="cc851-143">Zalecamy, aby aplikacje były zgodne z tą konwencją.</span><span class="sxs-lookup"><span data-stu-id="cc851-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="cc851-144">Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="cc851-145">Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="cc851-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="cc851-146">Wystąpienie usługi jest wymagane za pośrednictwem konstruktora klasy, w której jest używana usługa i jest przypisywana do pola prywatnego.</span><span class="sxs-lookup"><span data-stu-id="cc851-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="cc851-147">To pole jest używane w celu uzyskania dostępu do usługi w zależności od potrzeb w całej klasie.</span><span class="sxs-lookup"><span data-stu-id="cc851-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="cc851-148">W przykładowej aplikacji jest wymagane wystąpienie `IMyDependency` i używane do wywołania metody `WriteMessage` usługi:</span><span class="sxs-lookup"><span data-stu-id="cc851-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="cc851-149">Usługi wstrzykiwane do uruchamiania</span><span class="sxs-lookup"><span data-stu-id="cc851-149">Services injected into Startup</span></span>

<span data-ttu-id="cc851-150">Tylko następujące typy usług można wstrzyknąć do konstruktora `Startup`, gdy jest używany host generyczny (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span><span class="sxs-lookup"><span data-stu-id="cc851-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="cc851-151">Usługi można wstrzyknąć do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="cc851-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="cc851-152">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="cc851-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="cc851-153">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="cc851-153">Framework-provided services</span></span>

<span data-ttu-id="cc851-154">Metoda `Startup.ConfigureServices` jest odpowiedzialna za Definiowanie usług, z których korzysta aplikacja, w tym funkcji platformy, takich jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cc851-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="cc851-155">Początkowo `IServiceCollection` zapewniane `ConfigureServices` ma usługi zdefiniowane przez platformę w zależności od [konfiguracji hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="cc851-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="cc851-156">Aplikacja oparta na ASP.NET Core szablonie nie jest nietypowa, aby mieć setki usług zarejestrowanych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="cc851-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="cc851-157">W poniższej tabeli wymieniono niewielki przykład usług zarejestrowanych w ramach platformy.</span><span class="sxs-lookup"><span data-stu-id="cc851-157">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="cc851-158">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="cc851-158">Service Type</span></span> | <span data-ttu-id="cc851-159">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="cc851-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="cc851-160">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="cc851-161">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="cc851-162">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="cc851-163">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="cc851-164">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="cc851-165">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="cc851-166">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="cc851-167">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="cc851-168">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="cc851-169">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="cc851-170">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="cc851-171">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="cc851-172">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="cc851-173">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-173">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="cc851-174">Rejestrowanie dodatkowych usług przy użyciu metod rozszerzających</span><span class="sxs-lookup"><span data-stu-id="cc851-174">Register additional services with extension methods</span></span>

<span data-ttu-id="cc851-175">Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać pojedynczej metody rozszerzenia `Add{SERVICE_NAME}`, aby zarejestrować wszystkie usługi wymagane przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="cc851-175">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="cc851-176">Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) i <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span><span class="sxs-lookup"><span data-stu-id="cc851-176">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="cc851-177">Aby uzyskać więcej informacji, zapoznaj się z klasą <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cc851-177">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="cc851-178">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="cc851-178">Service lifetimes</span></span>

<span data-ttu-id="cc851-179">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-179">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="cc851-180">Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="cc851-180">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="cc851-181">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-181">Transient</span></span>

<span data-ttu-id="cc851-182">Przejściowe usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) są tworzone za każdym razem, gdy zażądają one kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-182">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="cc851-183">Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-183">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="cc851-184">Zakresie</span><span class="sxs-lookup"><span data-stu-id="cc851-184">Scoped</span></span>

<span data-ttu-id="cc851-185">Usługi okresu istnienia w zakresie (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) są tworzone raz dla każdego żądania klienta (połączenie).</span><span class="sxs-lookup"><span data-stu-id="cc851-185">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="cc851-186">W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do metody `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="cc851-186">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="cc851-187">Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej.</span><span class="sxs-lookup"><span data-stu-id="cc851-187">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="cc851-188">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="cc851-188">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="cc851-189">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-189">Singleton</span></span>

<span data-ttu-id="cc851-190">Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub po uruchomieniu `Startup.ConfigureServices`, a wystąpienie jest określone przy rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="cc851-190">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="cc851-191">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cc851-191">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="cc851-192">Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-192">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="cc851-193">Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="cc851-193">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="cc851-194">Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="cc851-194">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="cc851-195">Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="cc851-195">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="cc851-196">Metody rejestracji usług</span><span class="sxs-lookup"><span data-stu-id="cc851-196">Service registration methods</span></span>

<span data-ttu-id="cc851-197">Metody rozszerzenia rejestracji usług oferują przeciążenia, które są przydatne w określonych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="cc851-197">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="cc851-198">Metoda</span><span class="sxs-lookup"><span data-stu-id="cc851-198">Method</span></span> | <span data-ttu-id="cc851-199">Automatyczny</span><span class="sxs-lookup"><span data-stu-id="cc851-199">Automatic</span></span><br><span data-ttu-id="cc851-200">obiekt</span><span class="sxs-lookup"><span data-stu-id="cc851-200">object</span></span><br><span data-ttu-id="cc851-201">myśl</span><span class="sxs-lookup"><span data-stu-id="cc851-201">disposal</span></span> | <span data-ttu-id="cc851-202">Wiele</span><span class="sxs-lookup"><span data-stu-id="cc851-202">Multiple</span></span><br><span data-ttu-id="cc851-203">implementacje</span><span class="sxs-lookup"><span data-stu-id="cc851-203">implementations</span></span> | <span data-ttu-id="cc851-204">Przekaż argumenty</span><span class="sxs-lookup"><span data-stu-id="cc851-204">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="cc851-205">Przykład:</span><span class="sxs-lookup"><span data-stu-id="cc851-205">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="cc851-206">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-206">Yes</span></span> | <span data-ttu-id="cc851-207">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-207">Yes</span></span> | <span data-ttu-id="cc851-208">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-208">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="cc851-209">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="cc851-209">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="cc851-210">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-210">Yes</span></span> | <span data-ttu-id="cc851-211">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-211">Yes</span></span> | <span data-ttu-id="cc851-212">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-212">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="cc851-213">Przykład:</span><span class="sxs-lookup"><span data-stu-id="cc851-213">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="cc851-214">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-214">Yes</span></span> | <span data-ttu-id="cc851-215">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-215">No</span></span> | <span data-ttu-id="cc851-216">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-216">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="cc851-217">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="cc851-217">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="cc851-218">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-218">No</span></span> | <span data-ttu-id="cc851-219">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-219">Yes</span></span> | <span data-ttu-id="cc851-220">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-220">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="cc851-221">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="cc851-221">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="cc851-222">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-222">No</span></span> | <span data-ttu-id="cc851-223">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-223">No</span></span> | <span data-ttu-id="cc851-224">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-224">Yes</span></span> |

<span data-ttu-id="cc851-225">Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) .</span><span class="sxs-lookup"><span data-stu-id="cc851-225">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="cc851-226">Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="cc851-226">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="cc851-227">`TryAdd{LIFETIME}` metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-227">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="cc851-228">W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency` dla `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="cc851-228">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="cc851-229">Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` już ma zarejestrowanej implementacji:</span><span class="sxs-lookup"><span data-stu-id="cc851-229">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="cc851-230">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="cc851-230">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="cc851-231">Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*.</span><span class="sxs-lookup"><span data-stu-id="cc851-231">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="cc851-232">Wiele usług jest rozpoznawanych za pośrednictwem `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="cc851-232">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="cc851-233">Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="cc851-233">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="cc851-234">Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="cc851-234">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="cc851-235">W poniższym przykładzie pierwszy wiersz rejestruje `MyDep` dla `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="cc851-235">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="cc851-236">Drugi wiersz rejestruje `MyDep` dla `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="cc851-236">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="cc851-237">Trzeci wiersz nie działa, ponieważ `IMyDep1` już ma zarejestrowanej implementacji `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="cc851-237">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="cc851-238">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="cc851-238">Constructor injection behavior</span></span>

<span data-ttu-id="cc851-239">Usługi mogą być rozwiązywane przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="cc851-239">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="cc851-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="cc851-241">`ActivatorUtilities` jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modeli.</span><span class="sxs-lookup"><span data-stu-id="cc851-241">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="cc851-242">Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="cc851-242">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="cc851-243">Gdy usługi są rozwiązane przez `IServiceProvider` lub `ActivatorUtilities`, iniekcja konstruktora wymaga konstruktora *publicznego* .</span><span class="sxs-lookup"><span data-stu-id="cc851-243">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="cc851-244">Gdy usługi są rozpoznawane przez `ActivatorUtilities`, iniekcja konstruktora wymaga, aby tylko jeden odpowiedni Konstruktor istniał.</span><span class="sxs-lookup"><span data-stu-id="cc851-244">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="cc851-245">Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-245">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="cc851-246">Konteksty Entity Framework</span><span class="sxs-lookup"><span data-stu-id="cc851-246">Entity Framework contexts</span></span>

<span data-ttu-id="cc851-247">Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="cc851-247">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="cc851-248">Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Przeciążenie podczas rejestrowania kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc851-248">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="cc851-249">Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.</span><span class="sxs-lookup"><span data-stu-id="cc851-249">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="cc851-250">Opcje okresu istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="cc851-250">Lifetime and registration options</span></span>

<span data-ttu-id="cc851-251">Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje z unikatowym identyfikatorem, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="cc851-251">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="cc851-252">W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:</span><span class="sxs-lookup"><span data-stu-id="cc851-252">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="cc851-253">Interfejsy są zaimplementowane w klasie `Operation`.</span><span class="sxs-lookup"><span data-stu-id="cc851-253">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="cc851-254">Konstruktor `Operation` generuje identyfikator GUID, jeśli nie został podany:</span><span class="sxs-lookup"><span data-stu-id="cc851-254">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="cc851-255">Zarejestrowano `OperationService`, który zależy od poszczególnych typów `Operation`.</span><span class="sxs-lookup"><span data-stu-id="cc851-255">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="cc851-256">Gdy żądanie `OperationService` za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.</span><span class="sxs-lookup"><span data-stu-id="cc851-256">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="cc851-257">Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` usługi `IOperationTransient` są inne niż `OperationId` `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="cc851-257">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="cc851-258">`OperationService` otrzymuje nowe wystąpienie klasy `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="cc851-258">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="cc851-259">Nowe wystąpienie daje różne `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="cc851-259">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="cc851-260">Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` usługi `IOperationScoped` jest taka sama jak w przypadku `OperationService` w ramach żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="cc851-260">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="cc851-261">W przypadku żądań klientów obie usługi współdzielą inną wartość `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="cc851-261">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="cc851-262">Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane między wszystkimi żądaniami klientów i wszystkimi usługami, `OperationId` jest stałą we wszystkich żądaniach obsługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-262">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="cc851-263">W `Startup.ConfigureServices`każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:</span><span class="sxs-lookup"><span data-stu-id="cc851-263">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="cc851-264">Usługa `IOperationSingletonInstance` używa określonego wystąpienia o znanym IDENTYFIKATORze `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="cc851-264">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="cc851-265">Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).</span><span class="sxs-lookup"><span data-stu-id="cc851-265">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="cc851-266">Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi.</span><span class="sxs-lookup"><span data-stu-id="cc851-266">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="cc851-267">`IndexModel` Przykładowa aplikacja żąda każdego rodzaju typu `IOperation` i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="cc851-267">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="cc851-268">Na stronie zostaną wyświetlone wszystkie wartości `OperationId` klasy modelu strony i usługi za pomocą przypisań właściwości:</span><span class="sxs-lookup"><span data-stu-id="cc851-268">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="cc851-269">Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:</span><span class="sxs-lookup"><span data-stu-id="cc851-269">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="cc851-270">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="cc851-270">**First request:**</span></span>

<span data-ttu-id="cc851-271">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cc851-271">Controller operations:</span></span>

<span data-ttu-id="cc851-272">Przejściowy: d233e165-f417-469B-A866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="cc851-272">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="cc851-273">Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="cc851-273">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="cc851-274">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-274">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-275">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-275">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-276">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="cc851-276">`OperationService` operations:</span></span>

<span data-ttu-id="cc851-277">Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="cc851-277">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="cc851-278">Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="cc851-278">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="cc851-279">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-279">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-280">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-280">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-281">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="cc851-281">**Second request:**</span></span>

<span data-ttu-id="cc851-282">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cc851-282">Controller operations:</span></span>

<span data-ttu-id="cc851-283">Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="cc851-283">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="cc851-284">Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="cc851-284">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="cc851-285">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-285">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-286">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-286">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-287">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="cc851-287">`OperationService` operations:</span></span>

<span data-ttu-id="cc851-288">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="cc851-288">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="cc851-289">Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="cc851-289">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="cc851-290">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-291">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-292">Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="cc851-292">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="cc851-293">Obiekty *przejściowe* są zawsze różne.</span><span class="sxs-lookup"><span data-stu-id="cc851-293">*Transient* objects are always different.</span></span> <span data-ttu-id="cc851-294">Przejściowa wartość `OperationId` dla pierwszych i drugich żądań klienta różni się w zależności od operacji `OperationService` i między żądaniami klientów.</span><span class="sxs-lookup"><span data-stu-id="cc851-294">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="cc851-295">Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="cc851-295">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="cc851-296">Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.</span><span class="sxs-lookup"><span data-stu-id="cc851-296">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="cc851-297">*Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, czy wystąpienie `Operation` jest dostępne w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-297">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="cc851-298">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="cc851-298">Call services from main</span></span>

<span data-ttu-id="cc851-299">Utwórz <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> przy użyciu [IServiceScopeFactory. Isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-299">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="cc851-300">Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="cc851-300">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="cc851-301">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="cc851-301">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="cc851-302">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="cc851-302">Scope validation</span></span>

<span data-ttu-id="cc851-303">Gdy aplikacja jest uruchomiona w środowisku deweloperskim i wywołuje [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) do skompilowania hosta, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="cc851-303">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="cc851-304">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="cc851-304">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="cc851-305">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="cc851-305">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="cc851-306">Główny dostawca usług jest tworzony, gdy zostanie wywołane <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>.</span><span class="sxs-lookup"><span data-stu-id="cc851-306">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="cc851-307">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-307">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="cc851-308">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="cc851-308">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="cc851-309">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="cc851-309">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="cc851-310">Sprawdzanie poprawności zakresów usług przechwytuje te sytuacje w przypadku wywołania `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="cc851-310">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="cc851-311">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="cc851-311">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="cc851-312">Usługi żądania</span><span class="sxs-lookup"><span data-stu-id="cc851-312">Request Services</span></span>

<span data-ttu-id="cc851-313">Usługi dostępne w ramach żądania ASP.NET Core od `HttpContext` są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .</span><span class="sxs-lookup"><span data-stu-id="cc851-313">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="cc851-314">Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-314">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="cc851-315">Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, a nie `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-315">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="cc851-316">Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cc851-316">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="cc851-317">Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę.</span><span class="sxs-lookup"><span data-stu-id="cc851-317">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="cc851-318">To daje klasy, które są łatwiejsze do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="cc851-318">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="cc851-319">Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp do kolekcji `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-319">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="cc851-320">Projektowanie usług do iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="cc851-320">Design services for dependency injection</span></span>

<span data-ttu-id="cc851-321">Najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="cc851-321">Best practices are to:</span></span>

* <span data-ttu-id="cc851-322">Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-322">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="cc851-323">Unikaj stanowych, statycznych klas i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="cc851-323">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="cc851-324">Zaprojektuj aplikacje do korzystania z pojedynczych usług, aby uniknąć tworzenia stanu globalnego.</span><span class="sxs-lookup"><span data-stu-id="cc851-324">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="cc851-325">Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-325">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="cc851-326">Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-326">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="cc851-327">Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.</span><span class="sxs-lookup"><span data-stu-id="cc851-327">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="cc851-328">Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="cc851-328">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="cc851-329">Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="cc851-329">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="cc851-330">Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cc851-330">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="cc851-331">Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="cc851-331">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="cc851-332">Usuwanie usług</span><span class="sxs-lookup"><span data-stu-id="cc851-332">Disposal of services</span></span>

<span data-ttu-id="cc851-333">Kontener wywołuje <xref:System.IDisposable.Dispose*> dla typów <xref:System.IDisposable>, które tworzy.</span><span class="sxs-lookup"><span data-stu-id="cc851-333">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="cc851-334">Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.</span><span class="sxs-lookup"><span data-stu-id="cc851-334">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="cc851-335">W poniższym przykładzie usługi są tworzone przez kontener usługi i usuwane automatycznie:</span><span class="sxs-lookup"><span data-stu-id="cc851-335">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="cc851-336">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cc851-336">In the following example:</span></span>

* <span data-ttu-id="cc851-337">Wystąpienia usługi nie są tworzone przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-337">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="cc851-338">Planowane okresy istnienia usług nie są znane przez platformę.</span><span class="sxs-lookup"><span data-stu-id="cc851-338">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="cc851-339">Platforma nie usuwa automatycznie usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-339">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="cc851-340">Jeśli usługi nie zostały jawnie usunięte w kodzie dewelopera, są zachowywane do momentu zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-340">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

<span data-ttu-id="cc851-341">Aby zapoznać się z omówieniem opcji usuwania usług, zobacz [cztery sposoby usuwania interfejsu IDisposable w ASP.NET Core](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/).</span><span class="sxs-lookup"><span data-stu-id="cc851-341">For a discussion of service disposal options, see [Four ways to dispose IDisposables in ASP.NET Core](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/).</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="cc851-342">Zastępowanie kontenera usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="cc851-342">Default service container replacement</span></span>

<span data-ttu-id="cc851-343">Wbudowany kontener usług został zaprojektowany z myślą o potrzebach platformy i większości aplikacji konsumenckich.</span><span class="sxs-lookup"><span data-stu-id="cc851-343">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="cc851-344">Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, która nie jest obsługiwana przez wbudowany kontener, na przykład:</span><span class="sxs-lookup"><span data-stu-id="cc851-344">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="cc851-345">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="cc851-345">Property injection</span></span>
* <span data-ttu-id="cc851-346">Iniekcja oparta na nazwie</span><span class="sxs-lookup"><span data-stu-id="cc851-346">Injection based on name</span></span>
* <span data-ttu-id="cc851-347">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="cc851-347">Child containers</span></span>
* <span data-ttu-id="cc851-348">Niestandardowe zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="cc851-348">Custom lifetime management</span></span>
* <span data-ttu-id="cc851-349">Obsługa `Func<T>` w przypadku inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="cc851-349">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="cc851-350">Rejestracja oparta na Konwencji</span><span class="sxs-lookup"><span data-stu-id="cc851-350">Convention-based registration</span></span>

<span data-ttu-id="cc851-351">Za pomocą aplikacji ASP.NET Core można używać następujących kontenerów innych firm:</span><span class="sxs-lookup"><span data-stu-id="cc851-351">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="cc851-352">Autofac</span><span class="sxs-lookup"><span data-stu-id="cc851-352">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="cc851-353">DryIoc</span><span class="sxs-lookup"><span data-stu-id="cc851-353">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="cc851-354">Prolongaty</span><span class="sxs-lookup"><span data-stu-id="cc851-354">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="cc851-355">LightInject</span><span class="sxs-lookup"><span data-stu-id="cc851-355">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="cc851-356">Lamar</span><span class="sxs-lookup"><span data-stu-id="cc851-356">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="cc851-357">Stashbox</span><span class="sxs-lookup"><span data-stu-id="cc851-357">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="cc851-358">Unity</span><span class="sxs-lookup"><span data-stu-id="cc851-358">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="cc851-359">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="cc851-359">Thread safety</span></span>

<span data-ttu-id="cc851-360">Twórz bezpieczne dla wątków usługi pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="cc851-360">Create thread-safe singleton services.</span></span> <span data-ttu-id="cc851-361">Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="cc851-361">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="cc851-362">Metoda fabryki pojedynczej usługi, taka jak drugi argument dla [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="cc851-362">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="cc851-363">Podobnie jak w przypadku konstruktora typu (`static`), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="cc851-363">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="cc851-364">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="cc851-364">Recommendations</span></span>

* <span data-ttu-id="cc851-365">Rozpoznawanie usług `async/await` i `Task` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cc851-365">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="cc851-366">C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-366">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="cc851-367">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-367">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="cc851-368">Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-368">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="cc851-369">Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="cc851-369">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="cc851-370">Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="cc851-370">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="cc851-371">Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="cc851-371">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="cc851-372">Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="cc851-372">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="cc851-373">Unikaj używania *wzorca lokalizatora usługi*.</span><span class="sxs-lookup"><span data-stu-id="cc851-373">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="cc851-374">Na przykład nie należy wywoływać <xref:System.IServiceProvider.GetService*>, aby uzyskać wystąpienie usługi, gdy można użyć DI zamiast:</span><span class="sxs-lookup"><span data-stu-id="cc851-374">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="cc851-375">**Prawidłowy**</span><span class="sxs-lookup"><span data-stu-id="cc851-375">**Incorrect:**</span></span>

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

  <span data-ttu-id="cc851-376">**Poprawne**:</span><span class="sxs-lookup"><span data-stu-id="cc851-376">**Correct**:</span></span>

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

* <span data-ttu-id="cc851-377">Inna odmiana lokalizatora usługi, aby uniknąć, wprowadza fabrykę, która rozwiązuje zależności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="cc851-377">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="cc851-378">Obie te praktyki mieszają się [z niewersjami strategii kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="cc851-378">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="cc851-379">Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="cc851-379">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="cc851-380">Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="cc851-380">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="cc851-381">Wyjątki są rzadkimi&mdash;w większości specjalnych przypadków w ramach samej struktury.</span><span class="sxs-lookup"><span data-stu-id="cc851-381">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="cc851-382">DI jest *alternatywą* dla wzorców dostępu do obiektów static/Global.</span><span class="sxs-lookup"><span data-stu-id="cc851-382">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="cc851-383">Możesz nie być w stanie korzystać z zalet programu DI w przypadku jego mieszania z dostępem do obiektów statycznych.</span><span class="sxs-lookup"><span data-stu-id="cc851-383">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc851-384">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc851-384">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="cc851-385">Pisanie czystego kodu w ASP.NET Core z iniekcją zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="cc851-385">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="cc851-386">Zasada jawnych zależności</span><span class="sxs-lookup"><span data-stu-id="cc851-386">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="cc851-387">Niewersja kontenerów sterowania i wzorzec iniekcji zależności (Martin Fowlera)</span><span class="sxs-lookup"><span data-stu-id="cc851-387">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="cc851-388">Jak zarejestrować usługę z wieloma interfejsami w ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="cc851-388">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="cc851-389">ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia [niewersji kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.</span><span class="sxs-lookup"><span data-stu-id="cc851-389">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="cc851-390">Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="cc851-390">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="cc851-391">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc851-391">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="cc851-392">Przegląd iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="cc851-392">Overview of dependency injection</span></span>

<span data-ttu-id="cc851-393">*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="cc851-393">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="cc851-394">Zapoznaj się z następującą klasą `MyDependency` przy użyciu metody `WriteMessage`, z której zależą inne klasy w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc851-394">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="cc851-395">Wystąpienie klasy `MyDependency` można utworzyć w celu udostępnienia metody `WriteMessage` dla klasy.</span><span class="sxs-lookup"><span data-stu-id="cc851-395">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="cc851-396">Klasa `MyDependency` jest zależnością klasy `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="cc851-396">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="cc851-397">Klasa tworzy i bezpośrednio zależy od wystąpienia `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="cc851-397">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="cc851-398">Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="cc851-398">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="cc851-399">Aby zastąpić `MyDependency` z inną implementacją, należy zmodyfikować klasę.</span><span class="sxs-lookup"><span data-stu-id="cc851-399">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="cc851-400">Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="cc851-400">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="cc851-401">W dużym projekcie z wieloma klasami, w zależności od `MyDependency`, kod konfiguracji zostanie rozłożona przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="cc851-401">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="cc851-402">Ta implementacja jest trudna do testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="cc851-402">This implementation is difficult to unit test.</span></span> <span data-ttu-id="cc851-403">Aplikacja powinna korzystać z klasy imitacji lub stub `MyDependency`, co nie jest możliwe w przypadku tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="cc851-403">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="cc851-404">Iniekcja zależności eliminuje te problemy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cc851-404">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="cc851-405">Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-405">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="cc851-406">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-406">Registration of the dependency in a service container.</span></span> <span data-ttu-id="cc851-407">ASP.NET Core zawiera wbudowany kontener usługi <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="cc851-407">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="cc851-408">Usługi są zarejestrowane w metodzie `Startup.ConfigureServices` aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-408">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="cc851-409">*Iniekcja* usługi do konstruktora klasy, w której jest używana.</span><span class="sxs-lookup"><span data-stu-id="cc851-409">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="cc851-410">Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="cc851-410">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="cc851-411">W [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples)interfejs `IMyDependency` definiuje metodę dostarczaną przez usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="cc851-411">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="cc851-412">Ten interfejs jest implementowany przez konkretny typ, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="cc851-412">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="cc851-413">`MyDependency` żąda <xref:Microsoft.Extensions.Logging.ILogger`1> w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="cc851-413">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="cc851-414">Użycie iniekcji zależności w łańcuchu nie jest nietypowe.</span><span class="sxs-lookup"><span data-stu-id="cc851-414">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="cc851-415">Każda żądana zależność z kolei żąda własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-415">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="cc851-416">Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-416">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="cc851-417">Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.</span><span class="sxs-lookup"><span data-stu-id="cc851-417">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="cc851-418">w kontenerze usługi `IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="cc851-418">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="cc851-419">`IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-419">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="cc851-420">`ILogger<TCategoryName>` jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona przez platformę](#framework-provided-services) zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="cc851-420">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="cc851-421">Kontener rozwiązuje `ILogger<TCategoryName>` dzięki wykorzystaniu [typów otwartych (rodzajowych)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) typu konstruowanego](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="cc851-421">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="cc851-422">W przykładowej aplikacji usługa `IMyDependency` jest zarejestrowana z typem konkretnym `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="cc851-422">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="cc851-423">Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="cc851-423">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="cc851-424">[Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="cc851-424">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="cc851-425">Każda metoda rozszerzenia `services.Add{SERVICE_NAME}` dodaje usługi (i potencjalnie konfiguruje).</span><span class="sxs-lookup"><span data-stu-id="cc851-425">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="cc851-426">Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="cc851-426">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="cc851-427">Zalecamy, aby aplikacje były zgodne z tą konwencją.</span><span class="sxs-lookup"><span data-stu-id="cc851-427">We recommended that apps follow this convention.</span></span> <span data-ttu-id="cc851-428">Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-428">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="cc851-429">Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="cc851-429">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="cc851-430">Wystąpienie usługi jest wymagane za pośrednictwem konstruktora klasy, w której jest używana usługa i jest przypisywana do pola prywatnego.</span><span class="sxs-lookup"><span data-stu-id="cc851-430">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="cc851-431">To pole jest używane w celu uzyskania dostępu do usługi w zależności od potrzeb w całej klasie.</span><span class="sxs-lookup"><span data-stu-id="cc851-431">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="cc851-432">W przykładowej aplikacji jest wymagane wystąpienie `IMyDependency` i używane do wywołania metody `WriteMessage` usługi:</span><span class="sxs-lookup"><span data-stu-id="cc851-432">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="cc851-433">Usługi wstrzykiwane do uruchamiania</span><span class="sxs-lookup"><span data-stu-id="cc851-433">Services injected into Startup</span></span>

<span data-ttu-id="cc851-434">Tylko następujące typy usług można wstrzyknąć do konstruktora `Startup`, gdy jest używany host generyczny (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span><span class="sxs-lookup"><span data-stu-id="cc851-434">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="cc851-435">Usługi można wstrzyknąć do `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="cc851-435">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="cc851-436">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="cc851-436">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="cc851-437">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="cc851-437">Framework-provided services</span></span>

<span data-ttu-id="cc851-438">Metoda `Startup.ConfigureServices` jest odpowiedzialna za Definiowanie usług, z których korzysta aplikacja, w tym funkcji platformy, takich jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cc851-438">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="cc851-439">Początkowo `IServiceCollection` zapewniane `ConfigureServices` ma usługi zdefiniowane przez platformę w zależności od [konfiguracji hosta](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="cc851-439">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="cc851-440">Aplikacja oparta na ASP.NET Core szablonie nie jest nietypowa, aby mieć setki usług zarejestrowanych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="cc851-440">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="cc851-441">W poniższej tabeli wymieniono niewielki przykład usług zarejestrowanych w ramach platformy.</span><span class="sxs-lookup"><span data-stu-id="cc851-441">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="cc851-442">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="cc851-442">Service Type</span></span> | <span data-ttu-id="cc851-443">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="cc851-443">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="cc851-444">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-444">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="cc851-445">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-445">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="cc851-446">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-446">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="cc851-447">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-447">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="cc851-448">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-448">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="cc851-449">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-449">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="cc851-450">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-450">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="cc851-451">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-451">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="cc851-452">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-452">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="cc851-453">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-453">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="cc851-454">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-454">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="cc851-455">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-455">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="cc851-456">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-456">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="cc851-457">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-457">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="cc851-458">Rejestrowanie dodatkowych usług przy użyciu metod rozszerzających</span><span class="sxs-lookup"><span data-stu-id="cc851-458">Register additional services with extension methods</span></span>

<span data-ttu-id="cc851-459">Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać pojedynczej metody rozszerzenia `Add{SERVICE_NAME}`, aby zarejestrować wszystkie usługi wymagane przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="cc851-459">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="cc851-460">Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) i <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span><span class="sxs-lookup"><span data-stu-id="cc851-460">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="cc851-461">Aby uzyskać więcej informacji, zapoznaj się z klasą <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="cc851-461">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="cc851-462">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="cc851-462">Service lifetimes</span></span>

<span data-ttu-id="cc851-463">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-463">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="cc851-464">Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="cc851-464">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="cc851-465">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="cc851-465">Transient</span></span>

<span data-ttu-id="cc851-466">Przejściowe usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) są tworzone za każdym razem, gdy zażądają one kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-466">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="cc851-467">Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-467">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="cc851-468">Zakresie</span><span class="sxs-lookup"><span data-stu-id="cc851-468">Scoped</span></span>

<span data-ttu-id="cc851-469">Usługi okresu istnienia w zakresie (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) są tworzone raz dla każdego żądania klienta (połączenie).</span><span class="sxs-lookup"><span data-stu-id="cc851-469">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="cc851-470">W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do metody `Invoke` lub `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="cc851-470">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="cc851-471">Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej.</span><span class="sxs-lookup"><span data-stu-id="cc851-471">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="cc851-472">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="cc851-472">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="cc851-473">pojedynczego</span><span class="sxs-lookup"><span data-stu-id="cc851-473">Singleton</span></span>

<span data-ttu-id="cc851-474">Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub po uruchomieniu `Startup.ConfigureServices`, a wystąpienie jest określone przy rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="cc851-474">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="cc851-475">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="cc851-475">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="cc851-476">Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-476">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="cc851-477">Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="cc851-477">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="cc851-478">Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="cc851-478">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="cc851-479">Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="cc851-479">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="cc851-480">Metody rejestracji usług</span><span class="sxs-lookup"><span data-stu-id="cc851-480">Service registration methods</span></span>

<span data-ttu-id="cc851-481">Metody rozszerzenia rejestracji usług oferują przeciążenia, które są przydatne w określonych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="cc851-481">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="cc851-482">Metoda</span><span class="sxs-lookup"><span data-stu-id="cc851-482">Method</span></span> | <span data-ttu-id="cc851-483">Automatyczny</span><span class="sxs-lookup"><span data-stu-id="cc851-483">Automatic</span></span><br><span data-ttu-id="cc851-484">obiekt</span><span class="sxs-lookup"><span data-stu-id="cc851-484">object</span></span><br><span data-ttu-id="cc851-485">myśl</span><span class="sxs-lookup"><span data-stu-id="cc851-485">disposal</span></span> | <span data-ttu-id="cc851-486">Wiele</span><span class="sxs-lookup"><span data-stu-id="cc851-486">Multiple</span></span><br><span data-ttu-id="cc851-487">implementacje</span><span class="sxs-lookup"><span data-stu-id="cc851-487">implementations</span></span> | <span data-ttu-id="cc851-488">Przekaż argumenty</span><span class="sxs-lookup"><span data-stu-id="cc851-488">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="cc851-489">Przykład:</span><span class="sxs-lookup"><span data-stu-id="cc851-489">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="cc851-490">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-490">Yes</span></span> | <span data-ttu-id="cc851-491">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-491">Yes</span></span> | <span data-ttu-id="cc851-492">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-492">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="cc851-493">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="cc851-493">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="cc851-494">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-494">Yes</span></span> | <span data-ttu-id="cc851-495">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-495">Yes</span></span> | <span data-ttu-id="cc851-496">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-496">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="cc851-497">Przykład:</span><span class="sxs-lookup"><span data-stu-id="cc851-497">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="cc851-498">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-498">Yes</span></span> | <span data-ttu-id="cc851-499">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-499">No</span></span> | <span data-ttu-id="cc851-500">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-500">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="cc851-501">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="cc851-501">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="cc851-502">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-502">No</span></span> | <span data-ttu-id="cc851-503">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-503">Yes</span></span> | <span data-ttu-id="cc851-504">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-504">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="cc851-505">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="cc851-505">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="cc851-506">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-506">No</span></span> | <span data-ttu-id="cc851-507">Nie</span><span class="sxs-lookup"><span data-stu-id="cc851-507">No</span></span> | <span data-ttu-id="cc851-508">Yes</span><span class="sxs-lookup"><span data-stu-id="cc851-508">Yes</span></span> |

<span data-ttu-id="cc851-509">Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) .</span><span class="sxs-lookup"><span data-stu-id="cc851-509">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="cc851-510">Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="cc851-510">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="cc851-511">`TryAdd{LIFETIME}` metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-511">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="cc851-512">W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency` dla `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="cc851-512">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="cc851-513">Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` już ma zarejestrowanej implementacji:</span><span class="sxs-lookup"><span data-stu-id="cc851-513">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="cc851-514">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="cc851-514">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="cc851-515">Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*.</span><span class="sxs-lookup"><span data-stu-id="cc851-515">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="cc851-516">Wiele usług jest rozpoznawanych za pośrednictwem `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="cc851-516">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="cc851-517">Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="cc851-517">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="cc851-518">Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="cc851-518">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="cc851-519">W poniższym przykładzie pierwszy wiersz rejestruje `MyDep` dla `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="cc851-519">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="cc851-520">Drugi wiersz rejestruje `MyDep` dla `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="cc851-520">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="cc851-521">Trzeci wiersz nie działa, ponieważ `IMyDep1` już ma zarejestrowanej implementacji `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="cc851-521">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="cc851-522">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="cc851-522">Constructor injection behavior</span></span>

<span data-ttu-id="cc851-523">Usługi mogą być rozwiązywane przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="cc851-523">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="cc851-524"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-524"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="cc851-525">`ActivatorUtilities` jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modeli.</span><span class="sxs-lookup"><span data-stu-id="cc851-525">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="cc851-526">Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="cc851-526">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="cc851-527">Gdy usługi są rozwiązane przez `IServiceProvider` lub `ActivatorUtilities`, iniekcja konstruktora wymaga konstruktora *publicznego* .</span><span class="sxs-lookup"><span data-stu-id="cc851-527">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="cc851-528">Gdy usługi są rozpoznawane przez `ActivatorUtilities`, iniekcja konstruktora wymaga, aby tylko jeden odpowiedni Konstruktor istniał.</span><span class="sxs-lookup"><span data-stu-id="cc851-528">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="cc851-529">Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-529">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="cc851-530">Konteksty Entity Framework</span><span class="sxs-lookup"><span data-stu-id="cc851-530">Entity Framework contexts</span></span>

<span data-ttu-id="cc851-531">Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="cc851-531">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="cc851-532">Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Przeciążenie podczas rejestrowania kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc851-532">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="cc851-533">Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.</span><span class="sxs-lookup"><span data-stu-id="cc851-533">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="cc851-534">Opcje okresu istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="cc851-534">Lifetime and registration options</span></span>

<span data-ttu-id="cc851-535">Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje z unikatowym identyfikatorem, `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="cc851-535">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="cc851-536">W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:</span><span class="sxs-lookup"><span data-stu-id="cc851-536">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="cc851-537">Interfejsy są zaimplementowane w klasie `Operation`.</span><span class="sxs-lookup"><span data-stu-id="cc851-537">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="cc851-538">Konstruktor `Operation` generuje identyfikator GUID, jeśli nie został podany:</span><span class="sxs-lookup"><span data-stu-id="cc851-538">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="cc851-539">Zarejestrowano `OperationService`, który zależy od poszczególnych typów `Operation`.</span><span class="sxs-lookup"><span data-stu-id="cc851-539">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="cc851-540">Gdy żądanie `OperationService` za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.</span><span class="sxs-lookup"><span data-stu-id="cc851-540">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="cc851-541">Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` usługi `IOperationTransient` są inne niż `OperationId` `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="cc851-541">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="cc851-542">`OperationService` otrzymuje nowe wystąpienie klasy `IOperationTransient`.</span><span class="sxs-lookup"><span data-stu-id="cc851-542">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="cc851-543">Nowe wystąpienie daje różne `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="cc851-543">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="cc851-544">Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` usługi `IOperationScoped` jest taka sama jak w przypadku `OperationService` w ramach żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="cc851-544">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="cc851-545">W przypadku żądań klientów obie usługi współdzielą inną wartość `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="cc851-545">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="cc851-546">Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane między wszystkimi żądaniami klientów i wszystkimi usługami, `OperationId` jest stałą we wszystkich żądaniach obsługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-546">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="cc851-547">W `Startup.ConfigureServices`każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:</span><span class="sxs-lookup"><span data-stu-id="cc851-547">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="cc851-548">Usługa `IOperationSingletonInstance` używa określonego wystąpienia o znanym IDENTYFIKATORze `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="cc851-548">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="cc851-549">Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).</span><span class="sxs-lookup"><span data-stu-id="cc851-549">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="cc851-550">Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi.</span><span class="sxs-lookup"><span data-stu-id="cc851-550">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="cc851-551">`IndexModel` Przykładowa aplikacja żąda każdego rodzaju typu `IOperation` i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="cc851-551">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="cc851-552">Na stronie zostaną wyświetlone wszystkie wartości `OperationId` klasy modelu strony i usługi za pomocą przypisań właściwości:</span><span class="sxs-lookup"><span data-stu-id="cc851-552">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="cc851-553">Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:</span><span class="sxs-lookup"><span data-stu-id="cc851-553">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="cc851-554">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="cc851-554">**First request:**</span></span>

<span data-ttu-id="cc851-555">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cc851-555">Controller operations:</span></span>

<span data-ttu-id="cc851-556">Przejściowy: d233e165-f417-469B-A866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="cc851-556">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="cc851-557">Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="cc851-557">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="cc851-558">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-558">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-559">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-559">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-560">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="cc851-560">`OperationService` operations:</span></span>

<span data-ttu-id="cc851-561">Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="cc851-561">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="cc851-562">Zakres: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="cc851-562">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="cc851-563">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-563">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-564">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-564">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-565">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="cc851-565">**Second request:**</span></span>

<span data-ttu-id="cc851-566">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cc851-566">Controller operations:</span></span>

<span data-ttu-id="cc851-567">Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="cc851-567">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="cc851-568">Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="cc851-568">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="cc851-569">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-569">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-570">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-570">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-571">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="cc851-571">`OperationService` operations:</span></span>

<span data-ttu-id="cc851-572">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="cc851-572">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="cc851-573">Zakres: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="cc851-573">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="cc851-574">Pojedyncze: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="cc851-574">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="cc851-575">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="cc851-575">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="cc851-576">Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="cc851-576">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="cc851-577">Obiekty *przejściowe* są zawsze różne.</span><span class="sxs-lookup"><span data-stu-id="cc851-577">*Transient* objects are always different.</span></span> <span data-ttu-id="cc851-578">Przejściowa wartość `OperationId` dla pierwszych i drugich żądań klienta różni się w zależności od operacji `OperationService` i między żądaniami klientów.</span><span class="sxs-lookup"><span data-stu-id="cc851-578">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="cc851-579">Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="cc851-579">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="cc851-580">Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.</span><span class="sxs-lookup"><span data-stu-id="cc851-580">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="cc851-581">*Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, czy wystąpienie `Operation` jest dostępne w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-581">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="cc851-582">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="cc851-582">Call services from main</span></span>

<span data-ttu-id="cc851-583">Utwórz <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> przy użyciu [IServiceScopeFactory. Isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-583">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="cc851-584">Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="cc851-584">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="cc851-585">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="cc851-585">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="cc851-586">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="cc851-586">Scope validation</span></span>

<span data-ttu-id="cc851-587">Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="cc851-587">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="cc851-588">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="cc851-588">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="cc851-589">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="cc851-589">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="cc851-590">Główny dostawca usług jest tworzony, gdy zostanie wywołane <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*>.</span><span class="sxs-lookup"><span data-stu-id="cc851-590">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="cc851-591">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-591">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="cc851-592">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="cc851-592">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="cc851-593">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="cc851-593">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="cc851-594">Sprawdzanie poprawności zakresów usług przechwytuje te sytuacje w przypadku wywołania `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="cc851-594">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="cc851-595">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="cc851-595">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="cc851-596">Usługi żądania</span><span class="sxs-lookup"><span data-stu-id="cc851-596">Request Services</span></span>

<span data-ttu-id="cc851-597">Usługi dostępne w ramach żądania ASP.NET Core od `HttpContext` są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .</span><span class="sxs-lookup"><span data-stu-id="cc851-597">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="cc851-598">Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-598">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="cc851-599">Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, a nie `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-599">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="cc851-600">Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="cc851-600">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="cc851-601">Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę.</span><span class="sxs-lookup"><span data-stu-id="cc851-601">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="cc851-602">To daje klasy, które są łatwiejsze do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="cc851-602">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="cc851-603">Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp do kolekcji `RequestServices`.</span><span class="sxs-lookup"><span data-stu-id="cc851-603">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="cc851-604">Projektowanie usług do iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="cc851-604">Design services for dependency injection</span></span>

<span data-ttu-id="cc851-605">Najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="cc851-605">Best practices are to:</span></span>

* <span data-ttu-id="cc851-606">Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.</span><span class="sxs-lookup"><span data-stu-id="cc851-606">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="cc851-607">Unikaj stanowych, statycznych klas i elementów członkowskich.</span><span class="sxs-lookup"><span data-stu-id="cc851-607">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="cc851-608">Zaprojektuj aplikacje do korzystania z pojedynczych usług, aby uniknąć tworzenia stanu globalnego.</span><span class="sxs-lookup"><span data-stu-id="cc851-608">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="cc851-609">Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-609">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="cc851-610">Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-610">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="cc851-611">Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.</span><span class="sxs-lookup"><span data-stu-id="cc851-611">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="cc851-612">Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="cc851-612">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="cc851-613">Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="cc851-613">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="cc851-614">Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="cc851-614">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="cc851-615">Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="cc851-615">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="cc851-616">Usuwanie usług</span><span class="sxs-lookup"><span data-stu-id="cc851-616">Disposal of services</span></span>

<span data-ttu-id="cc851-617">Kontener wywołuje <xref:System.IDisposable.Dispose*> dla typów <xref:System.IDisposable>, które tworzy.</span><span class="sxs-lookup"><span data-stu-id="cc851-617">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="cc851-618">Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.</span><span class="sxs-lookup"><span data-stu-id="cc851-618">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="cc851-619">W poniższym przykładzie usługi są tworzone przez kontener usługi i usuwane automatycznie:</span><span class="sxs-lookup"><span data-stu-id="cc851-619">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="cc851-620">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="cc851-620">In the following example:</span></span>

* <span data-ttu-id="cc851-621">Wystąpienia usługi nie są tworzone przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-621">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="cc851-622">Planowane okresy istnienia usług nie są znane przez platformę.</span><span class="sxs-lookup"><span data-stu-id="cc851-622">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="cc851-623">Platforma nie usuwa automatycznie usług.</span><span class="sxs-lookup"><span data-stu-id="cc851-623">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="cc851-624">Jeśli usługi nie zostały jawnie usunięte w kodzie dewelopera, są zachowywane do momentu zamknięcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc851-624">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="cc851-625">Zastępowanie kontenera usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="cc851-625">Default service container replacement</span></span>

<span data-ttu-id="cc851-626">Wbudowany kontener usług został zaprojektowany z myślą o potrzebach platformy i większości aplikacji konsumenckich.</span><span class="sxs-lookup"><span data-stu-id="cc851-626">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="cc851-627">Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, która nie jest obsługiwana przez wbudowany kontener, na przykład:</span><span class="sxs-lookup"><span data-stu-id="cc851-627">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="cc851-628">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="cc851-628">Property injection</span></span>
* <span data-ttu-id="cc851-629">Iniekcja oparta na nazwie</span><span class="sxs-lookup"><span data-stu-id="cc851-629">Injection based on name</span></span>
* <span data-ttu-id="cc851-630">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="cc851-630">Child containers</span></span>
* <span data-ttu-id="cc851-631">Niestandardowe zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="cc851-631">Custom lifetime management</span></span>
* <span data-ttu-id="cc851-632">Obsługa `Func<T>` w przypadku inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="cc851-632">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="cc851-633">Rejestracja oparta na Konwencji</span><span class="sxs-lookup"><span data-stu-id="cc851-633">Convention-based registration</span></span>

<span data-ttu-id="cc851-634">Za pomocą aplikacji ASP.NET Core można używać następujących kontenerów innych firm:</span><span class="sxs-lookup"><span data-stu-id="cc851-634">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="cc851-635">Autofac</span><span class="sxs-lookup"><span data-stu-id="cc851-635">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="cc851-636">DryIoc</span><span class="sxs-lookup"><span data-stu-id="cc851-636">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="cc851-637">Prolongaty</span><span class="sxs-lookup"><span data-stu-id="cc851-637">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="cc851-638">LightInject</span><span class="sxs-lookup"><span data-stu-id="cc851-638">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="cc851-639">Lamar</span><span class="sxs-lookup"><span data-stu-id="cc851-639">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="cc851-640">Stashbox</span><span class="sxs-lookup"><span data-stu-id="cc851-640">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="cc851-641">Unity</span><span class="sxs-lookup"><span data-stu-id="cc851-641">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="cc851-642">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="cc851-642">Thread safety</span></span>

<span data-ttu-id="cc851-643">Twórz bezpieczne dla wątków usługi pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="cc851-643">Create thread-safe singleton services.</span></span> <span data-ttu-id="cc851-644">Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="cc851-644">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="cc851-645">Metoda fabryki pojedynczej usługi, taka jak drugi argument dla [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="cc851-645">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="cc851-646">Podobnie jak w przypadku konstruktora typu (`static`), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="cc851-646">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="cc851-647">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="cc851-647">Recommendations</span></span>

* <span data-ttu-id="cc851-648">Rozpoznawanie usług `async/await` i `Task` nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="cc851-648">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="cc851-649">C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-649">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="cc851-650">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-650">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="cc851-651">Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="cc851-651">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="cc851-652">Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="cc851-652">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="cc851-653">Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="cc851-653">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="cc851-654">Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="cc851-654">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="cc851-655">Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="cc851-655">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="cc851-656">Unikaj używania *wzorca lokalizatora usługi*, który miesza się [z niewersjami strategii kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="cc851-656">Avoid using the *service locator pattern*, which mixes [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

  * <span data-ttu-id="cc851-657">Nie wywołuj <xref:System.IServiceProvider.GetService*>, aby uzyskać wystąpienie usługi, gdy zamiast tego można użyć elementu DI:</span><span class="sxs-lookup"><span data-stu-id="cc851-657">Don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

    <span data-ttu-id="cc851-658">**Prawidłowy**</span><span class="sxs-lookup"><span data-stu-id="cc851-658">**Incorrect:**</span></span>

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

    <span data-ttu-id="cc851-659">**Poprawne**:</span><span class="sxs-lookup"><span data-stu-id="cc851-659">**Correct**:</span></span>

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

  * <span data-ttu-id="cc851-660">Należy unikać wstrzykiwania fabryki, która rozwiązuje zależności w czasie wykonywania przy użyciu <xref:System.IServiceProvider.GetService*>.</span><span class="sxs-lookup"><span data-stu-id="cc851-660">Avoid injecting a factory that resolves dependencies at runtime using <xref:System.IServiceProvider.GetService*>.</span></span>

* <span data-ttu-id="cc851-661">Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="cc851-661">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="cc851-662">Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="cc851-662">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="cc851-663">Wyjątki są rzadkimi&mdash;w większości specjalnych przypadków w ramach samej struktury.</span><span class="sxs-lookup"><span data-stu-id="cc851-663">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="cc851-664">DI jest *alternatywą* dla wzorców dostępu do obiektów static/Global.</span><span class="sxs-lookup"><span data-stu-id="cc851-664">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="cc851-665">Możesz nie być w stanie korzystać z zalet programu DI w przypadku jego mieszania z dostępem do obiektów statycznych.</span><span class="sxs-lookup"><span data-stu-id="cc851-665">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc851-666">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cc851-666">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="cc851-667">Pisanie czystego kodu w ASP.NET Core z iniekcją zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="cc851-667">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="cc851-668">Zasada jawnych zależności</span><span class="sxs-lookup"><span data-stu-id="cc851-668">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="cc851-669">Niewersja kontenerów sterowania i wzorzec iniekcji zależności (Martin Fowlera)</span><span class="sxs-lookup"><span data-stu-id="cc851-669">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="cc851-670">Jak zarejestrować usługę z wieloma interfejsami w ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="cc851-670">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)

::: moniker-end
