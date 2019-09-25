---
title: Uruchamianie aplikacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak Klasa startowa w ASP.NET Core konfiguruje usługi i potok żądań aplikacji.
ms.author: riande
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 47194f786b2d32fb343e8f1078a4400d6db37293
ms.sourcegitcommit: e54672f5c493258dc449fac5b98faf47eb123b28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71248332"
---
# <a name="app-startup-in-aspnet-core"></a>Uruchamianie aplikacji w ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex)i [Steve Smith](https://ardalis.com)

`Startup` Klasa konfiguruje usługi i potok żądań aplikacji.

## <a name="the-startup-class"></a>Klasa Startup

Aplikacje ASP.NET Core używają `Startup` klasy, która jest nazywana `Startup` Konwencją. `Startup` Klasa:

* Opcjonalnie zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> metodę konfigurowania *usług*aplikacji. Usługa to składnik wielokrotnego użytku, który zapewnia funkcjonalność aplikacji. Usługi są konfigurowane&mdash;również jako *zarejestrowane*&mdash; `ConfigureServices` w aplikacji i używane przez [iniekcję zależności (di)](xref:fundamentals/dependency-injection) lub <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Zawiera metodę tworzenia potoku przetwarzania żądań aplikacji.

`ConfigureServices`i `Configure` są wywoływane przez środowisko uruchomieniowe ASP.NET Core podczas uruchamiania aplikacji:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Powyższy przykład jest przeznaczony dla [Razor Pages](xref:razor-pages/index); wersja MVC jest podobna.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

