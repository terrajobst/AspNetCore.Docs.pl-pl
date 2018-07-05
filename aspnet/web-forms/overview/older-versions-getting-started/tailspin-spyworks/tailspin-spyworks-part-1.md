---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Część 1: Plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 1 obejmuje omówienie i plik/nowy projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 8c346e88277b6cf43676f7c6b58f9679a591d634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388214"
---
<a name="part-1-file--new-project"></a>Część 1: Plik -> Nowy projekt
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 1 obejmuje omówienie i plik/nowy projekt.


## <a id="_Toc260221666"></a>  Przegląd

Ten samouczek stanowi wprowadzenie do formularzy sieci Web platformy ASP.NET. Firma Microsoft będzie uruchamiana powoli, a więc środowisko programowania dla początkujących poziomu sieci web jest poprawny.

Aplikacja, którą tworzymy będzie jest proste magazyn online.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Goście mogą przeglądać produktów według kategorii:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Mogą wyświetlać pojedynczego produktu i dodać go do koszyka ich:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Mogą przeglądać ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Przejściem do wyewidencjonowania spowoduje wyświetlenie monitu o

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Po określeniu kolejności, zobaczy ekran potwierdzenia proste:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Firma Microsoft rozpocznie się, tworząc nowy projekt formularzy sieci Web platformy ASP.NET w programie Visual Studio 2010, a przyrostowe dodamy funkcje umożliwiające tworzenie kompletna aplikacja działa. Po drodze omówimy dostępu do bazy danych, widoku listy i siatki, stron aktualizacji danych, sprawdzanie poprawności danych, za pomocą stron wzorcowych dla układu strony spójne, AJAX, weryfikacji i użytkownika członkostwa.

Można wykonać krok po kroku, można również pobrać gotową aplikację z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Możesz użyć programu Visual Studio 2010 lub bezpłatnego programu Visual Web Developer 2010 z [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Aby skompilować aplikację, można użyć programu SQL Server lub bezpłatnego programu SQL Server Express do hosta bazy danych.

## <a id="_Toc260221667"></a>  Plik / nowy projekt

Zaczniemy, wybierając nowy projekt z menu Plik w programie Visual Studio. Wyświetlenie okna dialogowego Nowy projekt.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Wybierzemy Visual C# / szablonów sieci Web grupy po lewej stronie i w środkowej kolumnie Wybierz szablon "Aplikacja sieci Web programu ASP.NET". Nazwij swój projekt TailspinSpyworks i naciśnij przycisk OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Spowoduje to utworzenie projekcie. Spójrzmy na foldery, które znajdują się w naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Puste rozwiązanie nie jest całkowicie pusty — dodaje strukturę folderów podstawowe:

![](tailspin-spyworks-part-1/_static/image1.png)

Należy pamiętać, konwencje implementowany przez domyślny szablon projektu platformy ASP.NET 4.

- Folder "Konto" implementuje podstawowy interfejs użytkownika dla stron ASP. Podsystem członkostwo w sieci.
- Folder "Skryptów" służy jako repozytorium dla plików JavaScript po stronie klienta i podstawowe pliki .js jQuery są udostępnione domyślnie.
- Folder "Style" służy do organizowania nasze elementy wizualne witryny sieci web (arkusze stylów CSS)

Po naciśnięciu klawisza F5, aby uruchomić aplikację i renderowania strony default.aspx zobaczymy poniższe.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Naszym pierwszym rozszerzenie aplikacji będzie zastąpić plik Style.css za pomocą domyślnego szablonu formularzy sieci Web przy użyciu klas CSS i powiązane pliki obrazów, które będą renderowane asthetics wizualne, które warto dla naszej aplikacji Tailspin Spyworks.

Po wykonaniu tej czynności powoduje wyświetlenie strony default.aspx następująco.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Zwróć uwagę, linków obrazów w prawym górnym rogu strony i elementy menu, które zostały dodane do strony wzorcowej. Tylko łącza "Sign In" i "Konto" wskaż stron, które istnieją (wygenerowanych przez szablon domyślny) i pozostałej części strony, które wdrażamy jak nasza aplikacja jest wbudowana.

Przedstawimy również przenosić strony wzorcowej do katalogu style. Chociaż jest to tylko preferencję rozsądne może okazać się elementów była nieco łatwiejsza, jeśli firma zdecyduje się wprowadzić naszej aplikacji w "skinable" w przyszłości.

Po wykonaniu tego musisz zmienić stronę wzorcową odwołań we wszystkich plikach aspx generowane przez domyślne strony formularzy sieci Web platformy ASP.NET.

> [!div class="step-by-step"]
> [Next](tailspin-spyworks-part-2.md)
