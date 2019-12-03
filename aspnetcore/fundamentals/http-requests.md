---
title: Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core
author: stevejgordon
description: Dowiedz się więcej o używaniu interfejsu IHttpClientFactory do zarządzania wystąpieniami logicznych HttpClient w ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/27/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 746604bc92775a6fac124ee8bfcf37635786fe41
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717012"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="48db2-103">Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48db2-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="48db2-104">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)i [Kirka Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="48db2-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="48db2-105"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="48db2-106">`IHttpClientFactory` oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="48db2-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="48db2-107">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="48db2-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="48db2-108">Na przykład klient o nazwie *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="48db2-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="48db2-109">Domyślny klient można zarejestrować na potrzeby ogólnego dostępu.</span><span class="sxs-lookup"><span data-stu-id="48db2-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="48db2-110">Kodyfikacja koncepcji wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="48db2-111">Udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, które umożliwiają delegowanie programów obsługi w `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="48db2-112">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="48db2-113">Automatyczne zarządzanie pozwala uniknąć typowych problemów z usługą DNS (Domain Name System) występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="48db2-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="48db2-114">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="48db2-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="48db2-115">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="48db2-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="48db2-116">Przykładowy kod w tej wersji tematu używa <xref:System.Text.Json> do deserializacji zawartości JSON zwróconej w odpowiedziach HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="48db2-117">Aby uzyskać przykłady, które używają `Json.NET` i `ReadAsAsync<T>`, użyj selektora wersji, aby wybrać wersję 2. x tego tematu.</span><span class="sxs-lookup"><span data-stu-id="48db2-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="48db2-118">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="48db2-118">Consumption patterns</span></span>

<span data-ttu-id="48db2-119">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="48db2-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="48db2-120">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="48db2-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="48db2-121">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="48db2-122">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="48db2-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="48db2-123">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="48db2-124">Najlepsze podejście zależy od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="48db2-125">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="48db2-125">Basic usage</span></span>

<span data-ttu-id="48db2-126">`IHttpClientFactory` można zarejestrować, wywołując `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="48db2-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="48db2-127">`IHttpClientFactory` można żądać przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="48db2-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48db2-128">Poniższy kod używa `IHttpClientFactory`, aby utworzyć wystąpienie `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="48db2-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="48db2-129">Użycie `IHttpClientFactory` podobnej do powyższego przykładu jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="48db2-130">Nie ma to wpływu na sposób użycia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="48db2-131">W miejscach, w których `HttpClient` wystąpienia są tworzone w istniejącej aplikacji, Zastąp te wystąpienia wywołaniami <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="48db2-132">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-132">Named clients</span></span>

<span data-ttu-id="48db2-133">Nazwani klienci są dobrym wyborem w przypadku:</span><span class="sxs-lookup"><span data-stu-id="48db2-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="48db2-134">Aplikacja wymaga wielu odrębnych użycia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="48db2-135">Wiele `HttpClient`s ma inną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="48db2-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="48db2-136">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="48db2-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="48db2-137">W powyższym kodzie klient ma skonfigurowany program:</span><span class="sxs-lookup"><span data-stu-id="48db2-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="48db2-138">Adres podstawowy `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="48db2-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="48db2-139">Dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="48db2-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="48db2-140">Klient</span><span class="sxs-lookup"><span data-stu-id="48db2-140">CreateClient</span></span>

<span data-ttu-id="48db2-141">Za każdym razem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="48db2-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="48db2-142">Zostanie utworzone nowe wystąpienie `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="48db2-143">Akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="48db2-143">The configuration action is called.</span></span>

