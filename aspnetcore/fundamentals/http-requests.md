---
title: Zainicjuj żądań HTTP
author: stevejgordon
description: Dowiedz się więcej o zarządzaniu logicznej wystąpień HttpClient w ASP.NET Core za pomocą interfejsu IHttpClientFactory.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/22/2018
uid: fundamentals/http-requests
ms.openlocfilehash: e56c7a3ed80cc08103f6178859a1a99f1a5ec068
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327525"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="6b186-103">Zainicjuj żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="6b186-103">Initiate HTTP requests</span></span>

<span data-ttu-id="6b186-104">Przez [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="6b186-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="6b186-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) były rejestrowane i służy do konfigurowania i utworzyć [HttpClient](/dotnet/api/system.net.http.httpclient) wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b186-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="6b186-106">Zapewnia następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="6b186-106">It offers the following benefits:</span></span>

* <span data-ttu-id="6b186-107">Udostępnia centralną lokalizację do nazywania i konfigurowanie logicznej `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="6b186-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="6b186-108">Na przykład klienta "github" można zarejestrować i skonfigurowane do korzystania z usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="6b186-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="6b186-109">Domyślne klienta można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="6b186-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="6b186-110">Określa zasady koncepcji wychodzących oprogramowanie pośredniczące za pośrednictwem Delegowanie obsługi w `HttpClient` i udostępnia rozszerzenia na podstawie Polly przeprowadzać tego oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="6b186-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="6b186-111">Zarządza buforowanie i okresem istnienia bazowy `HttpClientMessageHandler` instancje, aby uniknąć typowe problemy DNS, które występują w przypadku ręcznego zarządzania `HttpClient` okresy istnienia.</span><span class="sxs-lookup"><span data-stu-id="6b186-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="6b186-112">Dodaje obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów tworzone przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="6b186-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="6b186-113">Wzorce użycia</span><span class="sxs-lookup"><span data-stu-id="6b186-113">Consumption patterns</span></span>

<span data-ttu-id="6b186-114">Istnieje kilka sposobów `IHttpClientFactory` mogą być używane w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="6b186-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="6b186-115">Podstawowe sposoby użycia</span><span class="sxs-lookup"><span data-stu-id="6b186-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="6b186-116">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="6b186-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="6b186-117">Typizowany klientów</span><span class="sxs-lookup"><span data-stu-id="6b186-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="6b186-118">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="6b186-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="6b186-119">Żaden z nich nie jest ściśle wyższego poziomu do innego.</span><span class="sxs-lookup"><span data-stu-id="6b186-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="6b186-120">Najlepszym rozwiązaniem jest uzależnione od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b186-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="6b186-121">Podstawowe sposoby użycia</span><span class="sxs-lookup"><span data-stu-id="6b186-121">Basic usage</span></span>

<span data-ttu-id="6b186-122">`IHttpClientFactory` Mogą być rejestrowane przez wywołanie metody `AddHttpClient` — metoda rozszerzenia na `IServiceCollection`w `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="6b186-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="6b186-123">Po zarejestrowaniu kodu może akceptować `IHttpClientFactory` dowolnym usług mogą zostać dodane z [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane).</span><span class="sxs-lookup"><span data-stu-id="6b186-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="6b186-124">`IHttpClientFactory` Może służyć do tworzenia `HttpClient` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="6b186-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="6b186-125">Przy użyciu `IHttpClientFactory` w ten sposób jest doskonałym sposobem Refaktoryzuj istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b186-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="6b186-126">Nie ma to wpływu na sposób `HttpClient` jest używany.</span><span class="sxs-lookup"><span data-stu-id="6b186-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="6b186-127">W miejscach gdzie `HttpClient` obecnie tworzone są wystąpienia, Zastąp te wystąpienia, w wyniku wywołania `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="6b186-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="6b186-128">Nazwane klientów</span><span class="sxs-lookup"><span data-stu-id="6b186-128">Named clients</span></span>

<span data-ttu-id="6b186-129">Jeśli aplikacja wymaga wielu różnych zastosowań `HttpClient`, każdy z innej konfiguracji opcją jest użycie **o nazwie klientów**.</span><span class="sxs-lookup"><span data-stu-id="6b186-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="6b186-130">Konfiguracja nazwane `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6b186-130">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="6b186-131">W powyższym kodzie `AddHttpClient` zostanie wywołany, podając nazwę "github".</span><span class="sxs-lookup"><span data-stu-id="6b186-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="6b186-132">Ten klient ma konfigurację domyślną, niektóre zastosowane&mdash;mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="6b186-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="6b186-133">Zawsze `CreateClient` jest nazywany nowe wystąpienie klasy `HttpClient` jest tworzony i jest wywoływana Akcja konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6b186-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="6b186-134">Użycie nazwanego klienta, można przekazać parametr typu string do `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="6b186-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="6b186-135">Określ nazwę klienta ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="6b186-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="6b186-136">W powyższym kodzie żądania nie trzeba podać nazwę hosta.</span><span class="sxs-lookup"><span data-stu-id="6b186-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="6b186-137">Można go przekazać tylko ścieżki, ponieważ jest używany adres podstawowy skonfigurowaną dla klienta.</span><span class="sxs-lookup"><span data-stu-id="6b186-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="6b186-138">Typizowany klientów</span><span class="sxs-lookup"><span data-stu-id="6b186-138">Typed clients</span></span>

