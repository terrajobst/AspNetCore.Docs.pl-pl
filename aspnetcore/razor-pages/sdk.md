---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/26/2020
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 2284131ce2d45ec6bc01ce38f91e2c951b108605
ms.sourcegitcommit: f3b1bcfd108e5d53f73abc0bf2555890869d953b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80321012"
---
# <a name="aspnet-core-razor-sdk"></a>Platforma ASP.NET Core Razor SDK

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Omówienie

[!INCLUDE[](~/includes/2.1-SDK.md)] zawiera `Microsoft.NET.Sdk.Razor` SDK MSBuild (Razor SDK). Zestaw Razor SDK:

::: moniker range=">= aspnetcore-3.0"

* Jest wymagana do kompilowania, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC lub [Blazor](xref:blazor/index) .
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementów, które umożliwiają dostosowanie kompilacji plików Razor ( *. cshtml* lub *. Razor*).

Zestaw SDK Razor zawiera elementy `Content` z atrybutami `Include` ustawionymi na `**\*.cshtml` i `**\*.razor` wzorców obsługi symboli wieloznacznych. Zgodne pliki są publikowane.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Standaryzacja środowiska tworzenia, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC.
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.

Zestaw SDK Razor zawiera element `Content` z atrybutem `Include` ustawionym na wzorzec `**\*.cshtml` obsługi symboli wieloznacznych. Zgodne pliki są publikowane.

::: moniker-end

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Korzystanie z zestawu SDK Razor

Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.

::: moniker range=">= aspnetcore-3.0"

Aby użyć zestawu Razor SDK do kompilowania bibliotek klas zawierających widoki Razor lub Razor Pages, zalecamy rozpoczęcie od szablonu projektu biblioteki klas Razor (RCL). RCL, który jest używany do kompilowania plików Blazor ( *. Razor*) w minimalnym stopniu wymaga odwołania do pakietu [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) . RCL, który jest używany do kompilowania widoków Razor lub stron (pliki *. cshtml* ), wymaga minimalnej `netcoreapp3.0` lub nowszej i ma `FrameworkReference` do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:

* Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Zazwyczaj odwołanie do pakietu `Microsoft.AspNetCore.Mvc` jest wymagane do uzyskania dodatkowych zależności, które są wymagane do kompilowania i kompilowania widoków Razor Pages i Razor. Co najmniej projektu należy dodać odwołania do pakietu do:

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  Pakiet `Microsoft.AspNetCore.Razor.Design` zawiera zadania kompilacji Razor i elementy docelowe dla projektu.

  Poprzednie pakiety są zawarte w `Microsoft.AspNetCore.Mvc`. Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> Pakiety `Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Jednakże Dokumentacja pakietu `Microsoft.AspNetCore.App` bez wersji zapewnia pakiet dla aplikacji, która nie zawiera najnowszej wersji `Microsoft.AspNetCore.Razor.Design`. Projekty muszą odwoływać się do spójnej wersji `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`), aby zostały uwzględnione najnowsze poprawki czasu kompilacji dla aparatu Razor. Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/Razor/issues/2553)w serwisie GitHub.

::: moniker-end

### <a name="properties"></a>Właściwości

Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:

* `RazorCompileOnBuild` &ndash; podczas `true`, kompiluje i emituje zestaw Razor w ramach kompilowania projektu. Wartość domyślna to `true`.
* `RazorCompileOnPublish` &ndash; podczas `true`, kompiluje i emituje zestaw Razor w ramach publikowania projektu. Wartość domyślna to `true`.

Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Począwszy od ASP.NET Core 3,0, widoki MVC lub Razor Pages nie są obsługiwane domyślnie, jeśli właściwości `RazorCompileOnBuild` lub `RazorCompileOnPublish` programu MSBuild w pliku projektu są wyłączone. Aplikacje muszą dodać jawne odwołanie do pakietu [Microsoft. AspNetCore. MVC. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) , jeśli aplikacja korzysta z kompilacji środowiska uruchomieniowego w celu przetworzenia plików *. cshtml* .

::: moniker-end

| Items | Opis |
| ----- | ----------- |
| `RazorGenerate` | Elementy elementu (pliki *. cshtml* ), które są danymi wejściowymi do generowania kodu. |
| `RazorComponent` | Elementy elementu (pliki*Razor* ), które są danymi wejściowymi do generowania kodu składnika Razor. |
| `RazorCompile` | Elementy elementów (pliki*CS* ), które są danymi wejściowymi do elementów docelowych kompilacji Razor. Użyj tego `ItemGroup`, aby określić dodatkowe pliki do skompilowania do zestawu Razor. |
| `RazorTargetAssemblyAttribute` | Element służącego do kodu generowania atrybuty dla zestawu Razor. Na przykład:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone. |

::: moniker range=">= aspnetcore-3.0"

| Właściwość | Opis |
| -------- | ----------- |
| `RazorTargetName` | Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor. |
| `RazorOutputPath` | Katalog wyjściowy Razor. |
| `RazorCompileToolset` | Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor. Prawidłowe wartości to `Implicit`, `RazorSDK`i `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Wartość domyślna to `true`. Gdy `true`, zawiera pliki *Web. config*, *. JSON*i *. cshtml* jako zawartość projektu. W odwołującym się do `Microsoft.NET.Sdk.Web`pliki katalogu *wwwroot* i plików konfiguracyjnych są również uwzględniane. |
| `EnableDefaultRazorGenerateItems` | Gdy `true`, zawiera pliki *. cshtml* z `Content` elementy w `RazorGenerate` elementy. |
| `GenerateRazorTargetAssemblyInfo` | Gdy `true`, program generuje plik *CS* zawierający atrybuty określone przez `RazorAssemblyAttribute` i zawiera plik w danych wyjściowych kompilacji. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Gdy `true`, program dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Gdy `true`, kopiuje pliki elementów `RazorGenerate` ( *. cshtml*) do katalogu publikowania. Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania. Wartość domyślna to `false`. |
| `PreserveCompilationReferences` | Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania. Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania. Ustaw, aby `true`, jeśli opublikowana aplikacja wymaga kompilacji środowiska uruchomieniowego. Na przykład ustaw wartość `true`, jeśli aplikacja modyfikuje pliki *. cshtml* w czasie wykonywania lub użyje widoków osadzonych. Wartość domyślna to `false`. |
| `IncludeRazorContentInPack` | Gdy `true`, wszystkie elementy zawartości Razor (pliki *. cshtml* ) są oznaczone do uwzględnienia w wygenerowanym pakiecie NuGet. Wartość domyślna to `false`. |
| `EmbedRazorGenerateSources` | Gdy `true`, program dodaje elementy RazorGenerate ( *. cshtml*) jako osadzone pliki do wygenerowanego zestawu Razor. Wartość domyślna to `false`. |
| `UseRazorBuildServer` | Gdy `true`, program używa procesu trwałego serwera kompilacji do odciążenia działania generowania kodu. Wartość domyślna to `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Gdy `true`, zestaw SDK generuje dodatkowe atrybuty używane przez MVC w czasie wykonywania w celu przeprowadzenia odnajdywania części aplikacji. |
| `DefaultWebContentItemExcludes` | Wzorzec obsługi symboli wieloznacznych dla elementów elementów, które mają zostać wykluczone z grupy elementów `Content` w projektach przeznaczonych dla zestawu SDK sieci Web lub Razor |
| `ExcludeConfigFilesFromBuildOutput` | W przypadku `true`pliki *config* i *JSON* nie są kopiowane do katalogu wyjściowego kompilacji. |
| `AddRazorSupportForMvc` | Gdy `true`, program skonfiguruje zestaw SDK Razor do dodawania obsługi konfiguracji MVC, która jest wymagana podczas kompilowania aplikacji zawierających widoki MVC lub Razor Pages. Ta właściwość jest niejawnie ustawiona dla projektów .NET Core 3,0 lub nowszych przeznaczonych dla zestawu SDK sieci Web |
| `RazorLangVersion` | Wersja języka Razor do celów. |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Właściwość | Opis |
| -------- | ----------- |
| `RazorTargetName` | Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor. |
| `RazorOutputPath` | Katalog wyjściowy Razor. |
| `RazorCompileToolset` | Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor. Prawidłowe wartości to `Implicit`, `RazorSDK`i `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Wartość domyślna to `true`. Gdy `true`, zawiera pliki *Web. config*, *. JSON*i *. cshtml* jako zawartość projektu. W odwołującym się do `Microsoft.NET.Sdk.Web`pliki katalogu *wwwroot* i plików konfiguracyjnych są również uwzględniane. |
| `EnableDefaultRazorGenerateItems` | Gdy `true`, zawiera pliki *. cshtml* z `Content` elementy w `RazorGenerate` elementy. |
| `GenerateRazorTargetAssemblyInfo` | Gdy `true`, program generuje plik *CS* zawierający atrybuty określone przez `RazorAssemblyAttribute` i zawiera plik w danych wyjściowych kompilacji. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Gdy `true`, program dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Gdy `true`, kopiuje pliki elementów `RazorGenerate` ( *. cshtml*) do katalogu publikowania. Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania. Wartość domyślna to `false`. |
| `CopyRefAssembliesToPublishDirectory` | Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania. Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania. Ustaw, aby `true`, jeśli opublikowana aplikacja wymaga kompilacji środowiska uruchomieniowego. Na przykład ustaw wartość `true`, jeśli aplikacja modyfikuje pliki *. cshtml* w czasie wykonywania lub użyje widoków osadzonych. Wartość domyślna to `false`. |
| `IncludeRazorContentInPack` | Gdy `true`, wszystkie elementy zawartości Razor (pliki *. cshtml* ) są oznaczone do uwzględnienia w wygenerowanym pakiecie NuGet. Wartość domyślna to `false`. |
| `EmbedRazorGenerateSources` | Gdy `true`, program dodaje elementy RazorGenerate ( *. cshtml*) jako osadzone pliki do wygenerowanego zestawu Razor. Wartość domyślna to `false`. |
| `UseRazorBuildServer` | Gdy `true`, program używa procesu trwałego serwera kompilacji do odciążenia działania generowania kodu. Wartość domyślna to `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Gdy `true`, zestaw SDK generuje dodatkowe atrybuty używane przez MVC w czasie wykonywania w celu przeprowadzenia odnajdywania części aplikacji. |
| `DefaultWebContentItemExcludes` | Wzorzec obsługi symboli wieloznacznych dla elementów elementów, które mają zostać wykluczone z grupy elementów `Content` w projektach przeznaczonych dla zestawu SDK sieci Web lub Razor |
| `ExcludeConfigFilesFromBuildOutput` | W przypadku `true`pliki *config* i *JSON* nie są kopiowane do katalogu wyjściowego kompilacji. |
| `AddRazorSupportForMvc` | Gdy `true`, program skonfiguruje zestaw SDK Razor do dodawania obsługi konfiguracji MVC, która jest wymagana podczas kompilowania aplikacji zawierających widoki MVC lub Razor Pages. Ta właściwość jest niejawnie ustawiona dla projektów .NET Core 3,0 lub nowszych przeznaczonych dla zestawu SDK sieci Web |
| `RazorLangVersion` | Wersja języka Razor do celów. |

::: moniker-end

Aby uzyskać więcej informacji na temat właściwości, zobacz [Właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Obiekty docelowe

Zestaw Razor SDK definiuje dwa podstawowe cele:

* `RazorGenerate` &ndash; kod generuje pliki *CS* z elementów `RazorGenerate` elementu. Użyj właściwości `RazorGenerateDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.
* `RazorCompile` &ndash; kompiluje wygenerowane pliki *. cs* w zestawie do zestawu Razor. Użyj `RazorCompileDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.
* `RazorComponentGenerate` &ndash; kod generuje pliki *CS* dla elementów `RazorComponent` elementu. Użyj właściwości `RazorComponentGenerateDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.

