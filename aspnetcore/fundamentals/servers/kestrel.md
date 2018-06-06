---
title: Kestrel implementacja serwera sieci web platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat Kestrel, serwer sieci web i platform dla platformy ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 22311d90fb327fc1e01f82759ad62551501a10c1
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734565"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>Kestrel implementacja serwera sieci web platformy ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Roaming Krzysztof](https://github.com/Tratcher), i [Stephen Halter](https://twitter.com/halter73)

Obsługujący wiele platform jest kestrel [serwera sieci web dla platformy ASP.NET Core](xref:fundamentals/servers/index). Kestrel to serwer sieci web, który jest domyślnie włączone w szablony projektów platformy ASP.NET Core.

Kestrel obsługuje następujące funkcje:

* HTTPS
* Uaktualnienie nieprzezroczyste używane do włączania [WebSockets](https://github.com/aspnet/websockets)
* Gniazda UNIX o wysokiej wydajności za Nginx

Kestrel jest obsługiwana na wszystkich platformach i wersje, które obsługuje .NET Core.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>Kiedy należy używać Kestrel z zwrotny serwer proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Albo konfiguracji&mdash;z lub bez zwrotnego serwera proxy&mdash;jest prawidłowa i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core w wersji 2.0 lub nowszej aplikacji.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, Kestrel można bezpośrednio jako serwer aplikacji.

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

Jeśli udostępnianie aplikacji z Internetem, za pomocą usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Zwrotny serwer proxy jest wymagany w przypadku wdrożeń krawędzi (ujawniony na ruch z Internetu) ze względów bezpieczeństwa. Wersje 1.x Kestrel nie mają pełny zestaw ochronę przed atakami, takimi jak odpowiednie limity czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.

---

W przypadku wielu aplikacji, które korzysta z tego samego adresu IP i port uruchomione na jednym serwerze istnieje scenariusz zwrotnego serwera proxy. Kestrel nie obsługuje ten scenariusz, ponieważ Kestrel nie obsługuje udostępniania tego samego adresu IP i portu między wiele procesów. Po skonfigurowaniu Kestrel do nasłuchiwania na porcie Kestrel obsługuje cały ruch do tego portu, niezależnie od tego, żądania nagłówek hosta. Zwrotny serwer proxy, który można udostępniać porty zdolność do przekazywania żądań do Kestrel na unikatowy adresu IP i portu.

Nawet jeśli zwrotnego serwera proxy nie jest wymagane, dobrym rozwiązaniem może być przy użyciu zwrotnego serwera proxy:

* Można go ograniczyć narażonych publicznej powierzchni aplikacje, które obsługuje.
* Zapewnia dodatkową warstwę konfiguracji i obrony.
* Może ją zintegrować lepiej z istniejącej infrastruktury.
* Takie rozwiązanie upraszcza równoważenia obciążenia i konfiguracja SSL. Tylko zwrotnego serwera proxy wymaga certyfikatu SSL i że serwer może komunikować się z serwerami aplikacji w sieci wewnętrznej przy użyciu zwykłego protokołu HTTP.

> [!WARNING]
> Jeśli nie używa zwrotny serwer proxy z hostem Filtrowanie włączone, [filtrowania host](#host-filtering) musi być włączona.

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>Jak używać Kestrel w aplikacji platformy ASP.NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) dodatku [Microsoft.AspNetCore.App metapackage] (xref:fundamentals / metapackage aplikacji) (platformy ASP.NET Core 2.1 lub nowszej).

Szablony projektów platformy ASP.NET Core domyślnie używają Kestrel. W *Program.cs*, wywołań kodu szablonów [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), które wywołuje [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) w tle.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Zainstaluj [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) pakietu NuGet.

Wywołanie [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) — metoda rozszerzenia na [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) w `Main` metoda określania żadnego [opcje Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) wymagane, jak pokazano w następnej sekcji.

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Opcje kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Serwer sieci web Kestrel ma ograniczenie opcje konfiguracji, które są szczególnie przydatne w przypadku wdrożeń skierowane do Internetu. Kilka ważnych ograniczeń, które można dostosować:

* Maksymalna liczba połączeń klientów
* Żądanie maksymalny rozmiar treści
* Szybkość danych treści żądania minimalna

Ustaw na tych i innych ograniczeń [limity](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) właściwość [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) klasy. `Limits` Właściwość przechowuje wystąpienia [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) klasy.

**Maksymalna liczba połączeń klientów**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

Można ustawić maksymalną liczbę równoczesnych połączeń TCP otwarte dla całej aplikacji z następującym kodem:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

Brak oddzielnych limit połączeń, które zostały uaktualnione do inny protokół (na przykład na żądanie Websocket) z HTTP lub HTTPS. Po uaktualnieniu połączenie nie jest uwzględniane `MaxConcurrentConnections` limit.

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

Maksymalna liczba połączeń jest nieograniczony (null) domyślnie.

**Żądanie maksymalny rozmiar treści**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

Domyślny rozmiar treści żądania maksymalna jest 30,000,000 bajtów, czyli około 28.6 MB.

Zalecane podejście do zastąpienia limitu w aplikacji platformy ASP.NET Core MVC jest użycie [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) atrybutu metody akcji:

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Oto przykład, który pokazuje, jak skonfigurować ograniczenia dla aplikacji na każde żądanie:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

Można zastąpić ustawienie dla określonego żądania w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

Przy próbie skonfigurować limit na żądanie, po uruchomieniu aplikacji można odczytać żądania, jest zgłaszany wyjątek. Brak `IsReadOnly` właściwość, która wskazuje, czy `MaxRequestBodySize` właściwość jest w stanie tylko do odczytu, co oznacza jest za późno skonfigurować limit.

**Szybkość danych treści żądania minimalna**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

Kestrel sprawdza co sekundę, jeśli dane jest otrzymywanych określonej szybkości w bajtach na sekundę. Jeżeli stawki spadnie poniżej wartości minimalnej, jest upłynął limit czasu połączenia. Okres prolongaty wynosi ilość czasu, że Kestrel daje klientowi zwiększyć jego częstotliwość wysyłania do minimum; wskaźnik nie jest zaznaczone, w tym czasie. Okres prolongaty pomaga uniknąć porzucenie połączeń, które początkowo wysyłania danych prędkością wolne ze względu na wolne TCP-start.

Minimalna szybkość domyślna jest 240 bajtów na sekundę z 5-sekundowego okres prolongaty.

Minimalna częstotliwość ma również zastosowanie do odpowiedzi. Kod, aby ustawić limit żądań i odpowiedzi limit jest taki sam, z wyjątkiem o `RequestBody` lub `Response` w nazwach właściwości i interfejsu.

Oto przykład pokazujący sposób konfigurowania stawki minimalną ilość danych w *Program.cs*:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-7)]

