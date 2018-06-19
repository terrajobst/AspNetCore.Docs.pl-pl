---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Część 8: Koszyka z aktualizacje interfejsu Ajax | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 8 obejmuje koszyk z aktualizacje interfejsu Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871288"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Część 8: Koszyku z aktualizacje interfejsu Ajax
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 8 obejmuje koszyk z aktualizacje interfejsu Ajax.


Firma Microsoft będzie umożliwiają użytkownikom umieścić albumów w ich koszyka bez rejestrowania, ale będzie trzeba zarejestrować jako gości, aby zakończyć proces realizacji transakcji. Proces kupowania i wyewidencjonowania spowoduje podzielić na dwa kontrolery: kontroler ShoppingCart, dzięki czemu anonimowo Dodawanie elementów do koszyka i kontrolera wyewidencjonowania, który obsługuje realizacji. Firma Microsoft będzie rozpoczynać się od koszyka, w tej sekcji, a następnie proces wyewidencjonowania w poniższej sekcji kompilacji.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Dodawanie klas modelu koszyka, kolejność i OrderDetail

Procesów naszego koszyka i wyewidencjonowania spowoduje, że korzystanie z niektórych nowych klas. Kliknij prawym przyciskiem myszy folder modeli i dodać klasę koszyka (Cart.cs) z następującym kodem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Ta klasa jest bardzo podobne do innych osób, możemy używany wykonanej do tej pory, z wyjątkiem atrybutu [klucz] właściwości RecordId. Elementy naszego koszyka będzie mieć identyfikator ciągu o nazwie CartID umożliwia anonimowego zakupów, ale w tabeli przedstawiono całkowitą klucza podstawowego o nazwie RecordId. Konwencja pierwszy kod Entity Framework oczekuje klucz podstawowy dla tabeli o nazwie koszyka będzie CartId lub identyfikator, że firma Microsoft może łatwo zastępuj za pomocą adnotacji lub kod Jeśli chcemy. To jest przykładowy sposób możemy użyć prostego konwencje w Entity Framework kod pierwszego po ich własnych nam, ale firma Microsoft jest nie ograniczone przez nich podczas nie.

Następnie Dodaj klasę zamówienia (Order.cs) z następującym kodem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Ta klasa śledzi informacje o kolejności Podsumowanie i dostarczania. **Nie będzie jeszcze skompilować**, ponieważ ma on właściwości nawigacji SzczegółyZamówień, która jest zależna od klasy nie utworzono jeszcze. Załóżmy ustalić, czy obecnie przez dodanie klasy o nazwie OrderDetail.cs, dodając następujący kod.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Wybierzemy jeden ostatniej aktualizacji do naszej klasy MusicStoreEntities uwzględnienie DbSets, który ujawnia te nowe klasy modelu, w tym również klasę DbSet&lt;wykonawcy&gt;. Zaktualizowano klasy MusicStoreEntities pojawia się jako poniżej.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Zarządzanie koszyku logika biznesowa

Następnie utworzymy klasy ShoppingCart w folderze modeli. ShoppingCart model obsługuje dostęp do danych do tabeli koszyka. Ponadto obsłuży logiki biznesowej na dodawanie i usuwanie elementów z koszyka zakupów.

Ponieważ nie chcemy wymagać od użytkowników zalogowania się do konta tak dodać elementy do ich koszyk, firma Microsoft będzie przypisywać użytkowników tymczasowego Unikatowy identyfikator (przy użyciu identyfikatora GUID lub Unikatowy identyfikator globalny) podczas uzyskiwania dostępu do koszyka. Ten identyfikator przy użyciu klasy sesji ASP.NET będzie przechowywane.

*Uwaga: Sesja programu ASP.NET jest wygodne miejsce do przechowywania informacji o użytkowniku, która wygaśnie po ich działania lokacji. Natomiast nadużycia stanu sesji może mieć wpływ na wydajność w witrynach większy, używanie firmę światła będzie działać również w celach demonstracyjnych.*

