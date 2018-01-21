---
title: Podstawowe informacje na temat platformy ASP.NET Core
author: rick-anderson
description: "Poznaj podstawowe pojęcia do tworzenia aplikacji platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d977c13eb5f4cbe8bac261733bdc747e6c19b2a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-fundamentals"></a>Podstawowe informacje na temat platformy ASP.NET Core

Aplikacja platformy ASP.NET Core jest aplikacji konsoli, która tworzy serwer sieci web w jego `Main` metody:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

`Main` Wywołuje metodę `WebHost.CreateDefaultBuilder`, który jest zgodny ze wzorcem Konstruktor do tworzenia host aplikacji sieci web. Konstruktor ma metody, które definiują serwera sieci web (na przykład `UseKestrel`) i Klasa początkowa (`UseStartup`). W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) automatycznie jest przydzielany serwer sieci web. Host sieci web platformy ASP.NET Core próbuje uruchomić na serwerze IIS, jeśli jest dostępna. Inne serwery w sieci web, takich jak [HTTP.sys](xref:fundamentals/servers/httpsys), mogą być używane przez wywołanie metody odpowiednie rozszerzenie. `UseStartup`znajduje się w następnej sekcji.

`IWebHostBuilder`, zwracany typ `WebHost.CreateDefaultBuilder` wywołania, udostępnia wiele metod opcjonalne. Oto niektóre z tych metod `UseHttpSys` do hostowania aplikacji w pliku HTTP.sys i `UseContentRoot` służący do określania zawartości katalogu. `Build` i `Run` metody Konstruuj `IWebHost` obiekt, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

`Main` Używa metody `WebHostBuilder`, który jest zgodny ze wzorcem Konstruktor do tworzenia host aplikacji sieci web. Konstruktor ma metody, które definiują serwera sieci web (na przykład `UseKestrel`) i Klasa początkowa (`UseStartup`). W powyższym przykładzie [Kestrel](xref:fundamentals/servers/kestrel) używany jest serwer sieci web. Inne serwery w sieci web, takich jak [WebListener](xref:fundamentals/servers/weblistener), mogą być używane przez wywołanie metody odpowiednie rozszerzenie. `UseStartup`znajduje się w następnej sekcji.

`WebHostBuilder`udostępnia wiele metod opcjonalne, w tym `UseIISIntegration` dla hostingu w usługach IIS i usług IIS Express i `UseContentRoot` służący do określania zawartości katalogu. `Build` i `Run` metody Konstruuj `IWebHost` obiekt, który hostuje aplikację i rozpoczyna nasłuchiwanie żądań HTTP.

---

## <a name="startup"></a>Uruchamianie

`UseStartup` Metoda `WebHostBuilder` Określa `Startup` klasy dla aplikacji:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

`Startup` Klasa jest służącego do określania potoku żądania obsługi i konfigurowana żadnych usług wymagane przez aplikację. `Startup` Klasy musi być publiczny i może zawierać następujących metod:

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

