---
title: Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core
author: stevejgordon
description: Dowiedz się więcej o używaniu interfejsu IHttpClientFactory do zarządzania wystąpieniami logicznych HttpClient w ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/01/2019
uid: fundamentals/http-requests
ms.openlocfilehash: d29814f0b057119aaa68a7435879d04987ca0b0b
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034169"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="3be9f-103">Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3be9f-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3be9f-104">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="3be9f-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

[!INCLUDE[](~/includes/not30.md)]

<span data-ttu-id="3be9f-105"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="3be9f-106">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="3be9f-106">It offers the following benefits:</span></span>

* <span data-ttu-id="3be9f-107">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="3be9f-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-108">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="3be9f-108">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="3be9f-109">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="3be9f-110">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="3be9f-111">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="3be9f-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="3be9f-112">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="3be9f-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="3be9f-113">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3be9f-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="3be9f-114">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="3be9f-114">Consumption patterns</span></span>

<span data-ttu-id="3be9f-115">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3be9f-115">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="3be9f-116">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="3be9f-116">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="3be9f-117">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-117">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="3be9f-118">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="3be9f-118">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="3be9f-119">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-119">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="3be9f-120">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-120">None of them are strictly superior to another.</span></span> <span data-ttu-id="3be9f-121">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-121">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="3be9f-122">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="3be9f-122">Basic usage</span></span>

<span data-ttu-id="3be9f-123">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-123">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="3be9f-124">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3be9f-124">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3be9f-125">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="3be9f-125">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="3be9f-126">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-126">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="3be9f-127">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="3be9f-127">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="3be9f-128">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-128">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="3be9f-129">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-129">Named clients</span></span>

<span data-ttu-id="3be9f-130">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="3be9f-130">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="3be9f-131">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-131">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="3be9f-132">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="3be9f-132">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="3be9f-133">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="3be9f-133">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="3be9f-134">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="3be9f-134">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="3be9f-135">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-135">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="3be9f-136">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="3be9f-136">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="3be9f-137">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-137">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="3be9f-138">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-138">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="3be9f-139">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="3be9f-139">Typed clients</span></span>

<span data-ttu-id="3be9f-140">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-140">Typed clients:</span></span>

* <span data-ttu-id="3be9f-141">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-141">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="3be9f-142">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-142">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="3be9f-143">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-143">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="3be9f-144">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="3be9f-144">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="3be9f-145">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-145">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="3be9f-146">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="3be9f-146">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="3be9f-147">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-147">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="3be9f-148">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="3be9f-148">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="3be9f-149">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-149">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="3be9f-150">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="3be9f-150">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="3be9f-151">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="3be9f-151">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="3be9f-152">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="3be9f-152">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="3be9f-153">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="3be9f-153">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="3be9f-154">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-154">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="3be9f-155">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-155">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="3be9f-156">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-156">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="3be9f-157">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="3be9f-157">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="3be9f-158">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-158">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="3be9f-159">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-159">Generated clients</span></span>

<span data-ttu-id="3be9f-160">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="3be9f-160">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="3be9f-161">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3be9f-161">Refit is a REST library for .NET.</span></span> <span data-ttu-id="3be9f-162">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="3be9f-162">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="3be9f-163">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-163">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="3be9f-164">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3be9f-164">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="3be9f-165">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="3be9f-165">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="3be9f-166">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="3be9f-166">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="3be9f-167">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="3be9f-167">Outgoing request middleware</span></span>

<span data-ttu-id="3be9f-168">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-168">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="3be9f-169">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-169">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="3be9f-170">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="3be9f-170">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="3be9f-171">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="3be9f-171">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="3be9f-172">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3be9f-172">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="3be9f-173">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-173">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="3be9f-174">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-174">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="3be9f-175">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="3be9f-175">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="3be9f-176">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="3be9f-176">The preceding code defines a basic handler.</span></span> <span data-ttu-id="3be9f-177">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-177">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="3be9f-178">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="3be9f-178">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="3be9f-179">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-179">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="3be9f-180">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-180">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="3be9f-181">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="3be9f-181">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3be9f-182">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-182">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="3be9f-183">Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-183">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="3be9f-184">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="3be9f-184">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="3be9f-185">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-185">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="3be9f-186">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="3be9f-187">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="3be9f-188">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="3be9f-188">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="3be9f-189">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-189">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="3be9f-190">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-190">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="3be9f-191">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-191">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="3be9f-192">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-192">Use Polly-based handlers</span></span>