Stawki na żądanie można skonfigurować w oprogramowaniu pośredniczącym:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

Aby uzyskać informacje o innych opcjach Kestrel i limity zobacz:

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Aby uzyskać informacji o opcjach Kestrel i limity zobacz:

* [Klasa KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>Konfiguracja punktu końcowego

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
Domyślnie program ASP.NET Core wiąże `http://localhost:5000`. Wywołanie [nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) lub [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metod [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) do konfiguracji prefiksy URL i portów dla Kestrel. `UseUrls`, `--urls` argumentu wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również służbowych, ale ma ograniczenia później wymienionych w tej sekcji.

`urls` Klucz konfiguracji hosta muszą pochodzić z konfiguracją hosta, nie konfiguracji aplikacji. Dodawanie `urls` klucza i wartości do *appsettings.json* nie wpływa na konfigurację hosta, ponieważ host jest w pełni zainicjowany w czasie konfiguracji zostanie odczytany z pliku konfiguracji. Jednak `urls` klucza w *appsettings.json* mogą być używane z [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) na Konstruktor hosta, aby skonfigurować hosta:

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Domyślnie program ASP.NET Core wiąże:

* `http://localhost:5000`
* `https://localhost:5001` (jeśli lokalne działania projektowe certyfikat znajduje się)

Utworzono certyfikatu deweloperskiego:

* Gdy [.NET Core SDK](/dotnet/core/sdk) jest zainstalowany.
* [Narzędzie certyfikaty deweloperów](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) służy do tworzenia certyfikatu.

Niektóre przeglądarki wymagają przyznanie jawnym uprawnieniem do przeglądarki do ufać certyfikatowi rozwoju lokalnego.

Platformy ASP.NET Core 2.1 i nowsze szablony projektów Konfigurowanie aplikacji są domyślnie uruchamiane na HTTPS i zawierać [obsługuje przekierowania protokołu HTTPS i HSTS](xref:security/enforcing-ssl).

Wywołanie [nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) lub [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) metod [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) do konfiguracji prefiksy URL i portów dla Kestrel.

`UseUrls`, `--urls` argumentu wiersza polecenia `urls` klucz konfiguracji hosta, a `ASPNETCORE_URLS` zmiennej środowiskowej również służbowych, ale ma ograniczenia później wymienionych w tej sekcji (domyślny certyfikat musi być dostępny dla punktu końcowego protokołu HTTPS Konfiguracja).

Platformy ASP.NET Core 2.1 `KestrelServerOptions` konfiguracji:

**ConfigureEndpointDefaults (akcji&lt;ListenOptions&gt;)**  
Określa konfigurację `Action` do uruchamiania dla każdego określonego punktu końcowego. Wywoływanie `ConfigureEndpointDefaults` wielokrotnie zastępuje przed `Action`s w ostatnich `Action` określony.

**ConfigureHttpsDefaults (akcji&lt;HttpsConnectionAdapterOptions&gt;)**  
Określa konfigurację `Action` do uruchamiania dla każdego punktu końcowego protokołu HTTPS. Wywoływanie `ConfigureHttpsDefaults` wielokrotnie zastępuje przed `Action`s w ostatnich `Action` określony.

**Configure(IConfiguration)**  
Tworzy ładujący konfiguracji, do konfigurowania Kestrel, która przyjmuje [wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) jako dane wejściowe. Konfiguracji należy określić zakres sekcji konfiguracyjnej dla Kestrel.

**ListenOptions.UseHttps**  
Skonfiguruj Kestrel do używania protokołu HTTPS.

`ListenOptions.UseHttps` rozszerzenia:

* `UseHttps` &ndash; Skonfiguruj Kestrel do używania protokołu HTTPS z domyślnego certyfikatu. Zgłasza wyjątek, jeśli jest skonfigurowany żaden certyfikat domyślny.
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` Parametry:

* `filename` jest ścieżka i nazwa pliku certyfikatu względem katalogu, który zawiera pliki zawartości aplikacji.
* `password` to hasło wymagane do uzyskania dostępu do danych certyfikatu X.509.
* `configureOptions` jest `Action` skonfigurować `HttpsConnectionAdapterOptions`. Zwraca `ListenOptions`.
* `storeName` jest magazyn certyfikatów, z którego można załadować certyfikatu.
* `subject` jest to nazwa podmiotu certyfikatu.
* `allowInvalid` Wskazuje, czy nieprawidłowe certyfikaty należy rozważyć, takich jak certyfikaty z podpisem własnym.
* `location` jest to lokalizacja magazynu można załadować certyfikatu z.
* `serverCertificate` jest to certyfikat X.509.

W środowisku produkcyjnym należy jawnie skonfigurować HTTPS. Co najmniej należy podać certyfikat domyślny.

Obsługiwane konfiguracje opisane w dalszej części:

* Brak konfiguracji
* Zastąp domyślnego certyfikatu z konfiguracji
* Zmiana ustawień domyślnych w kodzie

*Brak konfiguracji*

Nasłuchuje kestrel `http://localhost:5000` i `https://localhost:5001` (jeśli cert domyślny jest dostępna).

Określ adresy URL przy użyciu:

* `ASPNETCORE_URLS` Zmiennej środowiskowej.
* `--urls` argument wiersza polecenia.
* `urls` Klucz konfiguracji hosta.
* `UseUrls` metody rozszerzenia.

Aby uzyskać więcej informacji, zobacz [adresów URL serwera](xref:fundamentals/host/web-host#server-urls) i [zastępczą konfigurację](xref:fundamentals/host/web-host#override-configuration).

Wartość podana przy użyciu tych metod może być co najmniej jeden protokół HTTP i HTTPS punktów końcowych (HTTPS jeśli cert domyślny jest dostępna). Skonfiguruj wartość jako lista Rozdzielana średnikami (na przykład `"Urls": "http://localhost:8000;http://localhost:8001"`).

*Zastąp domyślnego certyfikatu z konfiguracji*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) wywołania `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` domyślnie załadować Kestrel konfiguracji. Domyślny schemat konfiguracji ustawień aplikacji HTTPS jest dostępna dla Kestrel. Skonfiguruj wiele punktów końcowych, w tym adresy URL i certyfikaty do użycia z pliku na dysku lub z magazynu certyfikatów.

W następujących *appsettings.json* przykład:

* Ustaw **AllowInvalid** do `true` Aby zezwolić na korzystanie z certyfikatów nieprawidłowe (na przykład certyfikaty z podpisem własnym).
* Punkt końcowy HTTPS, którego nie można określić certyfikatu (**HttpsDefaultCert** w następującym przykładzie) powraca do certyfikatu zdefiniowane w obszarze **certyfikaty** > **domyślne**  lub certyfikatu deweloperskiego.

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Zamiast **ścieżki** i **hasło** dla każdego certyfikatu węzła jest określenie certyfikatu przy użyciu pól magazynu certyfikatów. Na przykład **certyfikaty** > **domyślne** certyfikatu może być określona jako:

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

Informacje o schemacie:

* Nazwy punktów końcowych jest rozróżniana wielkość liter. Na przykład `HTTPS` i `Https` są prawidłowe.
* `Url` Parametr jest wymagany dla każdego punktu końcowego. Format dla tego parametru jest taka sama jak lokacja najwyższego poziomu `Urls` parametru konfiguracji, ale na maksymalnie jedną wartość.
* Te punkty końcowe zastąpienia zdefiniowane w najwyższego poziomu `Urls` konfiguracji, zamiast dodawania do nich. Punkty końcowe zdefiniowane w kodzie za pomocą `Listen` kumulują się z punktami końcowymi, zdefiniowane w sekcji konfiguracji.
* `Certificate` Sekcja jest opcjonalna. Jeśli `Certificate` sekcji nie jest określony, będą używane wartości domyślne, zdefiniowane w scenariuszach wcześniej. Jeśli domyślne nie są dostępne, serwer zgłasza wyjątek i nie można uruchomić.
* `Certificate` Sekcji obsługuje zarówno **ścieżki**&ndash;**hasło** i **podmiotu**&ndash;**magazynu** certyfikaty.
* W ten sposób może być zdefiniowana dowolną liczbę punktów końcowych, tak długo, jak ich nie powodują konfliktów portów.
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` Zwraca `KestrelConfigurationLoader` z `.Endpoint(string name, options => { })` metodę, która może służyć do uzupełnienia ustawienia skonfigurowanego punktu końcowego:

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  Można również uzyskać dostęp do `KestrelServerOptions.ConfigurationLoader` Aby zachować iteracja na istniejący moduł ładujący, takie jak udostępnione przez [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

* Jest dostępna w opcjach dostępnych w sekcji konfiguracyjnej dla każdego punktu końcowego `Endpoint` metody, dzięki czemu mogą być odczytywane ustawienia niestandardowe.
* Wiele konfiguracji mogą być ładowane przez wywołanie metody `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` ponownie, używając innej sekcji. Ostatniej konfiguracji jest używana, chyba że `Load` jawnie jest wywoływana w poprzednich wystąpień. Wywołanie nie metapackage `Load` , dzięki czemu można zastąpić jego sekcji konfiguracji domyślnej.
* `KestrelConfigurationLoader` wstecznych `Listen` rodziny interfejsów API z `KestrelServerOptions` jako `Endpoint` overloads, więc punktów końcowych kodu i konfiguracji mogą być konfigurowane w tym samym miejscu. Te przeciążenia nie używaj nazw i tylko używać domyślnych ustawień konfiguracji.

*Zmiana ustawień domyślnych w kodzie*

`ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` służy do zmiany ustawień domyślnych dla `ListenOptions` i `HttpsConnectionAdapterOptions`, tym przesłanianie domyślnego certyfikatu określonej w poprzednich scenariuszu. `ConfigureEndpointDefaults` i `ConfigureHttpsDefaults` powinna być wywoływana przed żadnych punktów końcowych są skonfigurowane.

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*Obsługa kestrel SNI*

[Wskazuje nazwę serwera (SNI)](https://tools.ietf.org/html/rfc6066#section-3) może służyć do obsługi wielu domen na tego samego adresu IP i portu. Dla SNI funkcji Klient wysyła nazwę hosta dla bezpiecznej sesji na serwerze podczas uzgadniania TLS, dzięki czemu serwer może dostarczyć prawidłowego certyfikatu. Klient używa certyfikatu złożonych szyfrowaną komunikację z serwerem podczas nawiązywania bezpiecznej sesji znajdujący się na uzgadnianie protokołu TLS.

Kestrel obsługuje SNI za pośrednictwem `ServerCertificateSelector` wywołania zwrotnego. Wywołanie zwrotne jest wywoływana raz dla każdego połączenia, aby umożliwić aplikacji Sprawdź nazwę hosta, a następnie wybierz odpowiedni certyfikat.

Obsługa SNI wymaga uruchomiona na platformie docelowej `netcoreapp2.1`. Na `netcoreapp2.0` i `net461`, wywołania zwrotnego jest wywoływane, ale `name` jest zawsze `null`. `name` Jest również `null` Jeśli klient nie udostępnia hosta nazwę parametru w uzgadniania TLS.

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```

::: moniker-end

**Powiązać gniazda TCP**

[Nasłuchiwania](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) metody wiąże gniazda TCP i zezwala na lambda opcje konfiguracji certyfikatu SSL:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

Przykład konfiguruje SSL dla punktu końcowego z [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions). Skonfiguruj inne ustawienia Kestrel dla określonych punktów końcowych za pomocą tego samego interfejsu API.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**Powiązany z gniazdem systemu Unix**

Nasłuchiwanie na gnieździe Unix z [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) zwiększonej wydajności z Nginx, jak pokazano w poniższym przykładzie:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

**Port 0**

Jeśli numer portu `0` określono Kestrel dynamicznie wiąże dostępnego portu. Poniższy przykład przedstawia sposób określić port, który Kestrel faktycznie powiązana w czasie wykonywania:

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Port0&highlight=3)]

Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:

```console
Now listening on: http://127.0.0.1:48508
```

**UseUrls, argument wiersza polecenia — adresów URL, klucz konfiguracji hosta adresy URL i ograniczenia zmiennej środowiskowej ASPNETCORE_URLS**

Skonfiguruj punkty końcowe z następujących metod:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` Argument wiersza polecenia
* `urls` Klucz konfiguracji hosta
* `ASPNETCORE_URLS` Zmienna środowiskowa

Te metody są przydatne w przypadku wprowadzania kodu pracy z serwerami innych niż Kestrel. Jednak należy pamiętać o tych ograniczeniach:

* SSL nie można używać z tych metod, chyba że domyślny certyfikat znajduje się w konfiguracji punktu końcowego protokołu HTTPS (na przykład za pomocą `KestrelServerOptions` konfiguracji lub plik konfiguracji, jak pokazano wcześniej w tym temacie).
* Gdy zarówno `Listen` i `UseUrls` podejścia są używane jednocześnie, `Listen` zastąpienie punktów końcowych `UseUrls` punktów końcowych.

**Konfiguracja punktu końcowego IIS**

Korzystając z usług IIS, powiązania adres URL dla zastąpienia IIS powiązania są ustawiane przez `Listen` lub `UseUrls`. Aby uzyskać więcej informacji, zobacz [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tematu.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Domyślnie program ASP.NET Core wiąże `http://localhost:5000`. Skonfiguruj prefiksy URL i portów dla Kestrel przy użyciu:

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) — metoda rozszerzenia
* `--urls` Argument wiersza polecenia
* `urls` Klucz konfiguracji hosta
* System konfiguracji platformy ASP.NET Core, w tym `ASPNETCORE_URLS` zmienna środowiskowa