Klasa ShoppingCart udostępnia następujące metody:

**AddToCart** przyjmuje jako parametr albumu i dodaje go do koszyka użytkownika. Ponieważ w tabeli koszyka śledzi ilości dla każdego albumu, zawiera logikę do utworzenia nowego wiersza, w razie potrzeby lub po prostu zwiększyć ilość, jeśli użytkownik ma już uporządkowane jedną kopię albumu.

**RemoveFromCart** ma identyfikator albumu i usuwa go z koszyka użytkownika. Jeśli użytkownik ma tylko jedną kopię album na ich koszyka, wiersza zostanie usunięty.

**EmptyCart** usuwa wszystkie elementy z koszyka użytkownika.

**GetCartItems** pobiera listę CartItems wyświetlania lub przetwarzania.

**GetCount** pobiera całkowitą liczbę użytkownik ma w ich koszyku albumy.

**GetTotal** oblicza całkowity koszt wszystkich elementów w koszyka.

**CreateOrder** konwertuje koszyka kolejności w fazie realizacji transakcji.

**GetCart** jest metodą statyczną, która pozwala na uzyskanie obiektu koszyka naszych kontrolerów. Używa **GetCartId** metodę, aby obsłużyć odczyt CartId z sesji użytkownika. Metoda GetCartId wymaga element HttpContextBase tak, aby go przeczytać CartId użytkownika z sesji użytkownika.

Poniżej przedstawiono pełną **ShoppingCart klasy**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Kontrolera koszyka zakupów należy do komunikowania się niektórych złożonych informacji do jego widoków, które nie prawidłowo mapowania na naszych obiekty modelu. Nie chcemy zmodyfikować naszych modeli do własnych naszych widoków; Klasy modeli powinno reprezentować naszych domeny, a nie w interfejsie użytkownika. Jedno rozwiązanie byłoby przekazywania informacji do naszej widoków przy użyciu klasy obiekt ViewBag robiliśmy przy użyciu informacji z listy rozwijanej Menedżer magazynu, ale przekazywanie wiele informacji za pomocą elementów ViewBag pobiera trudne do zarządzania.

To rozwiązanie jest użycie *ViewModel* wzorca. Korzystając z tego wzorca utworzymy jednoznacznie klas, które są zoptymalizowane pod kątem scenariuszami określony widok, a które udostępniają właściwości dla zawartości dynamicznej, wartości/wymagane przez naszym szablony widoku. Nasze klasy kontrolera można wypełnić i przekazać te klasy zoptymalizowanych pod kątem widoku do naszej Wyświetl szablon do użycia. Umożliwia to zabezpieczenie typów, sprawdzanie kompilacji i Edytor IntelliSense w widoku szablonów.

Utwórz dwa modele widok do użycia w naszym kontrolera koszyk: ShoppingCartViewModel będzie przechowywać zawartość koszyk użytkownika i ShoppingCartRemoveViewModel będzie używany do wyświetlania informacji potwierdzenie, gdy użytkownik usuwa element z ich koszyka.

Teraz Utwórz nowy folder ViewModels w folderze głównym naszych projektu w celu zachowania rzeczy organizowane. Kliknij prawym przyciskiem myszy projekt, wybierz opcję Dodaj / nowego folderu.

![](mvc-music-store-part-8/_static/image1.jpg)

Nazwa folderu ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

Następnie Dodaj klasę ShoppingCartViewModel w folderze ViewModels. Ma dwie właściwości: lista elementów z koszyka, a wartość dziesiętną do przechowywania całkowitej cen dla wszystkich elementów w koszyka.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Teraz Dodaj ShoppingCartRemoveViewModel do folderu ViewModels z następujących czterech właściwości.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Kontroler koszyka zakupów

Kontroler koszyku ma trzy główne cele: Dodawanie elementów do koszyka, usuwanie elementów z koszyka i wyświetlania elementów w koszyka. Spowoduje to, że użycie trzech klas możemy właśnie utworzony: ShoppingCartViewModel, ShoppingCartRemoveViewModel i ShoppingCart. Jak StoreController i StoreManagerController dodamy pola do przechowywania wystąpienia MusicStoreEntities.

