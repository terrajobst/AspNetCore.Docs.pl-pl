---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Wprowadzenie do składnika ASP.NET Web Pages — usuwanie danych bazy danych | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku dowiesz się, jak usunąć wpis poszczególnych baz danych. Założono, że zostały wykonane serii za pośrednictwem aktualizacji bazy danych w sieci Web platformy ASP.NET Pa....
ms.author: aspnetcontent
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 234b5f99c5d5f580316204c88ea1ab8c1269d452
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815359"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Wprowadzenie do wzorca ASP.NET Web Pages — usuwanie danych bazy danych
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku dowiesz się, jak usunąć wpis poszczególnych baz danych. Przyjęto założenie, że zostały wykonane serii za pośrednictwem [aktualizowanie bazy danych w programie ASP.NET Web Pages](updating-data.md).
> 
> Zawartość:
> 
> - Jak wybrać pojedynczego rekordu z listy rekordów.
> - Jak usunąć pojedynczy rekord z bazy danych.
> - Jak sprawdzić, czy konkretny przycisk został kliknięty w formularzu.
>   
> 
> Funkcje/technologie omówione:
> 
> - `WebGrid` Pomocnika.
> - SQL `Delete` polecenia.
> - `Database.Execute` Metodę, aby uruchomić SQL `Delete` polecenia.


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

W poprzednim samouczku przedstawiono sposób aktualizacji istniejącego rekordu bazy danych. W tym samouczku jest podobny, z tą różnicą, że zamiast aktualizowania rekordu, zostaną one usunięte. Procesy są bardzo podobne do, z tą różnicą, że usunięcie jest prostsze, więc w tym samouczku będą krótki.

W *filmy* stronie będą aktualizowane `WebGrid` pomocnika, tak że wyświetla **Usuń** łącze obok każdego filmu, która ma towarzyszyć **Edytuj** łącze dodano wcześniej.

![Filmy strona, wyświetlająca łącze usuwanie dla każdego filmu](deleting-data/_static/image1.png)

Podobnie jak w przypadku edytowania, po kliknięciu **Usuń** łącza, spowoduje to przejście do innej strony, której informacje film jest już w postaci:

![Usuwanie strony filmu przy użyciu filmu wyświetlane](deleting-data/_static/image2.png)

Można następnie kliknij przycisk aby trwale usunąć rekord.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Dodawanie Usuń łącze do listy filmów

Użytkownik rozpoczyna się przez dodanie **Usuń** połączyć `WebGrid` pomocnika. Ten link jest podobny do **Edytuj** łącze, które zostały dodane w poprzednim samouczku.

Otwórz *Movies.cshtml* pliku.

Zmiana `WebGrid` znaczników w treści strony przez dodanie kolumny. Oto znaczniki zmodyfikowane:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Nowa kolumna jest to:

[!code-html[Main](deleting-data/samples/sample2.html)]

Sposób siatki jest skonfigurowany, **Edytuj** kolumna jest skrajnie po lewej stronie w siatce i **Usuń** kolumna znajduje się po prawej stronie. (Brak przecinka po `Year` teraz kolumny w przypadku, gdy nie można zauważyć, że.) Nic specjalnego nie o dokąd te kolumny łącza, a następnie równie łatwo można je umieścić obok siebie. W tym przypadku są one utrudnić pomieszała się oddzielnie.

![Strona filmy wraz z łączami do edycji i szczegóły oznaczone, aby pokazać, że są one nie obok siebie](deleting-data/_static/image3.png)

Nowa kolumna pokazuje łącze (`<a>` elementu) którego tekst jest wyświetlany komunikat "Delete". Miejsce docelowe łącza (jego `href` atrybutu) jest kod, który ostatecznie jest rozpoznawany jako podobny do tego adresu URL za pomocą `id` wartość dla każdego filmu różnią się od:

[!code-css[Main](deleting-data/samples/sample3.css)]

Ten link spowoduje wywołanie stronę o nazwie *DeleteMovie* i przekaż go identyfikator filmu wybrano.

