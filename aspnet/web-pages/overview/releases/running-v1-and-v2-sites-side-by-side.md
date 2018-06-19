---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Uruchomione różne wersje stron sieci Web platformy ASP.NET (Razor) obok siebie | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób uruchamiania stron sieci Web platformy ASP.NET (Razor) witryn sieci Web na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898409"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Równolegle z różnymi wersjami stron sieci Web platformy ASP.NET (Razor)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób uruchamiania stron sieci Web platformy ASP.NET (Razor) witryn sieci Web na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji składnika ASP.NET Web Pages.
> 
> Zawartość:
> 
> - Domyślne zachowanie nowości w programie ASP.NET w przypadku witryn utworzonych z ASP.NET Web Pages.
> - Jak skonfigurować nową lokację do uruchamiania przy użyciu starszej wersji składnika ASP.NET Web Pages.
>   
> 
> Jest to funkcja ASP.NET, wprowadzona w artykule:
> 
> - `webPages:Version` Ustawienia konfiguracji.
>   
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z ASP.NET Web Pages 2 i stron sieci Web ASP.NET w wersji 1.0.


Strony sieci Web ASP.NET obsługuje możliwości korzystania z witryn sieci Web obok siebie. Dzięki temu można nadal uruchamiać starsze aplikacje ASP.NET Web Pages, tworzenie nowej aplikacji ASP.NET Web Pages i uruchomić wszystkie z nich na tym samym komputerze.

Poniżej przedstawiono niektóre czynności podczas instalacji stron sieci Web za pomocą programu WebMatrix:

- Domyślnie istniejących aplikacji stron sieci Web zostanie uruchomiony jako najnowszą wersję na komputerze. (Zestawy są zainstalowane w globalnej pamięci podręcznej zestawów (GAC) i są automatycznie używane).
- Jeśli chcesz uruchomić lokacji za pomocą innej wersji składnika ASP.NET Web Pages, można skonfigurować witryny, w tym celu. Jeśli witryna nie ma jeszcze *web.config* plików w katalogu głównym witryny, Utwórz nową i skopiuj następujący kod XML, zastępowanie istniejącej zawartości. Jeśli witryna zawiera już *web.config* plików, dodawanie `<appSettings>` element podobny do następującego do `<configuration>` sekcji.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  "— Jeśli nie określisz wersji w *web.config* pliku lokacji jest wdrożona jako najnowszej wersji. (Zestawy są kopiowane do *bin* folder w witrynie wdrożonej.)
- Nowe aplikacje tworzone przy użyciu szablonów witryn w sieci Web macierzy zawierają zestawy wersję stron sieci Web w tej witrynie *bin* folderu.

Ogólnie rzecz biorąc, zawsze można kontrolować wersję stron sieci Web do użycia z lokacji za pomocą NuGet odpowiednich zestawów do witryny *bin* folderu. Pakietów można znaleźć [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Dodatkowe zasoby

[Najważniejsze funkcje w składniku ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
