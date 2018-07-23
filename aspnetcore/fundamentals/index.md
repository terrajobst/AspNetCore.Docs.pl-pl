---
title: Podstawy platformy ASP.NET Core
author: rick-anderson
description: Poznaj podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 07/02/2018
uid: fundamentals/index
ms.openlocfilehash: 30c456685ce26522faff9b58fbd2977ad2f2869a
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202630"
---
# <a name="aspnet-core-fundamentals"></a>Podstawy platformy ASP.NET Core

Aplikacji ASP.NET Core jest aplikacją konsoli, która tworzy serwer sieci web w jego `Main` metody:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` Wywołuje metodę `WebHost.CreateDefaultBuilder`, która jest zgodna z wzorcem konstruktora w celu utworzenia hosta aplikacji sieci web. Konstruktor ma metody definiujące serwer sieci web (na przykład `UseKestrel`) i Klasa początkowa (`UseStartup`). W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) serwera sieci web jest przydzielany automatycznie. Host sieci web platformy ASP.NET Core próbuje uruchomić na serwerze IIS, jeśli jest dostępny. Inne serwery sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody rozszerzenia odpowiednie. `UseStartup` opisano w następnej sekcji.

`IWebHostBuilder`, zwracany typ `WebHost.CreateDefaultBuilder` wywołania, oferuje wiele metod opcjonalne. Oto niektóre z tych metod `UseHttpSys` do hostowania aplikacji w pliku HTTP.sys i `UseContentRoot` do określania zawartości katalogu głównego. `Build` i `Run` metod tworzenia `IWebHost` obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` Metoda używa `WebHostBuilder`, który jest zgodny ze wzorcem konstruktora w celu utworzenia hosta aplikacji sieci web. Konstruktor ma metody definiujące serwer sieci web (na przykład `UseKestrel`) i Klasa początkowa (`UseStartup`). W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) używany jest serwer sieci web. Inne serwery sieci web, takich jak [WebListener](xref:fundamentals/servers/weblistener), mogą być używane przez wywołanie metody rozszerzenia odpowiednie. `UseStartup` opisano w następnej sekcji.

`WebHostBuilder` udostępnia wiele metod opcjonalne, w tym `UseIISIntegration` do hostowania w usługach IIS i usług IIS Express i `UseContentRoot` do określania zawartości katalogu głównego. `Build` i `Run` metod tworzenia `IWebHost` obiektu, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.

---

## <a name="startup"></a>Uruchamianie

`UseStartup` Metody `WebHostBuilder` Określa `Startup` klasy dla twojej aplikacji:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` Klasa jest gdzie definiuje się Potok żądań obsługi i którym skonfigurowano wszystkich usług wymaganych przez aplikację. `Startup` Klasy musi być publiczna i może zawierać następujących metod:

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` definiuje [usług](#dependency-injection-services) używanych przez aplikację (na przykład, ASP.NET Core MVC, platformy Entity Framework Core, tożsamość). `Configure` definiuje [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) dla żądania potoku.

Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

## <a name="content-root"></a>Zawartość katalogu głównego

Główny zawartości jest ścieżka podstawowa do żadnej zawartości, używanych przez aplikację, takie jak widoki, [stron Razor](xref:razor-pages/index), a zasoby statyczne. Domyślnie zawartość katalogu głównego jest taka sama jak podstawowa ścieżka aplikacji dla pliku wykonywalnego hostingu aplikacji.

## <a name="web-root"></a>Katalog główny sieci Web

Katalog główny aplikacji sieci web jest to katalog, w do projektu zawierającego zasoby publicznej, statycznej, takie jak CSS, JavaScript i plików obrazów.

## <a name="dependency-injection-services"></a>Wstrzykiwanie zależności (usługi)

Usługa jest składnikiem, który jest przeznaczony do wspólnej wykorzystania w aplikacji. Usługi są udostępniane za pośrednictwem [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection). Platforma ASP.NET Core zawiera natywny **I**nWymagana **o**f **C**kontener ontrola (IoC), który obsługuje [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) domyślnie. W razie potrzeby można zastąpić domyślny kontener natywnych. Oprócz korzyści z jej Poluzuj powiązania DI sprawia, że usługi dostępne w całej aplikacji (na przykład [rejestrowania](xref:fundamentals/logging/index)).

Aby uzyskać więcej informacji, zobacz [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Oprogramowanie pośredniczące

W programie ASP.NET Core redagowania Twojego żądania potoku przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/middleware/index). Oprogramowanie pośredniczące platformy ASP.NET Core wykonuje logiki przetwarzania asynchronicznego `HttpContext` i następnie wywoła następne oprogramowanie pośredniczące w sekwencji lub kończy żądanie bezpośrednio. Składnik oprogramowania pośredniczącego o nazwie "XYZ", jest dodawany przez wywołanie `UseXYZ` metody rozszerzenia w `Configure` metody.

Platforma ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego:

* [Pliki statyczne](xref:fundamentals/static-files)
* [Routing](xref:fundamentals/routing)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression)
* [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting)

