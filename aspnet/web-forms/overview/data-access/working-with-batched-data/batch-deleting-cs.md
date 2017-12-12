---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Wsadowe usuwania (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Dowiedz się, jak usunąć wiele rekordów bazy danych w ramach jednej operacji. Warstwę interfejsu użytkownika firma Microsoft rozbudowanie rozszerzonego widoku GridView utworzony we wcześniejszej tut..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 71c4b323c2604d9e775f75272bb9565505580522
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="batch-deleting-c"></a>Wsadowe usuwania (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) lub [pobierania plików PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Dowiedz się, jak usunąć wiele rekordów bazy danych w ramach jednej operacji. Warstwę interfejsu użytkownika firma Microsoft zależą rozszerzonego widoku GridView utworzonej w samouczku wcześniej. W warstwie dostępu do danych Firma Microsoft zawijać wielu operacji usuwania w ramach transakcji, aby upewnić się, że wszystkie operacje usuwania powiedzie się lub wycofać wszystkie operacje usuwania.


## <a name="introduction"></a>Wprowadzenie

[Poprzedniego samouczek](batch-updating-cs.md) przedstawione w sposób tworzenia partii edycji interfejs przy użyciu w pełni edytowalne widoku GridView. W sytuacjach, gdy użytkownicy często edytowania wielu rekordów jednocześnie partii edycji interfejsu będzie wymagać mniej ogłaszania zwrotnego i mysz klawiatury kontekstu przełączników, zwiększając wydajność s użytkownika końcowego. Ta metoda przydaje się podobnie do strony, gdzie jest często dla użytkowników usunąć wielu rekordów w jednym Przejdź.

Każdy użytkownik, który został użyty w kliencie poczty e-mail w trybie online jest już zapoznać się z jednym z najbardziej typowych partii usuwanie interfejsów: przycisk wyboru w każdym wierszu w siatce z odpowiedniego Usuń wszystkie zaznaczone elementy (zobacz rysunek 1). W tym samouczku jest raczej krótka ponieważ firma Microsoft kolejnych już przeprowadzone wszystkie Praca w poprzednim samouczkach w opracowaniu zarówno interfejs sieci web, jak i metody, można usunąć serii rekordów jako pojedynczej Atomowej operacji. W [dodanie kolumny widoku GridView pola wyboru](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) samouczek utworzyliśmy Element GridView z kolumną pola wyboru, w [zawijania modyfikacje bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md) utworzyliśmy metody w samouczku logiki warstwy Biznesowej, który użyje transakcji, aby usunąć `List<T>` z `ProductID` wartości. W tym samouczku nasz zależą od i scal naszych poprzednich środowiska do tworzenia partii pracy usuwanie przykład.


[![Każdy wiersz zawiera pole wyboru](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Rysunek 1**: każdy wiersz zawiera pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image2.png))


## <a name="step-1-creating-the-batch-deleting-interface"></a>Krok 1: Tworzenie partii Usuwanie interfejsu

Ponieważ utworzyliśmy Usuwanie interfejsu w partii [dodanie kolumny widoku GridView pola wyboru](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) samouczek, firma Microsoft może po prostu skopiuj go do `BatchDelete.aspx` zamiast tworzenia go od początku. Uruchamianie przez otwarcie `BatchDelete.aspx` strony `BatchData` folderu i `CheckBoxField.aspx` strony `EnhancedGridView` folderu. Z `CheckBoxField.aspx` strony, przejdź do widoku źródła i skopiuj kod znaczników między `<asp:Content>` tagów, jak pokazano na rysunku 2.


[![Skopiuj kod deklaratywne znaczników CheckBoxField.aspx do Schowka](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Rysunek 2**: kopiowanie deklaratywne oznakowanie `CheckBoxField.aspx` do Schowka ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image4.png))


Następnie przejdź do widoku źródła w `BatchDelete.aspx` i Wklej zawartość Schowka w `<asp:Content>` tagów. Skopiować i wkleić kod z klasy związane z kodem w `CheckBoxField.aspx.cs` do należące do klasy związane z kodem w `BatchDelete.aspx.cs` ( `DeleteSelectedProducts` przycisk s `Click` program obsługi zdarzeń, `ToggleCheckState` metody i `Click` procedury obsługi zdarzeń Aby uzyskać `CheckAll` i `UncheckAll` przycisków). Po skopiowaniu przez tę zawartość `BatchDelete.aspx` klasy związane z kodem strony s powinna zawierać następujący kod:


[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Po skopiowaniu na deklaratywne znaczników i kodu źródłowego, Poświęć chwilę, aby przetestować `BatchDelete.aspx` wyświetlając go za pośrednictwem przeglądarki. Powinien zostać wyświetlony element GridView wyświetlanie pierwszych dziesięciu produktów w widoku GridView z każdym wierszem listę nazw produktów s, kategorii i cen wraz z wyboru. Powinien być trzy przyciski: Zaznacz wszystko, usuń zaznaczenie wszystkich i usunąć wybrane produkty. Kliknięcie przycisku Sprawdź wszystkie wybiera wszystkie pola wyboru, gdy Usuń zaznaczenie wszystkich czyści wszystkie pola wyboru. Klikając polecenie Usuń wybrane produkty wyświetla komunikat, który zawiera listę `ProductID` wartości zaznaczonych produktów, ale faktycznie nie powoduje usunięcia produktów.


[![Interfejs z CheckBoxField.aspx został przeniesiony do BatchDeleting.aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Rysunek 3**: interfejs z `CheckBoxField.aspx` został przeniesiony do `BatchDeleting.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image6.png))


## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Krok 2: Usuwanie zaznaczonych produktów, przy użyciu transakcji

Z partii, usuwanie interfejsu pomyślnie kopiowane do `BatchDeleting.aspx`, wszystkie pozostaje ma zaktualizuj kod, aby przycisk Usuń wybrane produkty Usuwa zaznaczone produktów za pomocą `DeleteProductsWithTransaction` metody w `ProductsBLL` klasy. Ta metoda dodane w [zawijania modyfikacje bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md) samouczek, przyjmuje jako dane wejściowe `List<T>` z `ProductID` wartości i usuwa każdego odpowiadającego `ProductID` w zakresie transakcja.

`DeleteSelectedProducts` Przycisk s `Click` obsługi zdarzeń obecnie są używane następujące `foreach` pętli do iteracji każdego wiersza w widoku GridView:


[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Dla każdego wiersza `ProductSelector` programowo odwołuje się formant wyboru sieci Web. Jeśli jest ono zaznaczone, wiersz s `ProductID` są pobierane z `DataKeys` kolekcji i `DeleteResults` etykiety s `Text` właściwość została zaktualizowana do komunikat wskazujący, że wiersz został wybrany do usunięcia.

Powyższy kod rzeczywistości nie usuwa żadnych rekordów co wywołanie `ProductsBLL` klasy s `Delete` metoda są oznaczone jako komentarz. Zostały tę logikę delete ma zostać zastosowany, kod usunie produktów, ale nie w niepodzielną operację. Oznacza to jeśli pierwszy usuwa kilka w sekwencji zakończyło się pomyślnie, ale późniejszą nie powiodło się (np. z powodu naruszenia ograniczenia klucza obcego), może zostać zgłoszony wyjątek, ale pozostają usunięto tych produktów już usunięty.

Aby upewnić się, że niepodzielność, należy zamiast tego użyć `ProductsBLL` klasy s `DeleteProductsWithTransaction` metody. Ponieważ ta metoda przyjmuje listę `ProductID` wartości, należy najpierw skompilować tę listę z siatki i przekaż go jako parametr. Najpierw utworzymy wystąpienia `List<T>` typu `int`. W ramach `foreach` pętli musimy dodać wybrane produkty `ProductID` wartości tej `List<T>`. Po pętli to `List<T>` muszą być przekazywane do `ProductsBLL` klasy s `DeleteProductsWithTransaction` metody. Aktualizacja `DeleteSelectedProducts` przycisk s `Click` program obsługi zdarzeń z następującym kodem:


[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

Zaktualizowany kod tworzy `List<T>` typu `int` (`productIDsToDelete`) i wypełnia je z `ProductID` wartości do usunięcia. Po `foreach` pętli, jeśli istnieje co najmniej jeden produkt zaznaczone, `ProductsBLL` klasy s `DeleteProductsWithTransaction` metoda jest wywoływana i przekazany do tej listy. `DeleteResults` Jest również wyświetlana etykieta i dane odbitych do widoku GridView (tak, aby nowo usunięte rekordy nie występują jako wiersze w siatce).

Na rysunku 4 przedstawiono widoku GridView po wybrano liczbę wierszy do usunięcia. Rysunek 5. pokazuje ekranu natychmiast po kliknął przycisk Usuń zaznaczone produktów. Należy pamiętać, że na rysunku 5 `ProductID` wartości usunięte rekordy są wyświetlane w etykiecie poniżej widoku GridView i te wiersze nie są już w widoku GridView.


[![Wybrane produkty zostaną usunięte.](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Rysunek 4**: wybrane produkty zostaną usunięte ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image8.png))


[![Wartości usunięte ProductID produktów to wymienione poniżej widoku GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Rysunek 5**: produkty usunięte `ProductID` wartości to wymienione poniżej widok GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](batch-deleting-cs/_static/image10.png))


> [!NOTE]
> Aby przetestować `DeleteProductsWithTransaction` niepodzielność metody s ręcznie dodaj wpis dla produktu `Order Details` tabeli, a następnie spróbuj usunąć tego produktu (wraz z innymi). Naruszenie ograniczenia klucza obcego zostanie wyświetlony podczas próby usunięcia produktu z skojarzone zamówienia, ale należy zwrócić uwagę, jak inne Usuwanie zaznaczonych produktów są przywracane.


## <a name="summary"></a>Podsumowanie

Tworzenie partii Usuwanie interfejsu obejmuje Dodawanie widoku GridView z kolumną pola wyboru i Web przycisk kontrolować, po kliknięciu, Usuń wszystkie zaznaczone wiersze jako pojedynczej Atomowej operacji. W tym samouczku budujemy taki interfejs przez piecing razem pracować w dwóch poprzednich samouczki, [dodanie kolumny widoku GridView pola wyboru](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) i [zawijania modyfikacje bazy danych w ramach transakcji](wrapping-database-modifications-within-a-transaction-cs.md). W pierwszym samouczku utworzyliśmy Element GridView z kolumną pola wyboru, a w tym ostatnim zaimplementowano metody w logiki warstwy Biznesowej, po upływie `List<T>` z `ProductID` wartości, usunąć je w zakresie transakcji.

W następnym samouczku utworzymy interfejs służący do wykonywania operacji wstawiania wsadowego.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Hilton Giesenow i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Poprzednie](batch-updating-cs.md)
[dalej](batch-inserting-cs.md)
