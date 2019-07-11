---
title: Migracja z programu ASP.NET Core 1.x do 2.0
author: scottaddie
description: W tym artykule omówiono wymagania wstępne oraz najbardziej typowe etapy migracji projektu ASP.NET Core 1.x do ASP.NET Core 2.0.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/1x-to-2x/index
ms.openlocfilehash: 056930f3c586153d13555bbb6036f46587e2352d
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815087"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a>Migracja z programu ASP.NET Core 1.x do 2.0

Przez [Scott Addie](https://github.com/scottaddie)

W tym artykule przedstawiono aktualizowania istniejącego projektu platformy ASP.NET Core 1.x do ASP.NET Core 2.0. Migrowanie aplikacji ASP.NET Core 2.0 umożliwia korzystanie z zalet [wiele nowych funkcji i ulepszeń wydajności](xref:aspnetcore-2.0).

Istniejące aplikacje programu ASP.NET Core 1.x opierają się poza szablony projektów specyficznych dla wersji. Zgodnie z rozwojem platformę ASP.NET Core tak zrobić, szablony projektów i kod startowy w nich zawarte. Oprócz aktualizowanie platformę ASP.NET Core, musisz zaktualizować kod aplikacji.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Zobacz [Rozpoczynanie pracy z platformą ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Aktualizuj Moniker platformy docelowej (TFM)

Projekty przeznaczone dla platformy .NET Core, należy użyć [TFM](/dotnet/standard/frameworks) wersji większa lub równa .NET Core 2.0. Wyszukaj `<TargetFramework>` w węźle *.csproj* plik i zastąp jego tekst wewnętrzny z `netcoreapp2.0`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

Projekty przeznaczone dla .NET Framework, należy użyć elementu TFM wersji większy lub równy .NET Framework 4.6.1. Wyszukaj `<TargetFramework>` w węźle *.csproj* plik i zastąp jego tekst wewnętrzny z `net461`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET core 2.0 oferuje znacznie większy obszar powierzchni niż .NET Core 1.x. Jeśli jest przeznaczony dla .NET Framework, wyłącznie ze względu na Brak interfejsów API na platformie .NET Core 1.x, przeznaczonych dla platformy .NET Core 2.0 jest zwykle działa.

Jeśli plik projektu zawiera `<RuntimeFrameworkVersion>1.{sub-version}</RuntimeFrameworkVersion>`, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/3221#issuecomment-413094268).

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Aktualizacja wersji zestawu SDK programu .NET Core w global.json

Jeśli rozwiązanie opiera się na [ *global.json* ](/dotnet/core/tools/global-json) plik, aby odwoływać się do określonej wersji zestawu .NET Core SDK, zaktualizuj jego `version` właściwości do korzystania z wersji 2.0 zainstalowany na komputerze:

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Aktualizuj odwołania do pakietu

*.Csproj* pliku w projekcie 1.x wyświetla każdy pakiet NuGet, używanych przez projekt.

W projektach programu ASP.NET Core 2.0 przeznaczonych dla platformy .NET Core 2.0, pojedynczy [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) odwoływać w *.csproj* plik zastępuje kolekcję pakietów:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

Wszystkie funkcje platformy ASP.NET Core 2.0 i Entity Framework Core 2.0 są uwzględnione w meta Microsoft.aspnetcore.all.

Projekty programu ASP.NET Core 2.0, przeznaczone dla .NET Framework powinny nadal odwoływać się do poszczególnych pakietów NuGet. Aktualizacja `Version` atrybut każdego `<PackageReference />` węzeł 2.0.0.

Na przykład poniżej przedstawiono listę `<PackageReference />` węzłów użytych w typowym projekcie platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Framework:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Aktualizowanie narzędzi interfejsu wiersza polecenia platformy .NET Core

W *.csproj* pliku, zaktualizuj `Version` atrybut każdego `<DotNetCliToolReference />` węzeł 2.0.0.

Na przykład poniżej przedstawiono listę narzędzi interfejsu wiersza polecenia używane w typowym projekcie platformy ASP.NET Core 2.0, przeznaczonych dla platformy .NET Core 2.0:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Zmień nazwę właściwości rezerwowe docelowego pakietu

*.Csproj* pliku projektu 1.x używane `PackageTargetFallback` węzła i zmiennej:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Zmień nazwę węzła i zmienną `AssetTargetFallback`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Zaktualizuj metodę Main w pliku Program.cs

W projektach 1.x `Main` metody *Program.cs* zapoznaniu się następująco:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

W projektach 2.0 `Main` metody *Program.cs* został uproszczony:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Wdrożenie tego wzorca nowe 2.0 zdecydowanie zaleca się i jest wymagana dla funkcji produktów, takich jak [migracje Core Entity Framework (EF)](xref:data/ef-mvc/migrations) do pracy. Aby na przykład uruchomić `Update-Database` z okna konsoli Menedżera pakietów lub `dotnet ef database update` polecenia wiersza (na projekty na platformie ASP.NET Core 2.0) generuje następujący błąd:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Dodawanie dostawcy konfiguracji

W projektach 1.x Dodawanie dostawcy konfiguracji aplikacji było wykonywane za pośrednictwem `Startup` konstruktora. Kroki trzeba tworzenia wystąpienia obiektu `ConfigurationBuilder`, ładowania odpowiednich dostawców (zmienne środowiskowe, ustawienia aplikacji itp.) i Inicjowanie członkiem `IConfigurationRoot`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

Poprzedni przykład ładuje `Configuration` Członkowskie przy użyciu ustawień konfiguracyjnych z *appsettings.json* oraz dowolne *appsettings.\< EnvironmentName\>.json* pliku dopasowania `IHostingEnvironment.EnvironmentName` właściwości. Lokalizacja tych plików znajduje się w tej samej ścieżce co *Startup.cs*.

W 2.0 projektów standardowy kod konfiguracji zarezerwowanymi 1.x projektów działa w tle. Na przykład zmienne środowiskowe i ustawienia aplikacji są ładowane podczas uruchamiania. Odpowiednik *Startup.cs* kodu jest ograniczona do `IConfiguration` inicjowania za pomocą wprowadzonego wystąpienie:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

Można usunąć domyślnych dostawców dodane przez `WebHostBuilder.CreateDefaultBuilder`, wywołaj `Clear` metody `IConfigurationBuilder.Sources` właściwości wewnątrz `ConfigureAppConfiguration`. Aby dodać ponownie dostawców, korzystanie z `ConfigureAppConfiguration` method in Class metoda *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

Konfiguracja używana przez `CreateDefaultBuilder` metody w poprzednim fragmencie kodu widać [tutaj](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).

Aby uzyskać więcej informacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Przenieś kod inicjowania bazy danych

W projektach 1.x przy użyciu programu EF Core 1.x, polecenia takie jak `dotnet ef migrations add` wykonuje następujące czynności:

1. Tworzy wystąpienie `Startup` wystąpienia
1. Wywołuje `ConfigureServices` metody do rejestrowania wszystkich usług przy użyciu iniekcji zależności (w tym `DbContext` typów)
1. Wykonuje jego wymagane zadania

W projektach 2.0 przy użyciu programu EF Core 2.0 `Program.BuildWebHost` wywoływana w celu uzyskania usług aplikacji. W przeciwieństwie do 1.x, to ma dodatkowe efekt uboczny wywoływania `Startup.Configure`. Jeśli aplikacja 1.x wywoływany kod inicjowania bazy danych w jego `Configure` metody, mogą wystąpić nieoczekiwane problemy. Na przykład jeśli jeszcze nie istnieje baza danych, rozmieszczania kod jest uruchamiany przed wykonaniem polecenia migracji programu EF Core. Ten problem powoduje, że `dotnet ef migrations list` polecenie, aby zakończyć się niepowodzeniem, jeśli baza danych jeszcze nie istnieje.

Rozważmy poniższy kod inicjowania inicjatora 1.x w `Configure` metody *Startup.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

W projektach 2.0, należy przenieść `SeedData.Initialize` wywołanie `Main` metody *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

Począwszy od wersji 2.0, to nic robić złym zwyczajem `BuildWebHost` z wyjątkiem kompilacji i skonfigurować hosta sieci web. Wszystko, co jest o uruchamianiu aplikacji powinno zostać obsłużone poza `BuildWebHost` &mdash; zwykle w `Main` metody *Program.cs*.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Sprawdź ustawienie kompilacja widoku Razor

Krótszy czas uruchamiania aplikacji i mniejsze pakiety opublikowane ma priorytetowe znaczenie dla Ciebie. Z tego względu [kompilacja widoku Razor](xref:mvc/views/view-compilation) jest domyślnie włączona w programie ASP.NET Core 2.0.

Ustawienie `MvcRazorCompileOnPublish` właściwości na wartość true nie jest już wymagane. O ile nie jest wyłączenie kompilacja widoku, właściwości mogą zostać usunięte z *.csproj* pliku.

Gdy przeznaczonych dla platformy .NET Framework, nadal należy jawnie odwoływać się do [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) pakietu NuGet w swojej *.csproj* pliku:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Zależą od funkcji "światła w górę" usługi Application Insights

Ważne jest bezproblemowa ustawień Instrumentacji wydajności aplikacji. Teraz możesz polegać na nowym [usługi Application Insights](/azure/application-insights/app-insights-overview) "światła w górę" Funkcje dostępne w narzędzia programu Visual Studio 2017.

Domyślnie projekty ASP.NET Core 1.1, utworzone w programie Visual Studio 2017 dodany usługi Application Insights. Jeśli nie używasz zestawu SDK usługi Application Insights bezpośrednio, poza *Program.cs* i *Startup.cs*, wykonaj następujące kroki:

1. Jeśli przeznaczony dla platformy .NET Core, należy usunąć następujące `<PackageReference />` węzła z *.csproj* pliku:

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. Jeśli przeznaczony dla platformy .NET Core, Usuń `UseApplicationInsights` wywołanie metody rozszerzenia z *Program.cs*:

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Usuń wywołanie interfejsu API usługi Application Insights po stronie klienta z *_Layout.cshtml*. Obejmuje on następujące dwa wiersze kodu:

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Jeśli używasz zestawu SDK usługi Application Insights bezpośrednio, nadal można to zrobić. W wersji 2.0 [meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage) zawiera najnowszą wersję usługi Application Insights, więc pojawia się błąd starszą wersję pakietu, jeśli masz odwołujące się do starszej wersji.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Przyjęcie ulepszenia uwierzytelniania/tożsamości

Platforma ASP.NET Core 2.0 ma nowy model uwierzytelniania i Liczba znaczących zmian w tożsamości platformy ASP.NET Core. Jeśli projekt utworzony z indywidualnymi kontami użytkowników, włączona lub jeśli ręcznie dodano uwierzytelniania lub tożsamości, zobacz [migracji uwierzytelnianie i tożsamość do ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Istotne zmiany w programie ASP.NET Core 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
