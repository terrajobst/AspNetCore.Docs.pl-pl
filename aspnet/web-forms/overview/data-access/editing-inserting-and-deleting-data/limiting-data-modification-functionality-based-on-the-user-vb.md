---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Ograniczenia funkcji modyfikacji danych na podstawie użytkownika (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W aplikacji sieci web, która umożliwia użytkownikom edytowanie danych konta innego użytkownika mogą mieć różne uprawnienia do edycji danych. W tym samouczku zajmiemy się jak t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: eccb8e114e0ecf772680a2e5b36a761736cf8025
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Ograniczenia funkcji modyfikacji danych na podstawie użytkownika (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) lub [pobierania plików PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> W aplikacji sieci web, która umożliwia użytkownikom edytowanie danych konta innego użytkownika mogą mieć różne uprawnienia do edycji danych. W tym samouczku zajmiemy się, jak ustawić dynamicznie na podstawie zaproszonych użytkownika funkcji modyfikacji danych.


## <a name="introduction"></a>Wprowadzenie

Wiele aplikacji sieci web obsługuje kont użytkowników i innych opcji, raporty i funkcjonalność, na podstawie zalogowanego użytkownika. Na przykład z naszymi samouczkami, firma Microsoft może użytkownicy będą firm dostawcy zalogować się do witryny i zaktualizuj informacje ogólne o swoich produktach - nazwy użytkownika i ilość na jednostkę, prawdopodobnie — wraz z informacjami o dostawcy, takie jak jego nazwa firmy adres, informacje kontaktowe osoby s i tak dalej. Ponadto firma Microsoft może być obejmują niektóre konta użytkowników dla osób z naszej firmy, dzięki czemu mogą zalogować się i zaktualizować informacje o produkcie, takich jak jednostki w magazynie, minimum i tak dalej. Naszej aplikacji sieci web mogą również zezwalać użytkownikom anonimowym odwiedź (osoby, które nie są zalogowani), ale będzie ograniczona ich jedynie do wyświetlania danych. Takie w systemie użytkownika konta w miejscu czy chcemy danych formantów sieci Web w naszej strony ASP.NET oferowanie Wstawianie, edytowanie i usuwanie możliwości odpowiednie dla obecnie zalogowanego użytkownika.

W tym samouczku zajmiemy się, jak ustawić dynamicznie na podstawie zaproszonych użytkownika funkcji modyfikacji danych. W szczególności utworzymy strona, która zawiera informacje dotyczące dostawców w edytowalnych DetailsView wraz z widoku GridView, który zawiera listę produktów, dostarczone przez dostawcę. W przypadku użytkowników, odwiedzając stronę z naszej firmy, mogą one: wyświetlanie wszystkich informacji dostawcy s; Edytowanie adresu; i zmodyfikowania tych informacji w odniesieniu do produktu dostarczony przez dostawcę. Jeśli jednak użytkownik należy do określonej firmy, mogą one tylko wyświetlić i edytować własnych informacji o adresie oraz można edytować tylko ich produktów, które nie zostały oznaczone jako przerywane.


[![Użytkownik z naszej firmy można edytować żadnych informacji s dostawcy](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Rysunek 1**: użytkownik z s nasze firmy można edytować dowolnego dostawcy informacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![Użytkownik z określonego dostawcę może tylko wyświetlanie i edytowanie informacji o ich](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Rysunek 2**: użytkownik z określonego dostawcy można tylko wyświetlanie i edytowanie informacji o ich ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Rozpoczynanie pracy dzięki s!

> [!NOTE]
> Platforma ASP.NET 2.0 systemu członkostwa s zapewnia platformę standardowych, rozszerzalne do tworzenia, zarządzania i sprawdzanie poprawności konta użytkownika. Ponieważ badanie systemu członkostwa wykracza poza zakres tego samouczka, w tym samouczku zamiast tego "substytutów" członkostwa, zezwalając anonimowe odwiedzających wybierz, czy są one z określonego dostawcę lub firmy. Więcej na członkostwo, można znaleźć w mojej [s badanie ASP.NET 2.0 członkostwa, ról i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) artykuł z serii.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Krok 1: Zezwalanie użytkownikowi określenie praw dostępu

W aplikacji sieci web rzeczywistych informacje o koncie s użytkownika czy obejmują one działał dla firmy lub dla określonego dostawcy, a te informacje będą programowo dostępne z naszej strony ASP.NET, gdy użytkownik jest zalogowany do lokacji. Te informacje można przechwytywać za pomocą programu ASP.NET 2.0 s role systemu, jako informacje o koncie użytkownika na poziomie przez system profilu, albo przez oznacza, że niektóre niestandardowe.

Od czasu w celu tego samouczka jest aby zademonstrować dostosowanie funkcji modyfikacji danych na podstawie zalogowanego użytkownika, a nie jest przeznaczona do członkostwa s pokazy ASP.NET 2.0, ról i profilu systemów, aby określić użyjemy mechanizm bardzo prosty możliwości dla użytkownika, odwiedzając stronę - DropDownList, z którego użytkownik może wskazywać, czy powinno być możliwe wyświetlić i edytować informacje o dostawcy i, można również co określonego dostawcę s informacji mogą wyświetlać i edytować. Jeśli użytkownik wskazuje, czy użytkownik może wyświetlać i edytować wszystkie informacje o dostawcach (ustawienie domyślne), klika strony przez wszystkich dostawców, edytować dowolne informacje adres s dostawcy i Edytuj nazwę i ilość na jednostkę, dla każdego produktu dostarczony przez wybranego dostawcę. Jeśli użytkownik wskazuje, że może ona tylko przeglądać i Edytuj określonego dostawcę, jednak, a następnie klika można tylko wyświetlić szczegóły i produktów dla jednego dostawcy i może tylko zaktualizować nazwę i ilość na informacje o jednostce dla produktów, które są *nie* przerywane.

Naszym pierwszym krokiem w tym samouczku, wówczas do tworzenia tego DropDownList i wypełnić ją dostawców w systemie. Otwórz `UserLevelAccess.aspx` strony `EditInsertDelete` folderu, Dodaj DropDownList których `ID` właściwość jest ustawiona na `Suppliers`i powiązać ten DropDownList nowy element ObjectDataSource o nazwie `AllSuppliersDataSource`.


[![Utwórz nowy element ObjectDataSource o nazwie AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Rysunek 3**: Utwórz nowy składnik o nazwie ObjectDataSource `AllSuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


Ponieważ chcemy tego DropDownList uwzględnienie wszystkich dostawców skonfigurować ObjectDataSource do wywołania `SuppliersBLL` klasy s `GetSuppliers()` metody. Również upewnij się, że element ObjectDataSource s `Update()` metody jest mapowany na `SuppliersBLL` klasy s `UpdateSupplierAddress` metody, jak ten element ObjectDataSource będą również używane przez widoku DetailsView, które będą dodawane w kroku 2.

Po zakończeniu pracy Kreatora ObjectDataSource, wykonaj kroki, konfigurując `Suppliers` DropDownList taki sposób, że widoczny jest `CompanyName` pola danych i używa `SupplierID` pola danych jako wartość dla każdego `ListItem`.


[![Skonfiguruj DropDownList dostawcy do użycia NazwaFirmy i pola IDDostawcy danych](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Rysunek 4**: Konfigurowanie `Suppliers` DropDownList użyć `CompanyName` i `SupplierID` pól danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


W tym momencie DropDownList Wyświetla nazwy firmy, dostawców w bazie danych. Jednak również należy dołączyć opcję "Pokaż i edytowanie wszystkich dostawców", aby DropDownList. Aby to zrobić, ustaw `Suppliers` DropDownList s `AppendDataBoundItems` właściwości `true` , a następnie dodaj `ListItem` których `Text` właściwość jest "Pokaż i edytowanie wszystkich dostawców", którego wartość jest `-1`. To można dodać bezpośrednio za pomocą deklaratywne znaczników lub poprzez projektanta, przechodząc do okna właściwości i klikając wielokropek w DropDownList s `Items` właściwości.

> [!NOTE]
> Odwołaj się do [ *wzorzec/szczegół filtrowania z DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) samouczka bardziej szczegółowe omówienie na dodawanie elementu Zaznacz wszystko do powiązanych z danymi DropDownList.


Po `AppendDataBoundItems` właściwość została ustawiona i `ListItem` dodany, znaczników deklaratywne s DropDownList powinien wyglądać następująco:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

Rysunek 5. pokazuje zrzut ekranu: naszych Bieżący postęp, podczas wyświetlania za pośrednictwem przeglądarki.


[![Lista DropDownList dostawców zawiera pokazu wszystkich element listy, a także dla każdego z dostawców](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Rysunek 5**: `Suppliers` DropDownList zawiera wszystkie Pokaż `ListItem`, oraz jednym dla każdego dostawcy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


Ponieważ chcemy Aktualizowanie interfejsu użytkownika, natychmiast po użytkownik zmienił ich wyboru, ustaw `Suppliers` DropDownList s `AutoPostBack` właściwości `true`. W kroku 2 utworzymy zawierające informacje dotyczące dostawców według wybranego DropDownList formantu widoku DetailsView. Następnie, w kroku 3, utworzymy program obsługi zdarzeń dla tego s DropDownList `SelectedIndexChanged` zdarzeń, w którym zostanie dodany kod, który wiąże odpowiednie informacje do widoku DetailsView ustalane na podstawie wybranego dostawcy.

## <a name="step-2-adding-a-detailsview-control"></a>Krok 2: Dodawanie formantu widoku DetailsView

Pozwól umożliwia wyświetlanie informacji o dostawcy Element DetailsView s. Dla użytkownika, który można wyświetlać i edytować wszystkich dostawców widoku DetailsView będzie obsługiwać stronicowania, umożliwiający użytkownikowi kroków opisanych w jeden rekord informacji o dostawcy naraz. Jeśli użytkownik pracuje dla określonego dostawcy, jednak widoku DetailsView są wyświetlane tylko określonego dostawcy s informacje i nie będzie zawierać interfejsu stronicowania. W obu przypadkach widoku DetailsView musi zezwolić użytkownikowi na edytowanie dostawcy s adresu, miasta i pola.

Na stronie poniżej Dodaj element DetailsView `Suppliers` DropDownList, ustaw jej `ID` właściwości `SupplierDetails`i powiązać `AllSuppliersDataSource` ObjectDataSource utworzony w poprzednim kroku. Następnie sprawdź włączyć stronicowanie i Włącz edytowanie pola wyboru z tagów inteligentnych s widoku DetailsView.

> [!NOTE]
> Jeśli opcja Włącz edytowanie w widoku DetailsView s inteligentne ADAM t tagu go s, ponieważ nie mapy ObjectDataSource s `Update()` metodę `SuppliersBLL` klasy s `UpdateSupplierAddress` metody. Poświęć chwilę, aby wrócić i zmienić tę konfigurację, zmienić, po upływie którego opcję Włącz edytowanie powinny być wyświetlane w widoku DetailsView tag inteligentny s.


Ponieważ `SuppliersBLL` klasy s `UpdateSupplierAddress` metody akceptuje tylko cztery parametry - `supplierID`, `address`, `city`, i `country` -zmodyfikować s widoku DetailsView BoundFields, aby `CompanyName` i `Phone` BoundFields są tylko do odczytu. Ponadto, Usuń `SupplierID` całkowicie elementu BoundField. Na koniec `AllSuppliersDataSource` ma obecnie ObjectDataSource jego `OldValuesParameterFormatString` ustawioną właściwość `original_{0}`. Poświęć chwilę, aby usunąć ustawienie właściwości z całkowicie składni deklaratywnej lub ustaw ją na wartość domyślna to `{0}`.

Po skonfigurowaniu `SupplierDetails` widoku DetailsView i `AllSuppliersDataSource` ObjectDataSource, firma Microsoft będzie mieć następujące deklaratywne znaczników:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

W tym momencie widoku DetailsView może być stronicowana za pośrednictwem i można go zaktualizować informacje o adresach wybranego dostawcy s niezależnie od opcji wybranej w `Suppliers` DropDownList (patrz rysunek 6).


[![Wszelkie informacje dostawców można wyświetlić i zaktualizować adres](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Rysunek 6**: wszystkich dostawców informacje można wyświetlić i zaktualizować jej adresu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Krok 3: Wyświetlanie informacji s wybranego dostawcy

Naszą stronę obecnie Wyświetla informacje dla wszystkich dostawców niezależnie od tego, czy została wybrana określonego dostawcę z `Suppliers` DropDownList. Aby wyświetlić tylko informacje dostawcy dla wybranego dostawcy należy dodać inny element ObjectDataSource do strony, który pobiera informacje dotyczące określonego dostawcy.

Dodaj nowy element ObjectDataSource ze stroną nazw `SingleSupplierDataSource`. W tagu inteligentnego, kliknij łącze skonfiguruj źródło danych i go używać `SuppliersBLL` klasy s `GetSupplierBySupplierID(supplierID)` metody. Jak `AllSuppliersDataSource` ObjectDataSource, mają `SingleSupplierDataSource` ObjectDataSource s `Update()` metody mapowane na `SuppliersBLL` klasy s `UpdateSupplierAddress` — metoda.


[![Skonfiguruj SingleSupplierDataSource ObjectDataSource przy użyciu metody GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Rysunek 7**: Konfigurowanie `SingleSupplierDataSource` ObjectDataSource użyć `GetSupplierBySupplierID(supplierID)` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


Następnie możemy re monit o określenie źródło parametru `GetSupplierBySupplierID(supplierID)` metody s `supplierID` parametru wejściowego. Ponieważ chcemy wyświetlić informacje dotyczące dostawcy z DropDownList, użyj `Suppliers` DropDownList s `SelectedValue` właściwość jako źródło parametru.


[![Użyj DropDownList dostawców jako IDDostawcy źródło parametru](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Rysunek 8**: Użyj `Suppliers` DropDownList jako `supplierID` źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


Nawet w przypadku tego dodany drugi ObjectDataSource, formantu widoku DetailsView jest obecnie skonfigurowany zawsze używaj `AllSuppliersDataSource` ObjectDataSource. Musimy dodać logikę, aby dopasować źródło danych używane przez element DetailsView w zależności od `Suppliers` zaznaczony element DropDownList. W tym celu należy utworzyć `SelectedIndexChanged` programu obsługi zdarzeń dla DropDownList dostawców. Można go utworzyć najłatwiej kliknąć dwukrotnie DropDownList w projektancie. Ten program obsługi zdarzeń musi określić, jakie źródła danych do użycia i musi ponownie powiązać dane do widoku DetailsView. Jest to realizowane przy użyciu następującego kodu:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

Program obsługi zdarzeń rozpoczyna się przez określenie, czy wybrano opcję "Pokaż i edytowanie wszystkich dostawców". Jeśli tak jest, ustawia `SupplierDetails` s widoku DetailsView `DataSourceID` do `AllSuppliersDataSource` i zwraca użytkownika na pierwszy rekord w zestawie dostawców przez ustawienie `PageIndex` właściwości na 0. Jeśli jednak użytkownik wybrał określonego dostawcę z DropDownList, s widoku DetailsView `DataSourceID` jest przypisany do `SingleSuppliersDataSource`. Niezależnie od tego, jakie dane źródłowego jest używana, `SuppliersDetails` tryb zostanie przywrócony do trybu tylko do odczytu i danych jest odbitych do widoku DetailsView przez wywołanie do `SuppliersDetails` kontroli s `DataBind()` metody.

Z tej obsługi zdarzeń w miejscu formantu widoku DetailsView zawiera obecnie wybranego dostawcy, chyba że wybrano opcję "Pokaż i edytowanie wszystkich dostawców", w którym to przypadku wszyscy dostawcy można wyświetlić za pomocą interfejsu stronicowania. Na rysunku nr 9 przedstawiono strony z opcji "Pokaż i edytowanie wszystkich dostawców"; należy pamiętać, że interfejs stronicowania istnieje, umożliwiając użytkownikowi odwiedź i aktualizowanie dostawcy. Rysunek nr 10 przedstawia strony z dostawcą Ma Maison zaznaczone. Tylko informacje s Ma Maison w tym przypadku jest widoczny i edytować.


[![Wszystkie informacje dostawców można wyświetlać i edytować](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Rysunek 9**: wszystkich dostawców informacje można wyświetlić i edytowany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![Można wyświetlać i edytować tylko informacje wybranego dostawcy s](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Na rysunku nr 10**: tylko s dostawcy wybrane informacje mogą być Viewed i edytowany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> W tym samouczku DropDownList i widoku DetailsView kontrolować s `EnableViewState` musi mieć ustawioną `true` (ustawienie domyślne) ponieważ DropDownList s `SelectedIndex` i s widoku DetailsView `DataSourceID` zmiany właściwości s należy pamiętać, między ogłaszania zwrotnego.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Krok 4: Wyświetlanie listy produktów dostawców w można edytować widoku GridView

Z pełną DetailsView naszych następnym krokiem jest uwzględnienie można edytować widoku GridView z listą tych produktów dostarczone przez wybranego dostawcę. Ten element GridView powinna zezwalać na modyfikacje tylko `ProductName` i `QuantityPerUnit` pól. Ponadto, jeśli użytkownik, odwiedzając stronę od określonego dostawcy, tylko umożliwiać aktualizacje produktów, które są *nie* przerywane. Musisz najpierw dodać przeciążenie metody w tym celu `ProductsBLL` klasy s `UpdateProducts` metodę, która przyjmuje tylko `ProductID`, `ProductName`, i `QuantityPerUnit` pola jako dane wejściowe. Firma Microsoft kolejnych przeprowadził przez ten proces wcześniej w wielu samouczki, więc let s właśnie przyjrzeć się kod w tym miejscu, które powinny zostać dodane do `ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Z tego przeciążenia utworzone, możemy re gotowe, aby dodać kontrolki widoku siatki i jego skojarzony element ObjectDataSource. Na stronie Dodaj nowy element GridView, ustaw jej `ID` właściwości `ProductsBySupplier`i skonfigurować go do używania nowych ObjectDataSource, o nazwie `ProductsBySupplierDataSource`. Ponieważ chcemy tego widoku GridView, aby wyświetlić listę tych produktów przez wybranego dostawcę użyć `ProductsBLL` klasy s `GetProductsBySupplierID(supplierID)` metody. Również mapować `Update()` metody do nowego `UpdateProduct` przeciążenia właśnie utworzyliśmy.


[![Skonfiguruj ObjectDataSource do użycia przeciążenia UpdateProduct właśnie utworzony](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Rysunek 11**: Konfigurowanie ObjectDataSource użyć `UpdateProduct` przeciążenia właśnie utworzony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


Firma Microsoft re monit o wybranie źródło parametru `GetProductsBySupplierID(supplierID)` metody s `supplierID` parametru wejściowego. Ponieważ chcemy pokazać produktów dla dostawcy wybranego w widoku DetailsView, użyj `SuppliersDetails` formantu widoku DetailsView s `SelectedValue` właściwość jako źródło parametru.


[![Jako źródło parametru należy użyć właściwości SelectedValue s SuppliersDetails widoku DetailsView](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Rysunek 12**: Użyj `SuppliersDetails` s widoku DetailsView `SelectedValue` właściwość jako źródło parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


Powrót do widoku GridView, Usuń wszystkie pola widoku GridView z wyjątkiem `ProductName`, `QuantityPerUnit`, i `Discontinued`, oznaczanie `Discontinued` CheckBoxField jako tylko do odczytu. Ponadto zaznacz opcję Włącz edytowanie z tagów inteligentnych s widoku GridView. Po dokonaniu zmian, deklaratywne znaczników dla widoku GridView i ObjectDataSource powinien wyglądać podobnie do następujących:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Tak jak w naszych poprzednich ObjectDataSources to jeden s `OldValuesParameterFormatString` właściwość jest ustawiona na `original_{0}`, który może powodować problemy podczas próby zaktualizowania nazwy produktu s lub ilość na jednostkę. Całkowicie Usuń tę właściwość z składni deklaratywnej lub ustaw ją na jego domyślny `{0}`.

W tej konfiguracji pełną naszą stronę teraz zawiera listę produktów dostarczone przez dostawcę w widoku GridView (patrz rysunek 13). Obecnie *żadnych* można zaktualizować nazwy produktu s lub ilość na jednostkę. Jednak należy zaktualizować logiki strony, tak aby takich funkcji jest zabroniony dla obejmowałyby produkty dla użytkowników skojarzonych z określonym dostawcą. Firma Microsoft będzie rozwiązania to ostatni element w kroku 5.


[![Produkty dostarczone przez dostawcę wybrane są wyświetlane.](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Rysunek 13**: produkty dostarczone przez dostawcę wybrane są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> Dodając ten można edytować widoku GridView `Suppliers` DropDownList s `SelectedIndexChanged` obsługi zdarzeń powinny zostać uaktualnione do zwrócenia widoku GridView do stanu tylko do odczytu. W przeciwnym razie zaznaczenie inny dostawca podczas środku edytowanie informacji o produkcie odpowiedni indeks w widoku GridView dla nowego dostawcy również będzie można edytować. Aby tego uniknąć, wystarczy ustawić GridView s `EditIndex` właściwości `-1` w `SelectedIndexChanged` obsługi zdarzeń.


Ponadto odwołania, jest ważne, czy można s widok stanu widoku GridView włączone (domyślnie). Jeśli ustawisz GridView s `EnableViewState` właściwości `false`, uruchom ryzyko równoczesnych użytkowników przypadkowo usunięcie lub edytowania rekordów. Zobacz [Ostrzeżenie: problem współbieżności z ASP.NET 2.0 GridViews/widoku DetailsView/FormViews tej edycji pomocy technicznej i/lub usunięcie i Whose stan widoku jest wyłączona](http://scottonwriting.net/sowblog/posts/10054.aspx) Aby uzyskać więcej informacji.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Krok 5: Nie zezwalaj na edytowanie jest przerywane produktów podczas pokazu i edytowanie wszystkich dostawców nie wybrano

Gdy `ProductsBySupplier` widoku GridView jest w pełni funkcjonalny, obecnie przyznaje ona za dużo dostęp do tych użytkowników, którzy są od określonego dostawcy. Na naszym reguł biznesowych takich użytkowników powinny nie była możliwa aktualizacja obejmowałyby produkty. Aby wymusić to, firma Microsoft można ukryć (lub wyłącz) przycisk Edytuj w widoku GridView wiersze, w których produkty wycofane podczas odwiedzania strony przez użytkownika od dostawcy.

Tworzenie procedury obsługi zdarzeń dla widoku GridView s `RowDataBound` zdarzeń. W tej obsłudze zdarzeń należy ustalić, czy użytkownik jest skojarzony z określonego dostawcę, który w tym samouczku, można ustalić sprawdzając s DropDownList dostawców `SelectedValue` właściwości — jeśli jego s coś innego niż -1, a następnie użytkownik jest skojarzone z określonym dostawcą. Dla tych użytkowników następnie należy określić, czy produkt jest już obsługiwana. Firma Microsoft wystarczy pobrać odwołanie do rzeczywistego `ProductRow` powiązane wystąpienie wiersza elementu GridView za pośrednictwem `e.Row.DataItem` właściwości, zgodnie z opisem w [ *wyświetlanie informacji podsumowania w widoku GridView s stopki* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) samouczek. Jeśli produkt jest przerywane, możemy chwycić programowe odwołanie do przycisk Edytuj w widoku GridView s CommandField przy użyciu techniki opisane w poprzedniej samouczek, [ *Dodawanie klienta potwierdzenia podczas usuwania* ](adding-client-side-confirmation-when-deleting-vb.md). Gdy istnieje odwołanie, firma Microsoft może ukryć lub Wyłącz przycisk.


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Z tym zdarzeniem obsługi w miejscu, podczas odwiedzania tej strony jako użytkownik z określonym dostawcą tych produktów, które są anulowane jest niemożliwa, jako przycisk Edytuj jest ukryta dla tych produktów. Na przykład Jacka Chef s Gumbo mieszany jest produktem wycofane radości Cajun Orleans nowego dostawcy. Podczas odwiedzania strony dla tego konkretnego dostawcy, przycisk Edytuj dla tego produktu jest ukryta dla procesów (patrz rysunek 14). Jednak przy użyciu "Pokaż i edytowanie wszystkich dostawców", przycisk Edytuj jest dostępna (patrz rysunek 15).


[![Dla użytkowników specyficznych dla dostawcy przycisk Edytuj Jacka Chef s Gumbo mieszany jest ukryty](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Rysunek 14**: dla użytkowników specyficznych dla dostawcy jest ukryty przycisk Edytuj Jacka Chef s Gumbo mieszanego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Dla wszystkich użytkowników Pokaż i edytowanie dostawcy jest wyświetlany przycisk Edytuj Jacka Chef s Gumbo mieszany](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Rysunek 15**: dla Pokaż i edytowanie wszystkich dostawców użytkowników, dla Jacka Chef s Gumbo mieszany jest wyświetlany przycisk Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Sprawdzanie uprawnień dostępu do warstwy logiki biznesowej

W tym samouczku platformy ASP.NET strony obsługuje całą logikę w odniesieniu do informacji, jakie użytkownik może zobaczyć i jakie produkty on aktualizacji. W idealnym przypadku tę logikę również będą zainstalowane na warstwę logiki biznesowej. Na przykład `SuppliersBLL` klasy s `GetSuppliers()` (która zwraca wszystkich dostawców) może obejmować wyboru, aby upewnić się, że obecnie zalogowany użytkownik jest *nie* skojarzony z określonym dostawcą. Podobnie `UpdateSupplierAddress` metoda może zawierać wyboru, aby upewnić się, że obecnie zalogowany użytkownik, albo działał dla firmy (i w związku z tym można zaktualizować wszystkie informacje adresu dostawcy) lub jest skojarzona z dostawcą, którego dane są aktualizowane.

I nie obejmują takie kontrole warstwy logiki warstwy Biznesowej, ponieważ w naszym samouczku prawa użytkownika s są określane przez DropDownList na stronie, która nie ma dostępu do klasy logiki warstwy Biznesowej. Podczas korzystania z systemu członkostwa lub jednego z schematów uwierzytelniania w poziomie z pola dostarczane przez platformę ASP.NET (takich jak uwierzytelnianie systemu Windows), aktualnie zalogowanego użytkownika s można uzyskać informacji o i ról z logiki warstwy Biznesowej, dzięki czemu takiego dostępu prawa sprawdza możliwe na prezentacji i warstwy logiki warstwy Biznesowej.

## <a name="summary"></a>Podsumowanie

W większości witryn, które zapewniają kont użytkowników należy dostosować interfejs modyfikacji danych, które są ustalane na podstawie zalogowanego użytkownika. Użytkownicy administracyjni można usuwać i edytować żadnych rekordów użytkowników innych niż administracyjne mogą być ograniczone tylko aktualizowania lub usuwania rekordów one utworzone samodzielnie. Może być niezależnie od scenariusza, formantów sieci Web, ObjectDataSource, danych i klasy warstwy logiki biznesowej można rozszerzyć, aby dodać lub odmówić niektórych funkcji, na podstawie zalogowanego użytkownika. W tym samouczku widzieliśmy jak ograniczyć dane widoczne i można edytować w zależności od tego, czy użytkownik został skojarzony z określonym dostawcą lub jeśli ich działał dla firmy.

W tym samouczku kończy się naszego badania Wstawianie, aktualizowanie i usuwanie danych za pomocą kontrolki GridView widoku DetailsView i FormView. Począwszy od w następnym samouczku, firma Microsoft będzie Włącz wymagające uwagi do dodawania stronicowania i sortowania pomocy technicznej.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-client-side-confirmation-when-deleting-vb.md)