W tym samouczku nie przejść do szczegółów dotyczących sposobu ten link jest tworzony, ponieważ jest niemal identyczny **Edytuj** link z poprzedniego samouczka ([aktualizowanie bazy danych w programie ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Tworzenie strony usuwania

Teraz możesz utworzyć stronę która będzie obiektem docelowym dla **Usuń** łącze w siatce.

> [!NOTE] 
> 
> **Ważne** technika polega na wybraniu rekordu do usunięcia, a następnie użyć osobnej stronie, a przycisk, aby upewnić się, proces jest bardzo ważne dla bezpieczeństwa. Jako przeczytane w poprzednich samouczkach, dzięki czemu *wszelkie* typu zmian do witryny sieci Web powinien *zawsze* odbywać się za pomocą formularza &mdash; oznacza to, za pomocą operację POST protokołu HTTP. Jeśli wprowadzono możliwość zmiany lokacji po prostu, klikając łącze, (tj. z użyciem operacji GET), osób może wysyłać żądania prostego do swojej witryny i usunąć swoje dane. Nawet przeszukiwarką aparat wyszukiwania, która jest indeksowania witryny przypadkowo można usunąć danych wystarczy poniższe linki.
> 
> Gdy aplikacja umożliwia użytkownikom modyfikowania rekordu, masz użytkownik widzi rekordu do edytowania mimo to. Jednak może być kuszące, aby pominąć ten krok w przypadku usuwania rekordu. Nie jednak pominąć ten krok. (To również przydatne w przypadku użytkowników zobaczyć rekord i upewnij się, że są usuwane rekord, który one przeznaczone.)
> 
> W kolejnych zestawie samouczków pokazano, jak dodać funkcję logowania, dzięki czemu użytkownik będzie musiał zalogować się przed usunięciem rekordu.


Utwórz stronę o nazwie *DeleteMovie.cshtml* i Zamień, co znajduje się w pliku następującym kodem:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Ten kod znaczników jest podobna *EditMovie* stron, chyba że zamiast używania pól tekstowych (`<input type="text">`), zawiera znaczników `<span>` elementów. Nie ma nic w tym miejscu można edytować. To wszystko, co należy zrobić, są wyświetlane szczegóły filmu, aby użytkownicy upewnić się, czy są usuwane prawo filmu.

Znaczniki już zawiera łącze, które umożliwia użytkownikowi, wróć do strony listy filmów.

Podobnie jak w *EditMovie* strony, identyfikator wybrany film jest przechowywany w ukrytym polu. (Jest przekazywany do strony w pierwszej kolejności wartość ciągu zapytania.) Brak `Html.ValidationSummary` wywołania, które będą wyświetlane błędy sprawdzania poprawności. W tym przypadku błąd może być, czy identyfikator filmu nie został przekazany do strony lub że identyfikator film jest nieprawidłowy. Taka sytuacja może wystąpić, jeśli ktoś uruchomiono tę stronę bez zaznaczania filmu w *filmy* strony.

Napis na przycisku jest **Usuwanie filmu**, a jej nazwa atrybutu jest ustawiona na `buttonDelete`. `name` Atrybut będzie używana w kodzie, wskaż przycisk, który formularz został przesłany.

Musisz napisać kod, 1) odczytać szczegóły filmu po wyświetleniu strony i (2) faktycznie usunąć film, gdy użytkownik kliknie przycisk.

## <a name="adding-code-to-read-a-single-movie"></a>Dodawanie kodu do odczytu pojedynczego filmu

W górnej części *DeleteMovie.cshtml* strony, należy dodać następujący blok kodu:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Ten kod znaczników jest taka sama jak odpowiedni kod w *EditMovie* strony. Ona pobiera identyfikator film z ciągu zapytania i używa Identyfikatora odczytać rekordu z bazy danych. Kod zawiera test weryfikacji (`IsInt()` i `row != null`) aby upewnić się, że identyfikator filmu przekazywany do strony jest nieprawidłowy.

Należy pamiętać o tym, ten kod uruchamiać tylko przy pierwszym uruchomieniu strony. Nie chcesz ponownie odczytywana nagrywanie filmu z bazy danych, gdy użytkownik kliknie **Usuwanie filmu** przycisku. W związku z tym, kod można odczytać filmu znajduje się wewnątrz test, który jest wyświetlany komunikat `if(!IsPost)` &mdash; czyli *Jeśli żądanie nie jest na operację post (przesyłania formularza)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Dodawanie kodu, aby usunąć wybrany film

Aby usunąć film, gdy użytkownik kliknie przycisk, Dodaj następujący kod wewnątrz zamykającym nawiasem klamrowym `@` bloku:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Ten kod jest podobny do kodu dotyczące aktualizacji istniejącego rekordu, ale jest prostsze. Kod działa na zasadzie SQL `Delete` instrukcji.

 Podobnie jak w *EditMovie* stronie kod znajduje się w `if(IsPost)` bloku. Tym razem `if()` warunek jest nieco bardziej skomplikowane: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Istnieją tutaj dwa warunki. Pierwsza to, że strona jest przekazywana, jak już wspomniano wcześniej &mdash; `if(IsPost)`.

Drugi warunek ma `!Request["buttonDelete"].IsEmpty()`, co oznacza, że żądanie ma obiekt o nazwie `buttonDelete`. Niewątpliwie jest to sposób pośredni testów, który przycisk formularz został przesłany. Jeśli formularz zawiera wiele przycisków przesyłania, tylko nazwę przycisku, który został kliknięty pojawi się w żądaniu. W związku z tym, logicznie Jeśli nazwa określonego przycisku pojawia się w żądaniu &mdash; lub opisany w kodzie, jeśli ten przycisk nie jest pusty &mdash; to przycisk, który formularz został przesłany.

`&&` Oznacza, że operator "i" (operator logiczny oraz). W związku z tym całą `if` warunek jest...

*To żądanie jest żądaniem post (nie żądanie po raz pierwszy)*  
  
 AND  
  
** `buttonDelete` *Przycisk został przycisku, który formularz został przesłany.*

Ten formularz (w rzeczywistości ta strona) zawiera tylko jeden przycisk, więc dodatkowy test na `buttonDelete` technicznie nie jest wymagane. Nadal którą zamierzasz wykonać operacji, która spowoduje to trwałe usunięcie danych. Dlatego należy się, jak to możliwe, że wykonujesz operację tylko wtedy, gdy użytkownik jawnie zgłosił żądanie. Załóżmy na przykład, możesz później rozszerzyć tę stronę i do niej dodać inny przycisk. Nawet, kod, który usuwa film zostanie uruchomione tylko wtedy, gdy `buttonDelete` kliknięcia przycisku.

Podobnie jak w *EditMovie* strony, możesz uzyskać identyfikator ukryte pola, a następnie uruchom polecenie SQL. Składnia `Delete` instrukcja jest:

`DELETE FROM table WHERE ID = value`

Należy koniecznie obejmują `WHERE` klauzuli i identyfikatora. Jeśli pozostawisz klauzuli WHERE *zostaną usunięte wszystkie rekordy w tabeli*. Jak wiesz już, przekażesz wartości Identyfikatora polecenia SQL przy użyciu symbolu zastępczego.

## <a name="testing-the-movie-delete-process"></a>Testowanie proces usuwania filmu

Teraz możesz przetestować. Uruchom *filmy* strony, a następnie kliknij przycisk **Usuń** obok filmu. Gdy *DeleteMovie* zostanie wyświetlona strona, kliknij przycisk **Usuwanie filmu**.

![Usuń stronę film z wyróżnionym przyciskiem Usuń filmu](deleting-data/_static/image4.png)

Po kliknięciu przycisku, kod usuwa filmy i zwraca listę filmu. Można wyszukać usunięte film i upewnij się, że jest ona zostać usunięta.

## <a name="coming-up-next"></a>Pojawi się dalej

Następny samouczek pokazuje, jak zapewnić wszystkich stron w witrynie, wspólnej układ i wygląd.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Kompletna lista strony filmu (aktualizowane wraz z łączami Delete)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Kompletna lista DeleteMovie strony

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET przy użyciu składni Razor](../introducing-razor-syntax-c.md)
- [Usuń instrukcję SQL w](http://www.w3schools.com/sql/sql_delete.asp) witrynie W3Schools

> [!div class="step-by-step"]
> [Poprzednie](updating-data.md)
> [dalej](layouts.md)
