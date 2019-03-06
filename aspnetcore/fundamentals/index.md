---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: 'Dowiedz się, podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: fundamentals/index
---
# <a name="aspnet-core-fundamentals"></a>Podstawy platformy ASP.NET Core

W tym artykule przedstawiono kluczowe tematy zrozumieć, jak opracowywać aplikacje platformy ASP.NET Core.

## <a name="the-startup-class"></a>Klasa Startup

`Startup` Klasy jest, gdy:

* Wszystkie wymagane przez aplikację usługi są skonfigurowane.
* Żądanie obsługi potoku jest zdefiniowana.

* Kod, aby skonfigurować (lub *zarejestrować*) usługi jest dodawany do `Startup.ConfigureServices` metody. *Usługi* przedstawiono składniki, które są używane przez aplikację. Na przykład obiekt kontekstu platformy Entity Framework Core to usługa.
* Kod, aby skonfigurować żądanie obsługi potoku jest dodawany do `Startup.Configure` metody. Potok składa się jako serię *oprogramowania pośredniczącego* składników. Na przykład oprogramowanie pośredniczące może obsługiwać żądań dotyczących plików statycznych lub przekierowywanie żądań HTTP do HTTPS. Każdy oprogramowania pośredniczącego wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.

::: moniker range=">= aspnetcore-2.0"

Poniżej znajduje się przykładowy `Startup` klasy:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

::: moniker-end

Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

## <a name="dependency-injection-services"></a>Wstrzykiwanie zależności (usługi)

ASP.NET Core ma framework iniekcji (DI) wbudowane zależności czy services sprawia, że skonfigurowano dostępne dla klasy aplikacji. Jednym ze sposobów, aby pobrać wystąpienie usługi w klasie jest tworzenie konstruktora przy użyciu parametru wymaganego typu. Parametr może być typ usługi lub interfejs. DI system oferuje usługi w czasie wykonywania.

::: moniker range=">= aspnetcore-2.0"

W tym miejscu to klasa, która używa DI w celu uzyskania obiektu kontekstu platformy Entity Framework Core. Przykładem iniekcji konstruktora jest wyróżniony wiersz:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

::: moniker-end

Gdy DI jest wbudowany, służy ona do umożliwiają podłączenie w kontenerze Inwersja kontroli (IoC) innych firm, jeśli użytkownik sobie tego życzy.

Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Oprogramowanie pośredniczące

Żądanie obsługi Potok składa się jako serię składników oprogramowania pośredniczącego. Każdy składnik wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.

Zgodnie z Konwencją składnik oprogramowania pośredniczącego jest dodawane do potoku za pomocą jego `Use...` metody rozszerzenia w `Startup.Configure` metody. Na przykład, aby umożliwić renderowanie pliki statyczne, należy wywołać `UseStaticFiles`.

::: moniker range=">= aspnetcore-2.0"

Wyróżniony kod w poniższym przykładzie konfiguruje żądania obsługi potoku:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

::: moniker-end

Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i możesz napisać niestandardowego oprogramowania pośredniczącego.

Aby uzyskać więcej informacji, zobacz [oprogramowania pośredniczącego](xref:fundamentals/middleware/index).

<a id="host"/>

## <a name="the-host"></a>Host

Tworzy aplikację ASP.NET Core *hosta* przy uruchamianiu. Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takich jak:

* Implementację serwera HTTP
* Składniki oprogramowania pośredniczącego
* Rejestrowanie
* DI
* Konfiguracja

Głównym powodem wraz ze wszystkimi zasobami współzależne aplikacji w jeden obiekt jest zarządzanie okresem istnienia: kontrolę nad uruchamianiem aplikacji i łagodne zamykanie.

Kod w celu utworzenia hosta jest `Program.Main` i następuje [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern). Metody są wywoływane w celu skonfigurowania każdego zasobu należącego do hosta. Metoda Konstruktor jest wywoływana w celu wyciągniesz go razem i Utwórz wystąpienie obiektu hosta.

::: moniker range="<= aspnetcore-2.2"

