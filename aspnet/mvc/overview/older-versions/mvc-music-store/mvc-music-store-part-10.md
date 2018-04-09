---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Część 10: Końcowe aktualizacje do nawigacji i lokacja, zawarcia | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 10 obejmuje końcowe aktualizacje do nawigacji i S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Część 10: Końcowe aktualizacje nawigacji i lokacja, zawarcia
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 10 obejmuje końcowe aktualizacje do nawigacji i lokacja, zawarcia.


Najważniejsze funkcje została ukończona dla witryny, ale wciąż istnieje niektóre funkcje do dodania do nawigacji w witrynie, strony głównej i strony Przeglądaj magazynu.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Tworzenie zakupów Podsumowanie widoku częściowego koszyka

Chcemy ujawniać liczba elementów w koszyku użytkownika w całej lokacji.

![](mvc-music-store-part-10/_static/image1.png)

Firma Microsoft to łatwo zaimplementować przez utworzenie widoku częściowego, która jest dodawana do naszej Site.master.

Jak pokazano wcześniej, kontrolera ShoppingCart obejmuje CartSummary metody akcji, która zwraca widok częściowy:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Aby utworzyć widok częściowy CartSummary, kliknij prawym przyciskiem folder widoków/ShoppingCart i wybierz polecenie Dodaj widok. Nazwa widoku CartSummary i zaznacz pole wyboru "Utwórz widok częściowy", jak pokazano poniżej.

![](mvc-music-store-part-10/_static/image2.png)

Widok częściowy CartSummary jest naprawdę proste — jest tylko łącze do widoku ShoppingCart indeksu, który zawiera liczbę elementów w koszyka. Kompletny kod dla CartSummary.cshtml wygląda następująco:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Firma Microsoft może zawierać widoku częściowego w dowolnej strony w witrynie, w tym wzorcu lokacji przy użyciu metody Html.RenderAction. RenderAction wymaga firmie Microsoft w celu określenia nazwy akcji ("CartSummary") i nazwy kontrolera ("ShoppingCart") jako poniżej.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Przed dodaniem to do lokacji układu, zostaną również utworzone Genre Menu, firma Microsoft może wprowadzać wszystkich naszych Site.master aktualizacji w tym samym czasie.

## <a name="creating-the-genre-menu-partial-view"></a>Tworzenie widoku częściowego Menu Genre

Firma Microsoft może ułatwić znacznie naszych użytkowników poruszać się po magazynie przez dodanie Menu Genre, w której znajduje się lista wszystkich gatunki dostępne w naszym magazynie.

![](mvc-music-store-part-10/_static/image3.png)

Firma Microsoft będzie taka sama wykonaj kroki również utworzyć GenreMenu widoku częściowego, a następnie możemy Dodaj oba te do poziomu głównego witryny. Najpierw dodaj następujące akcji kontrolera GenreMenu do StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Ta akcja zwraca listę gatunkami muzyki, które będą wyświetlane przez widok częściowy, który zostanie utworzony obok.

*Uwaga: Dodano atrybut [ChildActionOnly] do tej akcji kontrolera, co oznacza, że chcemy tylko tę akcję do użycia w widoku częściowego. Ten atrybut uniemożliwi wstrzymywane przechodząc do /Store/GenreMenu akcji kontrolera. Nie jest to wymagane dla widoków częściowych, ale jest dobrym rozwiązaniem, ponieważ chcemy upewnić się, że nasze akcji kontrolera są używane jako mamy zamierzają. Możemy również zwróconego PartialView zamiast widoku, który umożliwia aparat widoku wiedzieć, że nie należy używać układu dla tego widoku, jest on dołączony w innych widokach.*

Kliknij prawym przyciskiem myszy na GenreMenu akcji kontrolera oraz tworzenia widoku częściowego o nazwie GenreMenu, który jest silnie typizowane za pomocą klasy Genre widoku danych, jak pokazano poniżej.

![](mvc-music-store-part-10/_static/image4.png)

