---
title: Migracja z platformy ASP.NET Core 1.x 2.0
author: scottaddie
description: W tym artykule opisano wymagania wstępne i najbardziej typowe kroki dotyczące migrowania projekt platformy ASP.NET Core 1.x ASP.NET Core 2.0.
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: c462f38ba345a9eaf648967524cadd1ba45aee19
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a>Migracja z platformy ASP.NET Core 1.x 2.0

Przez [Scott Addie](https://github.com/scottaddie)

W tym artykule pomożemy Ci przez aktualizowanie istniejącego projektu platformy ASP.NET Core 1.x do składnika ASP.NET 2.0 Core. Migrowanie aplikacji platformy ASP.NET Core 2.0 umożliwia korzystanie z [wiele nowe funkcje i ulepszenia wydajności](xref:aspnetcore-2.0). 

Istniejące aplikacje 1.x platformy ASP.NET Core są oparte na znajdujące się na nich szablony przeznaczone do wersji projektu. W miarę rozwoju platformy ASP.NET Core framework środowisko tak zrobić szablonów projektu i kod startowy zawarte w nich. Oprócz aktualizacji w ramach platformy ASP.NET Core, musisz zaktualizować kod aplikacji.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne
Zobacz [wprowadzenie do platformy ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Zaktualizuj Moniker platformy docelowej (TFM)
Należy używać w projektach przeznaczonych dla platformy .NET Core [TFM](/dotnet/standard/frameworks#referring-to-frameworks) wersji większa lub równa .NET Core 2.0. Wyszukaj `<TargetFramework>` w węźle *.csproj* plików i zastąp jego tekst wewnętrzny z `netcoreapp2.0`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

Projektów przeznaczanie platformy .NET należy używać TFM wersji większa lub równa .NET Framework 4.6.1. Wyszukaj `<TargetFramework>` w węźle *.csproj* plików i zastąp jego tekst wewnętrzny z `net461`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET core 2.0 oferuje znacznie większą powierzchni niż .NET Core 1.x. Jeśli aplikacja jest przeznaczona dla .NET Framework wyłącznie z powodu brakującego interfejs API programu .NET Core 1.x przeznaczonych dla platformy .NET Core 2.0 jest zwykle działa.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Zaktualizuj wersję zestawu SDK programu .NET Core w global.json
Jeśli rozwiązanie opiera się na [ *global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) plik, aby zastosować określoną wersję zestawu SDK programu .NET Core, zaktualizuj jego `version` właściwości korzysta z wersji 2.0 na komputerze jest zainstalowany:

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Odwołania do pakietu aktualizacji
*.Csproj* każdy pakiet NuGet używane przez projekt zawiera listę plików w projekcie 1.x.

W projekcie platformy ASP.NET Core 2.0 przeznaczonych dla platformy .NET Core 2.0, jeden [metapackage](xref:fundamentals/metapackage) odwołania w *.csproj* plik zastępuje kolekcję pakietów:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

Wszystkie funkcje platformy ASP.NET Core 2.0 oraz Entity Framework Core 2.0 znajdują się w metapackage.

Platformy ASP.NET Core 2.0 projektów przeznaczanie platformy .NET ma przejść do odwołania się do poszczególnych pakietów NuGet. Aktualizacja `Version` atrybut każdego `<PackageReference />` węzeł 2.0.0.

Na przykład poniżej przedstawiono listę `<PackageReference />` węzłów używany w projekcie platformy ASP.NET Core 2.0 typowe przeznaczonych dla platformy .NET Framework:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Aktualizacja narzędzi interfejsu wiersza polecenia platformy .NET Core
W *.csproj* plików, zaktualizuj `Version` atrybut każdego `<DotNetCliToolReference />` węzeł 2.0.0.

Na przykład poniżej przedstawiono listę narzędzi interfejsu wiersza polecenia używane w typowych projektu programu ASP.NET 2.0 Core przeznaczonych dla platformy .NET Core 2.0:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Zmień nazwę właściwości pakietu docelowy powrotu
*.Csproj* pliku projektu 1.x używane `PackageTargetFallback` węzeł i zmienną:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Zmień nazwę węzeł i zmienna `AssetTargetFallback`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Metoda Main w pliku Program.cs aktualizacji
W projektach 1.x `Main` metody *Program.cs* po zapoznaniu się następująco:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

W projektach 2.0 `Main` metody *Program.cs* uproszczono:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Zastosowanie tego wzorca nowych 2.0 zdecydowanie zaleca się i jest wymagane dla produktu funkcji, takich jak [migracje Core Entity Framework (EF)](xref:data/ef-mvc/migrations) do pracy. Na przykład uruchomiona `Update-Database` z okna konsoli Menedżera pakietów lub `dotnet ef database update` polecenia wiersza (w przypadku projektów przekonwertować ASP.NET Core 2.0) generuje następujący błąd:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Dodawanie dostawcy konfiguracji
W projektach 1.x wielkoskalowych został Dodawanie dostawcy konfiguracji do uruchamianych aplikacji `Startup` konstruktora. Kroki związane z tworzenia wystąpienia `ConfigurationBuilder`, ładowanie odpowiednich dostawców (zmienne środowiskowe, ustawienia aplikacji, itp.) i Inicjowanie członkiem `IConfigurationRoot`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

Ładuje w poprzednim przykładzie `Configuration` elementu członkowskiego przy użyciu ustawień konfiguracyjnych z *appsettings.json* oraz wszelkie *appsettings.\< EnvironmentName\>JSON* Dopasowywanie plików `IHostingEnvironment.EnvironmentName` właściwości. Lokalizacja tych plików jest w tej samej ścieżce jako *Startup.cs*.

W projektach 2.0 schematyczny kod konfiguracji związane z projektami 1.x uruchamia wewnętrznych. Na przykład zmienne środowiskowe i ustawienia aplikacji są ładowane podczas uruchamiania. Odpowiednik *Startup.cs* kodu zostanie zmniejszona do `IConfiguration` inicjowania z wprowadzonym wystąpieniem:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

Aby usunąć domyślnych dodane przez dostawców `WebHostBuilder.CreateDefaultBuilder`, wywołania `Clear` metoda `IConfigurationBuilder.Sources` właściwości wewnątrz `ConfigureAppConfiguration`. Dodaj ponownie dostawców, należy skorzystać `ConfigureAppConfiguration` metody w *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

Konfiguracja używana przez `CreateDefaultBuilder` są widoczne w poprzednim fragment kodu metody [tutaj](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).

Aby uzyskać więcej informacji, zobacz [konfiguracji w programie ASP.NET Core](xref:fundamentals/configuration/index).

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Przenieś kod inicjujący bazy danych
W projektach 1.x przy użyciu EF Core 1.x, polecenia, takich jak `dotnet ef migrations add` wykonuje następujące czynności:
1. Tworzy wystąpienie `Startup` wystąpienia
2. Wywołuje `ConfigureServices` metody do rejestrowania wszystkich usług z iniekcji zależności (w tym `DbContext` typów)
3. Wykonuje jego wymagane zadania

W projektach 2.0 za pomocą EF Core 2.0 `Program.BuildWebHost` jest wywoływane w celu uzyskania usług aplikacji. W odróżnieniu od 1.x, to ma dodatkowe ubocznym wywoływania `Startup.Configure`. Jeśli aplikacja 1.x wywoływany kod inicjujący bazy danych w jego `Configure` metody, mogą wystąpić nieoczekiwane problemy. Na przykład jeśli jeszcze nie istnieje bazy danych, rozmieszczania kod jest uruchamiany przed wykonaniem polecenia EF Core migracji. Ten problem powoduje, że `dotnet ef migrations list` polecenie, aby zakończyć się niepowodzeniem, jeśli jeszcze nie istnieje bazy danych.

Należy wziąć pod uwagę następujące kod inicjujący inicjatora 1.x w `Configure` metody *Startup.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

W projektach 2.0, należy przenieść `SeedData.Initialize` wywołanie `Main` metody *Program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

Począwszy od 2.0, wykonywać żadnych czynności zły rozwiązaniem jest `BuildWebHost` z wyjątkiem kompilacji i skonfigurować hosta sieci web. Wszystko, co jest o uruchamianiu aplikacji powinno zostać obsłużone poza `BuildWebHost` &mdash; zwykle w `Main` metody *Program.cs*.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Przejrzyj ustawienia kompilacji widoku Razor
Skrócić czas uruchamiania aplikacji i mniejszych opublikowane pakiety są największe znaczenie dla Ciebie. Z tego względu [kompilacji widoku Razor](xref:mvc/views/view-compilation) jest domyślnie włączone w programie ASP.NET 2.0 Core.

Ustawienie `MvcRazorCompileOnPublish` właściwości na wartość true nie jest już wymagane. O ile nie jest wyłączenie widoku kompilacji, właściwość może zostać usunięte z *.csproj* pliku.

Jeśli celem .NET Framework, nadal należy jawnie odwoływać się do [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) pakietu NuGet w Twojej *.csproj* pliku:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Funkcje "w górę jasny" usługi Application Insights
Ważne jest łatwe ustawień Instrumentacji wydajność aplikacji. Teraz polega na nowym [usługi Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "światła w górę" Funkcje dostępne w narzędzi Visual Studio 2017 r.

Domyślnie projektów platformy ASP.NET Core 1.1 utworzonych w programie Visual Studio 2017 dodany usługi Application Insights. Jeśli nie używasz zestawu SDK usługi Application Insights bezpośrednio, poza *Program.cs* i *Startup.cs*, wykonaj następujące kroki:

1. Jeśli przeznaczonych dla platformy .NET Core, Usuń następujące `<PackageReference />` węzła z *.csproj* pliku:
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. Jeśli przeznaczonych dla platformy .NET Core, Usuń `UseApplicationInsights` wywołanie metody rozszerzenia z *Program.cs*:

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Usuń wywołanie interfejsu API klienta usługi Application Insights z *_Layout.cshtml*. Obejmuje on następujące dwa wiersze kodu:

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Jeśli korzystasz z zestawu SDK usługi Application Insights bezpośrednio, nadal można to zrobić. 2.0 [metapackage](xref:fundamentals/metapackage) zawiera najnowszą wersję usługi Application Insights, więc błąd starszą wersję pakietu jest dostępna w przypadku odwołujesz starszej wersji.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Przyjąć tożsamość/uwierzytelniania ulepszenia
Platformy ASP.NET Core 2.0 ma nowy model uwierzytelniania i Liczba znaczących zmian dotyczących tożsamości platformy ASP.NET Core. Jeśli utworzony projekt z włączoną indywidualnych kont użytkowników, lub jeśli ręcznie dodano uwierzytelniania lub tożsamości, zobacz [migracji uwierzytelnianie i tożsamość platformy ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Fundamentalne zmiany w podstawowej platformy ASP.NET 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