Platforma ASP.NET Core 2.x używa hosta sieci Web ( `WebHost` klasy) dla aplikacji sieci web. Udostępnia platformę `CreateDefaultBuilder` skonfigurować hosta z powszechnie używane opcje, takie jak następujące:

* Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.
* Konfiguracja obciążenia z *appsettings.json*, zmienne środowiskowe, argumenty wiersza polecenia i innych źródeł.
* Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Oto przykładowy kod, który tworzy hosta:

[!code-csharp[](index/snapshots/2.x/Program1.cs?highlight=9)]

Aby uzyskać więcej informacji, zobacz [hosta sieci Web](xref:fundamentals/host/web-host).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

W programie ASP.NET Core 3.0 to hosta sieci Web (`WebHost` klasy) lub ogólny hosta (`Host` klasy) może być używana w aplikacji sieci web. Ogólny hosta jest zalecana i hosta sieci Web jest dostępna dla zapewnienia zgodności.

Udostępnia platformę `CreateDefaultBuilder` i `ConfigureWebHostDefaults` metody, aby skonfigurować hosta z powszechnie używane opcje, takie jak następujące:

* Użyj [Kestrel](#servers) jako integracja sieci web serwera i Włącz usługi IIS.
* Konfiguracja obciążenia z *appsettings.json*, *appsettings. [ .Json EnvironmentName]*, zmienne środowiskowe i argumenty wiersza polecenia.
* Wyślij rejestrowania danych wyjściowych do konsoli i debugowanie dostawców.

Oto przykładowy kod, który tworzy hosta. Metody, które skonfigurować hosta z powszechnie używane opcje są wyróżnione.

[!code-csharp[](index/snapshots/3.x/Program1.cs?highlight=9-10)]

Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host) i [hosta sieci Web](xref:fundamentals/host/web-host)

::: moniker-end

### <a name="advanced-host-scenarios"></a>Scenariusze zaawansowane hosta

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Host sieci Web jest przeznaczony do zawierać implementację serwera HTTP, który nie jest wymagany dla innych rodzajów aplikacji .NET. Począwszy od hosta ogólny 2.1 (`Host` klasy) jest dostępna dla dowolnej aplikacji platformy .NET Core użyć&mdash;nie tylko aplikacje platformy ASP.NET Core. Ogólny hosta pozwala korzystać z kompleksowych funkcji, takich jak rejestrowanie, zarządzanie okresem istnienia DI, konfiguracji i aplikacji, w przypadku innych typów aplikacji. Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Ogólny Host jest dostępny dla dowolnej aplikacji platformy .NET Core użyć&mdash;nie tylko aplikacje platformy ASP.NET Core. Ogólny hosta pozwala korzystać z kompleksowych funkcji, takich jak rejestrowanie, zarządzanie okresem istnienia DI, konfiguracji i aplikacji, w przypadku innych typów aplikacji. Aby uzyskać więcej informacji, zobacz [ogólnego hosta](xref:fundamentals/host/generic-host).

::: moniker-end

Host umożliwia również uruchamianie zadań w tle. Aby uzyskać więcej informacji, zobacz [zadania w tle](xref:fundamentals/host/hosted-services).

## <a name="servers"></a>Serwery

Aplikacji ASP.NET Core używa implementację serwera HTTP, aby nasłuchiwała żądań HTTP. Żądania powierzchnie serwerów aplikacji jako zbiór [funkcje na żądanie](xref:fundamentals/request-features) składające się na `HttpContext`.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Platforma ASP.NET Core oferuje następujące implementacje serwera:

* *Kestrel* jest serwerem sieci web dla wielu platform. Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/). ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.
* *Serwer HTTP IIS* jest serwerem dla systemu windows, która korzysta z usług IIS. Z tym serwerem aplikacji ASP.NET Core i usługi IIS uruchamiane w tym samym procesie.
* *Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform. ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem. Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform. ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem. Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Platforma ASP.NET Core oferuje następujące implementacje serwera:

