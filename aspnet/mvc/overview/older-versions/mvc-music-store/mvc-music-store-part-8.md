---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Część 8: Koszyk z aktualizacjami Ajax | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 8 obejmuje koszyk z aktualizacjami Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369799"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Część 8: Koszyk z aktualizacjami Ajax
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 8 obejmuje koszyk z aktualizacjami Ajax.


Firma Microsoft będzie Zezwalaj użytkownikom na umieszczenie albumów w ich koszyka bez rejestrowania, ale należy zarejestrować się jako goście zakończyć proces realizacji transakcji. Proces zakupów i wyewidencjonowywania będą można podzielić na dwa kontrolery: kontroler ShoppingCart, co pozwala anonimowo Dodawanie elementów do koszyka i kontroler wyewidencjonowania, która obsługuje rozpoczęcie procesu realizowania zamówienia. Firma Microsoft będzie rozpoczynać koszyka w tej sekcji, a następnie tworzenie rozpoczęcie procesu realizowania zamówienia w poniższej sekcji.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Dodawanie klasy modelu koszyka, kolejność i OrderDetail

Nasze procesów koszyk i wyewidencjonowania spowoduje, że korzystanie z niektórych nowych klas. Kliknij prawym przyciskiem myszy folderu modeli i dodać klasę koszyka (Cart.cs) z następującym kodem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Ta klasa jest bardzo podobne do innych osób, które była używana do tej pory, z wyjątkiem atrybutu [klucz] dla właściwości identyfikator rekordu. Elementy naszego koszyka ma identyfikator ciągu o nazwie CartID, aby zezwolić na anonimowy zakupów, ale tabela zawiera klucz podstawowy liczbą całkowitą o nazwie identyfikator rekordu. Zgodnie z Konwencją najpierw z programem Entity Framework kod oczekuje klucz podstawowy dla tabeli o nazwie koszyka będzie CartId lub Identyfikatora, że firma Microsoft można łatwo zastąpić, za pośrednictwem adnotacje lub kod Jeśli chcemy. Jest to przykład jak możemy użyć konwencji proste w Entity Framework najpierw kod po ich własnych nam, ale firma Microsoft jest nie ograniczona przez nich Jeśli tak nie jest.

Następnie Dodaj klasę zamówienia (Order.cs) z następującym kodem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Ta klasa śledzi informacje o kolejności Podsumowanie i dostarczania. **Nie będzie jeszcze skompilować**, ponieważ ma ona właściwość nawigacji OrderDetails, która zależy od klasy, firma Microsoft nie został jeszcze utworzony. Teraz ustalić, że teraz, dodając klasę o nazwie OrderDetail.cs, dodając następujący kod.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Wybierzemy jedną Ostatnia aktualizacja do klasy naszych MusicStoreEntities w taki sposób, aby uwzględnić DbSets, który udostępnić te nowe klasy modelu dotyczy to również DbSet&lt;wykonawcy&gt;. Zaktualizowano klasa MusicStoreEntities jest wyświetlany jako poniżej.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Zarządzanie logikę biznesową koszyka

Następnie utworzymy klasy ShoppingCart w folderze modeli. ShoppingCart model obsługuje dostęp do danych do tabeli koszyka. Ponadto obsłuży logikę biznesową w celu dodawania i usuwania elementów z koszyka.

Ponieważ nie chcemy wymagać od użytkowników założyć konto tak dodać elementy do ich koszyka, przypiszemy użytkowników tymczasowe Unikatowy identyfikator (przy użyciu identyfikatora GUID lub Unikatowy identyfikator globalny) podczas uzyskiwania dostępu do koszyka. Przechowujemy będzie ten identyfikator przy użyciu klasy sesji programu ASP.NET.

*Uwaga: Sesja programu ASP.NET jest wygodne miejsce do przechowywania informacji o użytkowniku, która wygaśnie po opuszczania witryny. Gdy nieuprawnione użycie stanu sesji mogą mieć wpływ na wydajność w witrynach większe, nasze światła użycia będzie działać również w celach demonstracyjnych.*