`ConfigureServices`definiuje [usług](#dependency-injection-services) używany przez aplikację (na przykład ASP.NET Core MVC, Entity Framework Core, tożsamość). `Configure`definiuje [oprogramowanie pośredniczące](xref:fundamentals/middleware) dla żądania potoku.

Aby uzyskać więcej informacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

## <a name="content-root"></a>Główny zawartości

Główny zawartości jest ścieżki podstawowej do dowolnej zawartości używany przez aplikację, takich jak widoki, [stron Razor](xref:mvc/razor-pages/index)i zasoby statyczne. Domyślnie zawartości główny jest taka sama jak podstawowa ścieżka aplikacji dla pliku wykonywalnego hosting aplikacji.

## <a name="web-root"></a>Główny sieci Web

Katalog główny aplikacji sieci web jest katalog projektu zawierającego zasoby publiczne, statyczne, takie jak CSS, JavaScript i plików obrazów.

## <a name="dependency-injection-services"></a>Iniekcji zależności (usługi)

Usługa jest składnikiem, który jest przeznaczony do wspólnego wykorzystania w aplikacji. Usługi są udostępniane za pomocą [iniekcji zależności (Podpisane)](xref:fundamentals/dependency-injection). Platformy ASP.NET Core zawiera natywny **I**nWymagana **o**f **C**kontenera kontrolne (IoC), który obsługuje [iniekcji konstruktora](xref:mvc/controllers/dependency-injection#constructor-injection) domyślnie. W razie potrzeby można zastąpić domyślny kontener macierzystego. Oprócz korzyści luźne powiązanie, Podpisane sprawia, że usługi dostępne w całej aplikacji (na przykład [rejestrowanie](xref:fundamentals/logging/index)).

Aby uzyskać więcej informacji, zobacz [iniekcji zależności](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>Oprogramowanie pośredniczące

W ASP.NET Core redagowania Twojego żądania potoku przy użyciu [oprogramowanie pośredniczące](xref:fundamentals/middleware). Oprogramowanie pośredniczące platformy ASP.NET Core wykonuje asynchroniczne logiki na `HttpContext` , a następnie wywołuje następne oprogramowanie pośredniczące w sekwencji lub bezpośrednio kończy żądanie. Składnik oprogramowania pośredniczącego o nazwie "XYZ" jest dodawany przez wywołanie `UseXYZ` — metoda rozszerzenia w `Configure` metody.

Platformy ASP.NET Core zawiera bogaty zestaw wbudowanych oprogramowania pośredniczącego:

* [Pliki statyczne](xref:fundamentals/static-files)
* [Routing](xref:fundamentals/routing)
* [Uwierzytelnianie](xref:security/authentication/index)
* [Oprogramowanie pośredniczące kompresji odpowiedzi](xref:performance/response-compression)
* [Oprogramowanie pośredniczące ponownego zapisywania adresów URL](xref:fundamentals/url-rewriting)

[OWIN](http://owin.org)— oparte na oprogramowaniu pośredniczącym jest dostępne dla aplikacji platformy ASP.NET Core, a można pisać własne niestandardowe oprogramowania pośredniczącego.

Aby uzyskać więcej informacji, zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware) i [interfejsu Open Web dla platformy .NET (OWIN)](xref:fundamentals/owin).

## <a name="environments"></a>Środowisk

Środowiskach, na przykład "Programowanie" i "Production", są pojęcie pierwszą klasą w ASP.NET Core i można ustawić za pomocą zmiennych środowiskowych.

Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments).

## <a name="configuration"></a>Konfiguracja

Platformy ASP.NET Core wykorzystuje model konfiguracji na podstawie par nazwa wartość. Model konfiguracji nie jest oparty na `System.Configuration` lub *web.config*. Konfiguracja uzyskuje ustawienia z zestawu uporządkowanych dostawców konfiguracji. Dostawców wbudowanych konfiguracji obsługuje wiele formatów (XML, JSON, INI) i zmiennych środowiskowych w celu włączenia konfiguracji opartej na środowisku. Można również napisać własny dostawców niestandardowych konfiguracji.

Aby uzyskać więcej informacji, zobacz [konfiguracji](xref:fundamentals/configuration/index).

## <a name="logging"></a>Rejestrowanie

Platformy ASP.NET Core obsługuje interfejs API rejestrowania, który współpracuje z różnych dostawców rejestrowania. Dostawców wbudowanych obsługuje wysyłania dzienników do jednego lub więcej miejsc docelowych. Można platform rejestrowania innych firm.

[Rejestrowanie](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Obsługa błędów

Platformy ASP.NET Core ma wbudowane funkcje obsługi błędów w aplikacji, w tym strony wyjątek developer, niestandardowe strony błędów stron kodowych statycznych stanu i uruchamiania obsługi wyjątków.

Aby uzyskać więcej informacji, zobacz [obsługi błędu](xref:fundamentals/error-handling).

## <a name="routing"></a>Routing

Platformy ASP.NET Core oferuje funkcje routingu żądań aplikacji do obsługi trasy.

Aby uzyskać więcej informacji, zobacz [Routing](xref:fundamentals/routing).

## <a name="file-providers"></a>Plik dostawców

Platformy ASP.NET Core abstracts dostępu do systemu plików przy użyciu dostawców pliku, który zapewnia wspólny interfejs do pracy z plikami na różnych platformach.

Aby uzyskać więcej informacji, zobacz [dostawców pliku](xref:fundamentals/file-providers).

## <a name="static-files"></a>Pliki statyczne

Oprogramowanie pośredniczące plików statycznych służy plików statycznych, takich jak HTML, CSS, obrazu i JavaScript.

Aby uzyskać więcej informacji, zobacz [Praca z pliki statyczne](xref:fundamentals/static-files).

## <a name="hosting"></a>Hosting

Aplikacje platformy ASP.NET Core skonfigurować i uruchomić *hosta*, który jest odpowiedzialny za zarządzanie uruchamiania i okresem istnienia aplikacji.

Aby uzyskać więcej informacji, zobacz [hostingu](xref:fundamentals/hosting).

## <a name="session-and-application-state"></a>Stan sesji i aplikacji

Stan sesji jest funkcją platformy ASP.NET Core, który służy do zapisywania i przechowywania danych użytkownika, gdy użytkownik będzie przeglądać aplikacji sieci web.

Aby uzyskać więcej informacji, zobacz [sesji i stan aplikacji](xref:fundamentals/app-state).

## <a name="servers"></a>Serwery

Model hostowania w programie ASP.NET Core bezpośrednio nie nasłuchuje żądań. Model hostowania polega na implementację serwera HTTP do przekazywania żądań do aplikacji. Przekazane żądanie jest zawijany jako zestaw obiektów funkcji, które są dostępne za pośrednictwem interfejsów. Serwer sieci web zarządzany, i platform, w nazwie zawiera platformy ASP.NET Core [Kestrel](xref:fundamentals/servers/kestrel). Kestrel jest często wykonywane za produkcyjnym serwerem sieci web, takich jak [IIS](https://www.iis.net/) lub [Nginx](http://nginx.org). Kestrel może działać jako serwer graniczny.

Aby uzyskać więcej informacji, zobacz [serwerów](xref:fundamentals/servers/index) i następujące tematy:

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Moduł ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) (wcześniej nazywanych [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Lokalizacja i globalizacja

Tworzenie wielu języków witryny sieci Web za pomocą platformy ASP.NET Core umożliwia witrynę tak, aby uzyskać dostęp do większej liczby osób. Platformy ASP.NET Core udostępnia usługi i oprogramowanie pośredniczące dla lokalizowanie do różnych języków i kultur.

Aby uzyskać więcej informacji, zobacz [lokalizacja i globalizacja](xref:fundamentals/localization).

## <a name="request-features"></a>Funkcje na żądanie

Szczegóły implementacji serwera sieci Web związanych z żądaniami HTTP i odpowiedzi są zdefiniowane w interfejsach. Te interfejsy są używane przez implementacje serwera i oprogramowanie pośredniczące do tworzenia i modyfikowania procesu hostingu aplikacji.

Aby uzyskać więcej informacji, zobacz [żądania funkcji](xref:fundamentals/request-features).

## <a name="open-web-interface-for-net-owin"></a>Otwórz interfejs sieci Web dla platformy .NET (OWIN)

Platformy ASP.NET Core obsługuje interfejsu Open Web dla platformy .NET (OWIN). OWIN umożliwia aplikacjom sieci web jest całkowicie niezależna od serwerów sieci web.

Aby uzyskać więcej informacji, zobacz [interfejsu Open Web dla platformy .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>Protokół WebSockets

[Protokół WebSocket](https://wikipedia.org/wiki/WebSocket) jest to protokół umożliwiający kanały dwukierunkowej komunikacji trwałego za pośrednictwem połączeń TCP. Służy do aplikacji, takich jak rozmowa, giełdowych, gier i dowolnym wymaganych w czasie rzeczywistym funkcji w aplikacji sieci web. Platformy ASP.NET Core obsługuje funkcje gniazda sieci web.

Aby uzyskać więcej informacji, zobacz [Websocket](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Microsoft.AspNetCore.All metapackage

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage dla platformy ASP.NET Core obejmuje:

* Wszystkie obsługiwane pakiety przez zespół platformy ASP.NET Core.
* Wszystkie obsługiwane pakiety przez Entity Framework Core. 
* Zależności wewnętrzne i firm 3rd używane przez Entity Framework Core i ASP.NET Core.

Aby uzyskać więcej informacji, zobacz [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>Środowisko uruchomieniowe platformy .NET core a .NET Framework

Aplikacja platformy ASP.NET Core można kierować środowiska uruchomieniowego .NET Core lub .NET Framework.

Aby uzyskać więcej informacji, zobacz [wybór między .NET Core i .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Wybór między usługą platformy ASP.NET Core i ASP.NET

Aby uzyskać więcej informacji dotyczących wybierania pomiędzy platformy ASP.NET Core i ASP.NET, zobacz [wybór między platformy ASP.NET Core i ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
