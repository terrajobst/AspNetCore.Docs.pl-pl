---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: 1dc001c7c5fe320629835e06fe6db7fadabff94d
ms.sourcegitcommit: 6189b0ced9c115248c6ede02efcd0b29d31f2115
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985396"
---
# <a name="aspnet-core-razor-sdk"></a>Platforma ASP.NET Core Razor SDK

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Omówienie

[!INCLUDE[](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK). Zestaw Razor SDK:

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
* Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
* jest wymagana do kompilowania, pakowania i publikowania projektów zawierających pliki [Razor](xref:mvc/views/razor) dla ASP.NET Core projektów opartych na MVC lub projektów [Blazor](xref:blazor/index)
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementów, które umożliwiają dostosowanie kompilacji plików Razor (. cshtml lub. Razor).
::: moniker-end


::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Zestaw SDK Razor zawiera `<Content>` element `Include` z atrybutem ustawionym na `**\*.cshtml` wzorzec obsługi symboli wieloznacznych. Zgodne pliki są publikowane.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Zestaw SDK Razor zawiera `<Content>` elementy z `Include` atrybutami ustawionymi `**\*.cshtml` na `**\*.razor` wzorce i obsługi symboli wieloznacznych. Zgodne pliki są publikowane.

::: moniker-end

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Korzystanie z zestawu SDK Razor

Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:

* Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagana, aby otrzymać dodatkowe zależności, które są wymagane do kompilowania i kompilowania stron Razor i widokami Razor. Co najmniej projektu należy dodać odwołania do pakietu do:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  `Microsoft.AspNetCore.Razor.Design` Pakiet zawiera cele i zadania kompilacji Razor do projektu.

  Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`. Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
Aby użyć zestawu Razor SDK do kompilowania bibliotek klas zawierających widoki Razor lub Razor Pages zalecamy rozpoczęcie od szablonu projektu biblioteki klas Razor. Biblioteka klas Razor, która jest używana do kompilowania plików Blazor (. Razor), będzie wymagała co najmniej odwołania `Microsoft.AspNetCore.Components` do pakietu. Biblioteka klas Razor, która jest używana do kompilowania widoków i stron Razor (pliki. cshtml), będzie wymagała określania wartości docelowej `netcoreapp3.0` lub nowszej oraz może `FrameworkReference` zawierać do `Microsoft.AspNetCore.App`.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Jednak bez wersji `Microsoft.AspNetCore.App` odwołania do pakietu zawiera meta Microsoft.aspnetcore.all, aplikacja która nie zawiera najnowszą wersję `Microsoft.AspNetCore.Razor.Design`. Projekty muszą odwoływać się do wersję spójną `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`) tak, że uwzględniono najnowszych poprawek czas kompilacji dla elementu Razor. Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Właściwości

Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:

* `RazorCompileOnBuild` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu. Wartość domyślna to `true`.
* `RazorCompileOnPublish` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu. Wartość domyślna to `true`.

Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.

::: moniker range=">= aspnetcore-3.0"
> [!WARNING]
Począwszy od ASP.NET Core 3,0, widoki MVC lub Razor Pages nie będą obsługiwane domyślnie, jeśli `RazorCompileOnBuild` lub `RazorCompileOnPublish` jest wyłączone. Aplikacje muszą dodać jawne odwołanie do `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` pakietu, aby dodać obsługę kompilacji w czasie wykonywania, jeśli są one zależne od kompilacji środowiska uruchomieniowego w celu przetworzenia plików cshtml.
::: moniker-end


| Elementy | Opis |
| ----- | ----------- |
| `RazorGenerate` | Elementy elementu (pliki *. cshtml* ), które są danymi wejściowymi do generowania kodu. |
| `RazorComponent` | Elementy elementu (pliki*Razor* ), które są danymi wejściowymi generowania kodu składnika.
| `RazorCompile` | Elementy elementów (pliki*CS* ), które są danymi wejściowymi do elementów docelowych kompilacji Razor. Użyj tego `ItemGroup` , aby określić dodatkowe pliki do skompilowania do zestawu Razor. |
| `RazorTargetAssemblyAttribute` | Element służącego do kodu generowania atrybuty dla zestawu Razor. Na przykład:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone. |

| Właściwość | Opis |
| -------- | ----------- |
| `RazorTargetName` | Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor. | 
| `RazorOutputPath` | Katalog wyjściowy Razor. |
| `RazorCompileToolset` | Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor. Prawidłowe wartości to `Implicit`, `RazorSDK`, i `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Wartość domyślna to `true`. Gdy `true`, zawiera pliki *Web. config*, *. JSON*i *. cshtml* jako zawartość projektu. Gdy przywoływana za pomocą `Microsoft.NET.Sdk.Web`, plików w obszarze *wwwroot* i dołączane są także pliki konfiguracji. |
| `EnableDefaultRazorGenerateItems` | Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów. |
| `GenerateRazorTargetAssemblyInfo` | Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` włącznie z plikiem w danych wyjściowych kompilacji. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Gdy `true`, kopie `RazorGenerate` elementów ( *.cshtml*) pliki do katalogu publikowania. Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania. Wartość domyślna to `false`. |
| `CopyRefAssembliesToPublishDirectory` | Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania. Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania. Ustaw `true` Jeśli Twojej opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego. Na przykład, ustaw wartość `true` Jeśli aplikacja modyfikuje *.cshtml* plików w czasie wykonywania lub korzysta z osadzonych widoków. Wartość domyślna to `false`. |
| `IncludeRazorContentInPack` | Gdy `true`, wszystkie elementy zawartości Razor ( *.cshtml* pliki) są oznaczone do włączenia do wygenerowanego pakietu NuGet. Wartość domyślna to `false`. |
| `EmbedRazorGenerateSources` | Gdy `true`, dodaje RazorGenerate ( *.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor. Wartość domyślna to `false`. |
| `UseRazorBuildServer` | Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu. Wartością domyślną jest wartość `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | Gdy `true`zestaw SDK generuje dodatkowe atrybuty używane przez MVC w czasie wykonywania w celu przeprowadzenia odnajdywania części aplikacji. |

Aby uzyskać więcej informacji na temat właściwości, zobacz [Właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Obiekty docelowe

Zestaw Razor SDK definiuje dwa podstawowe cele:

* `RazorGenerate` &ndash; Generuje kod *.cs* plików ze `RazorGenerate` elementu elementów. Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.
* `RazorCompile` &ndash; Kompiluje generowane *.cs* pliki do zestawu Razor. Użyj `RazorCompileDependsOn` Aby określić dodatkowe obiekty docelowe, które można uruchomić przed lub po ten element docelowy.
* `RazorComponentGenerate`Kod generuje pliki *CS* dla `RazorComponent` elementów Item. &ndash; Użyj `RazorComponentGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.

### <a name="runtime-compilation-of-razor-views"></a>Kompilacja środowiska uruchomieniowego z widokami Razor

* Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego. Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji. Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.

* Dla aplikacji sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.

## <a name="razor-language-version"></a>Wersja języka Razor

W przypadku określania `Microsoft.NET.Sdk.Web` zestawu SDK wersja języka Razor jest wywnioskowana z wersji platformy docelowej aplikacji. W przypadku projektów `Microsoft.NET.Sdk.Razor` przeznaczonych dla zestawu SDK lub w rzadkich przypadkach, gdy aplikacja wymaga innej wersji języka Razor niż wartość wywnioskowana, można skonfigurować wersję, `<RazorLangVersion>` ustawiając właściwość w pliku projektu aplikacji:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Wersja języka Razor jest ściśle zintegrowana z wersją środowiska uruchomieniowego, dla którego została skompilowana. Określanie wersji językowej, która nie jest przeznaczona dla środowiska uruchomieniowego, nie jest obsługiwane i prawdopodobnie spowoduje błędy kompilacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatki do formatu csproj dla platformy .NET Core](/dotnet/core/tools/csproj)
* [Wspólne elementy projektu MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
