---
title: ASP.NET Core iniekcja zależności Blazor
author: guardrex
description: Zobacz, jak aplikacje Blazor mogą wstrzyknąć usługi do składników programu.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: a9330caa81eec0910206312283b3424c70cd1289
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819772"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="f5ac0-103">ASP.NET Core iniekcja zależności Blazor</span><span class="sxs-lookup"><span data-stu-id="f5ac0-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="f5ac0-104">Autor [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="f5ac0-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="f5ac0-105">Blazor obsługuje [iniekcję zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f5ac0-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f5ac0-106">Aplikacje mogą używać wbudowanych usług, wprowadzając je do składników programu.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="f5ac0-107">Aplikacje mogą także definiować i rejestrować niestandardowe usługi i udostępniać je w całej aplikacji za pomocą funkcji DI.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="f5ac0-108">DI jest techniką uzyskiwania dostępu do usług skonfigurowanych w centralnej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="f5ac0-109">Może to być przydatne w aplikacjach Blazor do:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="f5ac0-110">Udostępnianie pojedynczego wystąpienia klasy usługi w wielu składnikach, znanej jako pojedyncze usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="f5ac0-111">Oddziel składniki od klas konkretnych usług za pomocą abstrakcji odwołań.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="f5ac0-112">Rozważmy na przykład interfejs `IDataAccess` do uzyskiwania dostępu do danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="f5ac0-113">Interfejs jest implementowany przez konkretną `DataAccess` klasę i zarejestrowany jako usługa w kontenerze usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="f5ac0-114">Gdy składnik używa elementu di do odbierania `IDataAccess` implementacji, składnik nie jest połączony z konkretnym typem.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="f5ac0-115">Implementacja może zostać zamieniony, być może dla implementacji makiety w testach jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="f5ac0-116">Usługi domyślne</span><span class="sxs-lookup"><span data-stu-id="f5ac0-116">Default services</span></span>

<span data-ttu-id="f5ac0-117">Domyślne usługi są automatycznie dodawane do kolekcji usług aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="f5ac0-118">Usługa</span><span class="sxs-lookup"><span data-stu-id="f5ac0-118">Service</span></span> | <span data-ttu-id="f5ac0-119">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="f5ac0-119">Lifetime</span></span> | <span data-ttu-id="f5ac0-120">Opis</span><span class="sxs-lookup"><span data-stu-id="f5ac0-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="f5ac0-121">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f5ac0-121">Singleton</span></span> | <span data-ttu-id="f5ac0-122">Zapewnia metody wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu identyfikowanego przez identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="f5ac0-123">Należy zauważyć, że to `HttpClient` wystąpienie programu używa przeglądarki do obsługi ruchu HTTP w tle.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="f5ac0-124">[HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) jest automatycznie ustawiany na podstawowy prefiks identyfikatora URI aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="f5ac0-125">Aby uzyskać więcej informacji, zobacz <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="f5ac0-126">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f5ac0-126">Singleton</span></span> | <span data-ttu-id="f5ac0-127">Reprezentuje wystąpienie środowiska uruchomieniowego JavaScript, w którym są wysyłane wywołania języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="f5ac0-128">Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="f5ac0-129">Pojedynczego</span><span class="sxs-lookup"><span data-stu-id="f5ac0-129">Singleton</span></span> | <span data-ttu-id="f5ac0-130">Zawiera pomocników do pracy z identyfikatorami URI i stanem nawigacji.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="f5ac0-131">Aby uzyskać więcej informacji, zobacz [identyfikatory URI i pomocnika stanu nawigacji](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="f5ac0-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="f5ac0-132">Niestandardowy dostawca usług nie dostarcza automatycznie usług domyślnych wymienionych w tabeli.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="f5ac0-133">W przypadku użycia niestandardowego dostawcy usług i wymagania usług wymienionych w tabeli należy dodać wymagane usługi do nowego dostawcy usług.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="f5ac0-134">Dodawanie usług do aplikacji</span><span class="sxs-lookup"><span data-stu-id="f5ac0-134">Add services to an app</span></span>

<span data-ttu-id="f5ac0-135">Po utworzeniu nowej aplikacji zapoznaj się z `Startup.ConfigureServices` tą metodą:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="f5ac0-136">Metoda jest przenoszona<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>a ,którajestlistąobiektówdeskryptorausługi().<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="f5ac0-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="f5ac0-137">Usługi są dodawane przez dostarczenie deskryptorów usługi do kolekcji usług.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="f5ac0-138">Poniższy przykład demonstruje koncepcję z `IDataAccess` interfejsem i jego konkretną implementacją: `DataAccess`</span><span class="sxs-lookup"><span data-stu-id="f5ac0-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="f5ac0-139">Usługi można skonfigurować przy użyciu okresów istnienia podanych w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="f5ac0-140">Okres istnienia</span><span class="sxs-lookup"><span data-stu-id="f5ac0-140">Lifetime</span></span> | <span data-ttu-id="f5ac0-141">Opis</span><span class="sxs-lookup"><span data-stu-id="f5ac0-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="f5ac0-142">Po stronie klienta Blazor nie ma obecnie koncepcji DI Scopes.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-142">Blazor client-side doesn't currently have a concept of DI scopes.</span></span> <span data-ttu-id="f5ac0-143">`Scoped`-zarejestrowane usługi zachowują `Singleton` się jak usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="f5ac0-144">Jednak model hostingu po stronie serwera obsługuje `Scoped` okres istnienia.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="f5ac0-145">W składniku Razor Rejestracja usługi w zakresie jest objęta zakresem połączenia.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-145">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="f5ac0-146">Z tego powodu użycie usług objętych zakresem jest preferowane dla usług, które powinny być objęte zakresem bieżącego użytkownika, nawet jeśli bieżącym celem jest uruchomienie po stronie klienta w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="f5ac0-147">DI tworzy *pojedyncze wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="f5ac0-148">Wszystkie składniki wymagające `Singleton` usługi odbierają wystąpienie tej samej usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="f5ac0-149">Za każdym razem, gdy składnik uzyskuje wystąpienie `Transient` usługi z kontenera usługi, otrzymuje *nowe wystąpienie* usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="f5ac0-150">System DI jest oparty na systemie DI w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="f5ac0-151">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="f5ac0-152">Żądanie usługi w składniku</span><span class="sxs-lookup"><span data-stu-id="f5ac0-152">Request a service in a component</span></span>

<span data-ttu-id="f5ac0-153">Po dodaniu usług do kolekcji usług należy wstrzyknąć usługi do składników za pomocą [ \@](xref:mvc/views/razor#inject) dyrektywy wstrzyknięcia Razor.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="f5ac0-154">`@inject`ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="f5ac0-155">Wpisz &ndash; typ usługi do dodania.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="f5ac0-156">Właściwość &ndash; nazwa właściwości otrzymującej wstrzykiwanej usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="f5ac0-157">Właściwość nie wymaga ręcznego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="f5ac0-158">Kompilator tworzy właściwość.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-158">The compiler creates the property.</span></span>

<span data-ttu-id="f5ac0-159">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="f5ac0-160">Użyj wielu `@inject` instrukcji, aby wstrzyknąć różne usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="f5ac0-161">Poniższy przykład pokazuje, jak używać `@inject`.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="f5ac0-162">Implementowanie `Services.IDataAccess` usługi jest wstrzykiwane do właściwości `DataRepository`składnika.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="f5ac0-163">Zwróć uwagę, jak kod używa `IDataAccess` tylko abstrakcji:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="f5ac0-164">Wewnętrznie wygenerowana Właściwość (`DataRepository`) jest uzupełniona `InjectAttribute` atrybutem.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="f5ac0-165">Zazwyczaj ten atrybut nie jest używany bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="f5ac0-166">Jeśli klasa podstawowa jest wymagana dla składników i właściwości wstrzykiwane są również wymagane dla klasy bazowej, należy ręcznie dodać `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="f5ac0-167">W składnikach pochodnych klasy `@inject` bazowej dyrektywa nie jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="f5ac0-168">`InjectAttribute` Klasa bazowa jest wystarczająca:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="f5ac0-169">Korzystanie z usług DI w</span><span class="sxs-lookup"><span data-stu-id="f5ac0-169">Use DI in services</span></span>

<span data-ttu-id="f5ac0-170">Złożone usługi mogą wymagać dodatkowych usług.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-170">Complex services might require additional services.</span></span> <span data-ttu-id="f5ac0-171">W poprzednim przykładzie `DataAccess` może być `HttpClient` wymagana usługa domyślna.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="f5ac0-172">`@inject`(lub `InjectAttribute`) nie jest dostępny do użytku w usługach.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="f5ac0-173">Zamiast tego należy użyć *iniekcji konstruktora* .</span><span class="sxs-lookup"><span data-stu-id="f5ac0-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="f5ac0-174">Wymagane usługi są dodawane przez dodanie parametrów do konstruktora usługi.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="f5ac0-175">Gdy program DI tworzy usługę, rozpoznaje usługi, których wymaga w konstruktorze i udostępnia je odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="f5ac0-176">Wymagania wstępne dotyczące iniekcji konstruktora:</span><span class="sxs-lookup"><span data-stu-id="f5ac0-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="f5ac0-177">Jeden Konstruktor musi istnieć, którego argumenty mogą być zrealizowane przez DI.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="f5ac0-178">Dodatkowe parametry, które nie są objęte przez DI, są dozwolone, jeśli określają wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="f5ac0-179">Odpowiedni Konstruktor musi być *publiczny*.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="f5ac0-180">Musi istnieć jeden odpowiedni Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-180">One applicable constructor must exist.</span></span> <span data-ttu-id="f5ac0-181">W przypadku niejednoznaczności, polecenie DI zgłasza wyjątek.</span><span class="sxs-lookup"><span data-stu-id="f5ac0-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5ac0-182">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f5ac0-182">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
