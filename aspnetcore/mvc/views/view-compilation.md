---
title: Kompilacja pliku razor w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak kompilacji plikach Razor odbywa się w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 11195f00e922f6817a0fa0988fad9d8082dea30a
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142317"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Kompilacja pliku razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

Pliku Razor jest kompilowana w czasie wykonywania, gdy jest wywoływany skojarzonego widoku MVC. Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane. Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC. Publikowanie pliku Razor w czasie kompilacji nie jest obsługiwane. Opcjonalnie można kompilować plikach razor na czas publikacji i wdrożonych przy użyciu aplikacji&mdash;przy użyciu narzędzia wstępnej kompilacji.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Pliku Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC. Pliki razor są kompilowane w obu kompilacji i opublikować przy użyciu [Razor SDK](xref:razor-pages/sdk).

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Pliki razor są kompilowane w obu kompilacji i opublikować przy użyciu [Razor SDK](xref:razor-pages/sdk). Kompilacja środowiska uruchomieniowego może być opcjonalnie włączona konfigurując aplikację.

::: moniker-end

## <a name="razor-compilation"></a>Kompilacja razor

::: moniker range=">= aspnetcore-3.0"
Kompilacja i publikowania w czasie kompilacji plików Razor jest domyślnie włączona, przez zestaw SDK Razor. Po włączeniu kompilacja środowiska uruchomieniowego uzupełnienie utworzy czasu kompilacji, dzięki czemu plikach Razor do aktualizacji, jeśli są one editied.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Kompilacja i publikowania w czasie kompilacji plików Razor jest domyślnie włączona, przez zestaw SDK Razor. Po te są aktualizowane w plikach Razor do edycji jest obsługiwana w czasie kompilacji. Domyślnie tylko skompilowanych *Views.dll* i nie *.cshtml* plików lub odwołania do zestawów wymagane do kompilowania plików Razor są wdrażane przy użyciu aplikacji.

> [!IMPORTANT]
> Narzędzie wstępnej kompilacji jest przestarzała i zostanie usunięta w programie ASP.NET Core 3.0. Zalecamy przeprowadzić migrację do [Razor Sdk](xref:razor-pages/sdk).
>
> Zestaw Razor SDK jest efektywne tylko wtedy, gdy brak właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu. Na przykład ustawienie *.csproj* pliku `MvcRazorCompileOnPublish` właściwość `true` wyłącza zestaw Razor SDK.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Jeśli projekt jest przeznaczony dla .NET Framework, należy zainstalować [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.

Szablony projektów programu ASP.NET Core 2.x niejawnie ustaw `MvcRazorCompileOnPublish` właściwość `true` domyślnie. W związku z tym, ten element można bezpiecznie usunąć z *.csproj* pliku.

> [!IMPORTANT]
> Narzędzie wstępnej kompilacji jest przestarzała i zostanie usunięta w programie ASP.NET Core 3.0. Zalecamy przeprowadzić migrację do [Razor Sdk](xref:razor-pages/sdk).
>
> Wstępnej kompilacji pliku razor jest niedostępna podczas wykonywania [niezależna wdrożenia (— SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) programu ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Ustaw `MvcRazorCompileOnPublish` właściwości `true`i zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet. Następujące *.csproj* przykładzie wyróżniono tych ustawień:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Przygotowywanie aplikacji dla [wdrożenia zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [polecenia publikowania interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet-publish). Na przykład wykonaj następujące polecenie w katalogu głównym projektu:

```console
dotnet publish -c Release
```

A  *\<project_name >. PrecompiledViews.dll* zawierający skompilowane pliki Razor jest generowany po pomyślnym zakończeniu wstępnej kompilacji. Na przykład, poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* w ramach *WebApplication1.PrecompiledViews.dll*:

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>Kompilacja środowiska uruchomieniowego

::: moniker range="= aspnetcore-2.1"

Czas kompilacji, kompilacja jest uzupełniana przez kompilacja środowiska uruchomieniowego plików Razor. Platforma ASP.NET Core MVC będzie ponownie kompilować Razor pliki, gdy zawartość *.cshtml* zmianę pliku.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Czas kompilacji, kompilacja jest uzupełniana przez kompilacja środowiska uruchomieniowego plików Razor. <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> Pobiera lub ustawia wartość określającą, jeśli pliki Razor (widokami Razor i stron Razor) są ponownie kompilowane i zaktualizowany w przypadku plików zmienią się na dysku.

Wartość domyślna to `true` dla:

* Jeśli wersja zgodności aplikacji jest równa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> lub starszy
* Wersja zgodności aplikacji czy zestaw do zestawu do <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> lub nowszej, a aplikacja jest w środowisku programistycznym <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>. Innymi słowy, plikach Razor będzie nie należy ponownie skompilować w środowisku programistycznym bez chyba że <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> jest jawnie ustawione.

Wskazówki i przykłady ustawienie wersji zgodności aplikacji, zobacz <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Kompilacja środowiska uruchomieniowego jest włączane przy użyciu `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` pakietu. Aby włączyć kompilacja środowiska uruchomieniowego, aplikacje muszą:

* Zainstaluj [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) pakietu NuGet.
* Aktualizuj aplikację `ConfigureServices` obejmujący wywołanie `AddMvcRazorRuntimeCompilation`:

  ```csharp
  services
      .AddMvc()
      .AddRazorRuntimeCompilation()
  ```

Kompilacja środowiska uruchomieniowego do pracy po wdrożeniu, aplikacje Ponadto należy zmodyfikować swoje pliki projektu, aby ustawić `PreserveCompilationReferences` do `true`.

[!code-xml[](view-compilation/sample/RuntimeCompilation.csproj?highlight=3)]

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
