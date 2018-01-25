---
title: Implementacje serwera sieci Web w ASP.NET Core
author: tdykstra
description: "Wprowadza serwerów sieci web Kestrel i WebListener dla platformy ASP.NET Core. Zawiera wskazówki dotyczące sposobu wybierz jedną i kiedy należy użyć jednej z zwrotnego serwera proxy."
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 750b81c2c715598dbcfb6671e34016e30c1878d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementacje serwera sieci Web w ASP.NET Core

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), i [Roaming Krzysztof](https://github.com/Tratcher)

Aplikacja platformy ASP.NET Core jest uruchamiana z implementację serwera HTTP w procesie. Nasłuchuje implementację serwera HTTP żądania i udostępnia je do aplikacji jako zestawy [żądania funkcji](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) składające się na `HttpContext`.

Platformy ASP.NET Core dostarczany dwóch implementacji serwera:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [Kestrel](kestrel.md) serwera HTTP i platform opiera się na [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.

* [Sterownik HTTP.sys](httpsys.md) serwera HTTP systemu Windows opiera się na [sterownik Http.Sys jądra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [Kestrel](kestrel.md) serwera HTTP i platform opiera się na [libuv](https://github.com/libuv/libuv), biblioteki i platform asynchroniczne We/Wy.

* [WebListener](weblistener.md) serwera HTTP systemu Windows opiera się na [sterownik Http.Sys jądra](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

---

## <a name="kestrel"></a>Kestrel

Kestrel to serwer sieci web, który jest domyślnie włączone w szablonach nowy projekt platformy ASP.NET Core. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Samodzielnie lub z użyciem Kestrel *zwrotnego serwera proxy*, takie jak usługi IIS, Nginx lub Apache. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi.

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Albo konfiguracji &mdash; z lub bez zwrotnego serwera proxy &mdash; można również, czy Kestrel jest udostępniany tylko z siecią wewnętrzną.

Aby uzyskać informacje o tym, kiedy używać Kestrel z zwrotny serwer proxy, zobacz [wprowadzenie do Kestrel](kestrel.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Jeśli aplikacja akceptuje żądania tylko z siecią wewnętrzną, można użyć Kestrel przez samego siebie.

![Kestrel komunikuje się bezpośrednio z sieci wewnętrznej](kestrel/_static/kestrel-to-internal.png)

Jeśli udostępnianie aplikacji z Internetem, należy użyć usług IIS, Nginx lub Apache jako *zwrotnego serwera proxy*. Zwrotnego serwera proxy odbiera żądania HTTP z Internetem i przekazuje je do Kestrel po niektórych wstępne obsługi, jak pokazano na poniższym diagramie.

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotnego serwera proxy, na przykład Nginx, Apache lub programu IIS](kestrel/_static/kestrel-to-internet.png)

Najważniejsze przyczyny wdrożeń krawędzi (ujawniony na ruch z Internetu) przy użyciu zwrotnego serwera proxy jest zabezpieczeń. Wersje 1.x Kestrel nie mają pełny zestaw zabezpieczenia przed atakami. Obejmuje, ale nie jest ograniczony do odpowiednich limitów czasu, limity rozmiaru i limity liczby jednoczesnych połączeń.

Aby uzyskać informacje o tym, kiedy używać Kestrel z zwrotny serwer proxy, zobacz [wprowadzenie do Kestrel](kestrel.md).

---

Nie można użyć usług IIS, Nginx lub Apache bez Kestrel lub [implementacji niestandardowego serwera](#custom-servers). Platformy ASP.NET Core był przeznaczony do działania w swoim własnym procesie, tak, aby mogła działać spójnie na platformach. Usługi IIS, Nginx i Apache dyktowania własne procesu uruchamiania i środowiska; z nich korzystać bezpośrednio, platformy ASP.NET Core musi dostosowania do potrzeb każdej z nich. Przy użyciu implementacji serwera sieci web, takich jak Kestrel zapewnia platformy ASP.NET Core kontrolę nad procesu uruchamiania i środowiska. Dlatego zamiast w trakcie dostosowania platformy ASP.NET Core internetowych usług informacyjnych, Nginx lub Apache, tylko konfigurowania tych serwerów sieci web do serwera proxy żądań Kestrel. Umożliwia to rozmieszczenie Twojej `Program.Main` i `Startup` klasy były zasadniczo taki sam, niezależnie od tego, w którym jest wdrażany.

### <a name="iis-with-kestrel"></a>Usługi IIS z Kestrel

Korzystając z usług IIS lub usług IIS Express jako zwrotny serwer proxy dla platformy ASP.NET Core, aplikacji platformy ASP.NET Core uruchamia się w oddzielnej procesu z proces roboczy usług IIS. W procesie IIS specjalne Moduł IIS uruchamia do koordynowania relacji zwrotnego serwera proxy.  Jest to *platformy ASP.NET Core modułu*. Podstawowe funkcje platformy ASP.NET Core modułu są do uruchamiania aplikacji platformy ASP.NET Core, uruchom go ponownie uległa awarii i przesyłania dalej ruchu HTTP. Aby uzyskać więcej informacji, zobacz [platformy ASP.NET Core modułu](aspnet-core-module.md). 

### <a name="nginx-with-kestrel"></a>Nginx z Kestrel

Aby uzyskać informacje o sposobie używania Nginx w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [hosta w systemie Linux z Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache z Kestrel

Aby uzyskać informacje o sposobie używania Apache w systemie Linux jako zwrotnego serwera proxy dla Kestrel, zobacz [hosta w systemie Linux z Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli uruchamianie aplikacji platformy ASP.NET Core w systemie Windows, sterownik HTTP.sys stanowi alternatywę Kestrel. Sterownik HTTP.sys służy do scenariuszy, w którym udostępnianie aplikacji z Internetem i potrzebne funkcje HTTP.sys, które Kestrel nie są obsługiwane. 

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetem](httpsys/_static/httpsys-to-internet.png)

Można także HTTP.sys dla aplikacji, które są dostępne tylko z siecią wewnętrzną. 

![Sterownik HTTP.sys komunikuje się bezpośrednio z sieci wewnętrznej](httpsys/_static/httpsys-to-internal.png)

W przypadku scenariuszy sieci wewnętrznej Kestrel zazwyczaj jest zalecane w przypadku najlepszą wydajność; Jednak w niektórych scenariuszach, można użyć funkcji, która oferuje tylko w pliku HTTP.sys. Aby uzyskać informacje o funkcjach HTTP.sys, zobacz [HTTP.sys](httpsys.md).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Sterownik HTTP.sys nosi nazwę WebListener w ASP.NET Core 1.x. Po uruchomieniu programu ASP.NET Core aplikacji w systemie Windows, WebListener stanowi alternatywę, który służy do scenariuszy, w którym chcesz udostępnić aplikację do Internetu, ale nie można używać usług IIS.

![Weblistener komunikuje się bezpośrednio z Internetem](weblistener/_static/weblistener-to-internet.png)

WebListener można również zamiast Kestrel dla aplikacji, które są dostępne tylko z siecią wewnętrzną, jeśli potrzebujesz funkcji WebListener, które Kestrel nie są obsługiwane. 

![Weblistener komunikuje się bezpośrednio z sieci wewnętrznej](weblistener/_static/weblistener-to-internal.png)

W przypadku scenariuszy sieci wewnętrznej ogólnie zaleca się Kestrel, aby uzyskać najlepszą wydajność, ale w niektórych sytuacjach możesz chcieć użyć funkcji, która oferuje tylko WebListener. Aby uzyskać informacje o funkcjach WebListener, zobacz [WebListener](weblistener.md).

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a>Uwagi dotyczące infrastruktury serwera ASP.NET Core

[ `IApplicationBuilder` ](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) Dostępne w `Startup` klasy `Configure` ujawnia metody `ServerFeatures` właściwości typu [ `IFeatureCollection` ](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel i WebListener uwidacznia tylko jednej funkcji [ `IServerAddressesFeature` ](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementacje inny serwer może udostępniać dodatkowe funkcje.

`IServerAddressesFeature`można sprawdzić, port, który ma powiązany implementacją serwera w czasie wykonywania.

## <a name="custom-servers"></a>Niestandardowe serwerów

Jeśli wbudowane serwery nie spełniają potrzeb użytkownika, można utworzyć implementacji niestandardowego serwera. [Interfejsu Open Web dla platformy .NET (OWIN) przewodnik](../owin.md) pokazano, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementacji. Można go dowolnie implementuje tylko interfejsy funkcji aplikacja wymaga, że co najmniej musi obsługiwać [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) i [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

- [Kestrel](kestrel.md)
- [Kestrel z usługami IIS](aspnet-core-module.md)
- [Hosting w systemie Linux z Nginx](xref:host-and-deploy/linux-nginx)
- [Hosting w systemie Linux z Apache](xref:host-and-deploy/linux-apache)
- [HTTP.sys](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

- [Kestrel](kestrel.md)
- [Kestrel z usługami IIS](aspnet-core-module.md)
- [Hosting w systemie Linux z Nginx](xref:host-and-deploy/linux-nginx)
- [Hosting w systemie Linux z Apache](xref:host-and-deploy/linux-apache)
- [WebListener](weblistener.md)

---