Aby uzyskać więcej informacji na temat tych metod, zobacz [hostingu](xref:fundamentals/host/index).

**Konfiguracja punktu końcowego IIS**

Podczas korzystania z usług IIS, powiązania adres URL dla usług IIS zastąpienie powiązania ustawiony `UseUrls`. Aby uzyskać więcej informacji, zobacz [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) tematu.

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>Konfiguracja transportu

Od wersji platformy ASP.NET Core 2.1 Kestrel przez domyślny transport jest już oparta na Libuv, ale zamiast tego oparty na zarządzanych gniazda. Jest to istotne zmiany dla aplikacji programu ASP.NET 2.0 Core uaktualniania 2.1, który [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) i zależą od jednej z następujących pakietów:

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (bezpośrednie odwołanie do pakietu)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

Dla platformy ASP.NET Core 2.1 lub nowszej projektów używające [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) i wymagają użycia Libuv:

* Dodawanie zależności dla [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) pakietu aplikacji w pliku projektu:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="2.1.0" />
    ```

* Wywołanie [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a>Prefiksy adresów URL

Korzystając z `UseUrls`, `--urls` argumentu wiersza polecenia `urls` klucz konfiguracji hosta, lub `ASPNETCORE_URLS` zmiennej środowiskowej prefiksy URL może być w dowolnym z następujących formatów.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Prawidłowe są tylko prefiksy URL protokołu HTTP. Kestrel nie obsługuje protokołu SSL podczas konfigurowania powiązania adresu URL za pomocą `UseUrls`.

* Adres IPv4 z numerem portu

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.

* Adres IPv6 z numerem portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.

* Nazwa hosta z numerem portu

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  Nazwy, hostów `*`, i `+`, nie są specjalne. Niczego nie rozpoznany jako prawidłowy adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP. Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, użyj [HTTP.sys](xref:fundamentals/servers/httpsys) lub serwer zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS.

  > [!WARNING]
  > Jeśli nie używa zwrotny serwer proxy z hostem Filtrowanie włączone, Włącz [filtrowania host](#host-filtering).

* Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Gdy `localhost` określono Kestrel próbuje powiązać interfejsy sprzężenia zwrotnego protokołów IPv4 i IPv6. Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się. Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Adres IPv4 z numerem portu

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` jest szczególnych przypadkach, która jest powiązana z wszystkich adresów IPv4.