<span data-ttu-id="48db2-144">Aby utworzyć nazwanego klienta, Przekaż jego nazwę do `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="48db2-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="48db2-145">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="48db2-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="48db2-146">Kod może przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="48db2-147">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="48db2-147">Typed clients</span></span>

<span data-ttu-id="48db2-148">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="48db2-148">Typed clients:</span></span>

* <span data-ttu-id="48db2-149">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="48db2-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="48db2-150">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="48db2-151">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="48db2-152">Na przykład może zostać użyty pojedynczy klient z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="48db2-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="48db2-153">Dla pojedynczego punktu końcowego zaplecza.</span><span class="sxs-lookup"><span data-stu-id="48db2-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="48db2-154">Do hermetyzacji wszystkich logiki związanych z punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="48db2-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="48db2-155">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="48db2-156">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="48db2-156">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="48db2-157">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="48db2-157">In the preceding code:</span></span>

* <span data-ttu-id="48db2-158">Konfiguracja zostanie przeniesiona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="48db2-159">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="48db2-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="48db2-160">Można tworzyć metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcje.</span><span class="sxs-lookup"><span data-stu-id="48db2-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="48db2-161">Na przykład Metoda `GetAspNetDocsIssues` hermetyzuje kod w celu pobrania otwartych problemów.</span><span class="sxs-lookup"><span data-stu-id="48db2-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="48db2-162">Następujący kod wywołuje <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> w `Startup.ConfigureServices` w celu zarejestrowania klasy klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="48db2-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="48db2-163">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="48db2-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="48db2-164">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="48db2-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="48db2-165">Konfigurację dla określonego klienta można określić podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="48db2-166">`HttpClient` można hermetyzować w obrębie klienta z określonym typem.</span><span class="sxs-lookup"><span data-stu-id="48db2-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="48db2-167">Zamiast uwidaczniać ją jako właściwość, zdefiniuj metodę, która wywołuje wystąpienie `HttpClient` wewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="48db2-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="48db2-168">W poprzednim kodzie `HttpClient` jest przechowywany w prywatnym polu.</span><span class="sxs-lookup"><span data-stu-id="48db2-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="48db2-169">Dostęp do `HttpClient` odbywa się za pomocą metody `GetRepos` publicznej.</span><span class="sxs-lookup"><span data-stu-id="48db2-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="48db2-170">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-170">Generated clients</span></span>

<span data-ttu-id="48db2-171">`IHttpClientFactory` można używać w połączeniu z bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="48db2-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="48db2-172">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48db2-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="48db2-173">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="48db2-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="48db2-174">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="48db2-175">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="48db2-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="48db2-176">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="48db2-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="48db2-177">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="48db2-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="48db2-178">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="48db2-178">Outgoing request middleware</span></span>

<span data-ttu-id="48db2-179">`HttpClient` ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="48db2-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="48db2-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="48db2-181">Upraszcza definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="48db2-182">Obsługuje rejestrację i łańcuch wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="48db2-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="48db2-183">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="48db2-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="48db2-184">Ten wzorzec:</span><span class="sxs-lookup"><span data-stu-id="48db2-184">This pattern:</span></span>

  * <span data-ttu-id="48db2-185">Jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="48db2-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="48db2-186">Program udostępnia mechanizm zarządzania problemami z obcinaniem między żądaniami HTTP, takimi jak:</span><span class="sxs-lookup"><span data-stu-id="48db2-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="48db2-187">buforowanie</span><span class="sxs-lookup"><span data-stu-id="48db2-187">caching</span></span>
    * <span data-ttu-id="48db2-188">obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="48db2-188">error handling</span></span>
    * <span data-ttu-id="48db2-189">serializacja</span><span class="sxs-lookup"><span data-stu-id="48db2-189">serialization</span></span>
    * <span data-ttu-id="48db2-190">rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="48db2-190">logging</span></span>

<span data-ttu-id="48db2-191">Aby utworzyć program obsługi delegowania:</span><span class="sxs-lookup"><span data-stu-id="48db2-191">To create a delegating handler:</span></span>

* <span data-ttu-id="48db2-192">Pochodny od <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="48db2-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="48db2-193">Zastąp <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="48db2-194">Wykonaj kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="48db2-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="48db2-195">Poprzedni kod sprawdza, czy nagłówek `X-API-KEY` jest w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="48db2-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="48db2-196">Jeśli brakuje `X-API-KEY`, zwracany jest <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="48db2-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="48db2-197">Do konfiguracji `HttpClient` można dodać więcej niż jeden program obsługi z <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="48db2-197">More than one handler can be added to the configuration for a `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="48db2-198">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="48db2-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="48db2-199">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="48db2-200">Programy obsługi mogą zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="48db2-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="48db2-201">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="48db2-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="48db2-202">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="48db2-203">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="48db2-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="48db2-204">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="48db2-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="48db2-205">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="48db2-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="48db2-206">Przekaż dane do procedury obsługi przy użyciu [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="48db2-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="48db2-207">Użyj <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="48db2-208">Utwórz niestandardowy obiekt magazynu <xref:System.Threading.AsyncLocal`1>, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="48db2-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="48db2-209">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-209">Use Polly-based handlers</span></span>

<span data-ttu-id="48db2-210">`IHttpClientFactory` integruje się z biblioteką innych firm [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="48db2-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="48db2-211">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48db2-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="48db2-212">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="48db2-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="48db2-213">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="48db2-214">Rozszerzenia Polly obsługują dodawanie programów obsługi opartych na Polly do klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="48db2-215">Polly wymaga pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="48db2-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="48db2-216">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="48db2-216">Handle transient faults</span></span>

<span data-ttu-id="48db2-217">Błędy są zwykle wykonywane, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="48db2-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="48db2-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="48db2-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="48db2-219">Zasady skonfigurowane z `AddTransientHttpErrorPolicy` obsługują następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="48db2-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="48db2-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="48db2-220">HTTP 5xx</span></span>
* <span data-ttu-id="48db2-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="48db2-221">HTTP 408</span></span>

<span data-ttu-id="48db2-222">`AddTransientHttpErrorPolicy` zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowany do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="48db2-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="48db2-223">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="48db2-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="48db2-224">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="48db2-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="48db2-225">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="48db2-225">Dynamically select policies</span></span>

<span data-ttu-id="48db2-226">Metody rozszerzające umożliwiają dodawanie programów obsługi opartych na Polly, na przykład <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="48db2-227">Następujące `AddPolicyHandler` Przeciążenie sprawdza żądanie, aby zdecydować, które zasady zastosować:</span><span class="sxs-lookup"><span data-stu-id="48db2-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="48db2-228">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="48db2-229">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="48db2-230">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="48db2-231">Jest to typowy sposób zagnieżdżania zasad Pollyymi:</span><span class="sxs-lookup"><span data-stu-id="48db2-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="48db2-232">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="48db2-232">In the preceding example:</span></span>

* <span data-ttu-id="48db2-233">Dodawane są dwa procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-233">Two handlers are added.</span></span>
* <span data-ttu-id="48db2-234">Pierwsza procedura obsługi używa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="48db2-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="48db2-235">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="48db2-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="48db2-236">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="48db2-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="48db2-237">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli 5 nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="48db2-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="48db2-238">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="48db2-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="48db2-239">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="48db2-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="48db2-240">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="48db2-241">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="48db2-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="48db2-242">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="48db2-242">In the following code:</span></span>

* <span data-ttu-id="48db2-243">Dodawane są zasady "regularne" i "długie".</span><span class="sxs-lookup"><span data-stu-id="48db2-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="48db2-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> dodaje zasady "regularne" i "długie" z rejestru.</span><span class="sxs-lookup"><span data-stu-id="48db2-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="48db2-245">Aby uzyskać więcej informacji na temat integracji `IHttpClientFactory` i Polly, zobacz [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="48db2-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="48db2-246">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="48db2-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="48db2-247">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="48db2-248"><xref:System.Net.Http.HttpMessageHandler> jest tworzony na Nazwa klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="48db2-249">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="48db2-250">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="48db2-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="48db2-251">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="48db2-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="48db2-252">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="48db2-253">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="48db2-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="48db2-254">Niektóre programy obsługi powodują również, że połączenia są otwierane w nieskończoność, co może uniemożliwić oddziałanie programu obsługi w systemie DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="48db2-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="48db2-255">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="48db2-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="48db2-256">Wartość domyślną można przesłonić na podstawie nazwy klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="48db2-257">wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które **nie** wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="48db2-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="48db2-258">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="48db2-259">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="48db2-260">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="48db2-261">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="48db2-262">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="48db2-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="48db2-263">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="48db2-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="48db2-264">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="48db2-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="48db2-265">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="48db2-266">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="48db2-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="48db2-267">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="48db2-268">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="48db2-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="48db2-269">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="48db2-269">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="48db2-270">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="48db2-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="48db2-271">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="48db2-272">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="48db2-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="48db2-273">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="48db2-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="48db2-274">Cookie</span><span class="sxs-lookup"><span data-stu-id="48db2-274">Cookies</span></span>

<span data-ttu-id="48db2-275">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="48db2-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="48db2-276">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="48db2-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="48db2-277">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="48db2-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="48db2-278">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="48db2-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="48db2-279">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="48db2-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="48db2-280">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="48db2-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="48db2-281">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="48db2-281">Logging</span></span>

<span data-ttu-id="48db2-282">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="48db2-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="48db2-283">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="48db2-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="48db2-284">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="48db2-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="48db2-285">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="48db2-286">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="48db2-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="48db2-287">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="48db2-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="48db2-288">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="48db2-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="48db2-289">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="48db2-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="48db2-290">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="48db2-291">W przykładzie *MyNamedClient* te komunikaty są rejestrowane z kategorią dziennika "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="48db2-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="48db2-292">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="48db2-293">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="48db2-294">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="48db2-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="48db2-295">Może to obejmować zmiany w nagłówkach żądania lub w kodzie stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="48db2-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="48db2-296">Uwzględnienie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="48db2-297">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="48db2-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="48db2-298">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="48db2-299">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="48db2-300">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="48db2-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="48db2-301">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="48db2-302">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="48db2-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="48db2-303">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="48db2-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="48db2-304">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="48db2-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="48db2-305">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="48db2-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="48db2-306">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="48db2-306">In the following example:</span></span>

* <span data-ttu-id="48db2-307"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="48db2-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="48db2-308">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="48db2-309">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="48db2-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="48db2-310">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="48db2-311">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48db2-311">Additional resources</span></span>

* [<span data-ttu-id="48db2-312">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="48db2-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="48db2-313">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="48db2-314">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="48db2-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="48db2-315">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="48db2-315">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="48db2-316"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-316">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="48db2-317">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="48db2-317">It offers the following benefits:</span></span>

* <span data-ttu-id="48db2-318">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="48db2-318">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="48db2-319">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="48db2-319">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="48db2-320">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="48db2-320">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="48db2-321">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-321">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="48db2-322">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="48db2-322">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="48db2-323">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="48db2-323">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="48db2-324">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48db2-324">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="48db2-325">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="48db2-325">Consumption patterns</span></span>

<span data-ttu-id="48db2-326">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="48db2-326">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="48db2-327">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="48db2-327">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="48db2-328">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-328">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="48db2-329">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="48db2-329">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="48db2-330">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-330">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="48db2-331">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="48db2-331">None of them are strictly superior to another.</span></span> <span data-ttu-id="48db2-332">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-332">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="48db2-333">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="48db2-333">Basic usage</span></span>

<span data-ttu-id="48db2-334">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48db2-334">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="48db2-335">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="48db2-335">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48db2-336">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="48db2-336">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="48db2-337">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-337">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="48db2-338">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="48db2-338">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="48db2-339">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-339">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="48db2-340">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-340">Named clients</span></span>

<span data-ttu-id="48db2-341">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="48db2-341">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="48db2-342">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48db2-342">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="48db2-343">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="48db2-343">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="48db2-344">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="48db2-344">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="48db2-345">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="48db2-345">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="48db2-346">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-346">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="48db2-347">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="48db2-347">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="48db2-348">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="48db2-348">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="48db2-349">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-349">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="48db2-350">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="48db2-350">Typed clients</span></span>

<span data-ttu-id="48db2-351">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="48db2-351">Typed clients:</span></span>

* <span data-ttu-id="48db2-352">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="48db2-352">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="48db2-353">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-353">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="48db2-354">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-354">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="48db2-355">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="48db2-355">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="48db2-356">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-356">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="48db2-357">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="48db2-357">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="48db2-358">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-358">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="48db2-359">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="48db2-359">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="48db2-360">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-360">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="48db2-361">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="48db2-361">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="48db2-362">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="48db2-362">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="48db2-363">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="48db2-363">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="48db2-364">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="48db2-364">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="48db2-365">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-365">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="48db2-366">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-366">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="48db2-367">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="48db2-367">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="48db2-368">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="48db2-368">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="48db2-369">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="48db2-369">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="48db2-370">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-370">Generated clients</span></span>

<span data-ttu-id="48db2-371">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="48db2-371">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="48db2-372">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48db2-372">Refit is a REST library for .NET.</span></span> <span data-ttu-id="48db2-373">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="48db2-373">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="48db2-374">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-374">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="48db2-375">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="48db2-375">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="48db2-376">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="48db2-376">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="48db2-377">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="48db2-377">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="48db2-378">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="48db2-378">Outgoing request middleware</span></span>

<span data-ttu-id="48db2-379">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-379">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="48db2-380">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-380">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="48db2-381">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="48db2-381">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="48db2-382">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="48db2-382">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="48db2-383">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="48db2-383">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="48db2-384">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="48db2-384">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="48db2-385">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="48db2-385">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="48db2-386">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="48db2-386">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="48db2-387">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="48db2-387">The preceding code defines a basic handler.</span></span> <span data-ttu-id="48db2-388">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="48db2-388">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="48db2-389">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="48db2-389">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="48db2-390">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-390">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="48db2-391">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="48db2-391">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="48db2-392">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="48db2-392">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="48db2-393">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-393">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="48db2-394">Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="48db2-394">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="48db2-395">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="48db2-395">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="48db2-396">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-396">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="48db2-397">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="48db2-397">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="48db2-398">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="48db2-398">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="48db2-399">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="48db2-399">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="48db2-400">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="48db2-400">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="48db2-401">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-401">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="48db2-402">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="48db2-402">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="48db2-403">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-403">Use Polly-based handlers</span></span>

<span data-ttu-id="48db2-404">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="48db2-404">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="48db2-405">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48db2-405">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="48db2-406">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="48db2-406">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="48db2-407">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-407">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="48db2-408">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="48db2-408">The Polly extensions:</span></span>

* <span data-ttu-id="48db2-409">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-409">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="48db2-410">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="48db2-410">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="48db2-411">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="48db2-411">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="48db2-412">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="48db2-412">Handle transient faults</span></span>

<span data-ttu-id="48db2-413">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="48db2-413">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="48db2-414">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="48db2-414">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="48db2-415">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="48db2-415">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="48db2-416">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48db2-416">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="48db2-417">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="48db2-417">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="48db2-418">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="48db2-418">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="48db2-419">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="48db2-419">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="48db2-420">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="48db2-420">Dynamically select policies</span></span>

<span data-ttu-id="48db2-421">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="48db2-421">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="48db2-422">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="48db2-422">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="48db2-423">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="48db2-423">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="48db2-424">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-424">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="48db2-425">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-425">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="48db2-426">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-426">Add multiple Polly handlers</span></span>

<span data-ttu-id="48db2-427">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="48db2-427">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="48db2-428">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-428">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="48db2-429">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="48db2-429">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="48db2-430">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="48db2-430">Failed requests are retried up to three times.</span></span> <span data-ttu-id="48db2-431">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="48db2-431">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="48db2-432">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="48db2-432">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="48db2-433">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="48db2-433">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="48db2-434">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="48db2-434">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="48db2-435">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-435">Add policies from the Polly registry</span></span>

<span data-ttu-id="48db2-436">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="48db2-436">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="48db2-437">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="48db2-437">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="48db2-438">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="48db2-438">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="48db2-439">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="48db2-439">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="48db2-440">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="48db2-440">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="48db2-441">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="48db2-441">HttpClient and lifetime management</span></span>

<span data-ttu-id="48db2-442">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-442">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="48db2-443">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-443">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="48db2-444">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-444">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="48db2-445">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="48db2-445">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="48db2-446">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="48db2-446">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="48db2-447">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-447">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="48db2-448">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="48db2-448">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="48db2-449">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-449">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="48db2-450">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="48db2-450">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="48db2-451">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-451">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="48db2-452">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-452">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="48db2-453">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="48db2-453">Disposal of the client isn't required.</span></span> <span data-ttu-id="48db2-454">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-454">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="48db2-455">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-455">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="48db2-456">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="48db2-456">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="48db2-457">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-457">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="48db2-458">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-458">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="48db2-459">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="48db2-459">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="48db2-460">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="48db2-460">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="48db2-461">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="48db2-461">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="48db2-462">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-462">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="48db2-463">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="48db2-463">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="48db2-464">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-464">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="48db2-465">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="48db2-465">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="48db2-466">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="48db2-466">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="48db2-467">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="48db2-467">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="48db2-468">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-468">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="48db2-469">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="48db2-469">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="48db2-470">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="48db2-470">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="48db2-471">Cookie</span><span class="sxs-lookup"><span data-stu-id="48db2-471">Cookies</span></span>

<span data-ttu-id="48db2-472">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="48db2-472">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="48db2-473">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="48db2-473">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="48db2-474">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="48db2-474">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="48db2-475">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="48db2-475">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="48db2-476">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="48db2-476">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="48db2-477">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="48db2-477">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="48db2-478">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="48db2-478">Logging</span></span>

<span data-ttu-id="48db2-479">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="48db2-479">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="48db2-480">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="48db2-480">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="48db2-481">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="48db2-481">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="48db2-482">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-482">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="48db2-483">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-483">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="48db2-484">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="48db2-484">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="48db2-485">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="48db2-485">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="48db2-486">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="48db2-486">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="48db2-487">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-487">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="48db2-488">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-488">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="48db2-489">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="48db2-489">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="48db2-490">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-490">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="48db2-491">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="48db2-491">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="48db2-492">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="48db2-492">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="48db2-493">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="48db2-493">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="48db2-494">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="48db2-494">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="48db2-495">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-495">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="48db2-496">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-496">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="48db2-497">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="48db2-497">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="48db2-498">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-498">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="48db2-499">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="48db2-499">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="48db2-500">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="48db2-500">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="48db2-501">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="48db2-501">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="48db2-502">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="48db2-502">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="48db2-503">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="48db2-503">In the following example:</span></span>

* <span data-ttu-id="48db2-504"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="48db2-504"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="48db2-505">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-505">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="48db2-506">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="48db2-506">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="48db2-507">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-507">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="48db2-508">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48db2-508">Additional resources</span></span>

* [<span data-ttu-id="48db2-509">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="48db2-509">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="48db2-510">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-510">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="48db2-511">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="48db2-511">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

<span data-ttu-id="48db2-512">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="48db2-512">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="48db2-513"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-513">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="48db2-514">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="48db2-514">It offers the following benefits:</span></span>

* <span data-ttu-id="48db2-515">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="48db2-515">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="48db2-516">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="48db2-516">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="48db2-517">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="48db2-517">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="48db2-518">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-518">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="48db2-519">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="48db2-519">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="48db2-520">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="48db2-520">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="48db2-521">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="48db2-521">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48db2-522">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="48db2-522">Prerequisites</span></span>

<span data-ttu-id="48db2-523">Projekty docelowe .NET Framework wymagają instalacji pakietu NuGet [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) .</span><span class="sxs-lookup"><span data-stu-id="48db2-523">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="48db2-524">Projekty przeznaczone dla platformy .NET Core i odwołujące się do [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) zawierają już pakiet `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="48db2-524">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="48db2-525">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="48db2-525">Consumption patterns</span></span>

<span data-ttu-id="48db2-526">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="48db2-526">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="48db2-527">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="48db2-527">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="48db2-528">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-528">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="48db2-529">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="48db2-529">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="48db2-530">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-530">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="48db2-531">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="48db2-531">None of them are strictly superior to another.</span></span> <span data-ttu-id="48db2-532">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-532">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="48db2-533">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="48db2-533">Basic usage</span></span>

<span data-ttu-id="48db2-534">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48db2-534">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="48db2-535">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="48db2-535">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="48db2-536">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="48db2-536">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="48db2-537">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-537">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="48db2-538">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="48db2-538">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="48db2-539">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-539">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="48db2-540">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-540">Named clients</span></span>

<span data-ttu-id="48db2-541">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="48db2-541">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="48db2-542">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48db2-542">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="48db2-543">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="48db2-543">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="48db2-544">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="48db2-544">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="48db2-545">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="48db2-545">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="48db2-546">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-546">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="48db2-547">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="48db2-547">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="48db2-548">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="48db2-548">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="48db2-549">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-549">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="48db2-550">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="48db2-550">Typed clients</span></span>

<span data-ttu-id="48db2-551">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="48db2-551">Typed clients:</span></span>

* <span data-ttu-id="48db2-552">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="48db2-552">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="48db2-553">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-553">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="48db2-554">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-554">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="48db2-555">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="48db2-555">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="48db2-556">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-556">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="48db2-557">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="48db2-557">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="48db2-558">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-558">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="48db2-559">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="48db2-559">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="48db2-560">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-560">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="48db2-561">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="48db2-561">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="48db2-562">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="48db2-562">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="48db2-563">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="48db2-563">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="48db2-564">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="48db2-564">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="48db2-565">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-565">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="48db2-566">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-566">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="48db2-567">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="48db2-567">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="48db2-568">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="48db2-568">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="48db2-569">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="48db2-569">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="48db2-570">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="48db2-570">Generated clients</span></span>

<span data-ttu-id="48db2-571">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="48db2-571">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="48db2-572">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48db2-572">Refit is a REST library for .NET.</span></span> <span data-ttu-id="48db2-573">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="48db2-573">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="48db2-574">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-574">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="48db2-575">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="48db2-575">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="48db2-576">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="48db2-576">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="48db2-577">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="48db2-577">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="48db2-578">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="48db2-578">Outgoing request middleware</span></span>

<span data-ttu-id="48db2-579">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-579">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="48db2-580">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-580">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="48db2-581">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="48db2-581">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="48db2-582">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="48db2-582">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="48db2-583">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="48db2-583">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="48db2-584">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="48db2-584">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="48db2-585">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="48db2-585">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="48db2-586">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="48db2-586">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="48db2-587">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="48db2-587">The preceding code defines a basic handler.</span></span> <span data-ttu-id="48db2-588">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="48db2-588">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="48db2-589">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="48db2-589">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="48db2-590">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-590">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="48db2-591">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="48db2-591">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="48db2-592">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="48db2-592">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="48db2-593">Procedura obsługi **musi** być zarejestrowana w programie di jako przejściowa usługa, nigdy nie jest objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="48db2-593">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="48db2-594">Jeśli program obsługi jest zarejestrowany jako usługa objęta zakresem, a wszystkie usługi, od których zależy program obsługi, są jednorazowe:</span><span class="sxs-lookup"><span data-stu-id="48db2-594">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="48db2-595">Usługi programu obsługi mogły zostać zlikwidowane, zanim program obsługi znajdzie się poza zakresem.</span><span class="sxs-lookup"><span data-stu-id="48db2-595">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="48db2-596">Usunięte usługi obsługi powodują niepowodzenie procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-596">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="48db2-597">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując w typ procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-597">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="48db2-598">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="48db2-598">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="48db2-599">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="48db2-599">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="48db2-600">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="48db2-600">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="48db2-601">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="48db2-601">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="48db2-602">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-602">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="48db2-603">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="48db2-603">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="48db2-604">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-604">Use Polly-based handlers</span></span>

<span data-ttu-id="48db2-605">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="48db2-605">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="48db2-606">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="48db2-606">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="48db2-607">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="48db2-607">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="48db2-608">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-608">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="48db2-609">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="48db2-609">The Polly extensions:</span></span>

* <span data-ttu-id="48db2-610">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-610">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="48db2-611">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="48db2-611">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="48db2-612">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="48db2-612">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="48db2-613">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="48db2-613">Handle transient faults</span></span>

<span data-ttu-id="48db2-614">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="48db2-614">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="48db2-615">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="48db2-615">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="48db2-616">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="48db2-616">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="48db2-617">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="48db2-617">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="48db2-618">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="48db2-618">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="48db2-619">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="48db2-619">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="48db2-620">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="48db2-620">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="48db2-621">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="48db2-621">Dynamically select policies</span></span>

<span data-ttu-id="48db2-622">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="48db2-622">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="48db2-623">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="48db2-623">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="48db2-624">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="48db2-624">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="48db2-625">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-625">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="48db2-626">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-626">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="48db2-627">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-627">Add multiple Polly handlers</span></span>

<span data-ttu-id="48db2-628">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="48db2-628">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="48db2-629">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-629">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="48db2-630">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="48db2-630">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="48db2-631">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="48db2-631">Failed requests are retried up to three times.</span></span> <span data-ttu-id="48db2-632">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="48db2-632">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="48db2-633">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="48db2-633">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="48db2-634">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="48db2-634">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="48db2-635">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="48db2-635">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="48db2-636">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-636">Add policies from the Polly registry</span></span>

<span data-ttu-id="48db2-637">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="48db2-637">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="48db2-638">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="48db2-638">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="48db2-639">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="48db2-639">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="48db2-640">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="48db2-640">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="48db2-641">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="48db2-641">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="48db2-642">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="48db2-642">HttpClient and lifetime management</span></span>

<span data-ttu-id="48db2-643">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-643">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="48db2-644">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-644">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="48db2-645">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-645">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="48db2-646">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="48db2-646">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="48db2-647">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="48db2-647">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="48db2-648">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="48db2-648">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="48db2-649">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="48db2-649">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="48db2-650">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-650">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="48db2-651">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="48db2-651">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="48db2-652">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-652">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="48db2-653">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-653">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="48db2-654">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="48db2-654">Disposal of the client isn't required.</span></span> <span data-ttu-id="48db2-655">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="48db2-655">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="48db2-656">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-656">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="48db2-657">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="48db2-657">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="48db2-658">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-658">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="48db2-659">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="48db2-659">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="48db2-660">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="48db2-660">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="48db2-661">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="48db2-661">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="48db2-662">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="48db2-662">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="48db2-663">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="48db2-663">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="48db2-664">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="48db2-664">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="48db2-665">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="48db2-665">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="48db2-666">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="48db2-666">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="48db2-667">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="48db2-667">Create `HttpClient` instances using `new HttpClient(handler, dispostHandler: false)` as needed.</span></span>

<span data-ttu-id="48db2-668">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="48db2-668">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="48db2-669">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-669">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="48db2-670">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="48db2-670">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="48db2-671">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="48db2-671">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="48db2-672">Cookie</span><span class="sxs-lookup"><span data-stu-id="48db2-672">Cookies</span></span>

<span data-ttu-id="48db2-673">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="48db2-673">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="48db2-674">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="48db2-674">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="48db2-675">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="48db2-675">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="48db2-676">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="48db2-676">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="48db2-677">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="48db2-677">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="48db2-678">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="48db2-678">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="48db2-679">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="48db2-679">Logging</span></span>

<span data-ttu-id="48db2-680">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="48db2-680">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="48db2-681">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="48db2-681">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="48db2-682">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="48db2-682">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="48db2-683">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-683">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="48db2-684">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-684">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="48db2-685">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="48db2-685">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="48db2-686">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="48db2-686">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="48db2-687">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="48db2-687">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="48db2-688">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="48db2-688">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="48db2-689">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="48db2-689">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="48db2-690">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="48db2-690">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="48db2-691">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="48db2-691">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="48db2-692">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="48db2-692">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="48db2-693">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="48db2-693">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="48db2-694">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="48db2-694">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="48db2-695">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="48db2-695">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="48db2-696">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="48db2-696">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="48db2-697">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="48db2-697">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="48db2-698">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="48db2-698">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="48db2-699">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="48db2-699">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="48db2-700">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="48db2-700">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="48db2-701">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="48db2-701">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="48db2-702">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="48db2-702">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="48db2-703">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="48db2-703">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="48db2-704">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="48db2-704">In the following example:</span></span>

* <span data-ttu-id="48db2-705"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="48db2-705"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="48db2-706">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="48db2-706">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="48db2-707">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="48db2-707">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="48db2-708">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="48db2-708">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="48db2-709">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="48db2-709">Additional resources</span></span>

* [<span data-ttu-id="48db2-710">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="48db2-710">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="48db2-711">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="48db2-711">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="48db2-712">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="48db2-712">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
