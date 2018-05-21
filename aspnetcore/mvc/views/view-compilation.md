---
title: Kompilacja pliku razor i wstępnej kompilacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o zaletach prekompilowanie Razor plików i sposobu wykonywania wstępnej kompilacji pliku Razor w aplikacji platformy ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 03b11116a15c291452acd878e32cd015dc553dcc
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/20/2018
---
# <a name="razor-file-compilation-in-aspnet-core"></a>Kompilacja pliku razor w ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
Po wywołaniu skojarzonego widoku MVC pliku Razor jest kompilowana w czasie wykonywania. Publikowanie pliku Razor czas kompilacji nie jest obsługiwane. Opcjonalnie można kompilować pliki razor w czasie publikacji i wdrożone z aplikacją&mdash;narzędzie wstępnej kompilacji.
::: moniker-end
::: moniker range="= aspnetcore-2.0"
Plik Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC. Publikowanie pliku Razor czas kompilacji nie jest obsługiwane. Opcjonalnie można kompilować pliki razor w czasie publikacji i wdrożone z aplikacją&mdash;narzędzie wstępnej kompilacji.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Plik Razor jest kompilowana w czasie wykonywania, po wywołaniu skojarzonego widoku Razor strony lub MVC. Razor pliki kompilowane w obu kompilacji i opublikować za pomocą czasu [Razor SDK](xref:mvc/razor-pages/sdk).
::: moniker-end

## <a name="precompilation-considerations"></a>Zagadnienia dotyczące wstępnej kompilacji

Efekty uboczne prekompilowanie Razor plików są następujące:

* Mniejsze opublikowanego pakietu
* Skrócić czas uruchamiania
* Nie można edytować pliki Razor&mdash;nie ma skojarzonej zawartości z opublikowanego pakietu.

## <a name="deploy-precompiled-files"></a>Wdrażanie wstępnie skompilowanych plików

::: moniker range=">= aspnetcore-2.1"
Kompilacja kompilacji i opublikować godzinę Razor plików jest włączona domyślnie przez zestaw SDK Razor. Edytowanie plików Razor po te są aktualizowane jest obsługiwana w czasie kompilacji. Domyślnie tylko skompilowanych *Views.dll* i nie *.cshtml* pliki zostały wdrożone za pomocą aplikacji.

> [!IMPORTANT]
> Zestaw SDK Razor jest efektywne tylko wtedy, gdy żadne właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu. Na przykład ustawienie *.csproj* pliku `MvcRazorCompileOnPublish` właściwości `true` wyłącza Razor SDK.
::: moniker-end

::: moniker range="= aspnetcore-2.0"
Jeśli projekt jest przeznaczony dla środowiska .NET Framework, zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.

Szablony projektów platformy ASP.NET Core 2.x ustawiane niejawnie `MvcRazorCompileOnPublish` właściwości `true` domyślnie. W rezultacie tego elementu można bezpiecznie usunąć z *.csproj* pliku.

> [!IMPORTANT]
> Wstępnej kompilacji pliku razor jest niedostępna podczas wykonywania [niezależne wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w programie ASP.NET 2.0 Core.
::: moniker-end

::: moniker range="= aspnetcore-1.1"
Ustaw `MvcRazorCompileOnPublish` właściwości `true`i zainstaluj [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) pakietu NuGet. Następujące *.csproj* przykładzie wyróżniono te ustawienia:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
Przygotowanie aplikacji dla [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [.NET Core interfejsu wiersza polecenia Opublikuj polecenia](/dotnet/core/tools/dotnet-publish). Na przykład uruchom następujące polecenie w katalogu głównym projektu:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* zawierający skompilowane pliki Razor, jest generowany po wstępnej kompilacji zakończy się pomyślnie. Na przykład poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* w *WebApplication1.PrecompiledViews.dll*:

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
* <xref:mvc/razor-pages/sdk>
::: moniker-end