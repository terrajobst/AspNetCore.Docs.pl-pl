---
title: Kompilacja pliku Razor w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kompilacja plików Razor występuje w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661106"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Kompilacja pliku Razor w ASP.NET Core

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Plik Razor jest kompilowany w czasie wykonywania, gdy jest wywoływany skojarzony widok MVC. Publikowanie plików Razor w czasie kompilacji nie jest obsługiwane. Pliki Razor mogą być opcjonalnie kompilowane w czasie publikowania i wdrażane za pomocą aplikacji&mdash;przy użyciu narzędzia prekompilacji.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Plik Razor jest kompilowany w czasie wykonywania, gdy zostanie wywołana skojarzona Strona Razor lub widok MVC. Publikowanie plików Razor w czasie kompilacji nie jest obsługiwane. Pliki Razor mogą być opcjonalnie kompilowane w czasie publikowania i wdrażane za pomocą aplikacji&mdash;przy użyciu narzędzia prekompilacji.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Plik Razor jest kompilowany w czasie wykonywania, gdy zostanie wywołana skojarzona Strona Razor lub widok MVC. Pliki Razor są kompilowane w czasie kompilacji i publikowania przy użyciu [zestawu Razor SDK](xref:razor-pages/sdk).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Pliki Razor z rozszerzeniem *. cshtml* są kompilowane w czasie kompilacji i publikowania przy użyciu [zestawu Razor SDK](xref:razor-pages/sdk). Kompilacja środowiska uruchomieniowego może być opcjonalnie włączona przez skonfigurowanie aplikacji.

::: moniker-end

## <a name="razor-compilation"></a>Kompilacja Razor

::: moniker range=">= aspnetcore-3.0"

Kompilacja i czas publikowania pliki Razor są domyślnie włączone w zestawie SDK Razor. Gdy ta funkcja jest włączona, Kompilacja środowiska uruchomieniowego uzupełnia kompilację w czasie kompilacji, umożliwiając aktualizowanie plików Razor, jeśli są edytowane.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Kompilacja i czas publikowania pliki Razor są domyślnie włączone w zestawie SDK Razor. Edytowanie plików Razor po ich aktualizacji jest obsługiwane w czasie kompilacji. Domyślnie tylko skompilowane *widoki. dll* i brak plików *. cshtml* lub zestawy odwołań wymagane do kompilowania plików Razor są wdrażane przy użyciu aplikacji.

> [!IMPORTANT]
> Narzędzie prekompilacji jest przestarzałe i zostanie usunięte w ASP.NET Core 3,0. Zalecamy Migrowanie do [zestawu Razor SDK](xref:razor-pages/sdk).
>
> Zestaw SDK Razor działa tylko wtedy, gdy w pliku projektu nie są ustawione właściwości specyficzne dla wstępnej kompilacji. Na przykład ustawienie właściwości `MvcRazorCompileOnPublish` pliku *. csproj* na `true` powoduje wyłączenie zestawu Razor SDK.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Jeśli projekt jest przeznaczony .NET Framework, zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) :

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Jeśli projekt jest przeznaczony dla platformy .NET Core, nie są wymagane żadne zmiany.

Szablony projektu ASP.NET Core 2. x jawnie domyślnie ustawiają Właściwość `MvcRazorCompileOnPublish` na `true`. W związku z tym ten element może zostać bezpiecznie usunięty z pliku *. csproj* .

> [!IMPORTANT]
> Narzędzie prekompilacji jest przestarzałe i zostanie usunięte w ASP.NET Core 3,0. Zalecamy Migrowanie do [zestawu Razor SDK](xref:razor-pages/sdk).
>
> Prekompilacja pliku Razor jest niedostępna podczas wykonywania niezależnego [wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w ASP.NET Core 2,0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Ustaw właściwość `MvcRazorCompileOnPublish` na `true`i zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) . Poniższy przykład *. csproj* przedstawia następujące ustawienia:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Przygotuj aplikację dla [wdrożenia zależnego od platformy](/dotnet/core/deploying/#framework-dependent-deployments-fdd) przy użyciu [polecenia interfejs wiersza polecenia platformy .NET Core Publish](/dotnet/core/tools/dotnet-publish). Na przykład wykonaj następujące polecenie w katalogu głównym projektu:

```dotnetcli
dotnet publish -c Release
```

*> Project_name\<. Plik PrecompiledViews. dll* zawierający skompilowane pliki Razor jest tworzony, gdy kompilacja zakończy się pomyślnie. Na przykład poniższy zrzut ekranu przedstawia zawartość *index. cshtml* w *WebApplication1. PrecompiledViews. dll*:

![Widoki Razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>Kompilacja środowiska uruchomieniowego

::: moniker range="= aspnetcore-2.1"

Kompilacja w czasie kompilacji jest uzupełniana przez kompilację plików Razor w czasie wykonywania. ASP.NET Core MVC będzie ponownie kompilować pliki Razor, gdy zostanie zmieniona zawartość pliku *. cshtml* .

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Kompilacja w czasie kompilacji jest uzupełniana przez kompilację plików Razor w czasie wykonywania. <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Pobiera lub ustawia wartość określającą, czy pliki Razor (widoki Razor i Razor Pages) zostaną ponownie skompilowane i zaktualizowane w przypadku zmiany plików na dysku.

Wartość domyślna to `true` dla:

* Jeśli wersja zgodności aplikacji jest ustawiona na <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> lub wcześniejszą
* Jeśli wersja zgodności aplikacji jest ustawiona na <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> lub nowsza, a aplikacja znajduje się w <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>środowisku deweloperskim. Inaczej mówiąc, pliki Razor nie będą ponownie kompilowane w środowisku nieprogramistycznym, chyba że <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> jest jawnie ustawiona.

Aby uzyskać wskazówki i przykłady dotyczące ustawiania wersji zgodności aplikacji, zobacz <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Aby włączyć kompilację środowiska uruchomieniowego dla wszystkich środowisk i trybów konfiguracji:

1. Zainstaluj pakiet NuGet [Microsoft. AspNetCore. MVC. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) .

1. Zaktualizuj metodę `Startup.ConfigureServices` projektu w celu uwzględnienia wywołania `AddRazorRuntimeCompilation`. Na przykład:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a>Warunkowe włączenie kompilacji środowiska uruchomieniowego

Kompilacja środowiska uruchomieniowego może być włączona w taki sposób, że jest dostępna tylko na potrzeby lokalnego tworzenia. Warunkowe włączenie w ten sposób zapewnia, że opublikowane dane wyjściowe:

* Używa skompilowanych widoków.
* Jest mniejszy niż rozmiar.
* Nie włącza obserwatorów plików w środowisku produkcyjnym.

Aby włączyć kompilację środowiska uruchomieniowego na podstawie trybu środowiska i konfiguracji:

1. Warunkowo odwołuje się do pakietu [Microsoft. AspNetCore. MVC. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) w oparciu o wartość Active `Configuration`:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. Zaktualizuj metodę `Startup.ConfigureServices` projektu w celu uwzględnienia wywołania `AddRazorRuntimeCompilation`. Warunkowo wykonuj `AddRazorRuntimeCompilation` w taki sposób, aby działał tylko w trybie debugowania, gdy zmienna `ASPNETCORE_ENVIRONMENT` jest ustawiona na `Development`:

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
