---
title: Struktura katalogów platformy ASP.NET Core
author: guardrex
description: Więcej informacji na temat struktury katalogów opublikowanych aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838439"
---
# <a name="aspnet-core-directory-structure"></a>Struktura katalogów platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W ASP.NET Core katalogu aplikacji opublikowanych *publikowania*, składa się z pliki aplikacji, plików konfiguracji statycznej zasoby, pakietów i środowiska uruchomieniowego (dla [niezależne wdrożeń](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Typ aplikacji | Struktura katalogów |
| -------- | ------------------- |
| [Zależne od Framework wdrożenia](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Publikowanie&dagger;<ul><li>dzienniki&dagger; (opcjonalne, chyba że wymagane, aby odbierać dzienniki stdout)</li><li>Widoki&dagger; (MVC aplikacji; Jeśli nie są wstępnie skompilowana widoków)</li><li>Strony&dagger; (MVC ani stron Razor aplikacji; Jeśli nie są wstępnie skompilowana stron)</li><li>Wwwroot&dagger;</li><li>*\.pliki dll</li><li>\<Nazwa zestawu >. deps.json</li><li>\<Nazwa zestawu > .dll</li><li>\<Nazwa zestawu > .pdb</li><li>\<Nazwa zestawu >. PrecompiledViews.dll</li><li>\<Nazwa zestawu >. PrecompiledViews.pdb</li><li>\<Nazwa zestawu >. runtimeconfig.json</li><li>plik Web.config (wdrożenia usług IIS)</li></ul></li></ul> |
| [Samodzielne wdrożenia](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publikowanie&dagger;<ul><li>dzienniki&dagger; (opcjonalne, chyba że wymagane, aby odbierać dzienniki stdout)</li><li>System plików refs&dagger;</li><li>Widoki&dagger; (MVC aplikacji; Jeśli nie są wstępnie skompilowana widoków)</li><li>Strony&dagger; (MVC ani stron Razor aplikacji; Jeśli nie są wstępnie skompilowana stron)</li><li>Wwwroot&dagger;</li><li>\*pliki dll</li><li>\<Nazwa zestawu >. deps.json</li><li>\<Nazwa zestawu > .exe</li><li>\<Nazwa zestawu > .pdb</li><li>\<Nazwa zestawu >. PrecompiledViews.dll</li><li>\<Nazwa zestawu >. PrecompiledViews.pdb</li><li>\<Nazwa zestawu >. runtimeconfig.json</li><li>plik Web.config (wdrożenia usług IIS)</li></ul></li></ul> |

&dagger;Wskazuje katalog

*Publikowania* reprezentuje katalog *ścieżki katalogu głównego zawartości*, nazywany również *podstawowa ścieżka aplikacji*, wdrożenia. Nazwa znajduje się do *publikowania* katalogu wdrożonej aplikacji na serwerze, jego lokalizacji służy jako ścieżka fizyczna serwera hostowanej aplikacji.

*Wwwroot* katalogu, jeśli jest obecny, zawiera tylko zasoby statyczne.

Stdout *dzienniki* można utworzyć katalogu dla wdrożenia przy użyciu jednej z następujących dwóch metod:

* Dodaj następujące `<Target>` elementu do pliku projektu:

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

   `<MakeDir>` Elementu tworzy pustą *dzienniki* folderu w opublikowanych danych wyjściowych. Element używa `PublishDir` właściwości w celu określenia lokalizacji docelowej dla tworzenia folderu. Kilka metod wdrażania, takie jak narzędzie Web Deploy, Pomiń puste foldery podczas wdrażania. `<WriteLinesToFile>` Element generuje plik w *dzienniki* folder, który gwarantuje wdrożenia folderu na serwerze. Należy pamiętać, że tworzenie folderu może nadal się niepowodzeniem, jeśli proces roboczy nie ma dostępu do zapisu do folderu docelowego.

* Utwórz fizycznie *dzienniki* katalogu na serwerze w trakcie wdrażania.

Katalog wdrożenia musi mieć uprawnienia odczytu/Execute. *Dzienniki* katalog wymaga uprawnień do odczytu/zapisu. Dodatkowe katalogi, w którym zapisywane są pliki muszą mieć uprawnienia odczytu/zapisu.
