---
title: Wstrzykiwanie zależności w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak platformy ASP.NET Core implementuje wstrzykiwanie zależności i jak z niej korzystać.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: f1282e9bf34a96e9495f35dcb51974594c938fde
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814800"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="00d5c-103">Wstrzykiwanie zależności w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00d5c-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="00d5c-104">Przez [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="00d5c-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="00d5c-105">Platforma ASP.NET Core obsługuje zależności wzorzec projektowy oprogramowania iniekcji (DI), czyli technikę do osiągnięcia [Inwersja kontroli (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) między klasami i ich zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="00d5c-106">Aby uzyskać więcej informacji specyficznych dla wstrzykiwanie zależności w ramach kontrolerów MVC, zobacz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="00d5c-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="00d5c-107">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00d5c-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="00d5c-108">Omówienie wstrzykiwanie zależności</span><span class="sxs-lookup"><span data-stu-id="00d5c-108">Overview of dependency injection</span></span>

<span data-ttu-id="00d5c-109">A *zależności* jest dowolny obiekt, który wymaga innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="00d5c-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="00d5c-110">Sprawdź następujące `MyDependency` klasy `WriteMessage` metody, które zależą od innych klas w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="00d5c-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="00d5c-111">Wystąpienie `MyDependency` klasy mogą być tworzone się `WriteMessage` klasy dostępnej metody.</span><span class="sxs-lookup"><span data-stu-id="00d5c-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="00d5c-112">`MyDependency` Klasa jest zależą od elementu `IndexModel` klasy:</span><span class="sxs-lookup"><span data-stu-id="00d5c-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="00d5c-113">Klasa tworzy i zależy od bezpośrednio `MyDependency` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="00d5c-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="00d5c-114">Zależności w kodzie (na przykład w poprzednim przykładzie) są problematyczne i należy unikać z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="00d5c-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="00d5c-115">Aby zastąpić `MyDependency` z inną implementacją klasy muszą zostać zmodyfikowane.</span><span class="sxs-lookup"><span data-stu-id="00d5c-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="00d5c-116">Jeśli `MyDependency` ma zależności, musi być skonfigurowany przez klasę.</span><span class="sxs-lookup"><span data-stu-id="00d5c-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="00d5c-117">W dużym projekcie z wieloma klasami w zależności od `MyDependency`, kod konfiguracji staje się znajdują się na aplikację.</span><span class="sxs-lookup"><span data-stu-id="00d5c-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="00d5c-118">Ta implementacja jest trudny do testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="00d5c-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="00d5c-119">Aplikację należy użyć pozorny lub namiastki `MyDependency` klasy, która nie jest możliwe w przypadku tej metody.</span><span class="sxs-lookup"><span data-stu-id="00d5c-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="00d5c-120">Wstrzykiwanie zależności rozwiązuje te problemy za pomocą:</span><span class="sxs-lookup"><span data-stu-id="00d5c-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="00d5c-121">Użycie interfejsu lub klasy bazowej tworzących warstwę abstrakcji implementacji zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="00d5c-122">Rejestracja zależności w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="00d5c-123">Platforma ASP.NET Core zapewnia kontener wbudowanej usługi <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="00d5c-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="00d5c-124">Usługi są zarejestrowane w usłudze aplikacji `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="00d5c-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="00d5c-125">*Iniekcja* usługi do konstruktora klasy, w których jest używany.</span><span class="sxs-lookup"><span data-stu-id="00d5c-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="00d5c-126">Struktura przejmuje odpowiedzialność za tworzenie wystąpienia zależności i usuwania je, gdy nie jest już potrzebny.</span><span class="sxs-lookup"><span data-stu-id="00d5c-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="00d5c-127">W [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` interfejs definiuje metodę, która udostępnia usługę do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="00d5c-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="00d5c-128">Ten interfejs jest implementowany przez konkretny typ `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="00d5c-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="00d5c-129">`MyDependency` żądania <xref:Microsoft.Extensions.Logging.ILogger`1> w jego konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="00d5c-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="00d5c-130">Nie jest niczym niezwykłym używać wstrzykiwanie zależności w sposób połączonych.</span><span class="sxs-lookup"><span data-stu-id="00d5c-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="00d5c-131">Poszczególne zależności żądanego żądań z kolei swoje własne zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="00d5c-132">Jest rozpoznawana jako zależności na wykresie i zwraca w pełni rozpoznać usługę kontenera.</span><span class="sxs-lookup"><span data-stu-id="00d5c-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="00d5c-133">Zbiorczy zestaw zależności, które muszą być rozwiązane jest zwykle nazywany *drzewo zależności*, *wykres zależności*, lub *wykresu obiektu*.</span><span class="sxs-lookup"><span data-stu-id="00d5c-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="00d5c-134">`IMyDependency` i `ILogger<TCategoryName>` musi być zarejestrowana w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="00d5c-135">`IMyDependency` jest zarejestrowany w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="00d5c-136">`ILogger<TCategoryName>` jest on zarejestrowany infrastruktury abstrakcje rejestrowanie, dlatego ma [usługi dostarczane przez framework](#framework-provided-services) zarejestrowana domyślnie przez platformę.</span><span class="sxs-lookup"><span data-stu-id="00d5c-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="00d5c-137">Kontener jest rozpoznawana jako `ILogger<TCategoryName>` , wykorzystując [typy (Ogólne) otwórz](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminując konieczność łączenia się zarejestrować co [(ogólny) skonstruowany typ](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="00d5c-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="00d5c-138">W przykładowej aplikacji `IMyDependency` usługa jest zarejestrowana przy użyciu konkretnego typu `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="00d5c-139">Rejestracja zakresów okres istnienia usługi przez cały czas trwania pojedynczego żądania.</span><span class="sxs-lookup"><span data-stu-id="00d5c-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="00d5c-140">[Okresy istnienia usługi](#service-lifetimes) są opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="00d5c-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="00d5c-141">Każdy `services.Add{SERVICE_NAME}` — metoda rozszerzenia dodaje (i potencjalnie konfiguruje) usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="00d5c-142">Na przykład `services.AddMvc()` dodaje usług, stronami Razor i wymagają MVC.</span><span class="sxs-lookup"><span data-stu-id="00d5c-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="00d5c-143">Zaleca się, że aplikacje stosują taką Konwencję.</span><span class="sxs-lookup"><span data-stu-id="00d5c-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="00d5c-144">Metody rozszerzające w miejscu <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> przestrzeni nazw w celu hermetyzacji grupy rejestracji usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-144">Place extension methods in the <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="00d5c-145">Jeśli Konstruktor usługi wymaga [typ wbudowany](/dotnet/csharp/language-reference/keywords/built-in-types-table), takich jak `string`, typ może wprowadzone za pomocą [konfiguracji](xref:fundamentals/configuration/index) lub [wzorzec opcje](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="00d5c-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="00d5c-146">Za pośrednictwem konstruktora klasy, gdzie usługa jest używana i przypisane do pola prywatnego, wymagane jest wystąpienie usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="00d5c-147">Pole jest używane do uzyskania dostępu do usługi zgodnie z potrzebami w całej klasy.</span><span class="sxs-lookup"><span data-stu-id="00d5c-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="00d5c-148">W przykładowej aplikacji `IMyDependency` wystąpienie jest wymagane i używane do wywołania tej usługi `WriteMessage` metody:</span><span class="sxs-lookup"><span data-stu-id="00d5c-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="00d5c-149">Dostarczone do struktury usługi</span><span class="sxs-lookup"><span data-stu-id="00d5c-149">Framework-provided services</span></span>

<span data-ttu-id="00d5c-150">`Startup.ConfigureServices` Metoda jest odpowiedzialna definiowanie usługi, którego korzysta aplikacja, łącznie z funkcjami platformy, takie jak Entity Framework Core i ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="00d5c-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="00d5c-151">Początkowo `IServiceCollection` udostępniane `ConfigureServices` ma następujące usługi, które są zdefiniowane (w zależności od [konfiguracji hosta](xref:fundamentals/index#host)):</span><span class="sxs-lookup"><span data-stu-id="00d5c-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="00d5c-152">Typ usługi</span><span class="sxs-lookup"><span data-stu-id="00d5c-152">Service Type</span></span> | <span data-ttu-id="00d5c-153">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="00d5c-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="00d5c-154">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="00d5c-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="00d5c-155">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="00d5c-156">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="00d5c-157">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="00d5c-158">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="00d5c-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="00d5c-159">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="00d5c-160">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="00d5c-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="00d5c-161">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="00d5c-162">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="00d5c-163">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="00d5c-164">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="00d5c-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="00d5c-165">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="00d5c-166">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="00d5c-167">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-167">Singleton</span></span> |

<span data-ttu-id="00d5c-168">Po udostępnieniu register a service (i jej usługi zależne, jeśli jest to wymagane) metody rozszerzenia kolekcji usługi Konwencji jest użycie pojedynczego `Add{SERVICE_NAME}` metodę rozszerzenia, aby zarejestrować wszystkich usług wymaganych przez tę usługę.</span><span class="sxs-lookup"><span data-stu-id="00d5c-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="00d5c-169">Poniższy kod jest przykładem sposobu dodawania dodatkowych usług do kontenera przy użyciu metody rozszerzenia [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, i <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span><span class="sxs-lookup"><span data-stu-id="00d5c-169">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

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

<span data-ttu-id="00d5c-170">Aby uzyskać więcej informacji, zobacz <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> klasy w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="00d5c-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="00d5c-171">Okresy istnienia usługi</span><span class="sxs-lookup"><span data-stu-id="00d5c-171">Service lifetimes</span></span>

<span data-ttu-id="00d5c-172">Wybierz odpowiedni okres istnienia dla każdej zarejestrowanej usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="00d5c-173">Usługi ASP.NET Core mogą być skonfigurowane przy użyciu następujących okresów istnienia:</span><span class="sxs-lookup"><span data-stu-id="00d5c-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="00d5c-174">Przejściowe</span><span class="sxs-lookup"><span data-stu-id="00d5c-174">Transient</span></span>

<span data-ttu-id="00d5c-175">Usługi przejściowych okres istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) są tworzone za każdym razem, zleconej z kontenera usług.</span><span class="sxs-lookup"><span data-stu-id="00d5c-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="00d5c-176">Ten okres istnienia najlepiej uproszczone, bezstanowych usług.</span><span class="sxs-lookup"><span data-stu-id="00d5c-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="00d5c-177">O określonym zakresie</span><span class="sxs-lookup"><span data-stu-id="00d5c-177">Scoped</span></span>

<span data-ttu-id="00d5c-178">Zakres usług okres istnienia (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) są tworzone w jeden raz dla każdego żądania klienta (połączenie).</span><span class="sxs-lookup"><span data-stu-id="00d5c-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="00d5c-179">Korzystając z usługi o określonym zakresie w oprogramowaniu pośredniczącym, wprowadzić usługę do `Invoke` lub `InvokeAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="00d5c-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="00d5c-180">Nie wstrzyknąć przy użyciu iniekcji konstruktora, ponieważ wymusza usługę, aby zachowywać się jak wzorzec singleton.</span><span class="sxs-lookup"><span data-stu-id="00d5c-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="00d5c-181">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="00d5c-181">For more information, see <xref:fundamentals/middleware/index>.</span></span>

### <a name="singleton"></a><span data-ttu-id="00d5c-182">pojedyncze</span><span class="sxs-lookup"><span data-stu-id="00d5c-182">Singleton</span></span>

<span data-ttu-id="00d5c-183">Pojedyncze okres istnienia usługi (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) są tworzone po raz pierwszy masz żądanej (lub gdy `Startup.ConfigureServices` jest uruchamiany i wystąpienie jest określony za pomocą rejestracji usługi).</span><span class="sxs-lookup"><span data-stu-id="00d5c-183">Singleton lifetime services (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="00d5c-184">Każde kolejne żądanie używa tego samego wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="00d5c-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="00d5c-185">Jeśli aplikacja wymaga pojedynczego zachowanie, umożliwiając kontener usługi zarządzać okresem istnienia usługi jest zalecane.</span><span class="sxs-lookup"><span data-stu-id="00d5c-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="00d5c-186">Nie implementuje wzorzec projektowy pojedyncze i podać kod użytkownika do zarządzania okres istnienia obiektu w klasie.</span><span class="sxs-lookup"><span data-stu-id="00d5c-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="00d5c-187">Niebezpiecznie usługi o określonym zakresie z pojedynczego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="00d5c-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="00d5c-188">Może to spowodować usługi, aby nieprawidłowym stanie podczas przetwarzania kolejnych żądań.</span><span class="sxs-lookup"><span data-stu-id="00d5c-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="00d5c-189">Metody rejestracji usługi</span><span class="sxs-lookup"><span data-stu-id="00d5c-189">Service registration methods</span></span>

<span data-ttu-id="00d5c-190">Każda metoda rozszerzenia rejestracji usługa oferuje przeciążenia, które są przydatne w określonych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="00d5c-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="00d5c-191">Metoda</span><span class="sxs-lookup"><span data-stu-id="00d5c-191">Method</span></span> | <span data-ttu-id="00d5c-192">Automatyczne</span><span class="sxs-lookup"><span data-stu-id="00d5c-192">Automatic</span></span><br><span data-ttu-id="00d5c-193">object</span><span class="sxs-lookup"><span data-stu-id="00d5c-193">object</span></span><br><span data-ttu-id="00d5c-194">likwidacji</span><span class="sxs-lookup"><span data-stu-id="00d5c-194">disposal</span></span> | <span data-ttu-id="00d5c-195">Wielokrotne</span><span class="sxs-lookup"><span data-stu-id="00d5c-195">Multiple</span></span><br><span data-ttu-id="00d5c-196">implementacje</span><span class="sxs-lookup"><span data-stu-id="00d5c-196">implementations</span></span> | <span data-ttu-id="00d5c-197">Przekazywanie argumentów</span><span class="sxs-lookup"><span data-stu-id="00d5c-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="00d5c-198">Przykład:</span><span class="sxs-lookup"><span data-stu-id="00d5c-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="00d5c-199">Yes</span><span class="sxs-lookup"><span data-stu-id="00d5c-199">Yes</span></span> | <span data-ttu-id="00d5c-200">Yes</span><span class="sxs-lookup"><span data-stu-id="00d5c-200">Yes</span></span> | <span data-ttu-id="00d5c-201">Nie</span><span class="sxs-lookup"><span data-stu-id="00d5c-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="00d5c-202">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="00d5c-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="00d5c-203">Tak</span><span class="sxs-lookup"><span data-stu-id="00d5c-203">Yes</span></span> | <span data-ttu-id="00d5c-204">Yes</span><span class="sxs-lookup"><span data-stu-id="00d5c-204">Yes</span></span> | <span data-ttu-id="00d5c-205">Tak</span><span class="sxs-lookup"><span data-stu-id="00d5c-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="00d5c-206">Przykład:</span><span class="sxs-lookup"><span data-stu-id="00d5c-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="00d5c-207">Tak</span><span class="sxs-lookup"><span data-stu-id="00d5c-207">Yes</span></span> | <span data-ttu-id="00d5c-208">Nie</span><span class="sxs-lookup"><span data-stu-id="00d5c-208">No</span></span> | <span data-ttu-id="00d5c-209">Nie</span><span class="sxs-lookup"><span data-stu-id="00d5c-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="00d5c-210">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="00d5c-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="00d5c-211">Nie</span><span class="sxs-lookup"><span data-stu-id="00d5c-211">No</span></span> | <span data-ttu-id="00d5c-212">Yes</span><span class="sxs-lookup"><span data-stu-id="00d5c-212">Yes</span></span> | <span data-ttu-id="00d5c-213">Tak</span><span class="sxs-lookup"><span data-stu-id="00d5c-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="00d5c-214">Przykłady:</span><span class="sxs-lookup"><span data-stu-id="00d5c-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="00d5c-215">Nie</span><span class="sxs-lookup"><span data-stu-id="00d5c-215">No</span></span> | <span data-ttu-id="00d5c-216">Nie</span><span class="sxs-lookup"><span data-stu-id="00d5c-216">No</span></span> | <span data-ttu-id="00d5c-217">Tak</span><span class="sxs-lookup"><span data-stu-id="00d5c-217">Yes</span></span> |

<span data-ttu-id="00d5c-218">Aby uzyskać więcej informacji na temat usuwania typów, zobacz [usuwania usług](#disposal-of-services) sekcji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="00d5c-219">Jest to typowy scenariusz, w wielu implementacjach [pozorowanie typów testowych](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="00d5c-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="00d5c-220">`TryAdd{LIFETIME}` metody zarejestrować usługę tylko, jeśli go nie ma już implementację zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="00d5c-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="00d5c-221">W poniższym przykładzie pierwszy wiersz rejestruje `MyDependency` dla `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="00d5c-222">Drugi wiersz nie obowiązuje, ponieważ `IMyDependency` ma już zarejestrowanej implementacji:</span><span class="sxs-lookup"><span data-stu-id="00d5c-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="00d5c-223">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="00d5c-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="00d5c-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) Jeśli go nie ma już implementację metody tylko zarejestrować usługę *tego samego typu*.</span><span class="sxs-lookup"><span data-stu-id="00d5c-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="00d5c-225">Wiele usług są rozwiązywane za pośrednictwem `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="00d5c-226">Podczas rejestrowania usług, deweloper tylko chce, aby dodać wystąpienie, jeśli jeden z tego samego typu nie został już dodany.</span><span class="sxs-lookup"><span data-stu-id="00d5c-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="00d5c-227">Ogólnie rzecz biorąc ta metoda jest używana przez autorów biblioteki w celu uniknięcia rejestrowanie dwie kopie wystąpienia w kontenerze.</span><span class="sxs-lookup"><span data-stu-id="00d5c-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="00d5c-228">W poniższym przykładzie pierwszy wiersz rejestruje `MyDep` dla `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="00d5c-229">Drugi wiersz rejestruje `MyDep` dla `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="00d5c-230">Trzeci wiersz nie obowiązuje, ponieważ `IMyDep1` ma już zarejestrowanej implementacji `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="00d5c-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="00d5c-231">Zachowanie iniekcji konstruktora</span><span class="sxs-lookup"><span data-stu-id="00d5c-231">Constructor injection behavior</span></span>

<span data-ttu-id="00d5c-232">Usługi można rozwiązać przez dwa mechanizmy:</span><span class="sxs-lookup"><span data-stu-id="00d5c-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="00d5c-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Pozwala na tworzenie obiektów bez rejestracji usługi w kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="00d5c-234">`ActivatorUtilities` jest używana z abstrakcji widocznych dla użytkownika, takich jak pomocnicy tagów, integratorów modeli i kontrolerów MVC.</span><span class="sxs-lookup"><span data-stu-id="00d5c-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="00d5c-235">Konstruktory może akceptować argumenty, które nie są dostarczane przez wstrzykiwanie zależności, ale argumenty należy przypisać wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="00d5c-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="00d5c-236">Gdy usługi są rozwiązywane przez `IServiceProvider` lub `ActivatorUtilities`, wymaga iniekcji konstruktora *publicznych* konstruktora.</span><span class="sxs-lookup"><span data-stu-id="00d5c-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="00d5c-237">Gdy usługi są rozwiązywane przez `ActivatorUtilities`, iniekcji konstruktora wymaga, że tylko jeden konstruktor dotyczy istnieje.</span><span class="sxs-lookup"><span data-stu-id="00d5c-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="00d5c-238">Konstruktor przeciążenia są obsługiwane, ale może istnieć tylko jednego przeciążenia, której argumenty mogą wszystkie zostać spełnione przez wstrzykiwanie zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="00d5c-239">Entity Framework kontekstów</span><span class="sxs-lookup"><span data-stu-id="00d5c-239">Entity Framework contexts</span></span>

<span data-ttu-id="00d5c-240">Entity Framework kontekstów są zwykle dodawane do usługi kontenera przy użyciu [o określonym zakresie okres istnienia](#service-lifetimes) ponieważ operacji bazy danych w aplikacji sieci web są zazwyczaj ograniczone do żądania klienta.</span><span class="sxs-lookup"><span data-stu-id="00d5c-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="00d5c-241">Domyślny okres istnienia jest zakresem, jeśli okres istnienia nie jest określony przez [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) przeciążenia podczas rejestrowania kontekst bazy danych.</span><span class="sxs-lookup"><span data-stu-id="00d5c-241">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="00d5c-242">Usługi danego okresu istnienia nie używaj kontekstu bazy danych o okresie istnienia krótszy niż usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="00d5c-243">Opcje okres istnienia i rejestracji</span><span class="sxs-lookup"><span data-stu-id="00d5c-243">Lifetime and registration options</span></span>

<span data-ttu-id="00d5c-244">Aby zademonstrować różnica między opcjami okres istnienia i rejestracji, należy wziąć pod uwagę następujące interfejsy, które reprezentują zadania jako operacja o unikatowym identyfikatorze `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="00d5c-245">W zależności od tego, jak okres istnienia usługi operations jest skonfigurowany dla następujących interfejsów kontener zawiera takie same lub inne wystąpienie usługi zleconą przez klasę:</span><span class="sxs-lookup"><span data-stu-id="00d5c-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="00d5c-246">Interfejsy są implementowane w `Operation` klasy.</span><span class="sxs-lookup"><span data-stu-id="00d5c-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="00d5c-247">`Operation` Konstruktor generuje identyfikator GUID, jeśli jeden nie został dostarczony:</span><span class="sxs-lookup"><span data-stu-id="00d5c-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="00d5c-248">`OperationService` Jest zarejestrowany, zależy od wszystkich innych `Operation` typów.</span><span class="sxs-lookup"><span data-stu-id="00d5c-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="00d5c-249">Gdy `OperationService` żądania za pomocą iniekcji zależności odbierze nowe wystąpienie klasy poszczególnych usług albo istniejącego wystąpienia oparte na okres istnienia usług zależnych.</span><span class="sxs-lookup"><span data-stu-id="00d5c-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="00d5c-250">Podczas tworzenia usługi przejściowy zleconą z kontenera, `OperationId` z `IOperationTransient` usługi jest inny niż `OperationId` z `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="00d5c-251">`OperationService` odbiera nowe wystąpienie klasy `IOperationTransient` klasy.</span><span class="sxs-lookup"><span data-stu-id="00d5c-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="00d5c-252">Nowe wystąpienie daje innym `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="00d5c-253">Podczas tworzenia usługi o określonym zakresie dla każdego żądania klienta `OperationId` z `IOperationScoped` usługi jest taka sama jak w przypadku `OperationService` w żądaniu klienta.</span><span class="sxs-lookup"><span data-stu-id="00d5c-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="00d5c-254">Na żądania klientów obie te usługi udostępniania innym `OperationId` wartość.</span><span class="sxs-lookup"><span data-stu-id="00d5c-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="00d5c-255">W przypadku pojedynczego wystąpienia i pojedyncze wystąpienie usługi są tworzone po i stosować w przypadku wszystkich żądań klientów i wszystkich usług `OperationId` jest stały we wszystkich żądań obsługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="00d5c-256">W `Startup.ConfigureServices`, każdego typu jest dodawany do kontenera, zgodnie z jego nazwany okres istnienia:</span><span class="sxs-lookup"><span data-stu-id="00d5c-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="00d5c-257">`IOperationSingletonInstance` Usługa korzysta z określonego wystąpienia o identyfikatorze znanych `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="00d5c-258">Może to być oczywiste, gdy ten typ jest w użyciu (jego identyfikator GUID jest zer).</span><span class="sxs-lookup"><span data-stu-id="00d5c-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="00d5c-259">Przykładowa aplikacja pokazuje czasów istnienia obiektów wewnątrz i pomiędzy poszczególnych żądań.</span><span class="sxs-lookup"><span data-stu-id="00d5c-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="00d5c-260">Przykładowa aplikacja `IndexModel` żądań każdego rodzaju `IOperation` typu i `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="00d5c-261">Na stronie zostanie wyświetlony, wszystkie klasy modelu strony i usługi `OperationId` wartości za pomocą przypisania właściwości:</span><span class="sxs-lookup"><span data-stu-id="00d5c-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="00d5c-262">Następujące dwa dane wyjściowe wyświetla wyniki dwa żądania:</span><span class="sxs-lookup"><span data-stu-id="00d5c-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="00d5c-263">**Pierwsze żądanie:**</span><span class="sxs-lookup"><span data-stu-id="00d5c-263">**First request:**</span></span>

<span data-ttu-id="00d5c-264">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="00d5c-264">Controller operations:</span></span>

<span data-ttu-id="00d5c-265">Przejściowy: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="00d5c-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="00d5c-266">O określonym zakresie: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="00d5c-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="00d5c-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="00d5c-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="00d5c-268">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="00d5c-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="00d5c-269">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="00d5c-269">`OperationService` operations:</span></span>

<span data-ttu-id="00d5c-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="00d5c-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="00d5c-271">O określonym zakresie: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="00d5c-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="00d5c-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="00d5c-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="00d5c-273">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="00d5c-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="00d5c-274">**Drugie żądanie:**</span><span class="sxs-lookup"><span data-stu-id="00d5c-274">**Second request:**</span></span>

<span data-ttu-id="00d5c-275">Operacje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="00d5c-275">Controller operations:</span></span>

<span data-ttu-id="00d5c-276">Przejściowy: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="00d5c-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="00d5c-277">O określonym zakresie: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="00d5c-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="00d5c-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="00d5c-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="00d5c-279">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="00d5c-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="00d5c-280">`OperationService` operacje:</span><span class="sxs-lookup"><span data-stu-id="00d5c-280">`OperationService` operations:</span></span>

<span data-ttu-id="00d5c-281">Przejściowy: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="00d5c-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="00d5c-282">O określonym zakresie: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="00d5c-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="00d5c-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="00d5c-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="00d5c-284">Wystąpienie: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="00d5c-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="00d5c-285">Sprawdź, które `OperationId` wartości różnią się w ramach żądania i między żądaniami:</span><span class="sxs-lookup"><span data-stu-id="00d5c-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="00d5c-286">*Przejściowy* obiektów zawsze są różne.</span><span class="sxs-lookup"><span data-stu-id="00d5c-286">*Transient* objects are always different.</span></span> <span data-ttu-id="00d5c-287">Przejściowy `OperationId` wartość pierwszego i drugiego klient żąda różnią się w obu `OperationService` operacje wielu żądań klientów.</span><span class="sxs-lookup"><span data-stu-id="00d5c-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="00d5c-288">Nowe wystąpienie znajduje się do każdego żądania obsługi i żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="00d5c-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="00d5c-289">*Zakres* obiekty są takie same, w ramach żądania klienta, ale o różnych żądań klienta.</span><span class="sxs-lookup"><span data-stu-id="00d5c-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="00d5c-290">*Pojedyncze* obiekty są takie same dla każdego obiektu, a każde żądanie, niezależnie od tego, czy `Operation` wystąpienie znajduje się w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="00d5c-291">Wywoływanie usług z głównego</span><span class="sxs-lookup"><span data-stu-id="00d5c-291">Call services from main</span></span>

<span data-ttu-id="00d5c-292">Tworzenie <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> z [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) rozpoznawanie zakresu usługi w zakresie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="00d5c-293">Takie podejście jest przydatne do dostępu do usługi o określonym zakresie przy uruchamianiu do uruchamiania zadań inicjowania.</span><span class="sxs-lookup"><span data-stu-id="00d5c-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="00d5c-294">Poniższy przykład pokazuje, jak uzyskać kontekst dla `MyScopedService` w `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="00d5c-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="00d5c-295">Weryfikacja zakresu</span><span class="sxs-lookup"><span data-stu-id="00d5c-295">Scope validation</span></span>

<span data-ttu-id="00d5c-296">Gdy aplikacja jest uruchomiona w środowisku programistycznym, domyślny dostawca usług wykonuje testy, aby sprawdzić, czy:</span><span class="sxs-lookup"><span data-stu-id="00d5c-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="00d5c-297">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio rozwiązane od dostawcy usług w katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="00d5c-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="00d5c-298">Usługi o określonym zakresie nie są bezpośrednio lub pośrednio wprowadzony do pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="00d5c-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="00d5c-299">Dostawcy usług główny jest tworzone, gdy <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="00d5c-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="00d5c-300">Okres istnienia dostawcy usług głównego odnosi się do aplikacji/serwera. okres istnienia, gdy dostawca rozpoczyna się od aplikacji i zostanie usunięty podczas zamykania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="00d5c-301">Usługi o określonym zakresie są usuwane przez kontener, który je utworzył.</span><span class="sxs-lookup"><span data-stu-id="00d5c-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="00d5c-302">Jeśli usługi o określonym zakresie zostanie utworzony w kontenerze katalogu głównego, okres istnienia usługi skutecznie zostanie podwyższony do pojedynczego wystąpienia, ponieważ tylko są usuwane przez nadrzędny kontener, gdy serwer/aplikacji zostanie zamknięta.</span><span class="sxs-lookup"><span data-stu-id="00d5c-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="00d5c-303">Sprawdzanie poprawności usługi zakresy przechwytuje tych sytuacji gdy `BuildServiceProvider` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="00d5c-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="00d5c-304">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="00d5c-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="00d5c-305">Żądanie usługi</span><span class="sxs-lookup"><span data-stu-id="00d5c-305">Request Services</span></span>

<span data-ttu-id="00d5c-306">Usługi dostępne w ramach platformy ASP.NET Core poprosić `HttpContext` są udostępniane za pośrednictwem [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) kolekcji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="00d5c-307">Żądanie usługi reprezentują usługi skonfigurowane, a żądane w ramach aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="00d5c-308">Gdy obiekty określić zależności, są one spełnione przez typów znalezionych w `RequestServices`, a nie `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="00d5c-309">Ogólnie rzecz biorąc aplikacja nie należy bezpośrednio używać tych właściwości.</span><span class="sxs-lookup"><span data-stu-id="00d5c-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="00d5c-310">Zamiast tego żądania typy, klasy wymagają za pomocą konstruktorów klas i zezwolić na platformę iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="00d5c-311">Daje to klasy, które są łatwiejsze do testowania.</span><span class="sxs-lookup"><span data-stu-id="00d5c-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="00d5c-312">Preferuj żądania z zależności jako parametry konstruktora do uzyskiwania dostępu do `RequestServices` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="00d5c-313">Projekt usług do wstrzykiwania zależności</span><span class="sxs-lookup"><span data-stu-id="00d5c-313">Design services for dependency injection</span></span>

<span data-ttu-id="00d5c-314">Najlepsze rozwiązania są:</span><span class="sxs-lookup"><span data-stu-id="00d5c-314">Best practices are to:</span></span>

* <span data-ttu-id="00d5c-315">Projektowanie usług na potrzeby uzyskiwania ich zależności iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="00d5c-316">Należy unikać wywołania metody statycznej, stanowe.</span><span class="sxs-lookup"><span data-stu-id="00d5c-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="00d5c-317">Należy unikać bezpośredniego wystąpienia klas zależnych w ramach usług.</span><span class="sxs-lookup"><span data-stu-id="00d5c-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="00d5c-318">Bezpośrednie utworzenie wystąpienia couples kod do konkretnej implementacji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="00d5c-319">Małe, dobrze uwarunkowaną i łatwo przetestowane, należy utworzyć klasy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00d5c-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="00d5c-320">Jeśli klasa ma zbyt wiele zależności wprowadzonego, zwykle jest to znak, że klasa ma zbyt wiele obowiązki i narusza [pojedynczej odpowiedzialności zasady (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="00d5c-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="00d5c-321">Próba Refaktoryzuj klasy przez przeniesienie niektórych jego obowiązki do nowej klasy.</span><span class="sxs-lookup"><span data-stu-id="00d5c-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="00d5c-322">Należy pamiętać, który stron Razor strony modelu klasy i klas kontrolera MVC należy skoncentrować się na kwestie interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="00d5c-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="00d5c-323">Business reguł oraz danych dostęp do szczegółów implementacji powinny być przechowywane w odpowiednich do tych klas [oddzielić wątpliwości](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="00d5c-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="00d5c-324">Usuwanie usługi</span><span class="sxs-lookup"><span data-stu-id="00d5c-324">Disposal of services</span></span>

<span data-ttu-id="00d5c-325">Wywołania kontenera <xref:System.IDisposable.Dispose*> dla <xref:System.IDisposable> tworzy typy.</span><span class="sxs-lookup"><span data-stu-id="00d5c-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="00d5c-326">Jeśli wystąpienie zostanie dodany do kontenera przez kod użytkownika, nie są usuwane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="00d5c-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="00d5c-327">Domyślna usługa kontenera zastąpienia</span><span class="sxs-lookup"><span data-stu-id="00d5c-327">Default service container replacement</span></span>

<span data-ttu-id="00d5c-328">Kontener Wbudowane usługi jest przeznaczona do potrzebami ramach i większość aplikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="00d5c-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="00d5c-329">Zalecamy używanie kontenerze Wbudowane, chyba że potrzebujesz określonych funkcji, która nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="00d5c-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="00d5c-330">Niektóre z funkcji obsługiwanych w 3 kontenerach ze stron nie można odnaleźć w kontenerze Wbudowane:</span><span class="sxs-lookup"><span data-stu-id="00d5c-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="00d5c-331">Iniekcja właściwości</span><span class="sxs-lookup"><span data-stu-id="00d5c-331">Property injection</span></span>
* <span data-ttu-id="00d5c-332">Iniekcja na podstawie nazwy</span><span class="sxs-lookup"><span data-stu-id="00d5c-332">Injection based on name</span></span>
* <span data-ttu-id="00d5c-333">Kontenery podrzędne</span><span class="sxs-lookup"><span data-stu-id="00d5c-333">Child containers</span></span>
* <span data-ttu-id="00d5c-334">Zarządzanie okresem istnienia niestandardowe</span><span class="sxs-lookup"><span data-stu-id="00d5c-334">Custom lifetime management</span></span>
* <span data-ttu-id="00d5c-335">`Func<T>` Obsługa inicjowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="00d5c-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="00d5c-336">Zobacz [pliku readme.md wstrzykiwanie zależności](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) listę niektórych kontenerów, które obsługują kart.</span><span class="sxs-lookup"><span data-stu-id="00d5c-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="00d5c-337">Poniższy przykład zastępuje wbudowanych kontenerów za pomocą [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="00d5c-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="00d5c-338">Zainstaluj pakiety odpowiedniego kontenera:</span><span class="sxs-lookup"><span data-stu-id="00d5c-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="00d5c-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="00d5c-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="00d5c-340">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="00d5c-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="00d5c-341">Konfiguruj kontener w `Startup.ConfigureServices` i zwracają `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="00d5c-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="00d5c-342">Do użycia kontener firm 3 `Startup.ConfigureServices` musi zwracać `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="00d5c-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="00d5c-343">Konfigurowanie Autofac w `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="00d5c-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="00d5c-344">W czasie wykonywania Autofac umożliwia rozwiązanie typów oraz wstrzykiwania zależności.</span><span class="sxs-lookup"><span data-stu-id="00d5c-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="00d5c-345">Aby dowiedzieć się więcej o korzystaniu z Autofac za pomocą programu ASP.NET Core, zobacz [dokumentacji Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="00d5c-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="00d5c-346">Bezpieczeństwo wątków</span><span class="sxs-lookup"><span data-stu-id="00d5c-346">Thread safety</span></span>

<span data-ttu-id="00d5c-347">Tworzenie usługi singleton metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="00d5c-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="00d5c-348">Usługi singleton ma zależność od usługi przejściowy, przejściowe usługa może również wymagać bezpieczeństwo wątków, w zależności od sposobu ich wykorzystania przez wzorzec singleton.</span><span class="sxs-lookup"><span data-stu-id="00d5c-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="00d5c-349">Metoda fabryki pojedynczej usługi, takie jak drugi argument [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), nie musi być metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="00d5c-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="00d5c-350">Typ, takich jak (`static`) konstruktora, jego ma gwarantuje lze volat pouze jednou przez pojedynczy wątek.</span><span class="sxs-lookup"><span data-stu-id="00d5c-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="00d5c-351">Zalecenia</span><span class="sxs-lookup"><span data-stu-id="00d5c-351">Recommendations</span></span>

* <span data-ttu-id="00d5c-352">`async/await` i `Task` rozpoznawanie na podstawie usługi nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="00d5c-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="00d5c-353">C#nie obsługuje konstruktorów asynchronicznego; w związku z tym zalecany wzorzec jest użycie metod asynchronicznych po usunięciu synchronicznie usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="00d5c-354">Unikaj przechowywania danych i konfiguracji bezpośrednio w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="00d5c-355">Na przykład koszyka użytkownika zwykle nie należy dodać do kontenera usługi.</span><span class="sxs-lookup"><span data-stu-id="00d5c-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="00d5c-356">Należy użyć konfiguracji [wzorzec opcje](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="00d5c-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="00d5c-357">Podobnie należy unikać obiektów "symbol zastępczy danych", które istnieją tylko w celu umożliwienia dostępu do innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="00d5c-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="00d5c-358">Jest lepszym rozwiązaniem rzeczywisty element za pośrednictwem DI żądania.</span><span class="sxs-lookup"><span data-stu-id="00d5c-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="00d5c-359">Unikaj statycznych dostęp do usług (na przykład statycznie — wpisanie [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) użytku innym miejscu).</span><span class="sxs-lookup"><span data-stu-id="00d5c-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="00d5c-360">Unikaj używania *wzorzec lokalizatora usług*.</span><span class="sxs-lookup"><span data-stu-id="00d5c-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="00d5c-361">Na przykład nie wywołać <xref:System.IServiceProvider.GetService*> uzyskać wystąpienie usługi, gdy zamiast tego użyj DI:</span><span class="sxs-lookup"><span data-stu-id="00d5c-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="00d5c-362">**Niepoprawne:**</span><span class="sxs-lookup"><span data-stu-id="00d5c-362">**Incorrect:**</span></span>

  ```csharp
  public void MyMethod()
  {
      var options = 
          _services.GetService<IOptionsMonitor<MyOptions>>();
      var option = options.CurrentValue.Option;

      ...
  }
  ```

  <span data-ttu-id="00d5c-363">**Poprawne**:</span><span class="sxs-lookup"><span data-stu-id="00d5c-363">**Correct**:</span></span>

  ```csharp
  private readonly MyOptions _options;

  public MyClass(IOptionsMonitor<MyOptions> options)
  {
      _options = options.CurrentValue;
  }

  public void MyMethod()
  {
      var option = _options.Option;

      ...
  }
  ```

* <span data-ttu-id="00d5c-364">Inna wersja locator service, aby uniknąć wprowadza fabryki, który jest rozpoznawany jako zależności w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="00d5c-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="00d5c-365">Oba te rozwiązania mieszanego [Inwersja kontroli](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategii.</span><span class="sxs-lookup"><span data-stu-id="00d5c-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="00d5c-366">Unikaj statycznych dostęp do `HttpContext` (na przykład [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="00d5c-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="00d5c-367">Podobnie jak wszystkie zestawy zaleceń mogą wystąpić sytuacje, w których wymagane jest ignorowanie zaleceniem.</span><span class="sxs-lookup"><span data-stu-id="00d5c-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="00d5c-368">Wyjątki występują rzadko&mdash;przede wszystkim specjalne przypadki w samej strukturze.</span><span class="sxs-lookup"><span data-stu-id="00d5c-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="00d5c-369">DI jest *alternatywnych* do wzorce dostępu i statyczne/globalne obiektu.</span><span class="sxs-lookup"><span data-stu-id="00d5c-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="00d5c-370">Nie można korzystać z zalet DI, jeśli można łączyć z dostępem do obiektu statycznego.</span><span class="sxs-lookup"><span data-stu-id="00d5c-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00d5c-371">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="00d5c-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="00d5c-372">Pisanie czystego kodu w programie ASP.NET Core przy użyciu iniekcji zależności (MSDN)</span><span class="sxs-lookup"><span data-stu-id="00d5c-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="00d5c-373">Zasada jawne zależności</span><span class="sxs-lookup"><span data-stu-id="00d5c-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="00d5c-374">Inwersja kontroli kontenerów i wzorzec wstrzykiwanie zależności (Martina Fowlera)</span><span class="sxs-lookup"><span data-stu-id="00d5c-374">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="00d5c-375">Jak zarejestrować usługi z wieloma interfejsami w programie ASP.NET Core DI</span><span class="sxs-lookup"><span data-stu-id="00d5c-375">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
