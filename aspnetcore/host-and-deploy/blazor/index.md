---
title: Hostowanie i wdrażanie Blazor
author: guardrex
description: Dowiedz się, jak udostępniać i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5def0356d13975211dd234f6a6a9f5a993d003b7
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223181"
---
# <a name="host-and-deploy-blazor"></a>Hostowanie i wdrażanie Blazor

Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz **kompilacji** > **publikowania {aplikacja}** na pasku nawigacyjnym.
1. Wybierz *publikowania docelowej*. Aby opublikować lokalnie, wybierz **folderu**.
1. Zaakceptuj lokalizację domyślną **wybierz folder** pola lub określić inną lokalizację. Wybierz **Publikuj** przycisku.


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Program Visual Studio Code / .NET Core interfejsu wiersza polecenia](#tab/visual-studio-code+netcore-cli)

Użyj [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenie w celu opublikowania aplikacji przy użyciu konfiguracji wydania:

```console
dotnet publish -c Release
```

---

Publikowanie wyzwalacze aplikacji [przywrócić](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompilacje](/dotnet/core/tools/dotnet-build) projekcie przed utworzeniem zasobów dla wdrożenia. Jako część procesu kompilacji Aby zmniejszyć rozmiar pobierania aplikacji i czasów ładowania są usuwane nieużywane metod i zestawów.

Opublikowaniu aplikacji po stronie klienta Blazor */bin/wersji / {struktury docelowej} / dist* folderu. Opublikowaniu aplikacji po stronie serwera Blazor */bin/wersji / {struktury docelowej} / publish* folderu.

Serwer sieci web są wdrażane zasoby w folderze. Wdrożenie może być ręcznego lub zautomatyzowanego procesu w zależności od poziomu narzędzi deweloperskich w użyciu.

## <a name="deployment"></a>wdrażania

Aby uzyskać wskazówki dotyczące wdrażania zobacz następujące tematy:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Blazor bez użycia serwera hostingu z usługą Azure Storage

Aplikacje klienta Blazor mogą być udostępniane przez [usługi Azure Storage](https://azure.microsoft.com/services/storage/) jako zawartość statyczną bezpośrednio z kontenera magazynu.

Aby uzyskać więcej informacji, zobacz [hosta i wdrażanie Blazor po stronie klienta (wdrożenie autonomicznego): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).
