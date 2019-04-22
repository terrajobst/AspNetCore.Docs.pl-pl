---
title: Hostowanie i wdrażanie Blazor
author: guardrex
description: Dowiedz się, jak udostępniać i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 774fbb6fdaab14a015db4fb39de2e1ea73a1837b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982643"
---
# <a name="host-and-deploy-blazor"></a>Hostowanie i wdrażanie Blazor

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

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