<span data-ttu-id="3be9f-193">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="3be9f-193">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="3be9f-194">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3be9f-194">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="3be9f-195">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="3be9f-195">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="3be9f-196">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-196">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-197">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="3be9f-197">The Polly extensions:</span></span>

* <span data-ttu-id="3be9f-198">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-198">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="3be9f-199">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-199">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="3be9f-200">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-200">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="3be9f-201">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="3be9f-201">Handle transient faults</span></span>

<span data-ttu-id="3be9f-202">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="3be9f-202">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="3be9f-203">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="3be9f-203">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="3be9f-204">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="3be9f-204">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="3be9f-205">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-205">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3be9f-206">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="3be9f-206">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="3be9f-207">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-207">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="3be9f-208">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="3be9f-208">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="3be9f-209">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="3be9f-209">Dynamically select policies</span></span>

<span data-ttu-id="3be9f-210">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="3be9f-210">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="3be9f-211">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="3be9f-211">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="3be9f-212">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="3be9f-212">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="3be9f-213">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-213">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="3be9f-214">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-214">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="3be9f-215">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-215">Add multiple Polly handlers</span></span>

<span data-ttu-id="3be9f-216">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="3be9f-216">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="3be9f-217">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-217">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="3be9f-218">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-218">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="3be9f-219">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-219">Failed requests are retried up to three times.</span></span> <span data-ttu-id="3be9f-220">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="3be9f-220">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="3be9f-221">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-221">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="3be9f-222">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="3be9f-222">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="3be9f-223">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-223">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="3be9f-224">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-224">Add policies from the Polly registry</span></span>

<span data-ttu-id="3be9f-225">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-225">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="3be9f-226">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="3be9f-226">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="3be9f-227">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="3be9f-227">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="3be9f-228">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-228">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="3be9f-229">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="3be9f-229">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="3be9f-230">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="3be9f-230">HttpClient and lifetime management</span></span>

<span data-ttu-id="3be9f-231">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-231">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="3be9f-232">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-232">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="3be9f-233">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-233">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="3be9f-234">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-234">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="3be9f-235">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="3be9f-235">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="3be9f-236">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-236">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="3be9f-237">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="3be9f-237">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="3be9f-238">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-238">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="3be9f-239">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="3be9f-239">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="3be9f-240">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-240">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="3be9f-241">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-241">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="3be9f-242">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-242">Disposal of the client isn't required.</span></span> <span data-ttu-id="3be9f-243">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-243">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="3be9f-244">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-244">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-245">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-245">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="3be9f-246">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-246">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="3be9f-247">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-247">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="3be9f-248">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3be9f-248">Logging</span></span>

<span data-ttu-id="3be9f-249">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="3be9f-249">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="3be9f-250">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="3be9f-250">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="3be9f-251">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="3be9f-251">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="3be9f-252">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-252">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="3be9f-253">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-253">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="3be9f-254">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="3be9f-254">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="3be9f-255">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="3be9f-255">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="3be9f-256">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="3be9f-256">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="3be9f-257">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-257">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="3be9f-258">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-258">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="3be9f-259">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="3be9f-259">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="3be9f-260">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-260">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="3be9f-261">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="3be9f-261">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="3be9f-262">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-262">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="3be9f-263">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3be9f-263">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="3be9f-264">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="3be9f-264">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="3be9f-265">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-265">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="3be9f-266">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-266">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="3be9f-267">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="3be9f-267">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="3be9f-268">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-268">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="3be9f-269">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="3be9f-269">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="3be9f-270">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="3be9f-270">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="3be9f-271">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="3be9f-271">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="3be9f-272">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="3be9f-272">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="3be9f-273">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-273">In the following example:</span></span>

* <span data-ttu-id="3be9f-274"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-274"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="3be9f-275">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-275">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="3be9f-276">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3be9f-276">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="3be9f-277">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-277">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="3be9f-278">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3be9f-278">Additional resources</span></span>