Dodaj nowy kontroler koszyku do projektu przy użyciu szablonu pusty kontroler.

![](mvc-music-store-part-8/_static/image2.png)

W tym miejscu jest pełną kontroler ShoppingCart. Indeks i Dodaj kontroler akcji powinna wyglądać znajomo bardzo. Akcji kontrolera Usuń i CartSummary obsługi dwóch szczególnych przypadkach, które omówiono w następnej sekcji.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aktualizacje interfejsu AJAX z jQuery

Następnie utworzymy strona indeksu koszyka zakupów, która jest silnie typizowaną do ShoppingCartViewModel i używa szablonu widoku listy przy użyciu tej samej metody co przed.

![](mvc-music-store-part-8/_static/image3.png)

Jednak zamiast Html.ActionLink usunięcie elementów z koszyka, użyjemy jQuery do "okablować się" zdarzenie click dla wszystkich łączy w tym widoku, których klasa HTML biznesowych — właścicieli. Zamiast przesyłanie formularza, ten program obsługi zdarzeń kliknij właśnie wprowadzi wywołania zwrotnego AJAX do naszej RemoveFromCart akcji kontrolera. RemoveFromCart zwraca wynik Zserializowany do postaci JSON, który naszych wywołania zwrotnego jQuery następnie analizuje i wykonuje cztery szybkie aktualizacje do strony przy użyciu technologii jQuery:

- 1. Usuwa albumu usuniętych z listy
- 2. Aktualizuje liczba koszyka w nagłówku
- 3. Wyświetla komunikat o aktualizacji dla użytkownika
- 4. Aktualizuje cena razem koszyka

Ponieważ scenariusz Usuń jest obsługiwane przez wywołania zwrotnego Ajax w widoku indeksu, potrzebujemy nie dodatkowy widok RemoveFromCart akcji. W tym miejscu jest kompletny kod dla widoku /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Aby przetestować tę możliwość, musimy można dodać elementy do naszego koszyka sklepowego. Będziemy informować naszych **szczegóły magazynu** widok ma zawierać przycisk "Dodaj do koszyka". Są nam go, firma Microsoft może zawierać części albumu dodatkowe informacje, które dodano od momentu ostatniej aktualizacji możemy ten widok: Genre, wykonawcy ceny i albumu. Zaktualizowany kod widoku Szczegóły magazynu pojawia się, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Teraz możemy kliknij za pośrednictwem sklepu i przetestować Dodawanie i usuwanie albumów do i z naszego koszyka sklepowego. Uruchom aplikację, a następnie przejdź do indeksu magazynu.

![](mvc-music-store-part-8/_static/image4.png)

Następnie kliknij Genre, aby wyświetlić listę albumów.

![](mvc-music-store-part-8/_static/image5.png)

Klikając tytuł teraz pokazuje naszych zaktualizowane widoku Szczegóły albumu, w tym przycisku "Dodaj do koszyka".

![](mvc-music-store-part-8/_static/image6.png)

Kliknięcie przycisku "Dodaj do koszyka" przedstawiono naszych indeksu koszyka zakupów z lista podsumowania koszyka zakupów.

![](mvc-music-store-part-8/_static/image7.png)

Po załadowaniu zapasowych koszyku, możesz kliknąć Usuń z koszyka łącza w celu wyświetlenia aktualizacji interfejsu Ajax do koszyka.

![](mvc-music-store-part-8/_static/image8.png)

Firma Microsoft powstanie limit działającego koszyka, dzięki czemu użytkownicy wyrejestrować dodać elementy do ich koszyka. W poniższej sekcji firma Microsoft będzie zezwolić im na rejestrowanie i ukończyć proces wyewidencjonowania.


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-7.md)
> [dalej](mvc-music-store-part-9.md)