Klasa ShoppingCart udostępnia następujące metody:

**AddToCart** albumu jako parametr przyjmuje i dodaje go do koszyka użytkownika. Ponieważ w tabeli koszyka śledzi ilości dla każdego albumu, zawiera logikę w celu utworzenia nowego wiersza, jeśli to konieczne lub po prostu zwiększyć ilość, jeśli użytkownik ma już uporządkowane jedną kopię albumu.

**RemoveFromCart** przyjmuje identyfikator albumu i usuwa go z koszyka użytkownika. Jeśli użytkownik ma tylko jedną kopię album w ich koszyka, wiersza są usuwane.

**EmptyCart** spowoduje usunięcie wszystkich elementów z koszyka użytkownika.

**GetCartItems** umożliwia pobranie listy CartItems w celu wyświetlania lub przetwarzania.

**GetCount** pobiera całkowita liczba albumów użytkownika powoduje ich koszyk sklepowy.

**GetTotal** oblicza łączny koszt wszystkich elementów w koszyku.

**CreateOrder** konwertuje koszyka zamówienia w fazie wyewidencjonowania.

**GetCart** jest metoda statyczna, co pozwala naszym kontrolerów, aby uzyskać obiekt koszyka. Używa ona **GetCartId** metody, aby obsłużyć odczyt CartId z sesji użytkownika. Metoda GetCartId wymaga element HttpContextBase przeczytasz CartId użytkownika z sesji użytkownika.

Poniżej przedstawiono pełne **klasy ShoppingCart**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>Modele widoków

Nasze kontroler koszyka zakupów należy do komunikowania się niektórych złożone informacje do jego widoków, który nie jest mapowany nie pozostawia żadnych śladów naszych obiekty modelu. Nie chcemy zmodyfikować naszych modeli do potrzeb naszych widoków; Klasy modeli powinny reprezentować naszej domenie nie interfejs użytkownika. Jedno rozwiązanie będzie mógł przekazać informacje do naszych widoków przy użyciu klasy obiekt ViewBag postępowanie z informacje listy rozwijanej Menedżera Store, ale przekazywanie dużej ilości informacji za pomocą elementów ViewBag pobiera trudny do zarządzania.

To rozwiązanie jest użycie *ViewModel* wzorca. Korzystając z tego wzorca, możemy utworzyć silnie typizowanych klas, które są zoptymalizowane pod kątem scenariuszy określonego widoku, a które udostępnianie właściwości dla zawartości dynamicznej, wartości/wymagane przez naszych szablonów widoku. Naszych zajęć kontrolera można wypełnić i przekazać te zoptymalizowane pod kątem widoku klasy do naszych Wyświetl szablon do użycia. Dzięki temu, bezpieczeństwo typów, sprawdzanie w czasie kompilacji i Edytor technologii IntelliSense w szablonach widoku.

Utworzymy dwa modele widok do użycia w naszym kontroler koszyka: ShoppingCartViewModel będzie przechowywać zawartość koszyka użytkownika i ShoppingCartRemoveViewModel będzie służyć do wyświetlania informacji potwierdzenie, gdy użytkownik usunie coś z ich koszyka.

Utwórz nowy folder modele widoków w folderze głównym nasz projekt, aby zachować zorganizowane. Kliknij prawym przyciskiem myszy projekt, wybierz opcję Dodaj / nowy Folder.

![](mvc-music-store-part-8/_static/image1.jpg)

Nazwa folderu modele widoków.

![](mvc-music-store-part-8/_static/image1.png)

Następnie Dodaj klasę ShoppingCartViewModel w folderze modele widoków. Posiada dwie właściwości: lista elementów z koszyka, a wartość dziesiętną, aby pomieścić całkowita cena dla wszystkich elementów w koszyku.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Teraz Dodaj ShoppingCartRemoveViewModel do folderu modele widoków z następującymi właściwościami cztery.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Kontroler koszyka

