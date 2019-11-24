---
title: Uruchamianie aplikacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak Klasa startowa w ASP.NET Core konfiguruje usługi i potok żądań aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/02/2019
uid: fundamentals/startup
ms.openlocfilehash: 081eaa772d136477a37a3392877886327e0cda7c
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634034"
---
# <a name="app-startup-in-aspnet-core"></a>Uruchamianie aplikacji w ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)i [Steve Smith](https://ardalis.com)

Klasa `Startup` konfiguruje usługi i potok żądań aplikacji.

## <a name="the-startup-class"></a>Klasa Startup

Aplikacje ASP.NET Core używają klasy `Startup`, która nosi nazwę `Startup` według Konwencji. Klasa `Startup`:

* Opcjonalnie zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> metodę konfigurowania *usług*aplikacji. Usługa to składnik wielokrotnego użytku, który zapewnia funkcjonalność aplikacji. Usługi są *rejestrowane* w `ConfigureServices` i używane w aplikacji przez [iniekcję zależności (DI)](xref:fundamentals/dependency-injection) lub <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* Zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> metodę tworzenia potoku przetwarzania żądań aplikacji.

`ConfigureServices` i `Configure` są wywoływane przez środowisko uruchomieniowe środowiska ASP.NET Core podczas uruchamiania aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