* *Kestrel* jest serwerem sieci web dla wielu platform. Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/). ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem.
* *Sterownik HTTP.sys* dotyczy serwera Windows, który nie jest używany z usługami IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform. ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem. Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core oferuje *Kestrel* implementacji serwera dla wielu platform. ASP.NET Core w wersji 2.0 lub nowszej Kestrel może działać jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem. Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/).

---

::: moniker-end

Aby uzyskać więcej informacji, zobacz [serwerów](xref:fundamentals/servers/index).

## <a name="configuration"></a>Konfiguracja

Platforma ASP.NET Core zapewnia środowisko konfiguracji, które pobiera ustawienia jako pary nazwa wartość z uporządkowany zestaw dostawców konfiguracji. Brak dostawców wbudowanych konfiguracji dla różnych źródeł, takich jak *.json* pliki, *.xml* plików, zmienne środowiskowe i argumenty wiersza polecenia. Można także napisać dostawców konfiguracji niestandardowej.

Na przykład, można określić tę konfigurację pochodzi z *appsettings.json* i zmiennych środowiskowych. Wówczas, gdy wartość *ConnectionString* jest wymagana, struktura wygląda pierwszy w *appsettings.json* pliku. Jeśli wartość znajduje się tam, ale także w zmiennej środowiskowej, wartość zmiennej środowiskowej wyższy priorytet.