Klasa jest określana podczas kompilowania hosta aplikacji. [](xref:fundamentals/index#host) `Startup` Klasa jest zwykle określona przez [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) wywołanie metody w konstruktorze hosta: `Startup`

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

Host udostępnia usługi, które są dostępne dla `Startup` konstruktora klasy. Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`programu. Usługi hosta i aplikacji są dostępne w `Configure` systemach i w całej aplikacji.

Tylko następujące typy usług można wstrzyknąć do `Startup` konstruktora w przypadku użycia: <xref:Microsoft.Extensions.Hosting.IHostBuilder>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

Większość usług nie jest dostępna do momentu `Configure` wywołania metody.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Host udostępnia usługi, które są dostępne dla `Startup` konstruktora klasy. Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`programu. Zarówno usługi hosta, jak i aplikacje są dostępne w `Configure` systemie i w całej aplikacji.

Typowym zastosowaniem [iniekcji zależności](xref:fundamentals/dependency-injection) do `Startup` klasy jest wstrzyknięcie:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>Aby skonfigurować usługi według środowiska.
* <xref:Microsoft.Extensions.Configuration.IConfiguration>Aby odczytać konfigurację.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory>do utworzenia rejestratora w `Startup.ConfigureServices`programie.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Większość usług nie jest dostępna do momentu `Configure` wywołania metody.

::: moniker-end

### <a name="multiple-startup"></a>Wielokrotne uruchomienie

Gdy aplikacja definiuje oddzielne `Startup` klasy dla różnych środowisk (na `StartupDevelopment`przykład), odpowiednia `Startup` Klasa jest wybierana w czasie wykonywania. Kategoria, której sufiks nazwy jest zgodny z bieżącym środowiskiem, ma priorytet. Jeśli aplikacja jest uruchamiana w środowisku deweloperskim i zawiera `Startup` klasę `StartupDevelopment` i klasę, `StartupDevelopment` używana jest Klasa. Aby uzyskać więcej informacji, zobacz [Korzystanie z wielu środowisk](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Aby uzyskać więcej informacji na temat hosta, zobacz [hosta](xref:fundamentals/index#host) . Informacje dotyczące obsługi błędów podczas uruchamiania można znaleźć w temacie [Obsługa wyjątków uruchamiania](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> Metoda jest:

* Opcjonalny.
* Wywoływane przez hosta przed `Configure` metodą konfigurowania usług aplikacji.
* Gdzie są ustawiane [Opcje konfiguracji](xref:fundamentals/configuration/index) zgodnie z Konwencją.

Przed `Startup` wywołaniem metod host może skonfigurować niektóre usługi. Aby uzyskać więcej informacji, zobacz [hosta](xref:fundamentals/index#host).

W przypadku funkcji wymagających znaczącej konfiguracji `Add{Service}` istnieją metody rozszerzające w systemie. <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> Na przykład **Dodaj**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores i **Add**RazorPages:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

Dodanie usług do kontenera usług sprawia, że są one dostępne w aplikacji i w `Configure` metodzie. Usługi są rozwiązywane za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection) lub z <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

::: moniker range="< aspnetcore-3.0"

Aby [](xref:mvc/compatibility-version) uzyskać więcej informacji `SetCompatibilityVersion`, zobacz SetCompatibilityVersion.

::: moniker-end

## <a name="the-configure-method"></a>Metoda Configure

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Metoda służy do określania, w jaki sposób aplikacja reaguje na żądania HTTP. Potok żądań jest konfigurowany przez dodanie do <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> wystąpienia składników [pośredniczących](xref:fundamentals/middleware/index) . `IApplicationBuilder`jest dostępny dla `Configure` metody, ale nie jest ona zarejestrowana w kontenerze usługi. Hosting tworzy `IApplicationBuilder` i przekazuje go bezpośrednio do programu `Configure`.

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

Każda `Use` Metoda rozszerzenia dodaje jeden lub więcej składników pośredniczących do potoku żądania. Na przykład program <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> skonfiguruje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) , aby obsługiwało [pliki statyczne](xref:fundamentals/static-files).

Każdy składnik pośredniczący w potoku żądania jest odpowiedzialny za wywoływanie następnego składnika w potoku lub krótkiego obwodu łańcucha, jeśli jest to konieczne.

::: moniker range=">= aspnetcore-3.0"

Dodatkowe usługi, takie jak `IWebHostEnvironment`, `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`programie, można określić w `Configure` podpisie metody. Te usługi są wstrzykiwane, jeśli są dostępne.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory`lub dowolne elementy zdefiniowane w `ConfigureServices`programie, można określić w `Configure` podpisie metody. Te usługi są wstrzykiwane, jeśli są dostępne.

::: moniker-end

Aby uzyskać więcej informacji na temat sposobu `IApplicationBuilder` użycia i kolejności przetwarzania oprogramowania pośredniczącego, zobacz <xref:fundamentals/middleware/index>.

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>Konfigurowanie usług bez uruchamiania

Aby skonfigurować usługi i potok przetwarzania żądań bez użycia `Startup` klas, wywołań `ConfigureServices` i `Configure` wygodnych metod w konstruktorze hosta. Wiele wywołań do `ConfigureServices` dołączenia do siebie nawzajem. Jeśli istnieje `Configure` wiele wywołań metod, używane jest `Configure` ostatnie wywołanie.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a>Zwiększanie uruchamiania przy użyciu filtrów uruchamiania

Użyj <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> , aby skonfigurować oprogramowanie pośredniczące na początku lub na końcu aplikacji [Skonfiguruj](#the-configure-method) potok oprogramowania pośredniczącego. `IStartupFilter`służy do tworzenia potoku `Configure` metod. [IStartupFilter. configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) może ustawić oprogramowanie pośredniczące do uruchomienia przed lub po oprogramowaniu pośredniczącym dodanym przez biblioteki.

`IStartupFilter`implementuje <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, który odbiera i `Action<IApplicationBuilder>`zwraca. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> Definiuje klasę, aby skonfigurować potok żądania aplikacji. Aby uzyskać więcej informacji, zobacz [Tworzenie potoku oprogramowania pośredniczącego za pomocą IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Każdy `IStartupFilter` z nich może dodać jeden lub więcej middlewares w potoku żądania. Filtry są wywoływane w kolejności, w jakiej zostały dodane do kontenera usługi. Filtry mogą dodać oprogramowanie pośredniczące przed lub po przekazaniu kontroli do następnego filtru, w związku z czym dołączają do początku lub na końcu potoku aplikacji.

Poniższy przykład pokazuje, jak zarejestrować oprogramowanie pośredniczące w programie `IStartupFilter`. `RequestSetOptionsMiddleware` Oprogramowanie pośredniczące ustawia wartość opcji z parametru ciągu zapytania:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Jest skonfigurowany`RequestSetOptionsStartupFilter` w klasie:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` Jest skonfigurowany`RequestSetOptionsStartupFilter` w klasie:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

Jest zarejestrowany w kontenerze usługi w <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>. `IStartupFilter`

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

Po podaniu parametru `option` ciągu zapytania, oprogramowanie pośredniczące przetwarza przypisanie wartości przed wyrenderowaniem przez ASP.NET Core oprogramowanie pośredniczące.

Kolejność wykonywania oprogramowania pośredniczącego jest określana na podstawie `IStartupFilter` kolejności rejestracji:

* Wiele `IStartupFilter` implementacji może współistnieć z tymi samymi obiektami. Jeśli kolejność jest ważna, określ kolejność `IStartupFilter` rejestracji usług w celu dopasowania ich do kolejności, w jakiej middlewares powinny działać.
* Biblioteki mogą dodawać oprogramowanie pośredniczące z co najmniej `IStartupFilter` jedną implementacją, która jest uruchamiana przed lub po innym `IStartupFilter`oprogramowaniu pośredniczącym aplikacji zarejestrowanym w usłudze. Aby wywołać `IStartupFilter` oprogramowanie pośredniczące przed dodaniem oprogramowania pośredniczącego przez `IStartupFilter`bibliotekę:

  * Umieść rejestrację usługi przed dodaniem biblioteki do kontenera usługi.
  * Aby później wywołać, należy ustawić rejestrację usługi po dodaniu biblioteki.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Dodaj konfigurację podczas uruchamiania z zestawu zewnętrznego

Implementacja umożliwia dodawanie ulepszeń do aplikacji podczas uruchamiania z zewnętrznego zestawu poza `Startup` klasą aplikacji. <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
