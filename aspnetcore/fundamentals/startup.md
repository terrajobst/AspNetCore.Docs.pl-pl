---
title: Uruchamianie aplikacji w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak klasa startowa. w programie ASP.NET Core umożliwia skonfigurowanie usług i potok żądań aplikacji.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: 362186be6feeeefeca3c56688ee6420de5fb9659
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468627"
---
# <a name="app-startup-in-aspnet-core"></a>Uruchamianie aplikacji w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), i [Steve Smith](https://ardalis.com)

`Startup` Klasa służy do konfigurowania usług i potok żądań aplikacji.

## <a name="the-startup-class"></a>Klasa Startup

Użyj aplikacji platformy ASP.NET Core `Startup` klasy, która nosi nazwę `Startup` przez Konwencję. `Startup` Klasy:

* Opcjonalnie zawiera <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> metodę, aby skonfigurować aplikację *usług*. Usługa jest komponentów wielokrotnego użytku, który udostępnia funkcjonalność aplikacji. Usługi są skonfigurowane&mdash;też opisany jako *zarejestrowany*&mdash;w `ConfigureServices` , które są używane przez aplikację za pośrednictwem [wstrzykiwanie zależności (DI)](xref:fundamentals/dependency-injection) lub <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* Obejmuje <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> metodę, aby utworzyć potok przetwarzania żądania przez aplikację.

`ConfigureServices` i `Configure` są wywoływane przez środowisko uruchomieniowe, po uruchomieniu aplikacji:

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

