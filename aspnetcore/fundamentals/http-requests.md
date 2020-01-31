---
title: Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core
author: stevejgordon
description: Dowiedz się więcej o używaniu interfejsu IHttpClientFactory do zarządzania wystąpieniami logicznych HttpClient w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 9b9da82191a587be0603ee114562e9a964f05250
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2020
ms.locfileid: "76870401"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="f774f-103">Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f774f-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f774f-104">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)i [Kirka Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="f774f-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="f774f-105"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="f774f-106">`IHttpClientFactory` oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="f774f-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="f774f-107">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="f774f-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="f774f-108">Na przykład klient o nazwie *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="f774f-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="f774f-109">Domyślny klient można zarejestrować na potrzeby ogólnego dostępu.</span><span class="sxs-lookup"><span data-stu-id="f774f-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="f774f-110">Kodyfikacja koncepcji wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="f774f-111">Udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, które umożliwiają delegowanie programów obsługi w `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="f774f-112">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="f774f-113">Automatyczne zarządzanie pozwala uniknąć typowych problemów z usługą DNS (Domain Name System) występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="f774f-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="f774f-114">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="f774f-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="f774f-115">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f774f-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f774f-116">Przykładowy kod w tej wersji tematu używa <xref:System.Text.Json> do deserializacji zawartości JSON zwróconej w odpowiedziach HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="f774f-117">Aby uzyskać przykłady, które używają `Json.NET` i `ReadAsAsync<T>`, użyj selektora wersji, aby wybrać wersję 2. x tego tematu.</span><span class="sxs-lookup"><span data-stu-id="f774f-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="f774f-118">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="f774f-118">Consumption patterns</span></span>

<span data-ttu-id="f774f-119">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f774f-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="f774f-120">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="f774f-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="f774f-121">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="f774f-122">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="f774f-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="f774f-123">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="f774f-124">Najlepsze podejście zależy od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="f774f-125">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="f774f-125">Basic usage</span></span>

<span data-ttu-id="f774f-126">`IHttpClientFactory` można zarejestrować, wywołując `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f774f-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="f774f-127">`IHttpClientFactory` można żądać przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f774f-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f774f-128">Poniższy kod używa `IHttpClientFactory`, aby utworzyć wystąpienie `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f774f-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="f774f-129">Użycie `IHttpClientFactory` podobnej do powyższego przykładu jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="f774f-130">Nie ma to wpływu na sposób użycia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="f774f-131">W miejscach, w których `HttpClient` wystąpienia są tworzone w istniejącej aplikacji, Zastąp te wystąpienia wywołaniami <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="f774f-132">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-132">Named clients</span></span>

<span data-ttu-id="f774f-133">Nazwani klienci są dobrym wyborem w przypadku:</span><span class="sxs-lookup"><span data-stu-id="f774f-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="f774f-134">Aplikacja wymaga wielu odrębnych użycia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="f774f-135">Wiele `HttpClient`s ma inną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="f774f-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="f774f-136">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f774f-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="f774f-137">W powyższym kodzie klient ma skonfigurowany program:</span><span class="sxs-lookup"><span data-stu-id="f774f-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="f774f-138">Adres podstawowy `https://api.github.com/`.</span><span class="sxs-lookup"><span data-stu-id="f774f-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="f774f-139">Dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="f774f-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="f774f-140">Klient</span><span class="sxs-lookup"><span data-stu-id="f774f-140">CreateClient</span></span>

<span data-ttu-id="f774f-141">Za każdym razem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="f774f-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="f774f-142">Zostanie utworzone nowe wystąpienie `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="f774f-143">Akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f774f-143">The configuration action is called.</span></span>

