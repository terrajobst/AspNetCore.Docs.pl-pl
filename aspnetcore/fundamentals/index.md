---
title: Podstawy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe koncepcje tworzenia aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/index
ms.openlocfilehash: 7173a732a04bf3e598adef298fa9120c15dd52fb
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799368"
---
# <a name="aspnet-core-fundamentals"></a>Podstawy ASP.NET Core

Ten artykuł zawiera omówienie najważniejszych tematów dotyczących sposobu tworzenia aplikacji ASP.NET Core.

## <a name="the-startup-class"></a>Klasa Startup

Klasa `Startup` to:

* Usługi wymagane przez aplikację są skonfigurowane.
* Zdefiniowano potok obsługi żądań.

*Usługi* są składnikami, które są używane przez aplikację. Na przykład składnik rejestrowania to usługa. Kod do konfigurowania (lub *rejestrowania*) usług jest dodawany do metody `Startup.ConfigureServices`.

Potok obsługi żądań składa się z serii komponentów *oprogramowania pośredniczącego* . Na przykład oprogramowanie pośredniczące może obsługiwać żądania dotyczące plików statycznych lub przekierowywać żądania HTTP do protokołu HTTPS. Każde oprogramowanie pośredniczące wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie. Kod do konfigurowania potoku obsługi żądań został dodany do metody `Startup.Configure`.

Oto przykładowa Klasa `Startup`:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=3,12)]

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

## <a name="dependency-injection-services"></a>Wstrzykiwanie zależności (usługi)

ASP.NET Core ma wbudowaną platformę wstrzykiwania zależności (DI), która udostępnia skonfigurowane usługi dla klas aplikacji. Jednym ze sposobów uzyskania wystąpienia usługi w klasie jest utworzenie konstruktora z parametrem wymaganego typu. Parametr może być typem usługi lub interfejsem. System DI zapewnia usługę w czasie wykonywania.

Oto Klasa, która używa funkcji DI do pobrania obiektu kontekstu Entity Framework Core. Wyróżniony wiersz jest przykładem iniekcji konstruktora:

[!code-csharp[](index/snapshots/2.x/Index.cshtml.cs?highlight=5)]

Podczas gdy program jest wbudowany, został zaprojektowany z myślą o umożliwieniu podłączenia kontenera kontroli (IoC) innej firmy.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Oprogramowanie pośredniczące

Potok obsługi żądań składa się z serii komponentów oprogramowania pośredniczącego. Każdy składnik wykonuje operacje asynchroniczne na `HttpContext`, a następnie wywołuje następne oprogramowanie pośredniczące w potoku lub kończy żądanie.

Zgodnie z Konwencją składnik pośredniczący jest dodawany do potoku przez wywołanie jego metody rozszerzenia `Use...` w metodzie `Startup.Configure`. Na przykład, aby włączyć renderowanie plików statycznych, wywołaj `UseStaticFiles`.

Wyróżniony kod w poniższym przykładzie konfiguruje potok obsługi żądań:

[!code-csharp[](index/snapshots/2.x/Startup1.cs?highlight=14-16)]

ASP.NET Core zawiera rozbudowany zestaw wbudowanych programów pośredniczących i można napisać niestandardowe oprogramowanie pośredniczące.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index>.

## <a name="host"></a>Host

Aplikacja ASP.NET Core kompiluje *hosta* podczas uruchamiania. Host jest obiektem, który hermetyzuje wszystkie zasoby aplikacji, takie jak:

* Implementacja serwera HTTP
* Składniki oprogramowania pośredniczącego
* Rejestrowanie
* FOSFORAN
* Konfiguracja

Główną przyczyną uwzględnienia wszystkich zasobów zależnych od aplikacji w jednym obiekcie jest zarządzanie okresem istnienia: Kontrola uruchamiania aplikacji i bezpieczne zamykanie.

