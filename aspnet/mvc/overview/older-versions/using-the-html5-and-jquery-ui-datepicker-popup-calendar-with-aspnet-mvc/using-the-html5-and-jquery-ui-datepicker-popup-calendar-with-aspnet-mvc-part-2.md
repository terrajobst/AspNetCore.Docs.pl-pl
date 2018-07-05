---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 2 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 2818e69f912509a6392333bad8f5a1afa55884f6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375292"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 z ASP.NET MVC — część 2
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z edytora szablonów, szablony i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web platformy ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Dodanie automatycznego szablonu daty/godziny

W pierwszej części tego samouczka pokazano sposób dodawania atrybutów do modelu, aby jawnie określić formatowanie i jak możesz jawnie określić szablon, który zostanie użyty do wyświetlenia modelu. Na przykład [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu w następujących jawnie kodu Określa formatowanie `ReleaseDate` właściwości.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

W poniższym przykładzie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu, za pomocą `Date` wyliczenia, określa, że szablon dat powinny być używane do wyświetlenia modelu. Jeśli nie ma daty szablonu w projekcie, jest używany szablon wbudowanych daty.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Jednak ASP. MVC można wykonać dopasowanie typu przy użyciu konwencji over konfiguracji przez wyszukiwanie szablonu, który jest zgodna z nazwą typu. Dzięki temu można utworzyć szablon, który automatycznie formatuje dane bez użycia żadnych atrybutów lub kodu w ogóle. W tej części samouczka utworzysz szablon, który jest automatycznie stosowany do właściwości w modelu typu [daty/godziny](https://msdn.microsoft.com/library/system.datetime.aspx). Nie trzeba używać atrybutu lub inna konfiguracja Aby określić, że szablon powinien być używany do renderowania wszystkie właściwości modelu typu [daty/godziny](https://msdn.microsoft.com/library/system.datetime.aspx).

Poznasz również sposób, aby dostosować wyświetlanie indywidualne właściwości lub nawet poszczególnych pól.

Aby rozpocząć, możemy usunąć istniejące informacje o formatowaniu i wyświetlanie pełne dat w aplikacji.

Otwórz *Movie.cs* pliku i Skomentuj `DataType` atrybutu na `ReleaseDate` właściwości:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Należy zauważyć, że `ReleaseDate` właściwość wyświetli teraz daty i godziny, ponieważ jest to wartość domyślna, gdy została podana nie informacje o formatowaniu.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Dodawanie style CSS do testowania nowych szablonów

Przed utworzeniem szablonu do formatowania dat, należy dodać kilka reguły stylów CSS, które można stosować do nowych szablonów. Funkcje te pomogą możesz sprawdzić, czy nowy szablon używa renderowanej strony.

Otwórz *Content\Site.cs*s pliku i dodaj następujące reguły CSS do dolnej części pliku:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Dodawanie szablonów wyświetlania daty/godziny

Teraz możesz utworzyć nowy szablon. W *Views\Movies* folderze utwórz *DisplayTemplates* folderu.

W *Views\Shared* folderze utwórz *DisplayTemplates* folder i *EditorTemplates* folderu.

Szablony ekranu w *folder Views\Shared\DisplayTemplates* folder będzie używana przez wszystkich kontrolerów. Szablony ekranu w *Views\Movie\DisplayTemplates* folder będzie używany tylko przez `Movie` kontrolera. (Jeśli szablon o tej samej nazwie pojawia się w obu folderów szablonu w *Views\Movie\DisplayTemplates* folderu — czyli dokładniej szablonu — ma pierwszeństwo przed widoków zwrócony przez `Movie` kontrolera.)

W **Eksploratora rozwiązań**, rozwiń węzeł *widoków* folder, rozwiń węzeł *Shared* folder, a następnie kliknij prawym przyciskiem myszy *folder Views\Shared\DisplayTemplates* folderu.

Kliknij przycisk **Dodaj** a następnie kliknij przycisk **widoku**. **Dodaj widok** zostanie wyświetlone okno dialogowe.

W **nazwy widoku** wpisz `DateTime`. (Aby pasować do nazwy typu należy użyć tej nazwy).

Wybierz **Utwórz jako widok częściowy** pole wyboru. Upewnij się, że **Użyj układ strony wzorcowej** i **utworzyć widok silnie typizowane** nie są zaznaczone pola wyboru.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Kliknij przycisk **Dodaj**. A *DateTime.cshtml* szablon został utworzony w *folder Views\Shared\DisplayTemplates*.

Na poniższej ilustracji przedstawiono *widoków* folderu w **Eksploratora rozwiązań** po `DateTime` szablony ekranu i edytora są tworzone.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Otwórz *Views\Shared\DisplayTemplates\DateTime.cshtml* pliku i Dodaj następujący kod, który używa [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) metodę do właściwości w formacie daty bez godziny. ( `{0:d}` Format Określa format daty krótkiej.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Powtórz ten krok, aby utworzyć `DateTime` szablonu w *Views\Movie\DisplayTemplates* folderu. Użyj poniższego kodu w *Views\Movie\DisplayTemplates\DateTime.cshtml* pliku.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` Datą, która ma być wyświetlana czcionką pogrubioną red powoduje, że klasę CSS. Możesz dodać `loud-1` klasy CSS, po prostu jako tymczasowy środek, dzięki czemu można łatwo zobaczyć, kiedy tego konkretnego szablon jest używany.

Zostały wykonane zostanie utworzony i dostosowywać szablony używające programu ASP.NET na potrzeby wyświetlania dat. Bardziej ogólne szablonu (w *folder Views\Shared\DisplayTemplates* folder) zawiera prosty daty krótkiej. Szablon, który jest specjalnie pod kątem `Movie` kontrolera (w *Views\Movies\DisplayTemplates* folder) Wyświetla daty krótkiej, który również jest sformatowany jako pogrubiony czerwony tekst.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Przeglądarka wyświetla widok indeksu dla aplikacji.

`ReleaseDate` Właściwość teraz Wyświetla datę pogrubioną czcionką czerwony, bez czasu. Dzięki temu można potwierdzić, że `DateTime` oparte na szablonach pomocnika w *Views\Movies\DisplayTemplates* wybraniu folderu za pośrednictwem `DateTime` oparte na szablonach pomocy z folderu udostępnionego (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Teraz Zmień nazwę *Views\Movies\DisplayTemplates\DateTime.cshtml* plik *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Tym razem `ReleaseDate` właściwości wyświetla datę bez czasu i czerwony czcionki. Obrazuje to, że typu szablonu, który zawiera nazwę danych (w tym przypadku `DateTime`) jest automatycznie używany do wyświetlenia wszystkich właściwości modelu tego typu. Po zakończeniu zmieniona *DateTime.cshtml* plik *LoudDateTime.cshtml*, ASP.NET już nie można odnaleźć szablonu w *Views\Movies\DisplayTemplates* folderu, więc używać *DateTime.cshtml* szablonu z * Views\Movies\Shared\* folderu.

(Szablonu zgodnej jest uwzględniana wielkość liter, więc nazwa pliku szablonu mogły być utworzone przy użyciu dowolnej wielkości liter. Adapterem *DATETIME.chstml, datetime.cshtml*, i *DaTeTiMe.cshtml* wszystkie dopasuje `DateTime` typu.)

Aby zapoznać się z: w tym momencie `ReleaseDate` pola są wyświetlane przy użyciu *Views\Movies\DisplayTemplates\DateTime.cshtml* szablonu, który wyświetla dane przy użyciu formatu daty krótkiej, ale w przeciwnym razie dodaje nie specjalne formatowanie.

### <a name="using-uihint-to-specify-a-display-template"></a>Aby określić szablon wyświetlania przy użyciu UIHint

Jeśli aplikacja sieci web ma wiele `DateTime` pola i domyślnie, aby wyświetlić wszystkie lub większość z nich w trybie tylko do daty, *DateTime.cshtml* szablonu to dobra metoda. Ale co zrobić, jeśli masz kilka daty której chcesz wyświetlić pełną datę i godzinę? Nie ma sprawy. Można utworzyć dodatkowe szablony i użyć [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu, aby określić formatowanie dla pełnej daty i godziny. Można selektywnie użyć tego szablonu. Możesz użyć [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybut na poziomie modelu, lub możesz określić szablon w widoku. W tej sekcji pokazano, jak używać `UIHint` atrybutu, aby selektywnie zmienić formatowanie do niektórych wystąpień pól daty i godziny.

Otwórz *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* plik i Zastąp istniejący kod następującym kodem:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

To powoduje, że pełną datę i godzinę, które mają być wyświetlane i dodaje klasę CSS, która sprawia, że tekst zielony, jak i dużych.

Otwórz *Movie.cs* pliku i Dodaj [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu `ReleaseDate` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

To poinformuje platformę ASP.NET MVC, gdy zostanie wyświetlony `ReleaseDate` właściwości (w szczególności, a nie do byle `DateTime` obiektu), powinna korzystać *LoudDateTime.cshtml* szablonu.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Należy zauważyć, że `ReleaseDate` właściwości są obecnie wyświetlane data i godzina z użyciem dużej czcionki zielony.

Wróć do `UIHint` atrybutu w *Movie.cs* pliku, a komentarz dotyczący działanie więc *LoudDateTime.cshtml* nie będzie można użyć szablonu. Uruchom ponownie aplikację. Data wydania nie jest wyświetlana, duże i zielony. Sprawdza czy *Views\Shared\DisplayTemplates\DateTime.cshtml* szablon jest używany w widokach indeksu i szczegóły.

Jak wspomniano wcześniej, można także zastosować szablon w widoku umożliwiają zastosowanie szablonu poszczególne wystąpienia niektórych danych. Otwórz *Views\Movies\Details.cshtml* widoku. Dodaj `"LoudDateTime"` jako drugi parametr [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) wywołania dla `ReleaseDate` pola. Kompletny kod wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Określa, że `LoudDateTime` szablon powinien być używany do wyświetlania właściwości modelu, niezależnie od tego, jakie atrybuty są stosowane do modelu.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Sprawdź, czy używa strony indeksu filmu *Views\Shared\DisplayTemplates\DateTime.cshtml* szablonu (kolor czerwony pogrubienie) i *Movie\Details* strona używa *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* szablonu (dużych i zielony).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

W następnej sekcji utworzysz szablon dla typu złożonego.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