<span data-ttu-id="f774f-144">Aby utworzyć nazwanego klienta, Przekaż jego nazwę do `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="f774f-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="f774f-145">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="f774f-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="f774f-146">Kod może przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="f774f-147">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="f774f-147">Typed clients</span></span>

<span data-ttu-id="f774f-148">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="f774f-148">Typed clients:</span></span>

* <span data-ttu-id="f774f-149">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="f774f-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="f774f-150">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="f774f-151">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="f774f-152">Na przykład może zostać użyty pojedynczy klient z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="f774f-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="f774f-153">Dla pojedynczego punktu końcowego zaplecza.</span><span class="sxs-lookup"><span data-stu-id="f774f-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="f774f-154">Do hermetyzacji wszystkich logiki związanych z punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="f774f-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="f774f-155">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="f774f-156">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="f774f-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f774f-157">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="f774f-157">In the preceding code:</span></span>

* <span data-ttu-id="f774f-158">Konfiguracja zostanie przeniesiona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="f774f-159">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="f774f-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="f774f-160">Można tworzyć metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcje.</span><span class="sxs-lookup"><span data-stu-id="f774f-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="f774f-161">Na przykład Metoda `GetAspNetDocsIssues` hermetyzuje kod w celu pobrania otwartych problemów.</span><span class="sxs-lookup"><span data-stu-id="f774f-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="f774f-162">Następujący kod wywołuje <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> w `Startup.ConfigureServices` w celu zarejestrowania klasy klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="f774f-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="f774f-163">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="f774f-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="f774f-164">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="f774f-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="f774f-165">Konfigurację dla określonego klienta można określić podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="f774f-166">`HttpClient` można hermetyzować w obrębie klienta z określonym typem.</span><span class="sxs-lookup"><span data-stu-id="f774f-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="f774f-167">Zamiast uwidaczniać ją jako właściwość, zdefiniuj metodę, która wywołuje wystąpienie `HttpClient` wewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="f774f-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="f774f-168">W poprzednim kodzie `HttpClient` jest przechowywany w prywatnym polu.</span><span class="sxs-lookup"><span data-stu-id="f774f-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="f774f-169">Dostęp do `HttpClient` odbywa się za pomocą metody `GetRepos` publicznej.</span><span class="sxs-lookup"><span data-stu-id="f774f-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="f774f-170">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-170">Generated clients</span></span>

<span data-ttu-id="f774f-171">`IHttpClientFactory` można używać w połączeniu z bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="f774f-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="f774f-172">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f774f-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="f774f-173">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="f774f-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="f774f-174">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="f774f-175">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="f774f-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="f774f-176">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="f774f-176">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="f774f-177">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="f774f-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="f774f-178">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="f774f-178">Outgoing request middleware</span></span>

<span data-ttu-id="f774f-179">`HttpClient` ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f774f-180">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="f774f-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="f774f-181">Upraszcza definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="f774f-182">Obsługuje rejestrację i łańcuch wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="f774f-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="f774f-183">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="f774f-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="f774f-184">Ten wzorzec:</span><span class="sxs-lookup"><span data-stu-id="f774f-184">This pattern:</span></span>

  * <span data-ttu-id="f774f-185">Jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f774f-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="f774f-186">Program udostępnia mechanizm zarządzania problemami z obcinaniem między żądaniami HTTP, takimi jak:</span><span class="sxs-lookup"><span data-stu-id="f774f-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="f774f-187">buforowanie</span><span class="sxs-lookup"><span data-stu-id="f774f-187">caching</span></span>
    * <span data-ttu-id="f774f-188">obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="f774f-188">error handling</span></span>
    * <span data-ttu-id="f774f-189">serializacja</span><span class="sxs-lookup"><span data-stu-id="f774f-189">serialization</span></span>
    * <span data-ttu-id="f774f-190">rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="f774f-190">logging</span></span>

<span data-ttu-id="f774f-191">Aby utworzyć program obsługi delegowania:</span><span class="sxs-lookup"><span data-stu-id="f774f-191">To create a delegating handler:</span></span>

* <span data-ttu-id="f774f-192">Pochodny od <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="f774f-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="f774f-193">Zastąp <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="f774f-194">Wykonaj kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="f774f-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="f774f-195">Poprzedni kod sprawdza, czy nagłówek `X-API-KEY` jest w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="f774f-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="f774f-196">Jeśli brakuje `X-API-KEY`, zwracany jest <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="f774f-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="f774f-197">Do konfiguracji `HttpClient` można dodać więcej niż jeden program obsługi z <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="f774f-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="f774f-198">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="f774f-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="f774f-199">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="f774f-200">Programy obsługi mogą zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="f774f-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="f774f-201">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="f774f-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="f774f-202">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="f774f-203">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="f774f-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="f774f-204">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="f774f-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="f774f-205">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="f774f-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="f774f-206">Przekaż dane do procedury obsługi przy użyciu [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="f774f-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="f774f-207">Użyj <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="f774f-208">Utwórz niestandardowy obiekt magazynu <xref:System.Threading.AsyncLocal`1>, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="f774f-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="f774f-209">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-209">Use Polly-based handlers</span></span>

