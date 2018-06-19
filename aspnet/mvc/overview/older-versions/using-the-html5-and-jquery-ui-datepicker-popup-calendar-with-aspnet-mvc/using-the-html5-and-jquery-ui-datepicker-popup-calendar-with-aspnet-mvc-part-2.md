---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 2 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875451"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Używanie kalendarza podręcznego selektora daty interfejsu użytkownika jQuery i HTML5 platformie ASP.NET MVC — część 2
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku uczy podstaw sposób pracy z szablonami edytora, szablonów wyświetlania i kalendarza podręcznego selektora daty interfejsu użytkownika jQuery, w aplikacji sieci Web programu ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Dodawanie szablonu automatycznej daty i godziny

W pierwszej części tego samouczka przedstawiono sposób dodawania atrybutów do modelu, aby jawnie określić formatowania i sposób jawnie Określ szablon, który jest używany do renderowania modelu. Na przykład [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu w następujących jawnie kodu Określa formatowanie `ReleaseDate` właściwości.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

W poniższym przykładzie [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atrybutu przy użyciu `Date` wyliczenia, określa, czy szablon dat powinny być używane do renderowania modelu. Jeśli w projekcie nie ma żadnego szablonu datę, jest używany szablon dat wbudowanych.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Jednak ASP. MVC można wykonać dopasowywanie typu przy użyciu konwencji over konfiguracji, przeszukując szablonu, która jest zgodna z nazwą typu. Dzięki temu można utworzyć szablon, który automatycznie formatuje dane bez użycia ani kodu innych atrybutów w ogóle. Dla tej części samouczka utworzysz szablon, który jest automatycznie stosowana do właściwości modelu typu [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Nie trzeba używać atrybutu lub innych konfiguracji do określenia, czy szablon powinien zostać użyty do renderowania wszystkie właściwości modelu typu [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Dowiesz się również sposób, aby dostosować wyświetlanie indywidualne właściwości lub nawet poszczególnych pól.

Aby rozpocząć, umożliwia usunięcie istniejących informacji formatowania i wyświetlanie dat pełna w aplikacji.

Otwórz *Movie.cs* plików i Oznacz jako komentarz `DataType` atrybutu `ReleaseDate` właściwości:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Zwróć uwagę, że `ReleaseDate` właściwości są obecnie wyświetlane data i godzina, ponieważ jest to wartość domyślna, gdy nie formatowania informacje.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Dodawanie style CSS do testowania nowych szablonów

Przed utworzeniem szablonu formatowania dat dodasz kilka reguł stylu CSS, które można stosować do nowych szablonów. Te pomogą należy sprawdzić, czy nowy szablon używa renderowanej strony.

Otwórz *Content\Site.cs*s i dodaj następujące reguły CSS do końca pliku:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Dodanie szablonów wyświetlania daty i godziny

Teraz można utworzyć nowy szablon. W *Views\Movies* folderu, Utwórz *DisplayTemplates* folderu.

W *Views\Shared* folderu, Utwórz *DisplayTemplates* folderu i *EditorTemplates* folderu.

Szablony ekranu w *Views\Shared\DisplayTemplates* folder będzie używany przez wszystkie kontrolery. Szablony ekranu w *Views\Movie\DisplayTemplates* folder będzie używany tylko przez `Movie` kontrolera. (Jeśli szablon o tej samej nazwie, zostanie wyświetlony w obu folderów szablonu w *Views\Movie\DisplayTemplates* folderu — to znaczy więcej określonego szablonu — ma pierwszeństwo przed dla widoków zwracanych przez `Movie` kontroler.)

W **Eksploratora rozwiązań**, rozwiń węzeł *widoków* folder, rozwiń węzeł *Shared* folder, a następnie kliknij prawym przyciskiem myszy *Views\Shared\DisplayTemplates* folderu.

Kliknij przycisk **Dodaj** , a następnie kliknij przycisk **widoku**. **Dodaj widok** zostanie wyświetlone okno dialogowe.

W **nazwy widoku** wpisz `DateTime`. (Aby zgodna z nazwą typu należy użyć tej nazwy).

Wybierz **Utwórz jako widok częściowy** pole wyboru. Upewnij się, że **Użyj układ strony wzorcowej** i **utworzyć widok jednoznacznie** nie są zaznaczone pola wyboru.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Kliknij przycisk **Dodaj**. A *DateTime.cshtml* szablonu jest tworzony w *Views\Shared\DisplayTemplates*.

Poniższy obraz przedstawia *widoków* folderu w **Eksploratora rozwiązań** po `DateTime` edytorze i Wyświetl szablony są tworzone.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Otwórz *Views\Shared\DisplayTemplates\DateTime.cshtml* i Dodaj następujący kod, który używa [String.Format](https://msdn.microsoft.com/library/system.string.format.aspx) metody do właściwości w formacie daty bez czasu. ( `{0:d}` Format Określa format daty krótkiej.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Powtórz ten krok, aby utworzyć `DateTime` szablonu w *Views\Movie\DisplayTemplates* folderu. Użyj następującego kodu w *Views\Movie\DisplayTemplates\DateTime.cshtml* pliku.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` Klasy CSS powoduje, że data jest wyświetlana pogrubione czerwony. Możesz dodać `loud-1` klasy CSS, tak jak w przypadku tymczasowego miary tak, aby łatwo widoczny, gdy jest używany ten konkretny szablon.

Co to jest tworzony i dostosowywać szablony używające ASP.NET na potrzeby wyświetlania dat. Więcej ogólnych szablonu (w *Views\Shared\DisplayTemplates* folder) zawiera proste daty krótkiej. Szablon, który jest specjalnie z myślą o `Movie` kontrolera (w *Views\Movies\DisplayTemplates* folder) Wyświetla Data krótka, który również jest sformatowany pogrubioną czerwony.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Przeglądarka renderuje widok indeksu dla aplikacji.

`ReleaseDate` Właściwości są obecnie wyświetlane data pogrubioną czcionką red bez czasu. Dzięki temu można potwierdzić, że `DateTime` szablonem pomocnika w *Views\Movies\DisplayTemplates* wybraniu folderu za pośrednictwem `DateTime` szablonem pomocnika w folderze udostępnionym (*Views\Shared\ DisplayTemplates*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Teraz Zmień nazwę *Views\Movies\DisplayTemplates\DateTime.cshtml* pliku *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Teraz `ReleaseDate` właściwości wyświetla datę bez czasu i bez pogrubioną czcionką czerwony. To potwierdza, że typ szablonu, który ma nazwę danych (w tym przypadku `DateTime`) jest automatycznie używany do wyświetlania wszystkich właściwości modelu tego typu. Po zmieniona *DateTime.cshtml* pliku *LoudDateTime.cshtml*, ASP.NET nie znaleziono szablonu w *Views\Movies\DisplayTemplates* folderu, więc użyć *DateTime.cshtml* szablonu z * Views\Movies\Shared\* folderu.

(Zgodnego szablonu jest uwzględniana wielkość liter, więc może spowodowały nazwa pliku szablonu z dowolnej wielkości liter. For example *DATETIME.chstml, datetime.cshtml*, i *DaTeTiMe.cshtml* wszystkie spełniają kryteria `DateTime` typu.)

Aby przejrzeć: w tym momencie `ReleaseDate` polu jest wyświetlany za pomocą *Views\Movies\DisplayTemplates\DateTime.cshtml* szablonu, który zawiera dane przy użyciu formatu daty krótkiej, ale nie dodaje w przeciwnym razie nie specjalne formatu.

### <a name="using-uihint-to-specify-a-display-template"></a>Określ szablon wyświetlania przy użyciu UIHint

Jeśli aplikacja sieci web z wieloma `DateTime` pól i domyślnie, aby wyświetlić wszystkie lub większość z nich w trybie tylko do daty, *DateTime.cshtml* szablonu jest dobre rozwiązanie. Ale co zrobić, jeśli masz kilka dat której chcesz wyświetlić pełną datę i godzinę? Nie ma sprawy. Możesz utworzyć dodatkowe szablon i użyć [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybut, aby określić formatowanie dla pełnej datę i godzinę. Następnie można selektywnie zastosować ten szablon. Można użyć [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu na poziomie modelu lub możesz określić szablonu w widoku. W tej sekcji, zobaczysz sposób użycia `UIHint` atrybut do selektywnie formatowania dla niektórych wystąpień pól daty i godziny.

Otwórz *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* plików i Zastąp istniejący kod następującym kodem:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

To powoduje, że pełna datę i godzinę, które mają być wyświetlane i dodaje klasę CSS, która sprawia, że tekst zielony i duże.

Otwórz *Movie.cs* plik i dodać [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atrybutu `ReleaseDate` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Ten sposób platforma ASP.NET MVC który wyświetlanych `ReleaseDate` właściwości (w szczególności, a nie do dowolnego `DateTime` obiektu), należy go używać *LoudDateTime.cshtml* szablonu.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Zwróć uwagę, że `ReleaseDate` właściwości są obecnie wyświetlane data i godzina z użyciem dużej czcionki zielony.

Wróć do `UIHint` atrybutu w *Movie.cs* plik i dodać komentarz go tak *LoudDateTime.cshtml* nie będzie można użyć szablonu. Uruchom ponownie aplikację. Data wydania nie jest wyświetlana, duże i zielony. Sprawdza czy *Views\Shared\DisplayTemplates\DateTime.cshtml* szablon jest używany w widokach indeksu i szczegóły.

Jak wspomniano wcześniej, można także zastosować dla szablonu w widoku, która pozwala na zastosowanie szablonu do wystąpienia poszczególnych niektórych danych. Otwórz *Views\Movies\Details.cshtml* widoku. Dodaj `"LoudDateTime"` jako drugi parametr funkcji [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) wymagają `ReleaseDate` pola. Kompletny kod wygląda następująco:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Oznacza to, że `LoudDateTime` można użyć szablonu, aby wyświetlić właściwości modelu, niezależnie od tego, jakie atrybuty są stosowane do modelu.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.

Sprawdź, czy strona indeksu filmu używa *Views\Shared\DisplayTemplates\DateTime.cshtml* szablonu (pogrubienie czerwony) i *Movie\Details* strona używa *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* szablonu (dużych i zielony).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

W następnej sekcji utworzysz szablonu typu złożonego.

> [!div class="step-by-step"]
> [Poprzednie](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [dalej](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
