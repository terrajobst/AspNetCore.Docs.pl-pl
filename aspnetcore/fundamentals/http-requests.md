---
title: Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core
author: stevejgordon
description: Dowiedz się więcej o używaniu interfejsu IHttpClientFactory do zarządzania wystąpieniami logicznych HttpClient w ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: f33444b8fc08dc022da7700af53a218600290162
ms.sourcegitcommit: 169ea5116de729c803685725d96450a270bc55b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74733924"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="93d20-103">Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93d20-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="93d20-104">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)i [Kirka Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="93d20-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="93d20-105"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="93d20-106">`IHttpClientFactory` oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="93d20-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="93d20-107">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="93d20-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="93d20-108">Na przykład klient o nazwie *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="93d20-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="93d20-109">Domyślny klient można zarejestrować na potrzeby ogólnego dostępu.</span><span class="sxs-lookup"><span data-stu-id="93d20-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="93d20-110">Kodyfikacja koncepcji wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="93d20-111">Udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, które umożliwiają delegowanie programów obsługi w `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="93d20-112">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="93d20-113">Automatyczne zarządzanie pozwala uniknąć typowych problemów z usługą DNS (Domain Name System) występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="93d20-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="93d20-114">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="93d20-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="93d20-115">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="93d20-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="93d20-116">Przykładowy kod w tej wersji tematu używa <xref:System.Text.Json> do deserializacji zawartości JSON zwróconej w odpowiedziach HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="93d20-117">Aby uzyskać przykłady, które używają `Json.NET` i `ReadAsAsync<T>`, użyj selektora wersji, aby wybrać wersję 2. x tego tematu.</span><span class="sxs-lookup"><span data-stu-id="93d20-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="93d20-118">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="93d20-118">Consumption patterns</span></span>

<span data-ttu-id="93d20-119">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="93d20-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="93d20-120">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="93d20-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="93d20-121">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="93d20-122">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="93d20-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="93d20-123">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="93d20-124">Najlepsze podejście zależy od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="93d20-125">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="93d20-125">Basic usage</span></span>

<span data-ttu-id="93d20-126">`IHttpClientFactory` można zarejestrować, wywołując `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="93d20-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="93d20-127">`IHttpClientFactory` można żądać przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93d20-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="93d20-128">Poniższy kod używa `IHttpClientFactory`, aby utworzyć wystąpienie `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="93d20-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="93d20-129">Użycie `IHttpClientFactory` podobnej do powyższego przykładu jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="93d20-130">Nie ma to wpływu na sposób użycia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="93d20-131">W miejscach, w których `HttpClient` wystąpienia są tworzone w istniejącej aplikacji, Zastąp te wystąpienia wywołaniami <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="93d20-132">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-132">Named clients</span></span>

<span data-ttu-id="93d20-133">Nazwani klienci są dobrym wyborem w przypadku:</span><span class="sxs-lookup"><span data-stu-id="93d20-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="93d20-134">Aplikacja wymaga wielu odrębnych użycia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="93d20-135">Wiele `HttpClient`s ma inną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="93d20-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="93d20-136">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="93d20-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="93d20-137">W powyższym kodzie klient ma skonfigurowany program:</span><span class="sxs-lookup"><span data-stu-id="93d20-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="93d20-138">Adres podstawowy `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="93d20-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="93d20-139">Dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="93d20-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="93d20-140">Klient</span><span class="sxs-lookup"><span data-stu-id="93d20-140">CreateClient</span></span>

<span data-ttu-id="93d20-141">Za każdym razem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="93d20-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="93d20-142">Zostanie utworzone nowe wystąpienie `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="93d20-143">Akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="93d20-143">The configuration action is called.</span></span>