<span data-ttu-id="6b186-139">Klienci typu zawierają takie same możliwości jak nazwanego klientów bez konieczności używania ciągów jako klucze.</span><span class="sxs-lookup"><span data-stu-id="6b186-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="6b186-140">Klient z typowaniem podejście zapewnia IntelliSense i kompilatora pomoc w przypadku uzyskiwania dostępu klientów.</span><span class="sxs-lookup"><span data-stu-id="6b186-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="6b186-141">Udostępniają one jednej lokalizacji do konfigurowania i interakcję z określonego `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6b186-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="6b186-142">Na przykład jeden klient z typowaniem mogą być używane dla punktu końcowego pojedynczego wewnętrznej bazy danych i Hermetyzowanie wszystkich postępowania logiki z tego punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="6b186-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="6b186-143">Inną zaletą jest praca z Podpisane i mogą zostać dodane, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b186-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="6b186-144">Klient z typowaniem akceptuje `HttpClient` parametru w swoich konstruktorach:</span><span class="sxs-lookup"><span data-stu-id="6b186-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="6b186-145">W powyższym kodzie konfiguracji jest przenoszony do typu klienta.</span><span class="sxs-lookup"><span data-stu-id="6b186-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="6b186-146">`HttpClient` Obiektu jest ujawniona jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="6b186-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="6b186-147">Można zdefiniować metody specyficzne dla interfejsu API, które udostępniają `HttpClient` funkcji.</span><span class="sxs-lookup"><span data-stu-id="6b186-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="6b186-148">`GetAspNetDocsIssues` Metody hermetyzuje kod wymagany do kwerendy i analizy najnowsze otwarte problemy z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="6b186-148">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="6b186-149">Aby zarejestrować klienta typu ogólnego `AddHttpClient` — metoda rozszerzenia może być używana w `Startup.ConfigureServices`, określając klasy klient z typowaniem:</span><span class="sxs-lookup"><span data-stu-id="6b186-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="6b186-150">Klient z typowaniem jest zarejestrowany jako przejściowy z Podpisane.</span><span class="sxs-lookup"><span data-stu-id="6b186-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="6b186-151">Klient z typowaniem można wprowadzić i używane bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="6b186-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="6b186-152">Jeśli preferowane, konfiguracji dla typu klienta można określić podczas rejestracji w `Startup.ConfigureServices`, a nie w Konstruktorze typu klienta:</span><span class="sxs-lookup"><span data-stu-id="6b186-152">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="6b186-153">Istnieje możliwość całkowicie Hermetyzowanie `HttpClient` w ramach typu klienta.</span><span class="sxs-lookup"><span data-stu-id="6b186-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="6b186-154">Zamiast ujawnienie go jako właściwość, można podać metody publiczne którego wywołanie `HttpClient` wystąpienie wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="6b186-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="6b186-155">W powyższym kodzie `HttpClient` jest przechowywana jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="6b186-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="6b186-156">Dostęp do połączeń zewnętrznych przechodzi przez `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="6b186-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="6b186-157">Wygenerowany klientów</span><span class="sxs-lookup"><span data-stu-id="6b186-157">Generated clients</span></span>