* Adres IPv6 z numerem portu

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` jest to równoważne IPv6 IPv4 `0.0.0.0`.

* Nazwa hosta z numerem portu

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  Nazwy, hostów `*`, i `+` nie są specjalne. Wszystko, co nie jest rozpoznany adres IP lub `localhost` wiąże wszystkich protokołów IPv4 i IPv6, adresy IP. Aby powiązać różne nazwy hostów z różnych aplikacji platformy ASP.NET Core w tym samym porcie, użyj [WebListener](xref:fundamentals/servers/weblistener) lub serwer zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS.

* Host `localhost` nazwa portu numer lub sprzężenia zwrotnego protokołu IP z numerem portu

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  Gdy `localhost` określono Kestrel próbuje powiązać interfejsy sprzężenia zwrotnego protokołów IPv4 i IPv6. Jeśli żądany port jest używany przez inną usługę albo interfejsu sprzężenia zwrotnego, Kestrel nie powiedzie się. Jeśli z jakiegokolwiek powodu albo interfejsu sprzężenia zwrotnego jest niedostępny (większość często, ponieważ nie jest obsługiwany protokół IPv6), Kestrel dzienniki ostrzeżenie.

* Gniazda systemu UNIX

  ```
  http://unix:/run/dan-live.sock
  ```

**Port 0**

Jeśli numer portu jest `0` określono Kestrel dynamicznie wiąże dostępnego portu. Powiązanie z portu `0` jest dozwolona dla dowolnej nazwy hosta lub adres IP, z wyjątkiem `localhost`.

Gdy aplikacja jest uruchamiana, dane wyjściowe z okna konsoli wskazuje port dynamiczny, gdzie można połączyć aplikacji:

```console
Now listening on: http://127.0.0.1:48508
```

**Prefiksy adresów URL dla protokołu SSL**

Jeśli wywołanie `UseHttps` — metoda rozszerzenia, należy uwzględnić prefiksy URL z `https:`:

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> HTTPS i HTTP nie może być umieszczona w tym samym porcie.

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a>Filtrowanie hosta

Gdy Kestrel obsługuje konfigurację oparte na prefiksy, takich jak `http://example.com:5000`, Kestrel przede wszystkim ignoruje nazwy hosta. Host `localhost` jest szczególnych przypadkach używane do wiązania na adresy sprzężenia zwrotnego. Host żadnego, innych niż jawny adres IP jest powiązany z wszystkich publicznych adresów IP. Żadne z tych informacji jest używany do sprawdzania poprawności żądania `Host` nagłówków.

