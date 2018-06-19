---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Wzorzec/szczegół filtrowania przez dwie strony (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W ramach tego samouczka przyjrzymy się oddzielającego raportu wzorzec/szczegół między dwoma stronami. Na stronie "master" używamy kontrolce elementu powtarzanego do renderowania listę categ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6a3783175218438f2a9f735c3861c56e039a248e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887099"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Wzorzec/szczegół filtrowania przez dwie strony (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) lub [pobierania plików PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> W ramach tego samouczka przyjrzymy się oddzielającego raportu wzorzec/szczegół między dwoma stronami. Na stronie "główny" używamy kontrolce elementu powtarzanego do renderowania listy kategorii, gdy kliknięto, potrwa użytkownika na stronie "szczegóły", gdzie DataList dwie kolumny zawiera tych produktów należących do wybranej kategorii.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) widzieliśmy sposób wyświetlania raportów wzorzec/szczegół w jednej strony sieci web przy użyciu DropDownLists do wyświetlania rekordów "główny" i DataList do wyświetlenia "szczegóły". Inny wspólnego wzorca używane w raportach wzorzec/szczegół jest głównym rekordów na jednej stronie sieci web i szczegóły na innym. W starszych [wzorzec/szczegół filtrowania między dwoma stronami](../masterdetail/master-detail-filtering-across-two-pages-cs.md) samouczek, możemy zbadać tego wzorca przy użyciu Element GridView w celu wyświetlenia wszystkich dostawców w systemie. Ten element GridView uwzględnione pole hiperłącza HyperLinkField, który odwzorowany jako łącze do drugiej strony, przekazywanie wzdłuż `SupplierID` w zmiennej querystring. Druga strona używany element GridView do tworzenia listy produktów dostarczone przez wybranego dostawcę.

Raporty takie dwustronicowy wzorzec/szczegół można osiągnąć za pomocą formantów DataList i elementu powtarzanego. Jedyna różnica polega na tym, że elementu DataList ani powtarzanego zapewnia obsługę dla formantu pole hiperłącza HyperLinkField. Zamiast tego, firma Microsoft lub należy dodać formant sieci Web hiperłącza element kotwicy HTML (`<a>`) w formancie `ItemTemplate`. HyperLink `NavigateUrl` właściwości lub zakotwiczenia `href` atrybut można następnie dostosować przy użyciu podejścia deklaratywne lub programowe.

W tym samouczku firma Microsoft będzie zapoznać się przykładem, który zawiera listę kategorii na liście punktowanej na jednej stronie przy użyciu kontrolce elementu powtarzanego. Każdy element tej listy będzie zawierać nazwę i opis, kategoria o nazwie kategorii wyświetlany jako łącze do drugiej strony. Kliknięcie tego łącza spowoduje whisk użytkownika do drugiej strony, gdzie DataList wyświetli te produkty, które należą do wybranej kategorii.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Krok 1: Wyświetlanie kategorii na liście punktowanej

Pierwszym krokiem tworzenia żadnych raportów wzorzec/szczegół jest można uruchomić za pomocą wyświetlania rekordów "master". W związku z tym naszym pierwszym zadaniem jest wyświetlone kategorii na stronie "master". Otwórz `CategoryListMaster.aspx` strony `DataListRepeaterFiltering` , Dodaj kontrolce elementu powtarzanego i, z tagu inteligentnego, należy wybrać opcję Dodaj nowy element ObjectDataSource. Skonfiguruj nowy element ObjectDataSource, dzięki czemu uzyskuje dostęp do danych z `CategoriesBLL` klasy `GetCategories` — metoda (zobacz rysunek 1).


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetCategories klasy CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Rysunek 1**: Konfigurowanie ObjectDataSource użyć `CategoriesBLL` klasy `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Następnie określ elemencie powtarzanym szablony w taki sposób, że wyświetlane jako element na liście punktowanej każdej kategorii, nazwę i opis. Teraz nie została jeszcze martwić o każdej kategorii łącze do strony szczegółów. Poniżej przedstawiono znaczników deklaratywne dla elementu powtarzanego i ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Z tego znacznika pełną Poświęć chwilę, aby wyświetlić postęp naszych za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, powtarzanego renderuje jako listy punktowane przedstawiający nazwę i opis każdej kategorii.


[![Każda kategoria jest wyświetlany jako element listy punktowanej](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Rysunek 2**: każdej kategorii jest wyświetlany jako element listy punktowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Krok 2: Włączanie nazwę kategorii w łącze do strony szczegółów

Aby zezwolić użytkownikowi na wyświetlanie informacji "szczegóły" dla danej kategorii, należy dodać do każdej listy punktowanej elementu, gdy kliknięty, potrwa użytkownikowi na drugiej stronie łącza (`ProductsForCategoryDetails.aspx`). Ta druga strona będzie wyświetlana produktów dla wybranej kategorii przy użyciu elementu DataList. W celu określenia kategorii, którego łącze został kliknięty, należy przekazać klikniętej kategorii `CategoryID` do drugiej strony za pomocą mechanizmu. Najprostszy, najbardziej oczywistym sposobem transferu danych skalarnych z jednej strony do innego jest za pośrednictwem querystring, czyli opcji, które będą używane w tym samouczku. W szczególności `ProductsForCategoryDetails.aspx` strony będą oczekiwać wybranego *`categoryID`* wartości przekazywane querystring pole o nazwie `CategoryID`. Na przykład, aby wyświetlić produkty należące do tej kategorii, który ma `CategoryID` 1, użytkownik może odwiedzić `ProductsForCategoryDetails.aspx?CategoryID=1`.

Aby utworzyć hiperłącze dla każdego elementu listy punktowanej w elemencie powtarzanym musimy albo Dodaj formant sieci HyperLink Web lub element kotwicy HTML (`<a>`) do `ItemTemplate`. W zastosowaniach hiperłącze wyświetlane takie same dla każdego wiersza, wystarczy albo podejście. Dla wzmacniaki wolę za pomocą elementu zakotwiczenia. Aby użyć elementu zakotwiczenia, zaktualizuj ItemTemplate elemencie powtarzanym do:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Należy pamiętać, że `CategoryID` mogą zostać dodane bezpośrednio z poziomu elementu zakotwiczenia `href` atrybutu; jednak, aby tak Zadbaj ograniczyć `href` wartość atrybutu z apostrofów (i Uwaga cudzysłów), ponieważ `Eval` — metoda w ramach `href` atrybutu rozgranicza parametrach (`"CategoryID"`) w znaki cudzysłowu. Alternatywnie formant sieci Web hiperłącze można zamiast tego:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Uwaga jak statyczny część adresu URL — `ProductsForCategoryDetails.aspx?CategoryID` — jest dołączany do wyniku `Eval("CategoryID")` bezpośrednio z poziomu składnia wiązania z danymi przy użyciu ciągów.

Stosując kontrolki Hiperlinku jest, że można programowo dostęp do niej w elemencie powtarzanym `ItemDataBound` program obsługi zdarzeń, jeśli to konieczne. Na przykład można wyświetlić nazwę kategorii jako tekst, a nie jako łącze dla kategorii z ma skojarzone produktów. Tę operację można było wykonać programowo w `ItemDataBound` obsługi zdarzeń; dla kategorii bez skojarzone produktów, hiperłącze w `NavigateUrl` na pusty ciąg, powodując tę nazwę konkretnej kategorii można ustawić właściwości Renderowanie jako zwykły tekst (a nie jako łącze). Odwołaj się do [formatowania DataList i powtarzanego oparte na danych](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) samouczek, aby uzyskać więcej informacji na formatowanie zawartości DataList i jego elementu powtarzanego oparte na logiki za pośrednictwem `ItemDataBound` programu obsługi zdarzeń.

Jeśli wykonujesz, możesz zająć się za pomocą elementu zakotwiczenia lub sposobów kontroli hiperłącze na stronie. Niezależnie od tego podejścia, podczas wyświetlania strony przy użyciu nazwy kategorii ma być renderowany jako łącze do przeglądarki `ProductsForCategoryDetails.aspx`, przekazując odpowiednie `CategoryID` wartość (patrz rysunek 3).


[![Nazwy kategorii teraz łącze do ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Rysunek 3**: Kategoria nazwy teraz łącze do `ProductsForCategoryDetails.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Krok 3: Listę produktów, które należą do wybranej kategorii

Z `CategoryListMaster.aspx` strona kompletna, zdecydowaliśmy włączyć wymagające uwagi do wykonania na stronie "szczegóły" `ProductsForCategoryDetails.aspx`. Otwórz tę stronę, przeciągnij z przybornika do projektanta DataList i ustawić jej `ID` właściwości `ProductsInCategory`. Następnie wybierz z DataList tagów inteligentnych można dodać nowego elementu ObjectDataSource do strony, nadając mu nazwę `ProductsInCategoryDataSource`. Skonfiguruj ją tak, aby wywoływał `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metod; Ustaw listy rozwijanej listy w kartach INSERT, UPDATE i DELETE na (Brak).


[![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductsByCategoryID(categoryID) klasy ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Rysunek 4**: Konfigurowanie ObjectDataSource użyć `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Ponieważ `GetProductsByCategoryID(categoryID)` metoda przyjmuje parametr wejściowy (*`categoryID`*), Kreator wybierz źródło danych umożliwia nam określone źródło wartości parametru. Ustaw źródło parametru QueryString za pomocą pole QueryStringField `CategoryID`.


[![Użyj pola Querystring CategoryID jako źródło wartości parametru](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Rysunek 5**: pole Querystring służy `CategoryID` jako źródło wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Jak możemy opisane w poprzedniej samouczki, po zakończeniu pracy kreatora wybierz źródło danych programu Visual Studio automatycznie tworzy `ItemTemplate` dla elementu DataList listę poszczególnych nazwę pola danych i wartości. Zamień ten szablon, który wyświetla tylko produktu nazwy, dostawcy i cenę. Ponadto należy ustawić DataList `RepeatColumns` właściwości do 2. Po wprowadzeniu tych zmian DataList, a w elemencie ObjectDataSource znaczników deklaratywne powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Aby wyświetlić tą stronę w akcji, Rozpocznij od `CategoryListMaster.aspx` strony; obok, kliknij łącze na liście kategorii. W ten sposób spowoduje przejście do `ProductsForCategoryDetails.aspx`, przechodzącą wzdłuż `CategoryID` przez ciąg zapytania. `ProductsInCategoryDataSource` ObjectDataSource w `ProductsForCategoryDetails.aspx` będą pobrania tylko tych produktów dla określonej kategorii i wyświetlić je w DataList, która renderuje dwóch produktów dla każdego wiersza. Rysunek 6 przedstawia zrzut ekranu przedstawiający `ProductsForCategoryDetails.aspx` podczas wyświetlania napoje.


[![Napoje są wyświetlane dwa na jeden wiersz](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Rysunek 6**: napoje są wyświetlane dwa na jeden wiersz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Krok 4: Wyświetlane informacje o kategorii na ProductsForCategoryDetails.aspx

Gdy użytkownik kliknie ikonę kategorii w `CategoryListMaster.aspx`, podjęte w celu `ProductsForCategoryDetails.aspx` i wyświetlane produkty, które należą do wybranej kategorii. Jednak w `ProductsForCategoryDetails.aspx` nie ma żadnych wizualnych określające, jakie kategorii został wybrany. Użytkownik, który ma kliknij napoje, ale przypadkowo klikniętej przyprawy, nie ma możliwości programu realizująca ich błąd, po upływie `ProductsForCategoryDetails.aspx`. Celu rozwiązanie tego problemu, można wyświetlić informacji na temat wybranej kategorii — nazwy i opisu — w górnej części `ProductsForCategoryDetails.aspx` strony.

W tym celu Dodaj FormView powyżej w kontrolce elementu powtarzanego `ProductsForCategoryDetails.aspx`. Następnie dodaj nowy element ObjectDataSource do strony z FormView tagów inteligentnych o nazwie `CategoryDataSource` i skonfigurować go do używania `CategoriesBLL` klasy `GetCategoryByCategoryID(categoryID)` metody.


[![Dostęp do informacji o kategorii za pomocą metody GetCategoryByCategoryID(categoryID) klasy CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Rysunek 7**: dostęp do informacji o kategorii za pośrednictwem `CategoriesBLL` klasy `GetCategoryByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Za pomocą `ProductsInCategoryDataSource` ObjectDataSource dodanej w kroku 3 `CategoryDataSource`przez konfigurowanie źródła danych, Kreator wyświetli monit nam źródła dla `GetCategoryByCategoryID(categoryID)` wejściowych parametru metody. Użyj takich samych ustawień jak wcześniej, ustawienie źródło parametru na ciąg zapytania i wartość pole QueryStringField `CategoryID` (odwołują się do rysunek 5).

Po zakończeniu działania kreatora, program Visual Studio automatycznie tworzy `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate` dla widoku FormView. Ponieważ udostępniamy interfejsu tylko do odczytu, możesz usunąć `EditItemTemplate` i `InsertItemTemplate`. Ponadto możesz dostosowywać FormView `ItemTemplate`. Po usunięciu zbędny szablony i dostosowywanie właściwości ItemTemplate, FormView, a w elemencie ObjectDataSource znaczników deklaratywne powinien wyglądać podobny do następującego:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Rysunek nr 8 przedstawia zrzut podczas wyświetlania tej strony za pośrednictwem przeglądarki ekranu.

> [!NOTE]
> Oprócz FormView, zostały dodane również kontrolki Hiperlinku powyżej FormView prowadzące użytkownika do listy kategorii (`CategoryListMaster.aspx`). Możesz umieścić w innym miejscu tego łącza lub pominąć go całkowicie.


[![Informacje o kategorii jest teraz wyświetlany w górnej części strony](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Rysunek 8**: informacje o kategorii jest teraz wyświetlany w górnej części strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Krok 5: Wyświetlenie komunikatu, jeśli produkty nie należą do wybranej kategorii

`CategoryListMaster.aspx` Strona zawiera listę wszystkich kategorii w systemie, bez względu na to czy istnieją skojarzone produktów. Jeśli użytkownik kliknie kategorię ma skojarzone produktów DataList w `ProductsForCategoryDetails.aspx` nie będą renderowane, jako źródła danych nie ma żadnych elementów. Jak możemy przedstawiono w ciągu ostatnich samouczkach, zapewnia widoku GridView `EmptyDataText` właściwość, która może służyć do określenia wiadomość SMS, aby wyświetlić, jeśli nie ma żadnych rekordów w jego źródle danych. Niestety DataList ani elementu powtarzanego nie posiada tych właściwości.

Aby wyświetlić komunikat informujący użytkownika, że brak pasujących produktów dla wybranej kategorii, należy dodać etykietę kontroli do strony którego `Text` właściwości przypisano komunikat wyświetlany w przypadku, gdy brak pasującego produktów. Następnie należy ustawić programowo jego `Visible` na czy elementu DataList zawiera wszystkie elementy na podstawie właściwości.

Aby to zrobić, Rozpocznij od dodania etykiety poniżej elementu DataList. Ustaw jego `ID` właściwości `NoProductsMessage` i jego `Text` dla właściwości "Brak produktów dla wybranej kategorii..." Następnie należy programowo ustawić tę etykietę `Visible` na podstawie właściwości na czy żadnych danych została powiązana z `ProductsInCategory` DataList. Musi być przydziału po danych została powiązana z elementu DataList. Dla widoku GridView, widoku DetailsView i FormView, można utworzyć programu obsługi zdarzeń dla formantu `DataBound` zdarzenie, które są generowane po zakończeniu wiązania z danymi. Jednak elementu DataList ani powtarzanego nie posiada `DataBound` zdarzeń dostępne.

W tym przykładzie firma Microsoft może przypisać etykietę `Visible` właściwości w `Page_Load` program obsługi zdarzeń, ponieważ dane zostanie przypisana do elementu DataList przed strony `Load` zdarzeń. Jednak ta metoda nie będzie działało w przypadku ogólnych, jak dane z ObjectDataSource może być powiązane z DataList później w cyklu życia strony. Na przykład, jeśli dane wyświetlane opiera się na wartość innego formantu, takie jak jest podczas wyświetlania raportu wzorzec/szczegół za pomocą DropDownList do przechowywania rekordów "główny", dane mogą nie odbitych do formantu danych sieci Web do `PreRender` etapie cykl życia strony.

Jedno rozwiązanie, które będzie działać we wszystkich przypadkach jest przypisanie `Visible` właściwości `False` w DataList `ItemDataBound` (lub `ItemCreated`) program obsługi zdarzeń podczas wiązania z typem elementu `Item` lub `AlternatingItem`. W takim przypadku wiemy, że nie ma danych co najmniej jeden element w źródle danych i w związku z tym można ukryć `NoProductsMessage` etykiety. Oprócz tej obsługi zdarzeń dla DataList potrzebujemy jest także program obsługi zdarzeń `DataBinding` zdarzeń, w którym możemy zainicjować etykiety `Visible` właściwości `True`. Ponieważ `DataBinding` zdarzenia generowane przed `ItemDataBound` zdarzenia, etykiety w `Visible` właściwości początkowo zostanie ustawiona do `True`; Jeśli istnieją wszystkie elementy danych, jednak zostanie ustawiona `False`. Poniższy kod implementuje tę logikę:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Wszystkie kategorie w bazie danych Northwind są skojarzone z jednego lub więcej produktów. Do testowania tej funkcji, można ręcznie dostosowaniu bazy danych Northwind w tym samouczku, ponowne przypisywanie wszystkich produktów skojarzonych z kategorii produktu (`CategoryID` = 7) do kategorii ryby (`CategoryID` = 8). Można to zrobić z Eksploratora serwera przez wybranie nowego zapytania i użycie następujących `UPDATE` instrukcji:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Po zaktualizowaniu odpowiednio bazy danych, wróć do `CategoryListMaster.aspx` strony i kliknij łącze produktu. Ponieważ nie ma już żadnych produktów należących do tej kategorii produktów, powinna zostać wyświetlona "Brak produktów dla wybranej kategorii..." komunikat, jak pokazano na rysunku 9.


[![Zostanie wyświetlony komunikat, jeśli istnieją nr produktów należących do wybranej kategorii](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Rysunek 9**: komunikat jest wyświetlany w przypadku nr produktów należących do wybranej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Podsumowanie

Gdy wzorzec/szczegół raporty można wyświetlić głównego i szczegółów rekordy na jednej stronie, w wiele witryn sieci Web one są rozdzielone przez dwie strony sieci web. W tym samouczku analizujemy sposobu wdrażania raportu wzorzec/szczegół za kategorii wymienionych na liście punktowanej przy użyciu elementu powtarzanego "główny" strony sieci web i skojarzone wymienionego na stronie "szczegóły". Każdy element tej listy na stronie głównej sieci web zawiera łącze do strony szczegółów, który przekazał wzdłuż wiersza `CategoryID` wartość.

Na stronie szczegółów pobierania tych produktów dla określonego dostawcy zostało wykonane za pomocą `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody. *`categoryID`* Określono wartość parametru deklaratywnie przy użyciu `CategoryID` wartości querystring jako źródło parametru. Również Analizujemy sposób wyświetlania szczegółów kategorii na stronie szczegółów za pomocą FormView i wyświetli komunikat, jeśli nie było żadnych produktów należących do wybranej kategorii.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Kowalski Zack i Liz Shulok. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [dalej](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