### <a name="runtime-compilation-of-razor-views"></a>Kompilacja środowiska uruchomieniowego z widokami Razor

* Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego. Powoduje to błędy kompilacji, gdy model aplikacji opiera się na&mdash;kompilacji środowiska uruchomieniowego na przykład aplikacja korzysta z widoków osadzonych lub zmian po opublikowaniu aplikacji. Ustaw `CopyRefAssembliesToPublishDirectory` na `true`, aby kontynuować publikowanie zestawów odwołań.

* W przypadku aplikacji sieci Web upewnij się, że aplikacja jest ukierunkowana na zestaw `Microsoft.NET.Sdk.Web` SDK.

## <a name="razor-language-version"></a>Wersja języka Razor

W przypadku określania zestawu SDK `Microsoft.NET.Sdk.Web` wersja języka Razor jest wywnioskowana z wersji platformy docelowej aplikacji. W przypadku projektów przeznaczonych dla zestawu SDK `Microsoft.NET.Sdk.Razor` lub w rzadkich przypadkach, gdy aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowana, można skonfigurować wersję, ustawiając właściwość `<RazorLangVersion>` w pliku projektu aplikacji:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Wersja języka Razor jest ściśle zintegrowana z wersją środowiska uruchomieniowego, dla którego została skompilowana. Określanie wersji językowej, która nie jest przeznaczona dla środowiska uruchomieniowego, nie jest obsługiwane i prawdopodobnie powoduje błędy kompilacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatki do formatu csproj dla platformy .NET Core](/dotnet/core/tools/csproj)
* [Wspólne elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