Klasa `Startup` jest określana podczas kompilowania [hosta](xref:fundamentals/index#host) aplikacji. Klasa `Startup` jest zazwyczaj określana przez wywołanie metody [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) na konstruktorze hosta:

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

Host udostępnia usługi, które są dostępne dla konstruktora klasy `Startup`. Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`. Usługa Host i aplikacje są dostępne w `Configure` i w całej aplikacji.

Tylko następujące typy usług można wstrzyknąć do konstruktora `Startup`, gdy jest używany [host generyczny](xref:fundamentals/host/generic-host) (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):

* <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

Większość usług nie jest dostępna do momentu wywołania metody `Configure`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Host udostępnia usługi, które są dostępne dla konstruktora klasy `Startup`. Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`. Usługa Host i aplikacje są następnie dostępne w `Configure` i w całej aplikacji.

Typowym zastosowaniem [iniekcji zależności](xref:fundamentals/dependency-injection) do klasy `Startup` jest wstrzyknięcie:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> skonfigurować usługi według środowiska.
* <xref:Microsoft.Extensions.Configuration.IConfiguration> odczytać konfiguracji.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> utworzyć rejestrator w `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Większość usług nie jest dostępna do momentu wywołania metody `Configure`.

::: moniker-end

### <a name="multiple-startup"></a>Wielokrotne uruchomienie

Gdy aplikacja definiuje oddzielne klasy `Startup` dla różnych środowisk (na przykład `StartupDevelopment`), odpowiednia Klasa `Startup` jest wybierana w czasie wykonywania. Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet. Jeśli aplikacja jest uruchamiana w środowisku deweloperskim i zawiera zarówno klasę `Startup`, jak i klasę `StartupDevelopment`, używana jest Klasa `StartupDevelopment`. Aby uzyskać więcej informacji, zobacz [Korzystanie z wielu środowisk](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Aby uzyskać więcej informacji na temat hosta, zobacz [hosta](xref:fundamentals/index#host) . Informacje dotyczące obsługi błędów podczas uruchamiania można znaleźć w temacie [Obsługa wyjątków uruchamiania](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

Metoda <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> jest:

* Opcjonalna.
* Wywoływane przez hosta przed metodą `Configure`, aby skonfigurować usługi aplikacji.
* Gdzie są ustawiane [Opcje konfiguracji](xref:fundamentals/configuration/index) zgodnie z Konwencją.

Host może skonfigurować niektóre usługi przed wywołaniem metod `Startup`. Aby uzyskać więcej informacji, zobacz [hosta](xref:fundamentals/index#host).

W przypadku funkcji wymagających istotnej konfiguracji w <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>istnieją `Add{Service}` metody rozszerzenia. Na przykład **Dodaj**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores i **Add**RazorPages:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

Dodanie usług do kontenera usług sprawia, że są one dostępne w aplikacji i w metodzie `Configure`. Usługi są rozwiązywane za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection) lub z <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

::: moniker range="< aspnetcore-3.0"

Zobacz [SetCompatibilityVersion](xref:mvc/compatibility-version) , aby uzyskać więcej informacji na temat `SetCompatibilityVersion`.

::: moniker-end

## <a name="the-configure-method"></a>Metoda Configure

Metoda <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> służy do określania, w jaki sposób aplikacja reaguje na żądania HTTP. Potok żądań jest konfigurowany przez dodanie składników [pośredniczących](xref:fundamentals/middleware/index) do wystąpienia <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>. `IApplicationBuilder` jest dostępna dla metody `Configure`, ale nie jest ona zarejestrowana w kontenerze usługi. Hosting tworzy `IApplicationBuilder` i przekazuje go bezpośrednio do `Configure`.

[Szablony ASP.NET Core](/dotnet/core/tools/dotnet-new) konfigurują potok z obsługą:

* [Strona wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page)
* [Procedura obsługi wyjątków](xref:fundamentals/error-handling#exception-handler-page)
* [Zabezpieczenia protokołu HTTP Strict Transport (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [Przekierowanie HTTPS](xref:security/enforcing-ssl)
* [Pliki statyczne](xref:fundamentals/static-files)
* ASP.NET Core [MVC](xref:mvc/overview) i [Razor Pages](xref:razor-pages/index)

::: moniker range="< aspnetcore-3.0"

* [Ogólne rozporządzenie o ochronie danych (Rodo)](xref:security/gdpr)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

Każda metoda rozszerzenia `Use` dodaje jeden lub więcej składników pośredniczących do potoku żądania. Na przykład <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> konfiguruje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) do obsłużynia [plików statycznych](xref:fundamentals/static-files).

Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu łańcucha, jeśli jest to konieczne.

::: moniker range=">= aspnetcore-3.0"

Dodatkowe usługi, takie jak `IWebHostEnvironment`, `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`, można określić w podpisie metody `Configure`. Te usługi są wstrzykiwane, jeśli są dostępne.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`, można określić w podpisie metody `Configure`. Te usługi są wstrzykiwane, jeśli są dostępne.

::: moniker-end

Aby uzyskać więcej informacji na temat używania `IApplicationBuilder` i kolejności przetwarzania oprogramowania pośredniczącego, zobacz <xref:fundamentals/middleware/index>.

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>Konfigurowanie usług bez uruchamiania

Aby skonfigurować usługi i potok przetwarzania żądań bez użycia klasy `Startup`, wywołaj `ConfigureServices` i `Configure` wygodne metody w konstruktorze hosta. Wiele wywołań `ConfigureServices` dołączyć do siebie nawzajem. Jeśli istnieje wiele wywołań metod `Configure`, używane jest ostatnie wywołanie `Configure`.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a>Zwiększanie uruchamiania przy użyciu filtrów uruchamiania

Użyj <xref:Microsoft.AspNetCore.Hosting.IStartupFilter>:

* Aby skonfigurować oprogramowanie pośredniczące na początku lub na końcu aplikacji [Skonfiguruj](#the-configure-method) potok oprogramowania pośredniczącego bez jawnego wywołania do `Use{Middleware}`. `IStartupFilter` jest używany przez ASP.NET Core do dodawania wartości domyślnych na początku potoku bez konieczności jawnego rejestrowania przez autora aplikacji domyślnego oprogramowania pośredniczącego. `IStartupFilter` umożliwia innym wywołaniem składnika `Use{Middleware}` w imieniu autora aplikacji.
* Tworzenie potoku metod `Configure`. [IStartupFilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) może ustawić oprogramowanie pośredniczące do uruchomienia przed lub po oprogramowaniu pośredniczącym dodanym przez biblioteki.

`IStartupFilter` implementuje <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, który odbiera i zwraca `Action<IApplicationBuilder>`. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> definiuje klasę, aby skonfigurować potok żądania aplikacji. Aby uzyskać więcej informacji, zobacz [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Każdy `IStartupFilter` może dodać jeden lub więcej middlewares w potoku żądania. Filtry są wywoływane w kolejności, w jakiej zostały dodane do kontenera usługi. Filtry mogą dodać oprogramowanie pośredniczące przed lub po przekazaniu kontroli do następnego filtru, w związku z czym dołączają do początku lub na końcu potoku aplikacji.

Poniższy przykład pokazuje, jak zarejestrować oprogramowanie pośredniczące w `IStartupFilter`. Oprogramowanie pośredniczące `RequestSetOptionsMiddleware` ustawia wartość opcji z parametru ciągu zapytania:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` jest konfigurowana w klasie `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` jest konfigurowana w klasie `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

`IStartupFilter` jest zarejestrowany w kontenerze usługi w programie <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

Gdy podano parametr ciągu zapytania dla `option`, oprogramowanie pośredniczące przetwarza przypisanie wartości, zanim oprogramowanie pośredniczące ASP.NET Core renderuje odpowiedź.

Kolejność wykonywania oprogramowania pośredniczącego jest ustawiana w kolejności `IStartupFilter` rejestracji:

* Wiele implementacji `IStartupFilter` może współistnieć z tymi samymi obiektami. Jeśli kolejność jest ważna, określ kolejność `IStartupFilter` rejestracji usługi, aby odpowiadały kolejności, w jakiej middlewares.
* Biblioteki mogą dodawać oprogramowanie pośredniczące z co najmniej jedną `IStartupFilter` implementacjami, które są uruchamiane przed lub po innym oprogramowaniu pośredniczącym aplikacji zarejestrowanym w usłudze `IStartupFilter`. Aby wywoływać `IStartupFilter` oprogramowanie pośredniczące przed dodaniem oprogramowania pośredniczącego przez `IStartupFilter`biblioteki:

  * Umieść rejestrację usługi przed dodaniem biblioteki do kontenera usługi.
  * Aby później wywołać, należy ustawić rejestrację usługi po dodaniu biblioteki.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Dodaj konfigurację podczas uruchamiania z zestawu zewnętrznego

Implementacja <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zestawu zewnętrznego poza klasą `Startup` aplikacji. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
