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
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT)i [Kirka Larkin](https://github.com/serpent5)

<xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji. `IHttpClientFactory` oferuje następujące korzyści:

* Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych. Na przykład klient o nazwie *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/). Domyślny klient można zarejestrować na potrzeby ogólnego dostępu.
* Kodyfikacja koncepcji wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient`. Udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, które umożliwiają delegowanie programów obsługi w `HttpClient`.
* Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`. Automatyczne zarządzanie pozwala uniknąć typowych problemów z usługą DNS (Domain Name System) występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.
* Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).

Przykładowy kod w tej wersji tematu używa <xref:System.Text.Json> do deserializacji zawartości JSON zwróconej w odpowiedziach HTTP. Aby uzyskać przykłady, które używają `Json.NET` i `ReadAsAsync<T>`, użyj selektora wersji, aby wybrać wersję 2. x tego tematu.

## <a name="consumption-patterns"></a>Wzorce zużycia

Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:

* [Podstawowe użycie](#basic-usage)
* [Nazwani klienci](#named-clients)
* [Wpisane komputery klienckie](#typed-clients)
* [Wygenerowane klienci](#generated-clients)

Najlepsze podejście zależy od wymagań aplikacji.

### <a name="basic-usage"></a>Podstawowe użycie

`IHttpClientFactory` można zarejestrować, wywołując `AddHttpClient`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

`IHttpClientFactory` można żądać przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection). Poniższy kod używa `IHttpClientFactory`, aby utworzyć wystąpienie `HttpClient`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Użycie `IHttpClientFactory` podobnej do powyższego przykładu jest dobrym sposobem refaktoryzacji istniejącej aplikacji. Nie ma to wpływu na sposób użycia `HttpClient`. W miejscach, w których `HttpClient` wystąpienia są tworzone w istniejącej aplikacji, Zastąp te wystąpienia wywołaniami <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Nazwani klienci

Nazwani klienci są dobrym wyborem w przypadku:

* Aplikacja wymaga wielu odrębnych użycia `HttpClient`.
* Wiele `HttpClient`s ma inną konfigurację.

Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

W powyższym kodzie klient ma skonfigurowany program:

* Adres podstawowy `https://api.github.com/`.
* Dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.

#### <a name="createclient"></a>Klient

Za każdym razem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> jest wywoływana:

* Zostanie utworzone nowe wystąpienie `HttpClient`.
* Akcja konfiguracji jest wywoływana.

Aby utworzyć nazwanego klienta, Przekaż jego nazwę do `CreateClient`:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

W powyższym kodzie żądanie nie musi określać nazwy hosta. Kod może przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.

### <a name="typed-clients"></a>Wpisane komputery klienckie

Wpisane komputery klienckie:

* Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.
* Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.
* Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`. Na przykład może zostać użyty pojedynczy klient z określonym typem:
  * Dla pojedynczego punktu końcowego zaplecza.
  * Do hermetyzacji wszystkich logiki związanych z punktem końcowym.
* Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.

Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

W powyższym kodzie:

* Konfiguracja zostanie przeniesiona do określonego klienta.
* Obiekt `HttpClient` jest udostępniany jako właściwość publiczna.

Można tworzyć metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcje. Na przykład Metoda `GetAspNetDocsIssues` hermetyzuje kod w celu pobrania otwartych problemów.

Następujący kod wywołuje <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> w `Startup.ConfigureServices` w celu zarejestrowania klasy klienta z określonym typem:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Typ klienta jest rejestrowany jako przejściowy z DI. Określony klient może zostać dodany i wykorzystany bezpośrednio:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Konfigurację dla określonego klienta można określić podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

`HttpClient` można hermetyzować w obrębie klienta z określonym typem. Zamiast uwidaczniać ją jako właściwość, zdefiniuj metodę, która wywołuje wystąpienie `HttpClient` wewnętrznie:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

W poprzednim kodzie `HttpClient` jest przechowywany w prywatnym polu. Dostęp do `HttpClient` odbywa się za pomocą metody `GetRepos` publicznej.

### <a name="generated-clients"></a>Wygenerowane klienci

`IHttpClientFactory` można używać w połączeniu z bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit). REFIT to biblioteka REST dla platformy .NET. Konwertuje interfejsy API REST na interfejsy na żywo. Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.

Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:

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

Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:

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

Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:

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

## <a name="outgoing-request-middleware"></a>Oprogramowanie pośredniczące żądania wychodzące

`HttpClient` ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP. `IHttpClientFactory`:

* Upraszcza definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.
* Obsługuje rejestrację i łańcuch wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego. Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym. Ten wzorzec:

  * Jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.
  * Program udostępnia mechanizm zarządzania problemami z obcinaniem między żądaniami HTTP, takimi jak:

    * buforowanie
    * obsługa błędów
    * serializacja
    * rejestrowanie

Aby utworzyć program obsługi delegowania:

* Pochodny od <xref:System.Net.Http.DelegatingHandler>.
* Zastąp <xref:System.Net.Http.DelegatingHandler.SendAsync*>. Wykonaj kod przed przekazaniem żądania do następnego programu obsługi w potoku:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Poprzedni kod sprawdza, czy nagłówek `X-API-KEY` jest w żądaniu. Jeśli brakuje `X-API-KEY`, zwracany jest <xref:System.Net.HttpStatusCode.BadRequest>.

Do konfiguracji `HttpClient` można dodać więcej niż jeden program obsługi z <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI. `IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi. Programy obsługi mogą zależeć od usług dowolnego zakresu. Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.

Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.

Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane. Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:

* Przekaż dane do procedury obsługi przy użyciu [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).
* Użyj <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>, aby uzyskać dostęp do bieżącego żądania.
* Utwórz niestandardowy obiekt magazynu <xref:System.Threading.AsyncLocal`1>, aby przekazać dane.

## <a name="use-polly-based-handlers"></a>Korzystanie z programów obsługi opartych na Polly

`IHttpClientFactory` integruje się z biblioteką innych firm [Polly](https://github.com/App-vNext/Polly). Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET. Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.

Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`. Rozszerzenia Polly obsługują dodawanie programów obsługi opartych na Polly do klientów. Polly wymaga pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .

### <a name="handle-transient-faults"></a>Obsługa błędów przejściowych

Błędy są zwykle wykonywane, gdy zewnętrzne wywołania HTTP są przejściowe. <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych. Zasady skonfigurowane z `AddTransientHttpErrorPolicy` obsługują następujące odpowiedzi:

* <xref:System.Net.Http.HttpRequestException>
* HTTP 5xx
* HTTP 408

`AddTransientHttpErrorPolicy` zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowany do obsługi błędów reprezentujących możliwy błąd przejściowy:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`. Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.

### <a name="dynamically-select-policies"></a>Dynamiczne Wybieranie zasad

Metody rozszerzające umożliwiają dodawanie programów obsługi opartych na Polly, na przykład <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>. Następujące `AddPolicyHandler` Przeciążenie sprawdza żądanie, aby zdecydować, które zasady zastosować:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu. Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.

### <a name="add-multiple-polly-handlers"></a>Dodaj wiele programów obsługi Polly

Jest to typowy sposób zagnieżdżania zasad Pollyymi:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

W poprzednim przykładzie:

* Dodawane są dwa procedury obsługi.
* Pierwsza procedura obsługi używa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, aby dodać zasady ponawiania. Nieudane żądania są ponawiane do trzech razy.
* Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika. Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli 5 nieudanych prób wystąpi sekwencyjnie. Zasady wyłącznika są stanowe. Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.

### <a name="add-policies-from-the-polly-registry"></a>Dodawanie zasad z rejestru Polly

Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`.

W poniższym kodzie:

* Dodawane są zasady "regularne" i "długie".
* <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> dodaje zasady "regularne" i "długie" z rejestru.

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

Aby uzyskać więcej informacji na temat integracji `IHttpClientFactory` i Polly, zobacz [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient i zarządzanie okresem istnienia

Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`. <xref:System.Net.Http.HttpMessageHandler> jest tworzony na Nazwa klienta. Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.

`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów. Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.

Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP. Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń. Niektóre programy obsługi powodują również, że połączenia są otwierane w nieskończoność, co może uniemożliwić oddziałanie programu obsługi w systemie DNS (Domain Name System).

Domyślny okres istnienia programu obsługi wynosi dwie minuty. Wartość domyślną można przesłonić na podstawie nazwy klienta:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które **nie** wymagają usuwania. Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`.

Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`. Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.

### <a name="alternatives-to-ihttpclientfactory"></a>Alternatywy do IHttpClientFactory

Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:

* Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.
* Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.

Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.

- Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.
- Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.
- Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.

Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.

- `SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`. To udostępnianie uniemożliwia wyczerpanie gniazda.
- `SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.

### <a name="cookies"></a>Cookie

Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane. Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod. W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:

 - Wyłączanie obsługi automatycznej plików cookie
 - Unikanie `IHttpClientFactory`

Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Rejestrowanie

Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań. Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika. Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.

Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu. Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler". Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań. Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku. W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.

Rejestrowanie odbywa się również w potoku obsługi żądania. W przykładzie *MyNamedClient* te komunikaty są rejestrowane z kategorią dziennika "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler". W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania. W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.

Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku. Może to obejmować zmiany w nagłówkach żądania lub w kodzie stanu odpowiedzi.

Uwzględnienie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów.

## <a name="configure-the-httpmessagehandler"></a>Konfigurowanie HttpMessageHandler

Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.

`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów. Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata. Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Korzystanie z IHttpClientFactory w aplikacji konsolowej

W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:

* [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

W poniższym przykładzie:

* <xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .
* `MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`. `HttpClient` jest używany do pobierania strony sieci Web.
* `Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementowanie wzorca wyłącznika](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)

<xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji. Oferuje następujące korzyści:

* Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych. Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/). Domyślny klient można zarejestrować do innych celów.
* Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.
* Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.
* Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="consumption-patterns"></a>Wzorce zużycia

Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:

* [Podstawowe użycie](#basic-usage)
* [Nazwani klienci](#named-clients)
* [Wpisane komputery klienckie](#typed-clients)
* [Wygenerowane klienci](#generated-clients)

Żadna z nich nie jest ściśle wyższa od siebie. Najlepsze podejście zależy od ograniczeń aplikacji.

### <a name="basic-usage"></a>Podstawowe użycie

`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection). `IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji. Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany. W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Nazwani klienci

Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**. Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*. Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.

Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.

Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`. Określ nazwę klienta, który ma zostać utworzony:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

W powyższym kodzie żądanie nie musi określać nazwy hosta. Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.

### <a name="typed-clients"></a>Wpisane komputery klienckie

Wpisane komputery klienckie:

* Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.
* Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.
* Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`. Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.
* Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.

Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta. Obiekt `HttpClient` jest udostępniany jako właściwość publiczna. Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`. Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.

Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Typ klienta jest rejestrowany jako przejściowy z DI. Określony klient może zostać dodany i wykorzystany bezpośrednio:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta. Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne. Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.

### <a name="generated-clients"></a>Wygenerowane klienci

`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit). REFIT to biblioteka REST dla platformy .NET. Konwertuje interfejsy API REST na interfejsy na żywo. Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.

Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:

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

Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:

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

Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:

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

## <a name="outgoing-request-middleware"></a>Oprogramowanie pośredniczące żądania wychodzące

`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP. `IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta. Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego. Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym. Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core. Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.

Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>. Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Poprzedni kod definiuje procedurę obsługi podstawowej. Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu. Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.

Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi. To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI. `IHttpClientFactory` tworzy oddzielny zakres DI dla każdej procedury obsługi. Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu. Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.

Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.

Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane. Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:

* Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.
* Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.
* Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.

## <a name="use-polly-based-handlers"></a>Korzystanie z programów obsługi opartych na Polly

`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly). Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET. Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.

Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`. Rozszerzenia Polly:

* Obsługa dodawania programów obsługi opartych na Pollych do klientów.
* Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) . Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.

### <a name="handle-transient-faults"></a>Obsługa błędów przejściowych

Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe. Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych. Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.

Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`. Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`. Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.

### <a name="dynamically-select-policies"></a>Dynamiczne Wybieranie zasad

Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly. Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń. Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu. Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.

### <a name="add-multiple-polly-handlers"></a>Dodaj wiele programów obsługi Polly

Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

W poprzednim przykładzie dodawane są dwa programy obsługi. Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania. Nieudane żądania są ponawiane do trzech razy. Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika. Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie. Zasady wyłącznika są stanowe. Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.

### <a name="add-policies-from-the-polly-registry"></a>Dodawanie zasad z rejestru Polly

Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`. Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady. Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.

Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient i zarządzanie okresem istnienia

Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`. Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta. Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.

`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów. Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.

Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP. Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń. Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.

Domyślny okres istnienia programu obsługi wynosi dwie minuty. Wartość domyślną można przesłonić na podstawie nazwy klienta. Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

Usuwanie klienta nie jest wymagane. Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`. Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.

Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`. Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.

### <a name="alternatives-to-ihttpclientfactory"></a>Alternatywy do IHttpClientFactory

Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:

* Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.
* Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.

Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.

- Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.
- Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.
- Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.

Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.

- `SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`. To udostępnianie uniemożliwia wyczerpanie gniazda.
- `SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.

### <a name="cookies"></a>Cookie

Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane. Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod. W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:

 - Wyłączanie obsługi automatycznej plików cookie
 - Unikanie `IHttpClientFactory`

Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Rejestrowanie

Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań. Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika. Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.

Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu. Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań. Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku. W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.

Rejestrowanie odbywa się również w potoku obsługi żądania. W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci. W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.

Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku. Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.

Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.

## <a name="configure-the-httpmessagehandler"></a>Konfigurowanie HttpMessageHandler

Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.

`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów. Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata. Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Korzystanie z IHttpClientFactory w aplikacji konsolowej

W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:

* [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

W poniższym przykładzie:

* <xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .
* `MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`. `HttpClient` jest używany do pobierania strony sieci Web.
* `Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementowanie wzorca wyłącznika](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)

<xref:System.Net.Http.IHttpClientFactory> może być zarejestrowany i służy do konfigurowania i tworzenia wystąpień <xref:System.Net.Http.HttpClient> w aplikacji. Oferuje następujące korzyści:

* Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych. Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/). Domyślny klient można zarejestrować do innych celów.
* Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.
* Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.
* Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

Projekty docelowe .NET Framework wymagają instalacji pakietu NuGet [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) . Projekty przeznaczone dla platformy .NET Core i odwołujące się do [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) zawierają już pakiet `Microsoft.Extensions.Http`.

## <a name="consumption-patterns"></a>Wzorce zużycia

Istnieje kilka sposobów, `IHttpClientFactory` mogą być używane w aplikacji:

* [Podstawowe użycie](#basic-usage)
* [Nazwani klienci](#named-clients)
* [Wpisane komputery klienckie](#typed-clients)
* [Wygenerowane klienci](#generated-clients)

Żadna z nich nie jest ściśle wyższa od siebie. Najlepsze podejście zależy od ograniczeń aplikacji.

### <a name="basic-usage"></a>Podstawowe użycie

`IHttpClientFactory` można zarejestrować, wywołując metodę rozszerzenia `AddHttpClient` na `IServiceCollection`wewnątrz metody `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi wszędzie można wstrzyknąć przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection). `IHttpClientFactory` można użyć do utworzenia wystąpienia `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Używanie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji. Nie ma to wpływu na sposób, w jaki `HttpClient` jest używany. W miejscach, w których `HttpClient` wystąpienia są obecnie tworzone, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Nazwani klienci

Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient`, z których każda ma inną konfigurację, opcja jest używana **nazwanych klientów**. Konfigurację dla nazwanego `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

W poprzednim kodzie jest wywoływana `AddHttpClient`, dostarczając nazwę *GitHub*. Ten klient ma pewną konfigurację domyślną&mdash;a mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.

Za każdym razem, gdy `CreateClient` jest wywoływana, tworzone jest nowe wystąpienie `HttpClient` i akcja konfiguracji jest wywoływana.

Aby korzystać z nazwanego klienta, parametr ciągu można przesłać do `CreateClient`. Określ nazwę klienta, który ma zostać utworzony:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

W powyższym kodzie żądanie nie musi określać nazwy hosta. Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.

### <a name="typed-clients"></a>Wpisane komputery klienckie

Wpisane komputery klienckie:

* Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.
* Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.
* Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient`. Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.
* Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.

Klient z określonym typem akceptuje parametr `HttpClient` w jego konstruktorze:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta. Obiekt `HttpClient` jest udostępniany jako właściwość publiczna. Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają funkcje `HttpClient`. Metoda `GetAspNetDocsIssues` hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.

Aby zarejestrować klienta z określonym typem, Metoda rozszerzenia generycznego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> może być używana w ramach `Startup.ConfigureServices`, określając klasę klienta z określonym typem:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Typ klienta jest rejestrowany jako przejściowy z DI. Określony klient może zostać dodany i wykorzystany bezpośrednio:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices`, a nie w konstruktorze określonego klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta. Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują wystąpienie `HttpClient` wewnętrznie.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne. Wszyscy dostęp do wykonywania wywołań zewnętrznych odbywają się za pomocą metody `GetRepos`.

### <a name="generated-clients"></a>Wygenerowane klienci

`IHttpClientFactory` można używać w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit). REFIT to biblioteka REST dla platformy .NET. Konwertuje interfejsy API REST na interfejsy na żywo. Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient`, aby nawiązywać zewnętrzne wywołania HTTP.

Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:

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

Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:

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

Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:

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

## <a name="outgoing-request-middleware"></a>Oprogramowanie pośredniczące żądania wychodzące

`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP. `IHttpClientFactory` ułatwia zdefiniowanie programów obsługi dla każdego nazwanego klienta. Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego. Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym. Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core. Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.

Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną <xref:System.Net.Http.DelegatingHandler>. Zastąp metodę `SendAsync`, aby wykonać kod przed przekazaniem żądania do następnego programu obsługi w potoku:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Poprzedni kod definiuje procedurę obsługi podstawowej. Sprawdza, czy nagłówek `X-API-KEY` został uwzględniony w żądaniu. Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.

Podczas rejestracji do konfiguracji `HttpClient`można dodać co najmniej jeden program obsługi. To zadanie jest realizowane za pośrednictwem metod rozszerzających <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

W poprzednim kodzie `ValidateHeaderHandler` jest rejestrowany przy użyciu DI. Procedura obsługi **musi** być zarejestrowana w programie di jako przejściowa usługa, nigdy nie jest objęta zakresem. Jeśli program obsługi jest zarejestrowany jako usługa objęta zakresem, a wszystkie usługi, od których zależy program obsługi, są jednorazowe:

* Usługi programu obsługi mogły zostać zlikwidowane, zanim program obsługi znajdzie się poza zakresem.
* Usunięte usługi obsługi powodują niepowodzenie procedury obsługi.

Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując w typ procedury obsługi.

Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane. Każdy program obsługi otacza następną procedurę obsługi do momentu, w którym `HttpClientHandler` końcowy wykonuje żądanie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:

* Przekaż dane do procedury obsługi przy użyciu `HttpRequestMessage.Properties`.
* Użyj `IHttpContextAccessor`, aby uzyskać dostęp do bieżącego żądania.
* Utwórz niestandardowy obiekt magazynu `AsyncLocal`, aby przekazać dane.

## <a name="use-polly-based-handlers"></a>Korzystanie z programów obsługi opartych na Polly

`IHttpClientFactory` integruje się ze popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly). Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET. Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.

Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi wystąpieniami `HttpClient`. Rozszerzenia Polly:

* Obsługa dodawania programów obsługi opartych na Pollych do klientów.
* Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) . Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.

### <a name="handle-transient-faults"></a>Obsługa błędów przejściowych

Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe. Dołączono wygodną metodę rozszerzającą o nazwie `AddTransientHttpErrorPolicy`, która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych. Zasady skonfigurowane przy użyciu tej metody rozszerzenia obsługują `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.

Rozszerzenia `AddTransientHttpErrorPolicy` można używać w programie `Startup.ConfigureServices`. Rozszerzenie zapewnia dostęp do obiektu `PolicyBuilder` skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

W poprzednim kodzie zdefiniowane są zasady `WaitAndRetryAsync`. Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.

### <a name="dynamically-select-policies"></a>Dynamiczne Wybieranie zasad

Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly. Jedno takie rozszerzenie ma `AddPolicyHandler`, które ma wiele przeciążeń. Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu. Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.

### <a name="add-multiple-polly-handlers"></a>Dodaj wiele programów obsługi Polly

Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

W poprzednim przykładzie dodawane są dwa programy obsługi. Najpierw używa rozszerzenia `AddTransientHttpErrorPolicy`, aby dodać zasady ponawiania. Nieudane żądania są ponawiane do trzech razy. Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika. Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie. Zasady wyłącznika są stanowe. Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.

### <a name="add-policies-from-the-polly-registry"></a>Dodawanie zasad z rejestru Polly

Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry`. Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

W poprzednim kodzie, podczas dodawania `PolicyRegistry` do `ServiceCollection`zostanie zarejestrowane dwie zasady. Aby użyć zasad z rejestru, używana jest metoda `AddPolicyHandlerFromRegistry`, przekazując nazwę zasad do zastosowania.

Więcej informacji na temat integracji `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>HttpClient i zarządzanie okresem istnienia

Nowe wystąpienie `HttpClient` jest zwracane za każdym razem, gdy `CreateClient` jest wywoływana w `IHttpClientFactory`. Istnieje <xref:System.Net.Http.HttpMessageHandler> na nazwę klienta. Fabryka zarządza okresami istnienia wystąpień `HttpMessageHandler`.

`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabrykę w celu zmniejszenia zużycia zasobów. Wystąpienie `HttpMessageHandler` może być ponownie używane z puli podczas tworzenia nowego wystąpienia `HttpClient`, jeśli jego okres istnienia nie wygasł.

Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP. Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń. Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.

Domyślny okres istnienia programu obsługi wynosi dwie minuty. Wartość domyślną można przesłonić na podstawie nazwy klienta. Aby zastąpić ten element, wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder`, który jest zwracany podczas tworzenia klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

Usuwanie klienta nie jest wymagane. Likwidacja anuluje żądania wychodzące i gwarantuje, że danego wystąpienia `HttpClient` nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` śledzi i usuwa zasoby używane przez wystąpienia `HttpClient`. Wystąpienia `HttpClient` mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.

Utrzymywanie pojedynczej `HttpClient` wystąpienia przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory`. Ten wzorzec będzie niepotrzebny po przeprowadzeniu migracji do `IHttpClientFactory`.

### <a name="alternatives-to-ihttpclientfactory"></a>Alternatywy do IHttpClientFactory

Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:

* Problemy z wyczerpaniem zasobów przez pule `HttpMessageHandler` wystąpieniami.
* Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpień w regularnych odstępach czasu.

Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia o długim czasie istnienia <xref:System.Net.Http.SocketsHttpHandler>.

- Utwórz wystąpienie `SocketsHttpHandler`, gdy aplikacja zostanie uruchomiona i będzie używać jej na potrzeby życia aplikacji.
- Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> do odpowiedniej wartości na podstawie czasu odświeżania DNS.
- Utwórz wystąpienia `HttpClient` przy użyciu `new HttpClient(handler, dispostHandler: false)`, zgodnie z wymaganiami.

Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` w podobny sposób rozwiązują.

- `SocketsHttpHandler` udostępnia połączenia między wystąpieniami `HttpClient`. To udostępnianie uniemożliwia wyczerpanie gniazda.
- `SocketsHttpHandler` cykluje połączenia zgodnie z `PooledConnectionLifetime`, aby uniknąć nieodświeżonych problemów z usługą DNS.

### <a name="cookies"></a>Cookie

Wystąpienia `HttpMessageHandler` w puli powoduje, że obiekty `CookieContainer` są udostępniane. Nieoczekiwane udostępnianie obiektów `CookieContainer` często powoduje nieprawidłowy kod. W przypadku aplikacji wymagających plików cookie należy rozważyć następujące kwestie:

 - Wyłączanie obsługi automatycznej plików cookie
 - Unikanie `IHttpClientFactory`

Wywołaj <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>, aby wyłączyć automatyczną obsługę plików cookie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a>Rejestrowanie

Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań. Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika. Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.

Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu. Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań. Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku. W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.

Rejestrowanie odbywa się również w potoku obsługi żądania. W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci. W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.

Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku. Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.

Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.

## <a name="configure-the-httpmessagehandler"></a>Konfigurowanie HttpMessageHandler

Może być konieczne kontrolowanie konfiguracji `HttpMessageHandler` wewnętrznej używanej przez klienta.

`IHttpClientBuilder` jest zwracany podczas dodawania nazwanych lub wskazanych klientów. Metoda rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> może służyć do definiowania delegata. Delegat służy do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używanego przez tego klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a>Korzystanie z IHttpClientFactory w aplikacji konsolowej

W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:

* [Microsoft. Extensions. hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http)

W poniższym przykładzie:

* <xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usług [hosta ogólnego](xref:fundamentals/host/generic-host) .
* `MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient`. `HttpClient` jest używany do pobierania strony sieci Web.
* `Main` tworzy zakres do wykonania metody `GetPage` usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementowanie wzorca wyłącznika](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