::: moniker range="< aspnetcore-2.0"

Jako rozwiązanie alternatywne hosta pod zwrotny serwer proxy z filtrowania nagłówka hosta. Jest to jedyny obsługiwany scenariusz Kestrel w ASP.NET Core 1.x.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Jako obejście, za pomocą oprogramowania pośredniczącego Filtrowanie żądań przez `Host` nagłówka:

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

Zarejestruj poprzedniego `HostFilteringMiddleware` w `Startup.Configure`. Należy pamiętać, że [szeregowanie rejestracji oprogramowania pośredniczącego](xref:fundamentals/middleware/index#ordering) jest ważna. Rejestracja powinna wystąpić natychmiast po rejestracji diagnostycznych oprogramowanie pośredniczące (na przykład `app.UseExceptionHandler`).

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

Oprogramowanie pośredniczące oczekuje `AllowedHosts` klucza w *appsettings.json*/*appsettings.\< EnvironmentName > JSON*. Wartość jest rozdzielaną średnikami listę nazw hostów bez numerów portów:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Obejście tego problemu za pomocą oprogramowania pośredniczącego hosta filtrowania. Oprogramowanie pośredniczące hosta filtrowania są dostarczane przez [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) pakietu, który jest dostępny w [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (platformy ASP.NET Core 2.1 lub nowszej). Oprogramowanie pośredniczące jest dodawany przez [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), które wywołuje [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

Oprogramowanie pośredniczące hosta filtrowanie jest domyślnie wyłączona. Aby włączyć oprogramowanie pośredniczące, zdefiniuj `AllowedHosts` klucza w *appsettings.json*/*appsettings.\< EnvironmentName > JSON*. Wartość jest rozdzielaną średnikami listę nazw hostów bez numerów portów:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

*appSettings.JSON*:

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [Nagłówki oprogramowanie pośredniczące przekazane](xref:host-and-deploy/proxy-load-balancer) ma również [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) opcji. Przekazany dalej oprogramowanie pośredniczące nagłówki i oprogramowanie pośredniczące hosta filtrowania mają podobne funkcje w różnych scenariuszach. Ustawienie `AllowedHosts` z przekazywane oprogramowanie pośredniczące nagłówki jest odpowiednie, gdy nagłówek hosta nie jest zachowywane podczas przekazywania żądań z serwera zwrotnego serwera proxy lub usługę równoważenia obciążenia. Ustawienie `AllowedHosts` z oprogramowania pośredniczącego hosta filtrowanie jest gdy Kestrel zostanie użyty jako serwer graniczny lub gdy nagłówek hosta był kierowany bezpośrednio.
>
> Aby uzyskać więcej informacji na przekazywane oprogramowanie pośredniczące nagłówków, zobacz [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer).

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl)
* [Kod źródłowy kestrel](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230: Składnia i Routing komunikatów (5.4 sekcji: Host)](https://tools.ietf.org/html/rfc7230#section-5.4)
* [Konfigurowanie platformy ASP.NET Core do pracy z serwerów proxy i moduły równoważenia obciążenia](xref:host-and-deploy/proxy-load-balancer)
