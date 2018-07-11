---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Filtrowanie rekordu głównego/szczegółów przy użyciu kontrolki DropDownList (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku opisano sposób wyświetlania rekordy główne w kontrolki DropDownList i szczegóły wybranego elementu listy w GridView.
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: a1bd1a20950376244c1d461d139f3eee6bc9a9cc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816259"
---
<a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtrowanie rekordu głównego/szczegółów przy użyciu kontrolki DropDownList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) lub [Pobierz plik PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> W tym samouczku opisano sposób wyświetlania rekordy główne w kontrolki DropDownList i szczegóły wybranego elementu listy w GridView.


## <a name="introduction"></a>Wprowadzenie

Spotykanym typem raportu jest *rekordu głównego/szczegółów w raporcie*, w który raportu rozpoczyna się poprzez wyświetlenie niektórych zestawu rekordów "główną". Użytkownika można następnie przejść do jednego z rekordów wzorca ten sposób wyświetlania tego rekordu głównego "szczegółowych informacji." Wzorzec/szczegół raporty są idealnym wyborem w przypadku wizualizacji relacji jeden do wielu, taki jak raport przedstawiający wszystkie kategorie i następnie umożliwiając użytkownikowi wybranie konkretnej kategorii i wyświetlić jego skojarzone produkty. Ponadto raporty wzorzec/szczegół są przydatne do wyświetlania szczegółowych informacji z tabel szczególnie "szerokiego", (te, które mają wiele kolumn). Na przykład "główną" poziom raportu wzorzec/szczegół może wyświetlać tylko nazwę i jednostkę cena produktu produktów w bazie danych i przechodzenie do szczegółów do konkretnego produktu będą wyświetlane pola dodatkowe produktu (kategoria, dostawca, ilość na jednostkę, i itd.).

Istnieje wiele sposobów, za pomocą których można zaimplementować wzorzec/szczegół raportu. Za pośrednictwem to i trzech kolejnych samouczków przyjrzymy szerokiej gamy raportów wzorzec/szczegół. W tym samouczku opisano sposób wyświetlania wzorca rekordów w [kontrolki DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) i szczegóły dotyczące wybranego elementu listy w GridView. W szczególności w tym samouczku wzorzec/szczegół raportu wyświetli listę kategorii i informacje o produkcie.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Krok 1: Wyświetlanie kategorii w kontrolki DropDownList

Raport wzorzec/szczegół spowoduje wyświetlenie listy kategorii z kontrolki DropDownList, za pomocą elementu wybranej listy produktów wyświetlane dalej na dół strony w GridView. Pierwsze zadanie w przód od nas, następnie jest kategorie wyświetlane w kontrolki DropDownList. Otwórz `FilterByDropDownList.aspx` strony w `Filtering` folderu, przeciągnij z przybornika do projektanta strony kontrolki DropDownList i ustaw jego `ID` właściwość `Categories`. Następnie kliknij łącze Wybierz źródło danych z kontrolki DropDownList tagu inteligentnego. Spowoduje to wyświetlenie Kreatora konfiguracji źródła danych.


[![Określ źródło danych metody DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Rysunek 1**: Określ źródło danych metody DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))


Wybierz dodać nowe kontrolki ObjectDataSource, o nazwie `CategoriesDataSource` wywołującej `CategoriesBLL` klasy `GetCategories()` metody.


[![Dodawanie nowego elementu ObjectDataSource, o nazwie CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Rysunek 2**: Dodaj nazwę nowej kontrolki ObjectDataSource `CategoriesDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))


[![Wybierz użyć klasy CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Rysunek 3**: możliwość wykorzystania `CategoriesBLL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetCategories()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource do użycia `GetCategories()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))


Po skonfigurowaniu ObjectDataSource musimy nadal określić które pole źródła danych powinny być wyświetlane w DropDownList i jedną powinna być skojarzona jako wartość dla elementu listy. Masz `CategoryName` pola jako ekran i `CategoryID` jako wartość dla każdego elementu listy.


[![Masz wyświetlana lista DropDownList na pole CategoryName i użyj CategoryID jako wartość](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Rysunek 5**: wyświetlone DropDownList `CategoryName` pola i użyj `CategoryID` jako wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))


W tym momencie mamy wypełniony rekordy z kontrolki DropDownList `Categories` tabeli (wszystkie wykonywane w ciągu około sześciu sekund). Rysunek 6. pokazuje nasz postęp tej pory, podczas wyświetlania za pośrednictwem przeglądarki.


[![Menu rozwijane zawiera listę bieżących kategorii](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Rysunek 6**: Wyświetla listę rozwijaną listę bieżących kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))


## <a name="step-2-adding-the-products-gridview"></a>Krok 2: Dodawanie kontrolki GridView produktów

Ten ostatni etap naszego raportu wzorzec/szczegół jest aby wyświetlić listę produktów skojarzonych z wybranej kategorii. W tym celu na stronie Dodaj GridView i utworzyć nowe kontrolki ObjectDataSource, o nazwie `productsDataSource`. Masz `productsDataSource` kontroli wybrakowane swoje dane z `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody.


[![Wybierz metodę GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Rysunek 7**: Wybierz `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))


Po wybraniu tej metody, Kreator ObjectDataSource nam monituje o podanie wartości dla metody *`categoryID`* parametru. Aby użyć wartości wybranych `categories` elementu DropDownList Ustaw źródło parametru do kontroli i ControlID do `Categories`.


[![Ustaw categoryID parametru na wartość DropDownList kategorii](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Rysunek 8**: Ustaw *`categoryID`* parametru na wartość `Categories` DropDownList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))


Poświęć chwilę, aby wyewidencjonować postępach w przeglądarce. Po pierwsze, odwiedzając stronę, tych produktów należy do wybranej kategorii (Beverages) są wyświetlane (jak pokazano na rysunku 9), ale zmiana metody DropDownList nie powoduje aktualizacji danych. Jest to spowodowane ogłaszania zwrotnego musi nastąpić GridView do zaktualizowania. W tym celu mamy dwie opcje (z których żadna nie wymaga pisania żadnego kodu):

- **Ustawianie kategorii DropDownList firmy**[właściwości AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**na wartość True.** (Można to zrobić, zaznaczając opcję włączenia automatycznego ogłaszania zwrotnego w tagu inteligentnego DropDownList.) Wywoła to odświeżenie strony zawsze wtedy, gdy wybrano DropDownList elementu zostanie zmieniony przez użytkownika. W związku z tym gdy użytkownik wybierze nową kategorię z metody DropDownList nastąpi odświeżenie strony i kontrolki GridView zostanie zaktualizowana z produktami w nowo wybranej kategorii. (Jest to podejście, które wcześniej używanych w ramach tego samouczka).
- **Dodawanie kontrolki przycisku w sieci Web obok metody DropDownList.** Ustaw jego `Text` właściwość odświeżania lub podobna. W tym podejściu użytkownik należy wybrać nową kategorię, a następnie kliknij przycisk. Kliknięcie przycisku powoduje odświeżenie strony i zaktualizuj GridView, aby wyświetlić listę tych produktów w wybranej kategorii.

Rysunki 9 i 10 ilustrują raportu wzorzec/szczegół w działaniu.


[![Po pierwsze, odwiedzając stronę, są wyświetlane produkty spożywczy](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Rysunek 9**: po pierwsze, odwiedzając stronę, są wyświetlane produkty spożywczy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))


[![Zaznaczenie nowego produktu (produkty) automatycznie powoduje odświeżenie strony, trwa aktualizowanie widoku GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Na rysunku nr 10**: zaznaczenie nowego produktu (produkty) automatycznie powoduje odświeżenie strony, aktualizowanie kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))


## <a name="adding-a----choose-a-category----list-item"></a>Dodawanie elementu listy "— Wybierz kategorię--"

Podczas odwiedzania najpierw `FilterByDropDownList.aspx` stronie kategorii pierwszego elementu listy przez kontrolki DropDownList (Beverages) jest zaznaczona domyślnie, przedstawiający produktów spożywczy w widoku GridView. Zamiast wyświetlanie pierwszej kategorii produktów, warto zamiast tego ma element DropDownList wybrany, jest wyświetlany komunikat podobny, "— Wybierz kategorię--".

Aby dodać nowy element listy do metody DropDownList, przejdź do okna właściwości, a następnie kliknąć wielokropek w `Items` właściwości. Dodaj nowy element listy z `Text` "— Wybierz kategorię--" i `Value` `-1`.


[![Dodaj wybierz kategorię — element listy](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Rysunek 11**: Dodaj wybierz kategorię — element listy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))


Alternatywnie można dodać elementu listy, dodając następujący kod do metody DropDownList:


[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

Ponadto należy ustawić kontrolki DropDownList `AppendDataBoundItems` na wartość True, ponieważ w przypadku kategorie są powiązane z metody DropDownList z kontrolki ObjectDataSource będzie zastępują wszystkie elementy dodane ręcznie listy Jeśli `AppendDataBoundItems` nie ma wartość True.


![Ustaw właściwość AppendDataBoundItems na wartość True](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Rysunek 12**: Ustaw `AppendDataBoundItems` właściwości na wartość True


Po wprowadzeniu tych zmian po raz pierwszy, odwiedzając stronę opcji "--Wybierz kategorię--" jest zaznaczone, a produkty nie są wyświetlane.


[![Po załadowaniu strony początkowej produkty nie są wyświetlane](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Rysunek 13**: są wyświetlane na początkowej stronie obciążenia bez produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))


Przyczyną produkty nie są wyświetlane po, ponieważ wybrano element listy "— Wybierz kategorię--" jest jego wartość wynosi `-1` i brak produktów w bazie danych o `CategoryID` z `-1`. Jeśli to zachowanie, które chcesz wszystko będzie gotowe na tym etapie. Jeśli jednak chcesz wyświetlić *wszystkich* z kategorii po wybraniu elementu listy "— Wybierz kategorię--" powrócić do `ProductsBLL` klasy i dostosowywanie `GetProductsByCategoryID(categoryID)` metodę, tak że wywołuje `GetProducts()` metoda Jeśli przekazany w *`categoryID`* parametru jest mniejsza od zera:


[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

Techniki używane w tym miejscu jest podobna do metody możemy używane do wyświetlania wszystkich dostawców w [parametry deklaratywne](../basic-reporting/declarative-parameters-cs.md) samouczków, mimo że w tym przykładzie używamy wartość `-1` do wskazania, że wszystkie rekordy powinny być pobrane w przeciwieństwie do `Nothing`. Jest to spowodowane *`categoryID`* parametru `GetProductsByCategoryID(categoryID)` metoda oczekuje jako przekazaną wartość całkowitą, natomiast w tym samouczku parametry deklaratywne możemy zostały przekazując jako parametr wejściowy w ciągu.

Zrzut ekranu pokazuje, rysunek 14 `FilterByDropDownList.aspx` po wybraniu opcji "--Wybierz kategorię--". W tym miejscu domyślnie są wyświetlane wszystkie produkty i użytkownika można zawęzić wyświetlania, wybierając określonej kategorii.


[![Wszystkie produkty są teraz wyświetlane domyślnie](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Rysunek 14**: wszystkie produkty są teraz wyświetlane domyślnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))


## <a name="summary"></a>Podsumowanie

Podczas wyświetlania danych powiązanych hierarchicznie, często pomaga do prezentowania danych za pomocą raportów wzorzec/szczegół, z których użytkownik może uruchomić perusing dane z góry hierarchii i przechodzenie do szczegółów. W tym samouczku zbadaliśmy tworzenia raportu proste wzorzec/szczegół wyświetlanie wybranej kategorii produktów. To było wykonywane przy użyciu kontrolki DropDownList dla listy kategorii i GridView produktów należących do wybranej kategorii.

W [następnego samouczka](master-detail-filtering-with-two-dropdownlists-vb.md) przeniesiemy interfejsu DropDownList w jednym kroku dodatkowo przy użyciu dwóch kontrolek DROPDOWNLIST.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [dalej](master-detail-filtering-with-two-dropdownlists-vb.md)
