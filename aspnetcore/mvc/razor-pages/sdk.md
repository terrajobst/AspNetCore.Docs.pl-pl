---
title: Razor platformy ASP.NET Core SDK
author: Rick-Anderson
description: Dowiedz się, jak Razor strony platformy ASP.NET Core umożliwia kodowania scenariusze strony łatwiejsze i bardziej wydajnej pracy niż przy użyciu platformy MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: cf0e1873c7ce500ce3b8ad2b3367555bdc41a576
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2018
---
# <a name="aspnet-core-razor-sdk"></a>Razor platformy ASP.NET Core SDK

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [](~/includes/2.1-SDK.md)] Obejmuje `Microsoft.NET.Sdk.Razor` MSBuild SDK (zestaw SDK Razor). Razor SDK:

* Standaryzuje doświadczeń dotyczących tworzenia, pakowanie i publikowanie projektów zawierających [Razor](xref:mvc/views/razor) pliki projektów opartych na platformy ASP.NET Core MVC.
* Zawiera zestaw wstępnie zdefiniowanych obiektów docelowych, właściwości i elementy, które umożliwia dostosowywanie kompilacji plików Razor.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Przy użyciu zestawu SDK elementu Razor.

Większość aplikacji sieci web nie jest konieczne wyraźnie odwołanie Razor SDK. 

Korzystanie z zestawu SDK Razor do tworzenia biblioteki klas zawierające widokami Razor lub Razor strony:

* Użyj `Microsoft.NET.Sdk.Razor` zamiast `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Zwykle odwołanie pakietu do `Microsoft.AspNetCore.Mvc` należy przenieść dodatkowe zależności wymagane do kompilacji i kompilowania stron Razor i widokami Razor. Co najmniej projektu musi dodać odwołania do pakietu do:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Poprzednie pakiety znajdują się w `Microsoft.AspNetCore.Mvc`. Następujący kod przedstawia podstawowy *.csproj* pliku, który korzysta z zestawu SDK Razor do tworzenia plików Razor aplikacji ASP.NET Core Razor strony:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Właściwości

Następujące właściwości formantu Razor zachowanie zestawu SDK w ramach kompilacji projektu:

* `RazorCompileOnBuild` : Gdy `true`, kompiluje i emituje zestawu Razor w ramach tworzenia projektu. Domyślnie `true`.
* `RazorCompileOnPublish` : Gdy `true`, kompiluje i emituje zestawu Razor jako część publikowania projektu. Domyślnie `true`.

Aby skonfigurować dane wejściowe i wyjściowe do zestawu SDK Razor służą poniższe właściwości i elementów:

| Elementy                                         | Opis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Element elementy (*.cshtml* plików), które są dane wejściowe do celów generowania kodu. |
| RazorCompile                                  | Element elementy (pliki .cs) danych wejściowych w celu obiekty docelowe kompilacji Razor. Użyj tego ItemGroup, aby określić dodatkowe pliki i skompilowany w zestawie Razor. |
| RazorTargetAssemblyAttribute                  | Element używany do kodu generowania atrybuty dla zestawu Razor. Na przykład:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Element dodany jako zasoby osadzone do wygenerowanego zestawu Razor |

| Właściwość                                      | Opis                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Nazwa pliku (bez rozszerzenia) zestawu utworzonego przez Razor. | 
| RazorOutputPath                               | Katalog wyjściowy Razor.                                      |
| RazorCompileToolset                           | Używany do określenia zestaw narzędzi używanych do tworzenia zestawu Razor. Prawidłowe wartości to `Implicit`,, i `PrecompilationTool`. |
| EnableDefaultContentItems                     | Gdy `true`, zawiera niektórych typów plików, takich jak *.cshtml* plików jako zawartość w projekcie. Gdy przywoływana za pomocą Microsoft.NET.Sdk.Web, zawiera także wszystkich plików w katalogu *wwwroot*i plików konfiguracji.         |
| EnableDefaultRazorGenerateItems               | Gdy `true`, zawiera *.cshtml* plików ze `Content` elementy w `RazorGenerate` elementów. |
| GenerateRazorTargetAssemblyInfo               | Gdy `true`, generuje *.cs* plik zawierający atrybuty określone w `RazorAssemblyAttribute` i dołącza go w danych wyjściowych kompilacji. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Gdy `true`, dodaje domyślny zestaw atrybutów zestawu do `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Gdy `true`, kopiuje elementy RazorGenerate (*.cshtml*) pliki do katalogu publikowania. Zwykle Razor pliki nie są wymagane przez opublikowanej aplikacji, jeśli uczestniczą w kompilacji na czas kompilacji lub opublikować czasu. Domyślnie `false`. |
| CopyRefAssembliesToPublishDirectory            | Gdy `true`, skopiuj odwołanie do zestawu elementów do katalogu publikowania. Zwykle zestawów odwołań nie są wymagane przez opublikowanej aplikacji, w przypadku kompilacji Razor na czas kompilacji lub opublikować czasu. Wartość `true`, jeśli opublikowanej aplikacji wymaga kompilacja środowiska uruchomieniowego, na przykład modyfikuje pliki cshtml w czasie wykonywania, lub używa osadzonych widoków. Domyślnie `false`. |
| IncludeRazorContentInPack                      | Gdy `true`, wszystkie elementy zawartości Razor (*.cshtml* pliki) zostanie oznaczona do włączenia wygenerowany pakiet NuGet. Domyślnie `false`. |
| EmbedRazorGenerateSources | Gdy `true`, dodaje RazorGenerate (*.cshtml*) elementów jako osadzone pliki do wygenerowanego zestawu Razor. Domyślnie `false`. |
| UseRazorBuildServer                           | Gdy `true`, używa procesu serwera kompilacji trwałe odciążania pracy generowania kodu. Domyślnie przyjmowana jest wartość `UseSharedCompilation`. |

### <a name="targets"></a>Obiekty docelowe
Zestaw SDK Razor definiuje dwa obiekty docelowe głównej:

* `RazorGenerate` -Generuje kod *.cs* pliki z RazorGenerate elementu elementów. Użyj `RazorGenerateDependsOn` właściwości w celu określenia dodatkowych celem można uruchamiać przed lub po tym elemencie docelowym.
* `RazorCompile` -Kompiluje wygenerowany *.cs* plików w zestawie Razor. Użyj `RazorCompileDependsOn` do określenia dodatkowych obiektów docelowych, które mogą być uruchamiane przed lub po tym elemencie docelowym.
