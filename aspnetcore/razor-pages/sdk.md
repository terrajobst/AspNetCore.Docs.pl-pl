---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak Razor Pages w ASP.NET Core sprawia, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i wydajniejsze niż używanie MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 606d2bdca3fa4fb1c81df73ac697d2175c3ab633
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334034"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Omówienie

@No__t-0 zawiera zestaw SDK programu MSBuild `Microsoft.NET.Sdk.Razor` (SDK Razor). Zestaw SDK Razor:

::: moniker range=">= aspnetcore-3.0"

* Jest wymagana do kompilowania, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC lub [Blazor](xref:blazor/index) .
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementów, które umożliwiają dostosowanie kompilacji plików Razor ( *. cshtml* lub *. Razor*).

Zestaw SDK Razor zawiera elementy `Content` z atrybutami `Include` ustawionymi na `**\*.cshtml` i `**\*.razor` wzorców obsługi symboli wieloznacznych. Zgodne pliki są publikowane.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Standaryzacja środowiska tworzenia, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC.
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementów umożliwiających Dostosowywanie kompilacji plików Razor.

Zestaw SDK Razor zawiera element `Content` z atrybutem `Include` ustawionym na obsługi symboli wieloznacznych wzorzec `**\*.cshtml`. Zgodne pliki są publikowane.

::: moniker-end

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Korzystanie z zestawu SDK Razor

Większość aplikacji sieci Web nie jest wymagana, aby jawnie odwoływać się do zestawu Razor SDK.

::: moniker range=">= aspnetcore-3.0"

Aby użyć zestawu Razor SDK do kompilowania bibliotek klas zawierających widoki Razor lub Razor Pages, zalecamy rozpoczęcie od szablonu projektu biblioteki klas Razor (RCL). RCL, który jest używany do kompilowania plików Blazor ( *. Razor*) w minimalnym stopniu wymaga odwołania do pakietu [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) . RCL, który jest używany do kompilowania widoków i stron Razor (pliki *. cshtml* ), wymaga minimalnej `netcoreapp3.0` lub nowszej i ma `FrameworkReference` do [pakietu Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) w pliku projektu.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Aby użyć zestawu Razor SDK do kompilowania bibliotek klas zawierających widoki Razor lub Razor Pages:

* Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Zazwyczaj odwołanie do pakietu `Microsoft.AspNetCore.Mvc` jest wymagane do uzyskania dodatkowych zależności, które są wymagane do kompilowania i kompilowania widoków Razor Pages i Razor. Co najmniej, projekt powinien dodawać odwołania do pakietów do:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  Pakiet `Microsoft.AspNetCore.Razor.Design` zawiera zadania kompilacji Razor i elementy docelowe dla projektu.

  Powyższe pakiety są zawarte w `Microsoft.AspNetCore.Mvc`. Poniższe znaczniki przedstawiają plik projektu, który używa zestawu Razor SDK do kompilowania plików Razor dla aplikacji Razor Pages ASP.NET Core:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> Pakiety `Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app). Niemniej jednak wersja `Microsoft.AspNetCore.App` pakietu zawiera pakiet dla aplikacji, która nie zawiera najnowszej wersji `Microsoft.AspNetCore.Razor.Design`. Projekty muszą odwoływać się do spójnej wersji `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`), aby uzyskać najnowsze poprawki czasu kompilacji dla aparatu Razor. Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/aspnet/Razor/issues/2553)w serwisie GitHub.

::: moniker-end

### <a name="properties"></a>Właściwości

Następujące właściwości kontrolują zachowanie zestawu SDK Razor w ramach kompilacji projektu:

* `RazorCompileOnBuild` &ndash; podczas `true`, kompiluje i emituje zestaw Razor w ramach kompilowania projektu. Wartość domyślna to `true`.
* `RazorCompileOnPublish` &ndash; podczas `true`, kompiluje i emituje zestaw Razor w ramach publikowania projektu. Wartość domyślna to `true`.

Właściwości i elementy w poniższej tabeli służą do konfigurowania danych wejściowych i wyjściowych do zestawu Razor SDK.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Począwszy od ASP.NET Core 3,0, widoki MVC lub Razor Pages nie są obsługiwane domyślnie, jeśli właściwości programu MSBuild `RazorCompileOnBuild` lub `RazorCompileOnPublish` w pliku projektu są wyłączone. Aplikacje muszą dodać jawne odwołanie do pakietu [Microsoft. AspNetCore. MVC. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) , jeśli aplikacja korzysta z kompilacji środowiska uruchomieniowego w celu przetworzenia plików *. cshtml* .

::: moniker-end

| Elementy | Opis |
| ----- | ----------- |
| `RazorGenerate` | Elementy elementu (pliki *. cshtml* ), które są danymi wejściowymi do generowania kodu. |
| `RazorComponent` | Elementy elementu (pliki*Razor* ), które są danymi wejściowymi do generowania kodu składnika Razor. |
| `RazorCompile` | Elementy elementów (pliki*CS* ), które są danymi wejściowymi do elementów docelowych kompilacji Razor. Użyj tego `ItemGroup`, aby określić dodatkowe pliki do skompilowania do zestawu Razor. |
| `RazorTargetAssemblyAttribute` | Elementy elementów używane do tworzenia kodu atrybutów dla zestawu Razor. Na przykład:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementy elementu dodane jako zasoby osadzone do wygenerowanego zestawu Razor. |

| Właściwość | Opis |
| -------- | ----------- |
| `RazorTargetName` | Nazwa pliku (bez rozszerzenia) zestawu utworzonego przez Razor. | 
| `RazorOutputPath` | Katalog wyjściowy Razor. |
| `RazorCompileToolset` | Służy do określenia zestawu narzędzi używanego do tworzenia zestawów Razor. Prawidłowe wartości to `Implicit`, `RazorSDK` i `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Wartość domyślna to `true`. Gdy `true`, zawiera pliki *Web. config*, *. JSON*i *. cshtml* jako zawartość projektu. W przypadku odwołania za pośrednictwem `Microsoft.NET.Sdk.Web` pliki katalogu *wwwroot* i plików konfiguracyjnych są również uwzględniane. |
| `EnableDefaultRazorGenerateItems` | Gdy `true`, zawiera pliki *. cshtml* z elementów `Content` w `RazorGenerate` elementów. |
| `GenerateRazorTargetAssemblyInfo` | Gdy `true`, program generuje plik *CS* zawierający atrybuty określone przez `RazorAssemblyAttribute` i zawiera plik w danych wyjściowych kompilacji. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Gdy `true`, program dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Gdy `true`, kopiuje pliki `RazorGenerate` ( *. cshtml*) do katalogu publikowania. Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub w czasie publikacji. Wartość domyślna to `false`. |
| `CopyRefAssembliesToPublishDirectory` | Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania. Zazwyczaj zestawy referencyjne nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor odbywa się w czasie kompilacji lub w czasie publikacji. Ustaw wartość `true`, jeśli opublikowana aplikacja wymaga kompilacji środowiska uruchomieniowego. Na przykład ustaw wartość `true`, jeśli aplikacja modyfikuje pliki *. cshtml* w czasie wykonywania lub użyje widoków osadzonych. Wartość domyślna to `false`. |
| `IncludeRazorContentInPack` | Gdy `true`, wszystkie elementy zawartości Razor (pliki *. cshtml* ) są oznaczone do uwzględnienia w wygenerowanym pakiecie NuGet. Wartość domyślna to `false`. |
| `EmbedRazorGenerateSources` | Gdy `true`, program dodaje elementy RazorGenerate ( *. cshtml*) jako osadzone pliki do wygenerowanego zestawu Razor. Wartość domyślna to `false`. |
| `UseRazorBuildServer` | Gdy `true`, program używa procesu trwałego serwera kompilacji do odciążenia operacji generowania kodu. Wartością domyślną jest wartość `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Gdy `true`, zestaw SDK generuje dodatkowe atrybuty używane przez MVC w czasie wykonywania w celu przeprowadzenia odnajdywania części aplikacji. |

Aby uzyskać więcej informacji na temat właściwości, zobacz [Właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Obiekty docelowe

Zestaw SDK Razor definiuje dwa podstawowe elementy docelowe:

* `RazorGenerate` &ndash; Kod generuje pliki *CS* z elementów `RazorGenerate` elementu. Użyj właściwości `RazorGenerateDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed tym elementem docelowym lub po nim.
* `RazorCompile` &ndash; kompiluje wygenerowane pliki *. cs* w zestawie Razor. Użyj `RazorCompileDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed lub po tym elemencie docelowym.
* `RazorComponentGenerate` &ndash; Kod generuje pliki *CS* dla elementów `RazorComponent` elementu. Użyj właściwości `RazorComponentGenerateDependsOn`, aby określić dodatkowe elementy docelowe, które mogą być uruchamiane przed tym elementem docelowym lub po nim.

### <a name="runtime-compilation-of-razor-views"></a>Kompilacja widoków Razor dla środowiska uruchomieniowego

* Domyślnie zestaw SDK Razor nie publikuje zestawów referencyjnych, które są wymagane do wykonania kompilacji środowiska uruchomieniowego. Powoduje to błędy kompilacji, gdy model aplikacji opiera się na przykładowej kompilacji środowiska uruchomieniowego @ no__t-0for, aplikacja używa widoków osadzonych lub zmian po opublikowaniu aplikacji. Ustaw wartość `CopyRefAssembliesToPublishDirectory` na `true`, aby kontynuować publikowanie zestawów odwołań.

* W przypadku aplikacji sieci Web upewnij się, że aplikacja jest ukierunkowana na zestaw SDK `Microsoft.NET.Sdk.Web`.

## <a name="razor-language-version"></a>Wersja języka Razor

W przypadku zestawu SDK `Microsoft.NET.Sdk.Web` wersja języka Razor jest wywnioskowana z wersji platformy docelowej aplikacji. W przypadku projektów przeznaczonych dla zestawu SDK `Microsoft.NET.Sdk.Razor` lub w rzadkich przypadkach, gdy aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowana, można skonfigurować wersję, ustawiając właściwość `<RazorLangVersion>` w pliku projektu aplikacji:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Wersja języka Razor jest ściśle zintegrowana z wersją środowiska uruchomieniowego, dla którego została skompilowana. Określanie wersji językowej, która nie jest przeznaczona dla środowiska uruchomieniowego, nie jest obsługiwane i prawdopodobnie powoduje błędy kompilacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatki do formatu csproj dla platformy .NET Core](/dotnet/core/tools/csproj)
* [Wspólne elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