<span data-ttu-id="93d20-144">Aby utworzyć nazwanego klienta, Przekaż jego nazwę do `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="93d20-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="93d20-145">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="93d20-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="93d20-146">Kod może przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="93d20-147">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="93d20-147">Typed clients</span></span>

<span data-ttu-id="93d20-148">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="93d20-148">Typed clients:</span></span>

* <span data-ttu-id="93d20-149">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="93d20-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="93d20-150">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="93d20-151">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="93d20-152">Na przykład może zostać użyty pojedynczy klient z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="93d20-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="93d20-153">Dla pojedynczego punktu końcowego zaplecza.</span><span class="sxs-lookup"><span data-stu-id="93d20-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="93d20-154">Do hermetyzacji wszystkich logiki związanych z punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="93d20-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="93d20-155">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="93d20-156">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="93d20-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="93d20-157">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="93d20-157">In the preceding code:</span></span>

* <span data-ttu-id="93d20-158">Konfiguracja zostanie przeniesiona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="93d20-159">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="93d20-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="93d20-160">Można tworzyć metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcje.</span><span class="sxs-lookup"><span data-stu-id="93d20-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="93d20-161">Na przykład Metoda `GetAspNetDocsIssues` hermetyzuje kod w celu pobrania otwartych problemów.</span><span class="sxs-lookup"><span data-stu-id="93d20-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="93d20-162">Następujący kod wywołuje <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> w `Startup.ConfigureServices` w celu zarejestrowania klasy klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="93d20-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="93d20-163">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="93d20-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="93d20-164">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="93d20-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="93d20-165">Konfigurację dla określonego klienta można określić podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="93d20-166">`HttpClient` można hermetyzować w obrębie klienta z określonym typem.</span><span class="sxs-lookup"><span data-stu-id="93d20-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="93d20-167">Zamiast uwidaczniać ją jako właściwość, zdefiniuj metodę, która wywołuje wystąpienie `HttpClient` wewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="93d20-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="93d20-168">W poprzednim kodzie `HttpClient` jest przechowywany w prywatnym polu.</span><span class="sxs-lookup"><span data-stu-id="93d20-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="93d20-169">Dostęp do `HttpClient` odbywa się za pomocą metody `GetRepos` publicznej.</span><span class="sxs-lookup"><span data-stu-id="93d20-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="93d20-170">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-170">Generated clients</span></span>

<span data-ttu-id="93d20-171">`IHttpClientFactory` można używać w połączeniu z bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="93d20-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="93d20-172">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="93d20-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="93d20-173">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="93d20-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="93d20-174">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="93d20-175">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="93d20-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="93d20-176">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="93d20-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="93d20-177">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="93d20-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="93d20-178">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="93d20-178">Outgoing request middleware</span></span>

<span data-ttu-id="93d20-179">`HttpClient` ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="93d20-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="93d20-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="93d20-181">Upraszcza definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="93d20-182">Obsługuje rejestrację i łańcuch wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="93d20-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="93d20-183">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="93d20-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="93d20-184">Ten wzorzec:</span><span class="sxs-lookup"><span data-stu-id="93d20-184">This pattern:</span></span>

  * <span data-ttu-id="93d20-185">Jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93d20-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="93d20-186">Program udostępnia mechanizm zarządzania problemami z obcinaniem między żądaniami HTTP, takimi jak:</span><span class="sxs-lookup"><span data-stu-id="93d20-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="93d20-187">buforowanie</span><span class="sxs-lookup"><span data-stu-id="93d20-187">caching</span></span>
    * <span data-ttu-id="93d20-188">obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="93d20-188">error handling</span></span>
    * <span data-ttu-id="93d20-189">serializacja</span><span class="sxs-lookup"><span data-stu-id="93d20-189">serialization</span></span>
    * <span data-ttu-id="93d20-190">rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="93d20-190">logging</span></span>

<span data-ttu-id="93d20-191">Aby utworzyć program obsługi delegowania:</span><span class="sxs-lookup"><span data-stu-id="93d20-191">To create a delegating handler:</span></span>

* <span data-ttu-id="93d20-192">Pochodny od <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="93d20-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="93d20-193">Zastąp <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="93d20-194">Wykonaj kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="93d20-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="93d20-195">Poprzedni kod sprawdza, czy nagłówek `X-API-KEY` jest w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="93d20-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="93d20-196">Jeśli brakuje `X-API-KEY`, zwracany jest <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="93d20-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="93d20-197">Do konfiguracji `HttpClient` można dodać więcej niż jeden program obsługi z <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="93d20-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="93d20-198">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="93d20-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="93d20-199">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="93d20-200">Programy obsługi mogą zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="93d20-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="93d20-201">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="93d20-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="93d20-202">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="93d20-203">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="93d20-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="93d20-204">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="93d20-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="93d20-205">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="93d20-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="93d20-206">Przekaż dane do procedury obsługi przy użyciu [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="93d20-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="93d20-207">Użyj <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="93d20-208">Utwórz niestandardowy obiekt magazynu <xref:System.Threading.AsyncLocal`1>, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="93d20-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="93d20-209">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-209">Use Polly-based handlers</span></span>

