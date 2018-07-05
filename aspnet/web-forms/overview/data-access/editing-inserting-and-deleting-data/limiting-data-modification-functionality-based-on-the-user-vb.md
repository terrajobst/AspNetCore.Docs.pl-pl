---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Ograniczanie funkcji modyfikacji danych na podstawie użytkownika (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W aplikacji sieci web, która umożliwia użytkownikom edytowanie danych konta innego użytkownika mogą mieć różne uprawnienia do edycji danych. W tym samouczku zajmiemy się jak t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: 8530dfd37570ed5fcc45ef4d6c30750c5d4d2152
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377119"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Ograniczanie funkcji modyfikacji danych na podstawie użytkownika (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) lub [Pobierz plik PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> W aplikacji sieci web, która umożliwia użytkownikom edytowanie danych konta innego użytkownika mogą mieć różne uprawnienia do edycji danych. W tym samouczku zajmiemy się, jak dynamicznie zmieniać funkcji modyfikacji danych, które są oparte na odwiedzającego użytkownika.


## <a name="introduction"></a>Wprowadzenie

Liczba aplikacji sieci web obsługuje konta użytkowników i zapewniają różne opcje, raportów i funkcji na podstawie zalogowanego użytkownika. Na przykład korzystając z samouczków może chcemy umożliwić użytkownikom pochodzących od firm dostawcy zalogować się do witryny i zaktualizuj informacje ogólne dotyczące ich produkty — nazwy użytkownika i ilość na jednostkę, prawdopodobnie — wraz z informacjami o dostawcy, takie jak jego nazwa firmy adres, informacje kontaktowe osoby s i tak dalej. Ponadto firma Microsoft ma zawierają niektóre konta użytkowników dla osób z naszej firmy, tak aby zalogować się i zaktualizuj informacje o produkcie, takich jak jednostki w magazynie, minimum i tak dalej. Nasza aplikacja sieci web może również zezwolić anonimowym użytkownikom na odwiedź (osób, które nie logowali), ale będzie je ograniczona jedynie do wyświetlania danych. Za pomocą takiego użytkownika konta systemu w miejscu czy chcemy danych kontrolki sieci Web w naszych stron ASP.NET do zaoferowania, wstawianie, edytowanie i usuwanie możliwości odpowiednie dla obecnie zalogowanego użytkownika.

W tym samouczku zajmiemy się, jak dynamicznie zmieniać funkcji modyfikacji danych, które są oparte na odwiedzającego użytkownika. W szczególności utworzymy strona, która wyświetla informacje o dostawcy w edytowalne DetailsView wraz z GridView, który zawiera listę produktów, dostarczone przez dostawcę. W przypadku użytkowników, odwiedzając stronę z naszej firmy, mogą oni: wyświetlanie wszystkich informacji o dostawcy s; Edytowanie adresu; i zmodyfikowania tych informacji dla każdego produktu, dostarczone przez dostawcę. Jeśli jednak użytkownik znajduje się w określonej firmy, mogą tylko wyświetlać i edytować własne informacje o adresie i można edytować tylko ich produkty, które nie zostały oznaczone jako wycofane.


[![Użytkownik z naszej firmy może edytować wszystkie informacje o dostawcach s](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Rysunek 1**: użytkownik z nasze firmy można edytować dowolnego dostawcy s informacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Użytkownik z określonego dostawcy mogą tylko wyświetlanie i edytowanie swoich informacji](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Rysunek 2**: użytkownik z określonego dostawcy mogą tylko wyświetlanie i edytowanie swoje informacje ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Rozpocznij pracę dzięki s!

> [!NOTE]
> Program ASP.NET 2.0 systemu członkostwa s oferuje standardowe, rozszerzalna platforma do tworzenia, zarządzania i sprawdzanie poprawności kont użytkownika. Ponieważ badanie systemu członkostwa wykracza poza zakres tego samouczka, w tym samouczku zamiast tego "elementów sztucznych" członkostwa, umożliwiając anonimowe osoby odwiedzające wybierz, czy są one od określonego dostawcy lub z naszej firmy. Aby uzyskać więcej informacji o członkostwo, dotyczą Moje [s badanie ASP.NET 2.0 członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) seria artykułów.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Krok 1: Zezwalanie użytkownikowi na określenie praw dostępu

W aplikacji sieci web rzeczywistych informacje o koncie s użytkownika obejmuje czy jego pracownicy współpracowali dla firmy lub dla określonego dostawcy, a informacje te będą dostępne z naszej strony ASP.NET po użytkownik zalogował się do witryny. Te informacje można przechwytywać za pośrednictwem systemu ról programu ASP.NET 2.0 s, jak informacje o koncie użytkownika za pośrednictwem systemu profilu lub za pomocą metod niestandardowego.

Ponieważ celem tego samouczka jest zademonstrowanie, dostosowując możliwości modyfikacji danych na podstawie zalogowanego użytkownika i nie jest przeznaczona do pokaz ASP.NET 2.0 s członkostwo, role i systemów profilu, użyjemy mechanizm bardzo prosty sposób, aby określić, funkcje dla użytkownika, odwiedzając stronę - DropDownList, z którego użytkownik może wskazać, jeśli należy je wyświetlać i edytować informacje o dostawcy lub alternatywnie, co informacje określonego dostawcy s mogą wyświetlać i edytować. Jeśli użytkownik wskazuje, czy ona wyświetlać i edytować wszystkie informacje (ustawienie domyślne), ona stronie za pośrednictwem wszystkich dostawców, edytować informacje o adresie dowolnego dostawcy s i edytować nazwę i ilość na jednostkę dla każdego produktu, dostarczone przez wybranego dostawcę. Jeśli użytkownik wskazuje, że może ona tylko przeglądać i edycji danego dostawcy, jednak, a następnie ona można tylko wyświetlić szczegóły i produktów do jednego dostawcy i może dotyczyć wyłącznie zaktualizuj nazwę i ilość na informacje o jednostkach dla tych produktów, które są *nie* wycofane.

Naszym pierwszym krokiem, w tym samouczku jest następnie do tworzenia tej metody DropDownList i wypełnianie jej dostawców w systemie. Otwórz `UserLevelAccess.aspx` strony w `EditInsertDelete` folderu, dodać kontrolki DropDownList którego `ID` właściwość jest ustawiona na `Suppliers`i powiąż ten DropDownList nowe kontrolki ObjectDataSource, o nazwie `AllSuppliersDataSource`.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Rysunek 3**: Utwórz nowy o nazwie elementu ObjectDataSource `AllSuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Ponieważ chcemy, aby ta lista DropDownList na uwzględnienie wszystkich dostawców, skonfiguruj kontrolki ObjectDataSource do wywołania `SuppliersBLL` klasy s `GetSuppliers()` metody. Ponadto upewnij się, że ObjectDataSource s `Update()` metody jest mapowany na `SuppliersBLL` klasy s `UpdateSupplierAddress` metody jako tej kontrolki ObjectDataSource będzie również używany przez DetailsView będziemy dodawać w kroku 2.

Po zakończeniu pracy Kreatora ObjectDataSource, wykonaj kroki, konfigurując `Suppliers` DropDownList tak, aby pokazywał `CompanyName` pola danych i używa `SupplierID` pola danych jako wartość dla każdego `ListItem`.


[![Konfigurowanie kontrolki DropDownList dostawcy do użycia CompanyName i pola danych IDDostawcy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie `Suppliers` DropDownList do użycia `CompanyName` i `SupplierID` pól danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


W tym momencie metody DropDownList Wyświetla nazwy firmy, dostawców w bazie danych. Jednakże należy również uwzględnić opcję "Pokaż/edytowanie wszystkich dostawców", do metody DropDownList. Aby to zrobić, należy ustawić `Suppliers` DropDownList s `AppendDataBoundItems` właściwości `true` , a następnie dodaj `ListItem` którego `Text` właściwość jest "Show/edytowanie wszystkich dostawców", którego wartość jest `-1`. To można dodawać bezpośrednio w oznaczeniu deklaracyjnym lub za pomocą projektanta, przechodząc do okna właściwości i klikając wielokropek w DropDownList s `Items` właściwości.

> [!NOTE]
> Odwołaj się do [ *wzorzec/szczegół filtrowanie przy użyciu kontrolki DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) samouczek dotyczący uzyskać bardziej szczegółowe informacje na temat dodawania elementu Zaznacz wszystko do powiązanych z danymi DropDownList.


Po `AppendDataBoundItems` właściwość została ustawiona i `ListItem` dodane, s DropDownList w oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Rysunek 5. pokazuje nasz Bieżący postęp zrzut ekranu podczas wyświetlania za pośrednictwem przeglądarki.


[![DropDownList dostawców zawiera pokazu wszystkie ListItem oraz jeden dla każdego z dostawców](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Rysunek 5**: `Suppliers` DropDownList zawiera wszystkie Pokaż `ListItem`, oraz jeden dla każdego dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Ponieważ chcemy zaktualizować interfejs użytkownika natychmiast, po użytkownik zmienił ich wyboru, ustaw `Suppliers` DropDownList s `AutoPostBack` właściwość `true`. W kroku 2 utworzymy kontrolce DetailsView, pokazujące informacje dla dostawców, w oparciu o wybór metody DropDownList. Następnie w kroku 3, utworzymy program obsługi zdarzeń dla tej metody DropDownList s `SelectedIndexChanged` zdarzeń, w którym dodasz kod, który tworzy powiązanie danych odpowiedniego dostawcy DetailsView na podstawie wybranego dostawcy.

## <a name="step-2-adding-a-detailsview-control"></a>Krok 2: Dodawanie kontrolki widoku szczegółów

Pozwól s Użyj DetailsView, aby pokazać informacje o dostawcach. Dla użytkownika, który można wyświetlać i edytować wszystkich dostawców DetailsView będzie obsługiwać stronicowanie, dzięki czemu użytkownik krokowo dostawcy informacji o jeden rekord jednocześnie. Jeśli użytkownik pracuje dla określonego dostawcy, jednak DetailsView są wyświetlane tylko określonego dostawcy s informacje i nie będzie zawierać interfejsu stronicowania. W obu przypadkach DetailsView musi umożliwić użytkownikowi edytowanie dostawcy s adres, miasto i pola.

Dodaj element DetailsView do strony pod `Suppliers` DropDownList, ustaw jego `ID` właściwości `SupplierDetails`, który należy powiązać `AllSuppliersDataSource` ObjectDataSource utworzony w poprzednim kroku. Następnie sprawdź włączone stronicowanie i Włącz edytowanie pól wyboru z tagu inteligentnego s DetailsView.

> [!NOTE]
> Jeśli opcja Włącz edytowanie w inteligentne s DetailsView don t otaguj go s, ponieważ nie mapować ObjectDataSource s `Update()` metody `SuppliersBLL` klasy s `UpdateSupplierAddress` metody. Poświęć chwilę, aby powrócić i wprowadzić zmiany, po upływie którego opcja Włącz edytowanie powinien pojawić się w tagu inteligentnego s DetailsView tej konfiguracji.


Ponieważ `SuppliersBLL` klasy s `UpdateSupplierAddress` metoda tylko przyjmuje cztery parametry - `supplierID`, `address`, `city`, i `country` -zmodyfikować DetailsView s BoundFields tak, aby `CompanyName` i `Phone` BoundFields są przeznaczone tylko do odczytu. Ponadto Usuń `SupplierID` elementu BoundField całkowicie. Na koniec `AllSuppliersDataSource` ma obecnie ObjectDataSource jego `OldValuesParameterFormatString` właściwością `original_{0}`. Poświęć chwilę, aby usunąć ustawienie tej właściwości z całkowicie składni deklaratywnej lub ustaw ją na wartość domyślną `{0}`.

Po skonfigurowaniu `SupplierDetails` DetailsView i `AllSuppliersDataSource` ObjectDataSource, będzie można korzystać z niej następujące znaczniki deklaratywne:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

W tym momencie może być stronicowana DetailsView za pośrednictwem i informacje o adresie wybranego dostawcy s może zostać zaktualizowany, niezależnie od opcji wybranej w `Suppliers` DropDownList (patrz rysunek 6).


[![Wszelkie informacje o dostawcy mogą być wyświetlane i zaktualizować jego adresu](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Rysunek 6**: dowolnego dostawcy informacje mogą być wyświetlane, a następnie zaktualizować jego adresu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Krok 3: Wyświetlanie informacji s wybranego dostawcy.

Naszą stronę obecnie Wyświetla informacje o wszystkich dostawców niezależnie od tego, czy została wybrana określonego dostawcę z `Suppliers` DropDownList. Aby wyświetlić tylko informacje o dostawcy dla wybranego dostawcy musimy dodać innego elementu ObjectDataSource do strony, taki, który pobiera informacje dotyczące określonego dostawcy.

Dodawanie nowego elementu ObjectDataSource ze stroną nadawania mu nazwy `SingleSupplierDataSource`. W tagu inteligentnego, kliknij łącze Konfiguruj źródła danych i jego użycia `SuppliersBLL` klasy s `GetSupplierBySupplierID(supplierID)` metody. Podobnie jak w przypadku `AllSuppliersDataSource` ObjectDataSource, mają `SingleSupplierDataSource` ObjectDataSource s `Update()` metoda mapowane na `SuppliersBLL` klasy s `UpdateSupplierAddress` metody.


[![Konfigurowanie kontrolki SingleSupplierDataSource ObjectDataSource przy użyciu metody GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Rysunek 7**: Konfigurowanie `SingleSupplierDataSource` ObjectDataSource do użycia `GetSupplierBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Następnie możemy ponownie wyświetlony monit o określenie parametru źródło `GetSupplierBySupplierID(supplierID)` metoda s `supplierID` parametr wejściowy. Ponieważ chcemy wyświetlić informacje o dostawcy wybrana w zaufanym DropDownList, użyj `Suppliers` DropDownList s `SelectedValue` właściwość jako źródło parametru.


[![Użyj metody DropDownList dostawców jako IDDostawcy źródła](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Rysunek 8**: Użyj `Suppliers` DropDownList jako `supplierID` źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Nawet w przypadku tego dodany drugi ObjectDataSource, kontrolce DetailsView jest obecnie skonfigurowany do zawsze używaj `AllSuppliersDataSource` ObjectDataSource. Musimy dodać logikę w celu dostosowania źródło danych używane przez DetailsView w zależności od `Suppliers` zaznaczony element DropDownList. Aby to zrobić, należy utworzyć `SelectedIndexChanged` program obsługi zdarzeń dla metody DropDownList dostawców. Najłatwiej można go utworzyć, klikając dwukrotnie plik DropDownList w projektancie. Ta procedura obsługi zdarzeń musi ustalić, jakie źródła danych i musi ponownie powiązać dane DetailsView. Jest to realizowane przy użyciu następującego kodu:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Program obsługi zdarzeń rozpoczyna się od określenia, czy wybrano opcję "Pokaż/edytowanie wszystkich dostawców". Jeśli tak jest, ustawia `SupplierDetails` DetailsView s `DataSourceID` do `AllSuppliersDataSource` i zwraca użytkownika do pierwszego rekordu w zestawie dostawców, ustawiając `PageIndex` właściwości na wartość 0. Jeśli jednak użytkownik wybrał określonego dostawcę z metody DropDownList DetailsView s `DataSourceID` jest przypisany do `SingleSuppliersDataSource`. Niezależnie od tego, jakie dane źródła jest używany, `SuppliersDetails` tryb zostanie przywrócona do trybu tylko do odczytu i danych jest odbitych do DetailsView przez wywołanie `SuppliersDetails` formantu s `DataBind()` metody.

Z tego programu obsługi zdarzeń w miejscu kontrolce DetailsView zawiera obecnie wybranego dostawcy, chyba że wybrano opcję "Pokaż/edytowanie wszystkich dostawców", w którym to przypadku wszyscy dostawcy mogą być wyświetlane za pomocą interfejsu stronicowania. Nr 9 przedstawiono strony za pomocą opcji "Show/edytowanie wszystkich dostawców"; należy pamiętać, że interfejs stronicowania obecny, umożliwiając użytkownikowi można znaleźć i zaktualizować dowolnemu. Na rysunku nr 10 przedstawiono strony z dostawcą Ma Maison zaznaczone. Tylko Ma Maison s informacje w tym przypadku jest widoczny i można edytować.


[![Wszystkie informacje dotyczące dostawców można wyświetlać i edytować](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Rysunek 9**: wszystkich dostawców informacje może wyświetlać i edytowana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Można wyświetlać i edytować tylko informacje wybranego dostawcy s](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Na rysunku nr 10**: s wybrany dostawca informacji może być Viewed i edytowana tylko ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> W tym samouczku DropDownList i DetailsView kontrolować s `EnableViewState` musi być równa `true` (ustawienie domyślne) ponieważ DropDownList s `SelectedIndex` i DetailsView s `DataSourceID` zmiany właściwości s należy pamiętać różnych ogłaszania zwrotnego.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Krok 4: Wyświetlanie listy produktów dostawców w edycji kontrolki GridView

Za pomocą pełną DetailsView naszym kolejnym krokiem jest umieszczenie GridView można edytować, który zawiera listę tych produktów, dostarczone przez wybranego dostawcę. Ta GridView powinien zezwalać na zmiany tylko `ProductName` i `QuantityPerUnit` pola. Ponadto w przypadku użytkowników, odwiedzając stronę od określonego dostawcy, jego Zezwalaj tylko na aktualizacje do tych produktów, które są *nie* wycofane. W tym należy najpierw dodać przeciążenia `ProductsBLL` klasy s `UpdateProducts` metodę, która przyjmuje tylko `ProductID`, `ProductName`, i `QuantityPerUnit` pola jako dane wejściowe. Firma Microsoft ve schodkowego wcześniej przez ten proces w samouczkach liczne teraz s Przyjrzyjmy się tylko w tym miejscu kodu, które powinny zostać dodane do `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Z tego przeciążenia utworzone, możemy ponownie gotowe, aby dodać kontrolki GridView i jego skojarzonego elementu ObjectDataSource. Dodaj nowe kontrolki GridView do strony, ustaw jego `ID` właściwości `ProductsBySupplier`i skonfiguruj ją tak, aby użyć nowego elementu ObjectDataSource, o nazwie `ProductsBySupplierDataSource`. Ponieważ chcemy, aby ta GridView, aby wyświetlić listę tych produktów przez wybranego dostawcę, należy użyć `ProductsBLL` klasy s `GetProductsBySupplierID(supplierID)` metody. Również mapować `Update()` metody do nowego `UpdateProduct` przeciążenia, którą właśnie utworzyliśmy.


[![Konfigurowanie kontrolki ObjectDataSource do użycia przeciążenia UpdateProduct właśnie utworzony](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Rysunek 11**: Konfigurowanie kontrolki ObjectDataSource do użycia `UpdateProduct` przeciążenia właśnie utworzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Możemy ponownie monit o wybranie źródła parametru `GetProductsBySupplierID(supplierID)` metoda s `supplierID` parametr wejściowy. Ponieważ chcemy wyświetlić produktów dla dostawcy wybrany w widoku DetailsView, użyj `SuppliersDetails` kontrolce DetailsView s `SelectedValue` właściwość jako źródło parametru.


[![Użyj właściwości SelectedValue s SuppliersDetails DetailsView jako źródło parametru](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Rysunek 12**: Użyj `SuppliersDetails` DetailsView s `SelectedValue` właściwość jako źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Wracając do kontrolki GridView, Usuń wszystkie pola GridView, z wyjątkiem `ProductName`, `QuantityPerUnit`, i `Discontinued`, oznaczanie `Discontinued` CheckBoxField jako tylko do odczytu. Ponadto zaznacz opcję Włącz edytowanie, za pomocą tagu inteligentnego s GridView. Po dokonaniu tych zmian, oznaczeniu deklaracyjnym GridView i kontrolki ObjectDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Podobnie jak w przypadku naszego poprzedniego ObjectDataSources ten jeden s `OldValuesParameterFormatString` właściwość jest ustawiona na `original_{0}`, co spowoduje problemy podczas próby zaktualizowania nazwy produktu s lub ilość na jednostkę. Usuń tę właściwość z składni deklaratywnej całkowicie lub ustaw go na wartość domyślną `{0}`.

W przypadku tej konfiguracji pełną naszą stronę teraz zawiera listę produktów dostarczone przez dostawcę wybrany w widoku GridView (zobacz rysunek 13). Obecnie *wszelkie* można zaktualizować nazwy produktu s lub ilość na jednostkę. Jednak należy zaktualizować logikę naszej stronie funkcjonalność jest zabroniony dla elementu uwzględniałyby produkty wycofane dla użytkowników skojarzonych z określonym dostawcą. Firma Microsoft będzie czoła tym ostatni element w kroku 5.


[![Produkty, dostarczone przez dostawcę wybrana są wyświetlane.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Rysunek 13**: wyświetlane są udostępniane przez dostawcę zaznaczone ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Dodając ten można edytować GridView `Suppliers` DropDownList s `SelectedIndexChanged` program obsługi zdarzeń powinny być aktualizowane, aby powrócić do stanu tylko do odczytu, widoku GridView. W przeciwnym razie wybranie innego dostawcy podczas środku edycji — informacje o produkcie odpowiedni indeks w widoku GridView dla nowego dostawcy będą również można edytować. Aby tego uniknąć, po prostu ustaw GridView s `EditIndex` właściwości `-1` w `SelectedIndexChanged` programu obsługi zdarzeń.


Ponadto odwołania, jest ważne, że GridView s stan widoku jest włączone (zachowanie domyślne). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, uruchom ryzyko, że liczba równoczesnych użytkowników przypadkowo usuwanie i edytowanie rekordów. Zobacz [Ostrzeżenie: problem współbieżności przy użyciu platformy ASP.NET 2.0 GridViews/DetailsView/FormViews tej edycji pomocy technicznej i/lub usuwanie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Krok 5: Nie zezwalaj na edytowanie wycofane produktów podczas pokazu/edytowanie wszystkich dostawców jest nie zaznaczone

Gdy `ProductsBySupplier` GridView jest w pełni funkcjonalny, obecnie przyzna zbyt szeroki dostęp do tych użytkowników, którzy są od określonego dostawcy. Na naszych reguł biznesowych takich użytkowników powinna nie można zaktualizować uwzględniałyby produkty wycofane. Aby to wymusić, możemy ukryć (lub wyłączyć) przycisk Edytuj, w widoku GridView wiersze, w których uwzględniałyby produkty wycofane podczas odwiedzania strony przez użytkownika od dostawcy.

Utwórz procedurę obsługi zdarzeń dla GridView s `RowDataBound` zdarzeń. W tej obsługi zdarzeń należy ustalić, czy użytkownik jest skojarzony z określonego dostawcy, który na potrzeby tego samouczka można ustalić, sprawdzając s DropDownList dostawców `SelectedValue` właściwość — jeśli jego coś innego niż -1, a następnie użytkownik jest skojarzone z określonym dostawcą. Dla tych użytkowników następnie należy określić, czy produkt jest obsługiwany. Firma Microsoft może Pobierz odwołanie do rzeczywistego `ProductRow` wystąpienia jest powiązana z wiersza w widoku GridView za pośrednictwem `e.Row.DataItem` właściwości, zgodnie z opisem w [ *wyświetlanie informacji podsumowania w kontrolce GridView s stopki* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) samouczek. Jeśli produkt jest przerywane, firma Microsoft Pobierz programowe odwołanie do przycisk Edytuj, w tym s GridView CommandField przy użyciu techniki opisane w poprzednim samouczku [ *Dodawanie klienta potwierdzenie podczas usuwania* ](adding-client-side-confirmation-when-deleting-vb.md). Gdy będziemy już mieć odwołania, możemy następnie ukryć lub wyłączanie przycisku.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Z tym zdarzeniem obsługi w miejscu, gdy odwiedzić tę stronę jako użytkownik z określonego dostawcy, na tych produktów, które są anulowane jest niemożliwa, jako przycisk Edytuj jest ukryty dla tych produktów. Na przykład s Jacka Chef Gumbo mieszany jest produktem nieobsługiwane dla dostawcy radości Cajun nowy Orlean. Podczas wyświetlania strony dla tego określonego dostawcy, przycisk edycji dla tego produktu jest ukryty kontakt (zobacz rysunek 14). Jednak podczas wizyt u klientów przy użyciu "Show/edytowanie wszystkich dostawców", przycisk edycji jest dostępny (patrz rysunek 15).


[![Dla użytkowników specyficzne dla dostawcy przycisk Edytuj s Jacka Chef Gumbo mieszany jest ukryta](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Rysunek 14**: specyficzne dla dostawcy użytkowników przycisk Edytuj s Jacka Chef Gumbo mieszany jest ukryty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Dla wszystkich użytkowników Show/Edytowanie dostawcy jest wyświetlany przycisk Edytuj s Jacka Chef Gumbo mieszany](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Rysunek 15**: Pokaz/edytowanie wszystkich dostawców użytkowników, s Jacka Chef Gumbo mieszany jest wyświetlany przycisk Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Sprawdzanie praw dostępu w warstwy logiki biznesowej

W tym samouczku platformy ASP.NET, strona obsługuje całą logikę w odniesieniu do informacji, które użytkownik może zobaczyć i jakie produkty on aktualizacji. W idealnym przypadku tę logikę również będą zainstalowane na warstwę logiki biznesowej. Na przykład `SuppliersBLL` klasy s `GetSuppliers()` metody (która zwraca wszystkich dostawców) może obejmować sprawdzenie w celu zapewnienia, że aktualnie zalogowanego użytkownika jest *nie* skojarzone z określonym dostawcą. Podobnie `UpdateSupplierAddress` metoda może obejmować sprawdzenie w celu zapewnienia, że aktualnie zalogowanego użytkownika, albo dla naszej firmy (i w związku z tym można zaktualizować wszystkie informacje o adresie dostawcy) lub jest skojarzony z dostawcą, którego dane są aktualizowane.

Czy mogę nie obejmują takie kontrole warstwy LOGIKI, ponieważ w naszym samouczku prawa użytkownika s są określane przez kontrolki DropDownList na stronie nie ma dostępu do klasy LOGIKI. Podczas korzystania z systemu członkostwa lub jeden schematów uwierzytelniania poza-gotową do dostarczanego przez platformę ASP.NET (takich jak uwierzytelnianie Windows), aktualnie zalogowanego użytkownika s informacji i informacje o rolach są dostępne z LOGIKI, sprawiając, że taki dostęp możliwe w prezentacji i warstwy LOGIKI sprawdza prawa.

## <a name="summary"></a>Podsumowanie

Większość witryn, które zapewniają konta użytkowników muszą Dostosowywanie interfejsu modyfikacji danych na podstawie zalogowanego użytkownika. Użytkownicy administracyjni mogą można usuwać i edytować każdy rekord innych niż administracyjne użytkownicy mogą być ograniczone do tylko aktualizowanie i usuwanie rekordów, które zostały utworzone samodzielnie. Może być niezależnie od scenariusza, danych kontrolki sieci Web, ObjectDataSource, i można rozszerzyć klasy warstwy logiki biznesowej do dodawania lub odmawiania niektórych funkcji, na podstawie zalogowanego użytkownika. W tym samouczku będziemy pokazaliśmy, jak ograniczyć dane wyświetlane i można edytować w zależności od tego, czy użytkownik został skojarzony z określonym dostawcą i jeśli jego pracownicy współpracowali dla firmy.

W tym samouczku kończy się nasze badania Wstawianie, aktualizowanie i usuwanie danych przy użyciu kontrolki GridView DetailsView i FormView. Począwszy od następnego samouczka, firma Microsoft będzie włączyć naszej uwagi do dodawania stronicowanie i sortowanie pomocy technicznej.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-client-side-confirmation-when-deleting-vb.md)
