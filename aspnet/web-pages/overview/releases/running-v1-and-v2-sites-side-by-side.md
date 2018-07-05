---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Uruchamianie różnych wersji składnika ASP.NET Web Pages (Razor) obok siebie | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób uruchamiania witryn sieci Web ASP.NET Web Pages (Razor) na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: aa2bf41054540cc67b7c0b4669e356e732fed8d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402460"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Równoległe uruchamianie różnych wersji wzorca ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób uruchamiania witryn sieci Web ASP.NET Web Pages (Razor) na tym samym komputerze lub serwerze, gdy witryn sieci Web są skonfigurowane do używania różnych wersji składnika ASP.NET Web Pages.
> 
> Zawartość:
> 
> - Domyślne zachowanie nowości w programie ASP.NET: w przypadku działania witryn utworzonych przy użyciu stron ASP.NET Web Pages.
> - Jak skonfigurować nową lokację do uruchamiania w starszej wersji składnika ASP.NET Web Pages.
>   
> 
> Jest to ASP.NET wprowadzonej w artykule:
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
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2 i stron sieci Web platformy ASP.NET w wersji 1.0.


Strony ASP.NET Web Pages obsługuje możliwość uruchamiania witryn sieci Web obok siebie. Dzięki temu można w dalszym ciągu uruchamiać starsze aplikacje ASP.NET Web Pages, tworzyć nowe aplikacje ASP.NET Web Pages i uruchomić wszystkie z nich na tym samym komputerze.

Oto kilka rzeczy do zapamiętania, po zainstalowaniu na stronach sieci Web za pomocą programu WebMatrix:

- Domyślnie istniejących aplikacji stron sieci Web działa jako najnowszej wersji na komputerze. (Zestawy są zainstalowane w globalnej pamięci podręcznej zestawów (GAC) i są automatycznie używane).
- Jeśli chcesz uruchomić witrynę, przy użyciu różnych wersji składnika ASP.NET Web Pages można skonfigurować witryny, aby to zrobić. Jeśli witryna nie ma jeszcze *web.config* plików w katalogu głównym witryny, Utwórz nową i skopiuj następujący kod XML do niego, zastępując istniejącą zawartość. Jeśli witryna zawiera już *web.config* Dodaj `<appSettings>` element podobny do następującego do `<configuration>` sekcji.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  "— Jeśli nie określisz wersji w *web.config* pliku lokacji jest wdrażany jako najnowszej wersji. (Zestawy są kopiowane do *bin* folder w witrynie wdrożone.)
- Nowe aplikacje, które możesz utworzyć przy użyciu szablonów witryn w sieci Web macierzy zawierają zestawy wersję stron sieci Web w tej witrynie *bin* folderu.

Ogólnie rzecz biorąc, zawsze można kontrolować wersję stron sieci Web za pomocą witryny za pomocą pakietu NuGet do instalowania odpowiednich zestawów w witrynie *bin* folderu. Pakietów można znaleźć [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Dodatkowe zasoby

[Najważniejsze funkcje we wzorcu ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
