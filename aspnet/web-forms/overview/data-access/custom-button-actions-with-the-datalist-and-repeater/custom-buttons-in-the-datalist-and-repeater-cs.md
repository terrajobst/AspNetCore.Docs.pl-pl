---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Przyciski niestandardowe DataList i powtarzanego (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziesz budujemy interfejs, który używa elementu powtarzanego do listy kategorii w systemie, z każdej kategorii, zapewniając przycisk, aby wyświetlić jego associ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: d6d07f1dc3f97523da6d9ee1d45302cac06b45d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876491"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Przyciski niestandardowe DataList i powtarzanego (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) lub [pobierania plików PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> W tym samouczku będziesz budujemy interfejs, który używa elementu powtarzanego do listy kategorii w systemie, z każdej kategorii, zapewniając przycisk Pokaż związane z nimi za pomocą formantu listy BulletedList produkty.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich samouczki siedemnaście DataList i powtarzanego możemy utworzyć kolejnych zarówno przykłady tylko do odczytu i edytowanie i usuwanie przykłady. Aby ułatwić edytowanie i usuwanie funkcji wewnątrz elementu DataList, dodaliśmy przyciski do DataList s `ItemTemplate` , po kliknięciu spowodowany odświeżania strony i wywołał zdarzenie DataList odpowiadający przycisk s `CommandName` właściwości. Na przykład dodawanie przycisku do `ItemTemplate` z `CommandName` wartość właściwości edycji powoduje DataList s `EditCommand` uruchomienie na ogłaszania zwrotnego; z `CommandName` usunąć zgłasza `DeleteCommand`.

Ponadto do edytowania i usuwania przycisków, formant DataList i powtarzanego mogą również obejmować przycisków, LinkButtons lub ImageButtons, po kliknięciu wykonać niektóre niestandardowej logiki po stronie serwera. W tym samouczku będziesz budujemy interfejs, który używa elementu powtarzanego do listy kategorii w systemie. W przypadku każdej kategorii powtarzanego zawiera przycisk, aby wyświetlić kategorii produktów s skojarzone za pomocą formantu listy BulletedList (zobacz rysunek 1).


[![Klikając przycisk Pokaż produktów łącze Wyświetla kategorii produktów s na liście punktowanej](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Rysunek 1**: kliknięcie Wyświetla Pokaż łącze produktów kategorii s produktów na liście punktowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Krok 1: Dodawanie przycisku niestandardowego samouczka stron sieci Web

Zanim przyjrzymy sposób dodawania niestandardowych przycisk umożliwiają s najpierw Poświęć chwilę, aby utworzyć stron ASP.NET w naszym projekt witryny sieci Web, które będą potrzebne w tym samouczku. Rozpocznij od dodania nowy folder o nazwie `CustomButtonsDataListRepeater`. Następnie dodaj następujące dwie strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strony głównej:

- `Default.aspx`
- `CustomButtons.aspx`


![Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Rysunek 2**: Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków


Podobnie jak w innych folderach `Default.aspx` w `CustomButtonsDataListRepeater` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Rysunek 3**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Ponadto dodanie stron jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po stronicowania i sortowania DataList i powtarzanego `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczki.


![Mapy witryny zawiera teraz wpis samouczek przyciski niestandardowe](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Rysunek 4**: mapy witryny zawiera teraz wpis samouczek przyciski niestandardowe


## <a name="step-2-adding-the-list-of-categories"></a>Krok 2: Dodawanie listy kategorii

W tym samouczku należy utworzyć elementu powtarzanego, w którym wyświetlane są wszystkie kategorie wraz z element LinkButton Pokaż produktów, kliknięcie spowoduje wyświetlenie produktów kategorii skojarzona s na liście punktowanej. Let s najpierw utworzyć prosty elementu powtarzanego, który zawiera listę kategorii w systemie. Uruchamianie przez otwarcie `CustomButtons.aspx` strony `CustomButtonsDataListRepeater` folderu. Przeciągnij elementu powtarzanego z przybornika do projektanta i zestaw jej `ID` właściwości `Categories`. Następnie należy utworzyć nowe kontroli źródła danych z tagów inteligentnych s elementu powtarzanego. W szczególności utworzyć nowe kontrolki ObjectDataSource o nazwie `CategoriesDataSource` który wybiera dane z `CategoriesBLL` klasy s `GetCategories()` metody.


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetCategories() s CategoriesBLL — klasa](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Rysunek 5**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` klasy s `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


W odróżnieniu od formant DataList, dla którego program Visual Studio tworzy domyślną `ItemTemplate` oparte na źródle danych, szablonów elementu powtarzanego s należy ręcznie zdefiniować. Ponadto szablonów elementu powtarzanego s muszą być utworzone i edytowane deklaratywnie (to znaczy występują s nie Edytuj szablony opcja w tagu elementu powtarzanego s).

Kliknij na karcie Źródło w lewym dolnym rogu i Dodaj `ItemTemplate` który wyświetla nazwę kategorii s w `<h3>` elementu i jego opis akapitu znacznik; obejmują `SeparatorTemplate` wyświetlający linia pozioma (`<hr />`) między nimi Kategoria. Również dodać LinkButton z jego `Text` właściwość Pokaż produktów. Po wykonaniu tych kroków, znaczniki deklaratywne s strony powinien wyglądać następująco:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Rysunek 6 przedstawia stronie podczas przeglądania za pośrednictwem przeglądarki. Każda nazwa i opis kategorii znajduje się. Pokaż produktów po kliknięciu przycisku, powoduje odświeżenie strony, ale nie wykonuje jeszcze żadnych działań.


[![Każda kategoria s nazwę i opis zostanie wyświetlona, wraz z element LinkButton Pokaż produktów](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Rysunek 6**: s kategorii każdą nazwę i opis zostanie wyświetlona, wraz z element LinkButton produktów Pokaż ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Krok 3: Wykonywanie po stronie serwera logiki podczas Pokaż produktów LinkButton zostanie kliknięty.

W dowolnym momencie po kliknięciu przycisku, LinkButton lub ImageButton w szablon DataList lub elementu powtarzanego odświeżania strony występuje i s DataList lub elementu powtarzanego `ItemCommand` generowane zdarzenie. Oprócz `ItemCommand` zdarzeń, DataList formant może też wiązać się z zdarzeń, bardziej szczegółowe Jeśli przycisk s `CommandName` właściwość jest ustawiona na jeden z zarezerwowanych ciągów (usuwanie, edytowanie, Anuluj, aktualizacji lub wybierz), ale `ItemCommand` zdarzenie jest *zawsze* uruchamiany.

Po kliknięciu przycisku wewnątrz elementu DataList lub elementu powtarzanego często musimy przekazują (takich jak który przycisk został kliknięty (w przypadku, może wystąpić wiele przycisków w formancie, takich jak zarówno edycji, a przycisk Usuń) i być może dodatkowe informacje wartość klucza podstawowego elementu, którego przycisk został kliknięty). Przycisk LinkButton i ImageButton Podaj dwie właściwości, których wartości są przekazywane do `ItemCommand` obsługi zdarzeń:

- `CommandName` ciąg zazwyczaj używany do identyfikowania każdego przycisku w szablonie
- `CommandArgument` często używane do przechowywania wartości niektóre pola danych, takie jak wartości klucza podstawowego

Na przykład ustawić LinkButton s `CommandName` właściwości ShowProducts i powiązania bieżącej wartości klucza podstawowego rekordu s `CategoryID` do `CommandArgument` właściwości przy użyciu składni wiązania z danymi `CategoryArgument='<%# Eval("CategoryID") %>'`. Po określeniu te dwie właściwości, składni deklaratywnej s LinkButton powinna wyglądać następująco:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Po kliknięciu przycisku, występuje odświeżania strony i s DataList lub elementu powtarzanego `ItemCommand` generowane zdarzenie. Program obsługi zdarzeń jest przekazywany przycisk s `CommandName` i `CommandArgument` wartości.

Utwórz program obsługi zdarzeń dla elementu powtarzanego s `ItemCommand` zdarzeń i zanotuj drugi parametr przekazany do obsługi zdarzeń (nazwane `e`). Jest to drugi parametr typu [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) i ma cztery następujące właściwości:

- `CommandArgument` wartość kliknięty przycisk s `CommandArgument` właściwości
- `CommandName` Wartość przycisku s `CommandName` właściwości
- `CommandSource` Odwołanie do formantu przycisku, który został kliknięty
- `Item` Odwołanie do [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) zawiera przycisk został kliknięty; każdy rekord powiązany powtarzanego jest dyskowe wyświetlane jako `RepeaterItem`

Od wybranej kategorii s `CategoryID` przekazany za pośrednictwem `CommandArgument` właściwości, możemy Pobierz zestaw produktów skojarzonych z wybranej kategorii w `ItemCommand` obsługi zdarzeń. Następnie można powiązać te produkty formantu listy BulletedList w `ItemTemplate` (których firma Microsoft kolejnych jeszcze, aby dodać). Wszystkie, pozostaje, a następnie jest dodanie listy BulletedList, odwołania `ItemCommand` obsługi zdarzeń i powiązać zestawu produktów dla wybranej kategorii, firma Microsoft będzie rozwiązania w kroku 4.

> [!NOTE]
> DataList s `ItemCommand` program obsługi zdarzeń jest przekazywany obiekt typu [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), które oferuje te same właściwości cztery jako `RepeaterCommandEventArgs` klasy.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Krok 4: Wyświetlanie wybranej kategorii produktów s na liście punktowanej

Produkty wybranej kategorii s mogą być wyświetlane w elemencie powtarzanym s `ItemTemplate` przy użyciu dowolnej liczby formantów. Można dodać inny zagnieżdżone elementu powtarzanego, DataList DropDownList, GridView i tak dalej. Ponieważ chcemy wyświetlić te produkty jako listy punktowane, jednak użyjemy formantu listy BulletedList. Powrót do `CustomButtons.aspx` stronie znaczników deklaratywne s, dodawanie do formantu listy BulletedList `ItemTemplate` po LinkButton Pokaż produktów. Ustaw BulletedLists s `ID` do `ProductsInCategory`. Listy BulletedList Wyświetla wartość w polu danych określona za pomocą `DataTextField` właściwości; ponieważ ta kontrolka będzie miała informacji o produkcie powiązane z nim, ustaw `DataTextField` właściwości `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

W `ItemCommand` program obsługi zdarzeń, odwoływać się przy użyciu tego formantu `e.Item.FindControl("ProductsInCategory")` i powiązać ją z zestawu skojarzonych z wybranej kategorii produktów.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Przed wykonaniem dowolnej akcji w `ItemCommand` program obsługi zdarzeń, jego s, aby najpierw należy sprawdzić wartość przychodzącego `CommandName`. Ponieważ `ItemCommand` obsługi zdarzenia generowane, gdy *żadnych* przycisku, jeśli istnieje wiele przycisków w przypadku użycia szablonu `CommandName` wartość do wykrycia jaka akcja ma być. Sprawdzanie `CommandName` Oto moot, ponieważ istnieje tylko jeden przycisk, ale jest dobrym nawyk do formularza. Następnie `CategoryID` wybranej kategorii są pobierane z `CommandArgument` właściwości. Formant listy BulletedList w szablonie jest odwołanie do i powiązany z wyników `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody.

W poprzedniej samouczki, takie jak używane przyciski wewnątrz elementu DataList [omówienie edytowanie i usuwanie danych elementu DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), Ustaliliśmy wartość klucza podstawowego danego elementu za pomocą `DataKeys` kolekcji. Chociaż to rozwiązanie działa dobrze z elementu DataList, nie ma powtarzanego `DataKeys` właściwości. Zamiast tego należy używamy o innym podejściu do podawania wartości klucza podstawowego, takich jak za pomocą przycisku s `CommandArgument` właściwości lub przypisując ukrytą kontrolkę etykiety w sieci Web w szablonie wartości klucza podstawowego i odczytywania wartości powrotem `ItemCommand`przy użyciu programu obsługi zdarzeń `e.Item.FindControl("LabelID")`.

Po zakończeniu `ItemCommand` program obsługi zdarzeń, Poświęć chwilę, aby przetestować tę stronę w przeglądarce. Jak pokazano na rysunku 7, klikając przycisk Pokaż produktów łącze powoduje odświeżenie strony i wyświetla produktów dla wybranej kategorii listy BulletedList. Ponadto należy pamiętać, że pozostaje te informacje o produkcie, nawet jeśli kliknięciu innych kategorii produktów Pokaż łącza.

> [!NOTE]
> Jeśli chcesz zmodyfikować zachowanie tego raportu tak, aby produkty tylko jedną kategorię s wymienione w czasie, wystarczy ustawić kontrolkę listy BulletedList s `EnableViewState` właściwości `False`.


[![Listy BulletedList służy do wyświetlania produktów wybranej kategorii](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Rysunek 7**: A listy BulletedList służy do wyświetlania produktów wybranej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Podsumowanie

Formant DataList i powtarzanego może zawierać dowolną liczbę przycisków, LinkButtons lub ImageButtons w ich szablonów. Przyciski, po kliknięciu powoduje odświeżenie strony i zgłosi `ItemCommand` zdarzeń. Aby skojarzyć akcji niestandardowej po stronie serwera z przycisk zostanie kliknięty, utworzyć programu obsługi zdarzeń dla `ItemCommand` zdarzeń. W tej obsłudze zdarzeń sprawdź najpierw przychodzącego `CommandName` wartość, aby ustalić, który przycisk został kliknięty. Opcjonalnie można podać dodatkowe informacje za pomocą przycisku s `CommandArgument` właściwości.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został firmy Dennis Patterson. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](custom-buttons-in-the-datalist-and-repeater-vb.md)
