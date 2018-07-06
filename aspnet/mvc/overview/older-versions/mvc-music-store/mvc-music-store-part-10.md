---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Część 10: Końcowe aktualizacje nawigacji i projektu witryny, podsumowanie | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 10 obejmuje końcowe aktualizacje nawigacji i S...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836813"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Część 10: Końcowe aktualizacje nawigacji i projektu witryny, podsumowanie
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 10 obejmuje końcowe aktualizacje nawigacji i projektu witryny, podsumowanie.


Najważniejsze funkcje została ukończona w naszej witrynie, ale wciąż istnieje pewne funkcje do dodania do nawigacji w witrynie, strony głównej i stronie przeglądanie Store.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Tworzenie zakupów podsumowania widoku częściowego koszyka

Chcemy udostępnić liczba elementów w koszyku użytkownika w całej lokacji.

![](mvc-music-store-part-10/_static/image1.png)

Firma Microsoft można łatwo zaimplementować to przez utworzenie widoku częściowego, który zostanie dodany do naszych Site.master.

Jak pokazano wcześniej, kontroler ShoppingCart obejmuje CartSummary metody akcji, która zwraca widok częściowy:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Aby utworzyć widok częściowy CartSummary, kliknij prawym przyciskiem myszy w folderze Widoki/ShoppingCart i wybierz pozycję Dodaj widok. Nazwa widoku CartSummary i zaznacz pole wyboru "Utwórz widok częściowy", jak pokazano poniżej.

![](mvc-music-store-part-10/_static/image2.png)

Widok częściowy CartSummary jest bardzo proste — jest tylko łącze do widoku ShoppingCart indeksu, który pokazuje liczbę elementów w koszyku. Kompletny kod dla CartSummary.cshtml jest następująca:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Firma Microsoft może zawierać widoku częściowego w dowolnej stronie w witrynie, łącznie z lokacji głównej, przy użyciu metody Html.RenderAction. RenderAction wymaga od nas określić nazwę akcji ("CartSummary") i nazwy kontrolera ("ShoppingCart") jako poniżej.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Przed dodaniem to lokacja układu, zostaną również utworzone Menu gatunku umożliwia wykonywanie wszystkich naszych Site.master aktualizacji w tym samym czasie.

## <a name="creating-the-genre-menu-partial-view"></a>Tworzenie widoku częściowego Menu gatunku

Firma Microsoft może ułatwić partii dla naszych użytkowników przejść w sklepie, dodając Menu gatunku, która zawiera listę wszystkich gatunki dostępne w naszym Sklepie.

![](mvc-music-store-part-10/_static/image3.png)

Firma Microsoft będzie należy wykonać takie same kroki również utworzyć widok częściowy GenreMenu, a następnie możemy Dodaj oba te do gałęzi głównej witryny. Najpierw dodaj następujące akcji kontrolera GenreMenu do StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Ta akcja zwraca listę gatunki, które będą wyświetlane przez widok częściowy, który zostanie następnie utworzymy.

*Uwaga: Tej akcji kontrolera, co oznacza, że ma być uruchamiany tylko ta akcja ma być używany z widoku częściowego dodaliśmy atrybutu [ChildActionOnly]. Ten atrybut zapobiega wykonywana, przechodząc do /Store/GenreMenu tej akcji kontrolera. Nie jest to wymagane dla widoków częściowych, ale jest dobrym rozwiązaniem, ponieważ chcemy się upewnić, że nasze akcji kontrolera są używane jako Chcieliśmy. Możemy również zwróconego PartialView zamiast widoku, który umożliwia aparat widoku, nazywa się czy nie należy używać układu dla tego widoku jest uwzględniane w innych widokach.*

Kliknij prawym przyciskiem myszy na GenreMenu akcji kontrolera oraz tworzenia widoku częściowego o nazwie GenreMenu, który jest silnie typizowane przy użyciu klasy gatunku widoku danych, jak pokazano poniżej.

![](mvc-music-store-part-10/_static/image4.png)