::: moniker range=">= aspnetcore-3.0"

Dostępne są dwa hosty: Host ogólny i host sieci Web. Zalecany jest host ogólny, a host sieci Web jest dostępny tylko w celu zapewnienia zgodności z poprzednimi wersjami.

Kod służący do tworzenia hosta znajduje się w `Program.Main`:

[!code-csharp[](index/snapshots/3.x/Program1.cs)]

Metody `CreateDefaultBuilder` i `ConfigureWebHostDefaults` umożliwiają skonfigurowanie hosta z najczęściej używanymi opcjami, takimi jak następujące:

* Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.
* Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.
* Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Dostępne są dwa hosty: host sieci Web i Host ogólny. W ASP.NET Core 2. x Host generyczny jest przeznaczony tylko dla scenariuszy innych niż sieci Web.

Kod służący do tworzenia hosta znajduje się w `Program.Main`:

[!code-csharp[](index/snapshots/2.x/Program1.cs)]

Metoda `CreateDefaultBuilder` konfiguruje hosta z najczęściej używanymi opcjami, takimi jak:

* Użyj [Kestrel](#servers) jako serwera sieci Web i Włącz INTEGRACJĘ usług IIS.
* Załaduj konfigurację z pliku *appSettings. JSON*, *appSettings. { Nazwa środowiska}. JSON*, zmienne środowiskowe, argumenty wiersza polecenia i inne źródła konfiguracji.
* Wyślij dane wyjściowe rejestrowania do konsoli programu i dostawców debugowania.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.

::: moniker-end

### <a name="non-web-scenarios"></a>Scenariusze inne niż internetowe

Host ogólny umożliwia innym typom aplikacji korzystanie z rozszerzeń struktury wycinania, takich jak rejestrowanie, iniekcja zależności (DI), konfiguracja i zarządzanie okresem istnienia aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host> i <xref:fundamentals/host/hosted-services>.

## <a name="servers"></a>Serwery

Aplikacja ASP.NET Core używa implementacji serwera HTTP do nasłuchiwania żądań HTTP. Serwer wyświetla żądania do aplikacji jako zestaw [funkcji żądania](xref:fundamentals/request-features) złożonych do `HttpContext`.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core udostępnia następujące implementacje serwera:

* *Kestrel* to Międzyplatformowy serwer sieci Web. Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/). W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.
* *Serwer http IIS* jest serwerem dla systemu Windows, który korzysta z usług IIS. Na tym serwerze aplikacja ASP.NET Core i usługi IIS działają w tym samym procesie.
* *Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* . W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie. Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[System](#tab/linux)

ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* . W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie. Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core udostępnia następujące implementacje serwera:

* *Kestrel* to Międzyplatformowy serwer sieci Web. Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy za pomocą [usług IIS](https://www.iis.net/). W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie.
* *Http. sys* to serwer dla systemu Windows, który nie jest używany z usługami IIS.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* . W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie. Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

# <a name="linuxtablinux"></a>[System](#tab/linux)

ASP.NET Core udostępnia międzyplatformową implementację serwera *Kestrel* . W ASP.NET Core 2,0 lub nowszej Kestrel może być uruchamiany jako publiczny serwer graniczny uwidoczniony bezpośrednio w Internecie. Kestrel jest często uruchamiana w odwrotnej konfiguracji serwera proxy z [Nginx](https://nginx.org) lub [Apache](https://httpd.apache.org/).

---

::: moniker-end

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Konfiguracja

ASP.NET Core udostępnia platformę konfiguracji, która pobiera ustawienia jako pary nazwa-wartość z uporządkowanego zestawu dostawców konfiguracji. Istnieją Wbudowani dostawcy konfiguracji dla różnych źródeł, takich jak pliki *. JSON* , pliki *. XML* , zmienne środowiskowe i argumenty wiersza polecenia. Możesz również napisać niestandardowych dostawców konfiguracji.

Można na przykład określić, że konfiguracja pochodzi z pliku *appSettings. JSON* i zmiennych środowiskowych. Następnie po zażądaniu wartości parametru *ConnectionString* struktura najpierw sprawdza plik *appSettings. JSON* . Jeśli wartość jest tam znaleziona, ale również w zmiennej środowiskowej, pierwszeństwo ma wartość ze zmiennej środowiskowej.

Aby zarządzać poufnymi danymi konfiguracyjnymi, takimi jak hasła, ASP.NET Core zapewnia [Narzędzie tajnego Menedżera](xref:security/app-secrets). W przypadku wpisów tajnych produkcji zalecamy [Azure Key Vault](xref:security/key-vault-configuration).

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

## <a name="options"></a>Opcje

Jeśli to możliwe, ASP.NET Core są zgodne ze *wzorcem opcji* przechowywania i pobierania wartości konfiguracyjnych. Wzorzec opcji używa klas do reprezentowania grup powiązanych ustawień.

Na przykład poniższy kod ustawia opcje obiektów WebSockets:

```csharp
var options = new WebSocketOptions  
{  
   KeepAliveInterval = TimeSpan.FromSeconds(120),  
   ReceiveBufferSize = 4096
};  
app.UseWebSockets(options);
```

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/options>.

## <a name="environments"></a>Wiejski

Środowiska wykonawcze, takie jak *programowanie*, *przemieszczanie*i *produkcja*, są pierwszą klasą koncepcji w ASP.NET Core. Aby określić środowisko, w którym działa aplikacja, należy ustawić zmienną środowiskową `ASPNETCORE_ENVIRONMENT`. ASP.NET Core odczytuje tę zmienną środowiskową przy uruchamianiu aplikacji i zapisuje wartość w implementacji `IHostingEnvironment`. Obiekt środowiska jest dostępny w dowolnym miejscu w aplikacji za pomocą funkcji DI.

Następujący przykładowy kod z klasy `Startup` konfiguruje aplikację w celu dostarczania szczegółowych informacji o błędzie tylko wtedy, gdy są uruchamiane w programie Development:

[!code-csharp[](index/snapshots/2.x/Startup2.cs?highlight=3-6)]

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

## <a name="logging"></a>Rejestrowanie

ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnymi dostawcami rejestrowania wbudowanych i innych firm. Dostępne są następujące dostawcy:

* Konsola
* Debugowanie
* Śledzenie zdarzeń w systemie Windows
* Dziennik zdarzeń systemu Windows
* TraceSource
* Azure App Service
* Application Insights platformy Azure

Zapisuj dzienniki z dowolnego miejsca w kodzie aplikacji, pobierając `ILogger` obiekt z metod rejestrowania i wywoływania.

Poniżej przedstawiono przykładowy kod, który używa obiektu `ILogger`, z iniekcją konstruktora i wyróżniania wywołań metody rejestrowania.

[!code-csharp[](index/snapshots/2.x/TodoController.cs?highlight=5,13,17)]

Interfejs `ILogger` umożliwia przekazanie dowolnej liczby pól dostawcy rejestrowania. Pola są często używane do konstruowania ciągu komunikatu, ale dostawcy mogą również wysyłać je jako oddzielne pola do magazynu danych. Ta funkcja umożliwia dostawcom rejestrowania implementowanie [rejestrowania semantycznego, znanego również jako rejestrowanie strukturalne](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.

## <a name="routing"></a>Routing

*Trasa* jest WZORCEM adresu URL, który jest mapowany do procedury obsługi. Procedura obsługi jest zazwyczaj stroną Razor, metodą akcji w kontrolerze MVC lub w oprogramowaniu pośredniczącym. Routing ASP.NET Core zapewnia kontrolę nad adresami URL używanymi przez aplikację.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.

## <a name="error-handling"></a>Obsługa błędów

ASP.NET Core ma wbudowane funkcje do obsługi błędów, takie jak:

* Strona wyjątków dla deweloperów
* Niestandardowe strony błędów
* Statyczne strony kodów stanu
* Obsługa wyjątków uruchamiania

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.

## <a name="make-http-requests"></a>Zgłaszanie żądań HTTP

Implementacja `IHttpClientFactory` jest dostępna do tworzenia wystąpień `HttpClient`. Fabryka:

* Zapewnia centralną lokalizację do nazywania i konfigurowania wystąpień `HttpClient` logicznych. Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi GitHub. Domyślny klient można zarejestrować do innych celów.
* Obsługuje rejestrację i łańcuch wielu procedur delegowania, aby utworzyć potok pośredniczący żądania wychodzącego. Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core. Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.
* Integruje się z usługą *Polly*, popularną biblioteką innej firmy na potrzeby obsługi błędów przejściowych.
* Zarządza buforowaniem i okresem istnienia podstawowych wystąpień `HttpClientMessageHandler`, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresów istnienia.
* Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem `ILogger`) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.

## <a name="content-root"></a>Katalog główny zawartości

Katalog główny zawartości jest ścieżką podstawową do:

* Plik wykonywalny obsługujący aplikację ( *. exe*).
* Skompilowane zestawy wchodzące w skład aplikacji ( *. dll*).
* Pliki zawartości inne niż kod używane przez aplikację, takie jak:
  * Pliki Razor ( *. cshtml*, *. Razor*)
  * Pliki konfiguracji ( *. JSON*, *. XML*)
  * Pliki danych ( *. DB*)
* [Katalog główny sieci Web](#web-root), zazwyczaj opublikowany folder *wwwroot* .

Podczas tworzenia:

* Katalog główny zawartości domyślnie jest katalogiem głównym projektu.
* Katalog główny projektu służy do tworzenia:
  * Ścieżka do plików zawartości nienależących do kodu aplikacji w katalogu głównym projektu.
  * [Katalog główny sieci Web](#web-root), zazwyczaj folder *wwwroot* w katalogu głównym projektu.

::: moniker range=">= aspnetcore-3.0"

Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host). Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#contentrootpath>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Alternatywna ścieżka katalogu głównego zawartości może być określona podczas [kompilowania hosta](#host). Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host#content-root>.

::: moniker-end

## <a name="web-root"></a>Katalog główny sieci Web

Katalog główny sieci Web jest ścieżką podstawową do publicznych, niekodowych, statycznych plików zasobów, takich jak:

* Arkusze stylów ( *. css*)
* JavaScript ( *. js*)
* Obrazy ( *. png*, *. jpg*)

Pliki statyczne są obsługiwane domyślnie tylko z katalogu głównego (i katalogów podrzędnych) sieci Web.

::: moniker range=">= aspnetcore-3.0"

Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host#webroot>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ścieżka katalogu głównego sieci Web jest domyślnie ustawiona na *{Content root}/wwwroot*, ale podczas [kompilowania hosta](#host)można określić inny katalog internetowy w sieci Web. Aby uzyskać więcej informacji, zobacz [katalog główny sieci Web](xref:fundamentals/host/web-host#web-root).

::: moniker-end

Zapobiegaj publikowaniu plików w pliku *wwwroot* przy użyciu [elementu projektu\<Content >](/visualstudio/msbuild/common-msbuild-project-items#content) . Poniższy przykład uniemożliwia opublikowanie zawartości w katalogu *wwwroot/lokalnym* i podkatalogach:

```xml
<ItemGroup>
  <Content Update="wwwroot\local\**\*.*" CopyToPublishDirectory="Never" />
</ItemGroup>
```

W plikach Razor ( *. cshtml*), ukośnik (`~/`) wskazuje na katalog główny sieci Web. Ścieżka rozpoczynająca się od `~/` jest nazywana *ścieżką wirtualną*.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.
