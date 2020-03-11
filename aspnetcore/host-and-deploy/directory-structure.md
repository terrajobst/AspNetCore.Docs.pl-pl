---
title: Struktura katalogów ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat struktury katalogów opublikowane aplikacje platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f7d6feec9961b7f6720d30d457fae5dcb6b34d6c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664109"
---
# <a name="aspnet-core-directory-structure"></a>Struktura katalogów ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Katalog *publikowania* zawiera elementy możliwe do wdrożenia aplikacji wytwarzane przez polecenie [dotnet Publish](/dotnet/core/tools/dotnet-publish) . Katalog zawiera:

* Pliki aplikacji
* Pliki konfiguracji
* Statyczne zasoby
* Pakiety
* Środowisko uruchomieniowe (tylko do[wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) )

| Typ aplikacji | Struktura katalogów |
| -------- | ------------------- |
| [Plik wykonywalny zależny od platformy (całego)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>Publikuj&dagger;<ul><li>Widoki&dagger; aplikacji MVC; Jeśli widoki nie są wstępnie skompilowane</li><li>Strony&dagger; aplikacji MVC lub Razor Pages, jeśli strony nie są wstępnie skompilowane</li><li>wwwroot&dagger;</li><li>Pliki \*. dll</li><li>{Nazwa zestawu}. deps. JSON</li><li>{Nazwa zestawu}. dll</li><li>{NAZWA ZESTAWU} {. Rozszerzenie} *. exe* rozszerzenie w systemie Windows, brak rozszerzenia w systemie MacOS lub Linux</li><li>{Nazwa zestawu}. pdb</li><li>{NAZWA ZESTAWU}. Widoki. dll</li><li>{NAZWA ZESTAWU}. Widoki. pdb</li><li>{Nazwa zestawu}. runtimeconfig. JSON</li><li>Web. config (wdrożenia usług IIS)</li><li>Generuj zrzut ([Narzędzie](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy)do obsługi zrzutów systemu Linux)</li><li>\*. so (Biblioteka udostępnionych obiektów systemu Linux)</li><li>\*. a (archiwum macOS)</li><li>\*. DYLIB (Biblioteka dynamiczna macOS)</li></ul></li></ul> |
| [Wdrażanie samodzielne (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publikuj&dagger;<ul><li>Widoki&dagger; aplikacji MVC, jeśli widoki nie są wstępnie skompilowane</li><li>Strony&dagger; aplikacji MVC lub Razor Pages, jeśli strony nie są wstępnie skompilowane</li><li>wwwroot&dagger;</li><li>Pliki \*. dll</li><li>{Nazwa zestawu}. deps. JSON</li><li>{Nazwa zestawu}. dll</li><li>{Nazwa zestawu}. exe</li><li>{Nazwa zestawu}. pdb</li><li>{NAZWA ZESTAWU}. Widoki. dll</li><li>{NAZWA ZESTAWU}. Widoki. pdb</li><li>{Nazwa zestawu}. runtimeconfig. JSON</li><li>Web. config (wdrożenia usług IIS)</li></ul></li></ul> |

&dagger;wskazuje katalog

Katalog *publikowania* reprezentuje *ścieżkę katalogu głównego zawartości*, nazywaną również *ścieżką bazową aplikacji*, wdrożenia. Dowolnych nazw do katalogu *publikacji* wdrożonej aplikacji na serwerze, jej lokalizacja służy jako ścieżka fizyczna serwera do hostowanej aplikacji.

Katalog *wwwroot* (jeśli istnieje) zawiera tylko zasoby statyczne.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Wdrażanie aplikacji .NET Core](/dotnet/core/deploying/)
* [Platformy docelowe](/dotnet/standard/frameworks)
* [Katalog programu .NET Core RID](/dotnet/core/rid-catalog)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Katalog *publikowania* zawiera elementy możliwe do wdrożenia aplikacji wytwarzane przez polecenie [dotnet Publish](/dotnet/core/tools/dotnet-publish) . Katalog zawiera:

* Pliki aplikacji
* Pliki konfiguracji
* Statyczne zasoby
* Pakiety
* Środowisko uruchomieniowe (tylko do[wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) )

| Typ aplikacji | Struktura katalogów |
| -------- | ------------------- |
| [Plik wykonywalny zależny od platformy (całego)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>Publikuj&dagger;<ul><li>Widoki&dagger; aplikacji MVC; Jeśli widoki nie są wstępnie skompilowane</li><li>Strony&dagger; aplikacji MVC lub Razor Pages, jeśli strony nie są wstępnie skompilowane</li><li>wwwroot&dagger;</li><li>*pliki. dll</li><li>{nazwa zestawu}. deps. json</li><li>{nazwa zestawu}. dll</li><li>{nazwa zestawu} {. Rozszerzenie} *. exe* w systemie Windows, brak rozszerzenia w systemie MacOS lub Linux</li><li>{nazwa zestawu}. pdb</li><li>{nazwa zestawu}. Widoki. dll</li><li>{nazwa zestawu}. Views. pdb</li><li>{nazwa zestawu}. runtimeconfig. JSON</li><li>plik Web. config (wdrożenia usług IIS)</li><li>zrzutu (narzędzie do zarządzania[zrzutem](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy)* systemu Linux)</li><li>.</li><li>*. a (archiwum macOS)</li><li>* . DYLIB (macOS Dynamic Library)</li></ul></li></ul> |
| [Wdrażanie samodzielne (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publikuj&dagger;<ul><li>Widoki&dagger; aplikacji MVC, jeśli widoki nie są wstępnie skompilowane</li><li>Strony&dagger; aplikacji MVC lub Razor Pages, jeśli strony nie są wstępnie skompilowane</li><li>wwwroot&dagger;</li><li>*. dll — pliki</li><li>{Nazwa zestawu}. deps. JSON</li><li>{Nazwa zestawu}. dll</li><li>{Nazwa zestawu}. exe</li><li>{Nazwa zestawu}. pdb</li><li>{NAZWA ZESTAWU}. Widoki. dll</li><li>{NAZWA ZESTAWU}. Widoki. pdb</li><li>{Nazwa zestawu}. runtimeconfig. JSON</li><li>Web. config (wdrożenia usług IIS)</li></ul></li></ul> |

&dagger;wskazuje katalog

Katalog *publikowania* reprezentuje *ścieżkę katalogu głównego zawartości*, nazywaną również *ścieżką bazową aplikacji*, wdrożenia. Dowolnych nazw do katalogu *publikacji* wdrożonej aplikacji na serwerze, jej lokalizacja służy jako ścieżka fizyczna serwera do hostowanej aplikacji.

Katalog *wwwroot* (jeśli istnieje) zawiera tylko zasoby statyczne.

Tworzenie folderu *dzienników* jest przydatne w przypadku [zaawansowanego rejestrowania debugowania modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs). Foldery w ścieżce przekazane do wartości `<handlerSetting>` nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu, aby umożliwić modułowi zapisanie dziennika debugowania.

Katalog *dzienników* można utworzyć dla wdrożenia przy użyciu jednego z następujących dwóch metod:

* Dodaj następujący `<Target>` elementu do pliku projektu:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   Element `<MakeDir>` tworzy pusty folder *Logs* w opublikowanym danych wyjściowych. Element używa właściwości `PublishDir`, aby określić lokalizację docelową tworzenia folderu. Kilka metod wdrażania, takich jak Web Deploy, Pomijaj puste foldery podczas wdrażania. Element `<WriteLinesToFile>` generuje plik w folderze *Logs* , który gwarantuje wdrożenie folderu na serwerze. Tworzenie folderów przy użyciu tego podejścia kończy się niepowodzeniem, jeśli proces roboczy nie ma dostępu do zapisu do folderu docelowego.

* Fizycznie Utwórz katalog *dzienników* na serwerze we wdrożeniu.

Katalog wdrożenia wymaga uprawnień do odczytu/wykonania. Katalog *dzienników* wymaga uprawnień do odczytu/zapisu. Dodatkowe katalogi, w przypadku których zapisano pliki wymagają uprawnień do odczytu/zapisu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Wdrażanie aplikacji .NET Core](/dotnet/core/deploying/)
* [Platformy docelowe](/dotnet/standard/frameworks)
* [Katalog programu .NET Core RID](/dotnet/core/rid-catalog)

::: moniker-end
