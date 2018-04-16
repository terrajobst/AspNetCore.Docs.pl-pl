---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Wprowadzenie do składnika ASP.NET Web Pages — usuwanie danych z baz danych | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek pokazuje, jak można usunąć wpisu poszczególne bazy danych. Zakłada się, że zostały wykonane serii za pośrednictwem aktualizacji bazy danych w sieci Web ASP.NET Pa....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Wprowadzenie do strony sieci Web ASP.NET — usuwanie danych z baz danych
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek pokazuje, jak można usunąć wpisu poszczególne bazy danych. Przyjęto założenie, że zostały wykonane serii za pomocą [aktualizowania bazy danych w programie ASP.NET Web Pages](updating-data.md).
> 
> Zawartość:
> 
> - Jak wybrać pojedynczego rekordu z listy rekordów.
> - Jak usunąć pojedynczego rekordu z bazy danych.
> - Jak sprawdzić, czy określone przycisk został kliknięty w formularzu.
>   
> 
> Funkcje/technologie omówione:
> 
> - `WebGrid` Pomocnika.
> - SQL `Delete` polecenia.
> - `Database.Execute` Metodę, aby uruchomić SQL `Delete` polecenia.


## <a name="what-youll-build"></a>Jakie będzie kompilacji

W poprzednich samouczka przedstawiono sposób aktualizacji istniejącego rekordu bazy danych. W tym samouczku jest podobny, z wyjątkiem zamiast aktualizacja rekordu, zostaną one usunięte. Procesy są znacznie takie same, z wyjątkiem tego, czy usuwanie jest prostsze, dlatego w tym samouczku będzie krótki.

W *filmy* strony, będzie aktualizowana `WebGrid` Pomocnik, którego nie wyświetla **usunąć** link obok każdego film, aby dołączyć **Edytuj** łącze dodano wcześniej.

![Usunięcie połączenia dla każdego Film przedstawiający strony filmy](deleting-data/_static/image1.png)

Podobnie jak w przypadku edycji, po kliknięciu **usunąć** łącza, spowoduje to przejście do innej strony, gdy informacje filmu są już w postaci:

![Usuń stronę film z filmu wyświetlane](deleting-data/_static/image2.png)

Można następnie kliknij przycisk, aby trwale usunąć rekord.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Dodawanie Usuń łącze do tej listy filmów

Użytkownik rozpoczyna się przez dodanie **usunąć** połączyć `WebGrid` pomocnika. To łącze jest podobny do **Edytuj** łącze dodanym w poprzednim samouczka.

Otwórz *Movies.cshtml* pliku.

Zmień `WebGrid` znaczników w treści strony przez dodanie kolumny. Oto modyfikacji kod znaczników:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Nowa kolumna jest ta:

[!code-html[Main](deleting-data/samples/sample2.html)]

Sposób siatki jest skonfigurowany, **Edytuj** kolumna jest po lewej stronie siatki i **usunąć** kolumna jest po prawej stronie. (Brak przecinka po `Year` kolumny, w przypadku, gdy nie można zauważyć, że.) Nie ma specjalnych o gdzie te kolumny łącza, a można łatwo umieszczone obok siebie. W tym przypadku są oddzielone w celu zapewnienia ich trudniej uzyskać zamienione.

![Aby pokazać, że nie są one obok siebie oznaczona jako filmy strona zawierająca łącza edycji i szczegóły](deleting-data/_static/image3.png)

Nowej kolumny, która zawiera link (`<a>` element) którego tekst mówi, "Delete". Element docelowy łącza (jego `href` atrybut) jest kod, który ostatecznie jest rozpoznawany jako ekran podobny do tego adresu URL z `id` wartość różne dla każdego filmu:

[!code-css[Main](deleting-data/samples/sample3.css)]

To łącze wywoła stronę o nazwie *DeleteMovie* i przekaż go identyfikator filmu wybrano.

