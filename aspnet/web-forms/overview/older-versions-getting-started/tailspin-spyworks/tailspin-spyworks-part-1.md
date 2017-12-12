---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "Część 1: Plik -> Nowy projekt | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 1 zawiera przegląd/nowy plik projektu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>Część 1: Plik -> Nowy projekt
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 1 zawiera przegląd/nowy plik projektu.


## <a id="_Toc260221666"></a>Omówienie

W tym samouczku jest wprowadzenie do formularzy sieci Web ASP.NET. Firma Microsoft będzie powoli uruchamianie, środowisko rozwoju poziomu sieci web dla początkujących użytkowników jest poprawny.

Aplikacji, które firma Microsoft będzie zbudować jest proste sklepu online.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Odwiedzający mogą przeglądać produkty według kategorii:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Mogą wyświetlać jeden produkt i dodać go do ich koszyka:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Mogą one przejrzeć ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Przejściem do wyewidencjonowania wyświetli monit o ich do

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Po określeniu kolejności, zobaczy ekran potwierdzenia prosty:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Rozpocznie się przez utworzenie nowego projektu formularzy sieci Web ASP.NET w programie Visual Studio 2010, a następnie stopniowo dodamy funkcje tworzenia kompletna aplikacja działa. Przeglądania omówione zostaną następujące czynności dostępu do bazy danych, widoki listy i siatki, strony aktualizacji danych, sprawdzanie poprawności danych, za pomocą stron wzorcowych układ strony spójne, AJAX, weryfikacji i użytkownika członkostwa.

Można wykonać krok po kroku, można również pobrać ukończona aplikacja z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Możesz użyć programu Visual Studio 2010 lub wolnego Visual Web Developer 2010 z [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Do skompilowania aplikacji, można użyć programu SQL Server lub wolnego programu SQL Server Express do hosta bazy danych.

## <a id="_Toc260221667"></a>Plik / nowy projekt

Zaczniemy, wybierając nowy projekt z menu Plik w programie Visual Studio. Spowoduje to wyświetlenie okna dialogowego Nowy projekt.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Wybierzemy Visual C# / szablonów sieci Web grupy po lewej stronie, a następnie wybierz szablon "Aplikacja sieci Web ASP.NET" w środkowej kolumnie. Nazwa projektu TailspinSpyworks i naciśnij przycisk OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Spowoduje to utworzenie naszych projektu. Spójrzmy na foldery, które są uwzględnione w naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Puste rozwiązanie nie jest pusty — dodaje strukturę folderów podstawowe:

![](tailspin-spyworks-part-1/_static/image1.png)

Należy pamiętać, konwencje implementowane przez domyślny szablon projektu programu ASP.NET 4.

- Folder "Konto" implementuje podstawowy interfejs użytkownika dla środowiska ASP. Podsystem członkostwa w sieci.
- Folder "Skryptów" służy jako repozytorium plików JavaScript po stronie klienta i podstawowe jQuery js pliki są udostępniane domyślnie.
- Folder "Style" jest używany do organizowania elementów wizualnych naszych witryny sieci web (arkusze stylów CSS)

Gdy firma Microsoft naciśnij klawisz F5, aby uruchomić aplikację i renderowania strony default.aspx przedstawiono poniżej.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Naszym pierwszym rozszerzenie aplikacji należy zastąpić plik Style.css z szablonu domyślnego WebForms klas CSS i powiązane pliki obrazów renderowanych visual asthetics, interesujące dla aplikacji Tailspin Spyworks.

Po wykonaniu tej czynności naszą stronę default.aspx renderuje się następująco.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Zwróć uwagę, linki obraz u góry rogu strony i elementy menu, które zostały dodane do strony wzorcowej. Tylko łącza "Logowania" i "Konto" wskaż stron, które istnieją (generowane przez szablon domyślny) i pozostałej części strony, które wprowadzimy jak mamy utworzyć w naszej aplikacji.

Chcemy też przenosić do katalogu stylów strony wzorcowej. Chociaż jest to tylko preferencję go może ułatwić nieco Jeśli firma zdecyduje się upewnij "skinable" naszej aplikacji w przyszłości.

Po wykonaniu, to trzeba będzie zmienić strony wzorcowej odwołań w wszystkie pliki aspx generowane przez domyślne strony formularzy sieci Web ASP.NET.

>[!div class="step-by-step"]
[Dalej](tailspin-spyworks-part-2.md)
