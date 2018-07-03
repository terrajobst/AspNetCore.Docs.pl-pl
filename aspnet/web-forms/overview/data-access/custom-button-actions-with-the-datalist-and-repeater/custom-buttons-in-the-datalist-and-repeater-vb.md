---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Przyciski niestandardowe w kontrolkach DataList i Repeater (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku utworzymy interfejs, który używa Repeater, aby wyświetlić listę kategorii w systemie, z każdej kategorii, zapewniając przycisku, aby wyświetlał jego associ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 7dc738e22f7670aa3eeb07d69888a4df47c1fc82
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371587"
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Przyciski niestandardowe w kontrolkach DataList i Repeater (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) lub [Pobierz plik PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> W tym samouczku utworzymy interfejs, który używa Repeater listy zadań do kategorii w systemie, z każdej kategorii, zapewniając przycisku, aby wyświetlał jego skojarzone produkty za pomocą kontroli BulletedList.


## <a name="introduction"></a>Wprowadzenie

W poprzednich samouczkach siedemnastu DataList i Repeater firma Microsoft ve utworzone zarówno przykłady tylko do odczytu i edytowania i usuwania przykłady. W celu ułatwienia edycji i usuwania możliwości w ramach kontrolką DataList, dodaliśmy przycisków do kontrolki DataList s `ItemTemplate` , po kliknięciu spowodowała odświeżenie strony i zgłoszone zdarzenie DataList, odpowiadający przycisk s `CommandName` właściwości. Na przykład dodanie przycisku, aby `ItemTemplate` z `CommandName` DataList s powoduje, że wartość właściwości edycji `EditCommand` do uruchomienia podczas ogłaszania zwrotnego; z `CommandName` Usuń zgłasza `DeleteCommand`.

Ponadto do edytowania i usuwania przyciski, kontrolek DataList i Repeater mogą również obejmować przycisków, LinkButtons lub ImageButtons, po kliknięciu wykonania niestandardowej logiki biznesowej po stronie serwera. W tym samouczku utworzymy interfejs, który używa Repeater, aby wyświetlić listę kategorii w systemie. Dla każdej kategorii powtarzanego będzie zawierać przycisku, aby wyświetlał kategorii produktów związane za pomocą kontroli BulletedList (patrz rysunek 1).


[![Kliknięcie przycisku Wyświetla łącze produktów pokaz produktów s kategorii na liście punktowanej](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Rysunek 1**: kliknięcie przycisku Wyświetla Pokaż łącza produktów kategorii s produktów na liście punktowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Krok 1: Dodawanie stron sieci Web z samouczka niestandardowy przycisk

Zanim przyjrzymy się jak dodać niestandardowy przycisk, chętnie s najpierw Poświęć chwilę, aby utworzyć strony ASP.NET w projekcie naszej witryny sieci Web, które będą potrzebne w tym samouczku. Rozpocznij od dodania nowy folder o nazwie `CustomButtonsDataListRepeater`. Następnie dodaj następujące dwie strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `CustomButtons.aspx`


![Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Rysunek 2**: Dodawanie stron ASP.NET niestandardowe samouczki dotyczące przycisków


Podobnie jak w przypadku innych folderów `Default.aspx` w `CustomButtonsDataListRepeater` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. Ten formant użytkownika, aby dodać `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Rysunek 3**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


Na koniec Dodaj strony jako wpisy, aby `Web.sitemap` pliku. Ściślej mówiąc, Dodaj następujący kod po stronicowanie i sortowanie za pomocą kontrolek DataList i Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczków.


![Mapa witryny zawiera teraz wpis dla samouczka przyciski niestandardowe](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Rysunek 4**: mapy witryny zawiera teraz wpis dla samouczka przyciski niestandardowe


## <a name="step-2-adding-the-list-of-categories"></a>Krok 2: Dodawanie listy kategorii

W tym samouczku należy utworzyć Repeater zawierającego wszystkie kategorie wraz z Pokaż element LinkButton produktów, kliknięcie spowoduje wyświetlenie produktów kategorii skojarzona s na liście punktowanej. Pozwól, s, najpierw Utwórz prostą elementu powtarzanego, który wyświetla listę kategorii w systemie. Zacznij od otwarcia `CustomButtons.aspx` stronie `CustomButtonsDataListRepeater` folderu. Przeciągnij Repeater z przybornika do projektanta i ustaw jego `ID` właściwość `Categories`. Następnie utwórz nowy formant źródła danych za pomocą tagu inteligentnego s elementu powtarzanego. Dokładniej mówiąc, Utwórz nowy kontrolka ObjectDataSource, o nazwie `CategoriesDataSource` który wybiera dane z `CategoriesBLL` klasy s `GetCategories()` metody.


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody klasy CategoriesBLL GetCategories() s](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Rysunek 5**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy s `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


W przeciwieństwie do kontrolki DataList, dla których program Visual Studio tworzy domyślną `ItemTemplate` oparty na źródle danych, szablony elementu powtarzanego s należy zdefiniować ręcznie. Ponadto, szablony elementu powtarzanego s muszą być tworzone i edytować w sposób deklaratywny (czyli tam s nie Edytuj szablony opcji tagu inteligentnego w elemencie powtarzanym s).

Kliknij na karcie Źródło w lewym dolnym rogu, a następnie dodaj `ItemTemplate` wyświetlającą nazwę kategorii s w `<h3>` elementu i jego opis, akapitu tag; uwzględnia `SeparatorTemplate` wyświetlającą linii poziomej (`<hr />`) między poszczególnymi Kategoria. Również dodać element LinkButton z jego `Text` właściwość ustawioną na wyświetlanie produktów. Po wykonaniu tych kroków, znaczniki deklaratywne s strony powinny wyglądać następująco:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

Rysunek 6 przedstawia stronę po wyświetleniu za pośrednictwem przeglądarki. Każda nazwa i opis kategorii znajduje się. Pokaż produktów po kliknięciu przycisku, powoduje odświeżenie strony, ale jeszcze nie wykonuje żadnych działań.


[![Każda kategoria s Nazwa i opis jest wyświetlany, oraz wyświetlić element LinkButton produktów](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Rysunek 6**: s każdej kategorii, nazwa i opis jest wyświetlany, oraz wyświetlić element LinkButton produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Krok 3: Wykonywanie po stronie serwera logiki po Pokaż produktów LinkButton kliknięciu

W dowolnym momencie po kliknięciu przycisku, element LinkButton lub ImageButton w szablonie w kontrolkach DataList lub Repeater występuje odświeżenie strony i s DataList lub Repeater `ItemCommand` generowane zdarzenie. Oprócz `ItemCommand` zdarzenia DataList kontroli może również wywoływać zdarzeń, bardziej szczegółowe, jeśli przycisk s `CommandName` właściwość jest ustawiona na jeden z ciągów zarezerwowane (Delete, Edytuj, Anuluj, aktualizacji lub wybierz), ale `ItemCommand` zdarzenie jest *zawsze* wyzwolone.

Kliknięcie przycisku w kontrolkach DataList lub Repeater często musimy przekazują której przycisk został kliknięty (w przypadku, że może istnieć wiele przycisków w formancie, takie jak zarówno edytowanie i przycisk usuwania) i być może niektóre dodatkowe informacje (np. wartość klucza podstawowego elementu, w której przycisk został kliknięty). Button, element LinkButton i ImageButton zapewniają dwie właściwości, których wartości są przekazywane do `ItemCommand` program obsługi zdarzeń:

- `CommandName` ciąg, który zazwyczaj używany do identyfikowania każdego przycisku w szablonie
- `CommandArgument` często używane do przechowywania wartości niektóre pola danych, na przykład wartość klucza podstawowego

W tym przykładzie należy ustawić LinkButton s `CommandName` właściwości ShowProducts i powiązania bieżącego rekordu s wartość klucza podstawowego `CategoryID` do `CommandArgument` właściwości przy użyciu składni wiązania danych `CategoryArgument='<%# Eval("CategoryID") %>'`. Po określeniu te dwie właściwości, składni deklaratywnej s LinkButton powinien wyglądać następująco:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Po kliknięciu przycisku, występuje odświeżenie strony i s DataList lub Repeater `ItemCommand` generowane zdarzenie. Program obsługi zdarzeń jest przekazywany przycisk s `CommandName` i `CommandArgument` wartości.

Utwórz procedurę obsługi zdarzeń dla elementu powtarzanego s `ItemCommand` zdarzeń i zanotuj drugi parametr przekazany do procedury obsługi zdarzeń (nazwane `e`). Ten drugi parametr jest typu [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) i ma następujące cztery właściwości:

- `CommandArgument` wartość kliknięto przycisk s `CommandArgument` właściwości
- `CommandName` Wartość przycisku s `CommandName` właściwości
- `CommandSource` Odwołanie do formantu przycisku który został kliknięty
- `Item` Odwołanie do [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) , która zawiera przycisk, który został kliknięty; każdy rekord powiązany z powtarzanego jest dyskowe widoczne jako `RepeaterItem`

Od wybranej kategorii s `CategoryID` jest przekazywany za pośrednictwem `CommandArgument` właściwości, firma Microsoft można uzyskać zestaw produktów skojarzonych z wybranej kategorii w `ItemCommand` programu obsługi zdarzeń. Te produkty następnie może być powiązana z kontrolką BulletedList w `ItemTemplate` (które firma Microsoft ve jeszcze, aby dodać). Wszystkie, które pozostaje, a następnie, jest dodanie BulletedList, odwołaj się do niego w `ItemCommand` programu obsługi zdarzeń i powiązać zestaw produktów dla wybranej kategorii, firma Microsoft będzie czoła w kroku 4.

> [!NOTE]
> DataList s `ItemCommand` programu obsługi zdarzeń jest przekazywany obiekt typu [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), który oferuje te same właściwości cztery jako `RepeaterCommandEventArgs` klasy.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Krok 4: Wyświetlanie wybranej kategorii produktów s liście punktowanej we wcześniejszej

Produkty wybranej kategorii s może być wyświetlana w elemencie powtarzanym s `ItemTemplate` przy użyciu dowolnej liczby kontrolek. Firma Microsoft może dodać zagnieżdżone innego elementu powtarzanego, kontrolką DataList, kontrolki DropDownList, GridView i tak dalej. Ponieważ chcemy wyświetlić produktów jako listy punktowane, jednak użyjemy kontroli BulletedList. Wracając do `CustomButtons.aspx` s oznaczeniu deklaracyjnym strony, należy dodać formant BulletedList do `ItemTemplate` po Pokaż element LinkButton produktów. Ustaw BulletedLists s `ID` do `ProductsInCategory`. BulletedList wyświetlana jest wartość pola danych określona za pomocą `DataTextField` właściwości; ponieważ ta kontrolka będzie miała informacje o produkcie powiązane z nim, ustaw `DataTextField` właściwość `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

W `ItemCommand` programu obsługi zdarzeń odwoływać się do tego formantu przy użyciu `e.Item.FindControl("ProductsInCategory")` i powiązać ją z zestawu produkty powiązane z wybranej kategorii.


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Przed wykonaniem dowolnej czynności w `ItemCommand` programu obsługi zdarzeń go s rozsądne najpierw sprawdzić wartość przychodzącego `CommandName`. Ponieważ `ItemCommand` programu obsługi zdarzeń jest uruchamiany, gdy *wszelkie* przycisku, jeśli istnieje wiele przycisków używany szablon `CommandName` wartość do określenia, jaka akcja ma być. Sprawdzanie `CommandName` Oto moot, ponieważ mamy jeden przycisk, ale to dobry sposób formularza. Następnie `CategoryID` w wybranej kategorii jest pobierana z `CommandArgument` właściwości. Następnie odwołania i powiązane z wynikami w szablonie kontrolki BulletedList `ProductsBLL` klasy s `GetProductsByCategoryID(categoryID)` metody.

W poprzednich samouczkach, które używane przycisków w kontrolkach DataList, taką jak [omówienie edytowania i usuwania danych w kontrolce DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), Ustaliliśmy, wartość klucza podstawowego danego elementu za pomocą `DataKeys` kolekcji. Chociaż to podejście sprawdza się również za pomocą kontrolki DataList, nie ma powtarzanego `DataKeys` właściwości. Zamiast tego trzeba użyć zapisu alternatywnym podejściem dla podanie wartości klucza podstawowego, takie jak za pomocą przycisku s `CommandArgument` właściwości lub przypisując wartość klucza podstawowego ukrytą kontrolkę etykiety w sieci Web w ramach szablonu i odczytu jego wartości w `ItemCommand`przy użyciu programu obsługi zdarzeń `e.Item.FindControl("LabelID")`.

Po zakończeniu `ItemCommand` procedura obsługi zdarzeń, Poświęć chwilę, w celu przetestowania tej strony w przeglądarce. Jak pokazano na rysunku 7, klikając przycisk Pokaż produktów link powoduje odświeżenie strony i wyświetla produktów dla wybranej kategorii BulletedList. Ponadto należy pamiętać, że te informacje o produkcie pozostaje, nawet wtedy, gdy inne kategorie produktów Pokaż linki zostały kliknięte.

> [!NOTE]
> Jeśli chcesz zmienić zachowanie tego raportu, taki sposób, że produkty tylko jedną kategorię s są wymienione w danym momencie, wystarczy ustawić dla formant BulletedList s `EnableViewState` właściwość `False`.


[![BulletedList służy do wyświetlania produktów wybranej kategorii](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Rysunek 7**: BulletedList służy do wyświetlania produktów wybranej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>Podsumowanie

Kontrolki DataList i Repeater może zawierać dowolną liczbę przycisków, LinkButtons lub ImageButtons w ramach własnych szablonów. Przyciski, po kliknięciu powoduje odświeżenie strony i zgłosić `ItemCommand` zdarzeń. Aby skojarzyć akcji niestandardowej po stronie serwera za pomocą przycisku, w momencie kliknięcia, należy utworzyć program obsługi zdarzeń dla `ItemCommand` zdarzeń. W tej obsługi zdarzeń najpierw sprawdzić przychodzącego `CommandName` wartość, aby określić, której przycisk został kliknięty. Opcjonalnie można podać dodatkowe informacje za pomocą przycisku s `CommandArgument` właściwości.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Dennis Patterson. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](custom-buttons-in-the-datalist-and-repeater-cs.md)