<span data-ttu-id="6b186-158">`IHttpClientFactory` może być używany w połączeniu z innych bibliotek innych firm takich jak [ponownego ustawienia](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="6b186-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="6b186-159">Refit to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="6b186-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="6b186-160">Konwertuje interfejsów API REST do interfejsów na żywo.</span><span class="sxs-lookup"><span data-stu-id="6b186-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="6b186-161">Implementacja interfejsu jest generowany dynamicznie przez `RestService`za pomocą `HttpClient` dokonanie zewnętrznych HTTP wywołania.</span><span class="sxs-lookup"><span data-stu-id="6b186-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="6b186-162">Interfejs i odpowiedzi są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="6b186-162">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="6b186-163">Klient z typowaniem mogą być dodawane przy użyciu Refit implementacji:</span><span class="sxs-lookup"><span data-stu-id="6b186-163">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="6b186-164">Interfejs zdefiniowanych mogą być używane w przypadku, gdy to konieczne, z implementacją Podpisane i Refit:</span><span class="sxs-lookup"><span data-stu-id="6b186-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="6b186-165">Wychodzące żądania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="6b186-165">Outgoing request middleware</span></span>

<span data-ttu-id="6b186-166">`HttpClient` jest już pojęcie Delegowanie obsługi, które mogą być połączone dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b186-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="6b186-167">`IHttpClientFactory` Ułatwia definiowania metod obsługi do zastosowania dla każdego klienta o nazwie.</span><span class="sxs-lookup"><span data-stu-id="6b186-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="6b186-168">Obsługuje rejestrację i tworzenie łańcucha programów obsługi wielu do tworzenia wychodzącego żądania potoku oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="6b186-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="6b186-169">Każdy z tych programów obsługi jest możliwość wykonywania pracy przed i po żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="6b186-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="6b186-170">Ten wzorzec jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b186-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="6b186-171">Wzorzec zapewnia mechanizm zarządzania kompleksowymi dotyczy wokół żądania HTTP, włączając buforowanie błąd obsługi serializacji i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="6b186-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="6b186-172">Aby utworzyć program obsługi, należy zdefiniować klasy wywodzące się z `DelegatingHandler`.</span><span class="sxs-lookup"><span data-stu-id="6b186-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="6b186-173">Zastąpienie `SendAsync` metodę, aby uruchomić kod przed przekazaniem żądania do następnej procedury obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="6b186-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="6b186-174">Poprzedni kod definiuje podstawowy program obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="6b186-175">Sprawdza, czy nagłówek X-API-KEY została uwzględniona w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="6b186-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="6b186-176">Jeśli brakuje nagłówka go uniknąć wywołanie HTTP i odpowiednią odpowiedź zwrócona.</span><span class="sxs-lookup"><span data-stu-id="6b186-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="6b186-177">Podczas rejestracji, można dodać do konfiguracji obsługi co najmniej jeden `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="6b186-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="6b186-178">To zadanie jest realizowane za pośrednictwem metody rozszerzenia na `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6b186-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="6b186-179">W powyższym kodzie `ValidateHeaderHandler` został zarejestrowany za pomocą Podpisane.</span><span class="sxs-lookup"><span data-stu-id="6b186-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="6b186-180">Program obsługi **musi** rejestrowana w Podpisane jako przejściowy.</span><span class="sxs-lookup"><span data-stu-id="6b186-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="6b186-181">Po zarejestrowaniu `AddHttpMessageHandler` można wywołać, przekazując typ obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="6b186-182">Procedury obsługi wielu może być zarejestrowany w kolejności ich powinien zostać wykonany.</span><span class="sxs-lookup"><span data-stu-id="6b186-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="6b186-183">Każdy program obsługi jest zawijana obsługi dalej do ostatecznego `HttpClientHandler` wykonuje żądanie:</span><span class="sxs-lookup"><span data-stu-id="6b186-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="6b186-184">Użyć obsługi na podstawie Polly</span><span class="sxs-lookup"><span data-stu-id="6b186-184">Use Polly-based handlers</span></span>

<span data-ttu-id="6b186-185">`IHttpClientFactory` integruje się z popularnych biblioteki innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="6b186-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="6b186-186">Polly to kompleksowe odporności i przejściowych biblioteki obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="6b186-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="6b186-187">Umożliwia deweloperom express zasad, na przykład ponów próbę, wyłącznika limitu czasu, izolacji grodziowego i rezerwowe w sposób fluent i bezpieczne wątkowo.</span><span class="sxs-lookup"><span data-stu-id="6b186-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="6b186-188">Metody rozszerzenia znajdują się na korzystanie z zasad Polly z skonfigurowane `HttpClient` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="6b186-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="6b186-189">Rozszerzenia Polly są dostępne w [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6b186-189">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="6b186-190">Ten pakiet nie jest zawarty w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6b186-190">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="6b186-191">Aby korzystać z rozszerzeń, jawnego `<PackageReference />` powinien być dołączony do projektu.</span><span class="sxs-lookup"><span data-stu-id="6b186-191">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="6b186-192">Po przywróceniu tego pakietu, metody rozszerzenia są dostępne do obsługi dodawania obsługi na podstawie Polly do klientów.</span><span class="sxs-lookup"><span data-stu-id="6b186-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="6b186-193">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="6b186-193">Handle transient faults</span></span>

<span data-ttu-id="6b186-194">Najbardziej typowych błędów, oczekiwanych może wystąpić, gdy zewnętrznych połączeń HTTP będzie mieć charakter przejściowy.</span><span class="sxs-lookup"><span data-stu-id="6b186-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="6b186-195">Wywołana metoda rozszerzenia wygodne `AddTransientHttpErrorPolicy` jest włączone, dzięki czemu zasady można definiować w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="6b186-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="6b186-196">Zasady skonfigurowane z ta dojścia metody rozszerzenia `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="6b186-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="6b186-197">`AddTransientHttpErrorPolicy` Rozszerzenia mogą być używane w `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6b186-197">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6b186-198">Rozszerzenie zapewnia dostęp do `PolicyBuilder` skonfigurowany do obsługi błędów reprezentujący możliwych błędów przejściowych obiektu:</span><span class="sxs-lookup"><span data-stu-id="6b186-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="6b186-199">W powyższym kodzie `WaitAndRetryAsync` zdefiniowane zasady.</span><span class="sxs-lookup"><span data-stu-id="6b186-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="6b186-200">Żądań zakończonych niepowodzeniem są zwalniane maksymalnie trzy razy z opóźnieniem 600 ms między próbami.</span><span class="sxs-lookup"><span data-stu-id="6b186-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="6b186-201">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="6b186-201">Dynamically select policies</span></span>

<span data-ttu-id="6b186-202">Metody rozszerzenia dodatkowe istnieje, które mogą służyć do dodania na podstawie Polly programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="6b186-203">Jedno z rozszerzeń jest `AddPolicyHandler`, który ma kilka przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="6b186-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="6b186-204">Przeciążeniami umożliwia żądania poddanych podczas definiowania zasad do zastosowania:</span><span class="sxs-lookup"><span data-stu-id="6b186-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="6b186-205">W poprzednim kodzie Jeśli żądania wychodzącego jest GET, stosowana jest 10 sekund limitu czasu.</span><span class="sxs-lookup"><span data-stu-id="6b186-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="6b186-206">W innej metody HTTP używany jest limit czasu 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="6b186-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="6b186-207">Dodawanie wielu obsług Polly</span><span class="sxs-lookup"><span data-stu-id="6b186-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="6b186-208">Jest często stosowanym rozwiązaniem zagnieździć Polly zasady umożliwiają korzystanie z funkcji rozszerzonej:</span><span class="sxs-lookup"><span data-stu-id="6b186-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="6b186-209">W powyższym przykładzie są dodawane dwóch metod obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="6b186-210">Używa pierwszego `AddTransientHttpErrorPolicy` rozszerzenia, aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="6b186-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="6b186-211">Żądań zakończonych niepowodzeniem są zwalniane maksymalnie trzy razy.</span><span class="sxs-lookup"><span data-stu-id="6b186-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="6b186-212">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje wyłącznika zasady.</span><span class="sxs-lookup"><span data-stu-id="6b186-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="6b186-213">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, ewentualnych pięć nieudanych prób sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="6b186-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="6b186-214">Zasady wyłącznika nie jest obiektem stanowym.</span><span class="sxs-lookup"><span data-stu-id="6b186-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="6b186-215">Wszystkie wywołania za pośrednictwem tego klienta udostępnianie tego samego stanu obwodu.</span><span class="sxs-lookup"><span data-stu-id="6b186-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="6b186-216">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="6b186-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="6b186-217">Jest to metoda regularnie używane zasady zarządzania je raz zdefiniować i zarejestrować je za pomocą `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="6b186-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="6b186-218">Metody rozszerzenia jest dostępne, dzięki czemu program obsługi ma zostać dodana przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="6b186-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="6b186-219">W powyższym kodzie PolicyRegistry jest dodawany do `ServiceCollection` i dwie zasady są zarejestrowane z nim.</span><span class="sxs-lookup"><span data-stu-id="6b186-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="6b186-220">Aby można było używać z rejestru, zasady `AddPolicyHandlerFromRegistry` metoda jest używana nazwa zasady do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="6b186-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="6b186-221">Więcej informacji o `IHttpClientFactory` i Polly integracji można znaleźć w [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="6b186-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="6b186-222">Zarządzanie HttpClient i okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="6b186-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="6b186-223">Zawsze `CreateClient` jest wywoływana na `IHttpClientFactory`, nowe wystąpienie klasy `HttpClient` jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="6b186-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="6b186-224">Będzie `HttpMessageHandler` na nazwanym klienta.</span><span class="sxs-lookup"><span data-stu-id="6b186-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="6b186-225">`IHttpClientFactory` pula będzie `HttpMessageHandler` wystąpienia utworzone przez fabrykę zmniejszenie zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="6b186-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="6b186-226">A `HttpMessageHandler` wystąpienia może nastąpić z puli, podczas tworzenia nowego `HttpClient` wystąpienia, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="6b186-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="6b186-227">Buforowanie obsługi jest pożądane, ponieważ każdy program obsługi zwykle zarządza własną podstawowej połączenia HTTP; Tworzenie obsługi więcej niż jest to konieczne może spowodować opóźnienia połączenia.</span><span class="sxs-lookup"><span data-stu-id="6b186-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="6b186-228">Niektóre programy obsługi również nie zamykaj połączenia przez czas nieokreślony, co może uniemożliwić obsługi reagowanie na zmiany DNS.</span><span class="sxs-lookup"><span data-stu-id="6b186-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="6b186-229">Domyślny okres istnienia obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="6b186-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="6b186-230">Wartość domyślna może zostać zastąpiona przez poszczególnych klientów o nazwie.</span><span class="sxs-lookup"><span data-stu-id="6b186-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="6b186-231">Aby ją zastąpić, należy wywołać `SetHandlerLifetime` na `IHttpClientBuilder` zwróceniu podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="6b186-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="6b186-232">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="6b186-232">Logging</span></span>

<span data-ttu-id="6b186-233">Klienci utworzony za pośrednictwem `IHttpClientFactory` rejestrowania komunikatów dziennika dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="6b186-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="6b186-234">Należy włączyć na poziomie odpowiednie informacje w konfigurację rejestrowania, zobacz domyślnych wiadomości dziennika.</span><span class="sxs-lookup"><span data-stu-id="6b186-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="6b186-235">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądania jest dostępny tylko na poziom śledzenia.</span><span class="sxs-lookup"><span data-stu-id="6b186-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="6b186-236">Kategoria dziennika używane dla każdego klienta zawiera nazwę klienta.</span><span class="sxs-lookup"><span data-stu-id="6b186-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="6b186-237">Klient o nazwie "MyNamedClient", na przykład rejestruje komunikaty z kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="6b186-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="6b186-238">Komunikaty z sufiksem "LogicalHandler" odbywa się na zewnątrz Potok żądań obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="6b186-239">Na żądanie komunikaty są rejestrowane przed innych programów obsługi w potoku zostały przetworzone go.</span><span class="sxs-lookup"><span data-stu-id="6b186-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="6b186-240">W odpowiedzi komunikaty są rejestrowane po wszelkich innych programów obsługi potoku otrzymali odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6b186-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="6b186-241">Rejestrowanie występuje także wewnątrz Potok żądań obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="6b186-242">W przypadku przykład "MyNamedClient" te komunikaty są rejestrowane przed kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="6b186-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="6b186-243">Dla żądania występuje po wykonaniu wszystkich innych programów obsługi i bezpośrednio przed wysłaniu żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="6b186-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="6b186-244">W odpowiedzi rejestrowanie obejmuje stanu odpowiedzi przed przekazaniem za pośrednictwem potoku obsługi.</span><span class="sxs-lookup"><span data-stu-id="6b186-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="6b186-245">Włączanie rejestrowania na zewnątrz i wewnątrz potoku umożliwia kontroli zmiany wprowadzone przez innych programów obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="6b186-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="6b186-246">Może to obejmować zmiany do żądania nagłówków, na przykład lub kod stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6b186-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="6b186-247">Łącznie z nazwą klienta w kategorii dziennika umożliwia dziennika filtrowania dla klientów o nazwie określonej w miarę potrzeby.</span><span class="sxs-lookup"><span data-stu-id="6b186-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="6b186-248">Klasa HttpMessageHandler skonfigurować</span><span class="sxs-lookup"><span data-stu-id="6b186-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="6b186-249">Może być konieczne konfiguracją wewnętrzny `HttpMessageHandler` używany przez klienta.</span><span class="sxs-lookup"><span data-stu-id="6b186-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="6b186-250">`IHttpClientBuilder` Jest zwracany, jeśli dodanie o nazwie lub wpisany klientów.</span><span class="sxs-lookup"><span data-stu-id="6b186-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="6b186-251">`ConfigurePrimaryHttpMessageHandler` — Metoda rozszerzenia może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="6b186-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="6b186-252">Delegat jest używany do tworzenia i konfigurowania serwera podstawowego `HttpMessageHandler` używany przez klienta:</span><span class="sxs-lookup"><span data-stu-id="6b186-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