* [<span data-ttu-id="3be9f-279">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="3be9f-279">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="3be9f-280">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-280">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="3be9f-281">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="3be9f-281">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="3be9f-282">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="3be9f-282">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="3be9f-283"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-283">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="3be9f-284">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="3be9f-284">It offers the following benefits:</span></span>

* <span data-ttu-id="3be9f-285">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="3be9f-285">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-286">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="3be9f-286">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="3be9f-287">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-287">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="3be9f-288">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-288">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="3be9f-289">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="3be9f-289">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="3be9f-290">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="3be9f-290">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="3be9f-291">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3be9f-291">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="3be9f-292">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="3be9f-292">Consumption patterns</span></span>

<span data-ttu-id="3be9f-293">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3be9f-293">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="3be9f-294">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="3be9f-294">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="3be9f-295">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-295">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="3be9f-296">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="3be9f-296">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="3be9f-297">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-297">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="3be9f-298">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-298">None of them are strictly superior to another.</span></span> <span data-ttu-id="3be9f-299">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-299">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="3be9f-300">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="3be9f-300">Basic usage</span></span>

<span data-ttu-id="3be9f-301">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-301">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="3be9f-302">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3be9f-302">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3be9f-303">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="3be9f-303">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="3be9f-304">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-304">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="3be9f-305">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="3be9f-305">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="3be9f-306">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-306">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="3be9f-307">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-307">Named clients</span></span>

<span data-ttu-id="3be9f-308">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="3be9f-308">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="3be9f-309">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-309">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="3be9f-310">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="3be9f-310">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="3be9f-311">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="3be9f-311">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="3be9f-312">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="3be9f-312">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="3be9f-313">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-313">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="3be9f-314">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="3be9f-314">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="3be9f-315">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-315">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="3be9f-316">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-316">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="3be9f-317">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="3be9f-317">Typed clients</span></span>

<span data-ttu-id="3be9f-318">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-318">Typed clients:</span></span>

* <span data-ttu-id="3be9f-319">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-319">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="3be9f-320">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-320">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="3be9f-321">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-321">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="3be9f-322">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="3be9f-322">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="3be9f-323">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-323">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="3be9f-324">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="3be9f-324">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="3be9f-325">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-325">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="3be9f-326">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="3be9f-326">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="3be9f-327">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-327">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="3be9f-328">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="3be9f-328">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="3be9f-329">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="3be9f-329">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="3be9f-330">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="3be9f-330">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="3be9f-331">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="3be9f-331">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="3be9f-332">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-332">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="3be9f-333">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-333">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="3be9f-334">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-334">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="3be9f-335">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="3be9f-335">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="3be9f-336">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-336">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="3be9f-337">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-337">Generated clients</span></span>

<span data-ttu-id="3be9f-338">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="3be9f-338">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="3be9f-339">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3be9f-339">Refit is a REST library for .NET.</span></span> <span data-ttu-id="3be9f-340">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="3be9f-340">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="3be9f-341">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-341">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="3be9f-342">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3be9f-342">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="3be9f-343">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="3be9f-343">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="3be9f-344">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="3be9f-344">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="3be9f-345">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="3be9f-345">Outgoing request middleware</span></span>

<span data-ttu-id="3be9f-346">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-346">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="3be9f-347">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-347">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="3be9f-348">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="3be9f-348">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="3be9f-349">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="3be9f-349">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="3be9f-350">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3be9f-350">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="3be9f-351">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-351">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="3be9f-352">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-352">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="3be9f-353">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="3be9f-353">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="3be9f-354">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="3be9f-354">The preceding code defines a basic handler.</span></span> <span data-ttu-id="3be9f-355">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-355">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="3be9f-356">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="3be9f-356">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="3be9f-357">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-357">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="3be9f-358">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-358">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="3be9f-359">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="3be9f-359">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3be9f-360">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-360">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="3be9f-361">Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-361">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="3be9f-362">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="3be9f-362">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="3be9f-363">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-363">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="3be9f-364">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-364">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="3be9f-365">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-365">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="3be9f-366">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="3be9f-366">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="3be9f-367">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-367">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="3be9f-368">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-368">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="3be9f-369">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-369">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="3be9f-370">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-370">Use Polly-based handlers</span></span>

<span data-ttu-id="3be9f-371">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="3be9f-371">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="3be9f-372">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3be9f-372">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="3be9f-373">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="3be9f-373">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="3be9f-374">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-374">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-375">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="3be9f-375">The Polly extensions:</span></span>

* <span data-ttu-id="3be9f-376">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-376">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="3be9f-377">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-377">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="3be9f-378">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-378">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="3be9f-379">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="3be9f-379">Handle transient faults</span></span>

<span data-ttu-id="3be9f-380">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="3be9f-380">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="3be9f-381">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="3be9f-381">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="3be9f-382">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="3be9f-382">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="3be9f-383">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-383">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3be9f-384">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="3be9f-384">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="3be9f-385">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-385">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="3be9f-386">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="3be9f-386">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="3be9f-387">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="3be9f-387">Dynamically select policies</span></span>

<span data-ttu-id="3be9f-388">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="3be9f-388">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="3be9f-389">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="3be9f-389">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="3be9f-390">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="3be9f-390">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="3be9f-391">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-391">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="3be9f-392">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-392">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="3be9f-393">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-393">Add multiple Polly handlers</span></span>

<span data-ttu-id="3be9f-394">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="3be9f-394">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="3be9f-395">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-395">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="3be9f-396">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-396">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="3be9f-397">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-397">Failed requests are retried up to three times.</span></span> <span data-ttu-id="3be9f-398">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="3be9f-398">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="3be9f-399">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-399">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="3be9f-400">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="3be9f-400">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="3be9f-401">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-401">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="3be9f-402">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-402">Add policies from the Polly registry</span></span>

<span data-ttu-id="3be9f-403">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-403">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="3be9f-404">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="3be9f-404">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="3be9f-405">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="3be9f-405">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="3be9f-406">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-406">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="3be9f-407">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="3be9f-407">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="3be9f-408">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="3be9f-408">HttpClient and lifetime management</span></span>

<span data-ttu-id="3be9f-409">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-409">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="3be9f-410">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-410">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="3be9f-411">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-411">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="3be9f-412">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-412">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="3be9f-413">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="3be9f-413">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="3be9f-414">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-414">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="3be9f-415">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="3be9f-415">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="3be9f-416">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-416">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="3be9f-417">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="3be9f-417">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="3be9f-418">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-418">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="3be9f-419">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-419">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="3be9f-420">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-420">Disposal of the client isn't required.</span></span> <span data-ttu-id="3be9f-421">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-421">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="3be9f-422">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-422">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-423">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-423">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="3be9f-424">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-424">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="3be9f-425">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-425">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="3be9f-426">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3be9f-426">Logging</span></span>

<span data-ttu-id="3be9f-427">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="3be9f-427">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="3be9f-428">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="3be9f-428">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="3be9f-429">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="3be9f-429">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="3be9f-430">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-430">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="3be9f-431">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-431">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="3be9f-432">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="3be9f-432">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="3be9f-433">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="3be9f-433">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="3be9f-434">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="3be9f-434">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="3be9f-435">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-435">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="3be9f-436">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-436">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="3be9f-437">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="3be9f-437">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="3be9f-438">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-438">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="3be9f-439">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="3be9f-439">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="3be9f-440">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-440">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="3be9f-441">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3be9f-441">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="3be9f-442">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="3be9f-442">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="3be9f-443">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-443">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="3be9f-444">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-444">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="3be9f-445">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="3be9f-445">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="3be9f-446">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-446">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="3be9f-447">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="3be9f-447">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="3be9f-448">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="3be9f-448">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="3be9f-449">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="3be9f-449">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="3be9f-450">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="3be9f-450">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="3be9f-451">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-451">In the following example:</span></span>

* <span data-ttu-id="3be9f-452"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-452"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="3be9f-453">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-453">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="3be9f-454">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3be9f-454">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="3be9f-455">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-455">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="3be9f-456">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3be9f-456">Additional resources</span></span>

* [<span data-ttu-id="3be9f-457">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="3be9f-457">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="3be9f-458">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-458">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="3be9f-459">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="3be9f-459">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="3be9f-460">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="3be9f-460">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="3be9f-461"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-461">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="3be9f-462">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="3be9f-462">It offers the following benefits:</span></span>

* <span data-ttu-id="3be9f-463">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="3be9f-463">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-464">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="3be9f-464">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="3be9f-465">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-465">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="3be9f-466">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-466">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="3be9f-467">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="3be9f-467">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="3be9f-468">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="3be9f-468">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="3be9f-469">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3be9f-469">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3be9f-470">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3be9f-470">Prerequisites</span></span>

<span data-ttu-id="3be9f-471">Projekty docelowe .NET Framework wymagają instalacji pakietu NuGet [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-471">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="3be9f-472">Projekty przeznaczone dla platformy .NET Core i odwołujące się do [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) zawierają już pakiet `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-472">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="3be9f-473">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="3be9f-473">Consumption patterns</span></span>

<span data-ttu-id="3be9f-474">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3be9f-474">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="3be9f-475">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="3be9f-475">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="3be9f-476">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-476">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="3be9f-477">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="3be9f-477">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="3be9f-478">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-478">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="3be9f-479">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-479">None of them are strictly superior to another.</span></span> <span data-ttu-id="3be9f-480">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-480">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="3be9f-481">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="3be9f-481">Basic usage</span></span>

<span data-ttu-id="3be9f-482">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-482">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="3be9f-483">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3be9f-483">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3be9f-484">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="3be9f-484">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="3be9f-485">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-485">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="3be9f-486">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="3be9f-486">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="3be9f-487">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-487">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="3be9f-488">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-488">Named clients</span></span>

<span data-ttu-id="3be9f-489">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="3be9f-489">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="3be9f-490">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-490">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="3be9f-491">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="3be9f-491">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="3be9f-492">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="3be9f-492">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="3be9f-493">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="3be9f-493">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="3be9f-494">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-494">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="3be9f-495">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="3be9f-495">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="3be9f-496">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-496">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="3be9f-497">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-497">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="3be9f-498">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="3be9f-498">Typed clients</span></span>

<span data-ttu-id="3be9f-499">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-499">Typed clients:</span></span>

* <span data-ttu-id="3be9f-500">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-500">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="3be9f-501">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-501">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="3be9f-502">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-502">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="3be9f-503">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="3be9f-503">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="3be9f-504">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3be9f-504">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="3be9f-505">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="3be9f-505">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="3be9f-506">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-506">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="3be9f-507">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="3be9f-507">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="3be9f-508">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-508">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="3be9f-509">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="3be9f-509">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="3be9f-510">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="3be9f-510">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="3be9f-511">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="3be9f-511">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="3be9f-512">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="3be9f-512">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="3be9f-513">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-513">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="3be9f-514">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-514">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="3be9f-515">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-515">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="3be9f-516">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="3be9f-516">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="3be9f-517">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-517">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="3be9f-518">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="3be9f-518">Generated clients</span></span>

<span data-ttu-id="3be9f-519">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="3be9f-519">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="3be9f-520">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3be9f-520">Refit is a REST library for .NET.</span></span> <span data-ttu-id="3be9f-521">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="3be9f-521">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="3be9f-522">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-522">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="3be9f-523">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="3be9f-523">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="3be9f-524">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="3be9f-524">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="3be9f-525">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="3be9f-525">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="3be9f-526">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="3be9f-526">Outgoing request middleware</span></span>

<span data-ttu-id="3be9f-527">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-527">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="3be9f-528">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-528">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="3be9f-529">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="3be9f-529">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="3be9f-530">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="3be9f-530">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="3be9f-531">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3be9f-531">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="3be9f-532">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-532">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="3be9f-533">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-533">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="3be9f-534">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="3be9f-534">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="3be9f-535">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="3be9f-535">The preceding code defines a basic handler.</span></span> <span data-ttu-id="3be9f-536">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-536">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="3be9f-537">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="3be9f-537">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="3be9f-538">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-538">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="3be9f-539">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-539">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="3be9f-540">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="3be9f-540">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3be9f-541">Procedura obsługi **musi** być zarejestrowana w programie di jako przejściowa usługa, nigdy nie jest objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="3be9f-541">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="3be9f-542">Jeśli program obsługi jest zarejestrowany jako usługa objęta zakresem, a wszystkie usługi, od których zależy program obsługi, są jednorazowe:</span><span class="sxs-lookup"><span data-stu-id="3be9f-542">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="3be9f-543">Usługi programu obsługi mogły zostać zlikwidowane, zanim program obsługi znajdzie się poza zakresem.</span><span class="sxs-lookup"><span data-stu-id="3be9f-543">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="3be9f-544">Usunięte usługi obsługi powodują niepowodzenie procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-544">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="3be9f-545">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując w typ procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-545">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="3be9f-546">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-546">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="3be9f-547">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-547">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="3be9f-548">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="3be9f-548">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="3be9f-549">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-549">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="3be9f-550">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-550">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="3be9f-551">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-551">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="3be9f-552">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-552">Use Polly-based handlers</span></span>

<span data-ttu-id="3be9f-553">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="3be9f-553">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="3be9f-554">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3be9f-554">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="3be9f-555">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="3be9f-555">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="3be9f-556">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-556">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-557">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="3be9f-557">The Polly extensions:</span></span>

* <span data-ttu-id="3be9f-558">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-558">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="3be9f-559">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-559">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="3be9f-560">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-560">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="3be9f-561">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="3be9f-561">Handle transient faults</span></span>

<span data-ttu-id="3be9f-562">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="3be9f-562">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="3be9f-563">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="3be9f-563">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="3be9f-564">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="3be9f-564">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="3be9f-565">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-565">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3be9f-566">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="3be9f-566">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="3be9f-567">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-567">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="3be9f-568">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="3be9f-568">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="3be9f-569">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="3be9f-569">Dynamically select policies</span></span>

<span data-ttu-id="3be9f-570">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="3be9f-570">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="3be9f-571">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="3be9f-571">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="3be9f-572">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="3be9f-572">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="3be9f-573">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-573">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="3be9f-574">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-574">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="3be9f-575">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-575">Add multiple Polly handlers</span></span>

<span data-ttu-id="3be9f-576">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="3be9f-576">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="3be9f-577">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-577">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="3be9f-578">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-578">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="3be9f-579">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="3be9f-579">Failed requests are retried up to three times.</span></span> <span data-ttu-id="3be9f-580">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="3be9f-580">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="3be9f-581">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="3be9f-581">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="3be9f-582">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="3be9f-582">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="3be9f-583">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-583">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="3be9f-584">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-584">Add policies from the Polly registry</span></span>

<span data-ttu-id="3be9f-585">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-585">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="3be9f-586">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="3be9f-586">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="3be9f-587">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="3be9f-587">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="3be9f-588">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-588">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="3be9f-589">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="3be9f-589">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="3be9f-590">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="3be9f-590">HttpClient and lifetime management</span></span>

<span data-ttu-id="3be9f-591">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-591">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="3be9f-592">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-592">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="3be9f-593">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-593">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="3be9f-594">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-594">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="3be9f-595">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="3be9f-595">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="3be9f-596">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="3be9f-596">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="3be9f-597">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="3be9f-597">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="3be9f-598">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-598">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="3be9f-599">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="3be9f-599">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="3be9f-600">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-600">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="3be9f-601">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-601">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="3be9f-602">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="3be9f-602">Disposal of the client isn't required.</span></span> <span data-ttu-id="3be9f-603">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="3be9f-603">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="3be9f-604">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-604">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="3be9f-605">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-605">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="3be9f-606">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-606">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="3be9f-607">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-607">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="3be9f-608">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="3be9f-608">Logging</span></span>

<span data-ttu-id="3be9f-609">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="3be9f-609">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="3be9f-610">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="3be9f-610">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="3be9f-611">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="3be9f-611">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="3be9f-612">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-612">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="3be9f-613">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-613">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="3be9f-614">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="3be9f-614">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="3be9f-615">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="3be9f-615">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="3be9f-616">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="3be9f-616">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="3be9f-617">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="3be9f-617">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="3be9f-618">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-618">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="3be9f-619">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="3be9f-619">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="3be9f-620">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-620">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="3be9f-621">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="3be9f-621">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="3be9f-622">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="3be9f-622">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="3be9f-623">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="3be9f-623">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="3be9f-624">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="3be9f-624">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="3be9f-625">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="3be9f-625">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="3be9f-626">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="3be9f-626">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="3be9f-627">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="3be9f-627">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="3be9f-628">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="3be9f-628">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="3be9f-629">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="3be9f-629">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="3be9f-630">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="3be9f-630">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="3be9f-631">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="3be9f-631">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="3be9f-632">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="3be9f-632">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="3be9f-633">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3be9f-633">In the following example:</span></span>

* <span data-ttu-id="3be9f-634"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="3be9f-634"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="3be9f-635">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="3be9f-635">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="3be9f-636">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3be9f-636">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="3be9f-637">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="3be9f-637">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="3be9f-638">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3be9f-638">Additional resources</span></span>

* [<span data-ttu-id="3be9f-639">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="3be9f-639">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="3be9f-640">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="3be9f-640">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="3be9f-641">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="3be9f-641">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end