---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: Interakcja z strony wzorcowej ze strony zawartość (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Sprawdza, czy sposób wywołania metody, ustaw właściwości, etc. strony wzorcowej z kodu na stronie zawartości.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: b550d8c2a64bb2ad91e1db7b2c25433f73dbd5b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>Interakcja z strony wzorcowej ze strony zawartość (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Sprawdza, czy sposób wywołania metody, ustaw właściwości, etc. strony wzorcowej z kodu na stronie zawartości.


## <a name="introduction"></a>Wprowadzenie

W ciągu ostatnich pięciu samouczki możemy sprawdzono jak tworzenie strony wzorcowej, zdefiniuj obszarów zawartości strony ASP.NET należy powiązać strony wzorcowej i zdefiniuj specyficznym dla strony zawartość. Gdy użytkownik zażąda określonej strony zawartości, zawartość i znaczników stron wzorcowych są zespolone w czasie wykonywania, co powoduje renderowanie hierarchii ujednoliconego formantu. W związku z tym już widzieliśmy taki w sposób, który mogą współdziałać strony wzorcowej i jedną z jej zawartości strony: Wyświetla stronę zawartość znaczników transfuse w formantach ContentPlaceHolder strony wzorcowej.

Co mamy jeszcze do sprawdzenia jest strony wzorcowej oraz strony zawartości można interakcje programowo. Oprócz Definiowanie znaczników dla strony wzorcowej ContentPlaceHolder formantów, strony zawartości można przypisać wartości do właściwości publiczne jego strony wzorcowej i wywołanie jego metody publiczne. Podobnie strony głównej może współpracować z jej zawartości strony. Programowe interakcji między strony wzorcowej i zawartości jest mniej typowych niż interakcji między ich deklaratywne znaczników, istnieje wiele scenariuszy, gdzie jest potrzebna takie programowe interakcji.

W tym samouczku omówione strony zawartości można programowo interakcje z jego strony wzorcowej; w następnym samouczku przedstawiono, jak strony wzorcowej podobnie zakłócają jej zawartości strony.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Przykłady programowe interakcji między strony zawartości i jego strony wzorcowej

Gdy określonym regionie strony musi być skonfigurowane dla strony Strona, używamy ContentPlaceHolder formantu. Ale co sytuacji, w której większość stron muszą Emituj niektórych danych wyjściowych, ale niewielkiej liczby stron konieczne dostosować go do wyświetlenia coś innego? Przykładem takiego, które firma Microsoft zbadane w [ *wielu Elementy ContentPlaceHolders i domyślne zawartości* ](multiple-contentplaceholders-and-default-content-cs.md) obejmuje samouczek, wyświetlanie interfejs logowania na każdej stronie. Podczas gdy większość stron powinna zawierać interfejs logowania, powinien zostać pominięty na kilka stron, takich jak: strony głównej logowania (`Login.aspx`); strony tworzenia konta; i innych stron, które są dostępne dla użytkowników uwierzytelnionych jedynie. [ *Wielu Elementy ContentPlaceHolders i zawartości domyślnej* ](multiple-contentplaceholders-and-default-content-cs.md) samouczku przedstawiono sposób definiowania zawartości domyślnej dla elementu ContentPlaceHolder na stronie głównej i następnie jak jej zastąpienie w tych stron where zawartości domyślnej nie była żądana.

Innym rozwiązaniem jest tworzenie publicznego właściwości lub metody w ramach strony głównej, która wskazuje, czy do wyświetlenia lub ukrycia interfejsu logowania. Na przykład strony wzorcowej może zawierać właściwości publicznej o nazwie `ShowLoginUI` użyto którego wartość można ustawić `Visible` właściwości formantu logowania na stronie głównej. Następnie można programowo ustawić te strony zawartości, której ma być pomijana interfejsu użytkownika logowania `ShowLoginUI` właściwości `false`.

Prawdopodobnie najbardziej typowym przykładem zawartości i interakcji z strony wzorcowej występuje, gdy dane wyświetlane na potrzeby strony wzorcowej należy odświeżyć, po akcji upłynął czas na stronie zawartości. Należy wziąć pod uwagę, że strony głównej, który zawiera element GridView wyświetlający pięć ostatnio dodane rekordy z tabeli określonej bazy danych, i że jedna z jej strony zawartości zawiera interfejs służący do dodawania nowych rekordów do tej samej tabeli.

Gdy użytkownik odwiedza stronę, aby dodać nowy rekord, stwierdza, że pięciu ostatnio dodany rekordów wyświetlanych na stronie głównej. Po wypełnieniu wartości w kolumnach nowego rekordu, użytkownik wyśle formularz. Przy założeniu, że ma GridView na stronie głównej jego `EnableViewState` właściwość na wartość true (ustawienie domyślne), jego zawartość zostanie ponownie załadowana z widoku stanu i w rezultacie pięć samego rekordów są wyświetlane nawet, jeśli nowszy rekord został dodany do bazy danych. Może to być niezrozumiałe dla użytkownika.

> [!NOTE]
> Nawet wtedy, gdy stan widoku GridView zostanie wyłączone, aby go rebinds do jego źródle danych, na każdym ogłaszania zwrotnego, on nadal nie będzie wyświetlany po prostu dodane rekord ponieważ danych jest powiązany z widoku GridView wcześniej w cyklu życia strony niż po dodaniu nowego rekordu do datab ASE.


Aby rozwiązać ten problem, dzięki czemu właśnie dodano rekord jest wyświetlany na stronie głównej elementu GridView odświeżania strony należy poinstruować Aby ponownie powiązać ze swoim źródłem danych widoku GridView *po* dodano nowy rekord w bazie danych. Wymaga interakcji między zawartością a strony wzorcowe, ponieważ interfejs do dodawania nowego rekordu (i jego programy obsługi zdarzeń) znajdują się w strony zawartości, ale GridView, który musi zostać odświeżona jest na stronie głównej.

Odświeżanie widoku strony wzorcowej z obsługi zdarzeń, na stronie zawartości jest jednym z najbardziej typowych potrzeb dotyczących zawartości i interakcja strony wzorcowej, Przyjrzyjmy się w tym temacie bardziej szczegółowo. Pobieranie na potrzeby tego samouczka zawiera bazę danych programu Microsoft SQL Server 2005 Express Edition o nazwie `NORTHWIND.MDF` w witrynie sieci Web `App_Data` folderu. Bazy danych Northwind przechowuje produktu, pracowników i informacji o sprzedaży dla fikcyjnej firmy Northwind Traders.

Krok 1 przeszukiwań przez wyświetlanie pięciu ostatnio dodane produktów w widoku GridView na stronie głównej. Krok 2 tworzy stronę zawartości do dodawania nowych produktów. Krok 3 wyszukuje w sposób tworzenia publicznej właściwości i metody na stronie głównej i krok 4 przedstawiono sposób programowo interfejsu z tych właściwości i metod ze strony zawartość.

> [!NOTE]
> W tym samouczku nie delve do szczegółowe informacje na temat pracy z danymi w programie ASP.NET. Kroki konfigurowania strony wzorcowej do wyświetlania danych i strony zawartości do wstawiania danych są kompletne, jeszcze breezy. Więcej informacji na temat przyjrzeć się wyświetlanie i wstawianie danych za pomocą formantów SqlDataSource i GridView można znaleźć zasobów w sekcji dalsze odczyty na końcu tego samouczka.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Krok 1: Wyświetlanie pięć ostatnio dodane produktów na stronie wzorcowej

Otwórz `Site.master` strony wzorcowej i Dodaj etykietę i kontrolce GridView do `leftContent` `<div>`. Czyści etykiety `Text` właściwość, ustaw jej `EnableViewState` właściwości na wartość false i jego `ID` właściwości `GridMessage`; Ustaw w widoku GridView `ID` właściwości `RecentProducts`. Następnie przy użyciu projektanta, rozwiń w widoku GridView tagów inteligentnych i wybierz powiązać go z nowego źródła danych. Spowoduje to uruchomienie Kreatora konfiguracji źródła danych. Ponieważ w bazie danych Northwind `App_Data` folder jest bazą danych programu Microsoft SQL Server, tworzenia SqlDataSource wybierając (zobacz rysunek 1); nazwa SqlDataSource `RecentProductsDataSource`.


[![Formant SqlDataSource o nazwie RecentProductsDataSource powiązać widoku GridView](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Rysunek 01**: powiązać widoku GridView kontroli SqlDataSource o nazwie `RecentProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


Następnym krokiem zapyta nam do określenia, jakie bazy danych do nawiązania połączenia. Wybierz `NORTHWIND.MDF` bazy danych plików z listy rozwijanej, a następnie kliknij przycisk Dalej. Ponieważ jest to już użyliśmy tej bazy danych po raz pierwszy, Kreator będzie oferować przechowywanie parametrów połączenia w `Web.config`. Jest przechowywanie parametrów połączenia przy użyciu nazwy `NorthwindConnectionString`.


[![Połączenie z bazą danych Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Rysunek 02**: połączenie z bazą danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


Kreator konfigurowania źródła danych zawiera dwa oznacza za pomocą których można określić zapytania używane do pobierania danych:

- Określając niestandardową instrukcję SQL lub procedurę składowaną lub
- Pobrania tabelę lub widok, a następnie określając kolumn do zwrócenia

Ponieważ chcemy zwrócić tylko pięć ostatnio dodane produktów, należy określić niestandardową instrukcję SQL. Użyj następującego zapytania wybierz:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5` — Słowo kluczowe zwraca tylko pierwsze pięć rekordy z zapytania. `Products` Klucza podstawowego tabeli `ProductID`, jest `IDENTITY` kolumny, która zapewnia nam, że każdy z produktów nowe dodane do tabeli wartości większej niż wartość poprzedniej pozycji. W związku z tym sortowanie wyników przez `ProductID` w kolejności malejącej zwraca począwszy ostatnio utworzonymi produktów.


[![Zwraca pięć ostatnio dodane produktów](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Rysunek 03**: zwraca pięć najbardziej ostatnio dodane produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


Po zakończeniu działania kreatora programu Visual Studio generuje dwa BoundFields elementu GridView do wyświetlenia `ProductName` i `UnitPrice` pola zwrócony z bazy danych. W tym momencie strony wzorcowej deklaratywne znacznika powinna zawierać znaczników podobny do następującego:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Jak widać, zawiera kod znaczników: formantu etykiety w sieci Web (`GridMessage`); widoku GridView `RecentProducts`, z dwoma BoundFields; i SqlDataSource kontrolkę, która zwraca pięciu ostatnio dodane produktów.

Z tym GridView utworzony i skonfigurowany, kontrolą SqlDataSource, odwiedź witrynę sieci Web za pośrednictwem przeglądarki. Jak pokazano na rysunku 4, zobaczysz, że siatki w lewym dolnym rogu, która zawiera pięć ostatnio dodany produktów.


[![Pięć ostatnio dodane produktów Wyświetla widoku GridView](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Rysunek 04**: widoku GridView Wyświetla pięć najbardziej ostatnio dodane produktów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> Możesz także oczyszczanie widoku GridView. Sugestie obejmują formatowania wyświetlone `UnitPrice` wartość jako walutę i przy użyciu czcionki i kolory tła, aby poprawić wygląd siatki.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Krok 2: Tworzenie strony zawartości można dodać nowych produktów

Nasze następne zadanie polega na utworzeniu strony zawartości, z którego użytkownik może dodawać nowych produktów do `Products` tabeli. Dodaj nową stronę zawartości do `Admin` folder o nazwie `AddProduct.aspx`, wprowadzania się, że aby powiązać `Site.master` strony wzorcowej. Rysunek 5. Pokazuje Eksplorator rozwiązań po dodaniu tej strony w witrynie sieci Web.


[![Dodaj nową stronę ASP.NET do folderu Admin.](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Rysunek 05**: Dodaj nową stronę ASP.NET do `Admin` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


Odwołaj się, że w [ *określenie tytułu, tagi Meta i inne nagłówków HTML na stronie wzorcowej* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) samouczek utworzyliśmy klasy niestandardowej strony podstawowej o nazwie `BasePage` który generowany tytuł strony, gdy był nie jest jawnie ustawione. Przejdź do `AddProduct.aspx` kodem strony klasy i została ona pochodzić od `BasePage` (a nie z `System.Web.UI.Page`).

Na koniec zaktualizuj `Web.sitemap` pliku, aby uwzględnić wpis dla tej lekcji. Dodaj następujący kod pod `<siteMapNode>` dla lekcji problemów nazewnictwa identyfikator formantu:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Jak pokazano na rysunku 6, to dodanie `<siteMapNode>` element jest widoczny w liście — lekcje.

Wróć do `AddProduct.aspx`. W zawartości formantu `MainContent` ContentPlaceHolder, Dodaj formant widoku DetailsView i nadaj mu nazwę `NewProduct`. Powiązać widoku DetailsView formant SqlDataSource o nazwie `NewProductDataSource`. Takie jak SqlDataSource w kroku 1, skonfigurować kreatora tak, aby korzystała z bazy danych Northwind i określić niestandardową instrukcję SQL. Ponieważ w widoku DetailsView będzie używany do dodawania elementów do bazy danych, należy określić zarówno `SELECT` instrukcji i `INSERT` instrukcji. Należy użyć następującego `SELECT` zapytania:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Następnie na karcie Wstaw Dodaj następujące `INSERT` instrukcji:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Po zakończeniu pracy kreatora przejdź do widoku DetailsView tagów inteligentnych i zaznacz pole wyboru "Włącz wstawianie". To działanie powoduje dodanie CommandField do widoku DetailsView z jego `ShowInsertButton` właściwością o wartości true. Ponieważ tego widoku DetailsView będą używane wyłącznie do wstawiania danych, ustaw DetailsView `DefaultMode` właściwości `Insert`.

To wszystko jest do niego! Umożliwia testowanie tej strony. Odwiedź stronę `AddProduct.aspx` za pośrednictwem przeglądarki, wprowadź nazwę i cen (patrz rysunek 6).


[![Dodawanie nowego produktu do bazy danych](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Rysunek 06**: Dodawanie nowego produktu do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Po wpisaniu nazwy i cen nowego produktu, kliknij przycisk Wstaw. Powoduje to formularz odświeżania. Strony, kontroli SqlDataSource w `INSERT` instrukcja jest wykonywana; jego dwa parametry są wypełniane przy użyciu wartości wprowadzonych przez użytkownika w widoku DetailsView dwóch pól tekstowych. Niestety jest nie wizualną opinię, która nastąpiła insert. Byłoby nieuprzywilejowany wiadomości wyświetlane, potwierdzenie, że dodano nowy rekord. I pozostaw to wykonywania dla czytnika. Ponadto po dodaniu nowego rekordu w widoku DetailsView GridView na stronie głównej wciąż pokazuje tego samego rekordów pięć jako przed; nie ma rekordu po prostu dodane. Zajmiemy się jak rozwiązać ten problem w kolejnych krokach.

> [!NOTE]
> Oprócz dodania jakiegoś wizualny, który insert zakończyła się pomyślnie, I czy zachęca do również zaktualizować DetailsView interfejsu wstawianie do uwzględnienia sprawdzania poprawności. Obecnie nie są bez sprawdzania poprawności. Jeśli użytkownik wprowadzi nieprawidłową wartość dla `UnitPrice` pola, takie jak "jest zbyt drogie," zostanie wygenerowany wyjątek odświeżania strony, gdy system próbuje przekonwertować tego ciągu wartości dziesiętnej. Aby uzyskać więcej informacji o dostosowywaniu Wstawianie interfejsu, zapoznaj się [ *Dostosowywanie interfejs modyfikacji danych* samouczek](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) z mojej [pracy z samouczkiem serii danych](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Krok 3: Tworzenie publicznej właściwości i metody w strony wzorcowej

W kroku 1 dodaliśmy formantu etykiety sieci Web o nazwie `GridMessage` powyżej widoku GridView na stronie głównej. Etykieta ma na celu opcjonalnie wyświetlić wiadomość. Na przykład, po dodaniu nowego rekordu do `Products` tabeli może chęć pokazania komunikat: "*ProductName* został dodany do bazy danych." Zamiast kodowane tekstu dla etykiety na stronie głównej firma Microsoft może być komunikat, aby mieć możliwość dostosowania przez strony zawartości.

Ponieważ formantu etykiety jest zaimplementowany jako zmienną chroniony element członkowski w strony wzorcowej nie jest dostępny bezpośrednio z strony z zawartością. Aby pracować z etykietą w ramach strony wzorcowej ze strony zawartość (lub dla tej sprawy żadnego formantu sieci Web, na stronie głównej) należy utworzyć na stronie głównej, który ujawnia kontrolka sieci Web lub służy jako serwer proxy, przez którą jednej z jego właściwości można właściwość publiczna  dostępne. Dodaj następującą składnię do klasy związane z kodem strony wzorcowej do udostępnienia jej `Text` właściwości:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Po dodaniu nowego rekordu do `Products` tabelę z zawartości strony `RecentProducts` GridView na stronie głównej trzeba ponownie powiązać do źródła danych. Aby ponownie powiązać wywołania GridView jego `DataBind` metody. Ponieważ widoku GridView na stronie głównej nie jest programowo dostępny dla stron zawartości, firma Microsoft, należy utworzyć publicznej metody na stronie głównej, która po wywołaniu, rebinds dane do widoku GridView. Dodaj następującą metodę do klasy związane z kodem strony wzorcowej:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

Z `GridMessageText` właściwości i `RefreshRecentProductsGrid` metody w miejscu, dowolnej strony zawartości można programowo ustawić lub odczytać wartość `GridMessage` etykiety `Text` właściwości lub ponownie powiązać dane `RecentProducts` widoku GridView. Krok 4 sprawdza uzyskiwania dostępu do strony wzorcowej publicznej właściwości i metody na stronie zawartości.

> [!NOTE]
> Nie zapomnij oznaczyć właściwości i metod jako strony wzorcowej `public`. Jeśli użytkownik jawnie oznacza tych właściwości i metod jako `public`, nie będzie dostępny ze strony zawartość.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Krok 4: Wywoływanie strony wzorcowej publiczne elementy członkowskie z zawartości strony

Teraz, gdy strona wzorcowa ma niezbędne publicznej właściwości i metody, już wszystko gotowe do wywołania tych właściwości i metody z `AddProduct.aspx` strony zawartość. W szczególności należy ustawić strony wzorcowej `GridMessageText` właściwości i wywołanie jego `RefreshRecentProductsGrid` metody po dodaniu nowego produktu do bazy danych. Wszystkie platformy ASP.NET danych sieci Web kontrolki wyzwalać zdarzeń bezpośrednio przed i po zakończeniu różnych zadań, które ułatwiają deweloperom strony niektóre akcje programowe przed lub po zadania. Na przykład gdy użytkownik końcowy kliknie przycisk Wstaw DetailsView, na ogłaszanie zgłasza widoku DetailsView jego `ItemInserting` zdarzeń przed rozpoczęciem Wstawianie przepływu pracy. Następnie wstawia rekord w bazie danych. Następujące, który wywołuje widoku DetailsView jego `ItemInserted` zdarzeń. W związku z tym, aby pracować ze stroną wzorcową po dodaniu nowego produktu, Tworzenie procedury obsługi zdarzeń DetailsView `ItemInserted` zdarzeń.

Istnieją dwa sposoby, które strony zawartości programowo mogą łączyć się z jego strony głównej:

- Przy użyciu `Page.Master` właściwość, która zwraca typowaniem luźnym odwołanie do strony głównej, lub
- Określ strony strony wzorcowej typu lub ścieżki pliku za pośrednictwem `@MasterType` dyrektywy; to automatycznie dodaje właściwość jednoznacznie stronę o nazwie `Master`.

Przeanalizujmy obu podejść.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Korzystanie z typowaniem luźnym`Page.Master`właściwości

Wszystkie strony sieci web platformy ASP.NET musi pochodzić od `Page` klasy, która znajduje się w `System.Web.UI` przestrzeni nazw. `Page` Klasa zawiera [ `Master` właściwości](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) która zwraca odwołanie do strony głównej. Jeśli strona nie ma strony wzorcowej `Master` zwraca `null`.

`Master` Właściwość zwraca obiekt typu [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (również znajduje się w `System.Web.UI` przestrzeni nazw) czyli typu podstawowego, w którym wszystkie strony wzorcowe pochodzi od. W związku z tym celu użyj właściwości publicznej lub metody zdefiniowane w naszej witrynie internetowej strony wzorcowej możemy rzutować `MasterPage` obiektu zwróconego z `Master` właściwości do odpowiedniego typu. Ponieważ firma Microsoft o nazwie naszych plik strony głównej `Site.master`, miał nazwę klasy związane z kodem `Site`. W związku z tym poniższy kod rzutowania `Page.Master` właściwości do wystąpienia klasy lokacji.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Teraz, gdy mamy rzutować typowaniem luźnym `Page.Master` właściwości `Site` typu Firma Microsoft może odwoływać się do właściwości i metod specyficzne dla lokacji. Jak pokazano na rysunku 7, właściwość publiczna `GridMessageText` pojawia się w listy rozwijanej IntelliSense.


[![IntelliSense zawiera publicznej właściwości i metody naszej strony wzorcowej](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Rysunek 07**: IntelliSense zawiera publicznej właściwości i metody naszej strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Jeśli nazwany plik strony głównej `MasterPage.master` , a następnie nazwę klasy związane z kodem strony wzorcowej jest `MasterPage`. Może to prowadzić do kodzie, kiedy następuje rzutowanie z typu `System.Web.UI.MasterPage` do Twojej `MasterPage` klasy. Krótko mówiąc należy do pełnej kwalifikacji typu, które są rzutowanie, które mogą być nieco trudnych przy użyciu model projekt witryny sieci Web. Moja sugestia byłoby albo upewnij się, że podczas tworzenia stronie głównej nazwy coś innego niż `MasterPage.master` lub nawet lepiej utworzyć jednoznacznie odwołania do strony wzorcowej.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Tworząc jednoznacznie odwołanie z`@MasterType`— dyrektywa

Podczas przeglądania ściśle widać, że strony ASP.NET klasę kodu jest częściowej klasy (Uwaga `partial` — słowo kluczowe w definicji klasy). Klasy częściowe zostały wprowadzone w C# i Visual Basic with.NET Framework 2.0 i mówiąc, Zezwalaj elementów członkowskich klas do zdefiniowania przez wiele plików. Plik klasy związane z kodem - `AddProduct.aspx.cs`, na przykład — zawiera kod, który nie developer strony utworzyć. Oprócz naszego kodu aparatu ASP.NET automatycznie tworzy plik osobnej klasy z właściwościami i obsługi zdarzeń w tym umożliwiło hierarchii klas strony deklaratywne znaczników.

Automatyczne generowanie kodu, który występuje zawsze, gdy jest odwiedzoną stronę ASP.NET paves sposób dla niektórych możliwości raczej interesujące i przydatne. W przypadku stron wzorcowych przypadku możemy powiedzieć aparatu ASP.NET, jakie strony wzorcowej jest używany przez naszą stronę zawartości generuje silnie typizowanego `Master` właściwości firmie Microsoft.

Użyj [ `@MasterType` dyrektywy](https://msdn.microsoft.com/library/ms228274.aspx) poinformowanie aparatu ASP.NET typu strony zawartości strony wzorcowej. `@MasterType` Dyrektywy może zaakceptować nazwy typu strony wzorcowej lub ścieżki pliku. Aby określić, że `AddProduct.aspx` strona używa `Site.master` jako jego strony wzorcowej, Dodaj następujące dyrektywy na początku `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Ta dyrektywa nakazuje aparatu ASP.NET można dodać odwołania do strony wzorcowej za pośrednictwem właściwości o nazwie jednoznacznie `Master`. Z `@MasterType` dyrektywy w miejscu, możemy zadzwonić `Site.master` wzorca publicznej właściwości i metody za pomocą strony `Master` właściwości bez żadnych rzutowania.

> [!NOTE]
> W przypadku pominięcia `@MasterType` dyrektywy, składnia `Page.Master` i `Master` zwracać samo: obiekt typowaniem luźnym stronę wzorcową. Jeśli dołączysz `@MasterType` następnie dyrektywy `Master` zwraca jednoznacznie odwołanie do określonej strony wzorcowej. `Page.Master`, jednak nadal zwraca typowaniem luźnym odwołanie. Aby bardziej szczegółowego przyjrzeć się dlaczego jest to możliwe i jak `Master` właściwości jest tworzony podczas `@MasterType` dyrektywa jest dołączony, zobacz [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)w wpis w blogu [ `@MasterType` w programie ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualizowanie strony wzorcowej po dodaniu nowego produktu

Teraz, aby było wiadomo, jak można wywołać publicznej właściwości i metody ze strony zawartości strony wzorcowej, już wszystko gotowe do aktualizacji `AddProduct.aspx` strony, aby po dodaniu nowego produktu odświeżania strony wzorcowej. Na początku kroku 4 utworzyliśmy program obsługi zdarzeń dla formantu widoku DetailsView `ItemInserting` zdarzenie, które wykonuje natychmiast, po dodaniu nowego produktu do bazy danych. Dodaj następujący kod do programu obsługi zdarzeń:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Powyższy kod używa zarówno typowaniem luźnym `Page.Master` właściwości i jednoznacznie `Master` właściwości. Należy pamiętać, że `GridMessageText` właściwość jest ustawiona na "*ProductName* dodane do..." Wartości produktu just dodawane są dostępne za pośrednictwem `e.Values` kolekcji; jak widać, po prostu dodane `ProductName` wartość jest dostępna za pośrednictwem `e.Values["ProductName"]`.

Rysunek nr 8 przedstawia `AddProduct.aspx` strony natychmiast po nowym produktem - Scotta Soda - został dodany do bazy danych. Należy zauważyć, że nazwa produktu po prostu dodane jest odnotowany w etykiety strony wzorcowej i odświeżeniu widoku GridView produktu i jego ceny.


[![Etykiety i Pokaż GridView produktu po prostu dodane strony wzorcowej](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Rysunek 08**: Etykieta strony wzorcowej i GridView Pokaż produktu Just-Added ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Podsumowanie

W idealnym przypadku strony wzorcowej i na stronach zawartości są całkowicie niezależne od siebie i wymagają żaden poziom interakcji. Gdy stron wzorcowych i stron zawartości należy tak zaprojektować tego celu, pamiętając, istnieje wiele typowych scenariuszy, w których strony zawartości muszą łączyć się z jego strony wzorcowej. Jedną z najbardziej typowych przyczyn koncentruje się wokół aktualizowanie konkretnego fragmentu wyświetlania strony wzorcowej oparte na niektóre akcje, które odwzorowanie na stronie zawartości.

Dobre wieści jest stosunkowo prosta mają strony zawartości programowo interakcji z jego strony wzorcowej. Rozpocznij od utworzenia publicznej właściwości lub metody na stronie głównej, który Hermetyzowanie funkcji, który ma być wywoływany przez strony zawartości. Następnie na stronie zawartości dostęp do strony wzorcowej właściwości i metod za pośrednictwem typowaniem luźnym `Page.Master` właściwości lub użyj `@MasterType` dyrektywy do tworzenia jednoznacznie odwołania do strony wzorcowej.

W następnym samouczku omówione sposobu ustawiania programowo interakcji z jednej z jej strony zawartości strony wzorcowej.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu i aktualizowanie danych w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Stron wzorcowych ASP.NET: Porady, wskazówki i pułapek](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Przekazywanie informacji między zawartości i stron wzorcowych](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Praca z danymi w samouczkach ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora wielu książek ASP/ASP.NET i twórcę 4GuysFromRolla.com pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 3.5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott jest osiągalny w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Nowak Zack. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](control-id-naming-in-content-pages-cs.md)
> [dalej](interacting-with-the-content-page-from-the-master-page-cs.md)
