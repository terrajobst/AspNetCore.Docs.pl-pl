---
title: Metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x i nowsze
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage obejmuje wszystkie obsługiwane pakiety Entity Framework Core i ASP.NET Core, wraz z ich zależności."
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapackage Microsoft.AspNetCore.All dla platformy ASP.NET Core 2.x

Ta funkcja wymaga platformy ASP.NET Core 2.x docelowy .NET Core 2.x.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage dla platformy ASP.NET Core obejmuje:

* Wszystkie obsługiwane pakiety przez zespół platformy ASP.NET Core.
* Wszystkie obsługiwane pakiety przez Entity Framework Core. 
* Zależności wewnętrzne i firm 3rd używane przez Entity Framework Core i ASP.NET Core. 

Wszystkie funkcje platformy ASP.NET Core 2.x i Entity Framework Core 2.x znajdują się w `Microsoft.AspNetCore.All` pakietu. Tego pakietu używać domyślnych szablonów projektu.

Numer wersji `Microsoft.AspNetCore.All` metapackage reprezentuje wersji platformy ASP.NET Core i wersji programu Entity Framework Core (zgodne z wersją .NET Core).

Aplikacje używające `Microsoft.AspNetCore.All` metapackage automatycznie korzystać z [.NET Core środowiska uruchomieniowego magazynu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Magazyn środowiska uruchomieniowego zawiera wszystkie zasoby środowiska uruchomieniowego potrzebne do uruchomienia aplikacji 2.x platformy ASP.NET Core. Jeśli używasz `Microsoft.AspNetCore.All` metapackage, **nie** zasobów, z którym związane są odwołania pakietów platformy ASP.NET Core NuGet zostały wdrożone za pomocą aplikacji &mdash; magazynie środowiska uruchomieniowego .NET Core zawiera te zasoby. Zasoby w magazynie środowiska uruchomieniowego są wstępnie skompilowana zwiększające czasu uruchomienia aplikacji.

Proces przycinanie pakietu służy do usuwania pakietów, które nie są używane. Przycięte pakiety są wyłączone w danych wyjściowych opublikowanej aplikacji.

Następujące *.csproj* pliku odwołań `Microsoft.AspNetCore.All` metapackage dla platformy ASP.NET Core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
