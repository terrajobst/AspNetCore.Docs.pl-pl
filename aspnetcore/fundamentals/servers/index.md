---
title: Implementacje serwera sieci Web w ASP.NET Core
author: rick-anderson
description: Wykrywa Kestrel i HTTP.sys serwerów sieci web dla platformy ASP.NET Core. Dowiedz się, wybierz serwer, jak i kiedy należy używać serwera zwrotnego serwera proxy.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: 38af9d0206d66ac7fd2dc13a5a8245e8f66df41e
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementacje serwera sieci Web w ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), i [Roaming Krzysztof](https://github.com/Tratcher)

Uruchamia aplikację platformy ASP.NET Core z implementację serwera HTTP w procesie. Implementacja serwera nasłuchuje żądań HTTP, a udostępnia je w aplikacji jako zestawy [żądania funkcji](xref:fundamentals/request-features) składane do [element HttpContext](/dotnet/api/system.web.httpcontext).

Platformy ASP.NET Core dostarczany dwóch implementacji serwera:

* [Kestrel](xref:fundamentals/servers/kestrel) jest domyślnie, serwer HTTP i platform dla platformy ASP.NET Core.
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP systemu Windows opiera się na [sterownik HTTP.sys jądra i interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w ASP.NET Core 1.x.)

## <a name="kestrel"></a>Kestrel

Kestrel jest domyślny serwer sieci web zawarte w szablonach projektu platformy ASP.NET Core.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel można samodzielnie lub za pomocą *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Albo konfiguracji &mdash; z lub bez zwrotnego serwera proxy &mdash; można również, czy Kestrel jest dostępne tylko z siecią wewnętrzną.

Aby uzyskać informacje, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli aplikacja będzie akceptować tylko żądania z siecią wewnętrzną, Kestrel można samodzielnie.

![Kestrel komunikuje się bezpośrednio z siecią wewnętrzną](kestrel/_static/kestrel-to-internal.png)

Jeśli aplikacja jest połączenie z Internetem, Kestrel musi używać usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi, jak pokazano na poniższym diagramie:

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Najważniejsze przyczyny wdrożeń krawędzi (ujawniony na ruch z Internetu) przy użyciu zwrotnego serwera proxy jest zabezpieczeń. Wersje 1.x Kestrel nie mają ważne funkcje zabezpieczeń do ochrony przed atakami z Internetu. Obejmuje, ale nie jest ograniczony do odpowiednich przekroczenia limitu czasu, limity rozmiaru żądania oraz limity jednoczesnych połączeń.

Aby uzyskać informacje, zobacz [użycie Kestrel z zwrotny serwer proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

---

Nie można używać usług IIS, Nginx i Apache bez Kestrel lub [implementacji niestandardowego serwera](#custom-servers). Platformy ASP.NET Core był przeznaczony do działania w swoim własnym procesie, tak, aby mogła działać spójnie na platformach. Usługi IIS, Nginx i Apache określają własne procedury uruchamiania i środowiska. Do używania tych technologii serwerowych bezpośrednio, platformy ASP.NET Core należy dostosować do wymagań każdego serwera. Przy użyciu implementacji serwera sieci web, takich jak Kestrel, platformy ASP.NET Core ma kontroli nad procesu uruchamiania i środowiska podczas hostowanych na inny serwer technologii.

### <a name="iis-with-kestrel"></a>Usługi IIS z Kestrel

Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) jako zwrotny serwer proxy dla platformy ASP.NET Core aplikacji platformy ASP.NET Core działa w procesie niezależnie od proces roboczy usług IIS. W procesie IIS [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) koordynuje relacji zwrotnego serwera proxy. Podstawowe funkcje platformy ASP.NET Core modułu są do uruchamiania aplikacji platformy ASP.NET Core, uruchom ponownie aplikację uległa awarii i przesyłał dalej ruch HTTP do aplikacji. Aby uzyskać więcej informacji, zobacz [platformy ASP.NET Core modułu](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Nginx z Kestrel

Aby uzyskać informacje dotyczące sposobu używania Nginx w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [hosta w systemie Linux z Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache z Kestrel

Aby uzyskać informacje dotyczące sposobu używania Apache w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [hosta w systemie Linux z Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli aplikacje ASP.NET Core są uruchamiane w systemie Windows, sterownik HTTP.sys stanowi alternatywę Kestrel. Ogólnie zaleca się kestrel w celu uzyskania najlepszej wydajności. Sterownik HTTP.sys może służyć w scenariuszach, w którym aplikacja jest połączenie z Internetem i wymagane funkcje są obsługiwane przez sterownik HTTP.sys, ale nie Kestrel. Informacji o funkcjach HTTP.sys, zobacz [HTTP.sys](xref:fundamentals/servers/httpsys).

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

Można także HTTP.sys dla aplikacji, które są dostępne tylko z siecią wewnętrzną. 

![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w ASP.NET Core 1.x. Jeśli aplikacje ASP.NET Core są uruchamiane w systemie Windows, WebListener to alternatywa dla scenariuszy, w której usługi IIS nie jest dostępne dla hosta aplikacji.

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

WebListener można również zamiast Kestrel dla aplikacji, które są dostępne tylko z siecią wewnętrzną, czy wymagane funkcje są obsługiwane przez WebListener, ale nie Kestrel. Aby uzyskać informacje na temat funkcji WebListener, zobacz [WebListener](xref:fundamentals/servers/weblistener).

![Weblistener komunikuje się bezpośrednio z siecią wewnętrzną](weblistener/_static/weblistener-to-internal.png)

---

## <a name="aspnet-core-server-infrastructure"></a>Platformy ASP.NET Core serwera infrastruktury

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) dostępne w `Startup.Configure` ujawnia metody [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) właściwości typu [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel i sterownik HTTP.sys (WebListener w ASP.NET Core 1.x) udostępniają tylko jednej funkcji, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementacje inny serwer może udostępniać dodatkowe funkcje.

`IServerAddressesFeature` można sprawdzić, port, który implementacją serwera został powiązany w czasie wykonywania.

## <a name="custom-servers"></a>Niestandardowe serwerów

Jeśli wbudowane serwery nie spełniają wymagania dotyczące aplikacji, można utworzyć implementacji niestandardowego serwera. [Interfejsu Open Web dla platformy .NET (OWIN) przewodnik](xref:fundamentals/owin) pokazano, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementacji. Tylko interfejsy funkcji, które aplikacja korzysta z wymagają wykonania, jeśli co najmniej [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) i [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) muszą być obsługiwane.

## <a name="server-startup"></a>Uruchamianie serwera

Korzystając z [programu Visual Studio](https://www.visualstudio.com/vs/), [programu Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), lub [Visual Studio Code](https://code.visualstudio.com/), serwer jest uruchamiany po uruchomieniu aplikacji przez (zintegrowane środowisko projektowe IDE). W programie Visual Studio w systemie Windows, profile uruchamiania może służyć do Uruchom aplikację i serwera przy użyciu jednej [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) lub konsoli. W programie Visual Studio Code, aplikacji i serwerze są uruchomione przez [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), który uaktywnia debugera środowisko CoreCLR. Za pomocą programu Visual Studio dla komputerów Mac, aplikacji i serwerze są uruchomione przez [Mono Soft-Tryb debugera](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Podczas uruchamiania aplikacji z wiersza polecenia w folderze projektu [dotnet Uruchom](/dotnet/core/tools/dotnet-run) uruchamia aplikację i serwer (Kestrel i tylko w pliku HTTP.sys). Konfiguracja jest określona przez `-c|--configuration` opcja, która jest ustawiona jako `Debug` (ustawienie domyślne) lub `Release`. Jeśli profile uruchamiania znajdują się w *launchSettings.json* plików, użyj `--launch-profile <NAME>` możliwość ustawienia profilu uruchamiania (na przykład `Development` lub `Production`). Aby uzyskać więcej informacji, zobacz [dotnet Uruchom](/dotnet/core/tools/dotnet-run) i [pakowania dystrybucji .NET Core](/dotnet/core/build/distribution-packaging) tematów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Kestrel z usługami IIS](xref:fundamentals/servers/aspnet-core-module)
* [Hosting w systemie Linux z Nginx](xref:host-and-deploy/linux-nginx)
* [Hosting w systemie Linux z Apache](xref:host-and-deploy/linux-apache)
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) (dla platformy ASP.NET Core 1.x, zobacz [WebListener](xref:fundamentals/servers/weblistener))
