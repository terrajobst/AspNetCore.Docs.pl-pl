---
title: Tworzenie żądania HTTP w programie ASP.NET Core przy użyciu IHttpClientFactory
author: stevejgordon
description: Dowiedz się więcej o zarządzaniu logicznego wystąpienia klasy HttpClient w programie ASP.NET Core za pomocą interfejsu IHttpClientFactory.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 8b95f63c0e06a2b7d1d66064def192f91b8ffbb4
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874962"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>Tworzenie żądania HTTP w programie ASP.NET Core przy użyciu IHttpClientFactory

Przez [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), i [Steve Gordon](https://github.com/stevejgordon)

<xref:System.Net.Http.IHttpClientFactory> Były rejestrowane i umożliwia konfigurowanie i tworzenie <xref:System.Net.Http.HttpClient> wystąpień w aplikacji. Oferuje następujące korzyści:

* Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień. Na przykład *github* klienta można zarejestrować i skonfigurowany do dostępu [GitHub](https://github.com/). Domyślne klienta można zarejestrować do innych celów.
* Określa zasady koncepcji wychodzących oprogramowania pośredniczącego za pośrednictwem Delegowanie obsługi w `HttpClient` i zapewnia rozszerzenia na podstawie Polly oprogramowaniu pośredniczącym, aby korzystać z zalet, który.
* Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.
* Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

::: moniker range="<= aspnetcore-2.2"

## <a name="prerequisites"></a>Wymagania wstępne

Projekty przeznaczone dla .NET Framework wymagają instalacji [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) pakietu NuGet. Projekty przeznaczone dla platformy .NET Core i odwołania [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) uwzględniają już `Microsoft.Extensions.Http` pakietu.

::: moniker-end

## <a name="consumption-patterns"></a>Wzorce użycia

Istnieje kilka sposobów `IHttpClientFactory` mogą być używane w aplikacji:

* [Podstawowe użycia](#basic-usage)
* [Nazwane klientów](#named-clients)
* [Wpisane klientów](#typed-clients)
* [Wygenerowany klientów](#generated-clients)

Żadna z nich są ściśle nadrzędne w stosunku do innego. Najlepszym rozwiązaniem, zależy od ograniczeń aplikacji.

### <a name="basic-usage"></a>Podstawowe użycia

`IHttpClientFactory` Mogą być rejestrowane przez wywołanie metody `AddHttpClient` metody rozszerzenia `IServiceCollection`w programie `Startup.ConfigureServices` metody.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

Po zarejestrowaniu kod może akceptować `IHttpClientFactory` dowolnym usług może wprowadzone z [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). `IHttpClientFactory` Może służyć do tworzenia `HttpClient` wystąpienie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Za pomocą `IHttpClientFactory` w ten sposób jest to dobry sposób, w jakie możesz refaktoryzować istniejącej aplikacji. Nie ma ona wpływu na sposób `HttpClient` jest używany. W miejscach gdzie `HttpClient` aktualnie są tworzone wystąpienia, Zastąp wywołanie w celu te wystąpienia <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Nazwane klientów

Jeśli aplikacja wymaga wielu różnych zastosowań `HttpClient`, każdy z innej konfiguracji opcją jest użycie **o nazwie klientów**. Konfiguracja nazwane `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

W poprzednim kodzie `AddHttpClient` jest wywoływana, podając nazwę *github*. Ten klient ma kilka zastosowano konfigurację domyślną&mdash;mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.

Każdorazowo `CreateClient` jest wywoływana, nowe wystąpienie klasy `HttpClient` jest tworzony i jest wywoływana Akcja konfiguracji.

Korzystanie z klienta o nazwie, mogą być przekazywane jako parametr ciągu `CreateClient`. Określ nazwę klienta, który ma zostać utworzony:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

W poprzednim kodzie żądania nie należy określić nazwę hosta. Go przekazać tylko ścieżki, ponieważ jest używany adres podstawowy skonfigurowaną dla klienta.

### <a name="typed-clients"></a>Wpisane klientów

Wpisane klientów zapewniają takie same możliwości jak nazwane klientów bez konieczności używania ciągów jako klucze. Podejście klient z typowaniem zawiera funkcję IntelliSense i kompilatora pomoc podczas korzystania z klientów. Zapewniają one pojedyncza lokalizacja umożliwiająca Konfigurowanie i wchodzić w interakcje z określonym `HttpClient`. Na przykład klient z typowaniem pojedynczego mogą być używane dla punktu końcowego pojedynczego zaplecza i hermetyzacji wszystkich logiki radzenia sobie z tym punktem końcowym. Inną zaletą jest praca z DI i może zostać wprowadzona, gdy jest to wymagane w aplikacji.

Klient z typowaniem akceptuje `HttpClient` parametru w jego konstruktorze:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

W poprzednim kodzie konfiguracji jest przenoszony do klient z typowaniem. `HttpClient` Obiektu jest przedstawiany jako właściwość publiczną. Można zdefiniować metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcji. `GetAspNetDocsIssues` Metoda umożliwia hermetyzację kodu wymaganego do kwerendy i przeanalizuj najnowsze otwarte problemy z repozytorium GitHub.

Aby zarejestrować klient z typowaniem, ogólnego <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> — metoda rozszerzenia mogą być używane w `Startup.ConfigureServices`, określanie klasy klient z typowaniem:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Klient z typowaniem jest zarejestrowany jako przejściowy z DI. Klient z typowaniem można wprowadzony i używane bezpośrednio:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Jeśli preferowane, można określić konfigurację klient z typowaniem podczas rejestracji w `Startup.ConfigureServices`, zamiast w Konstruktorze wpisane klienta:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Istnieje możliwość całkowicie hermetyzacji `HttpClient` w ramach klient z typowaniem. Zamiast uwidaczniania go jako właściwość, można podać metody publiczne która wywołuje metodę `HttpClient` wystąpienia wewnętrznie.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

W poprzednim kodzie `HttpClient` jest przechowywany jako pole prywatne. Dostęp do nawiązywania połączeń zewnętrznych przechodzi przez `GetRepos` metody.

### <a name="generated-clients"></a>Wygenerowany klientów

`IHttpClientFactory` mogą być używane w połączeniu z innymi bibliotekami innych firm takich jak [ponownego ustawienia](https://github.com/paulcbetts/refit). Refit jest biblioteką REST dla platformy .NET. Interfejsów API REST są konwertowane na żywo interfejsów. Implementacja interfejsu jest generowana dynamicznie przez `RestService`przy użyciu `HttpClient` zapewnienie zewnętrznych HTTP wywołuje.

Interfejs i odpowiedzi są zdefiniowane do reprezentowania zewnętrznego interfejsu API i odpowiedzi przez punkt końcowy:

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

Klient z typowaniem mogą być dodawane przy użyciu Refit można wygenerować wdrożenia:

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

Interfejs zdefiniowany mogą być używane w przypadku, gdy jest to konieczne, z implementacją dostarczone przez DI i Refit:

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

## <a name="outgoing-request-middleware"></a>Wychodzące żądanie oprogramowania pośredniczącego.

`HttpClient` ma już pojęcie Delegowanie obsługi, które mogą być połączone ze sobą dla wychodzących żądań HTTP. `IHttpClientFactory` Ułatwia do definiowania programów obsługi do zastosowania dla każdego klienta o nazwie. Obsługuje rejestrację i tworzenie łańcuchów z wielu obsług do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie. Każda z tych programów obsługi jest możliwość wykonywania pracy przed i po nim żądania wychodzącego. Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core. Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.

Aby utworzyć program obsługi, zdefiniuj Klasa pochodząca od <xref:System.Net.Http.DelegatingHandler>. Zastąp `SendAsync` metodę, aby wykonać kod przed przekazaniem żądania do następnej procedury obsługi w potoku:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Powyższy kod określa podstawowe programu obsługi. Sprawdza, czy `X-API-KEY` nagłówka zostało uwzględnione w żądaniu. Jeśli brakuje nagłówka, może uniknąć wywołania HTTP i zwracają odpowiedniej odpowiedzi.

Podczas rejestracji, jeden lub więcej programów obsługi można dodać do konfiguracji `HttpClient`. To zadanie jest realizowane za pośrednictwem metody rozszerzenia na <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

W poprzednim kodzie `ValidateHeaderHandler` DI jest zarejestrowany. `IHttpClientFactory` Tworzy oddzielny zakres DI dla każdej procedury obsługi. Programy obsługi mogą być zależne od usług z dowolnego zakresu. Usługi, które zależą od obsługi są usuwane po usunięciu programu obsługi.

Gdy zarejestrowana, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> może być wywoływana, przekazując typ dla programu obsługi.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

W poprzednim kodzie `ValidateHeaderHandler` DI jest zarejestrowany. Program obsługi **musi** być zarejestrowany w DI jako usługa przejściowy, nigdy nie o określonym zakresie. Jeśli program obsługi jest zarejestrowany jako usługa o określonym zakresie, wszystkie usługi, które zależy od programu obsługi są możliwe do rozporządzania programu obsługi usług można zlikwidowany, zanim program obsługi wykracza poza zakres, co spowoduje, że program obsługi ma zakończyć się niepowodzeniem.

Gdy zarejestrowana, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> może być wywoływana, przekazując typ procedury obsługi.

::: moniker-end

Procedury obsługi wielu można zarejestrować w kolejności ich powinien zostać wykonany. Każdy program obsługi kolejna procedura obsługi jest zawijany do momentu końcowe `HttpClientHandler` wykonuje żądanie:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Na udostępnianie stanu danego żądania obsługi komunikatów, użyj jednej z następujących metod:

* Przekazywanie danych do obsługi przy użyciu `HttpRequestMessage.Properties`.
* Użyj `IHttpContextAccessor` do dostępu do bieżącego żądania.
* Utwórz niestandardową `AsyncLocal` obiekt magazynu, aby przekazać dane.

## <a name="use-polly-based-handlers"></a>Użyj obsługi na podstawie Polly

`IHttpClientFactory` integruje się z popularnymi biblioteki innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly). Polly jest odporność kompleksowe i przejściowych Biblioteka obsługi błędów dla platformy .NET. Dzięki niej deweloperzy mogą express zasad, takich jak ponownych prób, wyłącznik, limit czasu, grodziowym izolacji i rezerwowe w sposób fluent i metodą o bezpiecznych wątkach.

Metody rozszerzenia są udostępniane, aby umożliwić korzystanie z zasad w usłudze Polly skonfigurowane `HttpClient` wystąpień. Polly rozszerzenia:

* Obsługuje dodawanie na podstawie Polly programów obsługi dla klientów.
* Mogą być używane po zainstalowaniu [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) pakietu NuGet. Pakiet nie jest zawarty w udostępnionej platformy ASP.NET Core.

### <a name="handle-transient-faults"></a>Obsługa błędów przejściowych

Najbardziej typowe błędy występują, gdy zewnętrznych połączeń HTTP jest przejściowy. Wywołana metoda wygodne rozszerzenie `AddTransientHttpErrorPolicy` jest włączone, co pozwala zasad w celu zdefiniowania do obsługi błędów przejściowych. Zasady skonfigurowane przy użyciu tego uchwytu — metoda rozszerzenia `HttpRequestException`, odpowiedzi 5xx protokołu HTTP i odpowiedzi HTTP 408.

`AddTransientHttpErrorPolicy` Rozszerzenia mogą być używane w `Startup.ConfigureServices`. Rozszerzenie udostępnia `PolicyBuilder` obiektu skonfigurowane do obsługi reprezentujący możliwych błędów przejściowych błędów:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

W poprzednim kodzie `WaitAndRetryAsync` zdefiniowano zasad. Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy z opóźnieniem, 600 ms między próbami.

### <a name="dynamically-select-policies"></a>Dynamiczne Wybieranie zasad

Metody rozszerzające dodatkowe istnieje, który może służyć do dodawania obsługi na podstawie Polly. Jedno z rozszerzeń jest `AddPolicyHandler`, który ma wiele przeciążeń. Jednego przeciążenia umożliwia wysłanie żądania do kontroli podczas definiowania zasad do zastosowania:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

W poprzednim kodzie przypadku wychodzące żądanie HTTP GET, 10-sekundowy limit jest stosowany. Inna metoda HTTP używany jest limit czasu 30 sekund.

### <a name="add-multiple-polly-handlers"></a>Dodawanie wielu obsług Polly

Powszechne jest wprowadzanie zagnieździć Polly zasady, aby zapewnić ulepszone funkcje:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

W powyższym przykładzie dwóch metod obsługi są dodawane. Używa pierwszego `AddTransientHttpErrorPolicy` rozszerzenia, aby dodać zasady ponawiania prób. Żądania zakończone niepowodzeniem są zwalniane maksymalnie trzy razy. Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika. Dalsze zewnętrznych żądania są blokowane przez 30 sekund, jeśli pięć nieudanych prób występują po kolei. Wyłącznik zasady są stanowe. Wszystkie połączenia za pośrednictwem tego klienta współużytkować ten sam stan obwodu.

### <a name="add-policies-from-the-polly-registry"></a>Dodawanie zasad z rejestru Polly

Podejście do zarządzania zasadami regularnie używane dotyczy je jeden raz zdefiniować i zarejestrować ich za pomocą `PolicyRegistry`. Metody rozszerzenia jest pod warunkiem, co pozwala program obsługi ma zostać dodana za pomocą zasad z rejestru:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

W poprzednim kodzie dwie zasady są rejestrowane podczas `PolicyRegistry` jest dodawany do `ServiceCollection`. Aby użyć zasad z rejestru, `AddPolicyHandlerFromRegistry` metoda jest używana nazwa zasady do zastosowania.

Więcej informacji o `IHttpClientFactory` Polly integracji można znaleźć na [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Zarządzanie HttpClient i okresu istnienia

Nowy `HttpClient` wystąpienie jest zwracane każdorazowo `CreateClient` jest wywoływana w `IHttpClientFactory`. Brak <xref:System.Net.Http.HttpMessageHandler> na klienta o nazwie. Fabryka zarządza okresy istnienia `HttpMessageHandler` wystąpień.

`IHttpClientFactory` pule `HttpMessageHandler` wystąpienia utworzone przez fabryki, aby zmniejszyć wykorzystanie zasobów. `HttpMessageHandler` Wystąpienia może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli nie upłynął jego okres istnienia.

Buforowanie obsługi jest pożądane, ponieważ każdy program obsługi zwykle zarządza własną bazowego połączeń HTTP. Tworzenie więcej obsługi niż jest to konieczne może spowodować opóźnienia w połączeniu. Niektóre procedury obsługi również nie zamykaj połączeń przez czas nieokreślony, co może uniemożliwić obsługi reagowanie na zmiany DNS.

Domyślny okres istnienia obsługi wynosi dwie minuty. Wartość domyślna może zostać zastąpiona przez poszczególnych klientów o nazwie. Aby zastąpić go, należy wywołać <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> na `IHttpClientBuilder` , jest zwracana podczas tworzenia klienta:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

Usuwanie klienta nie jest wymagana. Usuwanie anuluje wychodzących żądań i gwarancje danego `HttpClient` wystąpienia nie można używać po wywołaniu <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` śledzi i usuwa zasoby używane przez `HttpClient` wystąpień. `HttpClient` Wystąpień ogólnie można traktować jako obiektów platformy .NET nie wymaga usunięcia.

Utrzymywanie pojedynczej `HttpClient` wystąpienie aktywności przez długi czas jest typowy wzorzec używany przed powstania `IHttpClientFactory`. Ten wzorzec staje się niepotrzebne po przeprowadzeniu migracji do `IHttpClientFactory`.

## <a name="logging"></a>Rejestrowanie

Klientów utworzonych za pomocą `IHttpClientFactory` rejestrowania komunikatów dziennika dla wszystkich żądań. Włącz poziom odpowiednie informacje w swojej konfiguracji rejestrowania, aby wyświetlić komunikaty dziennika domyślne. Dodatkowe rejestrowanie, takich jak rejestrowanie nagłówków żądania, jest dostępny tylko na poziom śledzenia.

Kategoria dziennika używane dla każdego klienta zawiera nazwę klienta. Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Sufiks wiadomości *LogicalHandler* odbywa się poza potoku program obsługi żądania. Na żądanie komunikaty są rejestrowane, zanim innych programów obsługi w potoku zostały przetworzone go. W odpowiedzi komunikaty są rejestrowane po innych obsługi potoku otrzymane odpowiedzi.

Rejestrowanie występuje także w potoku programu obsługi żądania. W *MyNamedClient* przykład te komunikaty są rejestrowane względem kategoria dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Dla żądania dzieje się po wykonaniu innych programów obsługi i od razu, zanim żądanie zostanie wysłane w sieci. W odpowiedzi rejestrowanie obejmuje stan odpowiedzi przed przekazaniem za pośrednictwem potoku programu obsługi.

Włączanie rejestrowania na zewnątrz i wewnątrz potok umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku. Może to obejmować zmiany nagłówki, żądań, na przykład lub kod stanu odpowiedzi.

W tym nazwa klienta w kategorii dziennika umożliwia dziennika filtrowania dla określonych klientów o nazwie, gdy jest to konieczne.

## <a name="configure-the-httpmessagehandler"></a>Klasa HttpMessageHandler skonfigurować

Może być konieczne do kontrolowania konfiguracji wewnętrzny `HttpMessageHandler` używany przez klienta.

`IHttpClientBuilder` Jest zwracany, podczas dodawania o nazwie lub wpisane klientów. <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> — Metoda rozszerzenia może służyć do definiowania delegata. Delegat jest używany do tworzenia i konfigurowania podstawowego `HttpMessageHandler` używane przez tego klienta:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Implementowanie ponownych prób wywołania HTTP z wykorzystaniem wykładniczego wycofywania z zasadami dotyczącymi HttpClientFactory i Polly](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Implementowanie wzorca wyłącznika](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
