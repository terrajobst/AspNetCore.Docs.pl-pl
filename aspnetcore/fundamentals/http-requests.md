---
title: Zainicjuj żądań HTTP
author: stevejgordon
description: Dowiedz się więcej o zarządzaniu logicznej wystąpień HttpClient w ASP.NET Core za pomocą interfejsu IHttpClientFactory.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 1f2c7522a10220cd9520d78846d2e897115447c2
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838764"
---
# <a name="initiate-http-requests"></a>Zainicjuj żądań HTTP

Przez [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), i [Steve Gordon](https://github.com/stevejgordon)

`IHttpClientFactory` Były rejestrowane i służy do konfigurowania i utworzyć [HttpClient](/dotnet/api/system.net.http.httpclient) wystąpień w aplikacji. Zapewnia następujące korzyści:

* Udostępnia centralną lokalizację do nazywania i konfigurowanie logicznej `HttpClient` wystąpień. Na przykład klienta "github" można zarejestrować i skonfigurowane do korzystania z usługi GitHub. Domyślne klienta można zarejestrować do innych celów.
* Określa zasady koncepcji wychodzących oprogramowanie pośredniczące za pośrednictwem Delegowanie obsługi w `HttpClient` i udostępnia rozszerzenia na podstawie Polly przeprowadzać tego oprogramowania pośredniczącego.
* Zarządza buforowanie i okresem istnienia bazowy `HttpClientMessageHandler` instancje, aby uniknąć typowe problemy DNS, które występują w przypadku ręcznego zarządzania `HttpClient` okresy istnienia.
* Dodaje obsługi można skonfigurować rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysłanych przez klientów tworzone przez fabrykę.

## <a name="consumption-patterns"></a>Wzorce użycia

Istnieje kilka sposobów `IHttpClientFactory` mogą być używane w aplikacji:

* [Podstawowe sposoby użycia](#basic-usage)
* [Nazwane klientów](#named-clients)
* [Typizowany klientów](#typed-clients)
* [Wygenerowany klientów](#generated-clients)

Żaden z nich nie jest ściśle wyższego poziomu do innego. Najlepszym rozwiązaniem jest uzależnione od ograniczeń aplikacji.

### <a name="basic-usage"></a>Podstawowe sposoby użycia

`IHttpClientFactory` Mogą być rejestrowane przez wywołanie metody `AddHttpClient` — metoda rozszerzenia na `IServiceCollection`w `ConfigureServices` metody w pliku Startup.cs.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

Po zarejestrowaniu kodu może akceptować `IHttpClientFactory` dowolnym usług mogą zostać dodane z [iniekcji zależności](xref:fundamentals/dependency-injection) (Podpisane). `IHttpClientFactory` Może służyć do tworzenia `HttpClient` wystąpienie:

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

Przy użyciu `IHttpClientFactory` w ten sposób jest doskonałym sposobem Refaktoryzuj istniejącej aplikacji. Nie ma to wpływu na sposób `HttpClient` jest używany. W miejscach gdzie `HttpClient` obecnie tworzone są wystąpienia, Zastąp te wystąpienia, w wyniku wywołania `CreateClient`.

### <a name="named-clients"></a>Nazwane klientów

Jeśli aplikacja wymaga wielu różnych zastosowań `HttpClient`, każdy z innej konfiguracji opcją jest użycie **o nazwie klientów**. Konfiguracja nazwane `HttpClient` można określić podczas rejestracji w `ConfigureServices`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

W powyższym kodzie `AddHttpClient` zostanie wywołany, podając nazwę "github". Ten klient ma konfigurację domyślną, niektóre zastosowane&mdash;mianowicie adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.

Zawsze `CreateClient` jest nazywany nowe wystąpienie klasy `HttpClient` jest tworzony i jest wywoływana Akcja konfiguracji.

Użycie nazwanego klienta, można przekazać parametr typu string do `CreateClient`. Określ nazwę klienta ma zostać utworzony:

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

W powyższym kodzie żądania nie trzeba podać nazwę hosta. Można go przekazać tylko ścieżki, ponieważ jest używany adres podstawowy skonfigurowaną dla klienta.

### <a name="typed-clients"></a>Typizowany klientów

Klienci typu zawierają takie same możliwości jak nazwanego klientów bez konieczności używania ciągów jako klucze. Klient z typowaniem podejście zapewnia IntelliSense i kompilatora pomoc w przypadku uzyskiwania dostępu klientów. Udostępniają one jednej lokalizacji do konfigurowania i interakcję z określonego `HttpClient`. Na przykład jeden klient z typowaniem mogą być używane dla punktu końcowego pojedynczego wewnętrznej bazy danych i Hermetyzowanie wszystkich postępowania logiki z tego punktu końcowego. Inną zaletą jest praca z Podpisane i mogą zostać dodane, gdy jest to wymagane w aplikacji.

Klient z typowaniem akceptuje `HttpClient` parametru w swoich konstruktorach:

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

W powyższym kodzie konfiguracji jest przenoszony do typu klienta. `HttpClient` Obiektu jest ujawniona jako właściwość publiczna. Można zdefiniować metody specyficzne dla interfejsu API, które udostępniają `HttpClient` funkcji. `GetAspNetDocsIssues` Metody hermetyzuje kod wymagany do kwerendy i analizy najnowsze otwarte problemy z repozytorium GitHub.

Aby zarejestrować klienta typu ogólnego `AddHttpClient` — metoda rozszerzenia może być używana w `ConfigureServices`, określając klasy klient z typowaniem:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

Klient z typowaniem jest zarejestrowany jako przejściowy z Podpisane. Klient z typowaniem można wprowadzić i używane bezpośrednio:

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

Jeśli preferowane, konfiguracji dla typu klienta można określić podczas rejestracji w `ConfigureServices`, a nie w Konstruktorze typu klienta:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

Istnieje możliwość całkowicie Hermetyzowanie `HttpClient` w ramach typu klienta. Zamiast ujawnienie go jako właściwość, można podać metody publiczne którego wywołanie `HttpClient` wystąpienie wewnętrznie.

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

W powyższym kodzie `HttpClient` jest przechowywana jako pole prywatne. Dostęp do połączeń zewnętrznych przechodzi przez `GetRepos` metody.

### <a name="generated-clients"></a>Wygenerowany klientów

`IHttpClientFactory` może być używany w połączeniu z innych bibliotek innych firm takich jak [ponownego ustawienia](https://github.com/paulcbetts/refit). Refit to biblioteka REST dla platformy .NET. Konwertuje interfejsów API REST do interfejsów na żywo. Implementacja interfejsu jest generowany dynamicznie przez `RestService`za pomocą `HttpClient` dokonanie zewnętrznych HTTP wywołania.

Interfejs i odpowiedzi są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:

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

Klient z typowaniem mogą być dodawane przy użyciu Refit implementacji:

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

Interfejs zdefiniowanych mogą być używane w przypadku, gdy to konieczne, z implementacją Podpisane i Refit:

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

## <a name="outgoing-request-middleware"></a>Wychodzące żądania oprogramowania pośredniczącego.

`HttpClient` jest już pojęcie Delegowanie obsługi, które mogą być połączone dla wychodzących żądań HTTP. `IHttpClientFactory` Ułatwia definiowania metod obsługi do zastosowania dla każdego klienta o nazwie. Obsługuje rejestrację i tworzenie łańcucha programów obsługi wielu do tworzenia wychodzącego żądania potoku oprogramowania pośredniczącego. Każdy z tych programów obsługi jest możliwość wykonywania pracy przed i po żądania wychodzącego. Ten wzorzec jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w ASP.NET Core. Wzorzec zapewnia mechanizm zarządzania kompleksowymi dotyczy wokół żądania HTTP, włączając buforowanie błąd obsługi serializacji i rejestrowania.

Aby utworzyć program obsługi, należy zdefiniować klasy wywodzące się z `DelegatingHandler`. Zastąpienie `SendAsync` metodę, aby uruchomić kod przed przekazaniem żądania do następnej procedury obsługi w potoku:

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

Poprzedni kod definiuje podstawowy program obsługi. Sprawdza, czy nagłówek X-API-KEY została uwzględniona w żądaniu. Jeśli brakuje nagłówka go uniknąć wywołanie HTTP i odpowiednią odpowiedź zwrócona.

Podczas rejestracji, można dodać do konfiguracji obsługi co najmniej jeden `HttpClient`. To zadanie jest realizowane za pośrednictwem metody rozszerzenia na `IHttpClientBuilder`.

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

W powyższym kodzie `ValidateHeaderHandler` został zarejestrowany za pomocą Podpisane. Program obsługi **musi** rejestrowana w Podpisane jako przejściowy. Po zarejestrowaniu `AddHttpMessageHandler` można wywołać, przekazując typ obsługi.

Procedury obsługi wielu może być zarejestrowany w kolejności ich powinien zostać wykonany. Każdy program obsługi jest zawijana obsługi dalej do ostatecznego `HttpClientHandler` wykonuje żądanie:

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a>Użyć obsługi na podstawie Polly

`IHttpClientFactory` integruje się z popularnych biblioteki innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly). Polly to kompleksowe odporności i przejściowych biblioteki obsługi błędów dla platformy .NET. Umożliwia deweloperom express zasad, na przykład ponów próbę, wyłącznika limitu czasu, izolacji grodziowego i rezerwowe w sposób fluent i bezpieczne wątkowo.

Metody rozszerzenia znajdują się na korzystanie z zasad Polly z skonfigurowane `HttpClient` wystąpień. Rozszerzenia Polly są dostępne w pakiecie NuGet o nazwie "Microsoft.Extensions.Http.Polly". Ten pakiet nie jest domyślnie instalowane przez metapackage "Microsoft.AspNetCore.App". Aby korzystać z rozszerzeń, PackageReference powinny być jawnie uwzględnione w projekcie.

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

Po przywróceniu tego pakietu, metody rozszerzenia są dostępne do obsługi dodawania obsługi na podstawie Polly do klientów.

### <a name="handle-transient-faults"></a>Obsługa błędów przejściowych

Najbardziej typowych błędów, oczekiwanych może wystąpić, gdy zewnętrznych połączeń HTTP będzie mieć charakter przejściowy. Wywołana metoda rozszerzenia wygodne `AddTransientHttpErrorPolicy` jest włączone, dzięki czemu zasady można definiować w celu obsługi błędów przejściowych. Zasady skonfigurowane z ta dojścia metody rozszerzenia `HttpRequestException`, odpowiedzi HTTP 5xx i odpowiedzi HTTP 408.

`AddTransientHttpErrorPolicy` Rozszerzenia mogą być używane w `ConfigureServices`. Rozszerzenie zapewnia dostęp do `PolicyBuilder` skonfigurowany do obsługi błędów reprezentujący możliwych błędów przejściowych obiektu:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

W powyższym kodzie `WaitAndRetryAsync` zdefiniowane zasady. Żądań zakończonych niepowodzeniem są zwalniane maksymalnie trzy razy z opóźnieniem 600 ms między próbami.

### <a name="dynamically-select-policies"></a>Dynamiczne Wybieranie zasad

Metody rozszerzenia dodatkowe istnieje, które mogą służyć do dodania na podstawie Polly programów obsługi. Jedno z rozszerzeń jest `AddPolicyHandler`, który ma kilka przeciążeń. Przeciążeniami umożliwia żądania poddanych podczas definiowania zasad do zastosowania:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

W poprzednim kodzie Jeśli żądania wychodzącego jest GET, stosowana jest 10 sekund limitu czasu. W innej metody HTTP używany jest limit czasu 30 sekund.

### <a name="add-multiple-polly-handlers"></a>Dodawanie wielu obsług Polly

Jest często stosowanym rozwiązaniem zagnieździć Polly zasady umożliwiają korzystanie z funkcji rozszerzonej:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

W powyższym przykładzie są dodawane dwóch metod obsługi. Używa pierwszego `AddTransientHttpErrorPolicy` rozszerzenia, aby dodać zasady ponawiania. Żądań zakończonych niepowodzeniem są zwalniane maksymalnie trzy razy. Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje wyłącznika zasady. Dalsze żądania zewnętrzne są blokowane przez 30 sekund, ewentualnych pięć nieudanych prób sekwencyjnie. Zasady wyłącznika nie jest obiektem stanowym. Wszystkie wywołania za pośrednictwem tego klienta udostępnianie tego samego stanu obwodu.

### <a name="add-policies-from-the-polly-registry"></a>Dodawanie zasad z rejestru Polly

Jest to metoda regularnie używane zasady zarządzania je raz zdefiniować i zarejestrować je za pomocą `PolicyRegistry`. Metody rozszerzenia jest dostępne, dzięki czemu program obsługi ma zostać dodana przy użyciu zasad z rejestru:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

W powyższym kodzie PolicyRegistry jest dodawany do `ServiceCollection` i dwie zasady są zarejestrowane z nim. Aby można było używać z rejestru, zasady `AddPolicyHandlerFromRegistry` metoda jest używana nazwa zasady do zastosowania.

Więcej informacji o `IHttpClientFactory` i Polly integracji można znaleźć w [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Zarządzanie HttpClient i okresem istnienia

Zawsze `CreateClient` jest wywoływana na `IHttpClientFactory`, nowe wystąpienie klasy `HttpClient` jest zwracany. Będzie `HttpMessageHandler` na nazwanym klienta. `IHttpClientFactory` pula będzie `HttpMessageHandler` wystąpienia utworzone przez fabrykę zmniejszenie zużycia zasobów. A `HttpMessageHandler` wystąpienia może nastąpić z puli, podczas tworzenia nowego `HttpClient` wystąpienia, jeśli jego okres istnienia nie wygasł. 

Buforowanie obsługi jest pożądane, ponieważ każdy program obsługi zwykle zarządza własną podstawowej połączenia HTTP; Tworzenie obsługi więcej niż jest to konieczne może spowodować opóźnienia połączenia. Niektóre programy obsługi również nie zamykaj połączenia przez czas nieokreślony, co może uniemożliwić obsługi reagowanie na zmiany DNS.

Domyślny okres istnienia obsługi wynosi dwie minuty. Wartość domyślna może zostać zastąpiona przez poszczególnych klientów o nazwie. Aby ją zastąpić, należy wywołać `SetHandlerLifetime` na `IHttpClientBuilder` zwróceniu podczas tworzenia klienta:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a>Rejestrowanie

Klienci utworzony za pośrednictwem `IHttpClientFactory` rejestrowania komunikatów dziennika dla wszystkich żądań. Należy włączyć na poziomie odpowiednie informacje w konfigurację rejestrowania, zobacz domyślnych wiadomości dziennika. Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądania jest dostępny tylko na poziom śledzenia.

Kategoria dziennika używane dla każdego klienta zawiera nazwę klienta. Klient o nazwie "MyNamedClient", na przykład rejestruje komunikaty z kategorii `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Komunikaty z sufiksem "LogicalHandler" odbywa się na zewnątrz Potok żądań obsługi. Na żądanie komunikaty są rejestrowane przed innych programów obsługi w potoku zostały przetworzone go. W odpowiedzi komunikaty są rejestrowane po wszelkich innych programów obsługi potoku otrzymali odpowiedzi.

Rejestrowanie występuje także wewnątrz Potok żądań obsługi. W przypadku przykład "MyNamedClient" te komunikaty są rejestrowane przed kategorii dziennika `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Dla żądania występuje po wykonaniu wszystkich innych programów obsługi i bezpośrednio przed wysłaniu żądania w sieci. W odpowiedzi rejestrowanie obejmuje stanu odpowiedzi przed przekazaniem za pośrednictwem potoku obsługi.

Włączanie rejestrowania na zewnątrz i wewnątrz potoku umożliwia kontroli zmiany wprowadzone przez innych programów obsługi potoku. Może to obejmować zmiany do żądania nagłówków, na przykład lub kod stanu odpowiedzi.

Łącznie z nazwą klienta w kategorii dziennika umożliwia dziennika filtrowania dla klientów o nazwie określonej w miarę potrzeby.

## <a name="configure-the-httpmessagehandler"></a>Klasa HttpMessageHandler skonfigurować

Może być konieczne konfiguracją wewnętrzny `HttpMessageHandler` używany przez klienta.

`IHttpClientBuilder` Jest zwracany, jeśli dodanie o nazwie lub wpisany klientów. `ConfigurePrimaryHttpMessageHandler` — Metoda rozszerzenia może służyć do definiowania delegata. Delegat jest używany do tworzenia i konfigurowania serwera podstawowego `HttpMessageHandler` używany przez klienta:

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
