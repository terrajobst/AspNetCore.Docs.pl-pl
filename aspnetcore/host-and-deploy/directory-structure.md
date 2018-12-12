---
title: Struktura katalogów platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat struktury katalogów opublikowane aplikacje platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ee0bebb8b5c688f8471d6420d1641b87ac271f6c
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284568"
---
# <a name="aspnet-core-directory-structure"></a>Struktura katalogów platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

*Publikowania* katalog zawiera zasoby do wdrożenia aplikacji, utworzona przez testowany [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia. Katalog zawiera:

* Pliki aplikacji
* Pliki konfiguracji
* Statyczne elementy zawartości
* Pakiety
* Środowisko uruchomieniowe ([niezależna wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) tylko)

| Typ aplikacji | Struktura katalogów |
| -------- | ------------------- |
| [Wdrożenie zależny od struktury](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Publikowanie&dagger;<ul><li>Dzienniki&dagger; (opcjonalnie, o ile nie jest wymagane w celu odbierania dzienników stdout)</li><li>Widoki&dagger; (MVC aplikacji; Jeśli widoków nie są wstępnie skompilowane)</li><li>Strony&dagger; (MVC ani stron Razor aplikacji; Jeśli strony nie są wstępnie skompilowane)</li><li>Wwwroot&dagger;</li><li>*\.pliki dll</li><li>{Nazwa zestawu} deps.JSON</li><li>{Nazwa zestawu} .dll</li><li>{Nazwa zestawu} .pdb</li><li>{NAZWA ZESTAWU}. Views.dll</li><li>{NAZWA ZESTAWU}. Views.pdb</li><li>{Nazwa zestawu}.runtimeconfig.json</li><li>plik Web.config (w przypadku wdrożeń usług IIS)</li></ul></li></ul> |
| [Niezależne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publikowanie&dagger;<ul><li>Dzienniki&dagger; (opcjonalnie, o ile nie jest wymagane w celu odbierania dzienników stdout)</li><li>Widoki&dagger; (MVC aplikacji; Jeśli widoków nie są wstępnie skompilowane)</li><li>Strony&dagger; (MVC ani stron Razor aplikacji; Jeśli strony nie są wstępnie skompilowane)</li><li>Wwwroot&dagger;</li><li>\*pliki z rozszerzeniem dll</li><li>{Nazwa zestawu} deps.JSON</li><li>{Nazwa zestawu} .dll</li><li>{Nazwa zestawu} .exe</li><li>{Nazwa zestawu} .pdb</li><li>{NAZWA ZESTAWU}. Views.dll</li><li>{NAZWA ZESTAWU}. Views.pdb</li><li>{Nazwa zestawu}.runtimeconfig.json</li><li>plik Web.config (w przypadku wdrożeń usług IIS)</li></ul></li></ul> |

&dagger;Określa katalog

*Publikowania* reprezentuje katalog *ścieżka katalogu głównego zawartości*, nazywane również *ścieżki podstawowej aplikacji*, wdrożenia. Jest nadać dowolną nazwę *publikowania* katalogu wdrożonej aplikacji na serwerze, jego lokalizacji służy jako serwer ścieżka fizyczna do hostowanych aplikacji.

*Wwwroot* katalogu, jeśli jest obecny, zawiera tylko statyczne elementy zawartości.

Stdout *dzienniki* katalogu mogą być tworzone dla wdrożenia przy użyciu jednej z następujących dwóch metod:

* Dodaj następujący kod `<Target>` element do pliku projektu:

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

   `<MakeDir>` Element utworzy pustą *dzienniki* folderu w opublikowanych danych wyjściowych. Element używa `PublishDir` właściwości w celu określenia lokalizacji docelowej dla tworzenia folderu. Kilka metod wdrażania, takie jak narzędzie Web Deploy, Pomiń pustych folderów podczas wdrażania. `<WriteLinesToFile>` Element generuje plik w *dzienniki* folderu, który gwarantuje wdrożenia folderu na serwerze. Tworzenie folderu przy użyciu tej metody nie powiedzie się, jeśli proces roboczy nie ma dostępu do zapisu do folderu docelowego.

* Utwórz fizycznie *dzienniki* katalogu na serwerze w trakcie wdrażania.

Katalog wdrażania musi mieć uprawnienia odczytu/Execute. *Dzienniki* katalogu musi mieć uprawnienia odczytu/zapisu. Dodatkowe katalogi, w którym są zapisywane pliki muszą mieć uprawnienia odczytu/zapisu.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [Wdrażanie aplikacji .NET core](/dotnet/core/deploying/)
* [Platformy docelowe](/dotnet/standard/frameworks)
* [.NET core RID katalogu](/dotnet/core/rid-catalog)
