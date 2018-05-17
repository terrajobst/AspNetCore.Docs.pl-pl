---
title: Kompilacja widoku razor i wstępnej kompilacji w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak włączyć kompilacji widoku MVC Razor i wstępnej kompilacji w aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Razor (cshtml) pliku kompilacji w ASP.NET Core

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Widokami razor są kompilowane w czasie wykonywania, gdy widok jest wywoływany. Platformy ASP.NET Core 2.1.0 lub nowszym widoki kompilacji w kompilacji i opublikować za pomocą czasu [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk). W ASP.NET Core 1.1 i ASP.NET Core 2.0, widoki Opcjonalnie można kompilowana w publikacji i wdrażać z aplikacją &mdash; narzędzie wstępnej kompilacji. 



Kwestie do rozważenia wstępnej kompilacji:

* Prekompilowanie widoków powoduje mniejsze opublikowanego pakietu i skrócić czas uruchamiania.
* Nie można edytować pliki Razor prekompilowanie widoków. Edytowany widoków nie będzie znajdować się w opublikowanego pakietu. 

Aby wdrożyć prekompilowany widoków:

# <a name="aspnet-core-21tabaspnetcore21"></a>[Platformy ASP.NET Core 2.1](#tab/aspnetcore21/)
Tworzenie i publikowanie czas kompilacji Razor plików jest włączona domyślnie przez zestaw Sdk Razor. Edytowanie plików Razor, po ich aktualizacji jest obsługiwana w czasie kompilacji. Domyślnie tylko skompilowanych *Views.dll* i cshtml żadne pliki nie zostały wdrożone za pomocą aplikacji. 
    
> [!IMPORTANT]
> Zestaw Sdk Razor jest efektywne tylko wtedy, gdy nie właściwości specyficzne dla wstępnej kompilacji są ustawione w pliku projektu. Na przykład ustawienie `MvcRazorCompileOnPublish` w Twojej *.csproj* pliku spowoduje wyłączenie Razor Sdk.

# <a name="aspnet-core-20tabaspnetcore20"></a>[Platformy ASP.NET Core 2.0](#tab/aspnetcore20/)

Jeśli projekt jest przeznaczony dla środowiska .NET Framework, zawierać odwołanie do pakietu [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Jeśli projekt jest przeznaczony dla platformy .NET Core, żadne zmiany nie są niezbędne.

Szablony projektów platformy ASP.NET Core 2.x niejawnie ustawia `MvcRazorCompileOnPublish` do `true` domyślnie oznacza tego węzła można bezpiecznie usunąć z *.csproj* pliku.
    
> [!IMPORTANT]
> Wstępnej kompilacji widoku razor, gdy nie jest dostępny wykonywania [niezależne wdrożenia (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) w programie ASP.NET 2.0 Core. 

Przygotowanie aplikacji dla [wdrożenia zależne od framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) z [.NET Core interfejsu wiersza polecenia Opublikuj polecenia](/dotnet/core/tools/dotnet-publish). Na przykład uruchom następujące polecenie w katalogu głównym projektu:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* zawierający skompilowanych widokami Razor, jest generowany po wstępnej kompilacji zakończy się pomyślnie. Na przykład poniższy zrzut ekranu przedstawia zawartość *Index.cshtml* wewnątrz *WebApplication1.PrecompiledViews.dll*:

![Widokami razor wewnątrz biblioteki DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Ustaw `MvcRazorCompileOnPublish` do `true` i zawiera odwołanie do pakietu `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Następujące *.csproj* przykładzie wyróżniono te ustawienia:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

