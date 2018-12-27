---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/index
ms.openlocfilehash: 11dc6336ae7667038983c967f28232bef325f5bb
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637773"
---
# <a name="aspnet-core-fundamentals"></a>Podstawy platformy ASP.NET Core

Aplikacji ASP.NET Core jest aplikacją konsoli, która tworzy serwer sieci web w jego `Program.Main` metody. `Main` Metody jest to aplikacja *zarządzany punkt wejścia*:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

.NET Core hosta:

* Ładunki [środowisko uruchomieniowe programu .NET Core](https://github.com/dotnet/coreclr).
* Korzysta z pierwszym argumentem wiersza polecenia jako ścieżka do zarządzanej plik binarny, który zawiera punkt wejścia (`Main`) i rozpoczyna wykonywanie kodu.

`Main` Wywołuje metodę [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), zgodny [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern) w celu utworzenia hosta sieci web. Konstruktor ma metody definiujące serwera sieci web (na przykład <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) i Klasa początkowa (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web jest przydzielany automatycznie. Host sieci web platformy ASP.NET Core podejmie próbę uruchomienia [Internet Information Services (IIS)](https://www.iis.net/), jeśli jest dostępna. Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie. `UseStartup` zostało wyjaśnione w dalszej [uruchamiania](#startup) sekcji.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, zwracany typ `WebHost.CreateDefaultBuilder` wywołania, oferuje wiele metod opcjonalne. Oto niektóre z tych metod `UseHttpSys` do hostowania aplikacji w pliku HTTP.sys i <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> do określania zawartości katalogu głównego. <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> i <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> metod tworzenia <xref:Microsoft.AspNetCore.Hosting.IWebHost> obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

.NET Core hosta:

* Ładunki [środowisko uruchomieniowe programu .NET Core](https://github.com/dotnet/coreclr).
* Korzysta z pierwszym argumentem wiersza polecenia jako ścieżka do zarządzanej plik binarny, który zawiera punkt wejścia (`Main`) i rozpoczyna wykonywanie kodu.

`Main` Metoda używa <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, zgodny [wzorzec konstruktora](https://wikipedia.org/wiki/Builder_pattern) w celu utworzenia hosta aplikacji sieci web. Konstruktor ma metody definiujące serwer sieci web (na przykład <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) i Klasa początkowa (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) używany jest serwer sieci web. Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie. `UseStartup` zostało wyjaśnione w dalszej [uruchamiania](#startup) sekcji.

`WebHostBuilder` udostępnia wiele metod opcjonalne, w tym <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> do hostowania w usługach IIS i usług IIS Express i <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> do określania zawartości katalogu głównego. <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> i <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> metod tworzenia <xref:Microsoft.AspNetCore.Hosting.IWebHost> obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.

::: moniker-end

## <a name="startup"></a>Uruchamianie

`UseStartup` Metody `WebHostBuilder` Określa `Startup` klasy dla twojej aplikacji:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

`Startup` Klasa jest gdzie definiuje się Potok żądań obsługi i którym skonfigurowano wszystkich usług wymaganych przez aplikację. `Startup` Klasy musi być publiczna i może zawierać następujących metod:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> definiuje [usług](#dependency-injection-services) używanych przez aplikację (na przykład, ASP.NET Core MVC, platformy Entity Framework Core, tożsamość). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> definiuje [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) o nazwie w Potok żądań.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

## <a name="content-root"></a>Zawartość katalogu głównego

Główny zawartości jest ścieżka podstawowa do żadnej zawartości, używanych przez aplikację, takie jak [stron Razor](xref:razor-pages/index), MVC widoków i statyczne elementy zawartości. Domyślnie zawartość katalogu głównego jest tej samej lokalizacji co aplikację podstawową ścieżkę dla pliku wykonywalnego, hostowanie aplikacji.

## <a name="web-root-webroot"></a>Katalog główny sieci Web (webroot)

Webroot aplikacji jest to katalog, w do projektu zawierającego zasoby publicznej, statycznej, takie jak CSS, JavaScript i plików obrazów. Domyślnie *wwwroot* jest webroot.

Dla elementu Razor (*.cshtml*) plików ukośnika tylda `~/` wskazuje webroot. Począwszy od ścieżki `~/` są określane jako ścieżek wirtualnych.

## <a name="dependency-injection-services"></a>Wstrzykiwanie zależności (usługi)

A *usługi* to składnik, który jest przeznaczony do wspólnej wykorzystania w aplikacji. Usługi są udostępniane za pośrednictwem [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Platforma ASP.NET Core obejmuje natywne kontener Inwersja kontroli (IoC), który obsługuje [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) domyślnie. Jeśli chcesz, możesz zastąpić domyślny kontener. W uzupełnieniu do jego [luźne sprzężenia korzyści](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI sprawia, że usługi, takie jak [rejestrowania](xref:fundamentals/logging/index), która jest dostępna w całej aplikacji.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>Oprogramowanie pośredniczące

W programie ASP.NET Core redagowania Twojego żądania potoku przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index). Oprogramowanie pośredniczące platformy ASP.NET Core wykonuje operacje asynchroniczne na `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w potoku lub kończy żądanie.

Zgodnie z Konwencją składnik oprogramowania pośredniczącego o nazwie "XYZ" zostanie dodany do potoku za pomocą wywołania `UseXYZ` metody rozszerzenia w `Configure` metody.

Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego i można napisać własne niestandardowe oprogramowanie pośredniczące. [Otwórz interfejs sieci Web platformy .NET (OWIN)](xref:fundamentals/owin), który umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web jest obsługiwana w aplikacji platformy ASP.NET Core.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/middleware/index> i <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Inicjowanie żądań HTTP

<xref:System.Net.Http.IHttpClientFactory> jest dostępna w celu uzyskania dostępu do <xref:System.Net.Http.HttpClient> wystąpień do wysyłania żądań HTTP.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Środowiska

Środowisk, takich jak *rozwoju* i *produkcji*, są najwyższej jakości pojęcie w programie ASP.NET Core i można ustawić przy użyciu zmiennej środowiskowej, plik ustawień i argument wiersza polecenia.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/environments>.

## <a name="hosting"></a>Hosting

Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/index>.

## <a name="servers"></a>Serwery

Model hostingu w programie ASP.NET Core bezpośrednio nie nasłuchuje żądań. Model hostingu opiera się na implementację serwera HTTP do przekazywania żądań do aplikacji.

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Platforma ASP.NET Core oferuje następujące implementacje serwera:

* [Kestrel](xref:fundamentals/servers/kestrel) serwera zarządzanego dla wielu platform sieci web. Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/). Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.
* Serwer HTTP usług IIS (`IISHttpServer`) jest [wewnątrz procesowego](xref:fundamentals/servers/index#in-process-hosting-model) dla usług IIS.
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwer jest serwerem sieci web dla platformy ASP.NET Core na Windows.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera. Kestrel jest serwerem zarządzanego dla wielu platform sieci web. Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera. Kestrel jest serwerem zarządzanego dla wielu platform sieci web. Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/). Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Platforma ASP.NET Core oferuje następujące implementacje serwera:

* [Kestrel](xref:fundamentals/servers/kestrel) serwera zarządzanego dla wielu platform sieci web. Kestrel często jest uruchamiany w konfiguracji zwrotny serwer proxy przy użyciu [IIS](https://www.iis.net/). Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwer jest serwerem sieci web dla platformy ASP.NET Core na Windows.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera. Kestrel jest serwerem zarządzanego dla wielu platform sieci web. Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Używa platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel) implementacji serwera. Kestrel jest serwerem zarządzanego dla wielu platform sieci web. Kestrel często jest uruchamiany w ramach konfiguracji zwrotny serwer proxy przy użyciu [Nginx](http://nginx.org) lub [Apache](https://httpd.apache.org/). Można również uruchomić kestrel jako serwer graniczny publicznymi, bezpośrednie połączenie z Internetem, ASP.NET Core w wersji 2.0 lub nowszej.

---

::: moniker-end

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Konfiguracja

Platforma ASP.NET Core używa modelu konfiguracji na podstawie par nazwa wartość. Model konfiguracji nie jest oparty na <xref:System.Configuration> lub *web.config*. Konfiguracja pobiera ustawienia z uporządkowany zestaw dostawców konfiguracji. Dostawcy wbudowana Konfiguracja obsługują różne formaty plików (XML, JSON, INI), zmienne środowiskowe i argumenty wiersza polecenia. Można również napisać własne dostawców konfiguracji niestandardowej.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Rejestrowanie

Platforma ASP.NET Core obsługuje rejestrowanie interfejsu API, który współdziała z różnych dostawców logowania. Wbudowani dostawcy obsługuje wysyłanie dzienników do jednego lub więcej miejsc docelowych. Może służyć struktur rejestrowania innych firm.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Obsługa błędów

Platforma ASP.NET Core ma wbudowane scenariusze obsługi błędów w aplikacji, w tym stroną wyjątków dla deweloperów, strony błędów niestandardowych, stron kodowych stan statycznych i uruchamiania obsługi wyjątków.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/error-handling>.

## <a name="routing"></a>Routing

Platforma ASP.NET Core oferuje scenariusze routing żądań aplikacji do obsługi trasy.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/routing>.

## <a name="background-tasks"></a>Zadania w tle

Zadania w tle są implementowane jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań tła, który implementuje <xref:Microsoft.Extensions.Hosting.IHostedService> interfejsu.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Dostęp do obiektu HttpContext

`HttpContext` podczas przetwarzania żądań ze stronami Razor i MVC jest automatycznie dostępny. W sytuacjach gdzie `HttpContext` nie jest łatwo dostępne, możesz uzyskać dostęp `HttpContext` za pośrednictwem <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interfejsu i jego domyślna implementacja <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/httpcontext>.
