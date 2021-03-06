---
title: Migrowanie z ASP.NET Core 1. x do 2,0
author: scottaddie
description: W tym artykule opisano wymagania wstępne i Najczęstsze kroki migrowania projektu ASP.NET Core 1. x do ASP.NET Core 2,0.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/1x-to-2x/index
ms.openlocfilehash: c46f50a418cf630980ac2ba94407e4370d36e7d5
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667616"
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a>Migrowanie z ASP.NET Core 1. x do 2,0

Przez [Scott Addie](https://github.com/scottaddie)

W tym artykule przeprowadzimy Cię przez aktualizację istniejącego projektu ASP.NET Core 1. x do ASP.NET Core 2,0. Migrowanie aplikacji do ASP.NET Core 2,0 pozwala korzystać z [wielu nowych funkcji i ulepszeń wydajności](xref:aspnetcore-2.0).

Istniejące aplikacje ASP.NET Core 1. x są oparte na szablonach projektu specyficznych dla wersji. W miarę rozwoju struktury ASP.NET Core, więc szablony projektu i początkowy kod zawarty w nich. Oprócz aktualizowania platformy ASP.NET Core, należy zaktualizować kod aplikacji.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Zobacz [wprowadzenie do ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Aktualizuj moniker platformy docelowej (TFM)

Projekty ukierunkowane na platformę .NET Core powinny korzystać z [TFM](/dotnet/standard/frameworks) wersji nowszej niż lub równej platformie .net Core 2,0. Wyszukaj węzeł `<TargetFramework>` w pliku *. csproj* i Zastąp tekst wewnętrzny tekstem `netcoreapp2.0`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

Projekty ukierunkowane na .NET Framework powinny używać TFM wersji większej lub równej .NET Framework 4.6.1. Wyszukaj węzeł `<TargetFramework>` w pliku *. csproj* i Zastąp tekst wewnętrzny tekstem `net461`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> Program .NET Core 2,0 oferuje znacznie większy obszar powierzchni niż .NET Core 1. x. Jeśli chcesz, aby .NET Framework tylko z powodu brakujących interfejsów API w programie .NET Core 1. x, dla których będzie możliwe działanie programu .NET Core 2,0.

Jeśli plik projektu zawiera `<RuntimeFrameworkVersion>1.{sub-version}</RuntimeFrameworkVersion>`, zobacz [ten problem](https://github.com/dotnet/AspNetCore/issues/3221#issuecomment-413094268)w usłudze GitHub.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Aktualizacja wersji zestaw .NET Core SDK w pliku Global. JSON

Jeśli Twoje rozwiązanie polega na pliku [Global. JSON](/dotnet/core/tools/global-json) , który będzie przeznaczony dla konkretnej wersji zestaw .NET Core SDK, zaktualizuj swoją właściwość `version`, aby użyć wersji 2,0 zainstalowanej na maszynie:

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Aktualizuj odwołania do pakietów

Plik *. csproj* w projekcie 1. x wyświetla każdy pakiet NuGet używany przez ten projekt.

W projekcie ASP.NET Core 2,0 docelowym .NET Core 2,0, pojedyncze odwołanie do [pakietu](xref:fundamentals/metapackage) w pliku *csproj* zastępuje kolekcję pakietów:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

Wszystkie funkcje ASP.NET Core 2,0 i Entity Framework Core 2,0 są zawarte w pakiecie.

Projekty ASP.NET Core 2,0 .NET Framework powinny nadal odwoływać się do poszczególnych pakietów NuGet. Zaktualizuj atrybut `Version` każdego węzła `<PackageReference />` do 2.0.0.

Na przykład poniżej znajduje się lista węzłów `<PackageReference />` używanych w typowym projekcie ASP.NET Core 2,0 .NET Framework:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Narzędzia aktualizacji interfejs wiersza polecenia platformy .NET Core

W pliku *. csproj* zaktualizuj atrybut `Version` każdego węzła `<DotNetCliToolReference />` do 2.0.0.

Na przykład poniżej przedstawiono listę narzędzi interfejsu wiersza polecenia używanych w typowym projekcie ASP.NET Core 2,0, dla którego jest przeznaczony program .NET Core 2,0:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Zmień nazwę właściwości rezerwowej elementu docelowego pakietu

Plik *. csproj* projektu 1. x użył `PackageTargetFallback` węzła i zmiennej:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

Zmień nazwę węzła i zmiennej na `AssetTargetFallback`:

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Aktualizowanie metody Main w Program.cs

W projektach 1. x Metoda `Main` *program.cs* wyglądać następująco:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

W projektach 2,0 Metoda `Main` *program.cs* została uproszczona:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

Przyjęcie tego nowego wzorca 2,0 jest zdecydowanie zalecane i jest wymagane do pracy z funkcjami produktu, takimi jak [Entity Framework (EF) Core](xref:data/ef-mvc/migrations) . Na przykład uruchomienie `Update-Database` z okna konsoli Menedżera pakietów lub `dotnet ef database update` z wiersza polecenia (w projektach przekonwertowanych na ASP.NET Core 2,0) spowoduje wygenerowanie następującego błędu:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a>Dodawanie dostawców konfiguracji

W projektach 1. x Dodawanie dostawców konfiguracji do aplikacji zostało zrealizowane za pośrednictwem konstruktora `Startup`. Kroki związane z tworzeniem wystąpienia `ConfigurationBuilder`, ładowanie odpowiednich dostawców (zmienne środowiskowe, ustawienia aplikacji itp.) oraz inicjowanie składowej `IConfigurationRoot`.

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

W powyższym przykładzie załadujesz składową `Configuration` przy użyciu ustawień konfiguracji z pliku *appSettings. JSON* , a także dowolnych plików *appSettings.\<EnvironmentName\>. JSON* zgodnych z właściwością `IHostingEnvironment.EnvironmentName`. Lokalizacja tych plików jest taka sama jak ścieżka *Startup.cs*.

W projektach 2,0, kod konfiguracji standardowa nieodłącz się od projektów 1. x działa w tle. Na przykład zmienne środowiskowe i ustawienia aplikacji są ładowane podczas uruchamiania. Równoważny kod *Startup.cs* został zredukowany do inicjowania `IConfiguration` przy użyciu wstrzykniętego wystąpienia:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

Aby usunąć domyślnych dostawców dodawanych przez `WebHostBuilder.CreateDefaultBuilder`, wywołaj metodę `Clear` na właściwości `IConfigurationBuilder.Sources` wewnątrz `ConfigureAppConfiguration`. Aby dodać dostawców z powrotem, użyj metody `ConfigureAppConfiguration` w *program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

W [tym miejscu](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152)można zobaczyć konfigurację używaną przez metodę `CreateDefaultBuilder` w poprzednim fragmencie kodu.

Aby uzyskać więcej informacji, zobacz [Konfiguracja w ASP.NET Core](xref:fundamentals/configuration/index).

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a>Przenieś kod inicjalizacji bazy danych

W projektach 1. x używających EF Core 1. x polecenie takie jak `dotnet ef migrations add` wykonuje następujące czynności:

1. Tworzy wystąpienie `Startup` wystąpienia
1. Wywołuje metodę `ConfigureServices`, aby zarejestrować wszystkie usługi przy użyciu iniekcji zależności (w tym typów `DbContext`)
1. Wykonuje wymagane zadania

W przypadku projektów 2,0 przy użyciu EF Core 2,0 `Program.BuildWebHost` jest wywoływana w celu uzyskania usług aplikacji. W przeciwieństwie do 1. x, ma to dodatkowy efekt uboczny wywoływania `Startup.Configure`. Jeśli aplikacja 1. x wywołała kod inicjalizacji bazy danych w metodzie `Configure`, mogą wystąpić nieoczekiwane problemy. Na przykład jeśli baza danych jeszcze nie istnieje, kod inicjujący jest uruchamiany przed wykonaniem polecenia EF Core migracji. Ten problem powoduje niepowodzenie polecenia `dotnet ef migrations list`, jeśli baza danych jeszcze nie istnieje.

Rozważmy następujący kod inicjujący inicjatora 1. x w metodzie `Configure` *Startup.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

W projektach 2,0, Przenieś `SeedData.Initialize` wywołanie do metody `Main` *program.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

Od 2,0 jest to niewłaściwe rozwiązanie w `BuildWebHost` z wyjątkiem kompilacji i konfiguracji hosta sieci Web. Wszystkie elementy, które mają na celu uruchomienie aplikacji, powinny być obsługiwane poza `BuildWebHost` &mdash; zwykle w metodzie `Main` *program.cs*.

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a>Przegląd ustawienia kompilacji widoku Razor

Skrócenie czasu uruchamiania aplikacji i mniejszych opublikowanych pakietów mają na celu najwyższą ważność. Z tego względu [kompilacja widoku Razor](xref:mvc/views/view-compilation) jest domyślnie włączona w ASP.NET Core 2,0.

Ustawienie dla właściwości `MvcRazorCompileOnPublish` wartości true nie jest już wymagane. Jeśli nie wyłączysz kompilacji widoku, właściwość może zostać usunięta z pliku *. csproj* .

Podczas określania wartości docelowej .NET Framework nadal trzeba jawnie odwoływać się do pakietu NuGet [Microsoft. AspNetCore. MVC. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) w pliku *. csproj* :

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Korzystaj z funkcji "lekkich" Application Insights

Istotna konfiguracja Instrumentacji wydajności aplikacji jest bardzo ważna. Teraz można polegać na nowych funkcjach [Application Insights](/azure/application-insights/app-insights-overview) "świateł-up" dostępnych w narzędziach programu Visual Studio 2017.

Projekty ASP.NET Core 1,1 utworzone w programie Visual Studio 2017 zostały dodane domyślnie Application Insights. Jeśli nie używasz bezpośrednio zestawu SDK Application Insights, poza *program.cs* i *Startup.cs*, wykonaj następujące kroki:

1. Jeśli celem jest .NET Core, usuń następujący węzeł `<PackageReference />` z pliku *. csproj* :

    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. W przypadku określania platformy .NET Core Usuń wywołanie metody rozszerzenia `UseApplicationInsights` z *program.cs*:

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Usuń Application Insights wywołanie interfejsu API po stronie klienta z *_Layout. cshtml*. Obejmuje dwa następujące wiersze kodu:

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

Jeśli używasz bezpośrednio zestawu SDK Application Insights, Kontynuuj. [Pakiet](xref:fundamentals/metapackage) "2,0" zawiera najnowszą wersję Application Insights, więc w przypadku odwoływania się do starszej wersji pojawia się błąd obniżenia poziomu pakietów.

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a>Przyjmowanie ulepszeń uwierzytelniania/tożsamości

ASP.NET Core 2,0 ma nowy model uwierzytelniania i wiele znaczących zmian dotyczących ASP.NET Core tożsamości. Jeśli projekt został utworzony z włączonymi indywidualnymi kontami użytkowników lub ręcznie dodaliśmy uwierzytelnianie lub tożsamość, zobacz [Migrowanie uwierzytelniania i tożsamości do ASP.NET Core 2,0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Istotne zmiany w ASP.NET Core 2,0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
