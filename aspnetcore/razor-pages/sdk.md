---
title: Platforma ASP.NET Core Razor SDK
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/sdk
ms.openlocfilehash: 4dd48b13272ed847ff83e8826e10678d732b53f9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38215890"
---
# <a name="aspnet-core-razor-sdk"></a>Platforma ASP.NET Core Razor SDK

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK). Zestaw Razor SDK:

* Standaryzuje środowisko wokół tworzenia, pakowania i publikowania projektów zawierających [Razor](xref:mvc/views/razor) plików dla projektów ASP.NET Core MVC.
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwiają dostosowanie kompilacji w plikach Razor.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Przy użyciu zestawu SDK elementu Razor.

Większość aplikacji sieci web nie trzeba wyraźnie odwoływać się do zestawu SDK Razor. 

Aby użyć zestaw Razor SDK do tworzenia biblioteki klas zawierający widokami Razor lub stron Razor:

* Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Zazwyczaj odwołania do pakietu do `Microsoft.AspNetCore.Mvc` jest wymagane w ramach dodatkowe zależności wymagany do kompilowania i kompilowania stron Razor i widokami Razor. Co najmniej musi dodać odwołania do pakietu do projektu:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Poprzedni pakiety znajdują się w `Microsoft.AspNetCore.Mvc`. Następujący kod przedstawia podstawowe *.csproj* pliku, który używa zestaw Razor SDK do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Właściwości

Następujące właściwości kontrolować zachowanie zestawu SDK Razor jako część kompilacji projektu:

* `RazorCompileOnBuild` : Gdy `true`, kompiluje i emituje zestaw Razor, w ramach tworzenia projektu. Wartość domyślna to `true`.
* `RazorCompileOnPublish` : Gdy `true`, kompiluje i emituje zestaw Razor, w ramach publikowania projektu. Wartość domyślna to `true`.

Aby skonfigurować dane wejściowe i wyjściowe do zestawu SDK Razor służą poniższe właściwości i elementy:

| Elementy                                         | Opis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Element elementów (*.cshtml* plików), które są dane wejściowe do celów generowania kodu. |
| RazorCompile                                  | Elementy (pliki CS), które to dane wejściowe do celów kompilacji Razor. Użyj tego ItemGroup, aby określić dodatkowe pliki do skompilowany w zestawie Razor. |
| RazorTargetAssemblyAttribute                  | Element służącego do kodu generowania atrybuty dla zestawu Razor. Na przykład:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Elementy dodane do wygenerowanego zestawu Razor jako zasobów osadzonych |

| Właściwość                                      | Opis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Nazwa pliku (bez rozszerzenia) zestaw utworzony przez Razor. | 
| RazorOutputPath                               | Katalog wyjściowy Razor.                                      |
| RazorCompileToolset                           | Używany do określenia zestawu narzędzi, używany do tworzenia zestawu Razor. Prawidłowe wartości to `Implicit`,, a `PrecompilationTool`. |
| EnableDefaultContentItems                     | Gdy `true`, obejmuje niektórych typów plików, takie jak *.cshtml* pliki zawartości w projekcie. Po odwołaniu za pośrednictwem Microsoft.NET.Sdk.Web obejmuje również wszystkie pliki w *wwwroot*i plików konfiguracji.         |
| EnableDefaultRazorGenerateItems               | Gdy `true`, obejmuje *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów. |
| GenerateRazorTargetAssemblyInfo               | Gdy `true`, generuje *.cs* plik zawierający atrybuty określone przez `RazorAssemblyAttribute` i dołączenia go w danych wyjściowych kompilacji. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Gdy `true`, kopiuje elementy RazorGenerate (*.cshtml*) pliki do katalogu publikowania. Zazwyczaj pliki Razor nie są potrzebne w opublikowanej aplikacji, jeśli uczestniczą w kompilacji w czasie kompilacji lub czasu publikowania. Wartość domyślna to `false`. |
| CopyRefAssembliesToPublishDirectory            | Gdy `true`, skopiuj elementy zestawu odwołania do katalogu publikowania. Zazwyczaj zestawy odwołań nie są wymagane przez opublikowanej aplikacji, jeśli kompilacja Razor występuje w czasie kompilacji lub czasu publikowania. Ustaw `true`, jeśli opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego, na przykład modyfikuje pliki cshtml w czasie wykonywania, lub wykorzystuje osadzonych widoków. Wartość domyślna to `false`. |
| IncludeRazorContentInPack                      | Gdy `true`, wszystkie elementy zawartości Razor (*.cshtml* pliki) zostanie oznaczona do włączenia do wygenerowanego pakietu NuGet. Wartość domyślna to `false`. |
| EmbedRazorGenerateSources | Gdy `true`, dodaje RazorGenerate (*.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor. Wartość domyślna to `false`. |
| UseRazorBuildServer                           | Gdy `true`, używa procesu serwera kompilacji trwałego odciążania roboczego generowania kodu. Wartością domyślną jest wartość `UseSharedCompilation`. |

### <a name="targets"></a>Obiekty docelowe
Zestaw Razor SDK definiuje dwa podstawowe cele:

* `RazorGenerate` -Generuje kod *.cs* pliki z RazorGenerate elementu elementów. Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych jest przeznaczony dla, uruchomić przed lub po ten element docelowy.
* `RazorCompile` -Kompiluje generowane *.cs* pliki do zestawu Razor. Użyj `RazorCompileDependsOn` Aby określić dodatkowe obiekty docelowe, które można uruchomić przed lub po ten element docelowy.

### <a name="runtime-compilation-of-razor-views"></a>Kompilacja środowiska uruchomieniowego z widokami Razor

* Domyślnie zestaw Razor SDK nie publikować zestawy odwołań, które są wymagane do przeprowadzenia kompilacja środowiska uruchomieniowego. Skutkuje to błędy kompilacji podczas model aplikacji zależy od kompilacja środowiska uruchomieniowego&mdash;na przykład aplikacja używa osadzonych widoków widoków lub zmiany po opublikowaniu aplikacji. Ustaw `CopyRefAssembliesToPublishDirectory` do `true` aby kontynuować publikowanie zestawów odwołań.

* Aplikacje sieci web, upewnij się, jest przeznaczona aplikacja `Microsoft.NET.Sdk.Web` zestawu SDK.