W tym samouczku nie przejdź do szczegółów dotyczących sposobu ten link jest tworzony, ponieważ jest niemal identyczny **Edytuj** łącza z poprzednich samouczek ([aktualizowania bazy danych w programie ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Tworzenie strony usuwania

Teraz można utworzyć strony, który ma być celu **usunąć** łącze w siatce.

> [!NOTE] 
> 
> **Ważne** technika polega na wybraniu rekordy do usunięcia, a następnie użyć osobnej stronie, a przycisk, aby potwierdzić proces jest niezwykle ważna dla bezpieczeństwa. Jako przeczytane w poprzednim samouczki, co *żadnych* typu zmian do witryny sieci Web powinien *zawsze* można zrobić za pomocą formularza sieci &mdash; oznacza to, za pomocą operacji POST protokołu HTTP. Jeśli wprowadzone możliwość zmiany lokacji po prostu, klikając łącze, (tj. z użyciem operacji GET), osób może prostego żądania do witryny i usuwać dane. Nawet przeszukiwarką aparat wyszukiwania, która jest indeksowania witryny przypadkowo można usunąć danych tylko za pomocą następujących łączy.
> 
> Gdy aplikacja umożliwia użytkownikom modyfikowania rekordu, masz użytkownik widzi rekordu do edycji mimo to. Jednak może się wydawać, aby pominąć ten krok w przypadku usuwania rekordu. Nie pomijaj choć tego kroku. (Jest również przydatne dla użytkowników zobaczyć rekord i upewnij się, że są one usuwanie rekordu, który przeznaczone.)
> 
> W kolejnych zestawu samouczek zobaczysz sposób dodawania funkcji logowania, użytkownik będzie musiał zalogować przed usunięciem rekordu.


Utwórz stronę o nazwie *DeleteMovie.cshtml* i Zastąp nowości w pliku o następujący kod:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Tego znacznika przypomina *EditMovie* stron, z wyjątkiem zamiast pól tekstowych (`<input type="text">`), zawiera kod znaczników `<span>` elementów. Nie ma w tym miejscu można edytować. Wystarczy, że będzie wyświetlane szczegóły film, dzięki czemu użytkownicy można upewnić się, że one usuwane film prawym.

Kod znaczników już zawiera łącze, które pozwala użytkownikowi na powrót do strony listy filmów.

Podobnie jak w *EditMovie* strony, identyfikator wybrany film jest przechowywany w ukrytym polu. (Jest przekazywany do strony w pierwszej kolejności wartość ciągu zapytania.) Brak `Html.ValidationSummary` wywołaniu, które będą wyświetlane błędy sprawdzania poprawności. W takim przypadku ten błąd może być czy identyfikator filmu nie został przekazany do strony lub że identyfikator film jest nieprawidłowy. Taka sytuacja może wystąpić, jeśli ktoś uruchomiono tę stronę bez zaznaczania filmu w *filmy* strony.

Podpis przycisku jest **usunąć film**, a jej nazwa atrybutu jest ustawiona na `buttonDelete`. `name` Atrybut będzie używany w kodzie, aby zidentyfikować przycisku formularz został przesłany.

Musisz napisać kod 1) odczytać szczegółów filmu po stronie jest najpierw wyświetlany i 2) rzeczywiście Usuń film, gdy użytkownik kliknie przycisk.

## <a name="adding-code-to-read-a-single-movie"></a>Dodawanie kodu do odczytu pojedynczego filmu

W górnej części *DeleteMovie.cshtml* Dodaj następujący blok kodu:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Ten kod znaczników jest taka sama jak odpowiedni kod w *EditMovie* strony. Pobiera identyfikator filmu poza ciąg zapytania, a używa Identyfikatora odczytać rekordu z bazy danych. Kod zawiera test weryfikacji (`IsInt()` i `row != null`) aby upewnić się, że identyfikator filmu przekazywany do strony jest nieprawidłowy.

Należy pamiętać, że ten kod należy uruchomić tylko przy pierwszym uruchomieniu strony. Użytkownik nie chce ponownie odczytywana filmu rekordu z bazy danych, gdy użytkownik kliknie **usunąć film** przycisku. W związku z tym kod, aby odczytać filmu znajduje się wewnątrz test, który wskazuje, że wskazuje `if(!IsPost)` &mdash; czyli *Jeśli żądanie nie jest operację post (przesyłania formularza)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Dodawanie kodu, aby usunąć wybrany film

