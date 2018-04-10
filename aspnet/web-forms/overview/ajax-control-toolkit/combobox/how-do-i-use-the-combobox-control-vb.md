---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Jak używać formantu ComboBox? (VB) | Microsoft Docs
author: microsoft
description: ComboBox jest formantem ASP.NET AJAX, łączącą elastyczność pole tekstowe z listy opcji, z których użytkownicy mogą wybrać.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e42844e326cb190502a51c5a85195b4752d7e827
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-use-the-combobox-control-vb"></a>Jak używać formantu ComboBox? (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> ComboBox jest formantem ASP.NET AJAX, łączącą elastyczność pole tekstowe z listy opcji, z których użytkownicy mogą wybrać.


Celem tego samouczka jest wyjaśnienie formantu Toolkit ComboBox kontroli AJAX. Element ComboBox działa jak połączenie między formantu standardowego ASP.NET DropDownList i kontrolki pola tekstowego. Możesz wybrać z listy już istniejących elementów lub wprowadź nowy element.

ComboBox przypomina Autouzupełnianie rozszerzeń formantu, ale kontrolki są używane w różnych scenariuszach. Rozszerzenie funkcji AutoComplete wysyła zapytanie do usługi sieci web, aby uzyskać zgodnych wpisów. ComboBox — formant, natomiast jest inicjowany z zbiór elementów. Przy użyciu ułatwia rozszerzeń autouzupełniania znaczeniu podczas pracy z dużych zestawów danych (miliony części samochód) przy użyciu formantu ComboBox sens podczas pracy z niewielki zestaw danych (dziesiątki części samochodów).

## <a name="selecting-from-a-static-list-of-items"></a>Wybierając z listy elementów statycznych

Let s rozpoczynać prosty przykład za pomocą kontrolki ComboBox. Załóżmy, że chcesz wyświetlić listę statycznych elementów na liście rozwijanej. Jednak chcesz pozostaw otwarte możliwości lista nie jest ukończone. Chcesz zezwolić użytkownikowi na wprowadzenie wartości niestandardowych do listy.

Firma Microsoft ll tworzenia nowej strony formularzy sieci Web ASP.NET i używać formantu ComboBox na stronie. Dodaj nową stronę ASP.NET do projektu i przejdź do widoku projektu.

Jeśli chcesz używać formantu ComboBox na stronie formantu ScriptManager należy dodać do strony. Przeciągnij formantu ScriptManager from beneath karta rozszerzenia AJAX na powierzchnię projektanta. Należy dodać formantu ScriptManager w górnej części strony; można dodać go bezpośrednio poniżej serwerowe otwierania &lt;formularza&gt; tagu.

Następnie przeciągnij kontrolki ComboBox na stronie. ComboBox — formant można znaleźć w przybornika formantów AJAX kontroli narzędzi i innych rozszerzeń formantu (zobacz rysunek 1).


[![Prosty formularz służący do tworzenia kart biznesowej](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Rysunek 01**: Wybieranie ComboBox — formant z przybornika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image2.png))


Firma Microsoft ll umożliwia wyświetlanie listy statycznej opcji kontrolki ComboBox. Użytkownik może wybrać określonym poziomie spiciness ich ds spośród trzech opcji: łagodne, średni i gorąca (patrz rysunek 2).


[![Wybierając z listy elementów statycznych](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Rysunek 02**: Wybieranie z listy statycznych elementów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image4.png))


Istnieją dwa sposoby, że te opcje można dodać do kontrolki ComboBox. Najpierw wybierz opcję zadanie Edytuj opcje, jeśli ustawiając kursor myszy nad formantem w widoku projektu i Otwórz Edytor elementu (patrz rysunek 3).


[![Edytowanie elementów ComboBox](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Rysunek 03**: elementy edycji ComboBox ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image6.png))


Drugą opcją jest dodanie listę elementów Between otwarcia i zamknięcia &lt;asp: ComboBox&gt; tagów w widoku źródła. Strona wyświetlania 1 zawiera zaktualizowane ComboBox, który ma na liście elementów.

**Wyświetlanie listy 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Po otwarciu strony w wyświetlania 1 można wybrać jedną z opcji istniejące z kontrolki ComboBox. Innymi słowy ComboBox działa jak lista DropDownList formantu.

Jednak masz również możliwość wprowadzania nową opcję (na przykład Super Spicy), który nie znajduje się w istniejącej listy. Dlatego ComboBox również działa jak kontrolki pola tekstowego.

Niezależnie od tego, czy wybrano istniejących wcześniej element lub wprowadź niestandardowego elementu, po przesłaniu formularza, wybrana opcja zostanie wyświetlona w formancie etykiety. Po przesłaniu formularza, btnSubmit\_kliknij obsługi wykonuje i aktualizuje etykiety (patrz rysunek 4).


[![Wyświetlanie wybranego elementu](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Rysunek 04**: wyświetlanie wybranego elementu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image8.png))


Element ComboBox obsługuje te same właściwości jako formant DropDownList pobierania wybranego elementu po przesłaniu formularza:

- SelectedItem.Text - Wyświetla wartość właściwości Text wybranego elementu.
- SelectedItem.Value - Wyświetla wartość właściwości wartość wybranego elementu lub wyświetla tekst wpisane w polu kombi.
- SelectedValue - taki sam jak SelectedItem.Value z tą różnicą, że ta właściwość umożliwia określenie domyślnej (początkową) wybranego elementu.

W przypadku wpisania niestandardowych wyborów w ComboBox następnie niestandardowy wybór jest przypisany do właściwości SelectedItem.Text i SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Wybieranie listę elementów z bazy danych

Lista elementów, które wyświetla ComboBox można pobrać z bazy danych. Na przykład do formantu SqlDataSource kontrolki ObjectDataSource, LinqDataSource albo obiektu EntityDataSource można powiązać ComboBox.

