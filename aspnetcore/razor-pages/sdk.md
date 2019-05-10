---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: de51c9443e639cd64c234b6975cf7252bb7a2b9a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901894"
---
# <a name="aspnet-core-razor-sdk"></a>Platforma ASP.NET Core Razor SDK

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK). Zestaw Razor SDK:

* Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Przy użyciu zestawu SDK elementu Razor.

Większość aplikacji sieci web nie są wymagane, aby jawnie odwołać się do zestawu SDK Razor.

Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:

* Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagana, aby otrzymać dodatkowe zależności, które są wymagane do kompilowania i kompilowania stron Razor i widokami Razor. Co najmniej projektu należy dodać odwołania do pakietu do:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  `Microsoft.AspNetCore.Razor.Design` Pakiet zawiera cele i zadania kompilacji Razor do projektu.

  Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`. Następujący kod przedstawia plik projektu, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` i `Microsoft.AspNetCore.Mvc.Razor.Extensions` pakiety są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Jednak bez wersji `Microsoft.AspNetCore.App` odwołania do pakietu zawiera meta Microsoft.aspnetcore.all, aplikacja która nie zawiera najnowszą wersję `Microsoft.AspNetCore.Razor.Design`. Projekty muszą odwoływać się do wersję spójną `Microsoft.AspNetCore.Razor.Design` (lub `Microsoft.AspNetCore.Mvc`) tak, że uwzględniono najnowszych poprawek czas kompilacji dla elementu Razor. Aby uzyskać więcej informacji, zobacz [problem w usłudze GitHub](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Właściwości

Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:

* `RazorCompileOnBuild` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu. Wartość domyślna to `true`.
* `RazorCompileOnPublish` &ndash; Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu. Wartość domyślna to `true`.

Właściwości i elementy w poniższej tabeli są używane do konfigurowania danych wejściowych i wyjściowych zestawu SDK Razor.

| Elementy | Opis |
| ----- | ----------- |
| `RazorGenerate` | Element elementów (*.cshtml* plików), które są dane wejściowe do celów generowania kodu. |
| `RazorCompile` | Element elementów (*.cs* plików), które są dane wejściowe do celów kompilacji Razor. Użyj tego ItemGroup, aby określić dodatkowe pliki do skompilowany w zestawie Razor. |
| `RazorTargetAssemblyAttribute` | Element służącego do kodu generowania atrybuty dla zestawu Razor. Na przykład:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Elementy dodane do wygenerowanego zestawu Razor jako zasoby osadzone. |

| Właściwość | Opis |
| -------- | ----------- |
| `RazorTargetName` | Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor. | 
| `RazorOutputPath` | Katalog wyjściowy Razor. |
| `RazorCompileToolset` | Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor. Prawidłowe wartości to `Implicit`, `RazorSDK`, i `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Wartość domyślna to `true`. Gdy `true`, obejmuje *web.config*, *.json*, i *.cshtml* pliki zawartości w projekcie. Gdy przywoływana za pomocą `Microsoft.NET.Sdk.Web`, plików w obszarze *wwwroot* i dołączane są także pliki konfiguracji. |
| `EnableDefaultRazorGenerateItems` | Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów. |
| `GenerateRazorTargetAssemblyInfo` | Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` włącznie z plikiem w danych wyjściowych kompilacji. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Gdy `true`, kopie `RazorGenerate` elementów (*.cshtml*) pliki do katalogu publikowania. Zazwyczaj pliki Razor nie są wymagane dla opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania. Wartość domyślna to `false`. |
| `CopyRefAssembliesToPublishDirectory` | Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania. Zazwyczaj zestawy odwołań nie są wymagane dla opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania. Ustaw `true` Jeśli Twojej opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego. Na przykład, ustaw wartość `true` Jeśli aplikacja modyfikuje *.cshtml* plików w czasie wykonywania lub korzysta z osadzonych widoków. Wartość domyślna to `false`. |
| `IncludeRazorContentInPack` | Gdy `true`, wszystkie elementy zawartości Razor (*.cshtml* pliki) są oznaczone do włączenia do wygenerowanego pakietu NuGet. Wartość domyślna to `false`. |
| `EmbedRazorGenerateSources` | Gdy `true`, dodaje RazorGenerate (*.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor. Wartość domyślna to `false`. |
| `UseRazorBuildServer` | Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu. Wartością domyślną jest wartość `UseSharedCompilation`. |

Aby uzyskać więcej informacji na temat właściwości, zobacz [właściwości programu MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Obiekty docelowe

Zestaw Razor SDK definiuje dwa podstawowe cele:

* `RazorGenerate` &ndash; Generuje kod *.cs* plików ze `RazorGenerate` elementu elementów. Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.
* `RazorCompile` &ndash; Kompiluje generowane *.cs* pliki do zestawu Razor. Użyj `RazorCompileDependsOn` Aby określić dodatkowe obiekty docelowe, które można uruchomić przed lub po ten element docelowy.

### <a name="runtime-compilation-of-razor-views"></a>Kompilacja środowiska uruchomieniowego z widokami Razor

* Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego. Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji. Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.

* Dla aplikacji sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.