Zaktualizuj kod widok GenreMenu widoku częściowego do wyświetlenia elementów przy użyciu nieuporządkowaną listę w następujący sposób.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aktualizowanie lokacji układ do wyświetlenia naszych widoki częściowe

W układzie lokacji można dodać nasze widoki częściowe (/widoki/Shared/\_Layout.cshtml) przez wywołanie metody Html.RenderAction(). Dodamy je w, a także pewne dodatkowe znaczników do wyświetlania, jak pokazano poniżej:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Teraz uruchamianych aplikacji, zostanie wyświetlone Genre w obszarze nawigacji po lewej stronie i Podsumowanie koszyka u góry.

## <a name="update-to-the-store-browse-page"></a>Aktualizowanie do magazynu Przeglądaj strony

Strona magazynu Przeglądaj będzie działać, ale wygląda bardzo dobre. Firma Microsoft może zaktualizować strony, aby pokazać albumów w układzie lepsze, aktualizując widoku kodu (znajdujący się w /Views/Store/Browse.cshtml) w następujący sposób:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

W tym miejscu są czynione Użyj Url.Action zamiast Html.ActionLink, dzięki czemu można zastosować, specjalne formatowanie do łącza obejmują kompozycji albumu.

*Uwaga: Firma Microsoft wyświetlanie tytułowych albumu ogólnych, dla tych albumów. Te informacje są przechowywane w bazie danych i można edytować za pomocą Menedżera magazynu. Jesteś Zapraszamy dodać własne kompozycji.*

Teraz, gdy firma Microsoft przejdź do określonego rodzaju, zostanie wyświetlone albumów siatką z kompozycji albumu.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualizowanie strony głównej pokazanie Top albumów sprzedaży

Chcemy funkcji naszych sprzedających albumów na stronie głównej zwiększenie sprzedaży. Wybierzemy niektórych aktualizacji do naszej HomeController dojście, którego i dodać niektóre dodatkowe grafiki.

Najpierw dodamy właściwości nawigacji do naszej klasy albumu tak, aby EntityFramework wie, że są powiązane. Kilka ostatnich wierszy z naszych **albumu** klasa powinna wyglądać następująco:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Uwaga: Wymaga to dodawanie przy użyciu instrukcji przenieść system.Collections.Generic — przestrzeń nazw.*

Najpierw dodamy pola storeDB i MvcMusicStore.Models przy użyciu instrukcje, jak naszym kontrolerów. Następnie dodamy następującą metodę do HomeController, który wysyła zapytanie do naszej bazie danych można znaleźć górny albumów sprzedaży według SzczegółyZamówień.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Jest to metoda prywatna, ponieważ nie chcemy udostępnić go jako akcji kontrolera. Firma Microsoft zawierają w HomeController dla uproszczenia, ale zaleca się przenieść logiki biznesowej do klasy oddzielne usługi zgodnie z potrzebami.

Z tym w miejscu możemy zaktualizować akcji kontrolera indeksu wyszukiwania 5 najlepiej sprzedających albumów i zwraca je do widoku.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Kompletny kod dla zaktualizowanej HomeController jest, jak pokazano poniżej.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Na koniec musimy zaktualizować naszych widoku indeksu Narzędzia główne, tak, aby go wyświetlić listę albumów aktualizowanie typu modelu i dodawania listy albumu w dół. Teraz nastąpi przekierowanie do tej możliwości, aby również dodać nagłówek i sekcja podwyższanie poziomu do strony.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Uruchamianych aplikacji, firma Microsoft będzie widoczny naszych zaktualizowanej strony głównej z najwyższym albumów sprzedaży i promocyjne wiadomości.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Wniosek

Firma Microsoft w tym samouczku czy ASP.NET MVC ułatwia można tworzyć zaawansowane witryny sieci Web z dostępem do bazy danych, członkostwa, AJAX, itp. bardzo szybko. Miejmy nadzieję, że w tym samouczku przyznał Ci narzędzia, które należy rozpocząć tworzenie własnych ASP.NET MVC aplikacji!


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-9.md)
