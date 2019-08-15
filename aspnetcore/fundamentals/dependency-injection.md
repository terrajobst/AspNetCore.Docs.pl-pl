---
title: Wstrzykiwanie zależności w ASP.NET Core
author: guardrex
description: Dowiedz się, w jaki sposób ASP.NET Core implementuje iniekcję zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: a984bb766e6876db4f8ed4c850a1984ba87d627d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022286"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="da4cc-103">Wstrzykiwanie zależności w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="da4cc-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="da4cc-104">[Steve Kowalski](https://ardalis.com/), [Scott Addie](https://scottaddie.com)i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="da4cc-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="da4cc-105">ASP.NET Core obsługuje wzorzec projektowania oprogramowania dla iniekcji zależności, który jest techniką do osiągnięcia niewersji [kontroli (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależnościami.</span><span class="sxs-lookup"><span data-stu-id="da4cc-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="da4cc-106">Aby uzyskać więcej informacji specyficznych dla iniekcji zależności w kontrolerach MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="da4cc-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="da4cc-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da4cc-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="da4cc-108">Przegląd iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="da4cc-108">Overview of dependency injection</span></span>

<span data-ttu-id="da4cc-109">*Zależność* jest dowolnym obiektem, który jest wymagany przez inny obiekt.</span><span class="sxs-lookup"><span data-stu-id="da4cc-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="da4cc-110">Zapoznaj się `MyDependency` `WriteMessage` z następującą klasą, korzystając z metody, na której zależą inne klasy w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="da4cc-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="da4cc-111">Wystąpienie `MyDependency` klasy można utworzyć w celu `WriteMessage` udostępnienia metody klasie.</span><span class="sxs-lookup"><span data-stu-id="da4cc-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="da4cc-112">Klasa jest zależnością `IndexModel`klasy: `MyDependency`</span><span class="sxs-lookup"><span data-stu-id="da4cc-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="da4cc-113">Klasa tworzy i bezpośrednio zależy `MyDependency` od wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="da4cc-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="da4cc-114">Zależności kodu (takie jak w poprzednim przykładzie) są problematyczne i należy je unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="da4cc-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="da4cc-115">Aby zastąpić `MyDependency` inną implementacją, należy zmodyfikować klasę.</span><span class="sxs-lookup"><span data-stu-id="da4cc-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="da4cc-116">Jeśli `MyDependency` ma zależności, muszą one być skonfigurowane przez klasę.</span><span class="sxs-lookup"><span data-stu-id="da4cc-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="da4cc-117">W dużym projekcie z wieloma klasami `MyDependency`, w zależności od tego, kod konfiguracji jest rozmieszczany w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="da4cc-118">Ta implementacja jest trudna do testowania jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="da4cc-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="da4cc-119">Aplikacja powinna używać klasy imitacji lub zastępczej `MyDependency` , która nie jest możliwa w przypadku tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="da4cc-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="da4cc-120">Iniekcja zależności eliminuje te problemy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="da4cc-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="da4cc-121">Użycie interfejsu lub klasy bazowej do abstrakcyjnej implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="da4cc-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="da4cc-122">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="da4cc-123">ASP.NET Core udostępnia wbudowany kontener usługi, <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="da4cc-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="da4cc-124">Usługi są zarejestrowane w `Startup.ConfigureServices` metodzie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="da4cc-125">*Iniekcja* usługi do konstruktora klasy, w której jest używana.</span><span class="sxs-lookup"><span data-stu-id="da4cc-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="da4cc-126">Struktura przejmuje odpowiedzialność za utworzenie wystąpienia zależności i jego likwidację, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="da4cc-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="da4cc-127">W [przykładowej aplikacji](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) `IMyDependency` interfejs definiuje metodę dostarczaną przez usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="da4cc-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="da4cc-128">Ten interfejs jest implementowany przez konkretny typ `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="da4cc-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="da4cc-129">`MyDependency`<xref:Microsoft.Extensions.Logging.ILogger`1> żąda w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="da4cc-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="da4cc-130">Użycie iniekcji zależności w łańcuchu nie jest nietypowe.</span><span class="sxs-lookup"><span data-stu-id="da4cc-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="da4cc-131">Każda żądana zależność z kolei żąda własnych zależności.</span><span class="sxs-lookup"><span data-stu-id="da4cc-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="da4cc-132">Kontener rozwiązuje zależności w grafie i zwraca w pełni rozwiązane usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="da4cc-133">Zestaw zbiorczy zależności, które muszą zostać rozwiązane, jest zwykle nazywany *drzewem zależności*, *wykresem zależności*lub *wykresem obiektów*.</span><span class="sxs-lookup"><span data-stu-id="da4cc-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="da4cc-134">`IMyDependency`i `ILogger<TCategoryName>` musi być zarejestrowany w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="da4cc-135">`IMyDependency`jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="da4cc-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="da4cc-136">`ILogger<TCategoryName>`jest zarejestrowany przez infrastrukturę abstrakcji rejestrowania, więc jest to [Usługa udostępniona](#framework-provided-services) przez platformę zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="da4cc-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="da4cc-137">Kontener jest rozpoznawany `ILogger<TCategoryName>` przez wykorzystanie [(rodzajowe) otwartych typów](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność zarejestrowania każdego [(rodzajowego) konstruowanego typu](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="da4cc-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="da4cc-138">W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana w konkretnym typie `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="da4cc-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="da4cc-139">Rejestracja zakresów okresu istnienia usługi do okresu istnienia pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="da4cc-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="da4cc-140">[Okresy istnienia usługi](#service-lifetimes) zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="da4cc-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="da4cc-141">Każda `services.Add{SERVICE_NAME}` Metoda rozszerzenia dodaje usługi (i potencjalnie konfiguruje).</span><span class="sxs-lookup"><span data-stu-id="da4cc-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="da4cc-142">Na przykład `services.AddMvc()` dodaje usługi Razor Pages i MVC.</span><span class="sxs-lookup"><span data-stu-id="da4cc-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="da4cc-143">Zalecamy, aby aplikacje były zgodne z tą konwencją.</span><span class="sxs-lookup"><span data-stu-id="da4cc-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="da4cc-144">Umieść metody rozszerzające w przestrzeni nazw [Microsoft. Extensions. DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) , aby hermetyzować grupy rejestracji usług.</span><span class="sxs-lookup"><span data-stu-id="da4cc-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="da4cc-145">Jeśli Konstruktor usługi wymaga [wbudowanego typu](/dotnet/csharp/language-reference/keywords/built-in-types-table), takiego jak `string`,, typ można wstrzyknąć przy użyciu [konfiguracji](xref:fundamentals/configuration/index) lub [wzorca opcji](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="da4cc-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="da4cc-146">Wystąpienie usługi jest wymagane za pośrednictwem konstruktora klasy, w której jest używana usługa i jest przypisywana do pola prywatnego.</span><span class="sxs-lookup"><span data-stu-id="da4cc-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="da4cc-147">To pole jest używane w celu uzyskania dostępu do usługi w zależności od potrzeb w całej klasie.</span><span class="sxs-lookup"><span data-stu-id="da4cc-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="da4cc-148">W przykładowej aplikacji `IMyDependency` wystąpienie jest wymagane i używane do wywołania `WriteMessage` metody usługi:</span><span class="sxs-lookup"><span data-stu-id="da4cc-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="da4cc-149">Usługi udostępniane przez platformę</span><span class="sxs-lookup"><span data-stu-id="da4cc-149">Framework-provided services</span></span>

<span data-ttu-id="da4cc-150">`Startup.ConfigureServices` Metoda jest odpowiedzialna za Definiowanie usług używanych przez aplikację, w tym funkcji platformy, takich jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="da4cc-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="da4cc-151">Początkowo, `IServiceCollection` `ConfigureServices` pod warunkiem, że zostały zdefiniowane następujące usługi (w zależności od [konfiguracji hosta](xref:fundamentals/index#host)):</span><span class="sxs-lookup"><span data-stu-id="da4cc-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="da4cc-152">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="da4cc-152">Service Type</span></span> | <span data-ttu-id="da4cc-153">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="da4cc-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="da4cc-154">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="da4cc-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="da4cc-155">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="da4cc-156">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="da4cc-157">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="da4cc-158">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="da4cc-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="da4cc-159">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="da4cc-160">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="da4cc-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="da4cc-161">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="da4cc-162">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="da4cc-163">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="da4cc-164">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="da4cc-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="da4cc-165">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="da4cc-166">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="da4cc-167">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-167">Singleton</span></span> |

<span data-ttu-id="da4cc-168">Gdy dostępna jest Metoda rozszerzenia kolekcji usług w celu zarejestrowania usługi (i zależnych od niej usług, jeśli jest to wymagane), Konwencja ma używać `Add{SERVICE_NAME}` pojedynczej metody rozszerzającej do rejestrowania wszystkich usług wymaganych przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="da4cc-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="da4cc-169">Poniższy kod stanowi przykład dodawania dodatkowych usług do kontenera przy użyciu metod rozszerzających [\<AddDbContext TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>i <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span><span class="sxs-lookup"><span data-stu-id="da4cc-169">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

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

<span data-ttu-id="da4cc-170">Aby uzyskać więcej informacji, zapoznaj się z <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> klasą w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="da4cc-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="da4cc-171">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="da4cc-171">Service lifetimes</span></span>

<span data-ttu-id="da4cc-172">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="da4cc-173">Usługi ASP.NET Core można skonfigurować przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="da4cc-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="da4cc-174">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="da4cc-174">Transient</span></span>

<span data-ttu-id="da4cc-175">Przejściowe usługi<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>okresu istnienia są tworzone za każdym razem, gdy zażądają one kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="da4cc-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="da4cc-176">Ten okres istnienia działa najlepiej w przypadku lekkich i bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="da4cc-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="da4cc-177">Zakresie</span><span class="sxs-lookup"><span data-stu-id="da4cc-177">Scoped</span></span>

<span data-ttu-id="da4cc-178">Usługi okresu istnienia w<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>zakresie () są tworzone raz dla każdego żądania klienta (połączenie).</span><span class="sxs-lookup"><span data-stu-id="da4cc-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="da4cc-179">W przypadku korzystania z usługi w zakresie w oprogramowaniu pośredniczącym należy wstrzyknąć usługę do `Invoke` metody `InvokeAsync` lub.</span><span class="sxs-lookup"><span data-stu-id="da4cc-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="da4cc-180">Nie wprowadzaj przez iniekcję konstruktora, ponieważ wymusza ona zachowanie usługi jako pojedynczej.</span><span class="sxs-lookup"><span data-stu-id="da4cc-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="da4cc-181">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="da4cc-181">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="da4cc-182">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="da4cc-182">Singleton</span></span>

<span data-ttu-id="da4cc-183">Pojedyncze usługi okresu istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) są tworzone podczas pierwszego żądania (lub gdy `Startup.ConfigureServices` jest uruchamiany, a wystąpienie jest określone przy rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="da4cc-183">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="da4cc-184">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="da4cc-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="da4cc-185">Jeśli aplikacja wymaga pojedynczych zachowań, zaleca się, aby można było zarządzać okresem istnienia usługi przez kontener usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="da4cc-186">Nie Wdrażaj wzorca projektu singleton i podaj kod użytkownika, aby zarządzać okresem istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="da4cc-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="da4cc-187">Rozwiązanie usługi o określonym zakresie z pojedynczej jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="da4cc-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="da4cc-188">Może to spowodować, że usługa będzie mieć nieprawidłowy stan podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="da4cc-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="da4cc-189">Metody rejestracji usług</span><span class="sxs-lookup"><span data-stu-id="da4cc-189">Service registration methods</span></span>

<span data-ttu-id="da4cc-190">Każda metoda rozszerzenia rejestracji usługi oferuje przeciążenia, które są przydatne w określonych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="da4cc-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="da4cc-191">Metoda</span><span class="sxs-lookup"><span data-stu-id="da4cc-191">Method</span></span> | <span data-ttu-id="da4cc-192">Automatyczne</span><span class="sxs-lookup"><span data-stu-id="da4cc-192">Automatic</span></span><br><span data-ttu-id="da4cc-193">object</span><span class="sxs-lookup"><span data-stu-id="da4cc-193">object</span></span><br><span data-ttu-id="da4cc-194">myśl</span><span class="sxs-lookup"><span data-stu-id="da4cc-194">disposal</span></span> | <span data-ttu-id="da4cc-195">Wielokrotne</span><span class="sxs-lookup"><span data-stu-id="da4cc-195">Multiple</span></span><br><span data-ttu-id="da4cc-196">implementacje</span><span class="sxs-lookup"><span data-stu-id="da4cc-196">implementations</span></span> | <span data-ttu-id="da4cc-197">Przekaż argumenty</span><span class="sxs-lookup"><span data-stu-id="da4cc-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="da4cc-198">Przykład:</span><span class="sxs-lookup"><span data-stu-id="da4cc-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="da4cc-199">Tak</span><span class="sxs-lookup"><span data-stu-id="da4cc-199">Yes</span></span> | <span data-ttu-id="da4cc-200">Yes</span><span class="sxs-lookup"><span data-stu-id="da4cc-200">Yes</span></span> | <span data-ttu-id="da4cc-201">Nie</span><span class="sxs-lookup"><span data-stu-id="da4cc-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="da4cc-202">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="da4cc-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="da4cc-203">Tak</span><span class="sxs-lookup"><span data-stu-id="da4cc-203">Yes</span></span> | <span data-ttu-id="da4cc-204">Yes</span><span class="sxs-lookup"><span data-stu-id="da4cc-204">Yes</span></span> | <span data-ttu-id="da4cc-205">Tak</span><span class="sxs-lookup"><span data-stu-id="da4cc-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="da4cc-206">Przykład:</span><span class="sxs-lookup"><span data-stu-id="da4cc-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="da4cc-207">Tak</span><span class="sxs-lookup"><span data-stu-id="da4cc-207">Yes</span></span> | <span data-ttu-id="da4cc-208">Nie</span><span class="sxs-lookup"><span data-stu-id="da4cc-208">No</span></span> | <span data-ttu-id="da4cc-209">Nie</span><span class="sxs-lookup"><span data-stu-id="da4cc-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="da4cc-210">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="da4cc-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="da4cc-211">Nie</span><span class="sxs-lookup"><span data-stu-id="da4cc-211">No</span></span> | <span data-ttu-id="da4cc-212">Yes</span><span class="sxs-lookup"><span data-stu-id="da4cc-212">Yes</span></span> | <span data-ttu-id="da4cc-213">Tak</span><span class="sxs-lookup"><span data-stu-id="da4cc-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="da4cc-214">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="da4cc-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="da4cc-215">Nie</span><span class="sxs-lookup"><span data-stu-id="da4cc-215">No</span></span> | <span data-ttu-id="da4cc-216">Nie</span><span class="sxs-lookup"><span data-stu-id="da4cc-216">No</span></span> | <span data-ttu-id="da4cc-217">Tak</span><span class="sxs-lookup"><span data-stu-id="da4cc-217">Yes</span></span> |

<span data-ttu-id="da4cc-218">Aby uzyskać więcej informacji na temat usuwania typów, zobacz sekcję [dotyczącą usuwania usług](#disposal-of-services) .</span><span class="sxs-lookup"><span data-stu-id="da4cc-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="da4cc-219">Typowym scenariuszem dla wielu implementacji jest [imitacja typów do testowania](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="da4cc-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="da4cc-220">`TryAdd{LIFETIME}`Metody rejestrują usługę tylko wtedy, gdy nie zarejestrowano jeszcze implementacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="da4cc-221">W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency`. `IMyDependency`</span><span class="sxs-lookup"><span data-stu-id="da4cc-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="da4cc-222">Drugi wiersz nie ma wpływu, ponieważ `IMyDependency` ma już zarejestrowaną implementację:</span><span class="sxs-lookup"><span data-stu-id="da4cc-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="da4cc-223">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="da4cc-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="da4cc-224">Metody [TryAddEnumerable (servicedescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) rejestrują usługę tylko wtedy, gdy nie istnieje jeszcze implementacja tego *samego typu*.</span><span class="sxs-lookup"><span data-stu-id="da4cc-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="da4cc-225">Wiele usług jest rozpoznawanych `IEnumerable<{SERVICE}>`za pośrednictwem.</span><span class="sxs-lookup"><span data-stu-id="da4cc-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="da4cc-226">Podczas rejestrowania usług deweloper chce tylko dodać wystąpienie, jeśli jeden z tych samych typów nie został jeszcze dodany.</span><span class="sxs-lookup"><span data-stu-id="da4cc-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="da4cc-227">Ogólnie rzecz biorąc, ta metoda jest używana przez autorów biblioteki, aby uniknąć rejestrowania dwóch kopii wystąpienia w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="da4cc-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="da4cc-228">W poniższym przykładzie pierwszy wiersz rejestruje `MyDep`. `IMyDep1`</span><span class="sxs-lookup"><span data-stu-id="da4cc-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="da4cc-229">Drugi wiersz rejestruje `MyDep`. `IMyDep2`</span><span class="sxs-lookup"><span data-stu-id="da4cc-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="da4cc-230">Trzeci wiersz nie działa, ponieważ `IMyDep1` ma już zarejestrowana `MyDep`implementacja:</span><span class="sxs-lookup"><span data-stu-id="da4cc-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="da4cc-231">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="da4cc-231">Constructor injection behavior</span></span>

<span data-ttu-id="da4cc-232">Usługi mogą być rozwiązywane przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="da4cc-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="da4cc-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>&ndash; Zezwala na tworzenie obiektów bez rejestracji usługi w kontenerze iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="da4cc-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="da4cc-234">`ActivatorUtilities`jest używany z abstrakcjami dostępnymi dla użytkowników, takimi jak pomocnicy tagów, kontrolery MVC i powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="da4cc-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="da4cc-235">Konstruktory mogą akceptować argumenty, które nie są dostarczane przez iniekcję zależności, ale argumenty muszą przypisywać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="da4cc-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="da4cc-236">Gdy usługi są rozwiązane `IServiceProvider` przez `ActivatorUtilities`lub, iniekcja konstruktora wymaga konstruktora *publicznego* .</span><span class="sxs-lookup"><span data-stu-id="da4cc-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="da4cc-237">Gdy usługi są rozwiązane `ActivatorUtilities`przez, iniekcja konstruktora wymaga, aby istnieje tylko jeden odpowiedni Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="da4cc-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="da4cc-238">Przeciążenia konstruktora są obsługiwane, ale może istnieć tylko jedno Przeciążenie, którego argumenty mogą być spełnione przez iniekcję zależności.</span><span class="sxs-lookup"><span data-stu-id="da4cc-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="da4cc-239">Konteksty Entity Framework</span><span class="sxs-lookup"><span data-stu-id="da4cc-239">Entity Framework contexts</span></span>

<span data-ttu-id="da4cc-240">Konteksty Entity Framework są zazwyczaj dodawane do kontenera usługi przy użyciu [okresu istnienia zakresu](#service-lifetimes) , ponieważ operacje bazy danych aplikacji sieci Web są zwykle ograniczone do żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="da4cc-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="da4cc-241">Domyślny okres istnienia jest objęty zakresem, jeśli okres istnienia nie jest określony przez Przeciążenie [\<AddDbContext TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Overload podczas rejestrowania kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="da4cc-241">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="da4cc-242">Usługi danego okresu istnienia nie powinny używać kontekstu bazy danych z krótszym okresem istnienia niż usługa.</span><span class="sxs-lookup"><span data-stu-id="da4cc-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="da4cc-243">Opcje okresu istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="da4cc-243">Lifetime and registration options</span></span>

<span data-ttu-id="da4cc-244">Aby zademonstrować różnicę między opcjami czasu istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacje `OperationId`z unikatowym identyfikatorem.</span><span class="sxs-lookup"><span data-stu-id="da4cc-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="da4cc-245">W zależności od sposobu skonfigurowania okresu istnienia usługi operacji dla następujących interfejsów kontener zawiera to samo lub inne wystąpienie usługi, gdy żądanie klasy:</span><span class="sxs-lookup"><span data-stu-id="da4cc-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="da4cc-246">Interfejsy są zaimplementowane w `Operation` klasie.</span><span class="sxs-lookup"><span data-stu-id="da4cc-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="da4cc-247">`Operation` Konstruktor generuje identyfikator GUID, jeśli nie został podany:</span><span class="sxs-lookup"><span data-stu-id="da4cc-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="da4cc-248">Zarejestrowano, który zależy od poszczególnych `Operation` typów. `OperationService`</span><span class="sxs-lookup"><span data-stu-id="da4cc-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="da4cc-249">Gdy `OperationService` żądanie jest wykonywane za pośrednictwem iniekcji zależności, otrzymuje nowe wystąpienie każdej usługi lub istniejące wystąpienie na podstawie okresu istnienia usługi zależnej.</span><span class="sxs-lookup"><span data-stu-id="da4cc-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="da4cc-250">Gdy usługi przejściowe są tworzone po zażądaniu kontenera, `OperationId` `IOperationTransient` `OperationId` usługa różni się od `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="da4cc-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="da4cc-251">`OperationService`odbiera nowe wystąpienie `IOperationTransient` klasy.</span><span class="sxs-lookup"><span data-stu-id="da4cc-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="da4cc-252">Nowe wystąpienie daje inną `OperationId`wartość.</span><span class="sxs-lookup"><span data-stu-id="da4cc-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="da4cc-253">Gdy usługi w zakresie są tworzone dla każdego żądania klienta, `OperationId` `IOperationScoped` usługa jest taka sama jak `OperationService` w ramach żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="da4cc-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="da4cc-254">W przypadku żądań klientów obie te usługi współdzielą inną `OperationId` wartość.</span><span class="sxs-lookup"><span data-stu-id="da4cc-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="da4cc-255">Gdy pojedyncze i pojedyncze usługi wystąpienia są tworzone raz i używane przez wszystkie żądania klientów i wszystkie usługi, `OperationId` jest to stała między wszystkimi żądaniami obsługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="da4cc-256">W `Startup.ConfigureServices`programie każdy typ jest dodawany do kontenera zgodnie z jego nazwanym okresem istnienia:</span><span class="sxs-lookup"><span data-stu-id="da4cc-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="da4cc-257">Usługa używa określonego wystąpienia o znanym `Guid.Empty`identyfikatorze. `IOperationSingletonInstance`</span><span class="sxs-lookup"><span data-stu-id="da4cc-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="da4cc-258">Jest to jasne, gdy ten typ jest używany (jego identyfikator GUID to wszystkie zera).</span><span class="sxs-lookup"><span data-stu-id="da4cc-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="da4cc-259">Przykładowa aplikacja pokazuje okresy istnienia obiektów w ramach poszczególnych żądań i między nimi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="da4cc-260">Przykładowa aplikacja `IndexModel` wysyła żądania każdego `IOperation` rodzaju typu i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="da4cc-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="da4cc-261">Następnie na stronie zostaną wyświetlone wszystkie `OperationId` wartości klasy modelu strony i usługi za pomocą przypisań właściwości:</span><span class="sxs-lookup"><span data-stu-id="da4cc-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="da4cc-262">Dwa następujące dane wyjściowe pokazują wyniki dwóch żądań:</span><span class="sxs-lookup"><span data-stu-id="da4cc-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="da4cc-263">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="da4cc-263">**First request:**</span></span>

<span data-ttu-id="da4cc-264">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="da4cc-264">Controller operations:</span></span>

<span data-ttu-id="da4cc-265">Przejściowy: d233e165-f417-469B-A866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="da4cc-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="da4cc-266">Zakresie 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="da4cc-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="da4cc-267">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="da4cc-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="da4cc-268">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="da4cc-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="da4cc-269">`OperationService`składowa</span><span class="sxs-lookup"><span data-stu-id="da4cc-269">`OperationService` operations:</span></span>

<span data-ttu-id="da4cc-270">Przejściowy: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="da4cc-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="da4cc-271">Zakresie 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="da4cc-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="da4cc-272">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="da4cc-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="da4cc-273">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="da4cc-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="da4cc-274">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="da4cc-274">**Second request:**</span></span>

<span data-ttu-id="da4cc-275">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="da4cc-275">Controller operations:</span></span>

<span data-ttu-id="da4cc-276">Przejściowy: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="da4cc-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="da4cc-277">Zakresie 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="da4cc-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="da4cc-278">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="da4cc-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="da4cc-279">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="da4cc-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="da4cc-280">`OperationService`składowa</span><span class="sxs-lookup"><span data-stu-id="da4cc-280">`OperationService` operations:</span></span>

<span data-ttu-id="da4cc-281">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="da4cc-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="da4cc-282">Zakresie 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="da4cc-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="da4cc-283">Pojedynczego 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="da4cc-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="da4cc-284">Np 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="da4cc-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="da4cc-285">Zauważ, że wartości `OperationId` różnią się w zależności od żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="da4cc-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="da4cc-286">Obiekty przejściowe są zawsze różne.</span><span class="sxs-lookup"><span data-stu-id="da4cc-286">*Transient* objects are always different.</span></span> <span data-ttu-id="da4cc-287">Wartość przejściowa `OperationId` dla pierwszego i drugiego żądania klienta różni się `OperationService` w zależności od operacji i między żądaniami klientów.</span><span class="sxs-lookup"><span data-stu-id="da4cc-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="da4cc-288">Nowe wystąpienie jest dostarczane do każdego żądania obsługi i żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="da4cc-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="da4cc-289">Obiekty w *zakresie* są takie same w obrębie żądania klienta, ale różnią się w zależności od żądań klientów.</span><span class="sxs-lookup"><span data-stu-id="da4cc-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="da4cc-290">*Pojedyncze* obiekty są takie same dla każdego obiektu i każdego żądania, niezależnie od tego, `Operation` czy wystąpienie jest dostarczone `Startup.ConfigureServices`w.</span><span class="sxs-lookup"><span data-stu-id="da4cc-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="da4cc-291">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="da4cc-291">Call services from main</span></span>

<span data-ttu-id="da4cc-292">Utwórz element <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory. isscope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) , aby rozwiązać usługę objętą zakresem w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="da4cc-293">Takie podejście jest przydatne do uzyskiwania dostępu do usługi w zakresie podczas uruchamiania do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="da4cc-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="da4cc-294">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService`: `Program.Main`</span><span class="sxs-lookup"><span data-stu-id="da4cc-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="da4cc-295">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="da4cc-295">Scope validation</span></span>

<span data-ttu-id="da4cc-296">Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług sprawdza, czy:</span><span class="sxs-lookup"><span data-stu-id="da4cc-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="da4cc-297">Usługi w zakresie nie są bezpośrednio lub pośrednio rozpoznawane przez dostawcę usług głównych.</span><span class="sxs-lookup"><span data-stu-id="da4cc-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="da4cc-298">Usługi w zakresie nie są bezpośrednio lub pośrednio wstrzykiwane do pojedynczych.</span><span class="sxs-lookup"><span data-stu-id="da4cc-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="da4cc-299">Dostawca usług głównych jest tworzony, gdy <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> jest wywoływany.</span><span class="sxs-lookup"><span data-stu-id="da4cc-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="da4cc-300">Okres istnienia dostawcy usług głównych odnosi się do okresu istnienia aplikacji/serwera, gdy dostawca zaczyna się od aplikacji i jest usuwany po zamknięciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="da4cc-301">Usługi o określonym zakresie są usuwane przez kontener, który go utworzył.</span><span class="sxs-lookup"><span data-stu-id="da4cc-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="da4cc-302">Jeśli w kontenerze głównym zostanie utworzona usługa o określonym zakresie, okres istnienia usługi zostanie skutecznie podwyższony do pojedynczej, ponieważ jest usuwany tylko przez kontener główny, gdy aplikacja/serwer jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="da4cc-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="da4cc-303">Sprawdzanie poprawności zakresów usług przechwytuje te `BuildServiceProvider` sytuacje, gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="da4cc-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="da4cc-304">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="da4cc-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="da4cc-305">Usługi żądania</span><span class="sxs-lookup"><span data-stu-id="da4cc-305">Request Services</span></span>

<span data-ttu-id="da4cc-306">Usługi dostępne w ramach żądania `HttpContext` ASP.NET Core są udostępniane za pośrednictwem kolekcji [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) .</span><span class="sxs-lookup"><span data-stu-id="da4cc-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="da4cc-307">Usługi żądania reprezentują usługi skonfigurowane i żądane jako część aplikacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="da4cc-308">Gdy obiekty określają zależności, są one spełnione przez typy Znalezione w `RequestServices`, nie. `ApplicationServices`</span><span class="sxs-lookup"><span data-stu-id="da4cc-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="da4cc-309">Ogólnie rzecz biorąc aplikacja nie powinna używać tych właściwości bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="da4cc-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="da4cc-310">Zamiast tego Zażądaj typów, które klasy wymagają za pośrednictwem konstruktorów klas, i zezwól na wstrzyknięcie zależności przez strukturę.</span><span class="sxs-lookup"><span data-stu-id="da4cc-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="da4cc-311">To daje klasy, które są łatwiejsze do przetestowania.</span><span class="sxs-lookup"><span data-stu-id="da4cc-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="da4cc-312">Preferuj żądania zależności jako parametry konstruktora, aby uzyskać dostęp `RequestServices` do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="da4cc-313">Projektowanie usług do iniekcji zależności</span><span class="sxs-lookup"><span data-stu-id="da4cc-313">Design services for dependency injection</span></span>

<span data-ttu-id="da4cc-314">Najlepsze rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="da4cc-314">Best practices are to:</span></span>

* <span data-ttu-id="da4cc-315">Projektowanie usług do korzystania z iniekcji zależności w celu uzyskania ich zależności.</span><span class="sxs-lookup"><span data-stu-id="da4cc-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="da4cc-316">Unikaj stanowych wywołań metod statycznych.</span><span class="sxs-lookup"><span data-stu-id="da4cc-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="da4cc-317">Unikaj bezpośredniego tworzenia wystąpień klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="da4cc-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="da4cc-318">Bezpośrednie utworzenie wystąpienia Couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="da4cc-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="da4cc-319">Twórz klasy aplikacji małymi, dobrze i łatwo przetestowane.</span><span class="sxs-lookup"><span data-stu-id="da4cc-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="da4cc-320">Jeśli Klasa prawdopodobnie ma zbyt wiele zawstrzykiwanych zależności, zwykle jest to znak, że Klasa ma zbyt wiele obowiązków i narusza [zasadę pojedynczej odpowiedzialności (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="da4cc-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="da4cc-321">Próba refaktoryzacji klasy przez przeniesienie niektórych jej obowiązków do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="da4cc-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="da4cc-322">Należy pamiętać, że klasy Razor Pages modelu strony i kontrolery MVC powinny skupić się na problemach z interfejsem użytkownika.</span><span class="sxs-lookup"><span data-stu-id="da4cc-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="da4cc-323">Reguły biznesowe i szczegóły implementacji dostępu do danych powinny być przechowywane w klasach odpowiednich do tych [oddzielnych obaw](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="da4cc-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="da4cc-324">Usuwanie usług</span><span class="sxs-lookup"><span data-stu-id="da4cc-324">Disposal of services</span></span>

<span data-ttu-id="da4cc-325">Kontener wywołuje <xref:System.IDisposable.Dispose*> typy, które <xref:System.IDisposable> tworzy.</span><span class="sxs-lookup"><span data-stu-id="da4cc-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="da4cc-326">Jeśli wystąpienie jest dodawane do kontenera przez kod użytkownika, nie zostanie usunięte automatycznie.</span><span class="sxs-lookup"><span data-stu-id="da4cc-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="da4cc-327">Zastępowanie kontenera usług domyślnych</span><span class="sxs-lookup"><span data-stu-id="da4cc-327">Default service container replacement</span></span>

<span data-ttu-id="da4cc-328">Wbudowany kontener usługi jest przeznaczony do obsługi potrzeb platformy i większości aplikacji konsumenckich.</span><span class="sxs-lookup"><span data-stu-id="da4cc-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="da4cc-329">Zalecamy użycie wbudowanego kontenera, chyba że potrzebna jest konkretna funkcja, której nie obsługuje.</span><span class="sxs-lookup"><span data-stu-id="da4cc-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="da4cc-330">Niektóre funkcje obsługiwane w kontenerach innych firm nie znajdują się w kontenerze wbudowanym:</span><span class="sxs-lookup"><span data-stu-id="da4cc-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="da4cc-331">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="da4cc-331">Property injection</span></span>
* <span data-ttu-id="da4cc-332">Iniekcja oparta na nazwie</span><span class="sxs-lookup"><span data-stu-id="da4cc-332">Injection based on name</span></span>
* <span data-ttu-id="da4cc-333">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="da4cc-333">Child containers</span></span>
* <span data-ttu-id="da4cc-334">Niestandardowe zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="da4cc-334">Custom lifetime management</span></span>
* <span data-ttu-id="da4cc-335">`Func<T>`Obsługa inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="da4cc-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="da4cc-336">Zobacz [plik Readme.MD iniekcji zależności](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) , aby uzyskać listę niektórych kontenerów, które obsługują karty sieciowe.</span><span class="sxs-lookup"><span data-stu-id="da4cc-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="da4cc-337">Poniższy przykład zastępuje wbudowany kontener z [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="da4cc-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="da4cc-338">Zainstaluj odpowiednie pakiety kontenerów:</span><span class="sxs-lookup"><span data-stu-id="da4cc-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="da4cc-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="da4cc-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="da4cc-340">Autofac. Extensions. DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="da4cc-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="da4cc-341">Skonfigurowanie kontenera w programie `Startup.ConfigureServices` i `IServiceProvider`zwrócenie:</span><span class="sxs-lookup"><span data-stu-id="da4cc-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="da4cc-342">Aby użyć kontenera innej firmy, `Startup.ConfigureServices` należy zwrócić `IServiceProvider`wartość.</span><span class="sxs-lookup"><span data-stu-id="da4cc-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="da4cc-343">Skonfiguruj Autofac w `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="da4cc-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="da4cc-344">W czasie wykonywania Autofac jest używany do rozpoznawania typów i wtryskiwania zależności.</span><span class="sxs-lookup"><span data-stu-id="da4cc-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="da4cc-345">Aby dowiedzieć się więcej o korzystaniu z programu Autofac z ASP.NET Core, zobacz [dokumentację Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="da4cc-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="da4cc-346">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="da4cc-346">Thread safety</span></span>

<span data-ttu-id="da4cc-347">Twórz bezpieczne dla wątków usługi pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="da4cc-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="da4cc-348">Jeśli usługa singleton ma zależność od przejściowej usługi, usługa przejściowa może również wymagać bezpieczeństwa wątku, w zależności od tego, w jaki sposób jest używana przez pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="da4cc-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="da4cc-349">Metoda fabryki pojedynczej usługi, taka jak drugi argument dla AddSingleton [\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być bezpieczna wątkowo.</span><span class="sxs-lookup"><span data-stu-id="da4cc-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="da4cc-350">Podobnie jak w przypadku`static`konstruktora typu (), gwarantowane jest wywoływanie jednokrotne przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="da4cc-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="da4cc-351">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="da4cc-351">Recommendations</span></span>

* <span data-ttu-id="da4cc-352">`async/await`Rozpoznawanie `Task` usług na podstawie nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="da4cc-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="da4cc-353">C#nie obsługuje konstruktorów asynchronicznych; w związku z tym zalecany wzorzec polega na użyciu metod asynchronicznych po synchronicznym rozpoznaniu usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="da4cc-354">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="da4cc-355">Na przykład koszyk użytkownika nie powinien być zazwyczaj dodawany do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="da4cc-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="da4cc-356">Konfiguracja powinna używać [wzorca opcji](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="da4cc-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="da4cc-357">Podobnie należy unikać obiektów "posiadaczy danych", które istnieją tylko w celu zezwolenia na dostęp do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="da4cc-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="da4cc-358">Lepiej jest zażądać rzeczywistego elementu za pośrednictwem DI.</span><span class="sxs-lookup"><span data-stu-id="da4cc-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="da4cc-359">Należy unikać statycznego dostępu do usług (na przykład statycznego wpisywania [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) do użytku w innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="da4cc-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="da4cc-360">Unikaj używania *wzorca lokalizatora usługi*.</span><span class="sxs-lookup"><span data-stu-id="da4cc-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="da4cc-361">Na przykład nie wywołuj <xref:System.IServiceProvider.GetService*> , aby uzyskać wystąpienie usługi, gdy można użyć di zamiast:</span><span class="sxs-lookup"><span data-stu-id="da4cc-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="da4cc-362">**Prawidłowy**</span><span class="sxs-lookup"><span data-stu-id="da4cc-362">**Incorrect:**</span></span>

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

  <span data-ttu-id="da4cc-363">**Poprawne**:</span><span class="sxs-lookup"><span data-stu-id="da4cc-363">**Correct**:</span></span>

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

* <span data-ttu-id="da4cc-364">Inna odmiana lokalizatora usługi, aby uniknąć, wprowadza fabrykę, która rozwiązuje zależności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="da4cc-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="da4cc-365">Obie te praktyki mieszają się z niewersjami strategii [kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="da4cc-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="da4cc-366">Unikaj dostępu statycznego do `HttpContext` (na przykład [IHttpContextAccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="da4cc-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="da4cc-367">Podobnie jak w przypadku wszystkich zestawów zaleceń, mogą wystąpić sytuacje, w których ignorowanie zalecenia jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="da4cc-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="da4cc-368">Wyjątki są rzadko&mdash;w większości specjalnych przypadków w ramach samej struktury.</span><span class="sxs-lookup"><span data-stu-id="da4cc-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="da4cc-369">DI jest *alternatywą* dla wzorców dostępu do obiektów static/Global.</span><span class="sxs-lookup"><span data-stu-id="da4cc-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="da4cc-370">Możesz nie być w stanie korzystać z zalet programu DI w przypadku jego mieszania z dostępem do obiektów statycznych.</span><span class="sxs-lookup"><span data-stu-id="da4cc-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da4cc-371">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="da4cc-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="da4cc-372">Pisanie czystego kodu w ASP.NET Core z iniekcją zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="da4cc-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="da4cc-373">Zasada jawnych zależności</span><span class="sxs-lookup"><span data-stu-id="da4cc-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="da4cc-374">Niewersja kontenerów sterowania i wzorzec iniekcji zależności (Martin Fowlera)</span><span class="sxs-lookup"><span data-stu-id="da4cc-374">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="da4cc-375">Jak zarejestrować usługę z wieloma interfejsami w ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="da4cc-375">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