<span data-ttu-id="f774f-210">`IHttpClientFactory` integruje się z biblioteką innych firm [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="f774f-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="f774f-211">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f774f-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="f774f-212">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="f774f-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="f774f-213">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="f774f-214">Rozszerzenia Polly obsługują dodawanie programów obsługi opartych na Polly do klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="f774f-215">Polly wymaga pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="f774f-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="f774f-216">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="f774f-216">Handle transient faults</span></span>

<span data-ttu-id="f774f-217">Błędy są zwykle wykonywane, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="f774f-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="f774f-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="f774f-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="f774f-219">Zasady skonfigurowane z `AddTransientHttpErrorPolicy` obsługują następujące odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="f774f-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="f774f-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="f774f-220">HTTP 5xx</span></span>
* <span data-ttu-id="f774f-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="f774f-221">HTTP 408</span></span>

<span data-ttu-id="f774f-222">`AddTransientHttpErrorPolicy` zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowany do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="f774f-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="f774f-223">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="f774f-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="f774f-224">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="f774f-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="f774f-225">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="f774f-225">Dynamically select policies</span></span>

<span data-ttu-id="f774f-226">Metody rozszerzające umożliwiają dodawanie programów obsługi opartych na Polly, na przykład <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="f774f-227">Następujące `AddPolicyHandler` Przeciążenie sprawdza żądanie, aby zdecydować, które zasady zastosować:</span><span class="sxs-lookup"><span data-stu-id="f774f-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="f774f-228">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="f774f-229">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="f774f-230">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="f774f-231">Jest to typowy sposób zagnieżdżania zasad Pollyymi:</span><span class="sxs-lookup"><span data-stu-id="f774f-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="f774f-232">W poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f774f-232">In the preceding example:</span></span>

* <span data-ttu-id="f774f-233">Dodawane są dwa procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-233">Two handlers are added.</span></span>
* <span data-ttu-id="f774f-234">Pierwsza procedura obsługi używa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="f774f-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="f774f-235">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="f774f-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="f774f-236">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="f774f-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="f774f-237">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli 5 nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="f774f-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="f774f-238">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="f774f-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="f774f-239">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="f774f-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="f774f-240">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="f774f-241">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="f774f-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="f774f-242">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="f774f-242">In the following code:</span></span>

* <span data-ttu-id="f774f-243">Dodawane są zasady "regularne" i "długie".</span><span class="sxs-lookup"><span data-stu-id="f774f-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="f774f-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> dodaje zasady "regularne" i "długie" z rejestru.</span><span class="sxs-lookup"><span data-stu-id="f774f-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="f774f-245">Aby uzyskać więcej informacji na temat integracji `IHttpClientFactory` i Polly, zobacz [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="f774f-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="f774f-246">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="f774f-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="f774f-247">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="f774f-248"><xref:System.Net.Http.HttpMessageHandler> jest tworzony na Nazwa klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="f774f-249">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="f774f-250">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="f774f-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="f774f-251">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="f774f-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="f774f-252">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="f774f-253">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="f774f-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="f774f-254">Niektóre programy obsługi powodują również, że połączenia są otwierane w nieskończoność, co może uniemożliwić oddziałanie programu obsługi w systemie DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="f774f-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="f774f-255">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="f774f-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="f774f-256">Wartość domyślną można przesłonić na podstawie nazwy klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="f774f-257">wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które **nie** wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="f774f-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="f774f-258">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="f774f-259">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="f774f-260">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="f774f-261">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="f774f-262">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="f774f-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="f774f-263">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="f774f-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="f774f-264">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="f774f-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="f774f-265">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="f774f-266">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="f774f-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="f774f-267">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="f774f-268">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="f774f-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="f774f-269">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, disposeHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="f774f-269">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="f774f-270">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="f774f-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="f774f-271">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="f774f-272">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="f774f-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="f774f-273">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="f774f-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="f774f-274">Cookie</span><span class="sxs-lookup"><span data-stu-id="f774f-274">Cookies</span></span>

<span data-ttu-id="f774f-275">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="f774f-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="f774f-276">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="f774f-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="f774f-277">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="f774f-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="f774f-278">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="f774f-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="f774f-279">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="f774f-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="f774f-280">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="f774f-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="f774f-281">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="f774f-281">Logging</span></span>

<span data-ttu-id="f774f-282">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="f774f-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="f774f-283">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="f774f-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="f774f-284">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="f774f-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="f774f-285">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="f774f-286">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="f774f-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="f774f-287">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="f774f-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="f774f-288">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="f774f-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="f774f-289">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="f774f-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="f774f-290">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="f774f-291">W przykładzie *MyNamedClient* te komunikaty są rejestrowane z kategorią dziennika "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="f774f-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="f774f-292">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="f774f-293">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="f774f-294">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="f774f-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="f774f-295">Może to obejmować zmiany w nagłówkach żądania lub w kodzie stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f774f-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="f774f-296">Uwzględnienie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="f774f-297">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="f774f-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="f774f-298">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="f774f-299">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="f774f-300">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="f774f-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="f774f-301">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="f774f-302">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="f774f-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="f774f-303">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="f774f-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="f774f-304">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="f774f-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="f774f-305">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="f774f-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="f774f-306">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f774f-306">In the following example:</span></span>

* <span data-ttu-id="f774f-307"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="f774f-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="f774f-308">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="f774f-309">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f774f-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="f774f-310">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="f774f-311">Oprogramowanie pośredniczące propagacji nagłówka</span><span class="sxs-lookup"><span data-stu-id="f774f-311">Header propagation middleware</span></span>

<span data-ttu-id="f774f-312">Propagacja nagłówka to ASP.NET Core oprogramowanie pośredniczące do propagowania nagłówków HTTP z przychodzącego żądania do wychodzących żądań klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-312">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="f774f-313">Aby użyć propagacji nagłówka:</span><span class="sxs-lookup"><span data-stu-id="f774f-313">To use header propagation:</span></span>

* <span data-ttu-id="f774f-314">Odwołuje się do pakietu [Microsoft. AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) .</span><span class="sxs-lookup"><span data-stu-id="f774f-314">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="f774f-315">Skonfiguruj oprogramowanie pośredniczące i `HttpClient` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f774f-315">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="f774f-316">Klient zawiera skonfigurowane nagłówki w żądaniach wychodzących:</span><span class="sxs-lookup"><span data-stu-id="f774f-316">The client includes the configured headers on outbound requests:</span></span>

  ```C#
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="f774f-317">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f774f-317">Additional resources</span></span>

* [<span data-ttu-id="f774f-318">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="f774f-318">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="f774f-319">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-319">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="f774f-320">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="f774f-320">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="f774f-321">Jak serializować i deserializować kod JSON w programie .NET</span><span class="sxs-lookup"><span data-stu-id="f774f-321">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="f774f-322">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="f774f-322">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="f774f-323"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-323">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="f774f-324">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="f774f-324">It offers the following benefits:</span></span>

* <span data-ttu-id="f774f-325">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="f774f-325">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="f774f-326">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="f774f-326">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="f774f-327">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="f774f-327">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="f774f-328">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-328">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="f774f-329">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="f774f-329">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="f774f-330">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="f774f-330">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="f774f-331">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f774f-331">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="f774f-332">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="f774f-332">Consumption patterns</span></span>

<span data-ttu-id="f774f-333">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f774f-333">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="f774f-334">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="f774f-334">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="f774f-335">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-335">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="f774f-336">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="f774f-336">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="f774f-337">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-337">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="f774f-338">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="f774f-338">None of them are strictly superior to another.</span></span> <span data-ttu-id="f774f-339">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-339">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="f774f-340">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="f774f-340">Basic usage</span></span>

<span data-ttu-id="f774f-341">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f774f-341">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="f774f-342">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f774f-342">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f774f-343">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f774f-343">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="f774f-344">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-344">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="f774f-345">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="f774f-345">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="f774f-346">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-346">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="f774f-347">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-347">Named clients</span></span>

<span data-ttu-id="f774f-348">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="f774f-348">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="f774f-349">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f774f-349">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="f774f-350">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="f774f-350">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="f774f-351">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="f774f-351">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="f774f-352">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f774f-352">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="f774f-353">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-353">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="f774f-354">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="f774f-354">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="f774f-355">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="f774f-355">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="f774f-356">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-356">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="f774f-357">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="f774f-357">Typed clients</span></span>

<span data-ttu-id="f774f-358">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="f774f-358">Typed clients:</span></span>

* <span data-ttu-id="f774f-359">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="f774f-359">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="f774f-360">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-360">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="f774f-361">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-361">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="f774f-362">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="f774f-362">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="f774f-363">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-363">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="f774f-364">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="f774f-364">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f774f-365">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-365">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="f774f-366">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="f774f-366">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="f774f-367">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-367">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="f774f-368">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="f774f-368">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="f774f-369">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="f774f-369">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="f774f-370">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="f774f-370">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="f774f-371">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="f774f-371">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="f774f-372">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-372">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="f774f-373">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-373">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="f774f-374">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="f774f-374">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="f774f-375">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="f774f-375">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="f774f-376">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="f774f-376">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="f774f-377">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-377">Generated clients</span></span>

<span data-ttu-id="f774f-378">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="f774f-378">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="f774f-379">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f774f-379">Refit is a REST library for .NET.</span></span> <span data-ttu-id="f774f-380">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="f774f-380">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="f774f-381">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-381">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="f774f-382">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="f774f-382">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="f774f-383">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="f774f-383">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="f774f-384">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="f774f-384">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="f774f-385">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="f774f-385">Outgoing request middleware</span></span>

<span data-ttu-id="f774f-386">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-386">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f774f-387">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-387">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="f774f-388">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="f774f-388">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="f774f-389">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="f774f-389">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="f774f-390">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f774f-390">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="f774f-391">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="f774f-391">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="f774f-392">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="f774f-392">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="f774f-393">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="f774f-393">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="f774f-394">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="f774f-394">The preceding code defines a basic handler.</span></span> <span data-ttu-id="f774f-395">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="f774f-395">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="f774f-396">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="f774f-396">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="f774f-397">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-397">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="f774f-398">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f774f-398">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="f774f-399">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="f774f-399">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="f774f-400">`IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-400">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="f774f-401">Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="f774f-401">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="f774f-402">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="f774f-402">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="f774f-403">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-403">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="f774f-404">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="f774f-404">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="f774f-405">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="f774f-405">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="f774f-406">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="f774f-406">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="f774f-407">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="f774f-407">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="f774f-408">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-408">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="f774f-409">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="f774f-409">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="f774f-410">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-410">Use Polly-based handlers</span></span>

<span data-ttu-id="f774f-411">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="f774f-411">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="f774f-412">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f774f-412">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="f774f-413">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="f774f-413">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="f774f-414">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-414">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="f774f-415">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="f774f-415">The Polly extensions:</span></span>

* <span data-ttu-id="f774f-416">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-416">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="f774f-417">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="f774f-417">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="f774f-418">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="f774f-418">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="f774f-419">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="f774f-419">Handle transient faults</span></span>

<span data-ttu-id="f774f-420">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="f774f-420">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="f774f-421">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="f774f-421">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="f774f-422">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="f774f-422">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="f774f-423">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f774f-423">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f774f-424">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="f774f-424">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="f774f-425">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="f774f-425">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="f774f-426">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="f774f-426">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="f774f-427">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="f774f-427">Dynamically select policies</span></span>

<span data-ttu-id="f774f-428">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="f774f-428">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="f774f-429">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="f774f-429">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="f774f-430">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="f774f-430">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="f774f-431">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-431">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="f774f-432">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-432">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="f774f-433">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-433">Add multiple Polly handlers</span></span>

<span data-ttu-id="f774f-434">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="f774f-434">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="f774f-435">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-435">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="f774f-436">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="f774f-436">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="f774f-437">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="f774f-437">Failed requests are retried up to three times.</span></span> <span data-ttu-id="f774f-438">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="f774f-438">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="f774f-439">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="f774f-439">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="f774f-440">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="f774f-440">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="f774f-441">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="f774f-441">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="f774f-442">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-442">Add policies from the Polly registry</span></span>

<span data-ttu-id="f774f-443">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="f774f-443">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="f774f-444">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="f774f-444">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="f774f-445">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="f774f-445">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="f774f-446">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="f774f-446">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="f774f-447">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="f774f-447">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="f774f-448">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="f774f-448">HttpClient and lifetime management</span></span>

<span data-ttu-id="f774f-449">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-449">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="f774f-450">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-450">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="f774f-451">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-451">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="f774f-452">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="f774f-452">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="f774f-453">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="f774f-453">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="f774f-454">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-454">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="f774f-455">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="f774f-455">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="f774f-456">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-456">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="f774f-457">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="f774f-457">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="f774f-458">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-458">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="f774f-459">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-459">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="f774f-460">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="f774f-460">Disposal of the client isn't required.</span></span> <span data-ttu-id="f774f-461">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-461">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="f774f-462">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-462">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="f774f-463">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="f774f-463">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="f774f-464">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-464">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="f774f-465">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-465">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="f774f-466">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="f774f-466">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="f774f-467">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="f774f-467">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="f774f-468">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="f774f-468">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="f774f-469">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-469">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="f774f-470">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="f774f-470">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="f774f-471">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-471">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="f774f-472">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="f774f-472">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="f774f-473">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, disposeHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="f774f-473">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="f774f-474">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="f774f-474">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="f774f-475">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-475">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="f774f-476">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="f774f-476">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="f774f-477">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="f774f-477">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="f774f-478">Cookie</span><span class="sxs-lookup"><span data-stu-id="f774f-478">Cookies</span></span>

<span data-ttu-id="f774f-479">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="f774f-479">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="f774f-480">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="f774f-480">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="f774f-481">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="f774f-481">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="f774f-482">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="f774f-482">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="f774f-483">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="f774f-483">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="f774f-484">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="f774f-484">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="f774f-485">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="f774f-485">Logging</span></span>

<span data-ttu-id="f774f-486">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="f774f-486">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="f774f-487">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="f774f-487">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="f774f-488">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="f774f-488">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="f774f-489">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-489">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="f774f-490">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-490">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="f774f-491">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="f774f-491">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="f774f-492">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="f774f-492">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="f774f-493">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="f774f-493">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="f774f-494">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-494">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="f774f-495">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-495">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="f774f-496">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="f774f-496">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="f774f-497">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-497">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="f774f-498">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="f774f-498">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="f774f-499">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f774f-499">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="f774f-500">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="f774f-500">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="f774f-501">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="f774f-501">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="f774f-502">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-502">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="f774f-503">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-503">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="f774f-504">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="f774f-504">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="f774f-505">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-505">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="f774f-506">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="f774f-506">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="f774f-507">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="f774f-507">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="f774f-508">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="f774f-508">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="f774f-509">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="f774f-509">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="f774f-510">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f774f-510">In the following example:</span></span>

* <span data-ttu-id="f774f-511"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="f774f-511"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="f774f-512">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-512">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="f774f-513">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f774f-513">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="f774f-514">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-514">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="f774f-515">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f774f-515">Additional resources</span></span>

* [<span data-ttu-id="f774f-516">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="f774f-516">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="f774f-517">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-517">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="f774f-518">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="f774f-518">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="f774f-519">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="f774f-519">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="f774f-520"><xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-520">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="f774f-521">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="f774f-521">It offers the following benefits:</span></span>

* <span data-ttu-id="f774f-522">Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych.</span><span class="sxs-lookup"><span data-stu-id="f774f-522">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="f774f-523">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="f774f-523">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="f774f-524">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="f774f-524">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="f774f-525">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-525">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="f774f-526">Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.</span><span class="sxs-lookup"><span data-stu-id="f774f-526">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="f774f-527">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="f774f-527">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="f774f-528">[Wyświetl lub pobierz przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f774f-528">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f774f-529">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f774f-529">Prerequisites</span></span>

<span data-ttu-id="f774f-530">Projekty docelowe .NET Framework wymagają instalacji pakietu NuGet [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) .</span><span class="sxs-lookup"><span data-stu-id="f774f-530">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="f774f-531">Projekty przeznaczone dla platformy .NET Core i odwołujące się do [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) zawierają już pakiet `Microsoft.Extensions.Http`.</span><span class="sxs-lookup"><span data-stu-id="f774f-531">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="f774f-532">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="f774f-532">Consumption patterns</span></span>

<span data-ttu-id="f774f-533">Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f774f-533">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="f774f-534">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="f774f-534">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="f774f-535">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-535">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="f774f-536">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="f774f-536">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="f774f-537">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-537">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="f774f-538">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="f774f-538">None of them are strictly superior to another.</span></span> <span data-ttu-id="f774f-539">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-539">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="f774f-540">Podstawowe użycie</span><span class="sxs-lookup"><span data-stu-id="f774f-540">Basic usage</span></span>

<span data-ttu-id="f774f-541">`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f774f-541">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="f774f-542">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f774f-542">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f774f-543">`IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="f774f-543">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="f774f-544">Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-544">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="f774f-545">Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="f774f-545">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="f774f-546">W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-546">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="f774f-547">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-547">Named clients</span></span>

<span data-ttu-id="f774f-548">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="f774f-548">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="f774f-549">Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f774f-549">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="f774f-550">W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="f774f-550">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="f774f-551">Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="f774f-551">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="f774f-552">Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="f774f-552">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="f774f-553">Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-553">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="f774f-554">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="f774f-554">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="f774f-555">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="f774f-555">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="f774f-556">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-556">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="f774f-557">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="f774f-557">Typed clients</span></span>

<span data-ttu-id="f774f-558">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="f774f-558">Typed clients:</span></span>

* <span data-ttu-id="f774f-559">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="f774f-559">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="f774f-560">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-560">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="f774f-561">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-561">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="f774f-562">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="f774f-562">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="f774f-563">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-563">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="f774f-564">Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="f774f-564">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f774f-565">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-565">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="f774f-566">Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="f774f-566">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="f774f-567">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-567">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="f774f-568">Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="f774f-568">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="f774f-569">Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="f774f-569">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="f774f-570">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="f774f-570">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="f774f-571">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="f774f-571">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="f774f-572">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-572">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="f774f-573">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-573">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="f774f-574">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="f774f-574">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="f774f-575">W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="f774f-575">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="f774f-576">Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="f774f-576">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="f774f-577">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="f774f-577">Generated clients</span></span>

<span data-ttu-id="f774f-578">`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="f774f-578">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="f774f-579">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f774f-579">Refit is a REST library for .NET.</span></span> <span data-ttu-id="f774f-580">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="f774f-580">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="f774f-581">Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-581">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="f774f-582">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="f774f-582">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="f774f-583">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="f774f-583">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="f774f-584">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="f774f-584">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="f774f-585">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="f774f-585">Outgoing request middleware</span></span>

<span data-ttu-id="f774f-586">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-586">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f774f-587">`IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-587">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="f774f-588">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="f774f-588">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="f774f-589">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="f774f-589">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="f774f-590">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f774f-590">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="f774f-591">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="f774f-591">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="f774f-592">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="f774f-592">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="f774f-593">Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="f774f-593">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="f774f-594">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="f774f-594">The preceding code defines a basic handler.</span></span> <span data-ttu-id="f774f-595">Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="f774f-595">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="f774f-596">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="f774f-596">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="f774f-597">Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-597">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="f774f-598">To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f774f-598">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="f774f-599">W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI.</span><span class="sxs-lookup"><span data-stu-id="f774f-599">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="f774f-600">Procedura obsługi **musi** być zarejestrowana w programie di jako przejściowa usługa, nigdy nie jest objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="f774f-600">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="f774f-601">Jeśli program obsługi jest zarejestrowany jako usługa objęta zakresem, a wszystkie usługi, od których zależy program obsługi, są jednorazowe:</span><span class="sxs-lookup"><span data-stu-id="f774f-601">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="f774f-602">Usługi programu obsługi mogły zostać zlikwidowane, zanim program obsługi znajdzie się poza zakresem.</span><span class="sxs-lookup"><span data-stu-id="f774f-602">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="f774f-603">Usunięte usługi obsługi powodują niepowodzenie procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-603">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="f774f-604">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując w typ procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-604">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="f774f-605">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="f774f-605">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="f774f-606">Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="f774f-606">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="f774f-607">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="f774f-607">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="f774f-608">Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="f774f-608">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="f774f-609">Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-609">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="f774f-610">Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="f774f-610">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="f774f-611">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-611">Use Polly-based handlers</span></span>

<span data-ttu-id="f774f-612">`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="f774f-612">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="f774f-613">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f774f-613">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="f774f-614">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="f774f-614">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="f774f-615">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-615">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="f774f-616">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="f774f-616">The Polly extensions:</span></span>

* <span data-ttu-id="f774f-617">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-617">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="f774f-618">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="f774f-618">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="f774f-619">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="f774f-619">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="f774f-620">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="f774f-620">Handle transient faults</span></span>

<span data-ttu-id="f774f-621">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="f774f-621">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="f774f-622">Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="f774f-622">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="f774f-623">Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="f774f-623">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="f774f-624">Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f774f-624">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f774f-625">Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="f774f-625">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="f774f-626">W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="f774f-626">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="f774f-627">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="f774f-627">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="f774f-628">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="f774f-628">Dynamically select policies</span></span>

<span data-ttu-id="f774f-629">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="f774f-629">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="f774f-630">Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="f774f-630">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="f774f-631">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="f774f-631">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="f774f-632">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-632">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="f774f-633">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-633">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="f774f-634">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-634">Add multiple Polly handlers</span></span>

<span data-ttu-id="f774f-635">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="f774f-635">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="f774f-636">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-636">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="f774f-637">Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="f774f-637">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="f774f-638">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="f774f-638">Failed requests are retried up to three times.</span></span> <span data-ttu-id="f774f-639">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="f774f-639">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="f774f-640">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="f774f-640">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="f774f-641">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="f774f-641">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="f774f-642">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="f774f-642">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="f774f-643">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-643">Add policies from the Polly registry</span></span>

<span data-ttu-id="f774f-644">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="f774f-644">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="f774f-645">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="f774f-645">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="f774f-646">W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady.</span><span class="sxs-lookup"><span data-stu-id="f774f-646">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="f774f-647">Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="f774f-647">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="f774f-648">Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="f774f-648">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="f774f-649">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="f774f-649">HttpClient and lifetime management</span></span>

<span data-ttu-id="f774f-650">Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-650">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="f774f-651">Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-651">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="f774f-652">Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-652">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="f774f-653">`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="f774f-653">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="f774f-654">Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="f774f-654">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="f774f-655">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-655">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="f774f-656">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="f774f-656">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="f774f-657">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-657">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="f774f-658">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="f774f-658">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="f774f-659">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-659">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="f774f-660">Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-660">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="f774f-661">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="f774f-661">Disposal of the client isn't required.</span></span> <span data-ttu-id="f774f-662">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="f774f-662">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="f774f-663">`IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-663">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="f774f-664">Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="f774f-664">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="f774f-665">Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-665">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="f774f-666">Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="f774f-666">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="f774f-667">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="f774f-667">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="f774f-668">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="f774f-668">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="f774f-669">Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="f774f-669">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="f774f-670">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="f774f-670">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="f774f-671">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="f774f-671">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="f774f-672">Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f774f-672">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="f774f-673">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="f774f-673">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="f774f-674">Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, disposeHandler: false)`, zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="f774f-674">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="f774f-675">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.</span><span class="sxs-lookup"><span data-stu-id="f774f-675">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="f774f-676">`SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-676">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="f774f-677">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="f774f-677">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="f774f-678">`SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="f774f-678">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="f774f-679">Cookie</span><span class="sxs-lookup"><span data-stu-id="f774f-679">Cookies</span></span>

<span data-ttu-id="f774f-680">Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="f774f-680">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="f774f-681">Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="f774f-681">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="f774f-682">W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="f774f-682">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="f774f-683">Wyłączanie obsługi automatycznej plików cookie</span><span class="sxs-lookup"><span data-stu-id="f774f-683">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="f774f-684">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="f774f-684">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="f774f-685">Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:</span><span class="sxs-lookup"><span data-stu-id="f774f-685">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="f774f-686">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="f774f-686">Logging</span></span>

<span data-ttu-id="f774f-687">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="f774f-687">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="f774f-688">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="f774f-688">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="f774f-689">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="f774f-689">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="f774f-690">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-690">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="f774f-691">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-691">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="f774f-692">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="f774f-692">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="f774f-693">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="f774f-693">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="f774f-694">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="f774f-694">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="f774f-695">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="f774f-695">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="f774f-696">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="f774f-696">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="f774f-697">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="f774f-697">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="f774f-698">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="f774f-698">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="f774f-699">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="f774f-699">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="f774f-700">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f774f-700">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="f774f-701">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="f774f-701">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="f774f-702">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="f774f-702">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="f774f-703">Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="f774f-703">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="f774f-704">`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów.</span><span class="sxs-lookup"><span data-stu-id="f774f-704">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="f774f-705">Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="f774f-705">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="f774f-706">Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="f774f-706">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="f774f-707">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="f774f-707">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="f774f-708">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="f774f-708">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="f774f-709">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="f774f-709">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="f774f-710">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="f774f-710">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="f774f-711">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f774f-711">In the following example:</span></span>

* <span data-ttu-id="f774f-712"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="f774f-712"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="f774f-713">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="f774f-713">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="f774f-714">`HttpClient` jest używany do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f774f-714">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="f774f-715">`Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="f774f-715">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="f774f-716">Oprogramowanie pośredniczące propagacji nagłówka</span><span class="sxs-lookup"><span data-stu-id="f774f-716">Header propagation middleware</span></span>

<span data-ttu-id="f774f-717">Propagacja nagłówka to społeczność obsługiwana przez oprogramowanie pośredniczące do propagowania nagłówków HTTP z przychodzącego żądania do wychodzących żądań klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f774f-717">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="f774f-718">Aby użyć propagacji nagłówka:</span><span class="sxs-lookup"><span data-stu-id="f774f-718">To use header propagation:</span></span>

* <span data-ttu-id="f774f-719">Odwołuje się do obsługiwanego przez społeczność portu [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation)pakietu.</span><span class="sxs-lookup"><span data-stu-id="f774f-719">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="f774f-720">ASP.NET Core 3,1 i nowsze obsługuje [Microsoft. AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="f774f-720">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="f774f-721">Skonfiguruj oprogramowanie pośredniczące i `HttpClient` w `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f774f-721">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="f774f-722">Klient zawiera skonfigurowane nagłówki w żądaniach wychodzących:</span><span class="sxs-lookup"><span data-stu-id="f774f-722">The client includes the configured headers on outbound requests:</span></span>

  ```C#
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="f774f-723">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f774f-723">Additional resources</span></span>

* [<span data-ttu-id="f774f-724">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="f774f-724">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="f774f-725">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="f774f-725">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="f774f-726">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="f774f-726">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
