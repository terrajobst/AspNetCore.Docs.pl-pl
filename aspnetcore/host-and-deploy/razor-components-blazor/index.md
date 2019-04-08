---
title: Hostowanie i wdrażanie składników Razor i Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje składniki Razor i Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069778"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a>Hostowanie i wdrażanie składników Razor i Blazor

Przez [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), i [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania przy użyciu [publikowania dotnet](/dotnet/core/tools/dotnet-publish) polecenia. Zintegrowanym środowisku programistycznym (IDE) może obsługiwać wykonywanie `dotnet publish` polecenia automatycznie przy użyciu jego wbudowanych funkcji publikowania, dzięki czemu może nie być konieczne ręczne wykonaj polecenie w wierszu polecenia, w zależności od rozwoju narzędzia w użyciu.

```console
dotnet publish -c Release
```

`dotnet publish` Wyzwalacze [przywrócić](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompilacje](/dotnet/core/tools/dotnet-build) projekcie przed utworzeniem zasobów dla wdrożenia. Jako część procesu kompilacji Aby zmniejszyć rozmiar pobierania aplikacji i czasów ładowania są usuwane nieużywane metod i zestawów. Wdrożenie jest tworzony w */bin/wersji / {struktury docelowej} / publish* folderu.

Zasoby w *publikowania* folderu wdrożonych na serwerze sieci web. Wdrożenie może być ręcznego lub zautomatyzowanego procesu w zależności od poziomu narzędzi deweloperskich w użyciu.

## <a name="deployment"></a>wdrażania

Aby uzyskać wskazówki dotyczące wdrażania zobacz następujące tematy:

* [Blazor po stronie klienta](xref:host-and-deploy/razor-components-blazor/blazor)
* [Server-side ASP.NET Core Razor Components](xref:host-and-deploy/razor-components-blazor/razor-components)