Aktualizowanie kodu widoku GenreMenu widoku częściowego do wyświetlenia elementów przy użyciu nieuporządkowaną listę w następujący sposób.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Trwa aktualizowanie układu witryny do wyświetlenia naszych widoki częściowe

Możemy dodać naszych widoki częściowe do układu witryny (/widoków/Shared/\_Layout.cshtml) przez wywołanie metody Html.RenderAction(). Dodamy ich obu w, a także niektóre dodatkową marżę do wyświetlania, jak pokazano poniżej:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Teraz gdy możemy uruchomić aplikację, zobaczymy gatunku w obszarze nawigacji po lewej stronie i Podsumowanie koszyka u góry.

## <a name="update-to-the-store-browse-page"></a>Aktualizuj stronę Przeglądaj Store

Na stronie przeglądania Store będzie działać, ale nie wygląda bardzo dobra. Firma Microsoft może aktualizować strony Aby pokazać albumów w lepszego układu, aktualizując widok kodu (znajdujący się w /Views/Store/Browse.cshtml) w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

W tym miejscu wprowadzamy użytkowania Url.Action zamiast Html.ActionLink, dzięki czemu można zastosować specjalne formatowanie łącza do uwzględnienia kompozycji albumu.

*Uwaga: Firma Microsoft wyświetlanie tytułowych albumu ogólny, dla tych albumów. Te informacje są przechowywane w bazie danych i można edytować za pomocą Menedżera Store. Zachęcamy do dodania własnej kompozycji.*

Teraz gdy możemy przejść do określonego rodzaju, zobaczymy albumów wyświetlane w siatce z w kompozycją albumu.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualizowanie stronę główną, aby pokazać albumów sprzedaży w górnej

Chcemy są wyposażone w naszym najlepiej sprzedających się ze zdjęciami, na stronie głównej, aby zwiększyć sprzedaż. Wybierzemy niektórych aktualizacji do naszych HomeController obsługiwały, a następnie dodaj niektóre dodatkowe grafiki.

Najpierw dodamy właściwość nawigacji do klasy Nasze albumu tak, aby EntityFramework wie, że są one skojarzone. Kilka ostatnich wierszy z naszych **albumu** klasy powinna teraz wyglądać następująco:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Uwaga: Wymaga to dodanie przy użyciu instrukcji w przestrzeni nazw System.Collections.Generic.*

Najpierw dodamy, pole storeDB i MvcMusicStore.Models przy użyciu instrukcji, tak jak w naszych innych kontrolerów. Następnie dodamy następującą metodę do HomeController, który sprawdza naszej bazie danych, odnaleźć najważniejsze albumów sprzedaży według OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Jest to metoda prywatna, ponieważ nie chcemy udostępnić go jako akcji kontrolera. Firma Microsoft jest uwzględniania go w HomeController dla uproszczenia, ale zaleca się przeniesienie logikę biznesową do klasy osobną usługą zgodnie z potrzebami.

Mając to na miejscu możemy zaktualizować akcji kontrolera indeks zapytania 5 najlepiej sprzedających się ze zdjęciami i przywrócić je do widoku.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Kompletny kod dla zaktualizowanego HomeController to, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Na koniec musimy aktualizacja naszych widoku Home indeks, tak, aby go wyświetlić listę albumów aktualizowanie typu modelu i dodając listy albumu do dołu. Firma Microsoft będzie okazji można również dodać nagłówek i sekcja promocji do strony.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Teraz gdy możemy uruchomić aplikację, zobaczymy zaktualizowane strony głównej z najważniejszych sprzedaży ze zdjęciami i naszej promocyjne wiadomości.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Wniosek

Zobaczyliśmy, że ASP.NET MVC można łatwo utworzyć zaawansowane witryny sieci Web z dostępu do bazy danych, członkostwo w technologii AJAX, itp. bardzo szybko. Miejmy nadzieję w tym samouczku przyznał Ci narzędzia, których potrzebujesz do rozpoczęcia tworzenia własnych platformy ASP.NET MVC aplikacji!


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-9.md)