[OWIN](http://owin.org)— oparte na oprogramowaniu pośredniczącym jest dostępna dla aplikacji platformy ASP.NET Core i można napisać własne niestandardowe oprogramowanie pośredniczące.

Aby uzyskać więcej informacji, zobacz [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) i [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Inicjowanie żądań HTTP

Informacji o używaniu `IHttpClientFactory` dostęp do `HttpClient` wystąpień do wysyłania żądań HTTP, zobacz [żądań HTTP zainicjować](xref:fundamentals/http-requests).

::: moniker-end

## <a name="environments"></a>Środowiska

Środowiskach, takich jak "Programowanie" i "Produkcyjne" to najwyższej jakości pojęcie w programie ASP.NET Core i można ustawić za pomocą zmiennych środowiskowych.

Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments).

## <a name="configuration"></a>Konfiguracja

Platforma ASP.NET Core używa modelu konfiguracji na podstawie par nazwa wartość. Model konfiguracji nie jest oparty na `System.Configuration` lub *web.config*. Konfiguracja pobiera ustawienia z uporządkowany zestaw dostawców konfiguracji. Dostawcy wbudowana Konfiguracja obsługują różne formaty plików (XML, JSON, INI) i zmiennych środowiskowych w celu włączenia konfiguracji opartej na środowisku. Można również napisać własne dostawców konfiguracji niestandardowej.

Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="logging"></a>Rejestrowanie

Platforma ASP.NET Core obsługuje interfejs API rejestrowania, która współdziała z różnych dostawców logowania. Wbudowani dostawcy obsługuje wysyłanie dzienników do jednego lub więcej miejsc docelowych. Może służyć struktur rejestrowania innych firm.

Aby uzyskać więcej informacji, zobacz [rejestrowania](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Obsługa błędów

Platforma ASP.NET Core ma wbudowane funkcje obsługi błędów w aplikacji, w tym stroną wyjątków dla deweloperów, strony błędów niestandardowych, stron kodowych stan statycznych i uruchamiania obsługi wyjątków.

Aby uzyskać więcej informacji, zobacz [sposób obsługi błędów](xref:fundamentals/error-handling).

## <a name="routing"></a>Routing

Platforma ASP.NET Core oferuje funkcje routingu żądań aplikacji do obsługi trasy.

Aby uzyskać więcej informacji, zobacz [Routing](xref:fundamentals/routing).

## <a name="file-providers"></a>Dostawcy plików

Platforma ASP.NET Core przenosi dostępu do systemu plików przy użyciu dostawcy plików, która zapewnia wspólny interfejs do pracy z plikami na wielu platformach.

Aby uzyskać więcej informacji, zobacz [dostawcy plików](xref:fundamentals/file-providers).

## <a name="static-files"></a>Pliki statyczne

Oprogramowanie pośredniczące plików statycznych służy plików statycznych, takich jak HTML, CSS, obrazów i JavaScript.

Aby uzyskać więcej informacji, zobacz [pliki statyczne](xref:fundamentals/static-files).

## <a name="hosting"></a>Hosting

Konfigurowanie aplikacji platformy ASP.NET Core i uruchamiania *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okres istnienia aplikacji.

Aby uzyskać więcej informacji, zobacz [hostów w programie ASP.NET Core](xref:fundamentals/host/index).

## <a name="session-and-app-state"></a>Stan sesji i aplikacji

Platforma ASP.NET Core oferuje kilka metod, aby zachować stan sesji i aplikacji, gdy użytkownik przegląda aplikacji sieci web.

Aby uzyskać więcej informacji, zobacz [stan sesji i aplikacji](xref:fundamentals/app-state).

## <a name="servers"></a>Serwery

Model hostingu w programie ASP.NET Core bezpośrednio nie nasłuchuje żądań. Model hostingu opiera się na implementację serwera HTTP do przekazywania żądań do aplikacji. Przekazane żądanie opakowaniu jako zbiór obiektów funkcji, które mogą być udostępniane za pośrednictwem interfejsów. Platforma ASP.NET Core obejmuje serwera zarządzanego dla wielu platform sieci web, nazywanego [Kestrel](xref:fundamentals/servers/kestrel). Kestrel jest często uruchamiać za produkcyjnym serwerze sieci web, takich jak [IIS](https://www.iis.net/) lub [Nginx](http://nginx.org). Kestrel może działać jako serwer graniczny.

Aby uzyskać więcej informacji, zobacz [serwerów](xref:fundamentals/servers/index) i następujące tematy:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Moduł ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej noszącą nazwę [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Globalizacja i lokalizacja

Tworzenie wielojęzycznych witryny sieci Web za pomocą programu ASP.NET Core umożliwia witryny dotrzeć do większej liczby osób. ASP.NET Core oferuje usługi oraz oprogramowanie pośredniczące lokalizowanie w różnych językach i kultur.

Aby uzyskać więcej informacji, zobacz [lokalizacja i globalizacja](xref:fundamentals/localization).

## <a name="request-features"></a>Funkcje na żądanie

Szczegóły implementacji serwera sieci Web związanych z żądań HTTP i odpowiedzi są zdefiniowane w interfejsach. Te interfejsy są używane przez implementacje serwera i oprogramowania pośredniczącego do tworzenia i modyfikowania potoku hostingu aplikacji.

Aby uzyskać więcej informacji, zobacz [żądania funkcji](xref:fundamentals/request-features).

## <a name="background-tasks"></a>Zadania w tle

Zadania w tle są implementowane jako *usługi hostowane*. Usługa hostowana jest klasą z logiką zadań tła, który implementuje [pomocą interfejsu IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interfejsu.

Aby uzyskać więcej informacji, zobacz [zadania z usługami hostowanymi w tle](xref:fundamentals/host/hosted-services).

## <a name="access-httpcontext"></a>Dostęp do obiektu HttpContext

Dostęp do `HttpContext` za pośrednictwem [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interfejsu i jego domyślna implementacja [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).

Aby uzyskać więcej informacji, zobacz <xref:fundamentals/httpcontext>.

## <a name="open-web-interface-for-net-owin"></a>Otwarty interfejs internetowy dla platformy .NET (OWIN)

Platforma ASP.NET Core obsługuje Open Web Interface for .NET (OWIN). OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.

Aby uzyskać więcej informacji, zobacz [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>Funkcja WebSockets

[WebSocket](https://wikipedia.org/wiki/WebSocket) to protokół, który umożliwia kanały komunikacja dwukierunkowa trwałego połączenia protokołu TCP. Jest używana w przypadku aplikacji, takie jak rozmowy, giełdowych, gry i dowolnym miejscu działanie funkcji w czasie rzeczywistym w aplikacji sieci web. Platforma ASP.NET Core obsługuje funkcje gniazda sieci web.

Aby uzyskać więcej informacji, zobacz [WebSockets](xref:fundamentals/websockets).

::: moniker range=">= aspnetcore-2.1"
## <a name="microsoftaspnetcoreapp-metapackage"></a>Microsoft.AspNetCore.App meta Microsoft.aspnetcore.all

[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) meta Microsoft.aspnetcore.all upraszcza zarządzanie pakietami. Aby uzyskać więcej informacji, zobacz [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end
::: moniker range="= aspnetcore-2.0"
## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All metapackage

[Pakiet](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta Microsoft.aspnetcore.all dla platformy ASP.NET Core obejmuje:

* Wszystkie obsługiwane pakiety przez zespół programu ASP.NET Core.
* We wszystkich obsługiwanych pakietów w kolejności od platformy Entity Framework Core.
* Zależności wewnętrzne i firm 3 używane przez program ASP.NET Core i Entity Framework Core.

Aby uzyskać więcej informacji, zobacz [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).
::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>Środowisko uruchomieniowe programu .NET core i .NET Framework

Aplikacji ASP.NET Core mogą określać docelową środowisko uruchomieniowe platformy .NET Core lub .NET Framework.

Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Wybieranie między platformą ASP.NET Core i ASP.NET

Aby uzyskać więcej informacji na temat wybierania między platformą ASP.NET Core i ASP.NET, zobacz [wybór między platformą ASP.NET Core i ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
