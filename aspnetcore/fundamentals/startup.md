---
title: Uruchamianie aplikacji w ASP.NET Core
author: ardalis
description: "Odkryj, jak klasa początkowa w ASP.NET Core umożliwia skonfigurowanie usług i aplikacji żądania potoku."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: c324918b33af82b619bb2251f32308e4a57c27e5
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="application-startup-in-aspnet-core"></a>Uruchamianie aplikacji w ASP.NET Core

Przez [Steve Smith](https://ardalis.com), [Dykstra Tomasz](https://github.com/tdykstra), i [Luke Latham](https://github.com/guardrex)

`Startup` Klasa umożliwia skonfigurowanie usług i aplikacji żądania potoku.

## <a name="the-startup-class"></a>Klasa startowa.

Użyj aplikacji platformy ASP.NET Core `Startup` klasy, która ma nazwę `Startup` przez Konwencję. `Startup` Klasy:

* Można opcjonalnie dołączyć [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metody do skonfigurowania usług aplikacji.
* Musi zawierać [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metodę w celu utworzenia potoku przetwarzania żądań aplikacji.

`ConfigureServices`i `Configure` są wywoływane przez środowisko uruchomieniowe, po uruchomieniu aplikacji:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Określ `Startup` klasy z [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) metody:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` Konstruktor klasy akceptuje zależności zdefiniowany przez hosta. Typowym zastosowaniem [iniekcji zależności](xref:fundamentals/dependency-injection) do `Startup` jest klasa do dodania:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) do skonfigurowania usług przez środowisko.
* [Wartości IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do skonfigurowania aplikacji podczas uruchamiania.

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Zamiast wstrzyknięcie `IHostingEnvironment` jest użycie podejście oparte na Konwencji. Aplikacji można zdefiniować oddzielne `Startup` klasy dla różnych środowisk (na przykład `StartupDevelopment`), a klasa początkowa odpowiednie jest wybierana w czasie wykonywania. Priorytety jest klasa, którego sufiks nazwy zgodny z bieżącym środowisku. Jeśli aplikacja jest uruchamiana w środowisku programistycznym i zawiera zarówno `Startup` klasy i `StartupDevelopment` klasy `StartupDevelopment` klasa jest używana. Aby uzyskać więcej informacji, zobacz [Praca w środowiskach wielu](xref:fundamentals/environments#startup-conventions).

Aby dowiedzieć się więcej o `WebHostBuilder`, zobacz [hostingu](xref:fundamentals/hosting) tematu. Aby informacji na temat obsługi błędów podczas uruchamiania, zobacz [obsługi wyjątków uruchamiania](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) metoda jest:

* Opcjonalny.
* Wywołanie przez hosta sieci web przed `Configure` metody do skonfigurowania usług aplikacji.
* Gdzie [opcje konfiguracji](xref:fundamentals/configuration/index) są ustawiane przez Konwencję.

Dodawanie usługi do kontenera usługi udostępnia je w aplikacji i w `Configure` metody. Usługi zostały rozwiązane za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) lub [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Host sieci web można skonfigurować niektóre usługi przed `Startup` metody są wywoływane. Szczegółowe informacje są dostępne w [hostingu](xref:fundamentals/hosting) tematu. 

Dla funkcji, które wymagają znacznej Instalatora, są `Add[Service]` metody rozszerzenia na [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Aplikacja sieci web typowe rejestruje usługi programu Entity Framework, tożsamości i MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Dostępne w uruchamiania usługi

Host sieci web zawiera niektóre usługi, które są dostępne dla `Startup` konstruktora klasy. Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`. Usługi aplikacji i hostów będą dostępne w `Configure` i w całej aplikacji.

## <a name="the-configure-method"></a>Konfiguruj — metoda

[Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) metoda jest używana do określania, jak aplikacji odpowiada na żądania HTTP. Potok żądań jest skonfigurowana przez dodanie [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) składników [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) wystąpienia. `IApplicationBuilder`jest dostępny dla `Configure` metody, ale nie jest zarejestrowany w kontenerze usług. Hosting tworzy `IApplicationBuilder` i przekazuje je bezpośrednio do `Configure` ([źródło odwołania](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Szablonów platformy ASP.NET Core](/dotnet/core/tools/dotnet-new) Konfiguruje potok o obsługę stronę dewelopera wyjątek [BrowserLink](http://vswebessentials.com/features/browserlink), stron błędów, pliki statyczne i ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Każdy `Use` — metoda rozszerzenia dodaje składnik oprogramowania pośredniczącego do potoku żądania. Na przykład `UseMvc` — metoda rozszerzenia dodaje [routingu oprogramowanie pośredniczące](xref:fundamentals/routing) do potoku żądania i konfiguruje [MVC](xref:mvc/overview) jako domyślny program obsługi.

Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory`, można także określić w podpisie metody. W przypadku wstrzykuje się dodatkowe usługi, jeśli są one dostępne.

Aby uzyskać więcej informacji na temat sposobu użycia `IApplicationBuilder`, zobacz [oprogramowanie pośredniczące](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Podręczne metody

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) i [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) wygodne metody mogą być używane zamiast określania `Startup` klasy. Wiele wywołań `ConfigureServices` dołączyć do siebie. Wiele wywołań `Configure` Użyj ostatnich wywołania metody.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Filtry uruchamiania

Użyj [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) do konfiguracji oprogramowania pośredniczącego na początku lub na końcu aplikacji [Konfiguruj](#the-configure-method) potoku oprogramowania pośredniczącego. `IStartupFilter`jest przydatne upewnić się, że oprogramowanie pośredniczące jest uruchamiany przed lub po dodanych przez bibliotek na początku lub na końcu potoku przetwarzania żądań aplikacji oprogramowania pośredniczącego.

`IStartupFilter`implementuje jednej metody [Konfiguruj](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), która odbiera i zwraca `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) definiuje klasę do konfigurowania potoku żądania aplikacji. Aby uzyskać więcej informacji, zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).

Każdy `IStartupFilter` implementuje middlewares co najmniej jeden z potokiem żądań. Filtry są wywoływane w kolejności, które zostały dodane do kontenera usług. Filtry mogą dodawać oprogramowanie pośredniczące przed lub po przekazanie sterowania do następnego filtru, w związku z tym ich dołączania na początku lub na końcu potoku aplikacji.

[Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) pokazuje, jak zarejestrować oprogramowania pośredniczącego z `IStartupFilter`. Przykładowa aplikacja zawiera oprogramowanie pośredniczące, która ustawia wartości opcji z parametru ciągu zapytania:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Jest skonfigurowany w `RequestSetOptionsStartupFilter` klasy:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Jest zarejestrowany w kontenerze usługi w `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Gdy parametr ciągu zapytania dla `option` została podana, oprogramowanie pośredniczące przetwarza przypisanie wartości, zanim oprogramowanie pośredniczące MVC renderuje odpowiedzi:

![Okno przeglądarki, przedstawiające renderowanej strony indeksu. Wartość opcji jest renderowana "Z oprogramowania pośredniczącego" oparte na żądania strony z parametru ciągu zapytania i wartość ustawienie "Z oprogramowania pośredniczącego".](startup/_static/index.png)

Kolejność wykonywania oprogramowanie pośredniczące zestaw jest w celu `IStartupFilter` rejestracji:

* Wiele `IStartupFilter` implementacje może współpracować z te same obiekty. Jeśli kolejność jest ważna, kolejność ich `IStartupFilter` usługi rejestracji, aby dopasować kolejności ich middlewares powinno być ono uruchomione.
* Biblioteki mogą dodawać oprogramowanie pośredniczące z co najmniej jednym `IStartupFilter` implementacje uruchamiane przed lub po innym oprogramowaniu pośredniczącym aplikacji zarejestrowany `IStartupFilter`. Do wywołania `IStartupFilter` oprogramowanie pośredniczące przed oprogramowanie pośredniczące dodane przez bibliotekę `IStartupFilter`, umieść rejestracji usługi przed biblioteki jest dodawany do kontenera usług. Aby wywołać później, umieść rejestracji usługi, po dodaniu biblioteki.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Hosting](xref:fundamentals/hosting)
* [Praca w środowiskach wielu](xref:fundamentals/environments)
* [Oprogramowanie pośredniczące](xref:fundamentals/middleware/index)
* [Rejestrowanie](xref:fundamentals/logging/index)
* [Konfiguracja](xref:fundamentals/configuration/index)
* [Klasa StartupLoader: FindStartupType — metoda (odwołanie do źródła)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
