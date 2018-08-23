---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Filtrowanie rekordu głównego/szczegółów na dwóch stronach (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku przyjrzymy się korzystania z oddzielnych raportu rekordu głównego/szczegółów na dwóch stronach. Na stronie "master" używamy kontrolką elementu powtarzanego do renderowania listę categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6d4f38201eaa25097dcba589f35506204d3f73a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756607"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Filtrowanie rekordu głównego/szczegółów na dwóch stronach (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) lub [Pobierz plik PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> W tym samouczku przyjrzymy się korzystania z oddzielnych raportu rekordu głównego/szczegółów na dwóch stronach. Na stronie "główną" używamy kontrolką elementu powtarzanego do renderowania listę kategorii, po kliknięciu spowoduje przejście użytkownikowi na stronie "szczegóły", gdzie DataList dwie kolumny pokazuje tych produktów należących do wybranej kategorii.


## <a name="introduction"></a>Wprowadzenie

W [poprzedni Samouczek](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) widzieliśmy sposób wyświetlania raportów wzorzec/szczegół w pojedynczej strony sieci web za pomocą kontrolek DropDownList, aby wyświetlić rekordy "główną" i DataList, aby wyświetlić "szczegóły". Inny typowy wzorzec używany dla rekordu głównego/szczegółów raportów jest głównym rekordy na jednej stronie sieci web i szczegółowe informacje na innym. We wcześniejszych przykładach [wzorzec/szczegół filtrowania na dwóch stronach](../masterdetail/master-detail-filtering-across-two-pages-cs.md) samouczku zbadaliśmy tego wzorca, korzystający z kontrolki GridView można wyświetlić wszystkich dostawców w systemie. Ta GridView uwzględnione pole hiperłącza HyperLinkField, co czyniło jako link do drugiej strony, przekazując wzdłuż `SupplierID` w zmiennej querystring. Drugiej stronie używane GridView, aby wyświetlić listę tych produktów, dostarczone przez wybranego dostawcę.

Raporty takie dwustronicowy wzorzec/szczegół można osiągnąć za pomocą kontrolek DataList i Repeater. Jedyna różnica polega na tym, że kontrolki DataList ani powtarzanego zapewnia obsługę kontroli pole hiperłącza HyperLinkField. Zamiast tego należy możemy dodać formantu sieci Web hiperłącze lub element kotwicy HTML (`<a>`) w kontrolce `ItemTemplate`. Hiperłącze `NavigateUrl` właściwości lub zakotwiczenia `href` atrybut może następnie być dostosowywany przy użyciu podejścia deklaratywnego lub programowy.

W tym samouczku przyjrzymy się przykładowi, który wyświetla listę kategorii, na liście punktowanej na jednej stronie przy użyciu kontrolką elementu powtarzanego. Każdy element tej listy będzie zawierać nazwę i opis, kategorii z nazwą kategorii wyświetlany jako łącze do drugiej strony. Kliknięcie tego linku spowoduje whisk użytkownika do drugiej strony, gdzie kontrolką DataList pokaże tych produktów, które należą do wybranej kategorii.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Krok 1: Wyświetlanie kategorii na liście punktowanej

Pierwszym krokiem w tworzeniu dowolny raport wzorzec/szczegół jest uruchomienie przez wyświetlanie rekordów "główną". W związku z tym naszym pierwszym zadaniem jest wyświetlania kategorii na stronie "główną". Otwórz `CategoryListMaster.aspx` stronie `DataListRepeaterFiltering` folderu, Dodaj kontrolką elementu powtarzanego oraz z tagu inteligentnego, zoptymalizowany pod kątem można dodać nowego elementu ObjectDataSource. Skonfiguruj nowe kontrolki ObjectDataSource uzyskuje dostęp do swoich danych z `CategoriesBLL` klasy `GetCategories` — metoda (patrz rysunek 1).


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetCategories klasy CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Rysunek 1**: Konfigurowanie kontrolki ObjectDataSource do użycia `CategoriesBLL` klasy `GetCategories` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Następnie zdefiniuj szablony elementu powtarzanego w taki sposób, aby wyświetlał każda nazwa i opis kategorii jako element na liście punktowanej. Spróbujmy jeszcze nie martwić o każdej kategorii link do strony szczegółów. Na poniższym obrazie przedstawiono oznaczeniu deklaracyjnym dla elementu powtarzanego i kontrolki ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Z tym pełną znaczników Poświęć chwilę, aby wyświetlić postępach za pośrednictwem przeglądarki. Jak pokazano na rysunku 2, powtarzanego renderuje jako listy punktowane przedstawiający nazwę i opis każdej z tych kategorii.


[![Każda kategoria jest wyświetlany jako element listy punktowanej](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Rysunek 2**: każdej kategorii jest wyświetlany jako element listy punktowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Krok 2: Włączanie nazwa kategorii do łącza do strony szczegółów

Aby zezwolić na użytkownika, aby wyświetlić informacje "szczegóły" dla danej kategorii, należy dodać do każdej listy punktowanej elementu, po kliknięciu spowoduje przejście użytkownikowi na drugiej stronie łącza (`ProductsForCategoryDetails.aspx`). Ta druga strona będzie wyświetlana produktów dla wybranej kategorii za pomocą kontrolek DataList. W celu określenia kategorii, w których link został kliknięty, należy przekazać kliknięto kategorii `CategoryID` do drugiej strony za pośrednictwem mechanizmu. Najprostszy, najbardziej skomplikowany sposób transferu danych skalarnych z jednej strony do innej jest za pośrednictwem ciąg zapytania, które opcji, których będziemy używać w ramach tego samouczka. W szczególności `ProductsForCategoryDetails.aspx` strony będą oczekiwać wybranego *`categoryID`* wartości, które mają być przekazane za pośrednictwem pole querystring o nazwie `CategoryID`. Na przykład, aby wyświetlić produktów do kategorii Beverages mającego `CategoryID` 1, użytkownik może odwiedzić `ProductsForCategoryDetails.aspx?CategoryID=1`.

Aby utworzyć hiperłącze dla każdego elementu listy punktowanej w elemencie powtarzanym musimy dodać formant Web hiperłącze lub element kotwicy HTML (`<a>`) do `ItemTemplate`. W scenariuszach, gdzie hiperłącze jest wyświetlane takie same dla każdego wiersza, każda z tych metod będzie wystarczająca. Aby uzyskać wzmacniaki wolę za pomocą elementu zakotwiczenia. Aby użyć elementu zakotwiczenia, zaktualizuj ItemTemplate elementu powtarzanego do:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Należy pamiętać, że `CategoryID` może wprowadzone bezpośrednio z poziomu elementu zakotwiczenia `href` atrybutu; jednak aby więc upewnij się, ograniczone czasowo `href` wartość atrybutu z apostrofy (i Uwaga cudzysłów), ponieważ `Eval` — metoda w ramach `href` atrybut rozgranicza jego ciągu (`"CategoryID"`) ze znakami cudzysłowu. Alternatywnie formantu sieci Web hiperłącza mogą być używane zamiast tego:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Uwaga jak statycznych część adresu URL — `ProductsForCategoryDetails.aspx?CategoryID` — jest dołączany do wyniku `Eval("CategoryID")` bezpośrednio z poziomu składnia wiązania z danymi za pomocą ciągów.

Jedną z zalet za pomocą kontrolki Hiperlinku jest, że programowego uzyskiwania z elementu powtarzanego `ItemDataBound` program obsługi zdarzeń, jeśli to konieczne. Na przykład można wyświetlić nazwę kategorii, jako tekst, a nie jako link do kategorii produktów skojarzone. Takie można programowo sprawdzić w `ItemDataBound` programu obsługi zdarzeń; dla kategorii bez skojarzonych produktów, hiperłącze firmy `NavigateUrl` można ustawić właściwości pusty ciąg, powodując wpisywanych określonej kategorii Renderowanie jako zwykły tekst (a nie jako link). Odwołaj się do [formatowanie elementów DataList i Repeater na podstawie od danych](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) samouczka, aby uzyskać więcej informacji na temat formatowanie elementów DataList i Repeater na zawartość w oparciu logiki programowej za pośrednictwem `ItemDataBound` programu obsługi zdarzeń.

Jeśli wykonujesz możesz używać elementu zakotwiczenia albo podejście kontroli hiperłącze na stronie. Niezależnie od tego podejścia, podczas wyświetlania strony przy użyciu nazwy kategorii ma być renderowany jako link do przeglądarki `ProductsForCategoryDetails.aspx`, przekazując odpowiednie `CategoryID` wartość (patrz rysunek 3).


[![Nazwy kategorii teraz połączyć się z ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Rysunek 3**: Kategoria nazwy teraz Link do `ProductsForCategoryDetails.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Krok 3: Wyświetlanie listy produktów, które należą do wybranej kategorii

Za pomocą `CategoryListMaster.aspx` strony ukończone, możemy przystąpić do naszej uwagi do wdrażania na stronie "szczegóły" Włącz `ProductsForCategoryDetails.aspx`. Otwórz tę stronę, przeciągnij kontrolką DataList z przybornika do projektanta i ustawić jej `ID` właściwość `ProductsInCategory`. Następnie wybierz z tagu inteligentnego DataList można dodać nowego elementu ObjectDataSource do strony, nadając mu nazwę `ProductsInCategoryDataSource`. Skonfiguruj ją w taki sposób wywoływanych przez nią `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody; zestaw z listy rozwijanej listy na kartach INSERT, UPDATE i DELETE (Brak).


[![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsByCategoryID(categoryID) klasy ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Rysunek 4**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Ponieważ `GetProductsByCategoryID(categoryID)` metoda akceptuje parametr wejściowy (*`categoryID`*), Kreator wybierz źródło danych umożliwia nam określić źródło parametru. Ustaw źródło parametru QueryString przy użyciu vlastnost QueryStringField `CategoryID`.


[![Używanie CategoryID pole Querystring jako parametru źródła](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Rysunek 5**: pole Querystring służy `CategoryID` jako źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Jak widzieliśmy w poprzednich samouczkach po zakończeniu pracy kreatora wybierz źródło danych, program Visual Studio automatycznie tworzy `ItemTemplate` dla kontrolki DataList zawierającego każdego dane nazwy i wartości pola. Zamień ten szablon, który wyświetla listę tylko w produkcie nazwę dostawcy i ceny. Ponadto należy ustawić DataList `RepeatColumns` właściwość 2. Po wprowadzeniu tych zmian swoje DataList i ObjectDataSource oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Aby wyświetlić tą stronę w działaniu, zacznij od `CategoryListMaster.aspx` stronie; następnie kliknąć łącze na liście kategorii. To spowoduje przejście do `ProductsForCategoryDetails.aspx`, przekazując wzdłuż `CategoryID` za pośrednictwem ciąg zapytania. `ProductsInCategoryDataSource` ObjectDataSource w `ProductsForCategoryDetails.aspx` zostanie następnie uzyskać tylko tych produktów dla określonej kategorii i wyświetlaj je w DataList, która renderuje dwa produkty każdego wiersza. Rysunek 6 przedstawia zrzut ekranu przedstawiający `ProductsForCategoryDetails.aspx` podczas wyświetlania Beverages.


[![Beverages są wyświetlane, dwa na jeden wiersz](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Rysunek 6**: Beverages są wyświetlane, dwa na jeden wiersz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Krok 4: Wyświetlanie informacji o kategorii na ProductsForCategoryDetails.aspx

Gdy użytkownik kliknie kategorii w `CategoryListMaster.aspx`, przekierowanie do `ProductsForCategoryDetails.aspx` i wyświetlane produkty, które należą do wybranej kategorii. Jednak w `ProductsForCategoryDetails.aspx` nie ma żadnych wizualnych do jakiej kategorii został wybrany. Użytkownik, który jest przeznaczony do kliknij Beverages, ale przypadkowo kliknięto Condiments nie ma możliwości realizacji ich błędu, gdy osiągną oni limit `ProductsForCategoryDetails.aspx`. Aby złagodzić to potencjalny problem, można też wyświetlać informacje o wybranej kategorii — jego nazwa i opis — u góry `ProductsForCategoryDetails.aspx` strony.

Aby to osiągnąć, należy dodać FormView powyżej kontrolce elementu powtarzanego w `ProductsForCategoryDetails.aspx`. Następnie dodaj do strony nowego elementu ObjectDataSource z tagu inteligentnego FormView o nazwie `CategoryDataSource` i skonfigurować go do używania `CategoriesBLL` klasy `GetCategoryByCategoryID(categoryID)` metody.


[![Uzyskiwanie dostępu do informacji o kategorii za pośrednictwem metody GetCategoryByCategoryID(categoryID) klasy CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Rysunek 7**: uzyskiwanie dostępu do informacji o kategorii za pośrednictwem `CategoriesBLL` klasy `GetCategoryByCategoryID(categoryID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Podobnie jak w przypadku `ProductsInCategoryDataSource` ObjectDataSource dodanych w kroku 3, `CategoryDataSource`firmy skonfigurować źródło danych monituje Kreator nam źródło `GetCategoryByCategoryID(categoryID)` metoda dane wejściowe podane przez parametr. Użyj dokładnie tych samych ustawień, ustawienie dla źródła parametru QueryString i wartość vlastnost QueryStringField `CategoryID` (odnoszą się do rysunek 5).

Po zakończeniu działania kreatora, program Visual Studio automatycznie tworzy `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate` dla widoku FormView. Ponieważ udostępniamy interfejsu tylko do odczytu, możesz usunąć `EditItemTemplate` i `InsertItemTemplate`. Ponadto możesz dostosować FormView `ItemTemplate`. Po usuwanie zbędnym szablonów i dostosowywanie właściwości ItemTemplate, Twoje FormView i ObjectDataSource oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Rysunek 8 przedstawia ekranu zrzut podczas wyświetlania tej strony za pośrednictwem przeglądarki.

> [!NOTE]
> Oprócz FormView, również zostały dodane kontrolki Hiperlinku powyżej FormView prowadzące użytkownika do listy kategorii (`CategoryListMaster.aspx`). Możesz umieścić ten link w innym miejscu lub całkowicie z niego zrezygnować.


[![Informacje o kategorii jest teraz wyświetlane u góry strony](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Rysunek 8**: informacje o kategorii jest teraz wyświetlany w górnej części strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Krok 5: Wyświetlenie komunikatu, jeśli produkty nie należą do wybranej kategorii

`CategoryListMaster.aspx` Strona zawiera listę wszystkich kategorii w systemie, niezależnie od tego czy istnieją skojarzone produkty. Jeśli użytkownik kliknie kategorię nie skojarzone produkty DataList w `ProductsForCategoryDetails.aspx` nie będą renderowane, ponieważ źródło danych nie będzie żadnych elementów. Jak widzieliśmy w poprzednich samouczkach widoku GridView zapewnia `EmptyDataText` właściwość, która może służyć do określania wiadomość SMS, wyświetlany w sytuacji, gdy nie ma żadnych rekordów w jego źródle danych. Niestety DataList ani elementu powtarzanego ma taką właściwość.

W celu wyświetlenia komunikatu, informujący użytkownika, że brak pasującego produktów dla wybranej kategorii, należy dodać etykietę formant, do strony którego `Text` właściwość jest przypisana widomość do wyświetlenia w przypadku, gdy brak pasującego produktów. Następnie należy programowo ustawić jej `Visible` na informację, czy w kontrolce DataList zawiera jakiekolwiek elementy na podstawie właściwości.

W tym celu najpierw Dodaj etykietę pod kontrolki DataList. Ustaw jego `ID` właściwości `NoProductsMessage` i jego `Text` właściwość "Brak produktów dla wybranej kategorii..." Następnie należy programowo ustawić tę etykietę `Visible` właściwość oparte na informację, czy wszystkie dane był powiązany z `ProductsInCategory` DataList. To przypisanie musi nastąpić po danych została powiązana z kontrolki DataList. Dla DetailsView, GridView i FormView możemy utworzyć program obsługi zdarzeń dla formantu `DataBound` zdarzenie, które są generowane po zakończeniu wiązania danych. Jednak w elemencie DataList ani powtarzanego posiada `DataBound` zdarzenia dostępne.

W tym przykładzie firma Microsoft może przypisać etykietę `Visible` właściwość `Page_Load` programu obsługi zdarzeń, ponieważ dane zostanie przypisany do kontrolki DataList przed strony `Load` zdarzeń. Jednak to podejście nie będzie działać w przypadku ogólnych, jak dane z kontrolki ObjectDataSource może być powiązane z DataList w dalszej części cyklu życia strony. Na przykład, jeśli dane wyświetlane opiera się na wartości w innej kontrolce, takie jak podczas wyświetlania raportu wzorzec/szczegół za pomocą kontrolki DropDownList do przechowywania rekordów "główną", dane mogą nie odbitych do formantu sieci Web danych, aż do `PreRender` etapie cykl życia strony.

Jedno rozwiązanie, które będzie działać dla wszystkich przypadków jest przypisanie `Visible` właściwości `False` w DataList `ItemDataBound` (lub `ItemCreated`) program obsługi zdarzeń podczas tworzenia powiązania z typem elementu `Item` lub `AlternatingItem`. W takim przypadku wiemy, że nie ma danych co najmniej jeden element w źródle danych i w związku z tym można ukryć `NoProductsMessage` etykiety. Oprócz tej obsługi zdarzeń również potrzebujemy program obsługi zdarzeń do DataList `DataBinding` zdarzeń, w którym możemy zainicjować etykiety `Visible` właściwość `True`. Ponieważ `DataBinding` zdarzeń, który jest uruchamiany przed `ItemDataBound` zdarzenia, etykieta firmy `Visible` początkowo będzie można ustawić właściwości `True`; w przypadku jakichkolwiek elementów danych, jednak zostanie ustawiona `False`. Poniższy kod implementuje tę logikę:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Wszystkie kategorie w bazie danych Northwind są skojarzone z co najmniej jednego produktu. Aby przetestować tę funkcję, czy mogę ręcznie dostosowaniu bazy danych Northwind w tym samouczku ponowne przypisywanie wszystkich produktów skojarzone z kategorii produktu (`CategoryID` = 7) do kategorii owoce morza (`CategoryID` = 8). Można to zrobić za pomocą Eksploratora serwera, wybierając nową kwerendę i za pomocą następujących `UPDATE` instrukcji:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Po zaktualizowaniu bazy danych w związku z tym, wróć do `CategoryListMaster.aspx` strony, a następnie kliknij łącze produktu. Ponieważ nie ma już wszystkie produkty należące do kategorii produktu, wyświetlony komunikat "Brak produktów dla wybranej kategorii...", jak pokazano na rysunku 9.


[![Zostanie wyświetlony komunikat w przypadku nr produkty należące do wybranej kategorii](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Rysunek 9**: wyświetlany jest komunikat, jeśli nie produkty należące do wybranej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Podsumowanie

Gdy wzorzec/szczegół raporty można wyświetlić zarówno węzła głównego, jak i szczegóły rekordów na jednej stronie, w wielu witryn sieci Web one są rozdzielone na dwóch stronach sieci web. W tym samouczku zobaczyliśmy, jak wdrożyć raport wzorzec/szczegół za posiadające kategorie wymienione na liście punktowanej przy użyciu Repeater na stronie sieci web "główną" i skojarzone produkty wymienione na stronie "szczegóły". Każdy element tej listy na głównej stronie sieci web zawiera łącze do strony szczegółów, który jest przekazywany z wiersza `CategoryID` wartość.

Na stronie szczegółów pobierania tych produktów dla określonego dostawcy było wykonywane za pośrednictwem `ProductsBLL` klasy `GetProductsByCategoryID(categoryID)` metody. *`categoryID`* Wartość parametru została określona w sposób deklaratywny przy użyciu `CategoryID` wartości querystring jako źródło parametru. Zobaczyliśmy także sposób wyświetlania szczegółów kategorii na stronie szczegółów przy użyciu kontrolce FormView oraz wyświetlić komunikat, jeśli nie było żadnych produktów należących do wybranej kategorii.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Zack Jones i Liz Shulok. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [dalej](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
