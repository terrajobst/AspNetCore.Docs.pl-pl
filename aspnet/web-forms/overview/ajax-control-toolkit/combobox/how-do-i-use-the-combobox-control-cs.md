---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Jak używać kontrolki ComboBox? (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Pole kombi to formant ASP.NET AJAX, który łączy elastyczność pole tekstowe z listy opcji, z których użytkownicy mogą wybrać.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 07da594647c9b43bd31644aa8c7fedb256a12916
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839284"
---
<a name="how-do-i-use-the-combobox-control-c"></a>Jak używać kontrolki ComboBox? (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Pole kombi to formant ASP.NET AJAX, który łączy elastyczność pole tekstowe z listy opcji, z których użytkownicy mogą wybrać.


Celem tego samouczka jest wyjaśnić formantu Toolkit ComboBox kontrolka AJAX. Pola kombi działa jak połączenie między standard kontrolki ASP.NET DropDownList i formant pola tekstowego. Możesz wybrać z listy istniejących elementów lub wprowadzić nowy element.

Pola kombi jest podobne do rozszerzeń kontrolki autouzupełniania, ale formanty są używane w różnych scenariuszach. Rozszerzenie autouzupełniania wysyła zapytanie do usługi sieci web, aby uzyskać pasujące wpisy. ComboBox — formant, z kolei jest inicjowany z zbiór elementów. Za pomocą Autouzupełnianie ułatwia extender sensie podczas pracy z dużym zestawem danych (w milionach części samochodowych) podczas korzystania z formantu ComboBox sens, kiedy praca wprowadzaj w małej grupie danych (dziesiątek, jak części samochodowych).

## <a name="selecting-from-a-static-list-of-items"></a>Wybranie z listy statycznych elementów

Pozwól s uruchomić przy użyciu prostego przykładu użycia kontrolki ComboBox. Wyobraź sobie, chcesz wyświetlić listę statycznych elementów na liście rozwijanej. Jednak mają pozostanie otwarte, możliwość lista nie jest pełny. Chcesz umożliwić użytkownikowi wprowadź wartość niestandardową do listy.

Firma Microsoft ll Tworzenie nowej strony ASP.NET Web Forms i korzystanie z kontrolki ComboBox na stronie. Dodawanie nowej strony programu ASP.NET do projektu, a następnie przełączyć do widoku projektu.

Jeśli chcesz użyć kontrolki ComboBox na stronie formantu ScriptManager należy dodać do strony. Przeciągnij formantu ScriptManager from beneath na karcie rozszerzenia AJAX na powierzchni projektanta. Należy dodać formantu ScriptManager w górnej części strony; można dodać w bezpośrednio poniżej po stronie serwera otwierania &lt;formularza&gt; tagu.

Następnie przeciągnij formant pola kombi na stronę. W przyborniku z innych kontrolek zestawu narzędzi AJAX Control Toolkit i rozszerzeń (patrz rysunek 1) można znaleźć kontrolki ComboBox.


[![Prosty formularz do tworzenia wizytówkę](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Rysunek 01**: Wybieranie formantu ComboBox w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Firma Microsoft ll umożliwia wyświetlanie statyczną listę opcji kontrolki ComboBox. Użytkownik może wybrać określonego poziomu spiciness ich ds listę trzy opcje: łagodne, średni i gorąca (patrz rysunek 2).


[![Wybranie z listy statycznych elementów](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Rysunek 02**: wybranie z listy statycznych elementów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Istnieją dwa sposoby, że te opcje można dodawać do kontrolki ComboBox. Najpierw wybierz opcję zadania opcje edytowania, po umieszczeniu wskaźnika myszy nad kontrolką w widoku Projekt i otworzyć Edytor elementu (zobacz rysunek 3).


[![Edytowanie elementów ComboBox](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Rysunek 03**: elementy edycji pola kombi ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image6.png))


Drugą opcją jest dodanie listę elementów między otwierającym i zamykającym &lt;asp: ComboBox&gt; tagów w widoku źródła. Na stronie w ofercie 1 zawiera zaktualizowane pola kombi, która ma na liście elementów.

**Wyświetlanie listy 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Po otwarciu strony w ofercie 1 można wybrać jedną z istniejących opcji z pola kombi. Innymi słowy pola kombi działa jak formant DropDownList.

Jednak masz również możliwość wprowadzania nowych wybór (na przykład Super Spicy), który nie znajduje się w istniejącej listy. Dlatego pola kombi również działa jak formant TextBox.

Niezależnie od tego, czy wybrać istniejącą wstępnie element lub możesz wprowadzić niestandardowego elementu po przesłaniu formularza, wybór wyświetlany w kontrolce etykiety. Po przesłaniu formularza btnSubmit\_kliknij program obsługi wykonuje i aktualizuje etykietę (zobacz rysunek 4).


[![Wyświetlanie wybranego elementu](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Rysunek 04**: wyświetlanie wybranego elementu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image8.png))


Pola kombi obsługuje te same właściwości co kontrolki DropDownList pobierania wybranego elementu po przesłaniu formularza:

- SelectedItem.Text - Wyświetla wartość właściwości tekstu zaznaczonego elementu.
- SelectedItem.Value - Wyświetla wartość właściwości wartość zaznaczonego elementu lub wyświetla tekst wpisany w pola kombi.
- SelectedValue — taka sama jak SelectedItem.Value z tą różnicą, że ta właściwość umożliwia określenie domyślnego (początkową) wybranego elementu.

Jeśli wpiszesz niestandardowy wybór w ComboBox następnie niestandardowy wybór jest przypisany do właściwości SelectedItem.Text i SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Wybierając listę elementów z bazy danych

Możesz pobrać listę elementów, które wyświetla pola kombi z bazy danych. Na przykład do kontrolki SqlDataSource, kontrolka ObjectDataSource, pokud lub EntityDataSource można powiązać kontrolki ComboBox.

Wyobraź sobie, że chcesz wyświetlić listę filmów w ComboBox. Chcesz pobrać listę filmów z tabeli bazy danych filmów. Wykonaj następujące kroki:

1. Utwórz stronę o nazwie Movies.aspx
2. Dodawanie formantu ScriptManager do strony, przeciągając ScriptManager jest dostępna na karcie rozszerzenia AJAX w przyborniku na stronę.
3. Na stronie Dodaj kontrolki ComboBox, przeciągając pola kombi na stronie.
4. W widoku Projekt, umieść kursor nad formantu ComboBox, a następnie wybierz pozycję **wybierz źródło danych** zadań opcji (zobacz rysunek 5). Zostanie uruchomiony Kreator konfiguracji źródła danych.
5. W **vybrat Zdroj dat** kroku, wybierz pozycję &lt;nowe źródło danych&gt; opcji.
6. W **wybierz typ źródła danych** kroku, wybierz bazę danych.
7. W **wybierz połączenie danych** kroku, wybierz bazę danych (na przykład MoviesDB.mdf).
8. W **Zapisz parametry połączenia do pliku konfiguracji aplikacji** kroku, wybierz opcję, aby zapisać parametry połączenia.
9. W **skonfigurować instrukcji Select** krok, wybierz tabelę bazy danych filmów i zaznacz wszystkie kolumny.
10. W **Testovat dotaz** kroku, kliknij przycisk Zakończ.
11. Ponownie **wybierz źródło danych** kroku, wybierz tytuł kolumny dla pola, aby wyświetlić i kolumny identyfikatora dla danych pola (patrz rysunek).
12. Kliknij przycisk OK, aby zamknąć kreatora.


[![Wybieranie źródła danych](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Rysunek 05**: Wybieranie źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Wybieranie pól tekstu i wartości danych](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Rysunek 06**: Wybieranie pól tekstu i wartości danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Po wykonaniu powyższych kroków, pola kombi jest powiązany z kontrolką SqlDataSource reprezentujący filmy z tabeli bazy danych filmów. Źródło strony wyglądają jak lista 2 (I oczyszczone nieco formatowanie).

**Wyświetlanie listy 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Należy zauważyć, że kontrolka ComboBox ma właściwość DataSourceID, który wskazuje na użyciu kontrolki SqlDataSource. Po otwarciu strony w przeglądarce zostanie wyświetlona lista filmów z bazy danych (zobacz rysunek 7). Możesz Wybierz film z listy lub wprowadź nowy film wpisując film do pola kombi.


[![Wyświetlanie filmów](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Rysunek 07**: wyświetlanie filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Ustawienie DropDownStyle

Właściwość ComboBox DropDownStyle służy do zmiany zachowania kontrolki ComboBox. Ta właściwość akceptuje istnieje dopuszczalne wartości:

- Lista rozwijana — wyświetla pola kombi (wartość domyślna), listę rozwijaną listę po kliknięciu strzałki i można wprowadzić wartość niestandardową.
- Proste — pola kombi automatycznie wyświetla listę rozwijaną i można wprowadzić wartość niestandardową.
- Metody DropDownList — pola kombi działa jak formant DropDownList.

Różnic między listy rozwijanej i proste jest, gdy zostanie wyświetlona lista elementów. W przypadku prostego zostanie wyświetlona lista, natychmiast, gdy użytkownik przenosi fokus do pola kombi. W przypadku listy rozwijanej możesz kliknąć strzałkę, aby wyświetlić listę elementów.

Wartość kontrolki DropDownList powoduje, że działają podobnie jak standardowy kontrolki DropDownList formant pola kombi. Jednak jest to istotna różnica. Starsze wersje programu Internet Explorer wyświetlane kontrolki DropDownList przy użyciu nieskończonej indeks więc kontrolki pojawią się przed dowolną kontrolkę umieszczony przed jej. Ponieważ pola kombi powoduje wyświetlenie kodu HTML &lt;div&gt; znaczników zamiast kodu HTML &lt;wybierz&gt; tagu kontrolki ComboBox poprawnie szanuje kolejności z.

## <a name="setting-the-autocompletemode"></a>Ustawienie parametr AutoCompleteMode

Właściwość parametr ComboBox AutoCompleteMode umożliwia określenie, co się dzieje, gdy użytkownik wpisze tekst do kontrolki ComboBox. Ta właściwość akceptuje następujące możliwe wartości:

- Brak — (wartość domyślna) podane przez pola kombi nie każde zachowanie automatycznego uzupełniania.
- Zaproponuj — umożliwia wyświetlenie listy pola kombi i jego wyróżnia pasujący element na liście (patrz rysunek 8).
- Dołącz — pola kombi nie wyświetla listy i dołącza go pasujący element z listy na wpisany (patrz rysunek 9).
- SuggestAppend — pola kombi umożliwia wyświetlenie listy i dołącza pasujący element z listy na wpisany (zobacz rysunek 10).


[![Pola kombi sprawia, że sugestię](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Rysunek 08**: sugestię sprawia, że pole kombi ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![Pole kombi dołącza pasujący tekst](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Rysunek 09**: ComboBox dołącza pasujący tekst ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![Pola kombi sugeruje i dołącza](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Na rysunku nr 10**: ComboBox sugeruje i dołącza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób wyświetlania ustalony zestaw elementów przy użyciu kontrolki ComboBox. Firma Microsoft powiązane kontrolki ComboBox, zarówno zestaw elementów statycznych i tabelę bazy danych. Na koniec pokazaliśmy ci, jak zmodyfikować zachowanie pola kombi, ustawiając jej właściwości, a parametr AutoCompleteMode DropDownStyle.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-combobox-control-vb.md)