`Startup` Określono klasę do aplikacji podczas aplikacji [hosta](xref:fundamentals/index#host) jest wbudowana. Host aplikacji jest wbudowany, kiedy `Build` jest wywoływana w Konstruktorze hosta w `Program` klasy. `Startup` Zwykle jest określona przez wywołanie metody [WebHostBuilderExtensions.UseStartup\<TStartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) metody w Konstruktorze hosta:

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

Host udostępnia usługi, które są dostępne dla `Startup` konstruktora klasy. Aplikacja dodaje dodatkowe usługi za pośrednictwem `ConfigureServices`. Usługi aplikacji i hostów będą dostępne w `Configure` i w całej aplikacji.

Typowym zastosowaniem [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) do `Startup` klasa jest do dodania:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> Aby skonfigurować usługi przez środowisko.
* <xref:Microsoft.Extensions.Configuration.IConfiguration> Odczytywanie konfiguracji.
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> można utworzyć rejestratora w `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Alternatywa wprowadza `IHostingEnvironment` jest wykorzystanie podejścia opartego na Konwencji. Kiedy aplikacja definiuje oddzielnych `Startup` klas w różnych środowiskach (na przykład `StartupDevelopment`), odpowiednie `Startup` klasy jest zaznaczona w czasie wykonywania. Klasy, w których sufiks nazwy pasuje do bieżącego środowiska jest podzielony na priorytety. Jeśli aplikacja jest uruchamiana w środowisku deweloperskim i obejmuje zarówno `Startup` klasy i `StartupDevelopment` klasy `StartupDevelopment` klasa jest używana. Aby uzyskać więcej informacji, zobacz [używanie wielu środowisk](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Aby dowiedzieć się więcej o hoście, zobacz [hosta](xref:fundamentals/index#host). Aby uzyskać informacji na temat obsługi błędów podczas uruchamiania, zobacz [uruchamiania obsługi wyjątków](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Metoda ConfigureServices

<xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> Metodą jest:

* Opcjonalna.
* Wywołanie przez hosta przed `Configure` metodę, aby skonfigurować usługi aplikacji.
* Gdzie [opcje konfiguracji](xref:fundamentals/configuration/index) są ustawiane przez Konwencję.

Typowy wzorzec polega na wywołaniu wszystkich `Add{Service}` metody, a następnie wywołać wszystkich `services.Configure{Service}` metody. Na przykład zobacz [Konfigurowanie tożsamości usługi](xref:security/authentication/identity#pw).

Host może skonfigurować niektóre usługi przed `Startup` metody są wywoływane. Aby uzyskać więcej informacji, zobacz [hosta](xref:fundamentals/index#host).

W przypadku funkcji, które wymagają znacznej Instalatora, istnieją `Add{Service}` metod rozszerzenia <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>. Typowa aplikacja platformy ASP.NET Core rejestruje usługi Entity Framework, tożsamości i MVC:

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

Dodawanie usług do kontenera usługi udostępnia je w aplikacji, a w `Configure` metody. Te usługi są rozpoznawane przez [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) lub <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

Zobacz [SetCompatibilityVersion](xref:mvc/compatibility-version) więcej informacji na temat `SetCompatibilityVersion`.

## <a name="the-configure-method"></a>Metoda Konfiguruj

<xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> Metoda jest używana do określenia, w jaki sposób aplikacja reaguje na żądania HTTP. Potok żądań jest skonfigurowany, dodając [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) składników <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> wystąpienia. `IApplicationBuilder` jest dostępna dla `Configure` metody, ale nie jest zarejestrowany w kontenerze usługi. Hostingu tworzy `IApplicationBuilder` i przekazuje je bezpośrednio do `Configure`.

[Szablony ASP.NET Core](/dotnet/core/tools/dotnet-new) Konfiguruje potok o obsługę:

* [Stronie wyjątków dla deweloperów](xref:fundamentals/error-handling#developer-exception-page)
* [Program obsługi wyjątków](xref:fundamentals/error-handling#exception-handler-page)
* [Zabezpieczenia transportu Strict HTTP (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [Przekierowania protokołu HTTPS](xref:security/enforcing-ssl)
* [Pliki statyczne](xref:fundamentals/static-files)
* [General Data Protection Regulation (GDPR)](xref:security/gdpr)
* Platforma ASP.NET Core [MVC](xref:mvc/overview) i [stron Razor](xref:razor-pages/index)

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

Każdy `Use` — metoda rozszerzenia dodaje jeden lub więcej składników oprogramowania pośredniczącego do potoku żądania. Na przykład `UseMvc` dodaje metody rozszerzenia [routingu oprogramowania pośredniczącego](xref:fundamentals/routing) do potoku żądania i konfiguruje [MVC](xref:mvc/overview) jako domyślny program obsługi.

Każdy składnik oprogramowania pośredniczącego w potoku żądania jest odpowiedzialny za wywoływanie następny składnik w potoku lub zwarcie łańcucha, jeśli to stosowne. Jeśli zwarcie nie występuje prowadzącą oprogramowanie pośredniczące, każdy oprogramowania pośredniczącego ma drugą szansę, aby przetworzyć żądanie przed wysłaniem go do klienta.

Dodatkowe usługi, takie jak `IHostingEnvironment` i `ILoggerFactory`, może zostać określony w `Configure` podpis metody. Jeśli zostanie określony, są wprowadzane dodatkowe usługi, jeśli są one dostępne.

Aby uzyskać więcej informacji na temat sposobu użycia `IApplicationBuilder` i kolejność przetwarzania oprogramowanie pośredniczące, zobacz <xref:fundamentals/middleware/index>.

## <a name="convenience-methods"></a>Wygodne metody

Konfigurowanie usług i potok przetwarzania żądania bez użycia `Startup` klasy, wywołaj `ConfigureServices` i `Configure` wygodne metody w Konstruktorze hosta. Wiele wywołań `ConfigureServices` dołączenia do siebie nawzajem. Jeśli wiele `Configure` istnieje metoda wywołuje metodę, ostatni `Configure` wywołanie jest używane.

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

## <a name="extend-startup-with-startup-filters"></a>Rozszerzanie uruchamiania przy użyciu filtrów uruchamiania

Użyj <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> do konfiguracji oprogramowania pośredniczącego na początku lub na końcu aplikacji [Konfiguruj](#the-configure-method) potoku oprogramowania pośredniczącego. `IStartupFilter` jest przydatne upewnić się, że oprogramowanie pośredniczące jest uruchamiany przed lub po oprogramowanie pośredniczące dodane przez biblioteki na początku lub końcu potoku przetwarzania żądań aplikacji.

`IStartupFilter` implementuje jedną metodę <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, który odbiera i zwraca `Action<IApplicationBuilder>`. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> Definiuje klasę, aby skonfigurować Potok żądań aplikacji. Aby uzyskać więcej informacji, zobacz [tworzenie potoku oprogramowania pośredniczącego z IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Każdy `IStartupFilter` implementuje middlewares co najmniej jeden w Potok żądań. Filtry są wywoływane w kolejności, w której zostały one dodane do kontenera usług. Filtry mogą dodawać oprogramowanie pośredniczące przed lub po przekazanie sterowania do następnego filtru, dlatego ich dołączania na początku lub końcu potoku aplikacji.

W poniższym przykładzie pokazano, jak zarejestrować oprogramowanie pośredniczące z `IStartupFilter`.

`RequestSetOptionsMiddleware` Oprogramowanie pośredniczące ustawia wartości opcji z parametru ciągu zapytania:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` Jest skonfigurowana w `RequestSetOptionsStartupFilter` klasy:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Jest zarejestrowany w kontenerze usługi w <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> i rozszerzają `Startup` z poza `Startup` klasy:

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

Gdy parametr ciągu zapytania dla `option` zostanie podana, oprogramowanie pośredniczące przetwarza przypisanie wartości, zanim oprogramowanie pośredniczące MVC renderuje odpowiedzi:

![Okno przeglądarki, przedstawiające renderowanej strony indeksu. Wartość opcji jest renderowany jako "Z oprogramowania pośredniczącego" oparte na żądanie strony za pomocą parametru ciągu zapytania i wartość równa "Z oprogramowania pośredniczącego" opcji.](startup/_static/index.png)

Oprogramowanie pośredniczące kolejność wykonywania jest ustawiona według kolejności z `IStartupFilter` rejestracji:

* Wiele `IStartupFilter` implementacji może współpracować z tych samych obiektów. Jeśli kolejność jest ważna, kolejność ich `IStartupFilter` usługi rejestracji, aby dopasować kolejności ich middlewares powinno być ono uruchomione.
* Biblioteki mogą dodawać oprogramowanie pośredniczące z co najmniej `IStartupFilter` implementacji, które są uruchamiane przed lub po innym oprogramowaniu pośredniczącym aplikacji zarejestrowane w usłudze `IStartupFilter`. Aby wywołać `IStartupFilter` oprogramowania pośredniczącego przed oprogramowanie pośredniczące dodane przez bibliotekę `IStartupFilter`, umieść rejestracji usługi, zanim biblioteki jest dodawany do kontenera usługi. Po dodaniu biblioteki, aby wywołać później, umieść rejestracji usługi.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Dodaj konfigurację podczas uruchamiania z zewnętrznego zestawu

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Implementacji umożliwia dodawanie rozszerzeń do aplikacji przy uruchamianiu z zewnętrznego zestawu poza aplikacji `Startup` klasy. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Host](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