<span data-ttu-id="93d20-210">`IHttpClientFactory` integruje się z biblioteką innych firm [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="93d20-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="93d20-211">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="93d20-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="93d20-212">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="93d20-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="93d20-213">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="93d20-214">Rozszerzenia Polly obsługują dodawanie programów obsługi opartych na Polly do klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="93d20-215">Polly wymaga pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="93d20-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="93d20-216">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="93d20-216">Handle transient faults</span></span>

<span data-ttu-id="93d20-217">Błędy są zwykle wykonywane, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="93d20-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="93d20-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="93d20-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="93d20-219">Zasady skonfigurowane z `AddTransientHttpErrorPolicy` obsługują następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="93d20-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="93d20-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="93d20-220">HTTP 5xx</span></span>
* <span data-ttu-id="93d20-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="93d20-221">HTTP 408</span></span>

<span data-ttu-id="93d20-222">`AddTransientHttpErrorPolicy` zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowany do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="93d20-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="93d20-223">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="93d20-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="93d20-224">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="93d20-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="93d20-225">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="93d20-225">Dynamically select policies</span></span>

<span data-ttu-id="93d20-226">Metody rozszerzające umożliwiają dodawanie programów obsługi opartych na Polly, na przykład <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="93d20-227">Następujące `AddPolicyHandler` Przeciążenie sprawdza żądanie, aby zdecydować, które zasady zastosować:</span><span class="sxs-lookup"><span data-stu-id="93d20-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="93d20-228">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="93d20-229">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="93d20-230">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="93d20-231">Jest to typowy sposób zagnieżdżania zasad Pollyymi:</span><span class="sxs-lookup"><span data-stu-id="93d20-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="93d20-232">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="93d20-232">In the preceding example:</span></span>

* <span data-ttu-id="93d20-233">Dodawane są dwa procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-233">Two handlers are added.</span></span>
* <span data-ttu-id="93d20-234">Pierwsza procedura obsługi używa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="93d20-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="93d20-235">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="93d20-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="93d20-236">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="93d20-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="93d20-237">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli 5 nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="93d20-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="93d20-238">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="93d20-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="93d20-239">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="93d20-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="93d20-240">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="93d20-241">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="93d20-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="93d20-242">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="93d20-242">In the following code:</span></span>

* <span data-ttu-id="93d20-243">Dodawane są zasady "regularne" i "długie".</span><span class="sxs-lookup"><span data-stu-id="93d20-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="93d20-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> dodaje zasady "regularne" i "długie" z rejestru.</span><span class="sxs-lookup"><span data-stu-id="93d20-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="93d20-245">Aby uzyskać więcej informacji na temat integracji `IHttpClientFactory` i Polly, zobacz [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="93d20-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="93d20-246">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="93d20-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="93d20-247">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="93d20-248"><xref:System.Net.Http.HttpMessageHandler> jest tworzony na Nazwa klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="93d20-249">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="93d20-250">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="93d20-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="93d20-251">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="93d20-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="93d20-252">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="93d20-253">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="93d20-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="93d20-254">Niektóre programy obsługi powodują również, że połączenia są otwierane w nieskończoność, co może uniemożliwić oddziałanie programu obsługi w systemie DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="93d20-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="93d20-255">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="93d20-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="93d20-256">Wartość domyślną można przesłonić na podstawie nazwy klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="93d20-257">wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które **nie** wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="93d20-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="93d20-258">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="93d20-259">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="93d20-260">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="93d20-261">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="93d20-262">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="93d20-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="93d20-263">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="93d20-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="93d20-264">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="93d20-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="93d20-265">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="93d20-266">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="93d20-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="93d20-267">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="93d20-268">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="93d20-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="93d20-269">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="93d20-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="93d20-270">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="93d20-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="93d20-271">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="93d20-272">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="93d20-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="93d20-273">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="93d20-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="93d20-274">Cookie</span><span class="sxs-lookup"><span data-stu-id="93d20-274">Cookies</span></span>

<span data-ttu-id="93d20-275">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="93d20-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="93d20-276">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="93d20-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="93d20-277">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="93d20-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="93d20-278">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="93d20-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="93d20-279">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="93d20-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="93d20-280">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="93d20-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="93d20-281">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="93d20-281">Logging</span></span>

<span data-ttu-id="93d20-282">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="93d20-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="93d20-283">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="93d20-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="93d20-284">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="93d20-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="93d20-285">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="93d20-286">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="93d20-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="93d20-287">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="93d20-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="93d20-288">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="93d20-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="93d20-289">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="93d20-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="93d20-290">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="93d20-291">W przykładzie *MyNamedClient* te komunikaty są rejestrowane z kategorią dziennika "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="93d20-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="93d20-292">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="93d20-293">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="93d20-294">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="93d20-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="93d20-295">Może to obejmować zmiany w nagłówkach żądania lub w kodzie stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="93d20-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="93d20-296">Uwzględnienie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="93d20-297">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="93d20-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="93d20-298">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="93d20-299">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="93d20-300">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="93d20-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="93d20-301">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="93d20-302">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="93d20-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="93d20-303">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="93d20-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="93d20-304">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="93d20-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="93d20-305">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="93d20-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="93d20-306">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="93d20-306">In the following example:</span></span>

* <span data-ttu-id="93d20-307"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="93d20-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="93d20-308">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="93d20-309">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="93d20-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="93d20-310">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="93d20-311">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="93d20-311">Additional resources</span></span>

* [<span data-ttu-id="93d20-312">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="93d20-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="93d20-313">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="93d20-314">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="93d20-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="93d20-315">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="93d20-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="93d20-316"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="93d20-317">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="93d20-317">It offers the following benefits:</span></span>

* <span data-ttu-id="93d20-318">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="93d20-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="93d20-319">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="93d20-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="93d20-320">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="93d20-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="93d20-321">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="93d20-322">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="93d20-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="93d20-323">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="93d20-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="93d20-324">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93d20-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="93d20-325">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="93d20-325">Consumption patterns</span></span>

<span data-ttu-id="93d20-326">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="93d20-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="93d20-327">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="93d20-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="93d20-328">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="93d20-329">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="93d20-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="93d20-330">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="93d20-331">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="93d20-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="93d20-332">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="93d20-333">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="93d20-333">Basic usage</span></span>

<span data-ttu-id="93d20-334">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93d20-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="93d20-335">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93d20-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="93d20-336">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="93d20-336">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="93d20-337">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="93d20-338">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="93d20-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="93d20-339">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="93d20-340">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-340">Named clients</span></span>

<span data-ttu-id="93d20-341">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="93d20-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="93d20-342">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93d20-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="93d20-343">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="93d20-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="93d20-344">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="93d20-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="93d20-345">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="93d20-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="93d20-346">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="93d20-347">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="93d20-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="93d20-348">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="93d20-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="93d20-349">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="93d20-350">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="93d20-350">Typed clients</span></span>

<span data-ttu-id="93d20-351">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="93d20-351">Typed clients:</span></span>

* <span data-ttu-id="93d20-352">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="93d20-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="93d20-353">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="93d20-354">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="93d20-355">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="93d20-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="93d20-356">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="93d20-357">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="93d20-357">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="93d20-358">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="93d20-359">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="93d20-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="93d20-360">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="93d20-361">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="93d20-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="93d20-362">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="93d20-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="93d20-363">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="93d20-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="93d20-364">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="93d20-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="93d20-365">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="93d20-366">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="93d20-367">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="93d20-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="93d20-368">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="93d20-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="93d20-369">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="93d20-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="93d20-370">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-370">Generated clients</span></span>

<span data-ttu-id="93d20-371">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="93d20-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="93d20-372">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="93d20-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="93d20-373">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="93d20-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="93d20-374">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="93d20-375">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="93d20-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="93d20-376">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="93d20-376">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="93d20-377">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="93d20-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="93d20-378">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="93d20-378">Outgoing request middleware</span></span>

<span data-ttu-id="93d20-379">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="93d20-380">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="93d20-381">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="93d20-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="93d20-382">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="93d20-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="93d20-383">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93d20-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="93d20-384">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="93d20-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="93d20-385">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="93d20-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="93d20-386">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="93d20-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="93d20-387">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="93d20-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="93d20-388">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="93d20-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="93d20-389">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="93d20-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="93d20-390">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-390">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="93d20-391">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="93d20-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="93d20-392">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="93d20-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="93d20-393">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="93d20-394">Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="93d20-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="93d20-395">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="93d20-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="93d20-396">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="93d20-397">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="93d20-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="93d20-398">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="93d20-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="93d20-399">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="93d20-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="93d20-400">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="93d20-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="93d20-401">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="93d20-402">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="93d20-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="93d20-403">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-403">Use Polly-based handlers</span></span>

<span data-ttu-id="93d20-404">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="93d20-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="93d20-405">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="93d20-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="93d20-406">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="93d20-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="93d20-407">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="93d20-408">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="93d20-408">The Polly extensions:</span></span>

* <span data-ttu-id="93d20-409">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="93d20-410">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="93d20-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="93d20-411">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="93d20-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="93d20-412">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="93d20-412">Handle transient faults</span></span>

<span data-ttu-id="93d20-413">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="93d20-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="93d20-414">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="93d20-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="93d20-415">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="93d20-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="93d20-416">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93d20-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="93d20-417">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="93d20-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="93d20-418">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="93d20-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="93d20-419">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="93d20-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="93d20-420">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="93d20-420">Dynamically select policies</span></span>

<span data-ttu-id="93d20-421">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="93d20-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="93d20-422">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="93d20-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="93d20-423">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="93d20-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="93d20-424">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="93d20-425">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="93d20-426">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="93d20-427">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="93d20-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="93d20-428">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="93d20-429">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="93d20-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="93d20-430">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="93d20-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="93d20-431">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="93d20-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="93d20-432">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="93d20-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="93d20-433">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="93d20-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="93d20-434">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="93d20-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="93d20-435">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="93d20-436">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="93d20-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="93d20-437">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="93d20-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="93d20-438">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="93d20-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="93d20-439">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="93d20-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="93d20-440">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="93d20-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="93d20-441">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="93d20-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="93d20-442">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="93d20-443">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="93d20-444">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="93d20-445">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="93d20-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="93d20-446">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="93d20-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="93d20-447">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="93d20-448">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="93d20-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="93d20-449">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="93d20-450">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="93d20-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="93d20-451">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="93d20-452">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="93d20-453">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="93d20-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="93d20-454">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="93d20-455">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="93d20-456">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="93d20-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="93d20-457">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="93d20-458">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="93d20-459">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="93d20-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="93d20-460">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="93d20-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="93d20-461">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="93d20-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="93d20-462">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-462">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="93d20-463">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="93d20-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="93d20-464">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="93d20-465">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="93d20-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="93d20-466">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="93d20-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="93d20-467">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="93d20-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="93d20-468">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="93d20-469">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="93d20-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="93d20-470">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="93d20-470">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="93d20-471">Cookie</span><span class="sxs-lookup"><span data-stu-id="93d20-471">Cookies</span></span>

<span data-ttu-id="93d20-472">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="93d20-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="93d20-473">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="93d20-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="93d20-474">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="93d20-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="93d20-475">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="93d20-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="93d20-476">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="93d20-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="93d20-477">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="93d20-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="93d20-478">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="93d20-478">Logging</span></span>

<span data-ttu-id="93d20-479">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="93d20-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="93d20-480">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="93d20-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="93d20-481">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="93d20-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="93d20-482">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="93d20-483">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="93d20-484">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="93d20-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="93d20-485">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="93d20-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="93d20-486">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="93d20-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="93d20-487">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="93d20-488">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="93d20-489">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="93d20-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="93d20-490">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="93d20-491">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="93d20-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="93d20-492">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="93d20-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="93d20-493">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="93d20-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="93d20-494">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="93d20-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="93d20-495">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="93d20-496">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="93d20-497">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="93d20-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="93d20-498">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="93d20-499">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="93d20-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="93d20-500">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="93d20-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="93d20-501">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="93d20-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="93d20-502">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="93d20-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="93d20-503">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="93d20-503">In the following example:</span></span>

* <span data-ttu-id="93d20-504"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="93d20-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="93d20-505">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="93d20-506">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="93d20-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="93d20-507">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="93d20-508">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="93d20-508">Additional resources</span></span>

* [<span data-ttu-id="93d20-509">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="93d20-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="93d20-510">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="93d20-511">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="93d20-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="93d20-512">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="93d20-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="93d20-513"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="93d20-514">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="93d20-514">It offers the following benefits:</span></span>

* <span data-ttu-id="93d20-515">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="93d20-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="93d20-516">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="93d20-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="93d20-517">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="93d20-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="93d20-518">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="93d20-519">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="93d20-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="93d20-520">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="93d20-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="93d20-521">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93d20-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93d20-522">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="93d20-522">Prerequisites</span></span>

<span data-ttu-id="93d20-523">Projekty docelowe .NET Framework wymagają instalacji pakietu NuGet [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) .</span><span class="sxs-lookup"><span data-stu-id="93d20-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="93d20-524">Projekty przeznaczone dla platformy .NET Core i odwołujące się do [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) zawierają już pakiet `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="93d20-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="93d20-525">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="93d20-525">Consumption patterns</span></span>

<span data-ttu-id="93d20-526">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="93d20-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="93d20-527">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="93d20-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="93d20-528">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="93d20-529">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="93d20-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="93d20-530">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="93d20-531">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="93d20-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="93d20-532">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="93d20-533">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="93d20-533">Basic usage</span></span>

<span data-ttu-id="93d20-534">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93d20-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="93d20-535">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93d20-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="93d20-536">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="93d20-536">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="93d20-537">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="93d20-538">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="93d20-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="93d20-539">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="93d20-540">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-540">Named clients</span></span>

<span data-ttu-id="93d20-541">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="93d20-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="93d20-542">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93d20-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="93d20-543">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="93d20-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="93d20-544">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="93d20-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="93d20-545">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="93d20-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="93d20-546">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="93d20-547">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="93d20-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="93d20-548">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="93d20-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="93d20-549">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="93d20-550">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="93d20-550">Typed clients</span></span>

<span data-ttu-id="93d20-551">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="93d20-551">Typed clients:</span></span>

* <span data-ttu-id="93d20-552">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="93d20-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="93d20-553">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="93d20-554">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="93d20-555">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="93d20-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="93d20-556">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="93d20-557">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="93d20-557">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="93d20-558">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="93d20-559">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="93d20-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="93d20-560">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="93d20-561">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="93d20-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="93d20-562">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="93d20-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="93d20-563">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="93d20-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="93d20-564">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="93d20-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="93d20-565">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="93d20-566">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="93d20-567">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="93d20-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="93d20-568">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="93d20-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="93d20-569">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="93d20-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="93d20-570">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="93d20-570">Generated clients</span></span>

<span data-ttu-id="93d20-571">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="93d20-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="93d20-572">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="93d20-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="93d20-573">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="93d20-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="93d20-574">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="93d20-575">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="93d20-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="93d20-576">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="93d20-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="93d20-577">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="93d20-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="93d20-578">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="93d20-578">Outgoing request middleware</span></span>

<span data-ttu-id="93d20-579">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="93d20-580">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="93d20-581">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="93d20-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="93d20-582">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="93d20-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="93d20-583">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93d20-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="93d20-584">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="93d20-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="93d20-585">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="93d20-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="93d20-586">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="93d20-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="93d20-587">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="93d20-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="93d20-588">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="93d20-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="93d20-589">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="93d20-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="93d20-590">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-590">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="93d20-591">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="93d20-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="93d20-592">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="93d20-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="93d20-593">Procedura obsługi **musi** być zarejestrowana w programie di jako przejściowa usługa, nigdy nie jest objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="93d20-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="93d20-594">Jeśli program obsługi jest zarejestrowany jako usługa objęta zakresem, a wszystkie usługi, od których zależy program obsługi, są jednorazowe:</span><span class="sxs-lookup"><span data-stu-id="93d20-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="93d20-595">Usługi programu obsługi mogły zostać zlikwidowane, zanim program obsługi znajdzie się poza zakresem.</span><span class="sxs-lookup"><span data-stu-id="93d20-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="93d20-596">Usunięte usługi obsługi powodują niepowodzenie procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="93d20-597">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując w typ procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="93d20-598">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="93d20-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="93d20-599">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="93d20-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="93d20-600">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="93d20-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="93d20-601">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="93d20-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="93d20-602">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="93d20-603">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="93d20-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="93d20-604">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-604">Use Polly-based handlers</span></span>

<span data-ttu-id="93d20-605">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="93d20-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="93d20-606">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="93d20-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="93d20-607">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="93d20-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="93d20-608">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="93d20-609">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="93d20-609">The Polly extensions:</span></span>

* <span data-ttu-id="93d20-610">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="93d20-611">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="93d20-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="93d20-612">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="93d20-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="93d20-613">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="93d20-613">Handle transient faults</span></span>

<span data-ttu-id="93d20-614">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="93d20-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="93d20-615">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="93d20-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="93d20-616">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="93d20-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="93d20-617">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="93d20-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="93d20-618">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="93d20-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="93d20-619">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="93d20-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="93d20-620">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="93d20-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="93d20-621">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="93d20-621">Dynamically select policies</span></span>

<span data-ttu-id="93d20-622">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="93d20-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="93d20-623">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="93d20-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="93d20-624">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="93d20-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="93d20-625">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="93d20-626">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="93d20-627">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="93d20-628">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="93d20-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="93d20-629">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="93d20-630">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="93d20-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="93d20-631">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="93d20-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="93d20-632">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="93d20-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="93d20-633">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="93d20-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="93d20-634">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="93d20-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="93d20-635">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="93d20-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="93d20-636">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="93d20-637">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="93d20-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="93d20-638">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="93d20-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="93d20-639">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="93d20-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="93d20-640">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="93d20-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="93d20-641">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="93d20-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="93d20-642">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="93d20-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="93d20-643">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="93d20-644">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="93d20-645">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="93d20-646">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="93d20-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="93d20-647">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="93d20-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="93d20-648">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="93d20-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="93d20-649">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="93d20-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="93d20-650">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="93d20-651">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="93d20-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="93d20-652">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="93d20-653">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="93d20-654">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="93d20-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="93d20-655">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="93d20-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="93d20-656">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="93d20-657">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="93d20-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="93d20-658">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="93d20-659">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="93d20-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="93d20-660">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="93d20-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="93d20-661">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="93d20-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="93d20-662">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="93d20-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="93d20-663">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="93d20-663">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="93d20-664">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="93d20-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="93d20-665">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93d20-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="93d20-666">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="93d20-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="93d20-667">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="93d20-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="93d20-668">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="93d20-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="93d20-669">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="93d20-670">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="93d20-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="93d20-671">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="93d20-671">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="93d20-672">Cookie</span><span class="sxs-lookup"><span data-stu-id="93d20-672">Cookies</span></span>

<span data-ttu-id="93d20-673">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="93d20-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="93d20-674">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="93d20-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="93d20-675">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="93d20-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="93d20-676">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="93d20-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="93d20-677">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="93d20-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="93d20-678">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="93d20-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="93d20-679">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="93d20-679">Logging</span></span>

<span data-ttu-id="93d20-680">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="93d20-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="93d20-681">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="93d20-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="93d20-682">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="93d20-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="93d20-683">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="93d20-684">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="93d20-685">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="93d20-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="93d20-686">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="93d20-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="93d20-687">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="93d20-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="93d20-688">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="93d20-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="93d20-689">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="93d20-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="93d20-690">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="93d20-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="93d20-691">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="93d20-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="93d20-692">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="93d20-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="93d20-693">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="93d20-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="93d20-694">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="93d20-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="93d20-695">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="93d20-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="93d20-696">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="93d20-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="93d20-697">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="93d20-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="93d20-698">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="93d20-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="93d20-699">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="93d20-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="93d20-700">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="93d20-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="93d20-701">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="93d20-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="93d20-702">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="93d20-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="93d20-703">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="93d20-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="93d20-704">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="93d20-704">In the following example:</span></span>

* <span data-ttu-id="93d20-705"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="93d20-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="93d20-706">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="93d20-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="93d20-707">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="93d20-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="93d20-708">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="93d20-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="93d20-709">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="93d20-709">Additional resources</span></span>

* [<span data-ttu-id="93d20-710">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="93d20-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="93d20-711">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="93d20-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="93d20-712">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="93d20-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
