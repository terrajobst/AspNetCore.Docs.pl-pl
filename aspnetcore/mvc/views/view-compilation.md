---
title: "Kompilacja widoku razor i wstępnej kompilacji w ASP.NET Core"
author: rick-anderson
description: "Dowiedz się, jak włączyć kompilacji widoku MVC Razor i wstępnej kompilacji w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: af1137a087ed145675f38ce5a10fd553044c5582
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Kompilacja widoku razor i wstępnej kompilacji w ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Widokami razor są kompilowane w czasie wykonywania, gdy widok jest wywoływany. ASP.NET podstawowe 1.1.0 i wyższe można opcjonalnie widokami Razor skompilować i wdrożyć je z aplikacją&mdash;proces znany jako wstępnej kompilacji. Szablony projektów platformy ASP.NET Core 2.x domyślnie włączone wstępnej kompilacji.

> [!IMPORTANT]
> Wstępnej kompilacji widoku razor jest obecnie niedostępna podczas wykonywania [niezależne wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w programie ASP.NET 2.0 Core. Funkcja będzie dostępna dla SCDs, gdy zwalnia 2.1. Aby uzyskać więcej informacji, zobacz [widoku kompilacja zakończy się niepowodzeniem, podczas kompilowania między dla systemu Linux w systemie Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Kwestie do rozważenia wstępnej kompilacji:

* Prekompilowanie widoków powoduje mniejsze opublikowanego pakietu i skrócić czas uruchamiania.
* Nie można edytować pliki Razor prekompilowanie widoków. Edytowany widoków nie będzie znajdować się w opublikowanego pakietu. 

Aby wdrożyć prekompilowany widoków:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Jeśli projekt jest przeznaczony dla środowiska .NET Framework, zawierać odwołanie do pakietu [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.

Szablony projektów platformy ASP.NET Core 2.x ustawiane niejawnie `MvcRazorCompileOnPublish` do `true` domyślnie oznacza tego węzła można bezpiecznie usunąć z *.csproj* pliku. Jeśli wolisz można jawne, nie powoduje żadnych problemów w ustawieniu `MvcRazorCompileOnPublish` właściwości `true`. Następujące *.csproj* przykładzie wyróżniono tego ustawienia:

[!code-xml[](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ustaw `MvcRazorCompileOnPublish` do `true`i zawiera odwołanie do pakietu `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Następujące *.csproj* przykładzie wyróżniono te ustawienia:

[!code-xml[](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Przygotowanie aplikacji dla [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) , wykonując polecenie, takie jak wymienione poniżej w katalogu głównym projektu:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* zawierający skompilowanych widokami Razor, jest generowany po wstępnej kompilacji zakończy się pomyślnie. Na przykład poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* wewnątrz *WebApplication1.PrecompiledViews.dll*:

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)