Kontroler koszyka ma trzy główne funkcje: Dodawanie elementów do koszyka, usunięcie elementów z koszyka i wyświetlania elementów w koszyku. Użyj trzech klas, firma Microsoft wprowadzi właśnie utworzony: ShoppingCartViewModel ShoppingCartRemoveViewModel i ShoppingCart. Tak jak w przypadku StoreController i StoreManagerController dodasz pole do przechowywania wystąpienie MusicStoreEntities.

Dodaj nowy kontroler koszyka do projektu przy użyciu szablonu pusty kontroler.

![](mvc-music-store-part-8/_static/image2.png)

Oto kompletny kontrolera ShoppingCart. Akcje indeksu i Dodaj kontroler powinien wyglądać znajomo. Akcji kontrolera Remove i CartSummary obsługiwać dwa przypadki specjalne, które omówimy w poniższej sekcji.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aktualizacje interfejsu AJAX przy użyciu jQuery

Następnie utworzymy strony indeksu koszyka zakupów, zdecydowanie jest wpisane w ShoppingCartViewModel, która używa tego szablonu w widoku listy przy użyciu tej samej metody przed.

![](mvc-music-store-part-8/_static/image3.png)

Jednak zamiast Html.ActionLink do usuwanie elementów z koszyka, użyjemy jQuery do "Podłączanie" Zdarzenie kliknięcia dla wszystkich łączy w tym widoku, które mają klas kodu HTML biznesowych — właścicieli. Zamiast Publikowanie formularza, ta procedura obsługi zdarzeń kliknij właśnie wprowadzi wywołania zwrotnego AJAX naszej RemoveFromCart akcji kontrolera. RemoveFromCart zwraca wynik Zserializowany do ciągu JSON, które naszym wywołania zwrotnego jQuery następnie analizuje i wykonuje czterech szybkich aktualizacji do strony, przy użyciu jQuery:

- 1. Usuwa albumu usuniętych z listy
- 2. Aktualizuje liczba koszyka w nagłówku
- 3. Wyświetla komunikat o aktualizacji dla użytkownika
- 4. Aktualizuje cena łączna koszyka

Ponieważ scenariusz Usuń jest obsługiwane przez wywołanie zwrotne Ajax w widoku indeksu, nie potrzebujemy dodatkowy widok RemoveFromCart akcji. Oto kompletny kod dla widoku /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Aby przetestować tę możliwość, musimy mieć możliwość dodawania elementów do naszego koszyka. Zaktualizujemy naszych **szczegóły Store** widok ma zawierać przycisk "Dodaj do koszyka". Są nam na tym, firma Microsoft może obejmować niektóre spośród albumu dodatkowe informacje, które dodaliśmy od czasu ostatniej aktualizacji firma w tym widoku: gatunku, wykonawcy, ceny i albumów. Zaktualizowany kod widoku szczegółów Store pojawi się, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Teraz możemy kliknij za pośrednictwem sklepu i przetestować, dodając i usuwając ze zdjęciami do i z naszego koszyka. Uruchom aplikację, a następnie przejdź do indeksu Store.

![](mvc-music-store-part-8/_static/image4.png)

Następnie kliknij na określonego rodzaju, aby wyświetlić listę albumów.

![](mvc-music-store-part-8/_static/image5.png)

Teraz kliknięcie tytuł pokazuje nasz zaktualizowane widoku Szczegóły albumu, w tym przycisk "Dodaj do koszyka".

![](mvc-music-store-part-8/_static/image6.png)

Kliknięcie przycisku "Dodaj do koszyka" przedstawiono naszych koszyk sklepowy indeksu za pomocą lista podsumowania koszyka zakupów.

![](mvc-music-store-part-8/_static/image7.png)

Po załadowaniu usługi koszyka, możesz kliknąć Usuń z koszyka łącza w celu wyświetlenia aktualizacji interfejsu Ajax do koszyka.

![](mvc-music-store-part-8/_static/image8.png)

Utworzyliśmy się działającego koszyk, co pozwala niezarejestrowanym użytkownikom dodać elementy do ich koszyka. W poniższej sekcji firma Microsoft będzie zezwolić im na rejestrowanie i ukończenie procesu realizowania zamówienia.


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-7.md)
> [dalej](mvc-music-store-part-9.md)