Zarządzanie konfiguracji poufne dane, takie jak hasła, ASP.NET Core zapewnia [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Wpisy tajne w środowisku produkcyjnym, zaleca się [usługi Azure Key Vault](/aspnet/core/security/key-vault-configuration).

Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="options"></a>Opcje

Jeśli to możliwe, następujące platformy ASP.NET Core *wzorzec opcje* do przechowywania i pobierania wartości konfiguracji. Wzorzec opcje używa klas do reprezentowania grup powiązane ustawienia.

Na przykład poniższy kod ustawia opcje WebSockets:

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Aby uzyskać więcej informacji, zobacz [opcje](xref:fundamentals/configuration/options).

## <a name="environments"></a>Środowiska

Środowiska wykonawcze, takich jak *rozwoju*, *przemieszczania*, i *produkcji*, to najwyższej jakości pojęcie w programie ASP.NET Core. Można określić, ustawiając działa w środowisku aplikacji `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej. Platforma ASP.NET Core odczytuje tę zmienną przy uruchamianiu aplikacji i przechowuje wartość w `IHostingEnvironment` implementacji. Obiekt środowiska jest dostępny w aplikacji za pośrednictwem DI w dowolnym miejscu.

::: moniker range=">= aspnetcore-2.0"

Poniższy przykładowy kod z `Startup` klasy konfiguruje aplikację, aby zapewnić szczegółowe informacje o błędzie, tylko wtedy, gdy działa w trakcie opracowywania:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

::: moniker-end

Aby uzyskać więcej informacji, zobacz [środowisk](xref:fundamentals/environments).

## <a name="logging"></a>Rejestrowanie

Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców rejestrowania wbudowanych oraz innych firm. Dostępni dostawcy są następujące:

* Konsola
* Debugowanie
* Śledzenie zdarzeń na Windows
* Dziennik zdarzeń Windows
* TraceSource
* Usługa Azure App Service
* Usługi Azure Application Insights

Zapis loguje się z dowolnego miejsca w kodzie aplikacji uzyskując `ILogger` obiekt z DI i wywoływania metod dziennika.

::: moniker range=">= aspnetcore-2.0"

Poniżej przedstawiono przykładowy kod, który używa `ILogger` obiektu przy użyciu iniekcji konstruktora i wywołań metod rejestrowania, które są wyróżnione.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

::: moniker-end

`ILogger` Interfejs pozwala przekazać dowolną liczbę pól do dostawcy logowania. Pola są często używane do konstruowania ciąg komunikatu, ale dostawca może także wysłać im jako oddzielne pola w magazynie danych. Ta funkcja umożliwia rejestrowanie dostawców, aby zaimplementować [semantycznego rejestrowania, nazywana również rejestrowaniem strukturalnym](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Aby uzyskać więcej informacji, zobacz [rejestrowania](xref:fundamentals/logging/index).

## <a name="routing"></a>Routing

A *trasy* jest wzorzec adresu URL, który jest mapowany do obsługi. Program obsługi jest zwykle strony Razor, metody akcji kontrolera MVC lub oprogramowanie pośredniczące. ASP.NET Core routingu zapewnia kontrolę nad adresów URL używanych przez aplikację.

Aby uzyskać więcej informacji, zobacz [Routing](xref:fundamentals/routing).

## <a name="error-handling"></a>Obsługa błędów

Platforma ASP.NET Core ma wbudowane funkcje obsługi błędów, takich jak:

* Stronie wyjątków dla deweloperów
* Strony błędów niestandardowych
* Stan statycznej strony kodowe
* Obsługa wyjątków uruchamiania

Aby uzyskać więcej informacji, zobacz [obsługi błędów](xref:fundamentals/error-handling).

::: moniker range=">= aspnetcore-2.1"

## <a name="make-http-requests"></a>Zgłaszanie żądań HTTP

Implementacja `IHttpClientFactory` jest dostępna dla tworzenia `HttpClient` wystąpień. Fabryka:

* Stanowi centralną lokalizację do nazywania i konfigurowanie logiczne `HttpClient` wystąpień. Na przykład *github* klienta można zarejestrować i skonfigurowane do korzystania z usługi GitHub. Domyślne klienta można zarejestrować do innych celów.
* Obsługuje rejestrację i tworzenie łańcucha wielu obsługi delegowania do tworzenia potoku oprogramowania pośredniczącego usługi wychodzące żądanie. Wzorzec ten jest podobny do potoku oprogramowanie pośredniczące dla ruchu przychodzącego w programie ASP.NET Core. Wzorzec zapewnia mechanizm zarządzania odciąż przekrojowe zagadnienia dotyczące żądań HTTP, w tym usługi pamięć podręczna obsługi serializacji i rejestrowania błędów.
* Integruje się z *Polly*, popularne biblioteki innych firm dotyczące obsługi błędów przejściowych.
* Zarządza buforowanie i okresem istnienia bazowego `HttpClientMessageHandler` wystąpienia, aby uniknąć problemów DNS, które występują, gdy ręcznego zarządzania `HttpClient` okresy istnienia.
* Dodanie obsługi można skonfigurować rejestrowania (za pośrednictwem *ILogger*) dla wszystkich żądań wysłanych przez klientów utworzonych przez fabrykę.

Aby uzyskać więcej informacji, zobacz [żądań HTTP należy](xref:fundamentals/http-requests).

::: moniker-end

## <a name="content-root"></a>Zawartość katalogu głównego

Główny zawartości jest ścieżka podstawowa do prywatnej zawartości używany przez aplikację, takie jak jego pliki Razor. Domyślnie zawartość katalogu głównego jest podstawową ścieżkę dla pliku wykonywalnego hostingu aplikacji. Być może alternatywną lokalizację określony podczas [tworzenia hosta](#host).

::: moniker range="<= aspnetcore-2.2"

Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/web-host#content-root).

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Aby uzyskać więcej informacji, zobacz [zawartości głównego](xref:fundamentals/host/generic-host#content-root).

::: moniker-end

## <a name="web-root"></a>Katalog główny sieci Web

Katalog główny sieci web (znany także jako *webroot*) to ścieżka podstawowa do publicznej, statycznej zasobów, takich jak CSS, JavaScript i plików obrazów. Oprogramowanie pośredniczące plików statycznych posłużą tylko pliki z katalogu głównego sieci web (i jego podkatalogi) domyślnie. Wartością domyślną jest ścieżka do katalogu głównego sieci web  *\<zawartości główny > / wwwroot*, ale innej lokalizacji może zostać określone, jeśli [tworzenia hosta](#host).

W aparacie Razor (*.cshtml*) plików ukośnika tylda `~/` wskazuje katalog główny sieci web. Począwszy od ścieżki `~/` są określane jako ścieżek wirtualnych.

Aby uzyskać więcej informacji, zobacz [pliki statyczne](xref:fundamentals/static-files).
