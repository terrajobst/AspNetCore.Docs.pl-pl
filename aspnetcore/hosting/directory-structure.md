---
title: "Struktura katalogów platformy ASP.NET Core"
author: guardrex
description: "Struktura katalogów opublikowanych aplikacji platformy ASP.NET Core."
keywords: "Platformy ASP.NET Core, struktura katalogów"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Struktura katalogów opublikowanych aplikacji platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W ASP.NET Core katalogu aplikacji *publikowania*, składa się z pliki aplikacji, plików konfiguracji statycznej zasoby, pakietów i środowiska uruchomieniowego (dla swojej aplikacji). Jest to tej samej struktury katalogów jako poprzednie wersje programu ASP.NET, w którym mieszka całą aplikację w katalogu głównym sieci web.

| Typ aplikacji | Struktura katalogów |
| --- | --- |
| Zależne od Framework wdrożenia | <ul><li>Publikowanie\*<ul><li>dzienniki\* (jeśli jest to wymienione w publishOptions)</li><li>System plików refs\*</li><li>środowisk uruchomieniowych\*</li><li>Widoki\* (jeśli jest to wymienione w publishOptions)</li><li>Wwwroot\* (jeśli jest to wymienione w publishOptions)</li><li>.dll — pliki</li><li>MyApp.deps.JSON</li><li>MyApp.dll</li><li>MyApp.pdb</li><li>moja_aplikacja. PrecompiledViews.dll (jeśli prekompilowanie widokami Razor)</li><li>moja_aplikacja. PrecompiledViews.pdb (jeśli prekompilowanie widokami Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>plik Web.config (jeśli jest to wymienione w publishOptions)</li></ul></li></ul> |
| Samodzielne wdrożenia | <ul><li>Publikowanie\*<ul><li>dzienniki\* (jeśli jest to wymienione w publishOptions)</li><li>System plików refs\*</li><li>Widoki\* (jeśli jest to wymienione w publishOptions)</li><li>Wwwroot\* (jeśli jest to wymienione w publishOptions)</li><li>.dll — pliki</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.pdb</li><li>moja_aplikacja. PrecompiledViews.dll (jeśli prekompilowanie widokami Razor)</li><li>moja_aplikacja. PrecompiledViews.pdb (jeśli prekompilowanie widokami Razor)</li><li>MyApp.runtimeconfig.JSON</li><li>plik Web.config (jeśli jest to wymienione w publishOptions)</li></ul></li></ul> |
\*Wskazuje katalog

Zawartość *publikowania* reprezentuje katalog *ścieżki katalogu głównego zawartości*, nazywany również *podstawowa ścieżka aplikacji*, wdrożenia. Nazwa znajduje się do *publikowania* directory we wdrożeniu, jego lokalizacji służy jako ścieżka fizyczna serwera hostowanej aplikacji. *Wwwroot* katalogu, jeśli jest obecny, zawiera tylko zasoby statyczne. *Dzienniki* katalogu mogą zostać zawarte we wdrożeniu, należy go utworzyć w projekcie i dodać `<Target>` element przedstawione poniżej, aby Twoje *.csproj* pliku lub fizycznie tworzenia katalogu na serwer.

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

Katalog wdrożenia wymaga uprawnienia odczytu/wykonywania podczas *dzienniki* katalog wymaga uprawnień do odczytu/zapisu. Dodatkowe katalogi, w którym zostanie zapisany zasoby muszą mieć uprawnienia odczytu/zapisu.