Aby usunąć film, gdy użytkownik kliknie przycisk, Dodaj następujący kod wewnątrz zamykającym `@` bloku:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Ten kod jest podobny do kodu aktualizowanie istniejącego rekordu, ale prostsze. Kod działa na zasadzie SQL `Delete` instrukcji.

 Podobnie jak w *EditMovie* strony, kod znajduje się w `if(IsPost)` bloku. Tym razem `if()` warunek jest nieco bardziej skomplikowane: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Istnieją dwa warunki, w tym miejscu. Pierwsza to, że strona jest przesyłane, jak przedstawiono przed &mdash; `if(IsPost)`.

Drugi warunek jest `!Request["buttonDelete"].IsEmpty()`, co oznacza, że żądanie ma obiekt o nazwie `buttonDelete`. Niewątpliwie jest to pośredni sposób testów, który przycisk formularz został przesłany. Jeśli formularz zawiera wiele przycisków przesyłania, pojawi się tylko nazwa przycisku, który został kliknięty w żądaniu. W związku z tym, logicznie Jeśli nazwa określonego przycisku pojawia się w żądaniu &mdash; lub w już wspomniano w kodzie, jeśli to nie jest pusty &mdash; jest przycisk formularz został przesłany.

`&&` Oznacza, że operator "i" (logiczne i). W związku z tym całą `if` warunku...

*To żądanie jest żądaniem post (nie żądanie po raz pierwszy)*  
  
 AND  
  
** `buttonDelete` *Przycisk został przycisk formularz został przesłany.*

Ten formularz (w rzeczywistości ta strona) zawiera tylko jeden przycisk Tak dodatkowy test na `buttonDelete` nie technicznie jest wymagana. Nadal którą zamierzasz wykonać operację, która spowoduje trwałe usunięcie danych. Dlatego należy się, jak to możliwe, że wykonujesz operację tylko wtedy, gdy użytkownik jawnie zgłosił żądanie. Załóżmy na przykład, możesz później rozszerzyć tę stronę i dodane do niej inne przyciski. Nawet wówczas kod, który usuwa filmu zostanie uruchomiony tylko wtedy, gdy `buttonDelete` przycisk został kliknięty.

Podobnie jak w *EditMovie* strony, można pobrać Identyfikatora ukryte pola, a następnie uruchom polecenie SQL. Składnia `Delete` instrukcja jest:

`DELETE FROM table WHERE ID = value`

Należy koniecznie obejmują `WHERE` klauzuli i identyfikatora. Jeśli Opuść klauzuli WHERE *zostaną usunięte wszystkie rekordy w tabeli*. Jak już wspomniano, należy przekazać wartość Identyfikatora polecenia SQL, za pomocą symbolu zastępczego.

## <a name="testing-the-movie-delete-process"></a>Testowanie procesu usuwania film

Teraz możesz przetestować. Uruchom *filmy* , a następnie kliknij przycisk **usunąć** obok filmu. Gdy *DeleteMovie* zostanie wyświetlona strona, kliknij przycisk **usunąć film**.

![Usuń stronę film z przycisk Usuń film wyróżnione](deleting-data/_static/image4.png)

Po kliknięciu przycisku kod usuwa filmy i zwraca listę filmu. Można znaleźć usuniętego filmu i upewnij się, że jest został usunięty.

## <a name="coming-up-next"></a>Powtarzający się dalej

Następnym samouczku przedstawiono sposób zapewnić wszystkich stron w witrynie wspólnego wygląd i układu.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Pełna lista strony Movie (zaktualizowane z łączami Usuń)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Pełna lista DeleteMovie strony

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor](../introducing-razor-syntax-c.md)
- [Instrukcja DELETE SQL](http://www.w3schools.com/sql/sql_delete.asp) w witrynie W3Schools

> [!div class="step-by-step"]
> [Poprzednie](updating-data.md)
> [dalej](layouts.md)
