---
title: "Moduł platformy ASP.NET Core"
author: tdykstra
description: "Wprowadza platformy ASP.NET Core modułu (ANCM), moduł usług IIS, który umożliwia Kestrel serwera sieci web usług IIS lub usług IIS Express jest używany jako serwer zwrotnego serwera proxy."
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/aspnet-core-module
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 153c40f0e825ff5826e916c7ea877a25d81954f1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-aspnet-core-module"></a>Wprowadzenie do platformy ASP.NET Core modułu

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), i [Roaming Krzysztof](https://github.com/Tratcher) 

Program ASP.NET Core modułu (ANCM) umożliwia uruchamianie platformy ASP.NET Core aplikacje za usług IIS, przy użyciu usług IIS dla co to jest dobrze sobie radzi (zabezpieczeń, możliwości zarządzania i wiele więcej) i [Kestrel](kestrel.md) dla co to jest dobrze sobie radzi (co naprawdę fast) i pobierania korzyści z obu tych technologii jednocześnie. **ANCM działa tylko w przypadku Kestrel; nie jest zgodna z WebListener (w ASP.NET Core 1.x) lub sterownik HTTP.sys (w 2.x).** 

Obsługiwane wersje systemu Windows:

* Windows 7 i Windows Server 2008 R2 lub nowszy

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-aspnet-core-module-does"></a>Jak działa moduł platformy ASP.NET Core

ANCM jest macierzysty moduł usług IIS, który przechwytuje do potoku usług IIS i przekierowuje ruch z zapleczem aplikacji platformy ASP.NET Core. Większość innych modułów, takich jak uwierzytelnianie systemu windows, nadal otrzymywać możliwość uruchamiania. Podczas obsługi został wybrany do żądania i mapowanie obsługi jest zdefiniowany w aplikacji ANCM tylko przejmuje kontrolę *web.config* pliku.

Ponieważ aplikacje platformy ASP.NET Core w procesie oddzielić od proces roboczy usług IIS, ANCM także przetwarzać zarządzania. ANCM uruchamia proces dla aplikacji platformy ASP.NET Core podczas pierwszego żądania jest dostępna i ponownie go uruchamia uległa awarii. Jest to zasadniczo takie samo zachowanie jako klasycznych aplikacji programu ASP.NET uruchomienia procesu w usługach IIS i są zarządzane przez usługę WAS (Windows Activation Service).

Oto diagramie przedstawiono relację między aplikacjami usług IIS, ANCM i ASP.NET Core.

![Moduł platformy ASP.NET Core](aspnet-core-module/_static/ancm.png)

Żądania pochodzą sieci Web i trafień sterownik Http.Sys trybu jądra, który kieruje je do usług IIS na głównej portu (80) lub portu protokołu SSL (port 443). ANCM przekazuje żądanie do aplikacji platformy ASP.NET Core na port protokołu HTTP skonfigurowany dla aplikacji, która nie jest port 80/443.

Kestrel nasłuchuje ruchu przychodzącego z ANCM.  ANCM Określa port, za pomocą zmiennej środowiskowej podczas uruchamiania i [UseIISIntegration](#call-useiisintegration) metody konfiguruje serwer do nasłuchiwania `http://localhost:{port}`. Istnieją dodatkowe kontrole do odrzucania żądań nie z ANCM. (ANCM nie obsługuje przekazywanie protokołu HTTPS, więc żądania są przekazywane za pośrednictwem protokołu HTTP, nawet jeśli odebranych przez usługi IIS przy użyciu protokołu HTTPS.)

Kestrel przejmuje żądań z ANCM i umieszcza je w potoku oprogramowania pośredniczącego platformy ASP.NET Core, który następnie je obsługuje i przekazuje je na jako `HttpContext` wystąpienia logiki aplikacji. Odpowiedzi aplikacji są następnie przekazywane z powrotem do usług IIS, które umieszcza je z powrotem do klienta HTTP, który zainicjował żądania.

ANCM ma kilka innych funkcji, jak również:

* Ustawia zmienne środowiskowe.
* Dzienniki `stdout` dane wyjściowe do pliku magazynu.
* Przekazuje tokeny uwierzytelniania systemu Windows.

## <a name="how-to-use-ancm-in-aspnet-core-apps"></a>Jak używać ANCM w aplikacji platformy ASP.NET Core

Ta sekcja zawiera omówienie procesu konfigurowania aplikacji platformy ASP.NET Core i serwera usług IIS. Aby uzyskać szczegółowe instrukcje, zobacz [hosta w systemie Windows z programem IIS](xref:host-and-deploy/iis/index).

### <a name="install-ancm"></a>Zainstaluj ANCM


Moduł platformy ASP.NET Core musi być zainstalowany w usługach IIS na serwerach i w usługach IIS Express na maszynach w rozwoju. W przypadku serwerów ANCM znajduje się w [pakietu .NET Core systemu Windows serwer obsługujący](https://aka.ms/dotnetcore-2-windowshosting). Programowanie maszyn Visual Studio automatycznie instaluje ANCM w usługach IIS Express i w usługach IIS, jeśli jest już zainstalowany na tym komputerze.

### <a name="install-the-iisintegration-nuget-package"></a>Zainstaluj pakiet IISIntegration NuGet

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) pakietu znajduje się w metapackages platformy ASP.NET Core ([Microsoft.AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore/) i [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ). Jeśli nie używasz jednego z metapackages, zainstaluj `Microsoft.AspNetCore.Server.IISIntegration` oddzielnie. `IISIntegration` Pakiet jest pakiet współdziałanie odczytujący zmiennych środowiskowych emitowane przez ANCM do skonfigurowania aplikacji. Zmienne środowiskowe, podaj informacje o konfiguracji, takie jak port do nasłuchiwania. 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

W aplikacji, należy zainstalować [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/). `IISIntegration` Pakiet jest pakiet współdziałanie odczytujący zmiennych środowiskowych emitowane przez ANCM do skonfigurowania aplikacji. Zmienne środowiskowe, podaj informacje o konfiguracji, takie jak port do nasłuchiwania. 

---

### <a name="call-useiisintegration"></a>Wywołanie UseIISIntegration

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`UseIISIntegration` — Metoda rozszerzenia na [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) jest wywoływana automatycznie po uruchomieniu z programu IIS.

Jeśli nie są przy użyciu jednej z platformy ASP.NET Core metapackages i nie zostały zainstalowane `Microsoft.AspNetCore.Server.IISIntegration` pakietów, Pobierz błąd w czasie wykonywania. Jeśli należy wywołać `UseIISIntegration` jawnie, Pobierz błąd w czasie kompilacji Jeśli pakiet nie jest zainstalowany.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

W aplikacji `Main` metody, wywołaj `UseIISIntegration` — metoda rozszerzenia na [ `WebHostBuilder` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder). 

[!code-csharp[](aspnet-core-module/sample/Program.cs?name=snippet_Main&highlight=12)]

---

`UseIISIntegration` Metoda szuka zmiennych środowiskowych, które ustawia ANCM i nie ops, jeśli nie można odnaleźć. To zachowanie ułatwia scenariuszy, takich jak tworzenie i testowanie na macOS lub Linux i wdrażanie na serwer z uruchomionymi usługami IIS. Podczas uruchamiania na macOS lub Linux, Kestrel działa jako serwer sieci web; Jednak gdy aplikacja jest wdrażana w środowisku usług IIS, automatycznie używa ANCM i usług IIS.

### <a name="ancm-port-binding-overrides-other-port-bindings"></a>Powiązanie portu ANCM przesłania pozostałych powiązaniach portu

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ANCM generuje portów dynamicznych do przypisania do procesu zaplecza. `UseIISIntegration` Metoda przejmuje ten port dynamiczny i konfiguruje Kestrel do nasłuchiwania `http://locahost:{dynamicPort}/`. Przesłania inne konfiguracje adresu URL, takie jak wywołania `UseUrls` lub [API nasłuchiwania na Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration). W związku z tym nie należy wywołać `UseUrls` lub jego Kestrel `Listen` interfejsu API, korzystając z ANCM. Jeśli zostanie wywołana `UseUrls` lub `Listen`, Kestrel nasłuchuje na porcie określić podczas uruchamiania aplikacji bez usług IIS.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ANCM generuje portów dynamicznych do przypisania do procesu zaplecza. `UseIISIntegration` Metoda przejmuje ten port dynamiczny i konfiguruje Kestrel do nasłuchiwania `http://locahost:{dynamicPort}/`. Przesłania inne konfiguracje adresu URL, takie jak wywołania `UseUrls`. W związku z tym nie należy wywołać `UseUrls` korzystając ANCM. Jeśli zostanie wywołana `UseUrls`, Kestrel nasłuchuje na porcie określić podczas uruchamiania aplikacji bez usług IIS.

W ASP.NET Core 1.0, jeśli wywołujesz `UseUrls`, wywołać ją **przed** należy wywołać `UseIISIntegration` tak, aby port skonfigurowany ANCM nie zostać zastąpione. To zamówienie wywołania nie jest wymagane w ASP.NET Core 1.1, ponieważ ustawienie ANCM zastępuje `UseUrls`.

---

### <a name="configure-ancm-options-in-webconfig"></a>Skonfiguruj opcje ANCM w pliku Web.config

Konfiguracja modułu platformy ASP.NET Core są przechowywane w *web.config* pliku, który znajduje się w folderze głównym. Ustawienia w tym pliku punktu do uruchamiania polecenia i argumentów, które uruchomiona aplikacja platformy ASP.NET Core. Dla przykładu *web.config* wskazówki na temat opcji konfiguracji i kod zobacz [odwołania do platformy ASP.NET Core modułu konfiguracji](xref:host-and-deploy/aspnet-core-module).

### <a name="run-with-iis-express-in-development"></a>Uruchom z programem IIS Express do rozwoju

Usługi IIS Express można uruchomić programu Visual Studio przy użyciu domyślnego profilu zdefiniowany za pomocą szablonów ASP.NET Core.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Konfiguracja serwera proxy używa protokołu HTTP i token parowania

Serwer proxy między ANCM i Kestrel używa protokołu HTTP. Przy użyciu protokołu HTTP jest optymalizacji wydajności których ruch między ANCM i Kestrel odbywa się na adres sprzężenia zwrotnego wylogowuje interfejsu sieciowego. Nie istnieje ryzyko z podsłuchiwaniu ruch między ANCM i Kestrel z lokalizacji wylogowuje na serwerze.

Token parowania służy do zagwarantowania, że żądań odebranych przez Kestrel były przekazywane przez serwer proxy przez usługi IIS i nie pochodzą z innego źródła. Token parowania zostanie utworzona i ustawiona w zmiennej środowiskowej (`ASPNETCORE_TOKEN`) przez ANCM. Token parowania zostanie również ustawiona w nagłówku (`MSAspNetCoreToken`) na każde żądanie przekazywane przez serwer proxy. Oprogramowanie pośredniczące IIS sprawdzanie żądań odbierze upewnij się, że parowania wartość tokenu nagłówka odpowiada wartości zmiennej środowiskowej. Jeśli wartości tokenów są niezgodne, żądanie jest rejestrowane i odrzucone. Parowania zmiennej środowiskowej tokenu i komunikacji między ANCM i Kestrel nie są dostępne z lokalizacji wylogowuje na serwerze. Bez wiedzy o parowania wartość tokenu, osoba atakująca nie może przesłać żądania, które pomijają wyboru w oprogramowaniu pośredniczącym usługi IIS.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące zasoby:

* [Przykładowa aplikacja dla tego artykułu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/aspnet-core-module/sample)
* [Kod źródłowy platformy ASP.NET Core modułu](https://github.com/aspnet/AspNetCoreModule)
* [Odwołania do konfiguracji modułu platformy ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Hosting w systemie Windows z usługami IIS](xref:host-and-deploy/iis/index)