Załóżmy, że chcesz wyświetlić listę filmów w ComboBox. Chcesz pobrać listy filmów z filmów tabeli bazy danych. Wykonaj następujące kroki:

1. Utwórz stronę o nazwie Movies.aspx
2. Dodawanie formantu ScriptManager na stronie przeciągając ScriptManager dostępna karta rozszerzenia AJAX w przyborniku na stronę.
3. Dodawanie formantu ComboBox do strony przeciągając ComboBox na stronie.
4. W widoku Projekt, umieść kursor nad formantu ComboBox, a następnie wybierz **wybierz źródło danych** zadań opcji (zobacz rysunek 5). Kreator konfiguracji źródła danych jest uruchomiona.
5. W **wybierz źródło danych** krok, wybierz opcję &lt;nowe źródło danych&gt; opcji.
6. W **wybierz typ źródła danych** kroku, wybierz bazę danych.
7. W **wybierz połączenie danych** kroku, wybierz bazę danych (na przykład MoviesDB.mdf).
8. W **zapisać parametry połączenia do pliku konfiguracji aplikacji** kroku, wybierz opcję, aby zapisać parametry połączenia.
9. W **Konfiguruj instrukcję Select** krok, wybierz filmy tabeli bazy danych i wybierz wszystkie kolumny.
10. W **zapytania testu** kroku, kliknij przycisk Zakończ.
11. W **wybierz źródło danych** krok, wybierz kolumnę tytułu pola do wyświetlenia i danych w kolumnie Identyfikator pola (patrz rysunek).
12. Kliknij przycisk OK, aby zamknąć kreatora.


[![Wybieranie źródła danych](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Rysunek 05**: Wybieranie źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![Wybieranie pól tekstowych i wartości danych](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Rysunek 06**: Wybieranie pól danych tekst i wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image12.png))


Po wykonaniu powyższych czynności ComboBox jest powiązany z SqlDataSource formant, który reprezentuje filmów, filmów tabeli bazy danych. Źródło strony wygląda jak lista 2 (I wyczyścić nieco formatowanie).

**Wyświetlanie listy 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Zwróć uwagę, że kontrolki ComboBox ma właściwość DataSourceID, która wskazuje kontroli SqlDataSource. Po otwarciu strony w przeglądarce zostanie wyświetlona lista filmów z bazy danych (patrz rysunek 7). Umożliwia pobranie film z listy lub wprowadź nowy filmu wpisując filmu do kontrolki ComboBox.


[![Wyświetlanie listy filmów](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Rysunek 07**: wyświetlanie listy filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Ustawienie parametr DropDownStyle

Właściwości ComboBox DropDownStyle służy do zmiany zachowania kontrolki ComboBox. Ta właściwość akceptuje możliwe wartości:

- Lista rozwijana - Wyświetla ComboBox (wartość domyślna), listy rozwijanej liście podczas kliknij strzałkę, a można wprowadzić wartość niestandardowych.
- Prosty — ComboBox automatycznie wyświetla listę rozwijaną i będzie można wprowadzić wartość niestandardową.
- DropDownList - ComboBox działa jak lista DropDownList formantu.

Różnic między listy rozwijanej i proste jest, gdy zostanie wyświetlona lista elementów. W przypadku prostego zostanie wyświetlona lista natychmiast po przeniesieniu fokus na element ComboBox. W przypadku listy rozwijanej musi kliknij strzałkę, aby wyświetlić listę elementów.

Wartość DropDownList powoduje, że działają podobnie jak formantu standardowego DropDownList kontrolki ComboBox. Istnieje jednak istotną różnicą. Starsze wersje programu Internet Explorer wyświetlanie kontroli DropDownList z nieskończone indeks, więc formantu pojawią się na początku każdego formantu umieszczony przed nim. Ponieważ element ComboBox renderowania kodu HTML &lt;div&gt; tag zamiast kodu HTML &lt;wybierz&gt; tagu ComboBox poprawnie szanuje porządku osi.

## <a name="setting-the-autocompletemode"></a>Ustawienie parametr AutoCompleteMode

Właściwość parametr ComboBox AutoCompleteMode umożliwia określenie, co się dzieje, gdy użytkownik wpisze tekst w polu kombi. Ta właściwość akceptuje następujące możliwe wartości:

- None — (wartość domyślna) ComboBox nie zapewnia żadnych zachowanie automatycznego zakończenia.
- Sugeruj - ComboBox umożliwia wyświetlenie listy i zawiera opis pasującego elementu na liście (patrz rysunek 8).
- Append — ComboBox nie wyświetla listy i dołącza go pasującego elementu z listy na wpisany (patrz rysunek 9).
- SuggestAppend - ComboBox umożliwia wyświetlenie listy i dołącza pasującego elementu z listy na wpisany (zobacz rysunek 10).


[![Sugestia sprawia, że element ComboBox](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Rysunek 08**: ComboBox sprawia, że sugestię ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![ComboBox dołącza szukanego tekstu](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Rysunek 09**: ComboBox dołącza szukanego tekstu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![Element ComboBox sugeruje i dołącza](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Na rysunku nr 10**: ComboBox sugeruje i dołącza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób umożliwia wyświetlanie ustalony zbiór elementów kontrolki ComboBox. Firma powiązana formantu ComboBox zarówno do statycznego zestawu elementów i tabeli bazy danych. Ponadto przedstawiono sposób zmodyfikować zachowanie ComboBox przez ustawienie właściwości parametr DropDownStyle, a parametr AutoCompleteMode.

> [!div class="step-by-step"]
> [Poprzednie](how-do-i-use-the-combobox-control-cs.md)
